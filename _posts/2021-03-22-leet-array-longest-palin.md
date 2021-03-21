---
layout: post
title:  "가장 긴 팰린드롬(longest-palindromic-substring)"
subtitle: "가장 긴 팰린드롬(longest-palindromic-substring)"
categories: boj
tags: queuestack
comments: true

---
![가장 긴 팰린드롬(longest-palindromic-substring)](https://leetcode.com/problems/longest-palindromic-substring/)

이 문제 방식을 풀때는 일명 슬라이딩 윈도우 라는것을 이용해야한다.

```
class Solution:
    def longestPalindrome(self, s: str) -> str:
        if len(s) < 2 or s == s[::-1]:
            return s
        
        def sol(arr,left,right):
            
            while left >= 0 and right < len(s) and arr[left] == arr[right]:
                left -=1
                right +=1
            
            return arr[left+1:right]
        
        res = ""
        
        for i in range(len(s)-1):
            res = max(res,sol(s,i,i),sol(s,i,i+1),key=len)
        
        return res
        
```

일단 알아둘점.

굳이 팰린드롬을 재귀적으로 체크하지않아도 리스트를 리버스시킨것과 기존것을 비교해서 같을때 우린 팰린드롬이라고 말할수있다.

그리고 이 문제는 앞에서부터 한칸한칸 움직이면서 팰린드롬을 체크해나가면 시간초과가 나온다.

만약에 1000개짜리 길이의 단어가 있을때 맨안쪽 중앙 문자만 다르고 나머지는 같다고했을떄

1000의 길이를 재귀적으로 잴때 1000에서 1000을 보고 틀리면 999에서 999를 보고.. 하면 시간이 엄청 걸릴수밖에없다.

위의방법은 이것을 이해해야한다.

일단은 처음부터 한칸씩 봐가는건 같다. 근데 이제 중요한건 sol()함수에서의 역할이다.

처음 들어온 좌표를 기준으로해서 왼쪽 한칸 오른쪽 한칸씩 계속해서 늘려나가는거다. 

그리고 만약에 틀렷을때 우리는 left+1 right-1 을 리턴하게되는데 이 이유는 조건에서 틀렷을때 그전 조건에서 맞아서 넘어갔을때까지가 팰린드롬이기때문이다.

그리고 짝수일경우 홀수일경우 2가지 경우를 대비해야한다. 그래서 

sol(s,i,i) sol(s,i,i+1) 를 한다.

그리고 각결과에 따른 key=len으로 가장긴 것을 res에 넣어둔다.

