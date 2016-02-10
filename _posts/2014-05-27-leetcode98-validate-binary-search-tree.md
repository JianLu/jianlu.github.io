---
layout: post
title: "[LeetCode] Validate Binary Search Tree"
date: 2014-05-27 05:59
description: Solution of Leetcode Question 98
categories: [Programming]
tags: [leetcode, binary search tree]
---

Question Link:

<a href="https://oj.leetcode.com/problems/validate-binary-search-tree/" target="_blank">
[https://oj.leetcode.com/problems/validate-binary-search-tree/]</a>

---

Inorder-traverse the tree. For each node, check if `current_node.val > prev_node.val`.

The impelmentation in Python is as follows

~~~ python
# Definition for a  binary tree node
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution:
    # @param root, a tree node
    # @return a boolean
    def isValidBST(self, root):
        stack = []
        prev_node = None
        p = root
        while stack or p:
            if p:
                stack.append(p)
                p = p.left
            else:
                p = stack.pop()
                if prev_node:
                    if prev_node.val >= p.val:
                        return False
                prev_node = p
                p = p.right
        return True
~~~

