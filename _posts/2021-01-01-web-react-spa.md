---
layout: post
title:  "SPA에 대해"
subtitle: "SPA에 대해"
categories: web
tags: react
comments: true

---

## Single Page Application

왜 싱글 페이지 어플리케이션일까?

그리고 왜 이것으로 리액트(대표적 라이브러리)가 이루어져 있을까?

사용자가 사용할때 딱 단일페이지 하나! 단 하나!의 html만 가져온다.

즉 하나의 페이지에서 내용만 바뀌는 것이다. 서버로부터 새 페이지(html)를 불러오지 않고 

현재 페이지를 동적으로 다시 작성하는 웹앱 혹은 그런 웹앱을 작성하는 패러다임, 디자인 패턴이다. 

최초로 한 번 페이지 전체를 로드한 후에는 데이터만 변경해서 쓸 수 있다. 

서버로부터 정적 파일을 한 번이나 여러 번에 걸쳐 다운로드 받고 사용자와 상호작용 중 필요한 데이터만 서버에서 동적으로 받는다. 

즉 맨처음에만 모든파일을 다운받은후 그 다음에는 json/xml으로 데이터를 교환할때 뺴고는 

네트워크 대역 자원이 드는 부분이 없다. 

## Component

컴포넌트 개념이란 컴포넌트들이 모여 한 페이지를 작성하고 특정 부분만 데이터를 바인딩하는 개념

<img width="783" alt="스크린샷 2021-01-01 오전 10 19 56" src="https://user-images.githubusercontent.com/56789064/103431895-e7fe5e80-4c1a-11eb-8ffc-acbd994e2f4c.png">

그래서 이 컴포넌트로 각각의 페이지로 구성되어 있는게 리액트이다.

## 구현방식

SPA 구현 방식은 여러 가지가 있는데 대표적인 게 Ajax를 통한 콘텐츠 로드이다. 

페이지 새로고침 없이 데이터가 교환되고 업데이트되는 것이다.

SPA 방식은 form 작성 시 ajax를 이용해서 form 데이터의 일부를 전송하는데, 그럼 서버는 JSON 데이터를 응답으로 보내주는 식이다. 

SPA는 화면 이동 시 필요한 데이터를 서버로부터 JSON 데이터로 전달받아 동적으로 렌더링한다.

컴포넌트 개념으로 작성된 페이지에 특히 적합한데, React를 사용해서 페이지가 컴포넌트 조각들로 이뤄진 페이지일 경우 컴포넌트의 내용 혹은 컴포넌트 자체를 교체하면 되기 때문에 SPA가 특히 좋다. 

## 그럼 한개의 페이지만 있는데 히스토리 관리는 어떻게하나??

기본적인 SPA는 CSR(Client Side Rendering, 클라이언트 사이드 렌더링) 방식을 채택한다. 

필요한 부분만 렌더링하며 새로고침이 없고 변하는 부분만 새로 그리는 것인데, URL이 바뀌지 않기에 히스토리 관리가 어려워질 수 있다. 특히 ajax 같은 경우 히스토리가 없어서 사용자가 폼 데이터를 전송한 페이지를 북마크해도 다음에 재접속했을 때 그 페이지가 아니라 초기 페이지를 보게 된다.

SPA를 구현하는 다른 방식인 Hash 방식의 경우 URL는 똑같고 URI의 차이가 생겨서 히스토리 관리가 가능해진다. 해시는 변경돼도 서버에 페이지가 갱신되지 않는다. 해시는 URI에서 #로 시작하는 문자열이다.

가령 내가 구글에 'uri'라고 검색했더니 주소창에 http://www.google.co.kr/search?q=uri 라고 나온다고 가정해보자. 이 때 URL은 google.co.kr/search 까지이다. 물음표(?) 뒤의 q=uri는 쿼리문 식별자이다. 그래서 이 부분은 URI이긴 하지만 URL은 아니다

### URL URI 차이

[이곳](https://lambdaexp.tistory.com/39)에 너무 이해가 잘 되게 적어서 그대로 가져온다.

URI는 인터넷 상의 자원을 식별하기 위한 문자열의 구성쯤으로 해석 될 수 있겠다.

http://en.wikipedia.org/wiki/URL
URI의 한 형태인 URL은 인터넷 상의 자원 위치를 나타낸다.

URL는 URI의 한 형태로, 바꿔 말하면 URI는 URL을 포함 하는 개념이다.
(URI > URL)

인터넷 상의 자원의 위치와 식별자.
언듯 보면 같은 것을 의미하는 듯 하다.
하지만 '자원의 위치'라는 것은 결국은 '하나의 파일 위치'를 나타내는 것임을 명심하자.

http://img0.gmodules.com/ig/images/korea/logo.gif
이와 같은 형식은 logo.gif라는 인터넷상의 자원 위치를 의미 한다.
이는 URI이면서도 URL라고 말할 수 있다.

다음은 어떠한가.
http://endic.naver.com/endic.nhn?docid=1232950
http://endic.naver.com/란 서버에 위치한 endic.nhn파일은 query string인 docid의 값에 따라 여러가지 결과를 나타낸다.

여기서 URL은 endic.nhn의 위치를 표기한 http://endic.naver.com/endic.nhn 까지이다.
내가 원하는 정보에 도달 하기위해서는 ?docid=1232950라는 식별자(Identifier)가 필요한 것이다.

결국 위의 http://endic.naver.com/endic.nhn?docid=1232950 주소는 URI이긴 하지만 URL은 아니다.

또다른 예를 들면,
http://endic.naver.com/endic.nhn?docid=1232950
http://endic.naver.com/endic.nhn?docid=1232690
위 두 주소는 같은 URL이고 다른 URI라고 할 수 있다.
(이건 좀 억지긴 하지만 개념을 이해하기 바란다.)

출처: https://lambdaexp.tistory.com/39 [프로그래머 인생길..]