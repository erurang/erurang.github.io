---
layout: post
title:  "List Comprehension"
subtitle: "List Comprehension"
categories: python
tags: grammar
comments: true

---

사용법

```
[출력표현식 for 요소 in 입력sequence if 조건식]
```

if는 있어도되고 없어도 됨.

```
dataset = [4,True,'Dave,2.1,3]

int_data =[num for num in dataset if type(num)==int]
print(int_data) // [4,3] 출력

int_data =[num*num for num in dataset if type(num)==int]
print(int_data) // [16,9] 출력
```

파이썬에선 코드를 간결하고 빠르게 하는 정말 중요한 문법임!


