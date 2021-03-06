---
layout: post
title:  "React Hook"
subtitle: "React Hook"
categories: antique
tags: antique
comments: true

---

## React Hook

React와 React Hook의 차이는 기존 React에서는 Class 컴포넌트만

상태와 라이프사이클을 가질 수 있었지만 function 컴포넌트에서도 

상태와 라이프사이클을 가질 수 있게 만든 것이 React Hook이다.

### 상태

```
<!-- 클래스 방식 -->
state = {}

<!-- 리액트 훅 -->
const [ 변수명 , 변수를업데이트할 변수명 ] = useState(초기값)

업데이트변수명( 값설정 )
```

### 생명주기 

기존 리액트의 라이프사이클을 따로 선언했다면 훅에서의 방식은 다르다

```
<!-- 클래스방식 -->
componentDidMount() 컴포넌트가 생성될때
componentDidUpdate() 컴포넌트가 업데이트될때

<!-- 리액트 훅 -->
useEffect(A,B)
```

A에서는 컴포넌트가 생성/업데이트 될때 실행할 코드를 적는곳이고

B에서는 인자를 넣을수 있는데 그 인자가 업데이트 될때만 `useEffect`를 실행하도록 조건을 줄수있다.

또는 처음 딱 한번만(처음 마운트가 되엇을때)와 실행되기를 바란다면 [] 빈배열을 인자로 주면 된다.

### render함수가 실행될때의 차이

#### 클래스방식

클래스 방식에는 처음에 클래스가 생성됫을때 딱 한번만 멤버변수가 생성되고
**Props나 State에 변화가있을때 render만 반복해서 호출이 된다.**

#### 리액트훅

**함수방식에서는 컴포넌트가 업데이트 되면 함수의 코드블럭 전체가 다시 호출이 된다.**

함수안에서 선언된 모든 매개변수들이 다시 생성(호출)된다.

즉 안의 모든 매개변수들이 다시 만들어지고 다시 값을계산하고 .. 의 반복이 일어난다

그럼 안의 `useState(초기값)`이 계속해서 초기화가 되는것 아닌가? 라고 생각할수있다.

useState는 아무리 많이 호출해도 state는 리액트가 따로 저장을해서 동일한값을 메모리에 저장해두어서

상태값의 초기화가 일어나지않는다. `useRef()` 도 똑같이 작동한다.

그래서 callback함수도 똑같이 새로 생성되지않도록 `useCallback()`을 이용할수있다.

이것을 사용하지 않으면 최상위 컴포넌트에서 어떤 업데이트가 일어날때 

자식 컴포넌트에서 memo를 사용하여도 부모 컴포넌트에서 다시 새로운 변수가 생성이되기때문에

리액트는 아 props가 변경됫구나 하고 렌더링을 시켜버린다.
