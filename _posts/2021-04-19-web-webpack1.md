---
layout: post
title:  "Webpack의 사용이유와 기본셋팅"
subtitle: "Webpack의 사용이유와 기본셋팅"
categories: web
tags: webpack
comments: true

---

## 웹팩이란 무엇인가?

웹팩은 모듈 번들러이다. 모듈번들러란 무엇인가? 우리가 웹개발을 할때 구성되는 자원들 (html,css,js,img...) 등등을 모두

모듈로 보고 이 모든파일들을 종합해서 하나의 파일로 만드는 도구입니다.

## 모듈이란?

모듈은 작은 코드 단위들을 의미합니다. (함수.. 등등)

# 웹팩이 등장한 이유는?

1. 파일단위의 자바스크립트 모듈관리의 필요성

a.js b.js 각각 두파일이 있다고 가정하자. 두파일에는 각각 a라는 변수가 지정되어있다.

```
a.js
----
const a = 10;

b.js
----
const a = 20;
```

이 파일을 index.html에 script로 적용시킨다고 한다면 a의 어떤값이 적용이될까?

```
<!-- index.html -->
<html>
  <head>
    <!-- ... -->
  </head>
  <body>
    <!-- ... -->
    <script src="./a.js"></script>
    <script src="./b.js"></script>
    <script>
        console.log(a)
    </script>
  </body>
</html>
```

a는 20이란 값이 출력되는것을 알수있다. 왜냐면 a.js에서 10을 할당하고 뒤이어 오는 파일인 b.js에서 20으로 재할당했기 떄문이다.

이렇게 개발을 하면서 변수를 신경쓰지않으면 이미 선언된 변수에 다른값을 할당할수도있다.

웹팩은 이것을 해결해준다.

2. 웹개발 자동화

코드 편집기에서 코드를 수정하고 브라우저에서 새로고침을 눌러야 적용된화면을 볼수있던것을 웹팩이 대신해준다.

html,css,js,img,css전처리기 변환을 대신해준다.

3. 웹의 빠른로딩속도와 높은성능

웹사이트의 로딩속도를 높이기위해서 브라우저에서 서버로 요청하는 파일의 숫자를 줄여(웹개발자동화) 파일을 압축하고 병합한다.

웹팩은 그때그떄 필요할때 파일을 요청하여 사용하자는 철학을 가지고있다.


## 웹팩설치

```
npm init
npm i webpack wepback-cli
```

다음과 같이 폴더를 구성하기

```
./
    index.html
    src
        --- index.js
```

pacakge.json scripts 수정

```
"scripts": {
    "build": "webpack"
}
```

webpack.config.js 생성

```
var path = require('path');

module.exports = {
  mode: 'none',
  entry: './src/index.js',
  output: {
    filename: 'main.js',
    path: path.resolve(__dirname, 'dist')
  }
};
```

`npm run build` 명령어로 빌드를 하면 src에 있는 index.js파일이 모든 의존성의 기본파일로 지정되고

이 파일을 의존하는 모든 파일들을 한파일로 합쳐서 dist/main.js로 저장해준다.