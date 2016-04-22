---
layout: post
title: "[LeetCode 112] Path Sum"
date: 2014-05-13 22:46
description: Solution of LeetCode Question 112
categories: [Programming]
tags: [leetcode, binary tree]
---

Question Link:

<https://leetcode.com/problems/path-sum/>{: target="_blank"}

---

## BFS Solution

One solution is to BFS the tree from the root,
and for each leaf we check if the path sum equals to the given sum value.

The code is as follows.

<div class="code-title">
<span class="code-fold" id="fold-btn-python1" onclick="$use('fold-body-python1', 'fold-btn-python1')">[-]</span>
BFS Python code accepted by LeetCode OJ
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
    # @param sum, an integer
    # @return a boolean
    def hasPathSum(self, root, sum):
        """
        BFS from the root to leaves.
        For each leaf, check the path sum with the target sum
        """
        if not root:
            return False
        q = [(root,0)]
        while q:
            new_q = []
            for n, s in q:
                s += n.val
                # If n is a leaf, check the path-sum with the given sum
                if n.left == n.right == None:
                    if sum == s:
                        return True
                else:  # Continue BFS
                    if n.left:
                        new_q.append((n.left,s))
                    if n.right:
                        new_q.append((n.right,s))
            q = new_q
        return False
~~~
{: #fold-body-python1}

## DFS Solution

The other solution is to DFS the tree, and do the same check for each leaf node.

<div class="code-title">
<span class="code-fold" id="fold-btn-python2" onclick="$use('fold-body-python2', 'fold-btn-python2')">[-]</span>
DFS Python code accepted by LeetCode OJ
</div>

~~~ python
class Solution:
    # @param root, a tree node
    # @param sum, an integer
    # @return a boolean
    def hasPathSum(self, root, sum):
        """
        DFS from the root to leaves.
        For each leaf, check the path sum with the target sum
        """
        # Special case
        if not root:
            return False
        # Record the current path
        path = [root]
        # The path sum of the current path
        current_sum = root.val
        # Hash set for keeping visited nodes
        visited = set()
        # Start DFS
        while path:
            node = path[-1]
            # Touch the leaf node
            if node.left == node.right == None:
                if sum == current_sum:
                    return True
                current_sum -= node.val
                path.pop()
            else:
                if node.left and node.left not in visited:
                    # Go deeper to the left
                    visited.add(node.left)
                    path.append(node.left)
                    current_sum += node.left.val
                elif node.right and node.right not in visited:
                    # Go deeper to the right
                    visited.add(node.right)
                    path.append(node.right)
                    current_sum += node.right.val
                else:
                    # Go back to the upper level
                    current_sum -= node.val
                    path.pop()
        return False
~~~
{: #fold-body-python2}