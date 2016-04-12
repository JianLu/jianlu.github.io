---
layout: post
title: "[LeetCode 101] Symmetric Tree"
date: 2014-05-27 04:56
description: Solution of LeetCode Question 101
categories: [Programming]
tags: [leetcode]
---

Question Link:

<https://leetcode.com/problems/symmetric-tree/>{: target="_blank"}

---

To solve the problem, we can traverse the tree level by level.
Within each level, we construct an array of values of the length 2^depth, and check if this array is symmetric.
The tree is symmetric only if all constructed arrays are all symmetric.

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
    def isSymmetric(self, root):
        """
        We traverse the tree from the root level by level.
        For each level, we construct a integer array of length 2^depth,
        then we check if the array is symmetric.
        """
        if not root:
            return True
        q = [root]
        next = True
        while next:
            next = False
            new_q = []
            numbers = []
            for node in q:
                if node:
                    numbers.append(node.val)
                    # Left child
                    if node.left:
                        next = True
                        new_q.append(node.left)
                    else:
                        new_q.append(None)
                    # Right child
                    if node.right:
                        next = True
                        new_q.append(node.right)
                    else:
                        new_q.append(None)
                else:
                    numbers.append(0)
            if not self.isSymmetricList(numbers):
                return False
            q = new_q
        return True

    def isSymmetricList(self, lst):
        low = 0
        high = len(lst)-1
        while low < high:
            if lst[low] != lst[high]:
                return False
            low += 1
            high -= 1
        return True

~~~
{: #fold-body-python}
