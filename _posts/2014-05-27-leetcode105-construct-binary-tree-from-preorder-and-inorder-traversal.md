---
layout: post
title: "[LeetCode 105] Construct Binary Tree from Preorder and Inorder Traversal"
date: 2014-05-27 04:26
description: Solution of LeetCode Question 105
categories: [Programming]
tags: [leetcode, binary tree]
---

Question Link:

<https://leetcode.com/problems/construct-binary-tree-from-preorder-and-inorder-traversal/>{: target="_blank"}

---

The basic idea is same to that of [Construct Binary Tree from Inorder and Postorder Traversal](/2014/05/27/leetcode106-construct-binary-tree-from-inorder-and-postorder-traversal/).
We solve it using a recursive function.
First, we find `preorder[0]` in inorder, let's say `inorder[i] == preorder`,
then construct the left tree from `preorder[1..i]` and `inorder[0..i-1]`
and the right tree from `preorder[i+1..n-1]` and `inorder[i+1..n-1]`.

The code is as follows.

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
    # @param preorder, a list of integers
    # @param inorder, a list of integers
    # @return a tree node
    def buildTree(self, preorder, inorder):
        n = len(preorder)
        if n == 0:
            return None
        elif n == 1:
            return TreeNode(preorder[0])
        else:
            mid_inorder = inorder.index(preorder[0])
            root = TreeNode(preorder[0])
            root.left = self.buildTree(preorder[1:mid_inorder+1], inorder[:mid_inorder])
            root.right = self.buildTree(preorder[mid_inorder+1:], inorder[mid_inorder+1:])
            return root
~~~
{: #fold-body-python}
