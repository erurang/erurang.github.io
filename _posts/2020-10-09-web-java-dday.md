---
layout: post
title:  "D-day 계산기"
subtitle: "D-day 계산기"
categories: web
tags: javascript
comments: true

---

https://codesandbox.io/s/d-day-lrp5l?file=/src/index.js:786-787

```
//목표 날짜를 정함.
const xmas = new Date("2020-12-24:00:00:00+0900").getTime();

setInterval(() => {
  // 현재 시간을 받아옴 gettime은 밀리세컨으로 받아옴
  const now = new Date().getTime();
  // 목표날짜와 현재시간의 차이를 계산함.
  const distance = xmas - now;

  // 밀리세컨드를 나눠서 계산함

  const days = Math.floor(distance / (1000 * 60 * 60 * 24));
  const hours = Math.floor(
    (distance % (1000 * 60 * 60 * 24)) / (1000 * 60 * 60)
  );
  const minutes = Math.floor((distance % (1000 * 60 * 60)) / (1000 * 60));
  const seconds = Math.floor((distance % (1000 * 60)) / 1000);

  const dDay = document.querySelector(".dDay");
  dDay.innerHTML = `${days}d ${hours}h ${minutes}m ${seconds}s`;
}, 1000);

```
만들면서 공부한것

Date함수

Date 함수에선 getTime()을 하게되면 시간을 ms로 받아온다.

그래서 D-day 계산기는 목표하는 날짜를 두고

목표날짜날.getTime() - 현재날짜.getTime() 을 해서

ms로 차이를 계산하여 D-day를 계산한다.

