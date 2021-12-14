---
layout: post
title:  "자바스크립트 Context this bind"
subtitle: "자바스크립트 Context this bind"
categories: web
tags: javascript
comments: true

---

## Javascript의 Context

자바스크립트에는 Global Context가 존재한다. 이 GC는 처음 자바스크립트 엔진이 실행될때 처음 스택에 쌓여서 실행되고

함수실행마다 스택에 차례대로쌓여 처리한다.

## Context가 어떤것인지 알기위해 바로 실습을 통해 알아보자.

```
{
  const person = {
    name: "js",
    age: 20,

    getAgeArrow: () => this.age,

    getAgeFunction() {
      return this.age;
    },
  };
}
```

person이라는 객체를 만들어서 name과 age의 변수를 만들었다.

그리고 function/arrow function 방식으로 person 객체안에 만들어진 변수 age에 접근하여 값을 돌려받는 코드를 작성하였다.

```
console.log(person.age); // 20
console.log(person.getAgeArrow()); // undefined
console.log(person.getAgeFunction()); // 20
```

화살표 함수에서는 현재 겍체안의 age에 접근이 불가능한것을 확인할수있다. 그리고 또 한가지 방법을 더 실습해보자.

```
const newPerson = person.getAgeFunction;
console.log(newPerson()) // undefined
```

newPerson이라는 변수안에 person.getAgeFunction 함수를 참조하도록 하였다.

그 후에 newPerson() 을 호출했더니 어? undefined가 출력되는걸 알수있다. 왜 값을 얻어오지 못하는것일까?

## Context의 2가지 방식

execution context 실행컨텍스트(기본)
- 어떤 객체의 메소드에 접근 후 실행한다.

위의 예시를 통해 정리해보면 

1. person 객체 안에 있는 getAgeFunction() 메소드에 접근한다.
2. getAgeFunction() 함수안에서 쓰인 this는 호출된(작동된) 시점의 소유자의 age 변수를 찾는다

자바스크립트는 실행컨텍스트 메커니즘을 가지고있기 때문에 실행하는 순간 this가 결정되게 된다.

lexical context 