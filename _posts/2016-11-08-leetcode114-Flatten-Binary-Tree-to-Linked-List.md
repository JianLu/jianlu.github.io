---
layout: post
title: "[LeetCode 114] Flatten Binary Tree to Linked List"
date: 2016-11-08 12:49:27
description: Solution of Leetcode Question 114
categories: [Programming]
tags: [leetcode, binary tree]
---

Question Link:

<https://leetcode.com/problems/flatten-binary-tree-to-linked-list/>{: target="_blank"}

---

## Dynamic Programming

Traverse the binary tree pre-orderly, and keep track the last visit node.
For each visit node, link the last visit node with the current node (set left to NULL and right to the current node).

## Python implementation

The python code is as follows. The code is based on the solution of
[[LeetCode 144] Binary Tree Preorder Traversal](/2014/04/04/leetcode144-Binary-Tree-Preorder-Traversal/){:target="_blank"}.

<div class="code-title">
<span class="code-fold" id="fold-btn-python" onclick="$use('fold-body-python', 'fold-btn-python')">[-]</span>
Python code accepted by LeetCode OJ
</div>

~~~ python
# Definition for a binary tree node.
# class TreeNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution(object):
    def flatten(self, root):
        """
        :type root: TreeNode
        :rtype: void Do not return anything, modify root in-place instead.
        """

        if root is None:
            return root

        # Stack for pre-order traverse
        S = [root]

        # Previous visited node
        prev_node = TreeNode(None)

        # Preorder traverse
        while S:
            N = S.pop()

            # Visit
            prev_node.left = None
            prev_node.right = N

            # Now N is the previously visited node
            prev_node = N

            # Push right which will be visited later
            if N.right:
                S.append(N.right)
            # Push left which will be visited earlier
            if N.left:
                S.append(N.left)

        # Set the last node
        prev_node.left = None
        prev_node.right = None

~~~
{: #fold-body-python}

## Extend Reading

There is another post [Convert a Binary Tree to a Circular Doubly Link List](/2016/11/08/BST_to_CDLL/){:target="_blank"},
which is a more complicated problem.
