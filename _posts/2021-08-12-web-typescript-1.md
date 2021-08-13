---
layout: post
title:  "Typescript란?"
subtitle: "Typescript란?"
categories: web
tags: typescript
comments: true

---

### Typescript가 만들어진 이유?

타입스크립트는 자바스크립트의 슈퍼셋 언어라고도 한다. 
일단 가장 큰 차이점은 타입을 지정해줄수있다는것이다.

간단히 아래코드를 통해 자바스크립트와 타입스크립트의 차이를 비교해보자.

```
// JS
const a = 1
const b = '2'

// TS
const a:number = 1
const b:string = '2'
```

JS의 경우 현재 눈으로 보기에는 상수와 문자라는것을 알수있지만.. 

만약에 정말 복잡한 코드였다고 가정해보자.

a와b를 보자마자 어떤 인풋/아웃풋을 받는지 단번에 아는경우는 정말 드물것이다.

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