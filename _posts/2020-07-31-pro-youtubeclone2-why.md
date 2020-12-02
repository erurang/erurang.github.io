---
layout: post
title:  "의문! 왜?? /users/users로 쳐야 접속이 될까?"
subtitle: "의문! 왜?? /users/users로 쳐야 접속이 될까?"
categories: project
tags: wetube
comments: true

---

**왜 home은 localhost:4000/ 으로 접속이 됬는데..**

왜 users는 localhost:4000/users 가 아니라

localhost:4000/users/users로 접속해야 res.send("users")가 나타날까?

하는 의문이 생겼다.

![1](https://user-images.githubusercontent.com/56789064/89041406-48ddee00-d380-11ea-9599-0ad0d423375f.jpg)
![2](https://user-images.githubusercontent.com/56789064/89041410-4a0f1b00-d380-11ea-8251-74c994894998.jpg)

처음부터 다시 만들어보면서 왜 그렇게될까 혼자서 고민한게 그거다.

그림에서 볼수있듯이 home의 url은 "/" 이고 users의 url은 "users" 였다.

왜 users는 users/users로 해야 "asdf" 가 웹에 나타날까?

**이유는 app.use(routes.users, userRouter)에**

**userRouter.get(routes.users, (res,req) => res.send("asdf"))**

**app.use의 /users에 get으로 /users를 더했기 때문이다.**

**그래서 /users만 써서 접속이 되게 하려면 userRouter.get의 url에**

**routes.home을 하게되면 /users / << 로 처리가 되서 가능하다.**

하루종일 이유를 찾아보다가 친구한테 물어보고 해결..