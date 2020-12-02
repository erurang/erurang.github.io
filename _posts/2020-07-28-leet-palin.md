---
layout: post
title:  "Valid Palindrome & collection"
subtitle: "Valid Palindrome & collection"
categories: leetcode
tags: leetcode
comments: true

---

파이썬의 내부함수와 pop의 사용으로 풀수있다.

https://leetcode.com/problems/valid-palindrome/

```
class Solution:
    def isPalindrome(self, s: str) -> bool:
        char = []
        for chars in s:
            if s.isalnum():
                char.append(s.lower())
        
        while len(char) >1:
            if char.pop(0) != char.pop():
                return False
        
        return True
```

isalnum() <-- 여기서 이제 존재한다는 뜻은 

공백은 무시한채 알파벳이나 숫자라는 뜻이고

char에 append를 해준다.

나는 여기서 풀떄 while문에서

```
if char[0] == -1 :
char.pop(0)
char.pop(-1)
```

이렇게 했었는데 이럴필요도 없이 그냥

아예 pop을 시킨뒤에 같은지 다른지만 판단하면 된다.


처리속도를 더 높히기 위한 Deque
---

Deque에서 구현된 pop(0) 대신 popleft()를 사용하면 

더 빠른 처리속도로 풀어낼수있다.


