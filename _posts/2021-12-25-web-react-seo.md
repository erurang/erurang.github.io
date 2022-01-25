---
layout: post
title:  "React에서의 SEO 친화적으로 만들기"
subtitle: "React에서의 SEO 친화적으로 만들기"
categories: web
tags: react
comments: true

---

예전에는 Next.js가 seo를 대신 해준다는 그런 말도 얼핏 들어보았지만, seo? 이게뭐 무슨상관이지? 생각이 들었는데

나는 한 웹사이트 프로젝트 개발을 끝낸후 SEO를 신경써야했다.

## Search Engine Optimization

줄여서 SEO라고 표기하는데 검색 엔진 최적화 방식이다. 이것은 여러 포털사이트에서 우리의 사이트가 잘 검색되도록 하는 방법이다.

포털사이트가 여러 사이트를 탐색한다. 각 사이트의 html 파일을 보고 분류하여 우리가 검색시 보여주는 역할을 해준다.

생각해볼것이있다. 잠깐만.. html 파일을 본다고? React는 index.html이라는 파일 하나만 가지고있다.

React는 `<div id="root"></div>` 라는 태그에 동적으로 태그를 추가하여 웹사이트를 만드는 SPA이다.

즉 포털사이트가 React로 만들어진 사이트를 딱 접속했을때 보는것은 설정된 meta태그들과, body에 있는 `<div id="root"></div>` 뿐이라는거다.

## 그럼 React에서 어떻게 이걸 해결하지?

React에서 pre-render를 시켜서 html을 만들어내는 방식이 있다.

`npm i react-snap` 설치후 package.json을 수정해주자

```
"scripts": {
    ...
    "postbuild": "react-snap"
},
"reactSnap" : {
    "include" : [
      "html 파일을 만들 router","","" ...
    ]
}
```

## index.html를 수정해주자
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

기존의 React 형식과 다른걸 볼수있다. 그리고 react-dom에서 hydrate와 render를 사용한다.

