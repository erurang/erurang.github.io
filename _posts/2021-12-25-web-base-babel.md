---
layout: post
title:  "babel은 무엇인가? 설치 및 적용해보기 nodemon"
subtitle: "babel은 무엇인가? 설치 및 적용해보기 nodemon"
categories: web
tags: base
comments: true

---

# Babel은 무엇일까?

<img width="925" alt="스크린샷 2022-02-09 오전 5 15 50" src="https://user-images.githubusercontent.com/56789064/153068427-b7f5439c-289c-4e40-a489-b35931ff66a5.png">

`Babel`은 트랜스 컴파일러로 우리가 최신 버전의 자바스크립트 문법을 사용할수 있도록 도와준다.

최신 버전의 자바스크립트 문법은 다 사용가능한거 아닌가? 라는 생각은 접어두자.

우리 브라우저는 아주아주아주 아주아주아주 멍청하기때문에(..?) 현대적인 JS문법을 이해하지 못한다.

그래서 `Babel`은 우리가 코딩하는 현대적 코드를(ES6..) 구식 코드로 대신 변환시켜주어 

브라우저가 이해할수있도록 도와주는 툴이라고 생각하면 된다.

## Babel을 사용해서 최신 문법을 사용해보자

<img width="1123" alt="스크린샷 2022-02-09 오전 5 18 36" src="https://user-images.githubusercontent.com/56789064/153068778-d82d6937-a07e-40ea-8795-8251a15bcf1c.png">

[Using Babel](https://babeljs.io/setup) 공식 홈페이지에서 각 언어들을 사용할때 어떤 `babel`을 사용해야할지에 대한 설명이 나와있다.

나는 `nodejs`를 위한 `babel`을 사용할것이기 때문에 `Node`버튼을 눌러보자.

<img width="1410" alt="스크린샷 2022-02-09 오전 5 20 17" src="https://user-images.githubusercontent.com/56789064/153069029-7be7b523-d4be-4c23-bad1-5e69a9886854.png">

### babel 설치 및 적용
```
npm install --save-dev @babel/core
npm install @babel/preset-env --save-dev
```

#### babel을 적용시키기 위해서 babel.config.json 파일 만든후 설정해주자

```
{
    "presets": ["@babel/preset-env"]
}
```

`babel`은 위 파일을 보고 우리의 코드를 변환시켜줄것이다. 근데 위에 적힌 `presets`와 `/preset-env` 은 무엇일까?

`presets`은 babel이 어떻게 코드를 해석할까요? 라고 설정하는 것이고 우리는 `["@babel/preset-env"]`로 설정을 하였다.

`presets`에는 여러가지 `babel`플러그인을 설정할수 있다. 아래엔 가장 널리 쓰이는 `babel` 플러그인이다.

<img width="479" alt="스크린샷 2022-02-09 오전 5 44 35" src="https://user-images.githubusercontent.com/56789064/153072494-cb2548fe-a60b-4db3-904a-962de6f25e53.png">

그중 우리가 사용할 `"@babel/preset-env"`는 아래와 같이 설명된다.

```
@babel/preset-env는 대상 환경에 필요한 구문 변환(선택사항, 브라우저 폴리필)을 세부적으로 관리할 필요 없이 최신 JavaScript를 사용할 수 있는 스마트 사전 설정입니다. 
```

이제 `babel` 설정은 끝이 났다. 

# package.json script를 수정해서 babel을 사용해보자

나는 `express`를 최신 자바스크립트 문법을 사용하기 위해 `babel`을 사용할것이다. 


### script 수정

```
"scripts": {
    "dev": "babel-node index.js"
},
```

위 명령어는 이제 `babel-node`로 `index.js`를 해석해줘! 라는 뜻이다.

### babel 적용전

```
const express = require("express");

console.log("hi im express");
```

`babel` 적용전 코드는 보는것처럼 아주 못생겼다. 최신 문법이 사용되고 있지않다. 하지만 우리는 최신문법을 사용하기 위해 `babel`을 적용시켰다!

### babel 적용후

```
import express from "express";

console.log("hi im express");
```

`babel` 덕분에 우리는 최신 문법으로 개발할수 있게 되었다.

# babel을 nodemon과 함께 사용해보자

<img width="1123" alt="스크린샷 2022-02-09 오전 5 18 36" src="https://user-images.githubusercontent.com/56789064/153068778-d82d6937-a07e-40ea-8795-8251a15bcf1c.png">


<img width="1390" alt="스크린샷 2022-02-09 오전 5 48 56" src="https://user-images.githubusercontent.com/56789064/153073130-78d0b9fa-2a07-442b-9820-9ab77e4c5df0.png">

`babel`이 `nodemon` 을 이용할수 있도록 `babel` 공식 홈페이지에서 `nodemon` 버튼을 누르고 따라서 설치해보자. 

우리는 이미 `@babel/core`를 설치했기때문에 `@babel/node`만 설치한 후 `script`를 수정해주자.

```
"scripts": {
    "dev": "nodemon --exec babel-node index.js"
},
```

`nodemon`이 `--exec` 실행해 `babel-node`로 `index.js`를! 이라는 명령어이다. 

![스크린샷 2022-02-09 오전 6 07 35](https://user-images.githubusercontent.com/56789064/153075773-a2a1f99c-1841-4569-9fe8-2ffbcc71a40f.png)

이로써 최신문법을 사용하기 위한 `babel` 세팅이 끝이 났다.