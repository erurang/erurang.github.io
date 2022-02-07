---
layout: post
title:  "HTML meta tag"
subtitle: "HTML meta tag"
categories: web
tags: html
comments: true

---

## HTML Meta tag

Head 태그 안에 들어가는 meta 태그는 브라우저가 우리 html의 정보를 얻을때 사용되는 태그이다

meta는 여러 속성들이 존재한다. 차근차근 알아보자

### 문자가 인코딩 되는 방식

```
<meta charset="UTF-8">
```

charset은 HTML에서 사용한 문자들이 어떤식으로 문자를 인코딩 할것인가에 대해 정의한것이다.

기본적으로 `UTF-8` 형식을 사용하며 최상위에 선언해놓는다.

## 반응형을 지원

```
<meta name="viewport" content="width=device-width, initial-scale=1.0">
```

### name과 content

```
<meta name="author" content="웹사이트를 만든 사람">
<meta name="description" content="웹사이트 설명">
```

### 서버나 사용자의 작동방식을 변경할수있는 방법

http의 통신 프로토콜

```
<meta http-equiv="">
```

