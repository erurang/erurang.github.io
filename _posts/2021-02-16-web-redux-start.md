---
layout: post
title:  "Redux는 왜 생겼을까?"
subtitle: "Redux는 왜 생겼을까?"
categories: web
tags: redux
comments: true

---

리덕스가 왜 생겼을까? 아래 사진과 같이 우리가 화면을 3개 가지고 있다고 가정해보자.

![image](https://user-images.githubusercontent.com/56789064/108443480-3388cc80-729c-11eb-8d70-cb7233cd6390.png)

각 화면에서 E와 아바타(형광펜)을 가져와야한다고 할때. 우린 각 화면마다 API를 요청해서 화면을 받아오게된다.

만약 이것이 사용하는 사람이 많으면? 그럼 같은 정보를 쓰지만 다른화면에서 똑같은 API를 중복으로 계속 요청받아 쓰게되어 좋지않다.

이것을 해결해보기위해 사람들은 차라리 API를 한번 받아서 큰 컨테이너안에서 props를 이용해 아래로 정보를 넘겨주자! 였다.

![image](https://user-images.githubusercontent.com/56789064/108444373-d857d980-729d-11eb-85e3-31aa713a76f8.png)

와우 정말 잘 생각한거같다. 한번만 API를 받아서 그것을 화면마다 Props로 넘겨주기만하면되니깐!

음.. 그런데 위와같은 예제에서는 props에서 받아서 화면에 넘기고 그 화면이 받은 정보를 표시해주고.. 2가지 단계인데

만약에 그사이에 다른 컴포넌트들이 존재해서 props -> 화면 -> 바디 -> 바디안의어떤 컴포넌트.... -> .. -> 정보표시!

이런 단계라고 생각을하면 우리는 정보표시를 하기위해서 props에 props에 props에 props에 .. 를 계속 반복해야한다.

이런 반복은 정말 좋지않다.. 그래서 필요한 컴포넌트에서 props이용없이 데이터를 가져오기위해 생겨난것이 redux와 mobx이다.

하나의 데이터 센터에서 컴포넌트가 필요로할때 바로 컴포넌트에 직접적으로 데이터를 쏴주는것이다.

**리덕스는 자바스크립트의 상태관리를 도와주기위해 나타났다.** 리액트와 뷰로 유명해진것일뿐..

리덕스의 작동 방식을 이해하기위해 우린 아주아~~주 간단한 바닐라자바스크립트부터 시작해서 이해를 해보자.

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
