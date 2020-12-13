---
layout: post
title:  "Node.js를 왜 쓸까?"
subtitle: "Node.js를 왜 쓸까?"
categories: web
tags: nodejs
comments: true

---

## Node.js에 대해 알아보자.

브라우저는 javascript밖에 돌릴수 없었고 서버를 구축하기 위해 다른 서버언어를 쓸수밖에 없었습니다.

하지만 Node.js는 자바스크립트 언어를 사용하며 브라우저외의 환경에서도 사용가능하게 해줍니다. 그리고 Mongodb의 자바스크립트(mongoose) 데이터베이스까지 지원하여서 서버사이드 언어에서 node.js의 사용은 더욱더 좋아졌습니다.

그래서 javascript로 front와 backend까지 개발이 가능해졌습니다.


```
npm install 모듈명
```

우리는 터미널에서 저 명령어로 다운을 받습니다. 여기서 npm은 뭘까요?

npm은 node package manager의 줄임말로 자주쓰이고 사용되는 코드들을 자바스크립트 패키지로 만들어놓은 곳입니다. 

다운로드한 패키지들은 package.json에 기록이 됩니다. 이 파일을 보고 이 프로젝트는 어떤 모듈을 사용하는구나 하고 알수있습니다.

**npm init** 을 하게되면 package.json 파일이 자동생성됩니다.

```
{
  "name": "이름",
  "version": "1.0.0",
  "description": "프로젝트설명",
  <!-- 패키지의 메인파일 -->
  "main": "init.js",

  "type": "module",
  
  <!-- 제작자 정보 -->
  "author": "erurang",
  
  "license": "ISC",
  
  <!-- npm 명령어 -->
  "scripts": {
    "start": "nodemon init.js --delay 2"
  },
  <!-- 프로젝트 실행에 필요한 모듈 -->
  "dependencies": {
    "express": "^4.17.1",
    "pug": "^3.0.0"
  },
  <!-- 개발에 필요한 모듈 -D -->
  "devDependencies": {
    "@babel/core": "^7.12.10",
    "@babel/node": "^7.12.10",
    "@babel/preset-env": "^7.12.10",
    "nodemon": "^2.0.6"
  }
}
```
package.json에서 npm으로 설치할때 개발자에게 필요한것과 프로젝트에 필요한 것을 구분하는게 좋습니다.

개발자에게 필요한 모듈은 -D로 devDependencies로 설치합니다.