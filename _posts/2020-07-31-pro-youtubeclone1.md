---
layout: post
title:  "Youtube-Clone 1"
subtitle: "Youtube-Clone 1"
categories: project
tags: wetube
comments: true

---

<a>https://nomadcoders.co/</a> **에서**

**유튜브클론 수강후 혼자서 다시 만들어보는 프로젝트임을 밝힘.**

기본 설정 설치하기
---
```
npm init
npm install express

npm install @babel/node
npm install @babel/preset-env
npm install @babel/core

npm install nodemon -D

```

npm init -> pacakge.json 생성
npm install -> node_moudules / pacakage-lock.json 생성

npm install @babel -> babel을  설치후


### .babelrc 파일을 생성후 아래 코드를 복붙

```
{
"presets":  ["@babel/preset-env"]
}
```

### pacakge.json 설정추가

```
"scripts":{
	"start": "nodemon --exec babel-node 파일명.js"
}
```

nodemon이 자동으로 저장때마다 서버를 재 시작해주면 

babel-node가 파일명.js를 해석해줌

### express로 서버 확인

```
import express from "express";

const app = express();
const PORT = 4000;

const handleListening = () => {
  console.log(`Listening on http://localhost:${PORT}`);
};

app.listen(PORT, handleListening);

```

서버가 실행이 되었는지 확인이 끝나면 다음단계로 넘어가자.