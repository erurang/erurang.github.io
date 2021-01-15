---
layout: post
title:  "TypeScript Type지정하기"
subtitle: "TypeScript Type지정하기"
categories: web
tags: typescript
comments: true

---

## Type의 종류에 대해 알아보자

이전글의 예시로 시작한다.

`sayHi()`라는 함수에 마우스를 올리면

![image](https://user-images.githubusercontent.com/56789064/104733773-885b9380-5782-11eb-93d5-5b32866d5e78.png)

`name: any age: any gender?: any => void` 을 볼수있다.

각 변수의 자료형은 `any` 어떤 타입이든 상관없다는 뜻이다.

void는 함수에서 리턴하는 값의 종류를 뜻한다. 왜 void냐면 우리는 단순히 log를 찍기때문이고 반환값이 없기때문이다.

만약 return이 string인데 반환값을 void로 지정한다면 오류가 뜨게된다.

<img width="484" alt="스크린샷 2021-01-15 오후 11 01 23" src="https://user-images.githubusercontent.com/56789064/104735955-9b239780-5785-11eb-97a4-8d5838eb18d5.png">

return밑에 빨간줄이 그여있는걸 볼수있다. 똑같이 `tsc`로 실행하면 결과가 나오는것을 볼수있다.

<img width="352" alt="스크린샷 2021-01-15 오후 11 03 41" src="https://user-images.githubusercontent.com/56789064/104736178-ed64b880-5785-11eb-9fa4-4aefa5bd6d0f.png">

## 매번 tsc를 쳐서 ts -> js로 컴파일하기 귀찮아!

그래서 나온 패키지가 있다.

>yarn add tsc-watch -D

설치후 아래와같이 `package.json`과 `tscomfig.json`을 수정해주자.

```
{
  "name": "typescript",
  "version": "1.0.0",
  "description": "learning typescript",
  "main": "index.js",
  "license": "MIT",
  "author": "erurang",
  "scripts": {
    // tsc-watch가 ts가 수정될때마다 자동으로 dist에 수정된 index.js를 생성함.
    "start": "tsc-watch --onSuccess \" node dist/index.js\"",
  },
  "devDependencies": {
    "tsc-watch": "^4.2.9"
  }
}

```

```
{
    "compilerOptions": {
        "module": "commonjs",
        "target": "ES6",
        "sourceMap": true,
        // 컴파일이 되고난후 dist란 폴더에 생성
        "outDir": "dist"
    },
    "include": [
        // src에 있는 모든파일을 ts로 컴파일함
        "src/**/*"
    ],
    "exclude": ["node_modules"]
}
```

이제 `yarn start`로 실행하면 tsc-watch가 파일변경점을 지켜보면서 저장시에 자동으로 수정이 되는걸 볼수있다.

<img width="639" alt="스크린샷 2021-01-15 오후 11 33 18" src="https://user-images.githubusercontent.com/56789064/104739441-0f603a00-578a-11eb-8db7-7d129fe236bd.png">
