---
layout: post
title: "React에서의 SEO 친화적으로 만들기"
subtitle: "React에서의 SEO 친화적으로 만들기"
categories: web
tags: react
comments: true
---

## Search Engine Optimization

줄여서 `SEO`라고 표기하는데 검색 엔진 최적화 방식이다. 이것은 여러 포털사이트에서 우리의 사이트가 잘 검색되도록 하는 방법이다.

포털사이트가 여러 사이트를 탐색한다. 각 사이트의 `html` 파일을 보고 분류하여 우리가 검색시 보여주는 역할을 해준다.

생각해볼것이있다. 잠깐만.. `html` 파일을 본다고? `React`는 `index.html`이라는 파일 하나만 가지고있다.

`React`는 `<div id="root"></div>` 라는 태그에 동적으로 태그를 추가하여 웹사이트를 만드는 `CSR`방식의 `SPA`이다.

포털사이트가 `React`로 만들어진 사이트를 딱 접속했을때 보는것은 설정된 `meta`태그들과, `body`에 있는 `<div id="root"></div>` 뿐이라는거다.

## SEO친화적으로 만들기

즉 리액트는 `CSR`이기 때문에 `SEO`를 개선시켜야만 한다. 그러려면 `SSR`을 사용하거나 서버에서 데이터를 포함한 `HTML`을 다 만들어주는 방식이 필요하다.

그래서 `Next.js`에서는 `HTML`에 데이터를 넣어서 `HTML`을 만드는 방식인 `SSG`와 `SSR`을 모두 지원하고있다.

하지만 리액트에서는 이 두방법 모두 현재까지는 지원하지않고있다. 리액트에서 `SEO`개선을 위한 방법은 `html`을 빌드시에 만들어버리는 방법이 존재한다.

## HTML을 pre-render시키기

`React`에서 `pre-render`를 시켜서 `html`을 만들어내는 방식인 `react-snap` 라이브러리가 존재한다.

터미널을 통해 `npm i react-snap` 설치 후 `package.json`을 수정해주자

```
"scripts": {
    ...
    "postbuild": "react-snap"
},
"reactSnap" : {
    "include" : [
      "html 파일을 만들 router","","" ...
      // ex) ['/', '/news', '/blog' ...]
    ]
}
```

스크립트를 수정 후 `npm run build`를 하면 `build`시에 `react-snap`으로 `pre-render`가 먼저 일어나게 된다.

`react-snap`은 빌드시점의 데이터를 불러와서 `HTML`에 입히는 방식이다. 자 그럼 이것이 `SSG`와 같은가? 아니다

`SSG`는 매 페이지마다 새로 서버에서 만들어서 리턴하는것이고, `react-snap`은 빌드시점의 서버데이터를 만드는것일뿐이다.

그러니 반쪽짜리 `SEO`라고 할수있다.

## index.js render()에 대해서

위에서 `react-snap`이 그 시점 데이터를 사용해 `HTML`을 만든다고 했다.

```
import React from "react";
import { render } from "react-dom";
import App from "./App";

const rootElement = document.getElementById("root");

render(<App />,rootElement);
```

리액트를 최초로 `render()`하는 시점인 루트파일인 `index.js`이다. 여기서 문제가생긴다. 무슨문제?

우리는 `react`가 `react-snap`으로 만들어진 `HTML`을 사용해 `dom`을 새로 만들지않고 이벤트만 덮어주기를 바랄것이다.

하지만 리액트는 `index.js`에 적혀있는대로 단순히 `render()`를 할뿐이다. 그렇기때문에 아래 사진과 같은 현상이 일어나게된다

![스크린샷 2022-06-24 오후 1 31 23](https://user-images.githubusercontent.com/56789064/175462613-78d140a8-f18f-4b3a-a75f-bd8b4cc3a8f0.png)

`react-snap`으로 만들어진 `HTML`을 브라우저가 받아서 `DOM`을 하나하나 만들기 시작하다가 `javascript code` 즉 `react` 코드를

다운받기 시작할때 리액트는 다시 `HTML`을 만들게된다. 즉 우리는 `HTML`코드를 가지고있지만 한번더 생성되는 과정을 거치는것이다.

이런 일이 일어나지않고 기존 `HTML`에 이벤트와 기능만 씌어줄수있도록 리액트에서는 `hydrate`라는 함수로 처리해줄수있다.

## hydrate() 사용하기

```
import React from "react";
import { render, hydrate } from "react-dom";
import App from "./App";

const rootElement = document.getElementById("root");

if (rootElement.hasChildNodes()) {
  hydrate(<App />,rootElement);
} else {
  render(<App />,rootElement);
}
```

이미 만들어진 `HTML`이 존재하면 `hydrate()`를 통해 리액트의 이벤트를 입히고, 없으면 `render()`를 해서 `DOM`을 구축시키도록 수정해준다.
