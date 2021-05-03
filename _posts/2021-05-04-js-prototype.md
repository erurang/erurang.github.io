---
layout: post
title:  "Prototype"
subtitle: "Prototype"
categories: web
tags: javascript
comments: true

---

### 프로토타입은 무엇일까??

자바스크립트는 프로토타입 기반 언어라고 불린다. 밑의 글을 보다보면 Class랑 무슨차인데? 하는 생각이 들것이다.

자바스크립트는 태생부터 클래스의 기능이 없다. (현재로 생기긴했음..) 그래서 클래스를 구현하기 위해서 프로토타입을 이용하여 구현했다.

프로토타입을 언제쓰는가? new 생성자 를 통해서 클래스를 흉내낼수있음!

개발을 하다보면 기존기능을 가져와 확장해야하는 경우가 생긴다. 예를들어 user라는 객체에서 기능을 살짝만 더 추가한 다른 객체를 만들고싶다고 가정해보자.

자바스크립트의 고유기능 프로토타입상속을 이용하면 가능하다.

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

![스크린샷 2021-05-03 오후 11 24 39](https://user-images.githubusercontent.com/56789064/116888705-bd72ef80-ac66-11eb-892d-0fd04d64b044.png)


### 프로토타입 링크와 체인

```
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

