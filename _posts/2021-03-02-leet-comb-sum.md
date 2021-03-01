---
layout: post
title:  "조합의 합(combination-sum)"
subtitle: "조합의 합(combination-sum)"
categories: boj
tags: backtracking
comments: true

---
[조합의 합(combination-sum)](https://leetcode.com/problems/combination-sum/)

```
class Solution:
    def combinationSum(self, candidates: List[int], target: int) -> List[List[int]]:        
        if len(candidates) == 1 and target < candidates[0]:
                return []
            
        ans = []

        def comb(candidate,path):
            if sum(path) == target:
                if sorted(path[:]) not in ans:
                    ans.append(sorted(path[:]))
                    return
                return
            
            elif sum(path) > target:
                return

            for i in candidate:
                path.append(i)
                comb(candidate,path)
                path.pop()

        comb(candidates,[])

        return ans
```

내 제출코드다. 일단 시간이 860ms 걸렸다. 이 문제를 풀때에 생각한건

일단 중복된 수를 가져가야되니까 for문을 후보군전체를 재귀로 들어간다.

그리고 합이 같아질때 예제의 정답상 오름차순으로 처리되어서 sorted를 해서 확인후에 어펜드했다.

그래서 속도가 저렇게 늦은거지 않을까 싶다. 왜냐면 sort도 n만큼 걸리고 in도 n만큼 걸리기때문에. 

아래는 책 코드

```
def dfs(nsum,index,path):
    if nsum < 0:
        return
    if nsum == 0:
        res.append(path)
        return
    
    for i in range(index,n+1):
        dfs(csum-candidates[i],i,path+[candidates[i]])

dfs(target,0,[])
```

이건 매번 합을 구하는게아니라 숫자에서 빼면서 0일때 처리되도록 되었다. 그래서 sum을 매번 하지않고도 속도가 더 빠를것이다.

그리고 내 코드와 다르게 sort도 따로 하지않는데 이건 그림을 보면서 이해하면된다.

![image](https://user-images.githubusercontent.com/56789064/109557020-e4079380-7b1a-11eb-995a-e9f4cc8d4104.png)

이게 dfs에서 두번째 매개변수로 인해서 자연스럽게 오름차순으로 정렬된다. 왜냐면 후보군이 `[2,3,4]` 라고칠떄

첫포문 2 -> 2,3,4 두번째포문 3 -> 3,4 4 -> 4  이렇게 줄여나가며 탐색이 되기 때문이다.

위코드는 80ms로 내 기존제출코드랑 10배 속도가 차이난다.
