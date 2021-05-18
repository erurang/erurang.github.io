---
layout: post
title:  "Redux middleware"
subtitle: "Redux middleware"
categories: web
tags: redux
comments: true

---

### 리덕스 미들웨어란?

![스크린샷 2021-05-18 오전 4 12 38](https://user-images.githubusercontent.com/56789064/118543502-4aea2f80-b78f-11eb-9e2b-be610d4328cb.png)

원래 리덕스가 `dispatch(action) => reducer => store` 순서였다면, 

중간에 미들웨어를 추가해서 액션이 디스패치되어 리듀서로 넘어가기전에 다른일을 할수있도록 설정한것이다.

다른일이라고 하면.. 액션에서 설정된것과 다르게 수정할수도있고,

리덕스는 기본이 동기적인 작동인데, 미들웨어를 통해서 비동기적으로 만들수도 있다. 

리덕스 미들웨어는 하나의 함수인데... 아래와 같은 형식으로 쓰인다

```
const middleware = store => next => action => { // do something.. }
```

음 이게 뭐야? 저걸 풀어쓰면 아래와 같은데..

```
function middleware(store) {
  return function(next) {
    return function(action){
      .. do something..
    }
  }
}
```
함수를 리턴하는 함수를 리턴하는 함수다. -_- ; 

일단은 이전글에서 만든 카운터에서 살짝만 코드를 추가해서 맛보기만 해보자.

### 미들웨어 맛보기

src폴더에 myLogger.js를 만들어주자
```
const myLogger = (store) => (next) => (action) => {
  console.log(action);
  console.log("\t prev:", store.getState());
  const result = next(action);
  console.log("\t next:", store.getState());
  return result;
};

export default myLogger;
```

콘솔로그로 액션이과 상태들을 받아서 비교할수있도록 위에서 말한 미들웨어의 기본 함수형의 모습과 같이 만들어두었다. `index.js`에 아래와 같이 추가해주자.

```
import myLogger from "./middlewares/myLogger";
import { createStore, applyMiddleware } from "redux";

const store = createStore(rootReducer, applyMiddleware(myLogger));
```

`applyMiddleware`를 통해서 `store`에 미들웨어를 연결할수있다. 

우리가 임시로 만든 `myLogger`를 연결해준뒤에 액션을 만들어보자.

![middle](https://user-images.githubusercontent.com/56789064/118544973-f8aa0e00-b790-11eb-9842-c08977b3ab45.gif)

![스크린샷 2021-05-18 오전 4 25 38](https://user-images.githubusercontent.com/56789064/118545091-1bd4bd80-b791-11eb-81a2-66d40d04c602.png)

출력값을보면 액션,이전상태,변경후상태 가 나타나는걸 볼수있다. 

이제 액션 이전상태를 받아서 다음 상태로 넘기기전에 우리가 원하는 동작을 하도록 만들수있는것이다. 

위에선 우리가 로거를 따로만들었는데, 로거 라이브러리가 따로있다. 

`yarn add redux-loger` 설치후 `index.js`를 다음과 같이 수정후에 액션을 다시 주면..

```
import logger from "redux-logger";

const store = createStore(rootReducer, applyMiddleware(myLogger, logger));
```

![스크린샷 2021-05-18 오후 4 39 05](https://user-images.githubusercontent.com/56789064/118611237-91737480-b7f7-11eb-823e-a08e70ac1a84.png)

조금더 깔끔한 출력을 볼수있다. 물론 이 로거도 정말 좋지만.. 리덕스에서 크롬 익스텐션과 함께 아래 기능을 제공하는데

```
yarn add redux-devtools-extension

// index.js
import { composeWithDevTools } from "redux-devtools-extension";

const store = createStore(
  rootReducer,
  composeWithDevTools(applyMiddleware(logger))
);
```

![exte](https://user-images.githubusercontent.com/56789064/118612991-45c1ca80-b7f9-11eb-8895-06852297a5f9.gif)

`composeWithDevTools()`로 미들웨어를 묶어주자. 그다음 크롬의 `redux` 탭을 눌른후 액션을 만들면

각 액션마다 기록되어 그 액션이 일어난 시간으로 점프(이동)이 가능하도록 더 좋은 라이브러리를 이용할수있다.
