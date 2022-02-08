---
layout: post
title:  "자바스크립트 Context와 Closure에 대해서"
subtitle: "자바스크립트 Context와 Closure에 대해서"
categories: web
tags: javascript
comments: true

---

# Execution Context??

클로저를 이해하기위해선 자바스크립트의 `Context`에 대해 이해해야한다.

간단히 말해서 실행컨텍스트는 자바스크립트 코드가 실행되기위해 필요한 환경을 정의한것이다.

실행 컨텍스트는 크게 3가지 `(LexicalEnvironment, VariableEnvironment, ThisBindig)`의 컴포넌트가 담기게 된다.

# 실행컨텍스트의 타입

### Global Execution Context

기본 실행 컨텍스트로, 프로그램 당 실행컨텍스트가 하나만 있을수 있다.

코드가 실행되기 위해 적재되면 자바스크립트 엔진이 `Global Execution Context`를 만든다.

`global object` 와 `this` 객체를 생성하는 과정을 포함하고 이때 `this`는 `global object`인 `window(browser)`또는 `global(node.js)`를 참조하게 된다.

### Functional Execution Context

함수가 호출(invoked)될 때 마다 생성되고 새로운 함수가 생성될때마다 새로운 `execution context`가 만들어진다.

각 함수는 고유의 `Execution Context`를 가지는데 이 실행 컨텍스트는 함수가 실행되거나 불릴때 만들어진다.

새로운 실행 컨텍스트가 만들어질때마다 여러 단계의 스텝을 거치게 된다.

# 실행 컨텍스트 실행 과정

`Execution stack`은 다른 프로그래밍 언어에서 `Calling Stack`이라고 알려져있다. 

이 `Execution stack`은 코드 실행 중에 생성된 모든 실행 컨텍스트를 저장하는 데 사용된다.

자바스크립트 엔진이 처음 우리의 스크립트 코드를 마주하면 자바스크립트엔진이 `global execution context`를 만들고 현재의 `execution stack`에 `push`한다.

함수가 실행 될때마다 자바스크립트 엔진은 새로운 `function execution context`를 만들고 `execution stack`의 최상단에 `push`한다.

자바스크립트 엔진은 `execution stack`의 최상단에있는 `execution context`를 실행한다. 

함수의 실행이 완료되면 `execution stack`에서 `pop`을 하고 자바스크립트 엔진은 다시 `execution stack`의 최상단의 `execution stack`을 실행한다.

### 예시를 통해 익혀보자

```
var a = 'hello world!'

function first() {
  console.log('first function')
  second();
  console.log('again first function')
}

function second() {
  console.log('second function')
}

first()
console.log('global execution context')
```

<img width="294" alt="스크린샷 2022-01-31 오후 12 32 13" src="https://user-images.githubusercontent.com/56789064/151735224-fb401472-8c03-4de1-90d5-95d0c4755cb4.png">

<img width="720" alt="스크린샷 2022-01-31 오후 12 32 59" src="https://user-images.githubusercontent.com/56789064/151735278-1f12a035-4d87-4d7a-9cff-4cfc44581b29.png">


위 코드가 브라우저에서 실행되면 자바스크립트 엔진은 `global execution context`를 **만들고** `execution stack`에 `push`한다.

`first()` 함수를 만나면 자바스크립트 엔진은 새로운 `execution context`를 **만들고** `execution stack`에 `push`한다

`first()` 함수 안에서 불린 `second()` 함수를 만나면 자바스크립트 엔진은 새로운 `execution context`를 **만들고** `execution stack`에 `push`한다.

`second()` 함수가 완료된 `second() execution context`는 `execution stack`에서 pop되어 제거되고

제어권은 다시 `first() execution context`로 넘어가게 된다.

`first()` 함수도 완료되면 `first() execution context`는 `execution stack`에서 제거되고 제어권은 `global execution context`로 넘어가게 된다.

모든 코드가 실행되고 나면 자바스크립트 엔진은 `global execution context`를 `execution stack`에서 지우고 스택이 완전히 비워진다.

# 어떻게 실행 컨텍스트가 만들어질까?

지금까지 우리는 자바스크립트 엔진이 `execution stack`과 `execution context`를 어떻게 실행하고 관리하는지 알아보았다.

이번엔 자바스크립트 엔진이 `execution context`를 어떻게 만드는가에 대해 이해해보자.

`execution context`는 크게 3가지 로 구성되어있다.
```
// Execution context in ES5ExecutionContext = {
  ThisBinding: <this value>,
  VariableEnvironment: { ... },
  LexicalEnvironment: { ... }
}
```


### 1) 생성단계 2) 실행단계 2개의 페이즈(단계) 로 나뉜다.

#### 1) 생성단계 (Creation Phase)

`execution context`는 생성단계 동안 만들어진다. 생성되는 동안 3가지 단계가 발생한다.

생성 단계에서 **`execution context`를 만들기만 하고 실행은 하지 않는다.!! 실행은 실행단계에서!**

1. `VariableEnvironment`가 만들어져서 `변수,arguments,함수 선언`을 저장한다. `var`의 경우에만 `undefined` 상태로 저장된다.
2. `LexicalEnviornment`가 생성된다. 생성단계때 만들어진 `LexicalEnvironment`는 **값이 할당이 안되어있다**. 즉 `variableEnvironment`의 사본이다
3. `this`의 값이 결정된다.


### Lexical Environment

### 1) 환경기록(environment record) 2) 외부 환경 참조(lexical environment reference) 3) this binding

### 환경기록 (environment Record)

```
var name = 'erurang'
var phone = '010-1234-5678'

function foo() {
  return {name,phone}
}

================================ global execution context (Phase : creation)

window : global object
this : window
name : undefined
phone : undefined
foo : fn()
```

**선언된 변수와 선언된 함수의 정의만** `VariableEnvironment` 컴포넌트 내에 저장한다. **값을 할당을 하지 않는다!!!!!!!!!**

이 과정에서 **변수와 함수의 영역이 메모리에 할당**되게 된다. `var`로 선언된 변수의 경우 `undefined`로 값이 할당된다. 

이들은 `Environment Record` 내의 `Declarative Environment Record` 라고 불리는 곳에 저장된다.

**함수의 경우 그 선언부가 메모리에 적재된다.** 메모리에 적재되고 난 후엔 호출이 가능해진다. 이것을 우린 `hoisting`이라고 한다.

### 2) 실행단계 (Execution Phase)

1. **값이 할당된다!!!**

함수가 호출 될 때 생성된다. 생성될때 `arguments`를 추가로 저장한다. 매개변수로 넘어온 객체의 인덱스,값,매개변수 개수를 포함한다.

```
var a = 10;

function print() {
  console.log(a);
}

print()
================================ global execution context (phase : Creation)

window : global object
this : window
a : 10
print : fn()

================================ function execution context (phase : Creation)
arguments: {length : 0}
this : window
```

`print()` 함수의 `execution context`에는 변수 `a`가 존재하지 않는다.` console.log(a)`가 실행될때 **현재 컨텍스트 외부의 컨텍스트를 참조**하게 된다. 

현재 **컨텍스트에 없는 변수나 함수를 찾기위해서 그 상위 컨텍스트를 참조하는 과정을 `Scope chain(스코프체인)`** 이라고 한다.

`Scope`는 현재의 `Execution Context`를 뜻한다. 이것을 이용해 `Closure`에 대한 설명이 가능해진다.

### this binding (실행 컨텍스트의 this를 결정하는 ThisBinding)

`function execution context` 에는 **함수가 호출된 경우에 따라 다르게 바인딩** 된다.

##### 함수가 객체 내 에서 호출된 경우. 즉 객체 레퍼런스를 통해 호출된 경우에 `this`는 호출된 오브젝트를 가르키게 된다.

##### 일반적인 함수내부에서 `this`를 호출하면 `this`는 `전역객체(window)`를 가르킨다.

```
const bindingTest = {
  name : "erurang",
  state : "hungry",
  print() {
    console.log(`${this.name} is ${this.state}`)
  }
}

bindingTest.print()

const bindingTest2 = bindingTest.print

bindingTest2()
```

![스크린샷 2022-02-07 오후 3 59 15](https://user-images.githubusercontent.com/56789064/152739886-129eb14e-950c-4b03-8e3e-1b37bab0dc82.png)

위의 코드를 보면 `this`가 어디서 어떻게 `this`를 포함하는 코드를 호출하느냐가 중요한것을 알수있다.

첫번째 `bindingTest.print()` 처럼 **객체의 메소드 방식으로 호출**하면 `this`는 **호출한 객체를 가르키게 되어** `name`과 `state`를 문제없이 출력하는데

두번째 `bindingTest2()`는 일반적인 함수호출 형태로 호출하여 `this`는 전역객체인 `window`를 가르키게되고 `window`엔 `name`과 `state`가 없기때문에 `undfined가` 출력되게 되는걸 볼수있다.

즉 `this`는 **누가(어디서) 호출하냐에 따라**서 **변경**된다는것을 볼수있다.

`this binding`을 이용하는 방법에 대해서는 [이글](https://erurang.github.io/web/2022/02/07/js-this-binding/)에서 더 다루도록 한다.


## Closure

`Schope chain`에 대해 설명했을때 이를 이용하면 `closure`를 이해할수 있다고 했다.

자바스크립트 함수는 함수를 리턴하면 `closure scope`를 생성한다. 즉 함수와 함수가 선언된 `execution context`의 조합이다.

예시로 익혀보자. 아래처럼 `num`을 `+1`씩 해주는 함수가 있다고 하자. 그런데 사이에서 `num`의 값을 임의로 바꿔버린다면?

```
let num = 1;

function increment() {
  return num++
}

console.log(increment()) // 1
console.log(increment()) // 2
console.log(increment()) // 3

num = 200;

console.log(increment()) // 200
```

우리는 `num`이 바뀌는것을 `Increment()` 함수에서 막을수가 없다. 왜냐하면 함수내에서 변수를 가지고있는것도 아니고, `num`은 `global`로 어디서든 접근 가능하기 때문이다.

즉. 우리는 바깥에서 접근할수 없는 방법이 필요하다. 그럼 선언부를 함수내로 옮겨볼까?

```
function increment() {
  let num = 1;
  return num++
}

console.log(increment()) // 1
console.log(increment()) // 1
console.log(increment()) // 1
console.log(increment()) // 1
```

흠.. 이러면 함수를 호출할때마다 1이라는 값이 고정되어버려서 의미가 없어졌다. 

위에서 나는 **자바스크립트 함수는 함수를 리턴하면 closure scope를 생성한다.** 라고 적어두었다. 아래의 예시를 따라해보자.

```
function increment() {
  let num = 1;
  return function () {
      return num++
  }
}

const inc = increment() 

console.log(inc()) // 1
console.log(inc()) // 2
console.log(inc()) // 3
console.log(inc()) // 4
```

`increment()`는 함수를 리턴하기때문에 함수를 실행후에 `inc`라는 변수에 함수를 할당해 주었다. 그 다음 `inc()`를 실행해보면 `1 2 3 4` 가 출력된다.

`inc`라는 변수에 할당된 `increment()`에서 리턴된 함수를 실행하면 `increment()`내의 `let num =1` 선언부는 접근할수가 없게된다.

그런데 어떻게 `inc()` 함수를 실행했을때 이미 접근할수 없게된 `increment()` 로컬변수 영역의 `num`변수에 접근하여서 숫자를 리턴하는것일까? 

<img width="588" alt="스크린샷 2022-02-07 오후 4 25 20" src="https://user-images.githubusercontent.com/56789064/152743260-6dfdee66-8dc4-45f7-bc51-e9a65d21e257.png">

함수 안의 함수가 바깥 함수에 있는 변수에 접근을 하게되면 이 접근한 변수를 `closure`라는 공간에 저장해둔다.

`increment()`를 실행한 순간 `num`에는 접근할수가 없지만. `inc()`로 함수내의 함수를 실행하면 위 그림처럼 `closure`라는 공간이 생기는 것을 볼수있다.

당연히 `return`한 `inc()` 함수내에 `local 변수`가 없기때문에 `local`은 `Undefeind`가 된다.

즉. `increment()` 함수가 실행되면 `num`의 지역 변수는 사라지지만 `closure`라는 공간에서 저장된 값을 활용하게 되는것이다. 

### 클로저를 왜 사용할까..

함수내에서 값을 호출하는데 **접근할수 없는 영역에** 값을 보호하면서 값을 계속 사용할수있다는 장점이 생긴다.

클로저를 접근할수있는건 클로저공간을 사용하는 함수뿐이다.