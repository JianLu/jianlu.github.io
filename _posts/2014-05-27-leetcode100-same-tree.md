---
layout: post
title: "[LeetCode 100] Same Tree"
date: 2014-05-27 05:09
description: Solution of LeetCode Question 100
categories: [Programming]
tags: [leetcode]
---

Question Link:

<https://leetcode.com/problems/same-tree/>{: target="_blank"}

---

No comments... straightforward to implement.
The following recursive version is accepted but the iterative version is not...

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
    # @param p, a tree node
    # @param q, a tree node
    # @return a boolean
    def isSameTree(self, p, q):
        """
        Check both trees level by level using BFS
        """
        if not p or not q:
            return p == q
        if p.val == q.val:
            return self.isSameTree(p.left, q.left) and self.isSameTree(p.right, q.right)
        else:
            return False
~~~
{: #fold-body-python}
