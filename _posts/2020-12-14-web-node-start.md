---
layout: post
title:  "package.json이란? express 설치와 nodemon"
subtitle: "package.json이란? express 설치와 nodemon"
categories: web
tags: nodejs
comments: true

---

# 기본 확인

노드가 먼저 설치되어 있나부터 확인해야한다. 커맨드에서 아래 명령어를 실행했을떄 깔려있다면 `node v숫자` 형식으로 돌려줄것이다.

```
node -v // node v숫자
```

`node`가 설치되어있다면 `index.js` 라는 임의 파일을 만들어서 `node index.js`명령어로 실행해보자.

#### index.js
```
console.log("hi i'm node~")
```

![스크린샷 2022-02-09 오전 4 54 10](https://user-images.githubusercontent.com/56789064/153065128-892f65bb-b087-49d4-b4db-bc3f2e1b3a36.png)

`node`가 잘 작동한다면 이젠 `npm package`를 위한 `package.json`을 작성해보자

# 패키지를 만들어보자

우리가 사용할 패키지를 만들기 위해서 `npm init` 명령어를 사용할수있다.

```
{
  "name": "test",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "repository": {
    "type": "git",
    "url": "git+깃 주소"
  },
  "author": "erurang",
  "license": "ISC",
  "bugs": {
    "url": "깃 이슈 주소"
  },
  "homepage": "깃 readme 주소",
  "dependencies": {
    // npm에서 설치한 패키지들이 적힌다. 즉 프로젝트가 실행되기위해 필요한 파일들이다.
  },
  "devDependencies": {
    // 
  }
}
```

이미 패키지 폴더를 `git`과 연결을 했을 경우 위처럼 나타날 것이다. 우리가 라이브러리(npm package)를 설치한다면 `package.json`에 모두 기록이 된다.

`dependencies`에는 이 **프로젝트를 실행하기 위한 패키지**들을 기록해 둔다. 그래서 `node_modules`를 다운받지않아도 `package.json`만 존재한다면

`npm install` 명령어를 통해서 `node_modules`에 필요한 패키지들을 다운받아 프로젝트를 실행할수 있게 된다.

`devDependencies` 에는 **개발자에게 필요한 의존성**이다. 예를들면 `babel` 같은 트랜스파일러이다.

즉 `package.json`은 이 프로젝트를 실행하기위한 명세 라고볼수있다.

# npm install

어떤 패키지를 설치할때에는 `npm install packageName` 명령어로 설치할수있다. 

이 명령어를 통해서 `npmjs.com`에서 많은 개발자들이 만든 라이브러리를 다운받을수있다. 

이 명령어로 설치를 한다면 `package.json`에는 프로젝트 실행에 필요한 패키지를 모아둔 `"dependencies": {}`에 추가되게 된다.

### 하지만 이런 설치 명령어도 본적이 있을것이다.

```
npm install --save--dev packageName
```

이 명령어로 설치를 한다면 `package.json`에는 개발자에게 필요한 패키지를 모아둔 `"devDependencies": {}`에 추가되게 된다.

둘다 똑같이 `node_modules`에 설치 된다. 개발자가 어떤것을 사용해야하는지 설치 명령어를 통해 나눈것이라고 보면 된다.

# Express 설치
```
npm i express
```

`Express`는 `nodejs` 기반으로 만들어진 서버를 만들어주는 패키지이다.

![스크린샷 2022-02-09 오전 5 00 17](https://user-images.githubusercontent.com/56789064/153066075-6203cf4f-c018-4ad1-83fa-ff254e8679b2.png)

설치를 하고나면  많은 폴더가 자동으로 설치되는데, `node_modules`에는 `npm`으로 설치한 모든 패키지가 들어있는 폴더이다.

![스크린샷 2022-02-09 오전 5 01 30](https://user-images.githubusercontent.com/56789064/153066240-d7f5b517-2050-45cf-8780-5efc7eaaacf0.png)

`express`를 다운로드 받았을뿐인데 많은 폴더들이 존재하는걸 볼수있다. 모두 `express`를 쓸수있게 준비된 파일들이라고 할수있다.

그리고 `package-lock.json` 파일도 생성되게 되는데 `package-lock.json`은 `node_modules`구조나 `package.json`이 수정되고 생성될 때 당시 의존성에 대한 정확하고 구체적인 정보(해쉬값)를 가지고있기 때문에 안전하게 알맞은 버전의 프로젝트를 다운받게 되는것이다.

### script를 수정해보자

![스크린샷 2022-02-09 오전 4 46 22](https://user-images.githubusercontent.com/56789064/153064253-90c8c505-f09f-4662-bfd1-8bd36933f572.png)

`script`는 무언가를 실행하고싶을때 사용할 명령어이다. 정말 다양한 명령어들이 있는데 이중 우리가 사용해볼것은 `start`명령어 이다

```
"scripts": {
    "start" : "node index.js"
},
```

위 스크립트의 뜻은 내가 `npm start` 라는 명령을 하면 너는 `node index.js` 명령을 실행하도록 해! 라는 뜻이다.

![스크린샷 2022-02-09 오전 4 56 59](https://user-images.githubusercontent.com/56789064/153065559-9a2674af-9adf-40cf-8239-773866492159.png)

우리는 똑같은 명령이지만 다른 명령어로 실행을 시킬수 있게 된다. 

# nodemon을 통해 

```
npm i nodemon
```

스크립트가 실행된 후 수정사항이 생겻을때 저장후 파일을 다시 실행하기위해 `script` 명령어를 다시쳐야만 한다. (npm run dev) .. 너무 귀찮지 않을까?

`nodemon`은 파일에 변경사항이 존재한다면 저장을 했을떄 명령어를 자동으로 실행시켜주는 모듈이다. 

### script 수정

```
"scripts": {
    "dev": "nodemon node index.js"
},
```
`nodemon`아 `node`로 `index.js`를 실행시켜줘! 라는 뜻이다.

![스크린샷 2022-02-09 오전 6 16 39](https://user-images.githubusercontent.com/56789064/153077099-6aa6f906-39f2-4e08-ab6d-5877a6b677fc.png)

`npm run dev` 명령어로 실행해보면 `nodemon`이 명령어를 수행하고난 후에도 커맨드가 종료되지않고 계속 유지되는것을 볼수있다. 

우리가 수정사항이 생겨 매번 명령어를 다시 칠 필요가 없이 저장만 한다면 `Nodemon`이 명령어를 다시 실행해주게 되어 개발을 더욱 편하게 할수 있게 되었다.

# 최신 자바스크립트 문법을 사용하기 위해서

우리는 `Babel` 이란 트랜스파일러를 추가로 설치해 줘야한다. [babel은 무엇인가? 설치 및 적용해보기](https://erurang.github.io/web/2021/12/25/web-base-babel/)에 정리해두었다.