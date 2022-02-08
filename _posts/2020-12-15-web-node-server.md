---
layout: post
title:  "express로 서버를 만들어보자"
subtitle: "express로 서버를 만들어보자"
categories: web
tags: nodejs
comments: true

---

# Express 서버생성

`import` 구문은 `babel`  세팅이 되어야만 사용할수있다. 이전에 설치한 `express` 를 적용해서 사용해보자.

```
import express from "express";

const app = express();

app.listen(4000, () => console.log(`listening on http://localhost:4000`));
```

서버는 항상 인터넷에 연결되어 있는 컴퓨터라고 생각하면 된다. 

위의 코드에서 볼수있듯이 `listen()`함수를 사용하고있는데 이것은 서버가 `Request(요청)`를 `listeing(듣고있음!)` 한다는 뜻이다.

`request(요청)`를 한다는 것은 사용자가 `http://naver.com`에 접속을 한다면 `naver` 서버는 `lisening`을 하고있다가 

이 `request`를 받고 서버에서 적절한 처리(html을 보내준다던가, 데이터를 보내준다던가..)를 해주는 것이다 

`listen(port,callback)` 함수는 2개의 인자를 받는다. `Port`에는 우리가 접속할 포트번호를 지정해 줄수있고

`callback`에는 서버가 성공적으로 실행됬을때 콜백함수를 지정해줄수있다.

나는 `() => console.log("listening on http://localhost:4000")` 를 통해 콜백을 지정해주었다.

즉 `Listen`은 서버가 시작할때 작동되는 함수이다.  `npm run dev` 를 통해서 서버를 실행해보자. 

![스크린샷 2022-02-09 오전 6 35 17](https://user-images.githubusercontent.com/56789064/153079541-1eda8e88-35d3-4255-b41d-2e61573f528d.png)

보통 서버가 시작된다면 `localhost:Port번호`로 로컬서버가 열리게 된다. 위에서 지정한 `http://localhost:4000`로 접속해 보자

<img width="279" alt="스크린샷 2022-02-09 오전 6 39 11" src="https://user-images.githubusercontent.com/56789064/153080129-8b8011d8-091d-4202-b721-3202c1fcb62c.png">

`Cannot GET/`이라는 무서운 영어가 우리를 반겼다. 하지만 성공적으로 서버가 실행된걸 알수있다.

우리는 단순히 서버를 만들기만 했다. 다음엔 우리가 만든 서버로 `request`를 조작해보자.