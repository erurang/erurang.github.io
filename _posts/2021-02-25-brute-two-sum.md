---
layout: post
title:  "두 수의 합"
subtitle: "두 수의 합"
categories: boj
tags: bruteforce
comments: true

---

[두 수의 합](https://leetcode.com/problems/longest-palindromic-substring/)

10^3 까지 있어서 왠지 2중돌리면 시간초과 날거같아서 생각하다가 이렇게 풀어봤음

제출코드 (런타임 40ms)

```
class Solution:
    def twoSum(self, nums: List[int], target: int) -> List[int]:
        left = 0
        right = 1

        while True:
            if left < right and len(nums) > left and len(nums) > right:
                if nums[left] + nums[right] == target:
                    return [left,right]
                    break
                else:
                    if right <= len(nums) and right+1 != len(nums):
                        right +=1
                    else:
                        left +=1
                        right = left +1

```

작년에 제출한 코드(런타임 5932ms) 거의 150배 차이..

```
class Solution:
    def twoSum(self, nums: List[int], target: int) -> List[int]:
        for i in range(len(nums)):
            for j in range(i+1,len(nums)):
                if nums[i] + nums[j] == target:
                    return [i,j]
```

