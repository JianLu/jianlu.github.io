---
layout: post
title: "[LeetCode 173] Binary Search Tree Iterator"
date: 2016-11-08 13:45:43
description: Solution of Leetcode Question 173
categories: [Programming]
tags: [leetcode, binary tree]
---

Question Link:

<https://leetcode.com/problems/binary-search-tree-iterator/>{: target="_blank"}

---

## Solution

This problem sounds complicated, but it is not.
The solution is just divided the algorithm of
[[LeetCode 144] Binary Tree Preorder Traversal](/2014/04/04/leetcode144-Binary-Tree-Preorder-Traversal/){:target="_blank"}
into multiple steps of `hasNext()` and `next()` functions.

We keep the stack which is used in the inorder traverse. We only pop up when the `next()` function is called.

## Python implementation

The python code is as follows.

<div class="code-title">
<span class="code-fold" id="fold-btn-python" onclick="$use('fold-body-python', 'fold-btn-python')">[-]</span>
Python code accepted by LeetCode OJ
</div>

~~~ python
# Definition for a  binary tree node
# class TreeNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class BSTIterator(object):
    def __init__(self, root):
        """
        :type root: TreeNode
        """
        # Create the stack until we found the first node
        self.stack = []
        while root:
            self.stack.append(root)
            root = root.left

    def hasNext(self):
        """
        :rtype: bool
        """
        return len(self.stack) > 0

    def next(self):
        """
        :rtype: int
        """
        # Pop the next node from the stack
        ret = self.stack.pop()

        # Continue the inorder traverse
        N = ret.right
        while N:
            self.stack.append(N)
            N = N.left

        # Return
        return ret.val

# Your BSTIterator will be called like this:
# i, v = BSTIterator(root), []
# while i.hasNext(): v.append(i.next())
~~~
{: #fold-body-python}
