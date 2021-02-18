---
layout: post
title:  "React-Redux Provider 와 Connect"
subtitle: "React-Redux Provider 와 Connect"
categories: web
tags: redux
comments: true

---

이번엔 리액트로 Todo를 만들며 React-redux에 적응해보자.

## 리덕스를 이해하기위한 기본 틀 및 설치

> yarn add create-react-app
> yarn add react-redux

우린 라우터를 쓸것이기때문에 리액트의 라우터중 해쉬라우터도 설치해주자

> yarn add react-router-dom

폴더를 아래와같이 만들어주자.

<img width="218" alt="스크린샷 2021-02-16 오전 7 50 11" src="https://user-images.githubusercontent.com/56789064/107999767-9b84ac00-702b-11eb-909c-724a23122157.png">

App.js
```
import React from "react";
import { HashRouter as Router, Route } from "react-router-dom";
import Detail from "../routes/Detail";
import Home from "../routes/Home";

function App() {
  return (
    <Router>
      <Route path="/" exact component={Home}></Route>
      <Route path="/:id" component={Detail}></Route>
    </Router>
  );
}

export default App;
```
App.js 에서는 각 라우터들에 맞게 컴포넌트가 화면을 표시할수있도록 만든다.

routes/Home.js
```
import React, { useState } from "react";

function Home() {
  const [text, setText] = useState("");

  function onChange(e) {
    setText(e.target.value);
  }
  
  function onSubmit(e) {
    e.preventDefault();
    console.log(text);
  }

  return (
    <>
      <h1>To Do!</h1>
      <form onSubmit={onSubmit}>
        <input type="text" value={text} onChange={onChange} />
        <button>Add</button>
      </form>
      <ul></ul>
    </>
  );
}

export default Home;
```
routes/Home.js 에서는 우리의 기본 ToDo의 형식을 만들어주었다.

다음으론 우리가 원하는 데이터의 변화를 저장할 store.js 파일을 만들어준다.

store.js
```
import { createStore } from "redux";

const ADD = "ADD";
const DELETE = "DELETE";

export const addToDo = (text) => {
  return {
    type: ADD,
    text,
  };
};

export const deleteToDo = (id) => {
  return {
    type: DELETE,
    id,
  };e
};

const reducer = (state = [], action) => {
  switch (action.type) {
    case ADD:
      return [{ text: action.text, id: Date.now() }, ...state];
    case DELETE:
      return state.filter((toDo) => toDo !== action.id);
    default:
      return state;
  }
};

const store = createStore(reducer);

export default store;
```

지금까지는 우리가 자바스크립트에서 했던것과 큰 차이가 없다. `.subscribe()`함수를 기억하고있나?

리듀서의 상태가 변화가 되었을때를 지켜보는 함수였었다.

## 리액트를 리덕스와 연결하기

우리의 리액트가 리덕스의 store에서 데이터변화가 일어날때 react가 바뀐부분을 render해주도록 알려주기위해서

다음과 같은 방법이 있다.

`react-redux`에 Provider라는것을 제공한다.

첫번쨰로 index.js에서 `react-redux`의 Provider를 의 App의 상위 컴포넌트로 만들어주자.

store는 우리가 만든 store를 연결해주는것이다.

```
import React from "react";
import ReactDOM from "react-dom";
import App from "./components/App";
import { Provider } from "react-redux";
import store from "./store";

ReactDOM.render(
  <Provider store={store}>
    <App />
  </Provider>,

  document.getElementById("root")
);
```

두번째로 우리는 redux state로부터 정보를 가지고 와야한다.

`store.getState()` 가 현재의 state를 돌려준다고 하였다.

redux에는 `connect()`라는 함수가 있다. 이건 말 그대로 내 컴포넌트를 store에 연결해주는것이다.

`connect`는 `state` `dispatch` 2개의 인자를 넣을수있는데 이건 관용적으로

`mapStateToProps mapDispatchToProps` 이렇게 2개로 명명해서 쓴다.

그중에 첫번째것부터 먼저 알아보자.

Home.js 인데 메인화면이라 생각~

```
import React, { useState } from "react";
import { connect } from "react-redux";

function Home(props) {
  console.log(props);
  const [text, setText] = useState("");
  function onChange(e) {
    setText(e.target.value);
  }
  function onSubmit(e) {
    e.preventDefault();
    console.log(text);
    setText("");
  }

  return (
    <>
      <h1>To Do!</h1>
      <form onSubmit={onSubmit}>
        <input type="text" value={text} onChange={onChange} />
        <button>Add</button>
      </form>
      <ul></ul>
    </>
  );
}

function mapStateToProps(state, props) {
  return { IamRedux: "Redux가 컴포넌트에 props로 쏴줌" };
}

export default connect(mapStateToProps)(Home);
```

여기서 볼것은 `Home(props)` props로 받아온것을 콘솔찍을때의 결과다.

아래의 이 부분을보면
```
function mapStateToProps(state, props) {
  return { IamRedux: "Redux가 컴포넌트에 props로 쏴줌" };
}

export default connect(mapStateToProps)(Home);
```

return이 된 부분이 뭐냐면 우리가 리덕스에 값을 넘겨준것이다.

그리고 그 넘겨진값은 컴포넌트의 props에 자동으로 올라가 받아올수있다.

![image](https://user-images.githubusercontent.com/56789064/108332440-24117100-7213-11eb-8014-b440ebeab569.png)

우리가 만든 IamRedux 가 props에 찍히는걸 볼수있다.

세번째로 데이터를 변화시켜보자.



