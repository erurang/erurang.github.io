---
layout: post
title: "CORS 해결을 위한 http-proxy-middleware"
subtitle: "CORS 해결을 위한 http-proxy-middleware"
categories: web
tags: javascript
comments: true
---

## 브라우저를 속이는 방식

우리는 이전글 [CORS란?](https://erurang.github.io/web/2022/08/09/cors/)에서 `CORS`에 대해서 이해해보았다.

`CORS`를 해결하기위해선 출처를 강제로 똑같이 만들어서 브라우저를 속이는 방법과

서버에서 헤더값을 던져줄때 비교하는 두 가지 방법이 존재한다.

이 두 방법중에 나는 브라우저를 속이는 방식에 대해 알아볼것이다.

## http-proxy-middle-ware 설치

![스크린샷 2022-08-14 오후 5 33 49](https://user-images.githubusercontent.com/56789064/184529042-3d7db5c7-aea6-4045-9a29-1cfc7f1e1eb6.png)

리액트로 개발하는 경우 `/src` 폴더에서 `setupProxy.js` 파일을 만들어준뒤 아래처럼 적어주자

```
const { createProxyMiddleware } = require("http-proxy-middleware");

module.exports = function (app) {
  app.use(
    createProxyMiddleware("/api", { // 서버로 요청할때 도메인을 임의로 변경시키기위해 사용할 문자열
      target: "https://api.server.com", // 내가 요청하고자하는 서버의 도메인
      changeOrigin: true,
      pathRewrite: {
        "^/api": "",
      },
    })
  );
};
```

위의 코드가 하는 역할을 알아보자. 일단 나는 **브라우저를 속일것**이라고 하였다.

## createProxyMiddleware

`createProxyMiddleware`의 첫번쨰 변수는 `Proxy`를 사용하고싶은 시작 도메인을 설정해주는것이다.

예를들어 브라우저에서 서버로 요청할 도메인이 `fetch(https://api.server.com/items/title)`라면

개발단계에서 위 도메인을 모두 쓰는것이 아닌 `fetch(/api/items/title)` 라고만 적으면 된다.

## target

`target`은 요청할 서버의 도메인을 적어준다. 이후부터 브라우저는 첫번째 인자로 설정한 문자열(`/api`)을 보고

이 문자열로 시작하는 요청들은 모두 `target`으로 시작하게 변환시킨다.

`fetch(/api/items/title)`로 요청을 하면 브라우저가 `fetch(https://api.server.com/items/title)` 변환하여 통신을 한다.

## pathRewrite

`pathRewrite`는 설정 문자열을 변경하는 것이다.

`"^/api": ""`의 뜻은 `/api`라는 단어로 시작하는 요청은 `""` 공백으로 변환시킨다는 뜻이다.

`/api`로 시작하는 도메인을 `target`인 서버로 요청을 보내는것 까진 하였다.

하지만 이 문자열은 우리가 임의로 설정한 것이기 때문에 없애주어야한다. 그렇지않으면

```
https://api.server.com/items/title (올바른 도메인)
https://api.server.com/api/items/title (틀린 도메인)
```

우리가 원하는 요청 주소는 첫번째인데 두번째로 요청이 가버려 오류가 뜨게된다.
