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

현재는 내 회사계정으로 로그인이 되어있는 상태다. 내 개인계정으로 바꿔보자.

## 로컬에 연결된 계정을 바꿔보자.

```
git config --global user.name 변경할 계정 이름
git config --global user.email 변경할 계정 이메일
```

![스크린샷 2022-07-19 오후 4 13 02](https://user-images.githubusercontent.com/56789064/179689078-3094c149-7a8f-47c7-8780-157f763f3c68.png)

## 계정을 바꿧으니 Push를 해보자

![스크린샷 2022-07-19 오후 4 15 41](https://user-images.githubusercontent.com/56789064/179689583-d564c1e7-28c9-4732-88da-4c36f1ebce04.png)

`Push`명령이 거부되었다고한다. 내 `erurang.github.io` `repo`에 접근이 거부되었다.

로그인이 정상적으로 되지않았다는 뜻이다. (당연히 이메일만 바꿧으니까..)

재 로그인을 위해 `Github`에 관련된 `keychain`을 삭제해줘야 한다.

## keychain 삭제

<img width="736" alt="스크린샷 2022-07-19 오후 4 16 44" src="https://user-images.githubusercontent.com/56789064/179689813-c6374cac-8e43-463f-8112-47c6812c856d.png">

`Github`와 관련된 모든 키체인을 삭제하자.

## 다시 Push

![스크린샷 2022-07-19 오후 4 19 51](https://user-images.githubusercontent.com/56789064/179690465-ca96ff00-4a34-4e90-bdef-a964362dd060.png)

`keychain`을 삭제한후 다시 `push` 명령을 하였다. 아까와는 다르게 비밀번호를 치라고 나온다.

하지만 내 비밀번호를 입력하면 `personal access token`이 필요하다고 나온다.

`Github`에서 안전상의 이유로 진짜 비밀번호를 입력하지 않고 토큰을 통해 로그인하도록 만든것이다.

## Github에서 비밀번호 토큰 발급 후 로그인

![스크린샷 2022-07-19 오후 4 21 35](https://user-images.githubusercontent.com/56789064/179690826-81acea3b-96e0-4f70-868b-eef26783a6e9.png)

`Github` 로그인 후 `setting/developer settings` 경로로 가보자.

`Personal access tokens`라는 탭이 보인다. 클릭후에 오른쪽 상단 `Generate new token`을 클릭한다.

![스크린샷 2022-07-19 오후 4 23 46](https://user-images.githubusercontent.com/56789064/179691259-1bf2b4e0-2ea2-4789-bdd7-709bab10602e.png)

내가 접근을 허용하고자하는 부분을 정할수있다. `repo`만 선택해도 사용하는데 큰 문제는 없다.

`repo` 체크후에 `Generate token`을 클릭하고 토큰을 만들자.

![스크린샷 2022-07-19 오후 4 25 30](https://user-images.githubusercontent.com/56789064/179691596-28a467e0-0fd3-46a8-87c8-a8134161de72.png)

이젠 우린 이 토큰을 사용하여 커맨드에서 로그인하면 된다. (토큰은 기억해둘것!)

다시 `push`를 해보자. `password`에 위에서 발급받은 토큰을 입력하자.

![스크린샷 2022-07-19 오후 4 27 47](https://user-images.githubusercontent.com/56789064/179692095-3bf258d9-1135-476c-b9cc-fcec64fa80b4.png)

`push`가 올바르게 작동한것을 볼수있다.
