---
layout: post
title:  "다익스트라 알고리즘"
subtitle: "다익스트라 알고리즘"
categories: cs
tags: algorithm
comments: true

---

## 다익스트라 알고리즘의 정의는 무엇인가?
- 한점에서 다른 한점까지 연결할때 최단거리를 구하는 알고리즘
- 즉 한 지점에서 다른 모든 노드와의 최단거리를 계산해서 연결하는 것임
- 각 노드마다 노드끼리 연결하는 간선의 가중치를 최단(최소) 로 해야하기때문에
    - heapq 최소힙을 이용해서 가장 작은 최단거리를 갱신해 나간다.


### [ 1 ] 예시 그래프로 그래프를 만들어 보자.

<img src="https://www.fun-coding.org/00_Images/dijkstra.png" width=300>


```
mygraph = {
    'A': {'B': 8, 'C': 1, 'D': 2},
    'B': {},
    'C': {'B': 5, 'D': 2},
    'D': {'E': 3, 'F': 5},
    'E': {'F': 1},
    'F': {'A': 5}
}
```
A에서 B까지 가는데 8, C까지 가는데 1 이렇게 읽을수있다.

### [ 2 ] 초기화화 힙적용

우리는 앞서 최단거리를 우선으로 찾아서 탐색해야 하므로 리스트에 push후에 pop하면 자동으로 최소값을 찾아서 리턴해주는
최소힙을 이 알고리즘 푸는데 이용하자고 하였다.


```
import heapq
```

힙 모듈을 적용한다.
모든 그래프에 대해 초기화를 한후 heappush 한다.


```
def dijkstra(graph, start):

    distances = {node: float('inf') for node in graph}
    distances[start] = 0
    queue = []
    heapq.heappush(queue, [distances[start], start])
```

다익스트라를 적용할 그래프와 시작점을 인자로 받는다.
그후 노드별로 현재 거리를 inf(무한대) 로 초기화를 시켜놓는다.
distances에는 graph에서 각 key 값들이 node로 들어가게된다. 그럼 distances는 어떤 상태일까?

```
distances = {
              A : float('inf), B : float('inf), C : float('inf), 
              D : float('inf), E : float('inf), F : float('inf)
            }
```
각 노드별로 inf로 초기화 되어있음을 알수있다.

### [ 3 ] 탐색시작

시작점을 0으로 초기화해서 탐색을 시작할 준비를 한다.
queue 라는 리스트에 heapq를 이용해 push를 해준다
그러니 현재 queue에는 어떤값이 있을까?

```
queue = [0,"A"]
```

이렇게 최단거리는 0이고 시작점은 A가 들어가있다.
이제 queue로 진입해보자.

``` 
    while queue:
        current_distance, current_node = heapq.heappop(queue)
```
여기서 저 둘 노드는 어떤값을 가지고있을까?

```
current_distance = 0
current_node = "A"
```
**여기서 잘 생각해보야한다.**
heappop이 되어 나온 current_distance는 그 전까지의 경로에서 
각 노드간의 최단거리를 distances에 갱신후에 넘어온것이다.
즉 pop되어 오는 애들은 pop되기 전까지는 그 시점까지는 가장 빠른 최단경로란 뜻이다.

위의 코드를 해석하면 A까지 오는데의 최단 경로는 0 이라는 뜻이다.

```
        for adjacent, weight in graph[current_node].items():
            distance = current_distance + weight
```

현재노드의에서 목적지(adjacent) 와 목적지까지의 가중치(weight)를 꺼내온다.

```
            if distance < distances[adjacent]:
                distances[adjacent] = distance
                heapq.heappush(queue, [distance, adjacent])
return distances
```

위의 코드를 해석해보자.

만약에 현재노드까지의최단거리 + 목적지까지의 경로값 < 현재 목적지에 기록된 최단거리보다 작다면 -> 업데이트 해야함.
업데이트 한다는 것은 최단거리를 찾앗다는 뜻. 최단거리를 heapq에 가중치,노드 로 업데이트해줌


### [ 4 ] 전체코드
```
mygraph = {
    'A': {'B': 8, 'C': 1, 'D': 2},
    'B': {},
    'C': {'B': 5, 'D': 2},
    'D': {'E': 3, 'F': 5},
    'E': {'F': 1},
    'F': {'A': 5}
}

import heapq

def dijkstra(graph, start):
    
    distances = {node: float('inf') for node in graph}
    distances[start] = 0
    queue = []
    heapq.heappush(queue, [distances[start], start])
    
    while queue:
        current_distance, current_node = heapq.heappop(queue)
        
        if distances[current_node] < current_distance:
            continue
            
        for adjacent, weight in graph[current_node].items():
            distance = current_distance + weight
            
            if distance < distances[adjacent]:
                distances[adjacent] = distance
                heapq.heappush(queue, [distance, adjacent])
                
    return distances

```

### [ 5 ] 시간복잡도

- 위 다익스트라 알고리즘은 크게 다음 두 가지 과정을 거침
  - 과정1: 각 노드마다 인접한 간선들을 모두 검사하는 과정
  - 과정2: 우선순위 큐에 노드/거리 정보를 넣고 삭제(pop)하는 과정
  
- 각 과정별 시간 복잡도
  - 과정1: 각 노드는 최대 한 번씩 방문하므로 (첫 노드와 해당 노드간의 갈 수 있는 루트가 있는 경우만 해당), 그래프의 모든 간선은 최대 한 번씩 검사
    - 즉, 각 노드마다 인접한 간선들을 모두 검사하는 과정은 O(E) 시간이 걸림, E 는 간선(edge)의 약자

  - 과정2: 우선순위 큐에 가장 많은 노드, 거리 정보가 들어가는 경우, 우선순위 큐에 노드/거리 정보를 넣고, 삭제하는 과정이 최악의 시간이 걸림
    - 우선순위 큐에 가장 많은 노드, 거리 정보가 들어가는 시나리오는 그래프의 모든 간선이 검사될 때마다, 배열의 최단 거리가 갱신되고, 우선순위 큐에 노드/거리가 추가되는 것임
    - 이 때 추가는 각 간선마다 최대 한 번 일어날 수 있으므로, 최대 O(E)의 시간이 걸리고, O(E) 개의 노드/거리 정보에 대해 우선순위 큐를 유지하는 작업은 $ O(log{E}) $ 가 걸림
      - 따라서 해당 과정의 시간 복잡도는 $ O(Elog{E}) $ 
    
### 총 시간 복잡도
  - 과정1 + 과정2 = O(E) + $ O(Elog{E}) $  = $ O(E + Elog{E}) = O(Elog{E}) $
