---
layout: post
title:  "섬의 갯수(number-of-islands)"
subtitle: "섬의 갯수(number-of-islands)"
categories: boj
tags: queuestack
comments: true

---
[섬의 갯수(number-of-islands)](https://leetcode.com/problems/number-of-islands/)

이건 앞서 백준문제를 bfs로 푼문제를 dfs로 릿에서 풀어본건데 dfs일떄 미약하게나마 속도가 더 빨랏다.

```
class Solution:
    def numIslands(self, grid: List[List[str]]) -> int:
        count = 0
        def dfs(x,y):
            if x < 0 or y < 0 or len(grid) <= x or len(grid[0]) <= y or grid[x][y] != '1':
                return

            grid[x][y] = 0

            dfs(x,y+1)
            dfs(x,y-1)
            dfs(x+1,y)
            dfs(x-1,y)


        for i in range(len(grid)):
            for j in range(len(grid[0])):
                if grid[i][j] == "1":
                    dfs(i,j)
                    count +=1
        return count
```