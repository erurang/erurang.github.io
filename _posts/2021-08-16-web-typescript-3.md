---
layout: post
title:  "Generic 조건걸기"
subtitle: "Generic 조건걸기"
categories: web
tags: typescript
comments: true

---

### Generic constrain

제네릭에 조건을 거는 연습을 해보자. 간단한 예시를 통해 공부해보자!

```
interface Employee {
    pay(): void;
}
```

직원 인터페이스를 하나 만들어주자. 그리고 직원은 풀타임과 파트타임 직원으로 나뉘어져있다. 

직원에게 돈을 지불하는 `pay()` 전역함수도 만들어주자

```
class FullTimeEmployee implements Employee {
    pay() {
      console.log("full time!");
    }
    workFullTime() {}
  }

class PartTimeEmployee implements Employee {
    pay() {
      console.log("part time!");
    }
    workPartTime() {}
}

function pay(employee : Employee) :Employee {
    employee.pay()
    return employee
}
```

풀타임/파트타임 둘다 `pay()`를 구현하고있으므로 인터페이스의 조건에 맞는다. 자 이제 인스턴스를 생성해보자

```
const fullTime = new FullTimeEmployee()
const partTime = new PartTimeEmployee()
```

우리는 이제 풀타임 / 파트타임을 일할수있는 인스턴스를 가졌다. 이 직원들에게 이제 비용을 지불해보자!

```
pay(fullTime)
pay(partTime)
```

우리는 `pay()`함수를 통해 타입이 `Employee`인 인터페이스를 받아 클래스에 구현된 `employee.pay()` 실행이 정상적으로 된것을 볼수있다.

```
full time!
part time!
```

이제 돈을 줬으니 다시 일을 시켜볼까? `fullTime.w...`

<img width="166" alt="스크린샷 2021-08-16 오후 4 29 38" src="https://user-images.githubusercontent.com/56789064/129526990-3fde3ed6-f64f-4c87-b5ab-d25dad270c16.png">

??? 우리가 생성한 `fullTime` 인스턴스의 `workFullTime()` 메소드가 사라진것을 볼수있다.. 이게 무슨일일까?

범인은 `pay()`의 리턴 타입에 있다.

```
function pay(employee : Employee) :Employee 
```

우리는 `employee`의 인자로 `Employee`타입을 받았는데. 이 `Employee`는 인자가 `fullTime`인지 `partTime`인지 신경쓰지않고

단순히 인터페이스에 구현된 `Employee`만 리턴하기때문에 기존 정보를 상실하게 되는것이다.

이 기본정보를 살리기위해서 우리는 받은 인자의 타입을 리턴해줄 필요성이 생겼다. 이게 무엇이였지? 제네릭!

```  
function pay<T>(employee: T): T {
    employee.pay(); // <---- Error!!
    return employee;
  }
```

우리는 제네릭타입을 받아서 제네릭 타입을 리턴하는 제네릭함수를 만들었다.

하지만 `employee.`에서 오류가 뜨는것을 볼수있다. 왜일까? 코딩을 하는 시점에서는 어떤 타입이든 받을수있는

제네릭 `T` 안에 `pay()` 함수가 구현되어있는지 안되어있는지 모르기때문이다. `pay()`가 없는 인자가 들어올수도 있기때문에

타입스크립트는 `pay()`라는 함수를 실행시킬수가 없는것이다. 그럼 제네릭 조건을 아래처럼 수정해주자.

```  
function pay<T extends Employee>(employee: T): T {
    employee.pay();
    return employee;
}
```

제네릭 표현 해석을하면 `<T extends Employee>` `Employee`를 확장한 인자만 가능하다는 조건을 걸어주면 오류가 나타나지않는것을 볼수있다.

오브젝트에선 아래처럼 `<T, K extends keyof T>`로 표현할수있는데, 

`K`는 상속을 받는데 `T`의 `keyof` 키값들로 이루어져 있다 라는 뜻이다. 

`getValue(obj2, "name")`에서 오류가 뜨는 이유는 `obj2`에 `name`이라는 키가 없기떄문이다.

```
  const obj1 = {
    name: "erurang",
    age: 20,
  };

  const obj2 = {
    animal: "dog",
  };

  function getValue<T, K extends keyof T>(obj: T, key: K): T[K] {
    return obj[key];
  }

  console.log(getValue(obj1, "name"));
  console.log(getValue(obj2, "animal"));

  // 오류!
  console.log(getValue(obj2, "name"));
```
