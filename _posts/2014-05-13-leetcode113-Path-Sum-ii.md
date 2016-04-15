---
layout: post
title: "[LeetCode 113] Path Sum II"
date: 2014-05-13 22:46
description: Solution of LeetCode Question 113
categories: [Programming]
tags: [leetcode, binary tree]
---

Question Link:

<https://leetcode.com/problems/path-sum-ii/>{: target="_blank"}

---

The basic idea here is same to that of
[Path Sum](/2014/05/13/leetcode112-Path-Sum/).
However, since the problem is asking for all possible root-to-leaf paths,
so we should use BFS but not DFS.

The python code is as follows.

<div class="code-title">
<span class="code-fold" id="fold-btn-python" onclick="$use('fold-body-python', 'fold-btn-python')">[-]</span>
Python code accepted by LeetCode OJ
</div>

~~~ python
# Definition for a  binary tree node
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution:
    # @param root, a tree node
    # @param sum, an integer
    # @return a boolean
    def pathSum(self, root, sum):
        """
        BFS from the root to leaves.
        For each leaf, check the path sum with the target sum
        """
        if not root:
            return []
        q = [([root],0)]
        res = []
        while q:
            new_q = []
            for path, s in q:
                n = path[-1]
                s += n.val
                # If path is a root-to-leaf path
                if n.left == n.right == None:
                    if sum == s:
                        res.append([p.val for p in path])
                else:  # Continue BFS
                    if n.left:
                        new_q.append((path+[n.left], s))
                    if n.right:
                        new_q.append((path+[n.right],s))
            q = new_q
        return res
~~~
{: #fold-body-python}
