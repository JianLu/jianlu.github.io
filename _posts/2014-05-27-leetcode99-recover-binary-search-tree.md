---
layout: post
title: "[LeetCode 99] Recover Binary Search Tree"
date: 2014-05-27 05:52
description: Solution of LeetCode Question 99
categories: [Programming]
tags: [leetcode]
---

Question Link:

<https://leetcode.com/problems/recover-binary-search-tree/>{: target="_blank"}

---
We know that the in-order traversal of a binary search tree should be a sorted array.
Therefore, we can compare each node with its previous node in the in-order to find the two swapped elements.

There are at most two cases that `current_node` < `prev_node`:

1. If the two swapped elements are adjacent, then there is only one occurrence that `current_node` < `prev_node`.
The two elements are just the `current_node` and `prev_node`.
2. If the two swapped elements are not neighbors, then there are two occurrences that `current_node` < `prev_node`.
The two elements are the `prev_node` in the first occurrence and the `current_node` in the second occurrence.

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
    # @param root, a tree node
    # @return a tree node
    def recoverTree(self, root):
        """
        Traverse the tree in the order of inorder and compare the node to its previous node.
        There are at most two occurrences that prev_node < current_node, if there are two elements swapped.
        If there is only one occurrence, then the two swapped nodes are prev_node and current_node.
        If there are two occurrences, then the two swapped nodes are prev_node in the first occurrence and current_node in the second occurrence.
        """
        prev_node = None
        current_node = None
        stack = []
        p = root
        res1 = None
        res2 = None
        while stack or p:
            if p:
                stack.append(p)
                p = p.left
            else:
                current_node = stack.pop()
                # Compare current node and previous node
                if prev_node:
                    if prev_node.val > current_node.val:
                        if res1:
                            res2 = current_node
                            break
                        else:
                            res1 = prev_node
                            res2 = current_node
                prev_node = current_node
                p = current_node.right
        res1.val, res2.val = res2.val, res1.val
        return root

~~~
{: #fold-body-python}
