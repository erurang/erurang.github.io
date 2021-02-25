---
layout: post
title:  "가장 긴 팰린드롬 부분 문자열"
subtitle: "가장 긴 팰린드롬 부분 문자열"
categories: boj
tags: string
comments: true

---

[가장 긴 팰린드롬 부분 문자열](https://leetcode.com/problems/longest-palindromic-substring/)

처음 제출에서 시간초과가 떳다.

```
"abababababababababababababababababababababababababababababababababababababababababababababababababababababababababababababababababababababababababababababababababababababababababababababababababababababababababababababababababababababababababababababababababababababababababababababababababababababababababababababababababababababababababababababababababababababababababababababababababababababababababababababababababababababababababababababababababababababababababababababababababababababababababababababababababababababababababababababababababababababababababababababababababababababababababababababababababababababababababababababababababababababababababababababababababababababababababababababababababababababababababababababababababababababababababababababababababababababababababababababababababababababababababababababababababababababababababababababababababababababababababababababababababababababababababababababababababababababababababababababababababababababababababababababababababababa"
```
이런게 인풋으로 들어오니까 for문 2번 돌리면 시간초과가 뜨더라. 그니까 문자가 최대 1000개인데 1000x1000 백만인데 거기서 리컬시브를 계속 들어가니까 시간초과가 뜨는가보다..

```
s = "cbbd"

def check(word):
    
    if len(word) <= 1:
        return True
    elif word[0] == word[-1]:
        return check(word[1:-1])
    else: 
        return False

res = []
word_p = ""

for i in range(len(s)-1):
    # 첫단어받고
    word = s[i]
    # 다음단어부터 체크
    for j in range(i+1,len(s)):
        
        # 다음단어랑 첫단어랑 더함
        word += s[j]
        
        # 단어가 팰린드롬이면
        if check(word):
            # 팰린드롬이네 넘겨
            word_p = word

    # 루프가 다 끝난뒤에 ..
    if check(word_p) and word_p != "":
        res.append([len(word_p),word_p])
    word_p = ""
        
if not res:
    print(s[0])
else:
    print(sorted(res,reverse=True)[0][1])
```

흠.. 그럼 for를 두번 안쓰고 처리해야되는데.. --- 업데이트예정

