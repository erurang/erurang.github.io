---
layout: post
title:  "React Component Render 순서를 보자"
subtitle: "React Component Render 순서를 보자"
categories: web
tags: react
comments: true

---

## React Component Render 순서를 보자

이 글은 [컴포넌트선언방식](https://erurang.github.io/web/2021/01/03/web-react-component/) 글의 Purecomponent와 memo에 관한 부연 설명이다.

현재 컴포넌트 순서는 다음과 같다.

```
index
    App
        nav
        tasks
            input
            task             
```

tasks에는 state에 기본적으로 3가지 상태 A,B,C 가 선언되있다고 하자.

각 task에선 tasks에서 선언된 상태들을 컴포넌트로 보여준다.

렌더링 되는 순서를 보자.

<img width="141" alt="스크린샷 2021-01-07 오후 3 54 02" src="https://user-images.githubusercontent.com/56789064/103861312-91a08c80-5100-11eb-8aec-a3b4d890bac3.png">

여기서 task에 있는 onclick을 실행해보면 어떻게될까?

<img width="137" alt="스크린샷 2021-01-07 오후 3 55 37" src="https://user-images.githubusercontent.com/56789064/103861459-cad8fc80-5100-11eb-886b-e421edb4c681.png">

모든 컴포넌트를 다시 렌더하는걸 볼수있다.

input은 리액트에서 컴포넌트 상태변경이 일어나 

다른 모든 컴포넌트가 다시 렌더될때 굳이 렌더가 다시 될 필요가 없다.

그래서 이럴경우 우리는 class선언방식일때는 Purecomponent를 function 선언방식일떈 memo를 사용한다.

<img width="132" alt="스크린샷 2021-01-07 오후 4 00 24" src="https://user-images.githubusercontent.com/56789064/103861915-771ae300-5101-11eb-9e2e-5e80cf36a7e9.png">

전의 사진에서는 task의 상태변화가 일어나 다시 렌더가 될떄 input까지 렌더가 되었으나

purecomponent를 사용하여 이 컴포넌트의 변경이 없을땐 렌더를 하지않게 설정하였으므로

input이 콘솔에 뜨지 않는걸 볼수있다.

꼭 리액트가 전체적으로 렌더할때 렌더가 될 필요가 없다면 memo와 purecomponent를 이용하여 성능을 높일수있다.

그렇지만 두 기능은 렌더가 되는것을 아주 얕게 비교하는데 얕게 비교한다는것은

a -> b a가 b의 ref(주소)를 가지고있을때 b안의 값이 변화가 일어나도 ref의 주소가 똑같다면

같다고 생각하고 리액트가 render를 하지 않는다. 이 점을 주의해서 두 기능을 사용해야한다.