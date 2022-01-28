---
layout: post
title:  "SSR CSR의 차이점"
subtitle: "SSR CSR의 차이점"
categories: web
tags: base
comments: true

---
## Server Side Rendering vs Client Side Rendering

[네이버d2](https://d2.naver.com/helloworld/7804182)에 잘 정리되있으니 여기도 보고올것.

## SSR

![스크린샷 2022-01-29 오전 4 00 13](https://user-images.githubusercontent.com/56789064/151605786-1d66f8f5-5096-47b1-9b14-1c4b569165b1.png)

```
URL 접속 => 서버에서 HTML을 만듬 => 만들어진 HTML을 받음 => 브라우저에 표시
```

SSR을 사용하면 모든 데이터가 매핑된 (초기화 된) 서비스 페이지를 클라이언트(브라우저)에게 바로 보여줄 수 있다. 

서버에서 HTML이 모두 만들어져서 브라우저는 받은 HTML로 페이지를 표현하기만 한다. 

서버에서 페이지를 구성하는 속도가 늦어질수록 화면에 UI가 나타나는 속도는 늦어지지만 

HTML이 모두 만들어진 상태로 브라우저에 표현되기 때문에 SEO 를 관리하기 편하다.

### CSR

![스크린샷 2022-01-29 오전 4 00 33](https://user-images.githubusercontent.com/56789064/151605829-53ffee4b-5f04-4fb3-9807-0ac7f6da4573.png)

```
URL 접속 => 서버에서 모든 Javascript파일을 받아옴 => 받아온 javascript로 UI를 만들고 브라우저에 표시
```

브라우저에서 어떤 페이지를 요청하면 프론트는 서버에서 자바스크립트 파일을 받아와서 UI를 만든다.

브라우저는 모든 자바스크립트를 받아오는 초기시간이 길어질수록 UI가 늦게 만들어 지게된다.

즉 네트워크 상황이 좋지 않다면 CSR을 이용할 경우 사용자들은 글을 보기 전에 상당 시간 하얀 화면을 봐야 할 수도 있다.

그래서 데이터를 받아오는 시간 동안 loading UI를 사용하여 표시하여 페이지가 작동하고있다는 것을 사용자에게 알려주는 방식으로 로딩시간을 기다린다.

하나의 페이지에서 javascript가 동적으로 UI를 만들기 때문에 SPA(Single page application)가 CSR 인 이유이다.

