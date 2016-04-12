---
layout: post
title: "[LeetCode 103] Binary Tree Zigzag Level Order Traversal"
date: 2014-05-27 04:36
description: Solution of LeetCode Question 103
categories: [Programming]
tags: [leetcode, binary tree]
---

Question Link:

<https://leetcode.com/problems/binary-tree-zigzag-level-order-traversal/>{: target="_blank"}

---

Just BFS from the root and for each level insert a list of values into the result.

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
    # @return a list of lists of integers
    def zigzagLevelOrder(self, root):
        """
        BFS from the root.
        For each level, insert a list of values into the result
        """
        res = []
        if not root:
            return res
        q = [root]
        reverse = False
        while q:
            new_q = []
            if reverse:
                res.append([n.val for n in q][::-1])
            else:
                res.append([n.val for n in q])
            reverse = not reverse
            for node in q:
                if node.left:
                    new_q.append(node.left)
                if node.right:
                    new_q.append(node.right)
            q = new_q
        return res
~~~
{: #fold-body-python}
