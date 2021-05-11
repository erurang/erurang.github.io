---
layout: post
title:  "TypeScript Generic"
subtitle: "TypeScript Generic"
categories: antique
tags: antique
comments: true

---

Generic이 무엇일까?

타입을 미리지정하지않고 입력값이 들어올때 타입을 정해주는것이 제네릭이다.

가장 이해하기 쉬운 stack을 예로 들어 제네릭을 익혀보자.

```
class Stack {
    arr: any[] = [];

    constructor() {}

    push(item:any):void {
        this.arr.push(item)
    }

    // any
    pop():any {
        return this.arr.pop()
    }

    // number
    pop():number {
        return this.arr.pop()
    }
}

const test = new Stack()

test.push(1)
console.log(test.arr);
test.pop()
console.log(test.arr);
```

첫째로 타입이 any로 지정된 경우에는 나중에 pop을 할때 타입이 정해지지않아서 일일이 타입을 검사해줘야하는 불편함이 생긴다.

두번째로 만약에 pop의 타입이 number일경우를 생각해보자. push는 any타입이라서 어떤 타입이든 넣을수있지만

타입이 number로 지정되었으니 number를 제외한 나머지타입이 pop이될때 typeError가 뜨게 될것이다.

이런경우에 우린 제네릭을 이용할수있다. 위 코드를 아래와같이 변경해보자

```
class Stack<T> {
    arr: T[] = [];

    constructor() {}

    push(item:T):void {
        this.arr.push(item)
    }

    // any
    pop():T {
        return this.arr.pop()
    }

    // number
    pop():T {
        return this.arr.pop()
    }
}

const test = new Stack()

const test2 = new Stack<type>()
```

test 에서는 아무것도 쓰지않으면 데이터가 입력될때 맞춰서 타입이 지정되고

test2에서는 지정된 타입으로 (string number ...) 로만 타입이 지정이된다.


