---
layout: post
title: "[LeetCode 108] Convert Sorted Array to Binary Search Tree"
date: 2014-05-22 06:46
description: Solution of LeetCode Question 108
categories: [Programming]
tags: [leetcode, binary search tree]
---

Question Link:

<https://leetcode.com/problems/convert-sorted-array-to-binary-search-tree/>{: target="_blank"}

---

Same idea to
[Convert Sorted List to Binary Search Tree](/2014/05/14/leetcode109-Convert-Sorted-List-to-Binary-Search-Tree/),
but we use a recursive function to construct the binary search tree.

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
    # @param num, a list of integers
    # @return a tree node
    def sortedArrayToBST(self, num):
        n = len(num)
        if n == 0:
            return None
        elif n == 1:
            return TreeNode(num[0])
        mid = n / 2
        root = TreeNode(num[mid])
        root.left = self.sortedArrayToBST(num[:mid])
        root.right = self.sortedArrayToBST(num[mid+1:])
        return root
~~~
{: #fold-body-python}
