---
layout: post
title:  "순열(combinations)"
subtitle: "순열(combinations)"
categories: boj
tags: backtracking
comments: true

---
[순열(combinations)](https://leetcode.com/problems/combinations/)

[백준의 N과 M ](https://www.acmicpc.net/problem/15649) 문제와 같다.


일단 숫자하나 집고 그다음 배열부터 보고 추가함 반복을 생가으로 풀었는데

```
class Solution:
    def combine(self, n: int, k: int) -> List[List[int]]:

        # 굳이 리스트 만들어서 메모리도 시간도 더들고
        board = [x for x in range(1,n+1)]

        ans = []

        def comb(arr,path):
            # 여기도물론 n번 봐야대니가
            if len(path) == k:
                ans.append(path[:])
                return

            for i in arr:
                # 여기서 시간이 많이걸리는듯. path n을 매번 비교하니까 n이 더 걸리고
                if len(path) >=1 and i <= path[-1]:
                    continue

                path.append(i)
                comb(arr[1:],path)
                path.pop()

        comb(board,[])
        return ans
```

근데 속도가 너무 느리다.. 정답은 맞는데 1392ms에 내가푼 방식이 하위 5%란다.. 흠 속도개선을 어떻게 하면 좋을까?

```
class Solution:
    def combine(self, n: int, k: int) -> List[List[int]]:
        ans = []

        def comb(path,start,k):
            if k == 0:
                ans.append(path[:])

            for i in range(start,n+1):
                path.append(i)
                comb(path,i+1,k-1)
                path.pop()

        comb([],1,k)
        return ans
```

이건 책의 코드로 430ms로 거의 내코드와 3배차이가난다.

이건 그래서 len에서 갯수를 세야되니 n만큼 시간이 더걸리는건가? 하고 생각했는데 o(1) 로 정의되어잇다.

그래서 이걸 가만히 코드를 보면 아래책의 코드는 앞에서 탐색을할필요없으면 가지를 쳐서 하지않고

위의코드는 그냥 비교를 먼저 하기때문에 테스트케이스에대한 시간이 더 걸리는것이다

그래서 아래풀이를 이해하는것이 좋다!.