---
layout: post
title:  "router를 만들어보자"
subtitle: "router를 만들어보자"
categories: web
tags: nodejs
comments: true

---

# bad routing 처리

만약 서버에서 응답해주는 `Url`이 3개가 있다고 해보자.

```
/login

/user/profile

/video/select
```

그렇다면 여태 배워왔던 방식으로 우리는 아래 방식으로 url을 처리하게 될것이다.

```
app.get("/login", ...response);
app.get("/user/profile", ...response);
app.get("/video/select", ...response);
```

`/user/profile /video/select` `/user`, `/video`로 시작하는 Url을 따로 묶을수는 없을까?

# root url을 통합해보자

```
const homeRouter = express.Router()
const userRouter = express.Router()
const videoRouter = express.Router()

app.use("/", homeRouter);
app.use("/user", userRouter);
app.use("/video", videoRouter);
```

방법은 간단하다 `express`에서 지원하는 `Router()`를 사용하면 된다. 라우터를 생성하여 루트 주소를 고정해주었다.

루트 주소를 고정해주기만 하였기 떄문에. 아직까지 우리가 원하는 `/user/profile /video/select` 는 작동하지 않는다.

`Router`를 사용하여 `Url`를 접근해보자

```
homeRouter.get("/login", (req, res) => res.send("로그인 접속!"));
userRouter.get("/profile", (req, res) => res.send("프로필 접속!"));
videoRouter.get("/select", (req, res) => res.send("select 접속!"));
```
![스크린샷 2022-02-09 오전 8 24 03](https://user-images.githubusercontent.com/56789064/153092610-4c2984b2-facd-4e54-8a79-c5e2951255da.png)

라우터로 루트를 고정시켜서 한결 라우터 관리가 쉬워졌다! 이렇게 라우터를 나눔으로써 각 url에 맞는 폴더를 나눠서 유지보수를 쉽게 할수있게 된다.

# 동적 라우팅 처리

만약에 동적 라우팅처리를 하지 않는 경우 어떻게 라우트 처리를 할수있을까?

```
userRouter.get("/1", (req, res) => res.send("유저 아이디 1 정보!"));
userRouter.get("/2", (req, res) => res.send("유저 아이디 2 정보!"));
userRouter.get("/3", (req, res) => res.send("유저 아이디 3 정보!"));
```

이런 끔찍한 경우를 겪지않게 하기위해 express에는 동적 라우팅 기능이 구현되어있다.

### 우리는 특정 유저에 대한 정보를 유저Id를 통해 찾는다고 해보자

`/user/userID` 형식으로 1번 유저의 정보를 얻기위해서 `/user/1` 로 서버에 요청을 해야할것이다.

이런 동적 라우팅은 어떻게 처리해야할까? 동적으로 사용하고자 하는 변수명 앞에 :를 붙여 `:변수명`로 사용하면 된다.

```
userRouter.get("/:id", (req, res) => res.send("유저 아이디 정보!"));
userRouter.get("/profile", (req, res) => res.send("프로필 접속!"));
```

![스크린샷 2022-02-09 오전 8 46 20](https://user-images.githubusercontent.com/56789064/153094747-d857b897-ad25-4d35-9818-757eaebf8194.png)

`user/1`로 접속하였을때 `유저아이디 정보!`를 성공적으로 응답한것을 볼수있다.

즉 동적라우팅은 우리가 `url`에 `param`을 넣는것을 가능하게 해준다. 

```
userRouter.get("/:id", (req, res) => {
  console.log(req.params)
  res.send("유저 아이디 정보!")
});
```

![스크린샷 2022-02-09 오전 8 54 01](https://user-images.githubusercontent.com/56789064/153095546-0a09d3c8-f6d9-485e-acbc-55d814157077.png)

`http://localhost:4000/user/3`으로 접속한 후에 콘솔에 무엇이 뜨는지 확인해보자.

`url`에서 우리가 동적으로 지정한 변수명 `id` 를 `req.params`에 `{ id: '3' }` 객체 형태로 사용할수 있는걸 볼수있다.
