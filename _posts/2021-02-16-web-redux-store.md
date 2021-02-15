---
layout: post
title:  "Store와 Reducer"
subtitle: "Store와 Reducer"
categories: web
tags: redux
comments: true

---

리덕스의 createStore를 사용해보자.

> yarn add redux

우리가 이전에 만든 카운터의 Index.js에 아래와 같이 추가해주자

> import { createStore } from "redux"

store는 뭘까? 이곳은 내가 상태관리를 하고싶은 data를 넣는 장소다.

상태관리 즉 state는 뭘까? state는 변하는 data를 말한다.

이전에 우리가 만들었던 index.js에서 변하는 data는 무엇일까?

> let count = 0;

카운트밖에없다. +1을하거나 -1을하거나.

즉 우리는 store에 변하는 data를 넣어주고 리덕스는 그 data의 변화되는 상태를 관리해준다.

index.js의 아래와 같이 다시 수정해보자.

```
import { createStore } from "redux";

const add = document.querySelector("#add");
const minus = document.querySelector("#minus");
const number = document.querySelector("#span");

const store = createStore();

```

저장후 웹을 보면 아래와 같은 경고가 뜨는것을 볼수있는데.

<img width="448" alt="스크린샷 2021-02-16 오전 5 16 29" src="https://user-images.githubusercontent.com/56789064/107989695-222e8e80-7016-11eb-9cda-40a48bea2a58.png">

아래와같이 코드를 변경해주자 오류는 사라질것이다.

```
const reducer = () => {
  // 예시를위한
  // return 'hello'
};
const store = createStore(reducer);
```

즉 오류가 뜬 이유는 데이터를 관리하는 장소(store)를 만들어주었으니 리덕스는 나에게

**이 data를 수정/변경할수있는 reducer라는것을 만들어주세요 라는 뜻이다.**

그럼 store에는 무엇이 들어있는것일까?

> console.log(store)
> console.log(store.getState());

<img width="345" alt="스크린샷 2021-02-16 오전 5 23 41" src="https://user-images.githubusercontent.com/56789064/107990158-24451d00-7017-11eb-95b9-f7ae328e3b52.png">

store에는 5개의 함수가있고 그중의 `getState()`를 실행하면 우리가 reducer에서 return한것을 받아오는걸 알수있다.

즉 reducer가 return하는것은 내 data라는것을 알수있다.

그럼 위에서 리듀서는 data를 수정/변경 할수있도록 해준다고 하였다.

오직 리듀서 안에서만 우리가 지정한 data를 수정할수있다는 것이다.

그럼 아래와같이 코드를 수정후 실행해보자

```
const reducer = (count = 0) => {
  return count;
};
console.log(store.getState())
```

우리가 default로 지정한 count가 return되는것을 알수있다.

그럼 이제 reducer안에서 숫자를 더하거나 뺴거나 하는 기능을 추가해야하는데.. 이건 어떻게 할수있을까?

다음으로 Action이라는 기능을 알아보자