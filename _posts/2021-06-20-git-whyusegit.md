---
layout: post
title: "왜 형상관리를 할까?"
subtitle: "왜 형상관리를 할까?"
categories: github
tags: github
comments: true
---

## Git에 대해 알아보자.

혼자서 공부할때는 나에게 `Git`은 내 소스코드를 저장해주는 저장소였을뿐 그 이상의 의미가 없었다.

하지만 회사에 입사한 후엔 `Git`을 잘 사용하는것은 정말 중요하다는걸 알게되었다.

`Git`을 왜 쓰면 좋은지 알아보자.

## Git을 쓰면 무엇이 좋을까?

![IMG_F3728455F334-1](https://user-images.githubusercontent.com/56789064/210329241-951c6496-8d9f-4628-81ac-133dbba50c35.jpeg)

혼자 로컬에서 개발을 할때는 단순히 `push` 작업만 존재할것이다.

만약 여기서 `push2`를 기준으로 `A`라는 기능이 필요해요! 라는 요청이 들어왔다고 해보자.

![IMG_FF39F8006055-1](https://user-images.githubusercontent.com/56789064/210330141-c2a0b583-351d-430c-887f-de1175eabf43.jpeg)

`push3`에서 `push2`의 시점으로 돌아가서 이 시점의 코드로 개발을 할수있다.

![IMG_5068698E070D-1](https://user-images.githubusercontent.com/56789064/210330148-f42b2fb4-a776-4420-ba25-a4ee12241e0f.jpeg)

`push2`에서 작업한 결과물도 저장을 해야한다. 그럼 단순히 `push`를 해야할까?

`HEAD(마지막)`브랜치인 `push3`가 아닌 `push2`에서 작업을 하였기 때문에 `push` 명령어는 작동하지않는다.

이럴때 우리는 새로운 브랜치를 만들어 다른 특정시점에서 개발된 코드를 사용해 또 다른 시점을 만들수 있다.

이번엔 `push2`에서 개발하던 사항을 `push4`에 적용해주세요! 라는 요청이 들어왔다고 해보자.

![IMG_1B611F0E8DEE-1](https://user-images.githubusercontent.com/56789064/210330791-22edc5e5-5b6d-4a5b-8ef1-e27c23e2d0f1.jpeg)

`Git`은 `merge`라는 기능을 통해 현재 브랜치에 있는 내용을 다른 브랜치에 병합시키는 기능도 제공한다.

언제든 원하는 시점에서 분기(`new branch`)를 만들고 언제든지 기존의 브랜치에 병합(`merge`)할수 있다.
