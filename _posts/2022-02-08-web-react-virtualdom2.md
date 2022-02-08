---
layout: post
title:  "리액트가 jsx를 해석하는 방법과 Virtual DOM을 만들어보기"
subtitle: "리액트가 jsx를 해석하는 방법과 Virtual DOM을 만들어보기"
categories: web
tags: react
comments: true

---

## 기존 DOM을 업데이트하는 방식

```
<!doctype html>
<html lang="en">
 <head></head>
 <body>
    <ul class="list">
        <li class="list__item">List item</li>
    </ul>
  </body>
</html>
```

위의 `HTML`의 `DOM`은 이렇게 구성될것이다

```
html
   ㄴ head lang="en"
   ㄴ body
      ㄴ ul class="list"
         ㄴ li class="list__item"
            ㄴ "List item"
```

여기서 첫번째 리스트 아이템을 수정하고, 리스트에 항목(li)를 하나 추가하고 싶다고 해보자.

```
// 첫번째 리스트 아이템을 수정
const listItemOne = document.getElementsByClassName("list__item")[0];
listItemOne.textContent = "List item one";

// 리스트에 항목(li)를 하나 추가
const list = document.getElementsByClassName("list")[0];
const listItemTwo = document.createElement("li");
listItemTwo.classList.add("list__item");
listItemTwo.textContent = "List item two";
list.appendChild(listItemTwo);
```

`DOM API`를 통해서 업데이트할 `HTML` 을 찾고, 새로운 요소를 생성하고, 속성을 추가하고, 추가하고, `DOM`을 업데이트시키고..

웹페이지의 크기가 커질수록 필요한 것만 선택하고 업데이트하는 것이 더욱 중요하다는걸 위의 예제만 봐도 알수있다.

## React를 한번 구현해보자.

### babel 컴파일러 설치

```
npm init
npm i @babel/cli @babel/core @babel/preset-react
```
### babel.config.json 파일 생성후 설정

```
{
    "presets": ["@babel/preset-react"]
}
```

### pacakge.json script추가

```
"scripts": {
   "build" :"babel src -d build -w"
}
```

### 폴더구성

![스크린샷 2022-02-08 오전 2 55 04](https://user-images.githubusercontent.com/56789064/152844597-0adc1a6a-f37d-4138-a1bf-1f68d233cae9.png)

### default index.html

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
    <h1>practice react</h1>
    <div id="root"></div>
    <script src="index.js" type="module"></script>
</body>
</html>
```

### script 실행

```
npm run build
```

`num run build`를 통해 `src` 폴더를 트랜스파일링 하여 `-d build`란 폴더를 만들어서 생성하고 

`-w` 을 통해서 변경사항이 파일에 적용되는 순간 새로 빌드하는 명령어이다.

## React는 html DOM을 이런식으로 관리하지않을까?

virtual dom은 html태그를 변환시키는 구조일 것이다. html이 아래처럼 생겼다면

```
<div id="root">
   <span>blabla</span>
</div>
```

아마도 버츄얼돔은 이런식으로 구현되지않을까?

```
{
    {
        tagName : 'div',
        props : {
            id: 'root'
            className : 'container'
        },
        children : [
            {
                tagName : 'span'           ,
                props : {},
                children : [
                    'blabla'
                ]
            }
        ]
    } 
}
```

이런 생각을 가지고, 아래의 단계를 하나하나 따라해보자.

## babel이 .jsx를 트랜스파일링하는 과정을 알아보자

### react.js

`react`에서 화면을 그리는 `render`와 `HTML`를 만드는 `createElement` 함수를 만들어보자.

```
export function render() {}

export function createElement() {}
```

### index.js

```
/* @jsx createElement */

import { createElement, render } from "./react.js";

function Title() {
  return <h2>동작을 할까?</h2>;
}

render(<Title />, document.querySelector("#root"));
```

`/* @jsx createElement */`는 지시문으로 `jsx`구문을 어떤 메소드로 변환시킬것인가를 정의하는 지시문이다. 

우리가만든 `createElement` 함수를 사용하여 `jsx`를 컴파일한다고 `babel`에게 알려주는 지시문이다.

리액트를 쓰듯이 `Title` 컴포넌트를 하나 만들어보고 저장을 한후 컴파일된 `build/index.js`를 보자.

### /build/index.js

```
/* @jsx createElement */
import { createElement, render } from "./react.js"; 

function Title() {
  return createElement("h2", null, "\uB3D9\uC791\uC744 \uD560\uAE4C?");
}

render(createElement(Title, null), document.querySelector("#root"));
```

`index.js`에서 `jsx`문법을 사용한것을 `babel` 트랜스 파일러가 `build`하여 만든 코드가 위와 같다.

`index.js`에서 만들었던 `jsx`를 리턴하는 `Title` 함수는 

`createElement("h2", null, "\uB3D9\uC791\uC744 \uD560\uAE4C?");` 이런식으로 변환된걸 알수있다.

우리가 리액트에서 사용하는 `jsx`는 `React.createElement(tagName,props,children)` 3가지 인자를 받아서 변환시키는것을 알수있다.

## createElement를 구현하여 VDOM을 만들어보자.

위에서 `babel`이 `jsx`를 `createElement(tagName,props,children)` 이런식으로 변환한다는것을 보았다.

`react.js` 파일에서 위 함수를 구현해보자.

```
export function createElement(tagName, props, ...children) {
  return { tagName, props, children };
}
```

`children` 자식으로 한두개가 아니기떄문에 가변인자(...)를 사용하였다.

`index.js` 파일을 아래와 같이 수정후에 저장후 `build/index.js`를 보자

```
// index.js

function Title() {
  return (
    <div>
      <h2>동작을 할까?</h2>
      <h2>잘 동작하는지 보자</h2>
    </div>
  );
}

console.log(Title());

====================

// build/index.js

import { createElement, render } from "./react.js"; 

function Title() {
  return createElement("div", null, 
      createElement("h2", null, "\uB3D9\uC791\uC744 \uD560\uAE4C?"), 
      createElement("h2", null, "\uC798 \uB3D9\uC791\uD558\uB294\uC9C0 \uBCF4\uC790") );
}
```
![스크린샷 2022-02-08 오전 3 42 38](https://user-images.githubusercontent.com/56789064/152851320-1c2a5ed6-0079-4927-9674-6e11254a9706.png)


`console.log(Title())`로 출력된 결과를 보면 우리가 `createElement()`에서 객체를 만들어서 리턴한 값을 보여준다.

그런데 우리는 단순히 객체만 리턴했을뿐인데 어떻게 재귀적으로 `children`안에서 객체를 생성했을까? `build/index.js` 에서 바뀐 코드를 보면 

`createElement("div", null, createElement("h2", null, "\uB.."), createElement("h2", null, "\uC.."));`

`children`이라는 가변인자마다 `createElement`를 다시 호출한것을 볼수있다.

## babel이 render를 트랜스파일링하는 과정을 알아보자

이번엔 `render`를 `babel`이 어떻게 어떻게 변환하는지 알아보자. 

우리가 리액트에서 렌더를 사용할때 `render(<Title />, document.querySelector("#root"));` 이런식으로

첫번째 인자에는 컴포넌트, 두번째 인자에는 업데이트할 컨테이너가 들어간다고 할수있다.

### react.js

```
export function render(component, container) {
    console.log(component)
}
```

과연 `component`에는 어떤값이 들어올지 한번 출력해보자.

![스크린샷 2022-02-08 오전 3 53 39](https://user-images.githubusercontent.com/56789064/152852919-324bd79f-fdba-402b-96cb-4e121925af72.png)

`console.log(component)`의 출력으로 `tagName`에는 `Title()` 함수가 들어가있는걸 볼수있다.

음.. 왜 함수가 넘어왔을까? 빌드된 결과물을 보자

## build/index.js

```
render(createElement(Title, null), document.querySelector("#root"));
```

`render`함수는 `<Title />`를 인자로 주면 `createElement`를 호출하고 인자로 `Title` 함수를 넘겨주고 속성은 없으니 `null`을 넘겨준것을 볼수있다.

그래서 우리가 `console.log`로 출력했을때 `tagName`이 `Title()`이 된것이다.

이것을 보면, `createElement` 함수는 `tagName`의 인자로 함수가 올수도있고, 리액트의 컴포넌트(`<Title />`)인 `string`이 올수도있다는것을 알수있다.

## createElement를 수정해주자.

<!-- 우리가 일반적인 호출로써 `jsx`로 만들어진 `component`를 만들면 createElement -->
### react.js

```
export function createElement(tagName, props, ...children) {
  if (typeof tagName === "function") {
    return tagName.apply(null, [props, ...children]);
  }
  return { tagName, props, children };
}
```
![스크린샷 2022-02-08 오전 3 53 39](https://user-images.githubusercontent.com/56789064/152852919-324bd79f-fdba-402b-96cb-4e121925af72.png)

![스크린샷 2022-02-08 오전 4 27 49](https://user-images.githubusercontent.com/56789064/152857997-28d48da6-b541-49fc-9a5b-ea82b3e19c5e.png)

위처럼 코드를 수정한후에 콘솔에 출력한 `component`를 이전과 비교해보면 `tagName`에 `Title()` 함수가 아닌 `vdom`이 잘 넘어온것을 볼수있다.

## render를 구현하여 진짜 UI를 만들어보자.

### react.js

```
// vdom => real dom
function renderRealDOM(vdom) {
  if (typeof vdom === "string") {
    return document.createTextNode(vdom);
  }

  if (vdom === undefined) return;

  const $el = document.createElement(vdom.tagName);

  vdom.children.map(renderRealDOM).forEach((node) => $el.appendChild(node));

  return $el;
}

export function render(vdom, container) {
  container.appendChild(renderRealDOM(vdom));
}
```

`render` 함수가 호출되면 우리는 기존의 노드 `container`에 변경점을 추가해주면 된다.

우리가 사용하는 `vdom`은 `html`이 아니기 때문에 `vdom`을 `html`로 변환해줄 `renderRealDOM` 함수를 만들었다.

#### if (typeof vdom === "string")

![스크린샷 2022-02-08 오전 4 46 56](https://user-images.githubusercontent.com/56789064/152860674-b6a15187-abed-4775-9cfd-7398c2728bdd.png)

마지막이 아닌경우 `{tagName, props, children}`의 구조이기 때문에 `children`이 마지막까지 도달하면 문자열만 남는걸 알수있다. 

재귀적으로 `renderRealDOM`함수를 호출하기때문에 재귀에서 벗어나기위한 방어코드이다.

#### if (vdom === undefined) return

만약에 vdom이 존재하지않을경우의 방어코드이다.

#### const $el = document.createElement(vdom.tagName);

`vdom`의 최상위 부모인 `tagName`의 요소를 만들어준다.

#### vdom.children.map(renderRealDOM).forEach((node) => $el.appendChild(node));

`vdom`의 `children`을 돌면서 `renderRealDOM`함수로 `vdom => html`로 변환된 `html`을 부모요소 `tagName`에 추가해준다

### 실행결과

![스크린샷 2022-02-08 오전 4 54 19](https://user-images.githubusercontent.com/56789064/152861675-616cf0f0-d862-4ead-a1ca-cf72a3ec33d2.png)

![스크린샷 2022-02-08 오전 4 55 57](https://user-images.githubusercontent.com/56789064/152861916-01e24bad-bf8d-47f7-8f5d-7964beea18f4.png)

우리가 만든 `vdom`이 `html`로 변환되어 부모요소에 잘 추가된것을 볼수있다.

### 정리

리액트에서 어떻게 `jsx`로 만들어진 컴포넌트를 `vdom`으로 만들고 `html`로 변환해 `render`가 이루어지는 작은 그림을 그려보았다.

하지만 우리가 여태해온 방법에서는 단순히 `jsx`를 변환하여 요소에 추가하는 것일뿐 진짜 리액트에서 `vdom`처럼 기존의 `dom`과 새로운 `dom`을 비교하여 바뀐부분만 변경하는 방식은 구현하지 않았다.

## 기존의 dom과 vdom을 비교하려면 어디서?

우리가 만든 코드에서는 어디서 비교할수있을까? `render()` 함수에서 비교해서 바뀐부분이 있다면 변경하도록 만들수 있을것이다.

```
export function render(vdom, container) {

    if(prevVdom !== nextVdom) {
      // 이부분..!     
    }

  container.appendChild(renderRealDOM(vdom));
}
```

음.. 그런데 우리는 `nextVdom`은 인자로 들어와서 알고있지만 ..

현재의 `html`을 만들어낸 이전의 `vdom`인 `prevVdom`은 어디서 관리하고 어디서 저장하고 있어야할까?

`render`는 함수인데.. 어떻게 이전상태를 가질수 있을까? **함수안에서 함수를 리턴하여 생기는 공간. 클로저를 이용한다!**

`render`함수를 아래처럼 변경해주자.

```
export const render = (function() {
    let prevVdom = null;

    return function(nextVdom,container) {

        if(prevVdom === null) {
            prevVdom = nextVdom
        }

        // diff
        // => prev와 next를 비교하는 알고리즘

        container.appendChild(renderRealDOM(nextVdom));      
    }
})()
```

![스크린샷 2022-02-08 오전 5 15 57](https://user-images.githubusercontent.com/56789064/152864640-cd7264ef-6b5e-437e-9007-4277d90f8b08.png)

()() 이 방식은 함수를 즉시호출하는 방법인데. 즉시호출하여 클로져 공간을 만드는것이다.

이렇게 `render`함수는 클로져 공간을 통해서 이전의 상태를 기억하여 이전의 `dom`과 현재의 `dom`을 비교할수 있게되었다.

리액트가 컴포넌트가 변경되었음을 비교를 할때의 기준은 불변성으로 판단하게 된다. 이 불변성은 얕은비교와 깊은비교로 나누는데

리액트의 경우 얕은비교(shallow comparison)로 객체가 바뀌었는지 판단한다

shallow Comparison에 대한 글은 [이곳](https://erurang.github.io/web/2021/05/06/js-spread2/)에 정리해두었다.

불변성에 대한 글은 [이곳](https://erurang.github.io/web/2021/05/06/js-immutability/)에 정리해두었다.