---
layout: post
title:  "Redux는 왜 생겼을까?"
subtitle: "Redux는 왜 생겼을까?"
categories: web
tags: react
comments: true

---

리덕스는 리액트의 상태관리를 도와주는 것일까? 노!

**리덕스는 자바스크립트의 상태관리를 도와주기위해 나타났다.** 리액트와 뷰로 유명해진것일뿐..

그럼 리덕스는 왜 생겼을까??

그것을 알기위해선 우린 아주아~~주 간단한 바닐라자바스크립트부터 시작해서 이해를 해보자.

일단 시작은 리액트를 깔아준뒤 파일들을 싹 날려주고 `index.html``index.js` 2개만 남겨놓자

요로코롬..
<img width="222" alt="스크린샷 2021-02-16 오전 4 28 11" src="https://user-images.githubusercontent.com/56789064/107986435-61a5ac80-700f-11eb-98a1-e98b14e8dbc1.png">

`index.js`는 아주 간단한 카운터를 하나 만들어주자

<img width="147" alt="스크린샷 2021-02-16 오전 4 29 08" src="https://user-images.githubusercontent.com/56789064/107986497-84d05c00-700f-11eb-8b97-04b68d3e3f86.png">

```
const add = document.querySelector("#add");
const minus = document.querySelector("#minus");
const number = document.querySelector("#span");

let count = 0;

number.innerText = count;

const handleAdd = () => {
  count = count + 1;
};
const handleMinus = () => {
  count = count - 1;
};

add.addEventListener("click", handleAdd);
minus.addEventListener("click", handleMinus);
```

우린 `handleAdd`와`handleMinus`로 카운트의 숫자를 조정하고있다. 그럼 add와 count를 눌러보면..

우리의 숫자는 반응하지않는다. 왜냐하면 우리는 카운트를 조정한건 맞으나.. html은 모르는거다. 바뀐지를 몰라요.

그래서 우린 아래 함수를 만들어줘서 현재의 count가 바꼇다는걸 알려야한다. 

```
const update = () => {
  number.innerText = count;
};
```
`handleAdd`와`handleMinus`에 `update()`를 추가하면 우리의 카운터가 정상작동하는것을 알수있다.

자. 위에서 시작할때 뭐라고 말하였는지 기억하나??

**리덕스는 자바스크립트의 상태관리**를 위해서 나타난것이다. 라고 말했던것을?

html에게 뭔가가 바뀌었다고 함수로 알려주는것이 리덕스의 기본이라고 할수있다.

다음엔 리덕스를 이용해서 카운터를 고쳐보자.
