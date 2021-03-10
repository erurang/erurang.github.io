---
layout: post
title:  "이진 트리의 최대 깊이(maximum-depth-of-binary-tree)"
subtitle: "이진 트리의 최대 깊이(maximum-depth-of-binary-tree)"
categories: boj
tags: Tree
comments: true

---
![이진 트리의 최대 깊이(maximum-depth-of-binary-tree)](https://leetcode.com/problems/maximum-depth-of-binary-tree/)

```
class Solution:
    def maxDepth(self,root: TreeNode) -> int:
        if root is None:
            return 0
        
        def level(node):

            q = [node]
            depth = 0

            while q:
                depth += 1
                
                for _ in range(len(q)):
                    node = q.pop(0)

                    if node.left:
                        q.append(node.left)
                    
                    if node.right:
                        q.append(node.right)
            return depth
```

이 문제는 바이너리트리의 높이를 묻는건데. 레벨순회 방식을 이용해서 한번씩 루프가 들어갈때마다

깊이를 +1 씩 해서 총 루트의 높이를 알수있다.