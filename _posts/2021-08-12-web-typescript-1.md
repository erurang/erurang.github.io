---
layout: post
title:  "Typescript란?"
subtitle: "Typescript란?"
categories: web
tags: typescript
comments: true

---

### javascript는 편의성을 너무 봐주는 언어다!

```
a = [1,2,3,4]
b = true

a + b = ???
```

위와같은 코드가 있다고 하자. 배열에 bool형을 더한다?? 이게 무슨 말이지..? 연산 자체가 말이 안된다는걸 알수있다.

그런데 자바스크립트는 위코드의 결과를 이렇게 돌려준다

```
'1,2,3,4false'
```

...? 배열을 없애고 string으로 변환시키고 bool형도 string으로 변환시킨 값을 보여준다.

```
1 + true = 2
```

1 + true의 경우도 true를 1로 변환시켜 2를 보여준다.

이렇듯 자바스크립트는 개발자의 편의성(?)을 너무 높여주어 예상치못한 오류가 일어날수가 있다.

또 다른 예로 함수의 인자를 생각해보자

```
function plus(a,b) {
    return a+b
}

plus(2,2) // 4
plus('a','b') // ab
plus('zzz') // zzzundefined
```

우리 (개발자 입장에서)는 숫자를 더할 생각으로 만든 Plus함수라는걸 안다. 하지만 컴퓨터는 이것이 인자를 숫자로받는지

문자로받는지 알수가없다. 그래서 인자를 주는대로 다 받게된다. 심지어 인자 2개가 필요하지만 1개만 주어도 위처럼 `undefined` 로 계산이 되어 나오는걸 볼수있따.

이렇듯 자바스크립트에서 타입을 사용하면서 개발의 실수를 줄이기위해 타입스크립트가 만들어지게 되었다.

### Typescript가 만들어진 이유?

타입스크립트는 자바스크립트의 슈퍼셋 언어라고도 한다. 일단 가장 큰 차이점은 타입을 지정해줄수있다는것이다.

```
// JS
const a = 1
const b = '2'

// TS
const a:number = 1
const b:string = '2'

// JS
function test(a,b) {
    // ~~~
    return a+b
}

test(1234,'z') // "1234z"
test('aaa','z') // "aaaz"

// TS
function test(a:number,b:number):number {
    // ~~~
    return a+b
}

test(1234,1234) // 2468
test('aaa','z') // error
```

JS의 경우 현재 눈으로 보기에는 상수와 문자라는것을 알수있지만..  만약에 정말 긴 문자나 숫자, 또는 복잡한 함수였다면?

a와b를 보자마자 어떤 인풋/아웃풋을 받는지 단번에 아는경우는 정말 드물것이다. `test()`함수의 경우 인자를 숫자도받고 문자도 받는데 

무슨 값을 리턴하는지는 우리가 함수를 확인해봐야 알것이다.

즉 개발자가 이 변수가 무엇을 뜻하는지 , 무엇을 리턴하는지 알려면 코드에서 어떤값이 리턴이 되는지 하나하나 따라가봐야한다.

하지만 TS는 인풋/아웃풋의 타입을 지정해놓기 떄문에 코드를 보자마자 바로 알수있다. 

코드를 보자마자 인풋과 아웃풋이 어떤것일지 알수있는것, 스크립트 실행전에 타입을 통한 오류를 미리 잡아낼수 있는점, 

정적인 타입스크립트가 가장 사랑받는 이유라고 할수있다.

### 타입스크립트 설치하기

```
yarn add typescript
npm i -g ts-node
```

`.ts` 확장자에서 타입을 지정해준채로 코딩이 가능하다. 

하지만 브라우저는 자바스크립트밖에 이해를 하지 못하기때문에 

Ts파일을 JS파일로 변환시켜야한다. 변환시키는 방법에는 2가지가 존재하는데

`tsc 파일명`으로 자바스크립트파일로 수동적인 변환 방법과

`tsc -w 파일명` 을 통해서 저장을 할때마다 자동으로 변환을 시킬수있다.

### 타입스크립트에서의 타입 선언법

원시형 타입 : [https://github.com/erurang/js/blob/master/typescript/1-type/1-1-basic.ts](https://github.com/erurang/js/blob/master/typescript/1-type/1-1-basic.ts)

함수형 타입 : [https://github.com/erurang/js/blob/master/typescript/1-type/1-2-function.ts](https://github.com/erurang/js/blob/master/typescript/1-type/1-2-function.ts)

튜플 타입 : [https://github.com/erurang/js/blob/master/typescript/1-type/1-3-array.ts](https://github.com/erurang/js/blob/master/typescript/1-type/1-3-array.ts)

타입 엘리어스 : [https://github.com/erurang/js/commit/5085250155d0e621146e453c5b8d7aa9df89b41f](https://github.com/erurang/js/commit/5085250155d0e621146e453c5b8d7aa9df89b41f)

유니온 타입 : [https://github.com/erurang/js/commit/7ee4fa65e20ea57786b75a7a64f424299d56875d](https://github.com/erurang/js/commit/7ee4fa65e20ea57786b75a7a64f424299d56875d)

인터섹션 타입 : [https://github.com/erurang/js/commit/aa25812ab85faa61907edd99820ae8f290893642](https://github.com/erurang/js/commit/aa25812ab85faa61907edd99820ae8f290893642)