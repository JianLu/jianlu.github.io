---
layout: post
title: "[LeetCode 257] Binary Tree Paths"
date: 2016-11-07 15:40:31
description: Solution of Leetcode Question 257
categories: [Programming]
tags: [leetcode, binary tree]
---

Question Link:

<https://leetcode.com/problems/binary-tree-paths/>{: target="_blank"}

---

## Solution

Just implement a DFS tree traverse algorithm.

## Python implementation

The python code is as follows.

<div class="code-title">
<span class="code-fold" id="fold-btn-python" onclick="$use('fold-body-python', 'fold-btn-python')">[-]</span>
Python code accepted by LeetCode OJ
</div>

~~~ python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution:
    # @param {TreeNode} root
    # @return {string[]}
    def binaryTreePaths(self, root):
        """
        Use DFS to traverse the tree
        """
        if root is None:
            return []

        res = []
        path = [root]
        prev_node = TreeNode(None)

        while path:
            N = path[-1]
            # Check if N is Leaf
            if N.left is None and N.right is None:
                print(path)
                res.append("->".join([str(x.val) for x in path]))
                prev_node = path.pop()
            else:
                # N is not the leaf, we have three possible options:
                # 1) Go left, if N has a left child and left child is not visited
                # 2) Go right, if N has a right child and (left child is visited or None)
                # 3) Go up, if right (as well as left) child is already visited or None
                if N.right == prev_node:
                    prev_node = path.pop()
                elif N.left == prev_node:
                    if N.right is None:
                        prev_node = path.pop()
                    else:
                        path.append(N.right)
                elif N.left is not None:
                    path.append(N.left)
                elif N.right is not None:
                    path.append(N.right)

        return res

~~~
{: #fold-body-python}
