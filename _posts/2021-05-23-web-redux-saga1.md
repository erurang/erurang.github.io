---
layout: post
title:  "redux-saga"
subtitle: "redux-saga"
categories: web
tags: redux
comments: true

---

### redux-saga란?

여태까지 배운 `redux`와 `redux-thunk`의 차이를 한번 훑어보자.

`redux`는 `{type,action}`을 이용해 액션 객체를 만들어서 동기적으로 처리했다.

`redux-thunk`는 함수를 디스패치할수있도록 액션이 액션을 부르는 형태로 이루어져있었고, 비동기적으로 처리할수있었다.

그럼 `redux-saga`는 어떤 특징이 있을까? `saga`는 `thunk`의 비동기처리를 조금더 매끄럽게 해주고, 

`Generator`라는 `javascript` 문법을 사용한다. 그리고 액션을 모니터링해서 특정 액션이 발생할때 특정 작업을 할수있게 해준다.

비동기 작업을 진행할때 기존 요청을 취소할수도있고, 비동기 요청이 시작될때 첫요청/마지막요청만 처리하게 만들수도있다.

이렇게 비동기적 처리를 조금더 간편하게 해주면서, `websocket` 통신도 쉽게 이용할수있도록 `channel` 형식을 지원한다.

즉 비동기 처리 작업을 `saga`를 사용해 더 매끄럽게, 더 편하게 도와준다.

그리고 함수를 `dispatch` 하는것이 아닌 순수 액션 객체로만 액션을 실행한다는 것이다.

### redux-saga 설치

`npm i redux-saga` 설치 후에 index.js의 리덕스 스토어에 연결해준다.

```
import createSagaMiddleWare from "redux-saga"
const sagaMiddleware = createSagaMiddleWare();

const store = createStore(
  rootReducer,
  composeWithDevTools(applyMiddleware(ReduxThunk, sagaMiddleware, logger))
);

sagaMiddleware.run(rootSaga);
```

`thunk`에서는 `store`와 리듀서를 연결해주는것으로 끝났지만 `saga`는 `run([saga함수])` 를 해줘야만 제대로 작동한다.

`rootSaga`를 만들기 전에 `Generator` 문법에대해 공부하고 가자

### Generator!

`redux-saga`는 `javascript의 Generator` 문법을 사용한다. 

`Generator`의 특징은 함수의 흐름을 특정구간에 멈추었다가 다시 실행할수있다는 것이다.

예제로 익혀보자, 만약에 아래와 같은 함수가 있다고 하자

```
function() {
  return 1;
  return 2;
  return 3;
  return 4;

}
```

위 함수는 한 스코프에 리턴이 4개나있다. 이건 무조건 `return 1` 만 작동하게되어있다. 

저런경우를 제네레이터 문법으로 사용할수있는데 `function`옆에 `*`을 붙여서 `function*` 로 선언한다.

```
function* generatorF() {
  console.log('1')
  yield 1;
  console.log('2')
  yield 2;
  console.log('3')
  yield 3;
  return 4
}
```

![스크린샷 2021-05-22 오후 9 11 55](https://user-images.githubusercontent.com/56789064/119226046-57c9a300-bb42-11eb-87ae-ead2749f944b.png)

실행해보면 `generatorF state : <suspended>` 를 볼수있는데 함수를 처음시작할때 멈춰있는 상태를 뜻한다. 

제네레이터를 시작하려면 `next()`를 사용하면 시작이된다.

![스크린샷 2021-05-22 오후 9 15 41](https://user-images.githubusercontent.com/56789064/119226196-de7e8000-bb42-11eb-9012-8286e102ffbd.png)

`next()`를 시작할떄마다 `{value,done}` 이 리턴되는걸 볼수있는데, 한번 실행될때마다 `yield`의 값이 `value`로 리턴되는걸 볼수있다.

모든 호출이 끝난뒤에 a를 다시 실행해보면 `generatorF {<closed>}` 라고 닫힌것을 볼수있다.

즉 함수가 실행되면 `yield`의 특정구간에서 멈춰놓았다가, 다시 실행할수있고, 제네레이터가 닫히기전까지 결과값을 보낼수있다는걸 알수있다.

### Redux-saga 사용해보기

간단한 예시로 앞에서 만들었던 카운터에 사가를 살짝쿵 입혀볼것인데 코드의 차이를 느껴보자.

### thunk로 만든 비동기 카운터 코드

```
const SET_DIFF = "counter/SET_DIFF";
const INCREASE = "counter/INCREASE";
const DECREASE = "counter/DECREASE";

// 액션함수 작성
export const setDiff = (diff) => ({ type: SET_DIFF, diff });
export const increase = () => ({ type: INCREASE });
export const decrease = () => ({ type: DECREASE });

// thunk를 위한 액션함수 작성
export const increaseAsync = () => (dispatch) => {
  setTimeout(() => {
    dispatch(increase());
  }, 1000);
};

export const decreaseAsync = () => (dispatch) => {
  setTimeout(() => {
    dispatch(decrease());
  }, 1000);
};
```

`thunk`는 액션에서 액션을 리턴하여 비동기적으로 만든것을 볼수있다. 액션에 함수가 끼여있어서 깔끔하지못하기도하다.

### `saga`로 만든 비동키 카운터 코드
```
import { delay, put, takeEvery, takeLatest } from "redux-saga/effects";

const INCREASE_ASYNC = "INCREASE_ASYNC";
const DECREASE_ASYNC = "DECREASE_ASYNC";

// 액션함수 생성
export const increaseAsync = () => ({ type: INCREASE_ASYNC });
export const decreaseAsync = () => ({ type: DECREASE_ASYNC });

function* increaseSaga() {
  // delay 몇초동안 기다려!
  yield delay(1000);

  // put 특정 액션을 디스패치해!
  yield put(increaseAsync());
}

function* decreaseSaga() {
  yield delay(5000);
  yield put(decreaseAsync());
}

export function* counterSaga() {
  yield takeEvery(INCREASE_ASYNC, increaseSaga);
  yield takeLatest(DECREASE_ASYNC, decreaseSaga);
}
```
형식적으로 함수명 끝에 `Saga`를 붙여준다.

`counterSaga()`안의 함수를 보면 `*(액션명,액션함수)`로 액션이 실행되면 액션함수를 `*`형식으로 실행한다는 뜻이다.

위의 카운터 코드를 예로 `INCREASE_ASYNC` 액션이 디스패치되면 `increaseSaga()`를 `takeEvery` 매번 실행합니다. 라는뜻이다.

`saga`에서는 순수 액션 객체를 통해 액션이 실행되어 조금더 깔끔한것을 볼수있다. 

### effects 주요함수

`delay`
- 설정된 시간 이후에 resolve하는 Promise객체를 리턴한다.
- 예시: delay(1000)

`all`
- 제너레이터 함수를 배열의 형태로 인자로 넣어주면, 제너레이터 함수들이 병행적으로 동시에 실행된다.
그리고 전부 `resolve`될때까지 기다린다. `Promise.all`과 비슷하다고 보면된다. 
- 예시: `yield all([testSaga1(), testSaga2()])` testSaga1()과 testSaga2()가 동시에 실행되고, 
모두 resolve될 때까지 기다리게 된다. 

`put`
- 특정 액션을 dispatch하도록 한다. 예시: put({type: 'INCREMENT]})

`takeEvery`
- 들어오는 모든 액션에 대해 특정 작업을 처리해 준다.

`takeLatest`
- 기존에 진행 중이던 작업이 있다면 취소 처리하고 가장 마지막으로 실행된 작업만 수행한다.

`call`
- 함수의 첫 번째 파라미터는 함수, 나머지 파라미터는 해당 함수에 넣을 인수이다.
- 예시: call(delay, 1000) →delay(1000)함수를 call함수를 사용해서 이렇게 쓸 수도 있다.

### rootSaga 만들기

이제 `counterSaga()`를 `store`에 연결해줘야한다.

`module/index.js`에서 `store`에 연결시키기위한 `saga`를 만들자.

```
import { all } from "redux-saga/effects";

export function* rootSaga() {
  yield all([counterSaga()]);
}

export default rootReducer;
```

### rootSaga store에 연결

`index.js`에서 사가를 연결시키기

```
import rootReducer, { rootSaga } from "./modules/index";
import createSagaMiddleWare from "redux-saga";

const sagaMiddleware = createSagaMiddleWare();

const store = createStore(
  rootReducer,
  composeWithDevTools(applyMiddleware(ReduxThunk, sagaMiddleware, logger))
);

sagaMiddleware.run(rootSaga);
```

중요한것은 스토어에 `saga`를 연결후에 `run()`을 스토어 다음으로 실행해줘야한다.

만약 `store`보다 먼저 실행시키면 오류가 뜨게되는데

![스크린샷 2021-05-23 오전 12 56 13](https://user-images.githubusercontent.com/56789064/119232868-ad617800-bb61-11eb-90e1-6aa757c47f38.png)

실행시키기전에 미들웨어를 먼저 마운트 시키라는 오류가 뜨게된다. `run()`을 하지않으면 `counterSaga()` 작동이 되지않게된다.

이제 카운터의 `+ / -` 버튼을 눌러보면서 `takeEvery/Latest`의 차이를 보자.

![11](https://user-images.githubusercontent.com/56789064/119266851-6473f700-bc27-11eb-8b79-b03d76368804.gif)


`+` 버튼을 누를떄는 모든 액션이 처리되는 반면 `-` 버튼에는 딱 한번만 처리되는걸 볼수있다.