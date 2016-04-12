---
layout: post
title: "[LeetCode 107] Binary Tree Level Order Traversal II"
date: 2014-05-27 02:22
description: Solution of LeetCode Question 107
categories: [Programming]
tags: [leetcode, binary tree]
---

Question Link:

<https://leetcode.com/problems/binary-tree-level-order-traversal-ii/>{: target="_blank"}

---

Use BFS from the tree root to traverse the tree level by level. The python code is as follows.

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
    def levelOrderBottom(self, root):
        """
        BFS from root, and record the values level by level
        """
        # List of levels
        res = []
        # Special case: empty tree
        if not root:
            return res
        level = [root]
        while level:
            temp = []
            res.insert(0, [n.val for n in level])
            for node in level:
                if node.left:
                    temp.append(node.left)
                if node.right:
                    temp.append(node.right)
            level = temp
        # Return
        return res
~~~
{: #fold-body-python}
