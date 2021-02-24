---
layout: post
title:  "가장 흔한 단어(Most common word)"
subtitle: "가장 흔한 단어(Most common word)"
categories: boj
tags: string
comments: true

---

[가장 흔한 단어(Most common word)](https://leetcode.com/problems/most-common-word/)

이 문제는 string list를 다루는 문젠대 내 지식이 되게 작다는걸 꺠닫게 해준 문제가 되겠다..

```
class Solution:
    def mostCommonWord(self, paragraph: str, banned: List[str]) -> str:
        
        # 문장을 받아올 워드리스트
        words = []
        
        # 각 문장에서
        for i in paragraph:
            # 영문이라면
            if i.isalnum():
                # 소문자로 만든후 문장에 넣고
                words.append(i.lower())
            # 아니라면 공백으로 구분할수있도록 공백을 넣음
            else:
                words.append(" ")
        
        # 이때까지 words를 찍어보면
        # ['b', 'o', 'b', ' ', 'h', 'i', 't', ' ', 'a', ' ', 'b', 'a', 'l', 'l', ' ', ' ', 't', 'h', 'e', ' ', 'h', 'i', 't', ' ', 'b', 'a', 'l', 'l', ' ', 'f', 'l', 'e', 'w', ' ', 'f', 'a', 'r', ' ', 'a', 'f', 't', 'e', 'r', ' ', 'i', 't', ' ', 'w', 'a', 's', ' ', 'h', 'i', 't', ' ']
        
        # 이런식으로나옴 이것을 ''.join 을 쓰게되면
        words = ''.join(words).split()
        
        # bob hit a ball ... 이렇게 str형태로 돌려받게된다. 이것을 .split()을 통해서 공백을 제거하면 리스트로 리턴받게된다.
        
        # defualtdict로 카운트하기
        word_count = defaultdict(int)
        
        # 중복제거
        banned_words = set(banned)
        
        for word in words:
            if word not in banned_words:
                word_count[word] +=1
        
        init = 0
        word = ""
        
        for k,v in word_count.items():
            if v > init:
                init = v
                word = k
                
        return word
```

