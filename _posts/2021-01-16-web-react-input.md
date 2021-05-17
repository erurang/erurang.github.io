---
layout: post
title:  "React Input"
subtitle: "React Input"
categories: antique
tags: antique
comments: true

---

## Input 상태 관리하기

리액트에서 인풋 상태를 관리하는 방법은 총 2가지가 있다.

### 첫째로 onChange 를 이용한다.

<img width="456" alt="스크린샷 2021-01-16 오후 8 09 59" src="https://user-images.githubusercontent.com/56789064/104810290-d4681000-5836-11eb-8137-801a64ccab1c.png">

input값이 변할때마다 onChange(변함을감지)가 실행되어서 상태를 매번 업데이트 해주는방법이 있다.

만약에 여러개의 input의 상태를 한번에 관리하고싶다면?

input에 name속성을 적어준다.

<img width="376" alt="스크린샷 2021-01-16 오후 9 00 21" src="https://user-images.githubusercontent.com/56789064/104811307-daadba80-583d-11eb-8c18-51d3da80909b.png">

console.log(e) 를 찍어보면 각 event.target의 name과 value를 확인해볼수있다.

<img width="163" alt="스크린샷 2021-01-16 오후 8 59 53" src="https://user-images.githubusercontent.com/56789064/104811297-c9fd4480-583d-11eb-9ec1-1388fe612509.png">

이제 이 input의 상태관리는 어떻게 하나?

<img width="320" alt="스크린샷 2021-01-16 오후 9 01 16" src="https://user-images.githubusercontent.com/56789064/104811332-fca73d00-583d-11eb-9f2a-659dffdcc0a0.png">

useState({})를 객체로 두어서 각 name과 nickname을 할당한다.

input의 value를 useState의 기본값으로 두고

onChange에서 업데이트를 상태를 업데이트를 할때

`...inputs`를 쓰는것을 볼수있다. 이것은 왜 쓰냐하면 기존의 상태값을 주소까지 그대로 복사해온다.

그런후에 `[name]:value` 를 사용해서 `[]`로 감싸면 받아온 name에 따라 다르게 처리된다.

위의 `...` 문법을 스프레드 라고하는데 이것은 불변성에 매우 중요한 역할을 한다.

리액트에서 기존 상태에 Push를 하면 리액트는 상태가 변화된줄 모르고 업데이트를 하지않는다.

왜냐면 react는 얕은 복사를 이용하여 렌더링 해야할지 말아야할지를 빠르게 파악하기 때문이다.

그래서 ...를 사용해 기본배열을 업데이트하고 새로운값을 추가해야지 리액트는 값이 달라졌다는걸 인식하고 컴포넌트 변화를 한다.

### 두번째로 useRef 를 이용한다.

<img width="295" alt="스크린샷 2021-01-16 오후 10 31 51" src="https://user-images.githubusercontent.com/56789064/104813140-a55b9980-584a-11eb-8ffe-dc2b493ca419.png">

input요소에 ref속성으로 useRef()로 만든 객체를 ref에 넣어준다.

