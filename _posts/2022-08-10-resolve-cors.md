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

`CORS`를 해결하기위해선 출처(도메인)를 서버와 똑같이 만들어서 브라우저를 속이는 방법이 있고,

클라이언트에서 서버를 호출할 때 헤더에 `Origin` 속성에 클라이언트 `URL`을 입력하여 넘겨 서버에서 헤더값을 바교하여 응답을 주는 방법이 있다.

이 두 방법중에 이 글에서는 브라우저를 속이는 방식에 대해 알아볼것이다. (이 방식은 로컬 환경에서만 작동한다는것을 알아두자)

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

나는 **브라우저를 속일것**이라고 하였다. `CORS`를 피하는 방식중 하나인 클라이언트를 같은 출처로 만들어 서버로 요청하는 것이다.

`createProxyMiddleware`의 첫번째 변수는 `Proxy`를 사용해서 출처를 바꾸고 싶을때 어떤 문자열로 시작할때 출처를 변경할지 설정해주는것이다.

`target`은 클라이언트에서 요청할 서버의 도메인을 적어준다. 이후부터 브라우저는 첫번째 인자로 설정한 문자열(`/api`)을 보고 이 문자열로 시작하는 요청들은 모두 `target`으로 시작하게 변환시킨다.

예를들어 브라우저에서 서버로 요청할 도메인이 `https://api.server.com/items/title`라면 개발단계에서 위 도메인을 모두 쓰는것이 아닌 `/api/items/title` 라고 사용하면

클라이언트가 보내는 요청을 `localhost:3000`가 아닌 `https://api.server.com/items/title`로 변경하여 같은 출처로 만들어 `CORS`를 피할수있다.

## pathRewrite

`pathRewrite`는 요청할 문자열을 변경하는 것이다. `"^/api": ""`의 뜻은 `/api`라는 단어로 시작하는 요청은 `""` 공백으로 변환시킨다는 뜻이다.

`pathRewrite`를 통해 우리가 만든 `/api` 문자열을 `""` 공백으로 바꾸지 않으면 브라우저는 아래처럼 요청을 하게된다.

```
- pathRewrite 를 통한 공백처리를 했을때

https://api.server.com/items/title (올바른 도메인)

- pathRewrite 를 통한 공백처리를 하지않았을때
https://api.server.com/api/items/title (틀린 도메인)
```

로컬 개발시 `CORS`를 피하는 가장 편한 방법을 알아보았다. 하지만 `CORS`를 해결하는 가장 좋은 방법은 서버에 클라이언트의 도메인을 등록하는것임을 알아두자.
