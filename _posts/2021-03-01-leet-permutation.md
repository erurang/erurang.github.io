---
layout: post
title:  "순열(permutation)"
subtitle: "순열(permutation)"
categories: boj
tags: backtracking
comments: true

---
[순열(permutation)](https://leetcode.com/problems/permutations/)

문제 풀떄는 그거만신경썻다.

일단 숫자를 하나 집는다. 그리고 집어온 숫자를 기억해야한다는걸 초점에둿다

그래서 path안에 이미 숫자가 존재하면 컨티뉴로 넘겼다.

```
class Solution:
    def permute(self, nums: List[int]) -> List[List[int]]:
        # 하나집고
        # 다음꺼집고
        # 0일때 돌아감
        # 반복
        ans = []
        def per(n_list, path):
    
            if len(path) == len(nums):
                ans.append(path[:])
                return

            else:
                for i in nums:
                    if i in path:
                        continue
                    path.append(i)
                    per(n_list,path)
                    path.pop()

        per(nums,[])
        
        return ans

```

이건 책 방식
```
# 결과값
res = []

# 이전걸 기억함
prev = []

def dfs(nums):
    
    if len(nums) == 0:
        res.append(prev[:])
    
    for e in nums:

        # 일단 현재의 nums를 복사함
        next_elem = nums[:]
        
        # 지금 해당하는 값을 지움
        next_elem = next_elem.remove(e)

        # 지운값을 넣고
        prev.append(e)
        
        # dfs 진입 
        dfs(next_elem)
        
        # 루프가 끝나고나면
        prev.pop()
        
dfs(nums)

```