---
layout: post
title:  "Next.js Page"
subtitle: "Next.js Page"
categories: web
tags: react
comments: true

---

## 페이지 관리

리액트에서는 HashRouter / BrowseRouter를 따로 설치하여 사용해야하지만

Next에서는 페이지를 만들때 pages 라는 폴더안에 `파일명.js`의 파일을 만들면

파일명이 페이지가 된다. `index.js`는 기본적으로 `/`을 담당하고있음.

![image](https://user-images.githubusercontent.com/56789064/105003851-85a8b900-5a76-11eb-8442-bc604236bc30.png)

위와 같이 만든후 서버를 켜주자.

<img width="306" alt="스크린샷 2021-01-19 오후 4 56 46" src="https://user-images.githubusercontent.com/56789064/105004435-521a5e80-5a77-11eb-8daa-30a56dd909e0.png">

정상적으로 작동하는것을 볼수있다.

`/about/page` 처럼 다단으로 이루어진건 어떻게 만들까?

about이라는 폴더를 만들고 `page.js`를 해주면 된다.
<img width="295" alt="스크린샷 2021-01-19 오후 5 42 16" src="https://user-images.githubusercontent.com/56789064/105009327-ad4f4f80-5a7d-11eb-99a5-adee6d2203ce.png">

<img width="378" alt="스크린샷 2021-01-19 오후 5 42 23" src="https://user-images.githubusercontent.com/56789064/105009342-b2140380-5a7d-11eb-8aee-4e8d958e8a63.png">

잘 작동하는것을 볼수있다.

## Link

```
import Link from "next/link"

<Link href="/"><a>홈</a></Link>
<Link href="/profile"><a>프로필</a></Link>
<Link href="/signup"><a>사인업</a></Link>
```
![link](https://user-images.githubusercontent.com/56789064/105011580-74fd4080-5a80-11eb-9600-1db94eae87f6.gif)


하이퍼링크 작동은 next안의 Link를 Import 시킨후 사용하면 된다!!

이 얼마나 간편한 방법인가! react router를 사용하지않아도 된다!
