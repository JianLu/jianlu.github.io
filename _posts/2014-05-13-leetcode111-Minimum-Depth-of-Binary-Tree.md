---
layout: post
title: "[LeetCode 111] Minimum Depth of Binary Tree"
date: 2014-05-13 22:53
description: Solution of LeetCode Question 111
categories: [Programming]
tags: [leetcode, binary tree]
---

Question Link:

<https://leetcode.com/problems/minimum-depth-of-binary-tree/>{: target="_blank"}

---

To find the minimum depth, we BFS from the root and record the depth.
For each level we add 1 to the depth and return the depth value when we reach a leaf.

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
    # @return an integer
    def minDepth(self, root):
        """
        BFS the tree and record the depth,
        return this value for the first time that a leaf is reached.
        """
        if not root:
            return 0
        depth = 1
        q = [root]
        while q:
            new_q = []
            for n in q:
                if n.left == n.right == None:
                    return depth
                if n.left:
                    new_q.append(n.left)
                if n.right:
                    new_q.append(n.right)
            q = new_q
            depth += 1
~~~
{: #fold-body-python}
