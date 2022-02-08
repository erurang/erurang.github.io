---
layout: post
title:  "Prototype"
subtitle: "Prototype"
categories: web
tags: javascript
comments: true

---

### 프로토타입은 무엇일까??

자바스크립트는 프로토타입 기반 언어라고 불린다. 

공통적으로 사용될만한 것을 상위객체에 선언하여 재활용을 가능하게 만든다. 즉 객체 간 상속관계를 가능하게 해준다.

### 예시 코드

```
const user1 = { name : 'js' }

const user2 = { name : 'nm' }

const user3 = { name : 'yw' }

console.log(user1,user2,user3)

```

![스크린샷 2022-02-08 오후 5 42 35](https://user-images.githubusercontent.com/56789064/152949949-e2e1278f-2834-4502-81ce-36dba817a587.png)

우리가 만든 오브젝트를 보면 `[[Prototype]]` 을 가지고있는걸 볼수있다. 이건 뭘까?

```
console.log(user1.toString())
```

![스크린샷 2022-02-08 오후 5 48 21](https://user-images.githubusercontent.com/56789064/152950907-1fe64301-c8be-42b6-925c-c2e00d76879a.png)

확실히 이상하다. 우리 객체에는 `toString()` 이라는 메소드가 없는데 `[object object]`라는 결과가 출력된걸 알수있다.

분명히 어느곳에 `toString()`이 선언되어있으니 이런 결과가 나오는것이 아닐까? 이것을 우리는 `prototype chaning` 이라고 한다.

## prototype chaning

모든 객체에는 `__proto__` 라고 하는 속성이 있다. 이 속성은 어떤 객체를 가르키고 있는데 바로 모든 객체들의 조상 `Object` 라고 하는 객체를 가르키고있다.

![스크린샷 2022-02-08 오후 5 52 42](https://user-images.githubusercontent.com/56789064/152951643-c57767d0-74f8-4514-b567-d4ebcc06cea4.png)

즉 `toString()` 이라는 메소드는 자바스크립트 최상위 `Object`에 선언되어 있는것이다.

그런데 위에서 우리 `user1 user2 user3` 은 어떻게 `toString()`을 찾아갈까?

자바스크립트 엔진은 현재 선언된 객체에서 `toString()` 메소드가 있는지 찾아보고, 

있다면 메소드를 실행하고 , 없다면 객체가 가지고있는 `__proto__` 가 가르키고 있는 객체에 `toString()`이 있는지 찾아본다. 

있다면 `__proto__` 내부의 `toString()` 메소드를 실행하고 `__proto__`에도 존재하지않으면 `undefined`를 실행하게 된다.

**그렇다면 모든 객체에 있는 proto를 서로 연결시켜준다면 어떻게 될까?**

## 객체를 __proto__로 연결해보자

```
const user1 = { name : 'js' , state : 'hungry'}

const user2 = { name : 'nm' , age : 28 }

const user3 = { name : 'yw' , smart : 'yes' }

user1.__proto__ = user2

console.log(user1.age)
console.log(user1.smart)

user2.__proto__ = user3

console.log(user1.smart)
```
![스크린샷 2022-02-08 오후 6 11 39](https://user-images.githubusercontent.com/56789064/152954817-9af8ebdd-7094-4532-9fe5-29b9858e41d6.png)


위에서 `user1.__proto__` 를 `user2`에 연결한 후에 `user1` 객체 내에는 존재하지않는 속성 `age`를 접근했다.

`user1`속성에는 `age`가 없기때문에 `proto`를 통해 `user2`로 이동하고 `age`를 접근해 `28`이라는 숫자가 출력되는걸 볼수있다.

`user1`의 속성에 `smart`도 없고 `user2`에도 없기때문에 결국엔 `undefined`가 출력되는걸 알수있다.

다시 `proto`를 사용해 `user2 ~3`을 연결해주고 `smart`를 찾으면 프로토타입으로 체이닝된 `user3.smart`를 `user1`이 참조할수있는걸 볼수있다.

## new로 객체를 생성하였을때 protoType

```
function Foo(name) {
  this.name = name
  
  //
  // this.__proto__ = Foo.prototype
  //
}

const f = new Foo('erurang')

console.log(f.name)

Foo.prototype.state = 'hungry'

console.log(f.state)
```
![스크린샷 2022-02-08 오후 6 33 15](https://user-images.githubusercontent.com/56789064/152958745-8ea08719-f713-4edb-b140-ba01105b5bdd.png)

함수도 프로토타입을 가지고있다. `new`를 통해 생성한 객체에 `state`라는 속성을 만들어주었다. 

`new`로 생성하는 순간에 함수내부적으로 `__proto__`는 `함수명.prototype`을 가르키게된다. 생성된 객체에는 `state`라는 속성이 없지만

`f.state` 가 `Foo` 함수 프로토타입에 선언된 `state`속성을 쫒아가서 출력하게 되는것이다.

## 정리

ES6 에서 class를 구현할수있기때문에 proto로 상속관계를 만들어내는 경우는 줄어들었다. 하지만 proto가 어떤식으로 작동하는지는 알아두자.

<!-- ### 프로토타입 링크와 체인
<!-- 

### `__proto__`에 대해 알아보자. 이것은 어디서 나타난 아이일까??

이 proto는 자바스크립트 자체적으로 null이나 다른 객체에대한 참조가 되는데, 다른 객체를 참조하는경우 참조하는 대상을 prototype이라 말한다.

예시와 함께 공부해보자.

```
let animal = {
    eats : true
}

let rabbit = {
    jumps : true
}
```

둘을 선언후에 실행해보면 animal과 rabbit에는 아래와같은 __proto__가 존재한다.

![스크린샷 2021-05-03 오후 11 24 39](https://user-images.githubusercontent.com/56789064/116888705-bd72ef80-ac66-11eb-892d-0fd04d64b044.png) -->

<!-- ```
rabbit.proto = animal
```

이렇게 rabbit의 proto를 animal로 지정해주면 무슨일이 일어날까?

![스크린샷 2021-05-03 오후 11 31 12](https://user-images.githubusercontent.com/56789064/116889553-a84a9080-ac67-11eb-8487-f7a6d5af2948.png)

`__proto__`가 두번 선언된것을 볼수있는데 처음 `__proto__`에서 먼저 선언한 `animal`의 속성값 `eats : true`가 존재한는걸 볼수있다.

우리는 이제 proto의 상속을 통하여 rabbit 오브젝트에서 animal 오브젝트의 eats의 속성을 사용할수있게 되었다.

![스크린샷 2021-05-03 오후 11 33 51](https://user-images.githubusercontent.com/56789064/116889880-07a8a080-ac68-11eb-9971-184600ec3305.png)

즉 자바스크립트의 로직은. rabbit에서 eats의 속성값이 있는지 본후, 존재하지 않는다면 rabbit이 상속받고있는 proto로 넘어가 eats를 실행한다 라고 생각하면 된다.

프로토타입에서 상속받은 속성값을 상속 프로퍼티 라고 표현한다. 이렇게 프로토는 여러개를 이어서 연결되어있는 형태를 프로토타입 체인 이라고 표현한다.

체인이 이루어질때의 주의사항 2가지다. 한 객체는 오직 하나만 상속을 받을수있다. 이렇게 여려번 체인이 걸쳐져있을때 `__proto__.__proto__...`로 사용이 가능하긴하다.

꼭 `__proto__.__proto__` 이렇게 접근하지않아도 위에서 말했듯 자바스크립트는 프로퍼티가 없으면 자동으로 프로토로 들어가서 찾아보기때문에 `longEar.walk()` 를 사용해도 실행되는걸 알수있다 (실행해보시길)

![스크린샷 2021-05-03 오후 11 51 24](https://user-images.githubusercontent.com/56789064/116892148-7ab31680-ac6a-11eb-9d8e-40d56411dd04.png)


### this가 존재하는 경우

```
let hamster = {
  stomach: [],

  eat(food) {
    this.stomach.push(food);
  }
};

let speedy = {
  __proto__: hamster,
  stomach: []
};

// speedy는 음식을 발견합니다.
speedy.eat("apple");
alert( speedy.stomach ); // apple

```

과연 여기서 this는 어떻게 처리가될까? this는 똑같이 현재 존재하는 함수를 가르킨다.

speedy의 eat는 hamster의 eat 프로퍼티로 넘어가 this.stomach이기 때문에 hamster의 stomach가 아닌 함수를 처음 호출한 speedy의 stomach에 push가 되는것을 볼수있다.

### 프로토타입을 꼭 proto를 사용해서 지정해야할까? 객체복사방법!

후에 자바스크립트언어가 발전하면서 메소드도 생겨나기 시작했다.

```
Object.create(proto, [descriptors]) – [[Prototype]]이 proto를 참조하는 빈 객체를 만듭니다. 이때 프로퍼티 설명자를 추가로 넘길 수 있습니다.

Object.getPrototypeOf(obj) – obj의 [[Prototype]]을 반환합니다.

Object.setPrototypeOf(obj, proto) – obj의 [[Prototype]]이 proto가 되도록 설정합니다
```

사용 예시는 다음과 같다

```
let animal = {
  eats: true
};

// 프로토타입이 animal인 새로운 빈 객체를 생성합니다.
let rabbit = Object.create(animal);

// 설명자도 함께 생성한 예
let rabbit = Object.create(animal, {
  jumps: {
    value: true
  }
});


alert(rabbit.eats); // true
alert(Object.getPrototypeOf(rabbit) === animal); // true
Object.setPrototypeOf(rabbit, {}); // rabbit의 프로토타입을 {}으로 바꿉니다.

```

![스크린샷 2021-05-04 오전 1 01 58](https://user-images.githubusercontent.com/56789064/116900975-55c3a100-ac74-11eb-9cbb-18451178890d.png)
 -->
