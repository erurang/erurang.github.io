---
layout: post
title:  "컨텍스트 스위칭"
subtitle: "컨텍스트 스위칭"
categories: cs
tags: os
comments: true

---

![context switching1](https://user-images.githubusercontent.com/56789064/95840724-7df2bd80-0d7f-11eb-8742-fe05cefa56c6.jpg)
![context switching2](https://user-images.githubusercontent.com/56789064/95840729-7f23ea80-0d7f-11eb-9efe-cdfed5782636.jpg)
![context switching3](https://user-images.githubusercontent.com/56789064/95840730-7f23ea80-0d7f-11eb-91d5-cdfb5e9fd5d1.jpg)
![context switching4](https://user-images.githubusercontent.com/56789064/95840731-7fbc8100-0d7f-11eb-979e-11a6bcef88dc.jpg)

컨텍스트 스위칭이 왜 필요한가?

1. 컴퓨터가 매번 한번의 Task만 처리한다면?
- 해당 Task가 끝날때 까지 다음 Task는 계속 대기상태임.
- 반응속도가 매우느리고 사용하기 불편해짐

2. 그렇다면 다양한 사람들이 동시에 사용하는것처럼 하려면?
- 빠른속도로 task를 바꿔서 실시간처럼 보이게함. 
- -> context switching의 등장

**context switching?**
- 하나의 프로세스가 CPU를 사용 중인 상태에서 다른 프로세스가 CPU를 사용하도록 하기 위해, 
이전에 실행중인 Task(프로세스)의 상태(문맥, Register값들에 대한 정보 -> context)를 보관(저장)하고 
새로운 Task(프로세스)의 Context정보로 교체(상태를 적재)하는 작업을 말한다

- 현재 진행하고있는 Task(process,thread..)의 상태를 저장하고 다음진행할 Task의 상태값을 읽어 적용함

- Task의 대부분 정보는 Register에 저장되어 PCB로 관리됨.
- 다음실행할 Task의 PCB를 읽어 Register에 적재하여 CPU가 이전에 진행했던 과정을 연속적으로 수행가능

- Context Switching 시, Context Switching 을 수행하는 CPU 는 Cache 를 초기화하고 Memory Mapping 을 초기화하는 작업을 거치는 등 아무 작업도 하지 못하므로 잦은 Context Switching 은 성능 저하를 가져온다.  
 - 일반적으로 멀티 프로세스를 통해 PCB를 Context Switching 하는 것보다 멀티 쓰레드를 통해 TCB 를 Context Switching 하는 비용이 더 적다고 알려져있다.

- 주로 Context Switching 은 Interrupt 에 의해 발생되는데, Hardware 를 통한 I/O 요청이나, OS / Driver 레벨의 Timer 기반 Scheduling 에 의해 발생한다.

  
  
