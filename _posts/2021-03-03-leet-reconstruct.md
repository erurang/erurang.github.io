---
layout: post
title:  "일정 재구성(reconstruct itinerary)"
subtitle: "일정 재구성(reconstruct itinerary)"
categories: boj
tags: backtracking
comments: true

---
[일정 재구성(reconstruct itinerary)](https://leetcode.com/problems/reconstruct-itinerary/)

여태 내가 append pop 으로 재귀를 풀어왔는데.. 그래서 내가 pop append 순으로 해보자는 생각을 못한거같다.

내가 defaultdict 를 이용하는 방식도 조금 부족하단걸 이문제에서 알았다.

```
graph = defaultdict(list)

for i in tickets:
    graph[i[0]].append(i[1])
    # graph[i[0]].sort()

for i in graph.values():
    i.sort()
```

일단 여기서부터 내가 좀 틀렸다. sort()를 두번이나 돌릴 이유가 없다 생각해서 아래에서 따로 sort를 했는데.. 더짧게는 이렇게 가능하다

```
for a,b in sorted(tickets):
    graph[a].append(b)
```

애초에 들어오는 입력값을 정렬시킨후에 그 다음에 a b를 추가하는법이다.

여기서 a,b 는 어짜피 두개 인자가 나오기때문에 그래서 a와 b로 나타낼수있다.

그다음에는 재귀가 돌아가는 과정인데 나는 처음에 어떻게 풀어야할까 고민했을때 일단 append를 한 후에 remove를 통해서 앞에 있는걸 지우고 다음으로 진행했다.

append는 내가 지나온 경로를 만드는건데..

이건 내가 처음 생각한 코드

```
def re(start):
    for i in goal[start]:
        ans.append(i)
        goal[start].remove(i)
        re(i)

re("JFK")
```

이건 책에 있는 코드

```
 def dfs(word):
    while graph[word]:
        dfs(graph[word].pop(0))
    
    res.append(word)

dfs("JFK")
```

내가 큐랑 스택을 잘 이용해야하고 재귀적인 생각을 해야하는점

dfs bfs를 기본을 다시 공부해야한다고 깨달은 문제.

