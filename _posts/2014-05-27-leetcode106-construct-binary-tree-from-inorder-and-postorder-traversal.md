---
layout: post
title: "[LeetCode 106] Construct Binary Tree from Inorder and Postorder Traversal"
date: 2014-05-27 04:17
description: Solution of LeetCode Question 106
categories: [Programming]
tags: [leetcode, binary tree]
---

Question Link:

<https://leetcode.com/problems/construct-binary-tree-from-inorder-and-postorder-traversal/>{: target="_blank"}

---

## Divide-and-Conquer

This problem can be easily solved using recursive method.

By given the inorder and postorder lists of the tree, i.e. `inorder[1..n]` and `postorder[1..n]`,
`postorder[n]` should be the root's value, then we find the position of `postorder[n]` in `inorder[1..n]`.
Suppose the position is `i`,
then `postorder[1..i-1]` and `inorder[1..i-1]` are the postorder and inorder lists of root's left tree
and `postorder[i..n-1]` and `inorder[i+1..n]` are the postorder and inorder lists of root's right tree.
So we can construct the tree recursively.

## Python Implement

The code of the recursive function is as follows.

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
    # @param inorder, a list of integers
    # @param postorder, a list of integers
    # @return a tree node
    def buildTree(self, inorder, postorder):
        n = len(inorder)
        if n == 0:
            return None
        elif n == 1:
            return TreeNode(postorder[-1])
        else:
            root = TreeNode(postorder[-1])
            mid_inorder = inorder.index(postorder[-1])
            root.left = self.buildTree(inorder[:mid_inorder], postorder[:mid_inorder])
            root.right = self.buildTree(inorder[mid_inorder+1:], postorder[mid_inorder:-1])
            return root
~~~
{: #fold-body-python}
