---
layout: post
title:  "가장 긴 중복된 노드 길이(longest-univalue-path)"
subtitle: "가장 긴 중복된 노드 길이(longest-univalue-path)"
categories: boj
tags: Tree
comments: true

---
![가장 긴 중복된 노드 길이(longest-univalue-path)](https://leetcode.com/problems/longest-univalue-path)

```
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right

class Solution:
    
    def longestUnivaluePath(self, root: TreeNode) -> int:
        self.ans = 0
        
        def sol(node):
            if node is None:
                return 0
            
            
            left = sol(node.left)
            right = sol(node.right)
            
            
            if node.left and node.val == node.left.val:
                left +=1
            else:
                left = 0 
            
            if node.right and node.val == node.right.val:
                right +=1
            else:
                right = 0 
            
            self.ans = max(self.ans,left+right)
            # 칸을 올라갈때 우리는 틀린값을 기억할 필ㅇ가없음. 그냥 최대값만 계속 쥐고가면됨
            # 그러니까 max(left,right)중에 하나만 큰값으로 가져감
            
            return max(left,right)
        
        sol(root)
        
        return self.ans
        
```

![image](https://user-images.githubusercontent.com/56789064/112347894-9db4e700-8d0a-11eb-9baf-25ab532fa613.png)

