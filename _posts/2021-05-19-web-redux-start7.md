---
layout: post
title:  "Redux thunk 맛보기"
subtitle: "Redux thunk 맛보기"
categories: web
tags: redux
comments: true

---

### 함수를 디스패치할수있는 thunk / saga

`thunk`는 객체가 아닌 함수를 디스패치 할수있도록 해준다. 

그러니까 기존의 리덕스는 객체`{}`로 이루어진 형식만 디스패치가 가능했다고 보면 된다.

`thunk`를 사용하지않고 `function`을 액션에 주면 어떻게될까? 궁금해서 해봤는데..

![스크린샷 2021-05-19 오전 1 51 16](https://user-images.githubusercontent.com/56789064/118692131-b510dc00-b844-11eb-8049-3a0cdae80f66.png)

무시무시한 빨간줄 에러를 구경하게되었다.. 리덕스에서 비동기작업/함수작업... 을 하기위해 만들어진 미들웨어의 종류엔 2가지가 있다.

`thunk`와 `saga`인데 일단 `thunk`부터 공부해보자 

(왜 thunk부터? 그건 나도몰라.. 나중에 사가 공부해보면 차이를 알겠지..)

#### thunk의 기본 문법 형식은 아래와같다.

```
const thunk = store => next => action =>
  typeof action === 'function'
    ? action(store.dispatch, store.getState)
    : next(action)
```

젤 안쪽부터 함수가 리턴되면서 `action타입이 함수라면 ? 액션(디스패치,현재상태) : 다음액션`이다. 

솔직히 보기만해서는 뭔지 모르겠다 진짜로. 리덕스 공부하다가 머리다빠질거같다. 그래도 공부해야지...

이전의 예제를 계속 이어서 공부해보자.

### 떵크 설치

`yarn add redux-thunk` 설치한 후에 `redux-thunk` 설정파일을 보면 아래와 같이 선언되있다.

```
function createThunkMiddleware(extraArgument) {
  return ({ dispatch, getState }) => next => action => {
    if (typeof action === 'function') {
      return action(dispatch, getState, extraArgument);
    }

    return next(action);
  };
}

const thunk = createThunkMiddleware();
thunk.withExtraArgument = createThunkMiddleware;

export default thunk;
```

```
if (typeof action === 'function') {
      return action(dispatch, getState, extraArgument);
    }
```

흠 `extraArgument?` 이건뭐야...? 일단 이건 제외하고.. 

나머지는 위의 기본 형식과 똑같이 선언된걸 볼수있다. 뒤에서 차근히 배울수있지 않을까?

### 리덕스떵크를 컴포넌트에 연결하기

`index.js`의 `store`에 `applyMiddlware`의 인자로 `ReduxThunk`를 넣어서 연결해주자

```
import ReduxThunk from "redux-thunk";

const store = createStore(
  rootReducer,
  composeWithDevTools(applyMiddleware(ReduxThunk, logger))
);
```

일단 스토어와 연결은 끝났다. 그럼 그다음엔? 뭘 해야할까? 이전과 같이 액션과 리듀서를 지정해줘야한다.

### 액션/리듀서 만들기

기존의 `module/counter.js`의 `increase()`와 `decrease()`를 아래처럼 수정해주자

```
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

서버와통신하는 시간을 1초라고 가정해보고, `setTimeout`을 이용하여 1초 뒤 디스패치(액션)를 통해 리듀서로 기존의 `increase decrease`가 넘어갈것이다.

### 컨테이너에 수정

액션을 수정했으니 컨테이너의 함수도 바뀐 액션으로 고쳐줘야한다.

기존 컨테이너에서 디스패치될 함수를 `increaseAsync(),decreaseAsync()`로 고쳐주자.

```
// import { decrease, increase, setDiff } from "../modules/counter";
// 아래로변경
import { increaseAsync, decreaseAsync, setDiff } from "../modules/counter";

// const onIncrease = () => dispatch(increase());
// const onDecrease = () => dispatch(decrease());

// 아래로 변경
const onIncrease = () => dispatch(increaseAsync());
const onDecrease = () => dispatch(decreaseAsync());
```

그럼 이제 버튼이 클릭이 된다고 가정하면 다음과 같은 순서로 리덕스에서 상태변화가 일어날것이라고 예측할수있다.

1. +/- 증감버튼을 누른다
2. `dispatch(increaseAsync())/dispatch(decreaseAsync())` 둘중하나가 작동한다.
3. 1초뒤 증가/감소 액션함수가 작동된다
4. 리듀서에서 액션타입에 맞는 일이 일어난다
5. 상태가 변경된다

우리가 1초뒤로 설정한것이 잘 작동하는걸 볼수있다.

![ㅊ](https://user-images.githubusercontent.com/56789064/118622551-63476200-b802-11eb-8362-369b3f97bf87.gif)

아 진짜.. 어렵다.. 단순한 리덕스 이해가 될라그러니깐 이젠 함수에 함수를 몰고오는.. 미들웨어가 머리아프게한다.

간단한 상태관리는 할수있겠는데.. 이걸로 어떻게 또 실시간으로 구현한다는건지 참 -_-.. 고수의 길은 멀고도멀다.

이번에 생긴 네이버 쇼핑 라이브, 리액트랑 리덕스던데.. 진짜 존경스럽다. 난 언제 저렇게 만들어볼까