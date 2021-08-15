---
layout: post
title:  "Generic 타입"
subtitle: "Generic 타입"
categories: web
tags: typescript
comments: true

---

### Generic이 무엇일까?

타입스크립트는 매번 타입을 지정해주는 정적인 언어였다. 앞서 배운 기본타입들로는 부족했던것일까?

제네릭은 무엇이고 제네릭은 왜 생겨난것일까? 아래 예제를 통해서 한번 이해해보자.

인자를 받아서 숫자인지 문자인지 판단하는 함수를 만든다고 해보자.

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

왜 저렇게밖에 만들수없을까? 우리는 변수에 숫자가올지 문자가올지만 모르기떄문에, 위처럼 하나하나 작성해주었다.

만약 인자로 `bool` 타입을 받는다면 또 `checkNotNullBool`이라는 임의의 함수를 하나 더 생성해야할것이다.

그럼 저렇게 만들바에.. 아예 타입을 `any`로 받아버리면 되지않을까?

```
function checkNotNull(arg: any | null): any {
    if (arg == null) throw new Error("not valid number");
    return arg;
}

const res3 = checkNotNull(1);
```

음 이제 `arg`는 어떤 타입의 인자든 다 받아올수있는 함수가 되었다!.. 그런데 문제가있다.

`res3`은 무슨타입일까? 당연히 `any`타입이 된다. 타입 정보를 상실하여 우리가 타입스크립트를 쓰는 이유가 사라지게 되었다..

이런걸 해결하기위해 `Generic` 문법이 생기게 되었다. 

```
function checkNotNullGeneric<GENERIC>(arg: GENERIC | null): GENERIC {
    if (arg == null) throw new Error("not valid number");
    return arg;
}

const res4 = checkNotNullGeneric(123);
const res5 = checkNotNullGeneric(true);
const res6 = checkNotNullGeneric('generic~');
```

`<GENERIC>`형식으로 사용된다고 선언한다. `<>`안의 변수명은 어떤 변수명이든 상관없다.

위의 함수는 이렇게 읽을수있다. `checkNotnullGeneric` 함수는 `Generic` 함수이고, arg는 `Generic`타입이나 `null` 일것이고 리턴되는 값은 `Generic` 타입이다. 

`res4`에서는 우리가 `123`을 인자로 주었기 때문에 함수의 제네릭은 `number`타입이 된다. 그래서 `res4`의 타입은 `number`다.

`res5`에서는 우리가 `true`을 인자로 주었기 때문에 함수의 제네릭은 `true`타입이 된다. 그래서 `res4`의 타입은 `true`다.

`res6`에서는 우리가 `generic`이라는 문자열을 인자로 주었기 때문에 함수의 제네릭은 `string`타입이 된다. 그래서 `res4`의 타입은 `string` 이 된다.

제네릭을 통해 우리는 어떤 인자가 들어오던 인자와 같은 타입을 리턴해줄수 있게 되었다.

함수에서만 제네릭이 가능할까? 놉. 인터페이스와 클래스에서도 제네릭을 사용할수있다.

```
interface Either<L, R> {
    left: () => L;
    right: () => R;
}

class SimpleEither<L, R> implements Either<L, R> {
    constructor(private leftV: L, private rightV: R) {}

    left(): L {
        return this.leftV;
    }

    right(): R {
        return this.rightV;
    }
}

const either1 = new SimpleEither(4, 5);
const either2 = new SimpleEither("zz", {});
const either3 = new SimpleEither(true, []);

console.log(either1, either2, either3);
```