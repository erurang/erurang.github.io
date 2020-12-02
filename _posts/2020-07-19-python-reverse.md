---
layout: post
title:  "list 원소 역정렬하는 [::-1]"
subtitle: "github.io 쓰는 첫 글"
categories: python
tags: grammar
---

- 리스트의 각 원소가 str일때 원소마다 역정렬 출력을 어떻게 할수 있을까?

---

Python에서 sort(),reverse()는 숫자만 가능하다. str일땐 에러가 뜬다.   
그럼 str은 어떻게 처리할것인가? (문자열뿐 아니라 list나 tuple도 가능하다)   

```
s = 'abcde'
print(s[::-1] # 'ebcba'
```

문자열을 [::-1] 이라는 인덱스로 호출을 하면,   
아주 단순하게 해당 문자열을 뒤집은 결과를 반환해준다.   

그럼 만약 [3:0:-1] 이라는 인덱스를 호출하면??   
```
s = 'abcde'
print(s[3:0:-1]) # dcb
```

3번 인덱스(d)부터 1번 인덱스 (b)까지 출력을 한다.   

```
s = 'abcde'
print(s[3::-1]) # dcba
```
3번 인덱스(d)부터 0번 인덱스(a)까지 출력을 한다.   