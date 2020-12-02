---
layout: post
title:  "프로세스 구조"
subtitle: "프로세스 구조"
categories: cs
tags: os
comments: true

---

![process1](https://user-images.githubusercontent.com/56789064/95735566-ee3a0a00-0cbf-11eb-800c-61fd299fc9c9.jpg)
![process2](https://user-images.githubusercontent.com/56789064/95735570-ef6b3700-0cbf-11eb-9069-121075cb9d18.jpg)
![process3](https://user-images.githubusercontent.com/56789064/95735571-ef6b3700-0cbf-11eb-8b4e-ea1ba87ab062.jpg)
![process4](https://user-images.githubusercontent.com/56789064/95735575-f003cd80-0cbf-11eb-9c25-60e51cd77e62.jpg)
![process5](https://user-images.githubusercontent.com/56789064/95735577-f003cd80-0cbf-11eb-8325-70983bd26eb0.jpg)


코드가 실행이 될때 어떻게 메모리에 담겨 실행이 되는지 공부하게 되었다.

PC(프로그램카운터)가 한줄한줄 주소를 읽어보면서

1. text

일단 코드가 실행이 되면 text(code) 영역에 기계어로 코드가 담기고

프로그램이 시작되고 끝날때까지 메모리에 계속 담겨있다.

2. data

코드안의 변수들은 data영역에 들어간다.
프로그램이 시작되고 끝날때까지 메모리에 계속 담겨있다.

3. stack

함수 호출과 관련된 변수들이 저장되는 영역

함수호출이 완료되면 소멸함 **즉. 메모리가 낭비되는 공간이 없음. **

**스택에 한계가 있어 한계를 초과하도록삽입할수 없음**

스택영역에 저장되는 함수호출 정보를 **스택프레임** 이라고함

프로그램이 자동으로 사용하는 **임시메모리**

메모리의 높은주소에서 낮은주소로 (밑으로) 할당됨. -->  **head 영역을 over하면 stack overflow라 칭함**

계산과정

함수가 호출되면 호출된 return address를 기억후에

stack영역에 들어가서 함수호출,함수안의 변수들이 실행(계산)이 되고 RETURN ADDRESS로 값을 반환한다.

함수의 return address를 저장해놓는 이유는 함수가 안에서 거듭되어 실행이 될때 어디서 오류가 낫는지 트랙킹을 할수있기 떄문이다.

4. heap

사용자가 직접 관리할수있는 영역

malloc을 통해서 메모리공간을 동적으로 할당하고 free를 통해 해제할수있음

메모리의 낮으주소에서 높은주소 방향으로 할당되므로 --> **heap이 stack영역을 넘으면 heap overflow라 칭함**

**프로그램에 필요한 개체의 개수나 크기를 알수없을때 heap영역을 사용함**

**heap을 할당함으로써 속도저하 heap을 해제함으로써 속도저하가 일어남**

