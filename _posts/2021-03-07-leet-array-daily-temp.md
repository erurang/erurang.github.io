---
layout: post
title:  "온도차이(daily-temperatures)"
subtitle: "온도차이(daily-temperatures)"
categories: boj
tags: queuestack
comments: true

---
![온도차이(daily-temperatures)](https://leetcode.com/problems/daily-temperatures/)

내 첫 제출코드

```
class Solution:
    def dailyTemperatures(self, T: List[int]) -> List[int]:
        # for 2 -> time limit..
        res = []
        
        for index, v1 in enumerate(T):
            day = False
            for index2, v2 in enumerate(T):
                if index >= index2:
                    continue
                
                if v1 < v2:
                    res.append(index2-index)
                    day = True
                    break
            if not day:
                res.append(0)
                
        return res
```

당연히 제출할때부터 For문 2번돌아서 시간초과라고 생각했다.

위코드는 포문을 2번돌면서 뒤의 숫자가 더클때 인덱스끼리 비교해서 처리하는 로직이다.

어떻게해도 포문을 어떻게 처리할지 모르겠어서.. 책을봣다

```
class Solution:
    def dailyTemperatures(self, T: List[int]) -> List[int]:
        # for 2 -> time limit..
        res = [0]*len(T)
        
        stack = []
        
        for index, value in enumerate(T):
            
            # 스택이 존재하고 스택의 끝값이 지금 뽑아온 벨류보다 작을때
            # 데이를 넣어야지
            while stack and value > T[stack[-1]]:
            # index
                last = stack.pop()
            # 현지 인덱스와 끝인덱스를 뺀게 걸린날짜
                res[last] = index-last
            # 
            stack.append(index)
        return res
```

이코드를 보자. 일단 res에는 데이를 기록할 리스트고

스택에는 내가 처리못한 인덱스를 넣을 곳이다.

포문을 진입할때 현재 스택이 존재하는지(아직 앞에서 처리못한게 있는지) 스택이 존재하면 지금의 온도와 마지막 스택의 온도를 비교한다.

이렇게 와일문을 돌리는데. 와일문을 돌릴때 여태 처리가 되지않은 온도까지 비교한다.

http://pythontutor.com/live.html#mode=edit

여기서 이코드를 돌려보면서 이해를했다..