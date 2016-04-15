---
layout: post
title: "[LeetCode 110] Balanced Binary Tree"
date: 2014-05-14 00:19
description: Solution of LeetCode Question 110
categories: [Programming]
tags: [leetcode, binary tree]
---

Question Link:

<https://leetcode.com/problems/balanced-binary-tree/>{: target="_blank"}

---

We use a recursive help function to determine whether a sub-tree is balanced,
if the tree is balanced, it also return the depth of the sub-tree.

A tree `T` is balanced if the following recursive conditions are satisfied:

1. `T`'s left sub-tree is balanced;
2. `T`'s right sub-tree is balanced;
3. The depth of the two sub-trees never differ by more than 1.

The python code is as follows,
where a recursive function check the balance in the way of top-down.

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
    # @return a boolean
    def isBalanced(self, root):
        return self.balanced_and_depth(root)[0]

    def balanced_and_depth(self, node):
        """
        Return [False, 0] if the sub-tree of node is not balanced,
        return [True, depth] otherwise
        """
        if not node:
            return True, 0
        lb, ld = self.balanced_and_depth(node.left)
        if not lb:
            return False, 0
        rb, rd = self.balanced_and_depth(node.right)
        if not rb:
            return False, 0
        if abs(ld-rd) > 1:
            return False, 0
        return True, 1 + max(ld,rd)
~~~
{: #fold-body-python}
