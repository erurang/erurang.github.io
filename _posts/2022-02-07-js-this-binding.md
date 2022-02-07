---
layout: post
title:  "자바스크립트 함수호출 call apply + this binding + arrow function"
subtitle: "자바스크립트 함수호출 call apply + this binding + arrow function"
categories: web
tags: javascript
comments: true

---

## this binding ??

이전 자바스크립트 context 글에서 자바스크립트가 this binding을 어떻게 처리하는지에 대해 간단히 적어두었다.

이 글에서는 this binding을 하는 여러가지 방법을 다룬다.

자바스크립트는 interpreter가 코드를 라인단위로 읽고 해석후에 실행시킨다.

## default this

<img width="653" alt="스크린샷 2022-02-07 오후 8 18 42" src="https://user-images.githubusercontent.com/56789064/152778461-2573de3c-e0d4-4e0a-9d60-301a6dd76049.png">

기본적으로 this는 전역 객체(window)를 가르킨다. 

## 함수내부의 this

<img width="242" alt="스크린샷 2022-02-07 오후 8 19 52" src="https://user-images.githubusercontent.com/56789064/152778655-94023db8-46fb-4cff-9a23-ff3c5920c7f5.png">

일반 함수를 실행하면 this는 window를 가르키는걸 볼수있다. 그리고 `(this === window) => true`를 통해 this는 window와 같다는것도 알수있다. 

## 객체의 메소드 this

![스크린샷 2022-02-07 오후 3 59 15](https://user-images.githubusercontent.com/56789064/152739886-129eb14e-950c-4b03-8e3e-1b37bab0dc82.png)

객체 내부의 메소드로 호출한 this는 그 객체 자신을 가르키게 된다.

하지만 일반적인 함수호출 형태로 호출한 this는 전역객체를 가르키는걸 볼수있다.

## new

<img width="342" alt="스크린샷 2022-02-07 오후 8 44 01" src="https://user-images.githubusercontent.com/56789064/152782089-0ee6f348-5d69-461d-aa57-01675f28c62f.png">

기존의 함수호출 방식에서의 this.name은 전역인 window를 가르키기때문에 `const name ='zzz'` 가 this.name으로 되어 `zzz is hungry`가 출력되는걸 볼수있다.

하지만 new로 선언할경우 this는 전역객체가 아닌 생성된 객체 자체를 가르키게 되어 this.name은 존재하지않음으로 undefined가 출력되는걸 볼수있다.

## call & apply 메소드와 this binding

함수호출 방식에는 3가지가 존재한다. 똑같이 함수를 호출한다는건 같다. 하지만 함수명뒤에 call과 apply는 첫번째 인자로 context를 받는다.

```
function sum(a, b, ...args) {
  console.log(...args); 
}

// 일반호출
sum(10,20,30,40)

sum.call(null,10,20,30,40)

sum.apply(null,[10,20,30,40])
```

![스크린샷 2022-02-07 오후 8 53 51](https://user-images.githubusercontent.com/56789064/152783429-7bf1857c-ae01-4531-9671-bb09bacee0bf.png)

call과 apply의 차이점은 call은 인자를 직접 주느냐 인자를 직접주지 않느냐 의 차이를 가진다.

call의 경우 인자를 변수에담아 줄 경우 작동하지않는것을 볼수있고 apply의 경우 인자를 변수로주어도 처리하는것을 볼수있다.

### context 인자로 주어서 사용해보기

<img width="249" alt="스크린샷 2022-02-07 오후 9 09 54" src="https://user-images.githubusercontent.com/56789064/152785530-e80e183b-b547-4215-a9a5-ee40c6902877.png">

context를 인자로 주지않은 경우 this.num은 전역값인 `1234` 를 출력하는걸 볼수있다.

apply나 call로 context를 numObject로 지정후에 호출하면 우리가 지정한 오브젝트의 num값을 this.num으로 사용하는것을 알수있다.

## arrow function에서의 this binding

ES6 문법의 화살표함수의 특징은 this가 binding하는 대상이 달라지는 것이다.

<img width="342" alt="스크린샷 2022-02-07 오후 9 44 53" src="https://user-images.githubusercontent.com/56789064/152790503-c0a4d3d4-291d-4433-afe4-06e4d2f94185.png">

위 코드는 살펴보자. 객체의 내부 print() 메소드를 통해 this.text가 출력되는걸 볼수있다.

print()함수안에서 forEach에서는 this.text가 다시 전역객체를 binding하고 있기 때문에 undefiend가 나타나는것을 볼수있다.

<img width="371" alt="스크린샷 2022-02-07 오후 9 47 43" src="https://user-images.githubusercontent.com/56789064/152790914-9717087b-2bdd-4226-9cc1-0729de6a5806.png">

하지만 arrow function의 경우엔 this는 전역객체가 아닌 부모객체인 obj를 가르키게되어 this.text가 제대로 출력되는것을 볼수있다.

이것을 lexical context(어휘맥락) 라고도 하는데 어휘적으로 읽었을때 이 함수는 이 this가 연결되어있다는것을 알리는 방법론이다.

즉 arrow function은 생성되는 그 즉시 this가 부모객체로 고정이 되는 차별점이 있다. 하지만 주의할점이 있는데.

<img width="300" alt="스크린샷 2022-02-07 오후 9 54 21" src="https://user-images.githubusercontent.com/56789064/152791889-9e78420a-0e5f-43e1-a1b0-1e1ea61e4914.png">
<img width="413" alt="스크린샷 2022-02-07 오후 9 55 18" src="https://user-images.githubusercontent.com/56789064/152792025-3d2b8f2b-3e6f-43f8-8270-389c3a0f2361.png">

객체의 메소드를 arrow function으로 선언할 경우에는 this가 부모객체를 가르키는것이 아닌, 전역객체를 가르킨다는것을 알아두자.

<img width="390" alt="스크린샷 2022-02-07 오후 9 57 20" src="https://user-images.githubusercontent.com/56789064/152792333-c28437ac-43cc-49bc-b15a-5b5f415905f6.png">

그리고 화살표 함수는 prototype을 가지고있지 않다. 그래서 new로 생성을 할수없다는것도 알아두자.
