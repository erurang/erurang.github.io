---
layout: post
title:  "유효한 팰린드롬"
subtitle: "유효한 팰린드롬"
categories: boj
tags: string
comments: true

---

[유효한 팰린드롬](https://leetcode.com/problems/valid-palindrome/)

```
### string이 주어지고 palindrom을 판단해야할때 이용할수있는 2가지 함수

# .isalnum() : 영문/숫자일때 리턴
# .isalpha() : 글자만 리턴
# .isdigit() : 숫자만 리턴

class Solution:
    def isPalindrome(self, s: str) -> bool:
        
        strs = []
        
        for i in s:
            # isalnum 은 영문/숫자 여부를 판별함
            # 트루라면 소문자로 넘겨줌
            if i.isalnum():
                strs.append(i.lower())
            
        # 와일을 돌면서 팝을하면 자동적으로 끝과 맨앞이 리스트에서 삭제됨
        while len(strs) > 1:
            if strs.pop(0) != strs.pop():
                return False
        
        return True

# 방식은 같지만 자체적으로 속도를 줄이기위한 덱사용

class Solution:
    def isPalindrome(self, s: str) -> bool:
        
        strs = deque()
        
        for i in s:
            if i.isalnum():
                strs.append(i.lower())

        while len(strs) > 1:
            if strs.popleft() != strs.pop():
                return False
        
        return True

```