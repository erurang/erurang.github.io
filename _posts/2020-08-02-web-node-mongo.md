---
layout: post
title:  "MongoDB & dotenv"
subtitle: "MongoDB & dotenv"
categories: web
tags: nodejs
comments: true

---

공식사이트에서 설치
---

mongodb community server를  설치

https://www.mongodb.com/try/download/community 


```
npm install mongoose
npm install dotenv
```

콘솔 mongod 명령어로 돌아가고 있는지 확인을 한후

mongo 명령어로 접근가능함

mongodb는 **데이터베이스**, mongoose는 **데이터베이스와 연결하는 툴**임.


이제 db를 관리해보자.
---
```
db.js
-----
import mongoose , {mongo} from " mongoose";

mongoose.connect("mongo://localhost:PORT/데이터베이스이름")

const db = mongoose.connection; <-- db에 연결

const handleOpen = () => console.log("Connected to DB");
const handleError = (error) => console.log("Error on DB Connection:${error}");

db.once("open", handleOpen);
db.on("error", handleError);

init.js
---
import "./db" <-- db를 연결해주자
```
여기서 port는 콘솔에서 mongod을 치면 나오는 port를 말함

db.once("open",handleOpen"); db를 한번(once) 실행했을 때

성공적으로 실행이 되었는지 파악하기위한 콜백함수

db.on("error", handleError); db를 실행(on) 했을때 

에러메세지가 뜰경우의 콜백함수

dotenv는 무엇인가?
---
환경설정 툴로써 어떤 설정을 숨겨서 쓰고싶을때 사용함

.env 파일을 생성하고 숨기고싶은 변수들을 적어줌
```
.env
-----
MONGO_URL="mongodb://localhost:27017/we-tube"
PORT=4000

---
import dotenv from "dotenv";
dotenv.config();

process.env.변수명;

```
저 변수들을 사용하는 파일에 process.env.변수명; 을 해주면 

정상작동 되는것을 알수있다. 우린 위의 db파일을 이렇게 수정할 수 있다.

```
import dotenv from "dotenv";
dotenv.config();

mongoose.connect("mongo://localhost:PORT/데이터베이스이름")
	-> mongoose.connect(process.env.MONGO_URL)
```

정말 깔끔해지는것을 알수있다!

.gitignore에 .env파일을 넣어서 db유출을 방지할수있음