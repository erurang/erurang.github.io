---
layout: post
title:  "상태를 수정하는 dispatch 와 action 과 subscribe"
subtitle: "상태를 수정하는 dispatch 와 action 과 subscribe"
categories: web
tags: redux
comments: true

---

리듀서에서 데이터의 상태를 변화시키는 방법에 대해 알아보자.

```
const reducer = (count = 0, action) => {
  console.log(action);
  return count;
};
const store = createStore(reducer);

store.dispatch({ type: "Hi" });
```

reducer에는 두번째 인자로 action을 받아올수있는데

action은 우리가 리듀서와 얘기할수있는 방법이라고 생각하면 된다.

얘기를 하기 위해선 `dispatch( { type: 해야할일 } )` 라는것을 사용하면된다.

위의 로그가 찍힌것을 보면 아래와같은데

<img width="278" alt="스크린샷 2021-02-16 오전 5 53 13" src="https://user-images.githubusercontent.com/56789064/107992056-4476db00-701b-11eb-9751-0b6662f1e9ea.png">

리덕스가 처음 실행될때(Init) 한번 실행되고 우리가 dispatch()로 불렀을때 실행되는것을 알수있다.

그럼 아래와같이 수정한후 결과를보자.

```
import { createStore } from "redux";

const add = document.querySelector("#add");
const minus = document.querySelector("#minus");
const number = document.querySelector("#span");

const reducer = (count = 0, action) => {
  if (action.type === "ADD") {
    console.log("숫자를 더합니다");
  }
  return count;
};
const store = createStore(reducer);

store.dispatch({ type: "ADD" });
```

<img width="110" alt="스크린샷 2021-02-16 오전 5 55 49" src="https://user-images.githubusercontent.com/56789064/107992239-a0d9fa80-701b-11eb-9064-2e271cdd7b5a.png">

우리가 디스패치를 통해 ADD라는 액션을 불렀고 리듀서에서는 액션의 타입이 add일때

숫자를 더합니다 라는 로그를 찍으라고 되어있다. 정상 실행된것을 볼수있다.

그럼 이것으로 더하고 뺴는것을 이렇게 만들면 결과는 어떻게나올까?

아래와같이 코드를 수정후 실행해보자.

```
import { createStore } from "redux";

const add = document.querySelector("#add");
const minus = document.querySelector("#minus");
const number = document.querySelector("#span");

const reducer = (count = 0, action) => {
  if (action.type === "ADD") {
    console.log("숫자를 더합니다");
    return count + 1;
  } else if (action.type === "Minus") {
    console.log("숫자를 뻅니다");
    return count - 1;
  } else {
    return count;
  }
};
const store = createStore(reducer);

store.dispatch({ type: "ADD" });
store.dispatch({ type: "ADD" });
store.dispatch({ type: "ADD" });
store.dispatch({ type: "Minus" });

console.log(store.getState());
```

<img width="282" alt="스크린샷 2021-02-16 오전 6 03 23" src="https://user-images.githubusercontent.com/56789064/107992751-af74e180-701c-11eb-9614-919fdc8b0471.png">

처음 리덕스가 실행됫을때의 초기값 0이 나온후 숫자의 추이를 볼수있다

이렇게 리덕스로 더욱더 쉬운 데이터(상태) 관리를 할수있게 되엇다.

이제 이것을 버튼으로 연결하려면 어떻게하면될까?

```
add.addEventListener("click", () => store.dispatch({ type: "ADD" }));
minus.addEventListener("click", () => store.dispatch({ type: "Minus" }));
```

요렇게 하면된다. 그런데 여기서 문제가 생긴다.

리덕스의 스토어를 통해서 데이터 관리는 할수있게 되었는데.. html이 카운터가 변하는지 알아채질못하는것이다.

이전에서 우리는 Update()를 통해 만들어주었는데 리덕스 스토어에는 5개의 함수가 있던것을 기억하는가?

그중에서 우린 `.subscribe()`를 이용할것이다. 이것은 리덕스의 스토어에 상태변화가 일어났다는걸

계속 지켜보고있는 함수라고 할수있다. 아래 코드를 추가후 결과물을보자.

```
store.subscribe(() => {
  console.log("상태변화가 일어남");
});
```
<img width="133" alt="스크린샷 2021-02-16 오전 6 13 35" src="https://user-images.githubusercontent.com/56789064/107993580-1ba41500-701e-11eb-9461-c0c3a123671a.png">

우리가 버튼을 클릭할때마다 상태변화가 일어났다는것을 알수있다.

그럼 subscribe를 이용해 상태변화가 일어날때마다 숫자가 변경되도록 아래와 같이 추가해주자.

```
const number = document.querySelector("#span");
...

store.subscribe(() => {
  number.innerText = store.getState();
});
```

![ddddda](https://user-images.githubusercontent.com/56789064/107993838-c4527480-701e-11eb-92eb-49ec5a426354.gif)

우리가 위에서 선언했던 숫자를 표현하는 number는 리덕스에서 상태변화가 일어날떄마다 숫자를 변경하는것을 알수있다.

우리가 리듀서에 지저한 if~else 구문은 switch로도 쓸수있다. 리덕스 공식문서엔 switch로 나와있다.

그리고 우리가 사용할 "ADD" "Minus" 변수들을 선언해서 사용하면 더욱더 보기좋고 편하다.

아래와 같이 수정해주고 실행해보자.
```
const ADD = "ADD"
const MINUS = "Minus"

switch (action.type) {
    case ADD:
      return count + 1;
    case Minus:
      return count - 1;
    default:
      return count;
  }
```

정상작동하고 우리가 보기에 더 편해진것을 알수있다.

다음엔 리덕스를 이용해 TODO리스트를 만들어보자.