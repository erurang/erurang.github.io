---
layout: post
title:  "빗물 트래핑(trapping-rain-water)"
subtitle: "빗물 트래핑(trapping-rain-water)"
categories: boj
tags: string
comments: true

---

[빗물 트래핑(trapping-rain-water)](https://leetcode.com/problems/trapping-rain-water/)

[1,0,3] 이런식으로 나오면 1을 기준으로 빗물을 계산하는 방식으로 풀었는데

만약에 [3,0,1] 이런식으로 나올때는 더 큰벽을 찾지 못하기떄문에 내 방식으론 풀수가 없었다.

그래서 이걸 고민하다 고민하다 고민해본게.. 가장 큰 높이벽을 기준으로 좌 우로 리스트를 나눳다.

그 리스트를 오름차순으로 할수있도록 reverse를 하여서 어떤경우에서도 뒤에 큰벽이 있도록 만들었다.

제출코드

```
class Solution:
    def trap(self, height: List[int]) -> int:
        ans = 0
        res = []


        if height:

            max_height = 0
            max_index = 0

            for index,value in enumerate(height):
                if value > max_height:
                    max_height = value
                    max_index = index

            left = height[:max_index] + [height[max_index]]

            right = height[max_index:]
            right.reverse()

            print(left, right)

            # 시작점
            start = left[0]

            # res에 들어있는 값보다 지금 값이 한칸이라도 높으면 res를 계산해야함.
            for i in left[1:]:

                # 만약 i가 start보다 클경우
                if i >= start:
                    for x in res:
                        ans += abs(start - x)
                    start = i
                    res = []
                else:
                    res.append(i)

            # 시작점
            start = right[0]

            # res에 들어있는 값보다 지금 값이 한칸이라도 높으면 res를 계산해야함.
            for i in right[1:]:

                # 만약 i가 start보다 클경우
                if i >= start:
                    for x in res:
                        ans += abs(start - x)
                    start = i
                    res = []
                else:
                    res.append(i)

            return ans
        else:
            return '0'
```