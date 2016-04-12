---
layout: post
title: "[LeetCode 104] Maximum Depth of Binary Tree"
date: 2014-05-27 04:30
description: Solution of LeetCode Question 104
categories: [Programming]
tags: [leetcode, binary tree]
---

Question Link:

<https://leetcode.com/problems/maximum-depth-of-binary-tree/>{: target="_blank"}

---

Simply BFS from root and count the number of levels. The code is as follows.

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
    # @return an integer
    def maxDepth(self, root):
        """
        Use BFS from root, and count the steps
        """
        if not root:
            return 0
        count = 0
        q = [root]
        while q:
            count += 1
            new_q = []
            for node in q:
                if node.left:
                    new_q.append(node.left)
                if node.right:
                    new_q.append(node.right)
            q = new_q
        return count
~~~
{: #fold-body-python}
