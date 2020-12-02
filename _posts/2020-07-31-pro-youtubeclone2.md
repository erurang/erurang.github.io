---
layout: post
title:  "Youtube-Clone 2"
subtitle: "Youtube-Clone 2"
categories: project
tags: wetube
comments: true

---

<a>https://nomadcoders.co/</a> **에서**

**유튜브클론 수강후 혼자서 다시 만들어보는 프로젝트임을 밝힘.**

init 과 app을 분할
---

```
init.js
-----
import app from "./app"; <-- export 된 app을 받아오는 import

const PORT = 4000;

const handleListening = () => {
  console.log(`Listening on http://localhost:${PORT}`);
};

app.listen(PORT, handleListening);

``` 

init에는 서버를 시작하기위한 코드만

```
app.js
-----
import express from "express";

const app = express();

export default app; <-- 다른 파일에서도 app을 쓰기위한 export

```
app에는 서버에게 구성되는것들을 모아두기위해 나눔

routes
---

이제 url 구분을 위해서 url을 총 관리하는 파일을 만듬.

```
routes.js
-----

// Global
const HOME = "/";
const JOIN = "/join";
const LOGIN = "/login";
const LOGOUT = "/logout";
const SEARCH = "/search";

// Users

const USERS = "/users";
const USER_DETAIL = "/:id";
const EDIT_PROFILE = "/edit-profile";
const CHANGE_PASSWORD = "/change-password";

// Videos

const VIDEOS = "/videos";
const UPLOAD = "/upload";
const VIDEO_DETAIL = "/:id";
const EDIT_VIDEO = "/:id/edit";
const DELETE_VIDEO = "/:id/delete";

const routes = {
  home: HOME,
  join: JOIN,
  login: LOGIN,
  logout: LOGOUT,
  search: SEARCH,
  users: USERS,
  userDetail: USER_DETAIL,
  editProfile: EDIT_PROFILE,
  changePassword: CHANGE_PASSWORD,
  videos: VIDEOS,
  upload: UPLOAD,
  videoDetail: VIDEO_DETAIL,
  editVideo: EDIT_VIDEO,
  deleteVideo: DELETE_VIDEO,
};

export default routes;
```

Global엔 어디서든 쓰일수 있는 url들

각 기능들에 따라서 하나하나 url을 만들어주고

const routes= { }에 모두 포함시켜서 export 시켜서

외부에서도 쓸수있게 해주자.

routes를 app.js에서 이용하기
---
```
app.js
-----
import express from "express";
import routes from "./routes"; <-- routes 추가

const app = express();

const check = () => {
  console.log("routes.home text");
};
app.use(routes.home,check);

export default app; 
```

localhost:4000에 접속시에 console에 routes.home test를 확인가능

![localhost](https://user-images.githubusercontent.com/56789064/89007778-5ecdbd80-d344-11ea-8a5c-e961e85bcac5.jpg)

하지만 이렇게 **무한로딩**이 되는걸 보게되는데 이유는

에러를 응답하거나 OK를 응답하거나 메세지를 응답하거나

**응답**을 해줘야 함.  그래서 우리는 위의 코드에 추가로

요청과 응답을 처리할수 있도록 (req,res)를 추가후

res.send("")로 응답을 해줌
```
const check = (req,res) => {
  console.log("routes.home text");
  res.send("Hi from home");
};
app.use(routes.home,check);
```
![res](https://user-images.githubusercontent.com/56789064/89008640-fc75bc80-d345-11ea-846c-6c411090f17a.jpg)

응답을 해주었기 때문에 무한로딩이 더 일어나지않고 페이지가 정상 표시가 되는걸 알수있다.

연결이 어떻게 시작되는가?
---
express에서 서버를 실행하면

const app = express(); 우리의 app에서 route가 존재하는지 살펴봄.

app.use(**"/" or route.home** , function )

ok~ home("/")을 요청하는구나 home을 찾은 다음에 

function이라는 함수를 실행함.

이제 function에서 처리해주는 res.send를 받아서 처리하게 만듬

이제 우리는 이사이에 middleware라는것을 넣을수있음.

middleware
---
```
const betweenHome = () => console.log("i'm between");
app.use("/", betweenHome, function);
```
![localhost](https://user-images.githubusercontent.com/56789064/89007778-5ecdbd80-d344-11ea-8a5c-e961e85bcac5.jpg)
이렇게주면 콘솔에서는 i'm between 을 확인할수 있지만

 **무한로딩**이 되는걸 볼수있음

왜냐하면 middleware는 응답을 해주지 않기때문임.

```
const betweenHome = (req,res,next) => {
  console.log("i'm between");
  next();
}
app.use("/", betweenHome, function);
```

그래서 요청을 계속 처리할수있는 권한을 주는

next();를 써서 다음으로 처리할수있게 권한을 넘겨줌.

물론 middleware를 이용해 뒷 함수가 실행되지않게

res.send()로 응답을 바로 해버리는 방법도 있음.

결론적으로 express의 모든 함수가 middleware가 될수있음.

**middleware 참조**
<a>[https://erurang.github.io/web/2020/07/27/web-node-5/](https://erurang.github.io/web/2020/07/27/web-node-5/)</a>


함수 나누기
---

routes에서 url 총 나눠 처리했듯이 

url 맞게 처리될수 있도록 function도 따로 처리해 주자.

왜냐면 매번 const check = (req,res) .. 이렇게 한화면에 다 처리하면 너무 복잡하기 때문이다. **divide and conquer!!**

실행하는 함수를 Controller로 구분지어 만들어주자.

```
import controller from "controller 파일경로";

app.use(routes.home,처리될함수(controller));
```
