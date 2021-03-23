---
layout: post
title:  "이진 트리의 직경(diameter-of-binary-tree)"
subtitle: "이진 트리의 직경(diameter-of-binary-tree)"
categories: boj
tags: Tree
comments: true

---
![이진 트리의 직경(diameter-of-binary-tree)](https://leetcode.com/problems/diameter-of-binary-tree/)

이 문제를 이해하려고 이틀을 매달렸다.

드디어 이해하고 내 제출코드를 적는다

```
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right

class Solution:
    # 아래서부터 거리를 갱신해나감
    res = 0
    def diameterOfBinaryTree(self, root: TreeNode) -> int:    
        
        def dfs(node):
            if node is None:
                return 0
            else:    
                left = dfs(node.left)
                right = dfs(node.right)
                
                self.res = max(self.res,left+right)
                
                return max(left,right)+1
        
        dfs(root)
        return self.res
```

일단 가장긴거리? 에서 우리는 이렇게 생각할수있어야한다.

왼쪽 서브트리에서의 리프노드와 오른쪽 서브트리의 리프노드의 합이 가장 긴 거리가 될것이란것.

res에는 가장 긴 거리를 축적해야한다. 예르들어서
```
                이전레벨 노드
왼쪽에서 가장 끝까지 온 노드와 오른쪽 노드에서 가장 끝까지 온 노드
```
라고 할때 이전레벨노드에서 가장 긴 거리는 왼쪽노드에서 가장 긴거리와 + 오른쪽노드에서 가장 긴 거리를 더한값이 될것이다

그래서 self.res = max(self.res,left+right) 를 한값을 매번 갱신시키면서 최장길이를 찾는다.

