---
layout: post
title:  "Generic 타입"
subtitle: "Generic 타입"
categories: web
tags: typescript
comments: true

---

## Generic은 왜 생겨났을까?

타입스크립트는 모든 타입을 지정해주는 정적인 언어였다. 기본적인 타입지정으로는 부족했던것일까?

제네릭은 무엇이고 제네릭은 왜 생겨난것일까? 아래 예제를 통해서 한번 이해해보자.

### 인자를 받아서 숫자인지 문자인지 판단하는 함수를 만든다고 해보자.

```
function checkNotNullNumber(arg: number | null): number {
    if (arg == null) throw new Error("not valid number");
    return arg;
}

function checkNotNullString(arg: string | null): string {
    if (arg == null) throw new Error("not valid number");
    return arg;
}
```

왜 저렇게밖에 만들수없을까? 우리는 변수에 숫자가올지 문자가올지만 모르기떄문에 인자를 달리받는 함수 2개를 만들었다.

```
function checkNotNull(arg: number | string | null): number {
    if (arg == null) throw new Error("not valid number");
    return arg;
}
```

하지만 위의 두 함수를 합쳐서 타입에 `| string` 을 추가하여 만들어도 된다. 음.. 근데 만약에말이야..

### 만약 인자로 `bool` 타입도 받을수있도록 해야한다면? 

```
function checkNotNull(arg: number | string | boolean | null): number {
    if (arg == null) throw new Error("not valid number");
    return arg;
}
```

기존함수의 매개변수에 `|boolean`을 타입을 추가하여 사용 가능하게 만들었다. 자 여기서 문제가발생한다.

매번 타입의 변화가 생길때마다 매개변수의 타입에 위의 경우들처럼 하나하나 추가해줘야할까?


```
function checkNotNull(arg: any | null): any {
    if (arg == null) throw new Error("not valid number");
    return arg;
}

const res3 = checkNotNull(1);
```

그냥 변수의 타입을 `any`로 받아버리면 되지않을까? `any`를 쓰는것부터 타입스크립트의 사용 의의가 사라진다. 

위와같은 문제점들을 해결해주는것이 **Generic**이 만들어진 이유이다.

## 제네릭 사용해보기

제네릭은 `<GENERIC>`형식으로 사용된다고 선언한다. `<>`안에는 변수명이 들어가는데 어떤 변수명이든 상관없다. 통상적으로 `<T> <S> <G> ...`가 사용된다.

위 예제의 함수를 제네릭을 사용해서 고쳐본다면 아래처럼 고칠수있다.

```
function checkNotNullGeneric<GENERIC>(arg: GENERIC): GENERIC {
    if (arg == null) throw new Error("not valid number");
    return arg;
}

console.log(checkNotNullGeneric(123))
console.log(checkNotNullGeneric(true))
console.log(checkNotNullGeneric('generic~'))
console.log(checkNotNullGeneric(null))
```

위의 함수는 이렇게 읽을수있다. `checkNotnullGeneric` 함수는 `Generic` 함수이고, arg는 `Generic`타입이며 리턴되는 값은 `Generic` 타입이다.

즉 제네릭은 우리가 인자로 넘겨주는 변수의 타입이 제네릭의 타입이 된다.

`123` 숫자를 인자로 주었기 때문에 함수의 제네릭은 `number`타입이 된다. `true`을 인자로 주었기 때문에 함수의 제네릭은 `true`타입이 된다. 

`generic` 문자열을 인자로 주었기 때문에 함수의 제네릭은 `string`타입이 된다. `null`을 주었을때 `Error`가 리턴된다. 

제네릭을 통해서 변수의 타입을 매번 지정해줄 필요가 없어진다는것 이것이 제네릭을 사용하는 이유이다.