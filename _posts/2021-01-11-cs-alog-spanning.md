---
layout: post
title:  "최소 신장 트리(크루스칼 프림 알고리즘)"
subtitle: "최소 신장 트리(크루스칼 프림 알고리즘)"
categories: cs
tags: algorithm
comments: true

---

## 신장트리란?

- Spanning Tree 라고도 불린다.
- 그래프의 모든 노드가 연결되어있으면서 트리의 속성을 만족하는 그래프
- 신장트리의 조건
    - 그래프의 모든 노드와 연결되야함
    - 트리의 속성을 만족함 (사이클이 존재하지않음)

## 최소신장트리란?

- 모든그래프를 연결한다
- 각 노드와 연결된 간선의 가중치를 본다
- 가중치를 최소화 하면서 모든그래프가 연결된다
- 크루스칼 알고리즘 과 프림알고리즘 이 존재한다

## 크루스칼 알고리즘

1. 가장 짧은 가중치를 찾아서 먼저 연결하기때문에 일단 모든 가중치를 Sort를 한다.
2. 노드끼리 연결할때 싸이클이 생성되는지 매번 체크를 한다.
3. 사이클이 있는지 없는지 어떻게 체크할까?
    1. Find 가중치가 쏘팅된 노드들의 각 부모를 찾는다
    2. Union 부모가 같은 경우는 싸이클이있으니 합치지 않고 다르면 싸이클이 없다는 뜻이니 합쳐준다.

### 전체코드

```
graph = {
    'node': ['A', 'B', 'C', 'D', 'E', 'F', 'G'],
    'weight': [
        (7, 'A', 'B'),
        (5, 'A', 'D'),
        (7, 'B', 'A'),
        (8, 'B', 'C'),
        (9, 'B', 'D'),
        (7, 'B', 'E'),
        (8, 'C', 'B'),
        (5, 'C', 'E'),
        (5, 'D', 'A'),
        (9, 'D', 'B'),
        (7, 'D', 'E'),
        (6, 'D', 'F'),
        (7, 'E', 'B'),
        (5, 'E', 'C'),
        (7, 'E', 'D'),
        (8, 'E', 'F'),
        (9, 'E', 'G'),
        (6, 'F', 'D'),
        (8, 'F', 'E'),
        (11, 'F', 'G'),
        (9, 'G', 'E'),
        (11, 'G', 'F')
    ]
}
```
1. sort를 이용하기 위해 각 가중치들을 맨 앞에두어서 최소값을 먼저 찾는다

```

# 부모노드를 기록할 딕셔너리
parent = {}

# 랭크를 기록할 딕셔너리
rank = {}

# 부모를 찾는 Find
def find(node):
    # 만약 부모가 다르다면
    if parent[node] != node:
        # parent[node] 의 부모를 계속 찾아나간다
        parent[node] = find(parent[node])

    # 찾아온 parent[node] 를 리턴한다
    return parent[node]


def union(node_v,node_u):
    
    # 각 부모의 노드 값을 가져와서
    root1 = find(node_v)
    root2 = find(node_u)

    # 그 부모의 랭크를 확인하고 
    if rank[root1] > rank[root2]:
        parent[root2] = root1
    else:
        parent[root1] = root2
    
        if rank[root1] == rank[root2]:
            rank[root2] += 1


def make_set(node):
    # 부모노드가 자기 자신으로 초기화를 시켜줌
    parent[node] = node
    # 아무것도 연결된게 없으니 0으로 초기화
    rank[node] = 0


def kruskal(graph):
    # 싸이클이 없을때의 노드
    mst = []
    
    
    # 초기화
    for node in graph[node]:
        make_set(node)

    # 가중치로 쏘팅
    weight = graph['weight']
    weigt.sort()


    # 가중치가 낮은 노드들을 하나하나 뽑으면서
    for weights in weight:
        weight, node_v, node_u = weights

        # 각 뽑아온 각 노드의 부모가 다른지 체크를 하고
        # 다르다는것은 싸이클이 존재하지 않는다는것
        if find(node_v) != find(node_u):
            # 합쳐준다
            union(node_v,node_u)
            mst.append(weights)
    
    return mst
```