---
layout: post
title:  "React 시작하기"
subtitle: "React 시작하기"
categories: antique
tags: antique
comments: true

---

## React 시작하기

[create-react-app](https://create-react-app.dev/docs/getting-started) 설정 홈페이지

리액트 공식 홈페이지에 Recommend Tool Chains를 보면 이런말이 적혀있다.

React를 배우고 있거나 아니면 새로운 싱글 페이지 앱을 만들고 싶다면 Create React App.

서버 렌더링 Node.js 웹사이트를 만들고 있다면 Next.js을 시도해보세요.

고정적인 콘텐츠 지향적 웹사이트를 만들고 있다면 Gatsby를 시도해보세요.

라고 나와있다. 나는 일단 리액트를 연습할것이니 `create-react-app` 을 사용한다.

일단 리액트를 설치하자. (노드는 필수적으로 깔려있어야함.)

3가지 설치방법이 있다

```
npm i create-react-app 폴더명
npx i create-react-app 폴더명
yarn create react-app 폴더명
```

잠깐 여태 `npm i 모듈명` 으로 설치를 했는데 `npm`과`npx` `yarn`이 무슨 차이야? 라고 생각할것이다.

### npm 
- 패키지들을 관리함
- 패키지를 설치/삭제/업데이트가 가능함. 실행은 불가함.
  - 실행하려면 프로젝트에 추가해서 프로젝트 자체를 실행해야함.
- package.json에 패키지들의 정보와 버전이 들어있음

### npx
- 설치하는것이아니라 실행을 함.

### yarn
- npm보다 성능/보안/버전관리를 개선한 패키지매니저

설치후에 나오는 파일을 보자.

<img width="334" alt="스크린샷 2021-01-03 오후 1 29 45" src="https://user-images.githubusercontent.com/56789064/103471793-c0002e00-4dc7-11eb-91d0-5fc81bb795fa.png">

```
node_modules : 설치된 라이브러리가 있는곳

public : 사용자에게 배포할때 보여지는곳들
- favicon.ico : 웹사이트 이미지
- index.html : 
- manifest.json : PWA 를 만들때 필요함
- rovots.txt : 웹크롤링을 위해 필요함

src : 소스코드들의 모임

.gitignore : 깃이 추적을 하지 않게 설정하는곳
.package.json : 프로젝트에 쓰이는 라이브러리와 버전이 명시되어있음
yarn.lock : yarn을 설치하고나면 
```

다 설치하고 나면 yarn 명령어들이 나온다.

<img width="553" alt="스크린샷 2021-01-03 오후 1 42 30" src="https://user-images.githubusercontent.com/56789064/103471893-88928100-4dc9-11eb-8f94-3ff65b52e1df.png">

리액트에는 기본적으로 일반적으로 사용되는 기본 환경설정을 미리 해놓는데

yarn eject는 세부적으로 설정을 할때 열어서 사용함.

eject안의 기본 패키지들은 다음과 같다.

```
Babel : ES 2015~ -> 브라우저도 이해할수있는 옛날코드로 변환해줌
webpack : 파일을 묶어서(번들) 사용자에게 제공해줌.
ESLint : 코드에 오류가있으면 그 즉시 오류를 알려줌
Jest : 유닛테스트를 도와주는 프레임워크
PostCSS : css 전처리기
```

전반적인 리액트의 구성에 대해 설명을 다 한거같다.

다음부턴 리액트의 개념에 대해 포스팅하겠다.