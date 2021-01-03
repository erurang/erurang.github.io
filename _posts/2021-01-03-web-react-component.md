---
layout: post
title:  "React Component"
subtitle: "React Component"
categories: web
tags: react
comments: true

---

## Component 선언

리액트는 컴포넌트 들로 이루어져있다.

한 화면이 각각 나뉘어진 컴포넌트들의 집합체인데

이 컴포넌트를 만드는 방법엔 2개가 있다.

Class - **상태**에 따라 컴포넌트가 업데이트 되야한다면 
```
import React from "react";

class 클래스명 extends Component {
  state = {

  };

  render() {
    return
  }
}
```

Function - **상태**가 없고 정적으로 데이터가 표현된다면
```
function 함수명() {
  return 
}
```

하지만 차후에 새로나온 React Hook에서는 함수에서도 state와 lifecycle method를 사용할수있게 되었다.

나는 기본 React에 대해 다루겠다.

## ReactDOM

```
import React from "react";
import ReactDOM from "react-dom";
import "./index.css";
import App from "./app";

ReactDOM.render(
  <React.StrictMode>
    <App />
  </React.StrictMode>,
  document.getElementById("root")
);
```

`<React.StrictMode>` 는 영어그대로 문법오류를 엄격하게 알려준다.

브라우저는 html css javascript 만 이해할수있다. 그래서 리액트는 html이 이해할수있게 변환해줘야하는데

`import ReactDOM from "react-dom";`

react의 문법들을 브라우저가 알아먹을수 있게 babel이 처리를 해주고.

우리가 만든 컴포넌트들을 html과 연결해주는 작업을 하는 애가 ReactDOM 이다.

그래서 리액트가 젤 상위에 있는 컴포넌트인 `<App />`의 요소를 id가 root인 html에 연결시켜준다.

