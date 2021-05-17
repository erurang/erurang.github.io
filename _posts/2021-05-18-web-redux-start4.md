---
layout: post
title:  "Redux를 React에 연결하기 모듈 만들기"
subtitle: "Redux를 React에 연결하기 모듈 만들기"
categories: web
tags: redux
comments: true

---

### 리덕스를 리액트에 연결, 컴포넌트 분리, 모듈

리덕스를 사용할때 대부분의 프로젝트에서는 액션과 리듀서 스토어를 각각의 폴더에 각 파일로 지정해놓는다. 

[NaverD2 React 적용가이드 - React와 Redux](https://d2.naver.com/helloworld/1848131) 이곳의 폴더구조를 보면 `action component reducer store` 역할별로 파일을 나눈것을 볼수있다.

위의 링크를 보면 이런말이 있다. 컴포넌트는 **컨테이너 컴포넌트**와 **프레젠테이션 컴포넌트**를 구분해 개발한다.

컨테이너 컴포넌트는 데이터를 관리, 프레젠테이션은 컴포넌트에서 받은(props)로 화면을 그려주는 역할에 맞춰야 한다는것이다.

나는 이번글을 쓰면서 컴포넌트를 구분해서 공부를 할것이지만 리덕스를 역할별로 나누지않고 `ducks pattern`을 사용할것이다.

`action`과 `reducer`를 같은 파일에 정의해주는것이다. 정의해줄떄 주의해야할점은 리듀서는 `export default` 액션은 `export` 를 사용하여 모듈화 시켜야 한다는 것이다.

Counter 예시를 만들어보면서 컴포넌트를 나누고 덕패턴으로 리덕스를 구성하면서 공부해보자.

### 설치

```
npx i create-react-app
npx i redux
npx i react-redux
```

### 모듈만들기

리액트를 설치후 `src`폴더에 `modules``containers``component` 3폴더를 만들어주자.

모듈 폴더에 `counter.js`을 생성 후 리듀서와 액션을 만들어주자.

```
// 다른 모듈과 중복되지않기위해 앞에 구분/액션 적어줌
const SET_DIFF = "counter/SET_DIFF";
const INCREASE = "counter/INCREASE";
const DECREASE = "counter/DECREASE";

// 액션함수 작성
export const setDiff = (diff) => ({ type: SET_DIFF, diff });
export const increase = () => ({ type: INCREASE });
export const decrease = () => ({ type: DECREASE });

// 초기상태 선언
const initialState = {
  number: 0,
  // 기본값
  diff: 1,
};

// 리듀서는 export default로 내보냄
export default function counter(state = initialState, action) {
  switch (action.type) {
    //   카운트값변경
    case SET_DIFF:
      return {
        ...state,
        diff: action.diff,
      };
    case INCREASE:
      return {
        ...state,
        number: state.number + state.diff,
      };
    case DECREASE:
      return {
        ...state,
        number: state.number - state.diff,
      };
    default:
      return state;
  }
}
```

액션과 리듀서를 만들었으니 다음에 해야할것은? 리듀서를 스토어에 연결하는것이다.

### 리듀서를 스토어에 연결하기

모듈폴더에 `index.js` 생성 후 스토어를 만들어주자. 근데 아래 방식은 일반적으로 reducer하나만 지정하는 방식과는 다른방식이다.

여러개의 리듀서&액션 을 스토어에 적용시키기위해 combineReducers를 제공한다. 

```
// root reducer
import { combineReducers } from "redux";
import counter from "./counter"; // 리듀서 모듈을 가져와서 적용

const rootReducer = combineReducers({
  counter,
  // .... 만든 Reducer 들을 합쳐줌
});

export default rootReducer;
```

이 다음에 해야할것은? 스토어를 리액트에 연결하는것이다.

### 스토어를 리액트에 연결하기

Provider를 통해서 App컴포넌트 상위로 묶어주고 store와 reducer를 연결해주자.

```
import { Provider } from "react-redux";
import { createStore } from "redux";
import rootReducer from "./modules/index";

const store = createStore(rootReducer);

ReactDOM.render(
  <Provider store={store}>
    <App />
  </Provider>,
  document.getElementById("root")
);
```

### 컴포넌트를 만들기 전에 이 그림을 눈으로 보고 로직을 이해해보자.

![스크린샷 2021-05-18 오전 12 22 06](https://user-images.githubusercontent.com/56789064/118513927-15354e80-b76f-11eb-85fb-015d4df6e3f1.png)


1. 리액트 컴포넌트에서 Action Creator가 호출된다.
2. 액션 생성자가 액션을 생성한다. 생성한 액션(타입,데이터)를 디스패치를 통해 스토어로 넘긴다.
   1. 액션을 생성하는 과정에서 비동기적 호출을 실행할수도있고 안할수도있다.
   2. 리덕스 자체는 동기적으로 작동하기떄문에 Middleware를 이용하여 비동기적으로 작동하게 만들수있다
   3. redux-saga -thunk가 존재한다.
3. 리듀서가 현재상태와 받은상태를 비교하여 새로운 상태를 스토어에 다시 넘긴다.
4. 스토어는 상태트리를 다시 만든후 컴포넌트로 넘김
5. 리액트가 새로운 상태트리에 맞춰 버츄얼돔을 렌더링하고 바뀐부분만 업데이트를 실행한다.

### 컨테이너 컴포넌트

위에서 컴포넌트는 2가지로 나누어야한다고 했다. 컨테이너는 데이터 로직을 담당하도록 만들것이다.

`containers`폴더에 `CounterContainer.js`를 만들어주자.
```
import React from "react";
import Counter from "../components/Counter";

const CounterContainer = () => {
  return ();
};

export default CounterContainer;
```
리액트 훅에는 `useSelector, useDispatch`가 존재한다. 

셀렉터는 리액트 컴포넌트에서 리덕스 상태를 조회할때 사용한다. 즉 `getState()`와 같은 기능이다.

아래와 같이 추가해주자

```
import React from "react";
import Counter from "../components/Counter";
import { useSelector, useDispatch } from "react-redux";
import { decrease, increase, setDiff } from "../modules/counter";

const CounterContainer = () => {
  return ();
};

export default CounterContainer;
```

여기서 컨테이너안에 리덕스의 데이터를 가져오고 액션을 생성해줘야한다.

```
const { number, diff } = useSelector((state) => ({
    number: state.counter.number,
    diff: state.counter.diff,
  }));

  const dispatch = useDispatch();

  const onIncrease = () => dispatch(increase());
  const onDecrease = () => dispatch(decrease());
  const onSetDiff = (diff) => dispatch(setDiff(diff));
```

셀렉터를 이용해 현재 상태에서 `state.counter.number / diff` 를 가져오는데 `state.[모듈명].변수` 형식이다.

디스패치를 통해 액션이 일어나면 행해야할 함수를 지정해주자. 그리고 프레젠테이션 컴포넌트를 생성해주자.

src 폴더의 components에 Counter.js를 만들어준후 컨테이너와 연결해주자. 모든 코드는 아래와 같다.
```
import React from "react";
import Counter from "../components/Counter";

import { useSelector, useDispatch } from "react-redux";
import { decrease, increase, setDiff } from "../modules/counter";

const CounterContainer = () => {
  // state는 state.getState()값임
  const { number, diff } = useSelector((state) => ({
    number: state.counter.number,
    diff: state.counter.diff,
  }));

  const dispatch = useDispatch();

  const onIncrease = () => dispatch(increase());
  const onDecrease = () => dispatch(decrease());
  const onSetDiff = (diff) => dispatch(setDiff(diff));

  return (
    <Counter
      number={number}
      diff={diff}
      onIncrease={onIncrease}
      onDecrease={onDecrease}
      onSetDiff={onSetDiff}
    />
  );
};

export default CounterContainer;

```

### 프레젠테이션 컴포넌트

프레젠테이션 컴포넌트는 정말 받은 데이터를 표시만 해주는 역할만 맡으면 된다.

```
import React from "react";

const Counter = ({ number, diff, onIncrease, onDecrease, onSetDiff }) => {
  const onChange = (e) => {
    onSetDiff(parseInt(e.target.value));
  };

  return (
    <div>
      <h1>{number}</h1>
      <input type="number" value={diff} onChange={onChange} />
      <button onClick={onIncrease}>+</button>
      <button onClick={onDecrease}>-</button>
    </div>
  );
};

export default Counter;
```

![counter](https://user-images.githubusercontent.com/56789064/118518511-28e2b400-b773-11eb-85c1-3b77dd1ca889.gif)

잘 작동하는것을 볼수있다. 다음엔 여기에 ToDoList를 추가하는 예시로 한번더 공부를 해보자.