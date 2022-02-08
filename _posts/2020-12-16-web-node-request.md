---
layout: post
title:  "서버가 request(요청)받으면 response(응답) 해주기"
subtitle: "서버가 request(요청)받으면 response(응답) 해주기"
categories: web
tags: nodejs
comments: true

---

# Cannot Get /

<img width="279" alt="스크린샷 2022-02-09 오전 6 39 11" src="https://user-images.githubusercontent.com/56789064/153080129-8b8011d8-091d-4202-b721-3202c1fcb62c.png">

자 우리는 이전시간에 이 무시무시한 서버 메세지를 보았다. 위 에러의 뜻은 이렇게 해석할수있다.

### `Cannot` 할수없어. `GET` 가져올수없어 `/` `url`을 

다르게 시험해볼까? `http://localhost:4000/zzzz`로 한번 접속해보자

![스크린샷 2022-02-09 오전 6 45 21](https://user-images.githubusercontent.com/56789064/153080904-49b48067-1ad8-40df-9281-9b1408f6f2e4.png)

이번엔 `Cannot Get /zzzz`가 우릴 반기는걸 알수있다. 

`GET`은 `HTTP`와 서버와 통신하기 위한 통신방법이다. `HTTP Method`는 [이곳]()에 정리해두었다.

즉 누군가가 서버에 `/` 를 요청을 한다면 서버는 `/`에 대한 알맞은 처리를 하여 응답(response)을 해주어야 한다는 뜻이다.

# response(응답)을 해보자

```
import express from "express";

const app = express();
const PORT = 4000;

// request를 처리해보자

app.get("/", () => console.log("누군가 home으로 접속했어!"));
//

app.listen(PORT, () => console.log(`listening on http://localhost:${PORT}`));
```

`index.js` 서버 파일을 위처럼 수정한 후에 로컬호스트로 접속해보자.

![스크린샷 2022-02-09 오전 6 56 06](https://user-images.githubusercontent.com/56789064/153082351-c75680fa-ae1a-4f0d-b36c-9fb145702ff1.png)

음.. 접속을 했는데 `Cannot Get /` 오류는 뜨지않지만 계속 로딩이 되는걸 볼수있다.. 하지만 우리의 서버가 실행된 터미널을 본다면

![스크린샷 2022-02-09 오전 6 56 51](https://user-images.githubusercontent.com/56789064/153082455-da3bae81-8473-44d8-b674-dac0f86cded0.png)

위 코드에서 `app.get("/", () => console.log("누군가 home으로 접속했어!"));` 이 부분은

누군가가 `get method`로 `/`에 접속을 하면 `"누군가 home으로 접속했어!"`를 출력해달라고 한 코드이다.

우리의 서버가 실행된 터미널에 `누군가 home으로 접속했어!`가 출력되었으니 우리 서버가 우리가 원하는 대로 사용자에게 응답을 준것이다!

그런데 왜 브라우저는 무한 로딩을 하고있을까?

### 무한로딩이 걸리는 이유

자 위의 과정은 이렇게 생각해야한다

```
브라우저에서 `/`로 request 요청 => lisetning 하고있던 Express server는 / 으로 누군가 요청을 했으니 console.log("누군가 home으로 접속했어!") 를 출력

=> 브라우저 ..??? 내가 요청한건 나한테 언제 response(응답)해주는거야???
```

그렇다. 서버는 `/`로 요청이 들어온후 요청을 받아서 처리를 했지만. 브라우저는 서버로부터 받은것이 하나도 없기 때문에(응답을 해주지 않았기 때문에) 

서버로부터 응답을 받을 때까지 무한 로딩을 하고있는 것이다. 즉 서버는 브라우저에게 우리는 응답해줬어요~ 를 알려줘야한다. 이럴때 응답코드를 사용하게 된다.

# 브라우저에게 응답해주기

```
app.get("/", (req,res) => res.send('응답 해줄게 해줄게~'));
```

우리가 넘겨준 콜백함수 인자에는 `req(request)`와 `res(response)`를 사용할수 있다.

요청이 들어왔을떄 응답을 해주기위해 `res`를 사용하고. `res`안에 정의된 메소드 `send`를 통해 서버로 응답을 보낸다.

![스크린샷 2022-02-09 오전 7 13 03](https://user-images.githubusercontent.com/56789064/153084411-3258127f-f74a-4a53-aa36-0d366762461b.png)

다시 서버를 실행시키고 `/`로 접속해보면 이번에는 무한로딩이 되지않고 서버에서 응답한 값이 브라우저에 보이는걸 알수있다.

즉 서버에서 응답을 보내서 브라우저의 요청을 끝내버린것이다.

<img width="686" alt="스크린샷 2022-02-09 오전 7 15 37" src="https://user-images.githubusercontent.com/56789064/153084751-5bd22552-949c-4b51-b0a6-1a531045d8c7.png">

`console.log(req)`를 해보면 엄청나게 많은 것이 출력되는데. 요청한 자의 정보나 쿠키나.. 어떤 메소드로 요청을 했는가.. 등등 많은것을 알수있다

`Express`의 `api`들은 [express api reference](https://expressjs.com/ko/4x/api.html#express.json%20)에서 확인할수있다.