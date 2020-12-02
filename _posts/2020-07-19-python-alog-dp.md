---
layout: post
title:  "동적계획법(Dynamic Programming)과 분할정복(Divide and Conquer)
"
subtitle: "동적계획법(Dynamic Programming)과 분할정복(Divide and Conquer)
"
categories: python
tags: algorithm
comments: true

---

동적계획법
- 큰 문제가 있으면 문제를 잘게 나눠서 해결한뒤에 
작은 문제의 결과값을 가지고 큰 문제들을 해결해나감. 
가장 최하위의 해답을 구한후에 저장한뒤에 
그 결과값을 이용해서 큰 문제를 풀어나감

- 각 문제를 딱 한번만 풀기 위해서 결과값을 저장하는 
   memoization 기법 이용
	- 예: 피보나치 수열	

분할정복
- 문제를 나눌 수 없을때까지 나누어서 각각의 결과값을 다시 합병하여 문제의 답을 얻는 알고리즘
  - 일반적으로 재귀함수로 구현

- 문제를 잘게 쪼갤때, 부분문제는 서로 중복되지 않음
  - 예: 병합정렬 , 퀵  정렬 등

공통점
- 문제를 잘게 쪼개서, 가장작은 단위로 분할

차이점
- 동적계획법
  - 부분문제는 중복되어, 상위 문제 해결시에 결과값이 재활용
  - memoization 기법 사용

- 분할 정복
  - 부분 문제는 서로 중복되지않음
  - momoization 기법 사용 안함

