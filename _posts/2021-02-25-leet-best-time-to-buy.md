---
layout: post
title:  "사고 팔기 가장 좋은 시점(best-time-to-buy-and-sell-stock)"
subtitle: "사고 팔기 가장 좋은 시점(best-time-to-buy-and-sell-stock)"
categories: boj
tags: bruteforce
comments: true

---

[사고 팔기 가장 좋은 시점(best-time-to-buy-and-sell-stock)](https://leetcode.com/problems/best-time-to-buy-and-sell-stock/)

음.. 이건 그냥 이해할때까지 외워버렸다. 이해? 아니 외웟다고 표현하는게 맞는거같다.

제출코드이다.

```
class Solution:
    def maxProfit(self, prices: List[int]) -> int:
        min_price = float('inf')
        profit = 0

        for price in prices:

            min_price = min(price,min_price)
            profit = max(profit,price-min_price)

        return profit
```

핵심은 이거다.

prices에서 값을 추출할때마다 이게 지금 까지의 최소값인지를 판단한다. 

그리고 값을 꺼낼때마다 이익을 계산하는데 최초의 이익은 0이라고 설정한다.

현재의 이익 , 꺼낸값 - 최소값 에서 큰것을 들고간다.

그럼 저점을 기준으로 제일 높을때 판매하는것이 된다.