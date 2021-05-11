---
layout: post
title:  "React Component & jsx"
subtitle: "React Component & jsx"
categories: antique
tags: antique
comments: true

---

## Component 선언

리액트는 컴포넌트 들로 이루어져있다.

한 화면이 각각 나뉘어진 컴포넌트들의 집합체인데

이 컴포넌트를 만드는 방법엔 2개가 있다.

## 옛날방식 (지금은 쓰지않음.)

```
import React, { Component } from "react"

class App extends Component {
  const name = "erurang"

  render() {
    1. return React.createElement('태그명', { 태그의속성들 }, 태그에 들어갈 요소)
    2. return <태그 속성>요소</태그>
    3. return <태그>{ 이 안에 자바스크립트 문법이나 요소를 사용가능 }</태그>
  }
}
```
이런식으로 컴포넌트를 만들었다. jsx라는 문법덕에 우리는 2번처럼 표현을 할수있게 되었다.

요새는 class와 function 으로 2가지 방법으로 만들수있다.

## Class - **상태**에 따라 컴포넌트가 업데이트 되야한다면 
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

## Function - **상태**가 없고 정적으로 데이터가 표현된다면

```
function 함수명() {
  return 
}
```

**PureComponent(class) memo(function)** 

기능도 있는데 이것은 리액트의 요소가 컴포넌트가 업데이트 될때

리액트는 기본적으로 모든요소를 전체적으로 다시 렌더하는데

**이전의 state와 props를 업데이트된 state와 props 비교하여**

똑같으면 render를 하지 않는다.

이 비교는 shallow하게 비교하는데. 

만약 오브젝트안의 데이터들의 변화가 일어나도 

오브젝트의 ref만 비교하여 ref가 같다면 변경을하지않는다. 

React Hook

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

리액트가 자체적으로 실행을 한번 더 해서 오류가 있는지 파악한다.


브라우저는 html css javascript 만 이해할수있다. 그래서 리액트는 html이 이해할수있게 변환해줘야하는데

`import ReactDOM from "react-dom";`

react의 문법들을 브라우저가 알아먹을수 있게 babel이 처리를 해주고.

우리가 만든 컴포넌트들을 html과 연결해주는 작업을 하는 애가 ReactDOM 이다.

그래서 리액트가 젤 상위에 있는 컴포넌트인 `<App />`의 요소를 id가 root인 html에 연결시켜준다.

## 결론

React는 state가 변경되면 전체적인 컴포넌트 전체를 다시 Render를 호출한다.

그럼 성능이슈가 있지 않을까? -> 리액트에서는 가상(버츄얼)의 DOM을 메모리에 따로두어서

실제로 업데이트가 되야하는 요소만 업데이트가 되어서 성능에 큰 문제가 없다.