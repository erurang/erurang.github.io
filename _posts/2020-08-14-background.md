---
layout: post
title:  "속성 - background"
subtitle: "속성 - background"
categories: antique
tags: antique
comments: true

---


background
---

![background image](https://user-images.githubusercontent.com/56789064/90164139-f665ee00-ddd1-11ea-86bf-0fe5623424c8.jpg)

```
background-image : url(경로);
```

원본사진은 우측상단처럼 나무한그루이지만

url을 적용햇을땐 x축과 y축으로 반복된다는걸 알수있다.

그래서 이걸 조정하기위해

```
background-repeat : repeat <-- 기본값
background-repeat : repeat-x <-- x축으로만 노출
background-repeat : repeat-y <-- y축으로만 노출
background-repeat : no-repeat <-- 이미지 하나만 노출
```

default는 repeat이고 조정하기 위한 설정이 있다.

그런데 사진의 위치도 조정하고싶다면?

![background position](https://user-images.githubusercontent.com/56789064/90165321-70e33d80-ddd3-11ea-93d1-3b61ce1a303a.jpg)

```
background-position : x y;
```

여기서 px은 이미지 좌측상단을 기준으로 처리를 함

화면을 스크롤을 할때는 어떻게 처리할까?
```
background-attachment : scroll <-- 기본값
background-attachment : local
background-attachment : fixed <-- 화면에 고정되어있음
```

이 모든걸 축약해서 쓸수있다.

```
background: [-color] [-image] [-repeat] [-attachment] [-position];


background: yellow url(https://www.w3schools.com/CSSref/img_tree.gif) no-repeat center top;
```
