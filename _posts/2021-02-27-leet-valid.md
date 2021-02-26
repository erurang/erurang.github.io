---
layout: post
title:  "유효한 괄호(valid-parentheses)"
subtitle: "유효한 괄호(valid-parentheses)"
categories: boj
tags: queuestack
comments: true

---
[유효한 괄호(valid-parentheses)](https://leetcode.com/problems/valid-parentheses/)

정말 간단한 배열을 이용하는 문제인데 나는 코드가 너무 더러워보여서 여기서 더 줄이고싶었다.

아래는 내 제출코드
```
class Solution:
    def isValid(self, s: str) -> bool:
        
        stack = []
        left = ["{","[","("]
        right = ["}","]",")"]
        
        if len(s) <= 1:
            return False
        
        if s[0] in right:
            return False
        
        for i in s:
            if i in left:
                stack.append(i)
            else:
                if not stack:
                    return False
                check = stack.pop()
                if check == left[0] and i == right[0]:
                    continue
                elif check == left[2] and i == right[2]:
                    continue
                elif check == left[1] and i == right[1]:
                    continue
                else:
                    return False
                
        if stack:
            return False
        return True
```

책에 적힌 제출코드
```
stack = []

# 테이블을 이렇게 만들어놓아서 위에서 예외처리하지않아도 예외처리되게끔 처리됨
table = {
    ')':'(',
    '}':'{',
    ']':'['
}

for char in s:
    # 키값이 아니면
    if char not in table:
        stack.append(char)
    # 예외처리도 한번에
    elif not stack or table[char] != stack.pop():
        return False
    # 예외처리도 한번에
    return len(stack) == 0
```