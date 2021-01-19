---
layout: post
title:  "http method"
subtitle: "http method"
categories: web
tags: base
comments: true

---

## http method에 대해 알아보자.

http method에는 get post put delete.. 여러 방식이 있다.

### get 방식
1. 우리가 URL로 웹사이트에 접근을하면 브라우저는 서버에 GET method를 요청합니다.
2. 서버가 클라이언트에게 html등.. 파일을 넘겨줍니다.
3. get은 주소창(url)을 이용해서 정보를 전달합니다. (제출정보가 다 적혀있음.)
4. 서버로부터 데이터를 가져와서 보여주는 용도

### Post 방식
1. 우리가 웹사이트에서 로그인/글쓰기 같은 작업을 할때 서버에 POST method를 요청.
2. 우리가 서버에 전달하는 정보는 url에 전달되지않고 http body로 전달됩니다.
3. 서버의 값이나 상태를 바꾸기 위한 용도

우리가 Post요청(서버로 전송)을 할때는 form태그의 enctype(인코딩타입)으로 방식을 지정해
데이터를 전송해야합니다. 인코딩 방법에는 3가지가 있습니다.

```
<!-- URL에 &으로 분리되고, "=" 기호로 값과 키를 연결하는 key-value tuple로 인코딩되는 값입니다. -->
application/x-www-form-urlencoded (default)
<!-- 파일을 전송할때 사용합니다 -->
multipart/form-data
<!-- 지금은 사용하지않음. -->
text/plain
```

### put 방식
1. post처럼 서버로 정보를 제출하나 **정보를 갱신하는 용도**
