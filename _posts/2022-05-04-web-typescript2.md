---
layout: post
title:  "tsconfig설정하기"
subtitle: "tsconfig설정하기"
categories: web
tags: typescript
comments: true

---

## tsconfig 파일은 ts파일을 컴파일하는 명세이다.

대부분의 라이브러리/프레임워크는 자체적으로 `Tsconfig`가 셋팅되어있기때문에, 우리가 직접 수정할일은 없다.

하지만 우리가 직접 수정해야할 부분이 생길수도있다. 그럴때를 위해 설정을 공부를해보자.

## 연습용 폴더 만들기

```
npm init
npm i -D typescript
```

![스크린샷 2022-05-04 오후 5 20 33](https://user-images.githubusercontent.com/56789064/166645556-8db2d337-f1dc-405a-a746-e3571248552c.png)

위 사진과같이 파일을 만들어주자.

## tsconfig.json 설정

![스크린샷 2022-05-04 오후 5 15 45](https://user-images.githubusercontent.com/56789064/166644983-cdd922d9-5c98-4ef9-921c-fa166ee26496.png)

브라켓을 열면 정말 많은 설정이 존재한다.

첫번째로 우리가 자바스크립트로 컴파일하고싶은 모든 디렉토리 경로를 넣어주기위해 `include`를 설정해주자.

```
{
    "include" : ["src"],   
}
```

## 컴파일 옵션
두번째로 `ts` 컴파일을 어떻게 할것인지 설정할 `compilerOptions`를 설정해주자.

```
{
    "include" : ["src"],
    "compilerOptions": {
        "outDir": "build" 
    }
}
```

`outDir`은 `ts`를 컴파일하여 변경된 `js`파일을 어느 경로에 만들까요? 라는 뜻이다. 

위에선 현재 디렉토리를 기준으로 `build`폴더에 만들어주세요. 라는 뜻이다.

```
"scripts": {
    "build": "tsc"
  }
```

`package.json`의 `script`를 수정해준후 `npm run build`를 통해 실행하면 

![스크린샷 2022-05-04 오후 5 24 29](https://user-images.githubusercontent.com/56789064/166646128-0507048f-348a-4ae1-85c1-c7772c19f740.png)

![스크린샷 2022-05-04 오후 5 24 40](https://user-images.githubusercontent.com/56789064/166646162-b305b7e4-8350-4d10-a98d-0f90e587950e.png)

`tsc` 타입스크립트가 `js`로 변환시킨 파일을 `build`에 만들어주게된다.

우리는 `ts`에서 `Arrow function`을 썻지만 `js`에서는 매우 낮은 버전인 `var`형태로 변한것을 볼수있다.

```
{
    "include" : ["src"],
    "compilerOptions": {
        "outDir": "build",
        "target": "ES6"
    }
}
```

![스크린샷 2022-05-04 오후 5 27 33](https://user-images.githubusercontent.com/56789064/166646563-f9fdb4f4-d51d-4c9f-bcc8-3760315daa83.png)

컴파일된 `js`를 조금더 현대적인 문법으로 바꾸고싶다면? `tsconfig`에서 `target` 속성에 변환되고자 하는 버전을 설정해주면 된다.

현재는 대부분의 브라우저에서 `ES6`를 지원하기때문에 `target`은 `ES6`로 고정하는것이 좋다.

### 자동완성 기능

세번쨰로 `ts` 파일이 어느 환경에서 사용될 것인가? `lib`을 사용해 설정해줄수있다. 

이해할수있게 말해서 `ts`가 미리 그 환경을 파악후에 우리를 도와주는것이다.

```
{
    "include" : ["src"],
    "compilerOptions": {
        "outDir": "build",
        "target": "ES6",
        "lib": ["ES6","DOM"]
    }
}
```

`frontend`에서 `DOM`을 조작할때 `documnet.~` 문법을 사용한다. `lib`설정을 하지않고 `index.ts` 에서 `documnet.`를 쓴다면

![스크린샷 2022-05-04 오후 5 36 10](https://user-images.githubusercontent.com/56789064/166647783-3b4e222d-50f5-478c-ac76-16b079c57944.png)

`ts`는 우린 `document`를 찾을수없어요! 당신이 필요로하는 타겟 라이브러리를 변경해보세요! `lib`에 `DOM`을 추가해보세요! 위와같이 불평할것이다. 

![스크린샷 2022-05-04 오후 5 40 20](https://user-images.githubusercontent.com/56789064/166648411-68a666a3-e8c6-40eb-9897-35e0fdebddd9.png)

`lib`에 `"DOM"`을 설정해준다면 위 사진처럼 `ts`가 `DOM`조작에 필요한 모든 `API`들을 끌어와서 보여준다!

즉 자동완성 기능을 제공해주어 개발을 더욱더 편하게 해준다!

### 엄격모드

```
{
    "include" : ["src"],
    "compilerOptions": {
        "outDir": "build",
        "target": "ES6",
        "lib": ["ES6","DOM"],
        "alwaysStrict": true,
        "noImplicitAny": true,
        "noImplicitThis": true,
    }
}
```

`"alwaysStrict": true`를 할시에 타입스크립트는 정말 엄격!하게 모든 문법을 검사해준다.

[https://ko.javascript.info/strict-mode](https://ko.javascript.info/strict-mode) 여기에서 `use strict`를 이해해보자.

### anytype 금지

```
{
    "include" : ["src"],
    "compilerOptions": {
        "outDir": "build",
        "target": "ES6",
        "lib": ["ES6","DOM"],
        "alwaysStrict": true,
        "noImplicitAny": true,
        "noImplicitThis": true,
    }
}
```

`any`타입을 쓰지않도록 해준다.

### 경로 줄이기

프로젝트를 하다보면 `import ../../../../libs/index.ts` 같이 `../../`를 많이써서 알아보기 힘든경우가 있다.

타입스크립트는 위와같은 경우를 `@`를 사용해 `import @libs/index.ts` 이렇게 간단하게 보이도록 도와준다.

```
"baseUrl" ".",
"paths": {
      "@libs/*" : ["libs/*"],
      "@components/*" :["components/*"]
}
```

예를들어 `baseUrl` 폴더 접근의 시작점이 되는 `"."` 현재 경로에서 `[libs/*]` 폴더로 접근하는 `/*` 모든 파일은 `@libs/파일명` 명령어로 쓰겠습니다. 라는 뜻이다.

이 글에 적은 것 뿐만아니라 다양한 설정 방법이 [https://www.typescriptlang.org/ko/docs/handbook/tsconfig-json.html](https://www.typescriptlang.org/ko/docs/handbook/tsconfig-json.html) 존재한다.
