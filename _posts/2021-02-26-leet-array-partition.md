---
layout: post
title:  "배열 파티션 I(array-Partition i)"
subtitle: "배열 파티션 I(array-Partition i)"
categories: boj
tags: queuestack
comments: true

---
![배열 파티션 I(array-Partition i)](https://leetcode.com/problems/array-partition-i/submissions/)

```
class Solution:
    def arrayPairSum(self, nums: List[int]) -> int:
        nums.sort()
        ans = 0

        for i in range(len(nums)):
            if i%2 == 0:
                ans += nums[i]
        return ans
```

결국에 최소로 더해서 최대가 되려면 정렬후에 앞의 2개수씩 묶은 최소값이 결국 최대값이됨