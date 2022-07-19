---
layout: post
title: "로컬 Git 계정 전환하기"
subtitle: "로컬 Git 계정 전환하기"
categories: github
tags: github
comments: true
---

## 한 로컬에서 두개의 Git계정을 가질순 없다.

`A`계정 레포에 업데이트를 하고싶어도, 내 컴퓨터가 `B`계정으로 로그인이 되어있다면, `git` 명령어들을 사용할수 없게 된다.

이 문제를 해결하기 위해서는 현재 계정을 로그아웃후 새로운 계정을 로그인 하는 방법이 필요하다. 이 과정에 대해 알아보자.

## 먼저 로컬에 연결되어있는 계정을 확인한다

```
git config user.name
git config user.email
```

<img width="352" alt="스크린샷 2022-07-19 오후 4 10 07" src="https://user-images.githubusercontent.com/56789064/179688501-d4974078-296c-45f6-bda5-bec9752e4fe6.png">

현재는 내 회사계정으로 로그인이 되어있는 상태다. 이것을 내

## 로컬에 연결된 계정을 바꾼다

```
git config --global user.name 변경할 계정 이름
git config --global user.email 변경할 계정 이메일
```

![스크린샷 2022-07-19 오후 4 13 02](https://user-images.githubusercontent.com/56789064/179689078-3094c149-7a8f-47c7-8780-157f763f3c68.png)

##
