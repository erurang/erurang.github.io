---
layout: post
title:  "배치처리 시스템과 시분할 시스템"
subtitle: "배치처리 시스템과 시분할 시스템"
categories: cs
tags: os
comments: true

---

## 배치처리 시스템과 시분할 시스템

운영체제가 응용프로그램을 실행할때 실행하는 방법에 대한 방법론이라고 할수있다.

### 배치처리 시스템의 예

```
A [xxxxxxxxx]
B [xx]
C [xxxxxxxxxxxxxxxx]
```

A,B,C의 응용프로그램이 존재한다고 해보자. 그리고 `[실행시간]` 이라고 가정해보자.

배치처리 시스템은 프로그램의 실행이 끝날때까지 다른 프로그램이 CPU를 사용하지 못한다. 즉 A C B를 순서로 실행된다고 가정하면

```
[xxxxxxxxx][xxxxxxxxxxxxxxxx][xx]
```

만약에 B를 실행하기 위해서는 A와 C의 시간이 끝날때까지 계속 대기해야하는 단점을 가지고있다.

즉 다중사용자 지원을 하지못하는 옛날 컴퓨터 방식이라고 할수있다. 다중 사용자를 지원하기위해서 `시분할 시스템` 이라는것이 생겼다.

### 시분할 시스템의 예

시분할 시스템은 다중사용자를 지원하고 컴퓨터 응답시간을 최소화 시키는 시스템이다.

```
[x]|[xx]|[xx]|[xx]|[xx]|[xx]|[xx]|[xx]|[xx]|[xx]|[xx]|[xx]|[xx]|[xx]
```

응용프로그램이 특정한 기준(scheduler)을 통해 프로세스간 실행시간을 잘게 쪼개서 여러 응용프로그램이 CPU를 나누어 사용하는것을 뜻한다.

즉 응답시간이 줄어들고 여러프로그램이 동시실행되는것처럼 느끼게 된다.


## 멀티테스킹과 멀티프로세싱의 차이

멀티 테스킹은  단일 CPU가 여러 프로그램을 실행하는것처럼 보이게 하는것

멀티 프로세싱은 여러CPU에서 하나의 응용프로그램을 잘게 짤라 병렬로 실행하는것

![스크린샷 2022-01-25 오전 11 50 52](https://user-images.githubusercontent.com/56789064/150902002-09d7b9dd-add3-4efb-9878-11e27549061d.png)


<!-- ![process state1](https://user-images.githubusercontent.com/56789064/93043591-43690880-f68d-11ea-8200-024a3bad6585.jpg)
![process state2](https://user-images.githubusercontent.com/56789064/93043593-43690880-f68d-11ea-8307-e8ef782340c0.jpg)
![process state3](https://user-images.githubusercontent.com/56789064/93043589-4237db80-f68d-11ea-80f9-12f548b8a437.jpg) -->