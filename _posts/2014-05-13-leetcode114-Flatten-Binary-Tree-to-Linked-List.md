---
layout: post
title: "[LeetCode 114] Flatten Binary Tree to Linked List"
date: 2014-05-13 22:19
description: Solution of LeetCode Question 114
categories: [Programming]
tags: [leetcode, binary tree]
---

Question Link:

<https://leetcode.com/problems/flatten-binary-tree-to-linked-list/>{: target="_blank"}

---

The problem is asking for flattening a binary tree to a linked list by the pre-order,
We can flatten tree from the root recursively:
For each node, we link it with its next child in the pre-order,
and leave the other child and flatten it in the same way.
We can do this by using a stack, the python code is as follows.

<div class="code-title">
<span class="code-fold" id="fold-btn-python" onclick="$use('fold-body-python', 'fold-btn-python')">[-]</span>
Python code accepted by LeetCode OJ
</div>

~~~ python
class Solution:
    # @param root, a tree node
    # @return nothing, do it in place
    def flatten(self, root):
        """
        Traverse the tree in pre-order,
        for each node, we link it and its previous node.
        """
        # Special case
        if not root:
            return
        # Previous node
        prev = root
        # Unlinked node in stack
        stack = []
        # Check root's children
        if root.right:
            stack.append(root.right)
        if root.left:
            stack.append(root.left)
        # Link all nodes in the tree
        while stack:
            # Get the next pre-order node
            next_right = stack.pop()
            # Flatten the previous node
            prev.right = next_right
            prev.left = None
            # Go to next pre_order node, and flatten its sub-tree
            prev = next_right
            # Check its sub-tree
            if prev.right:
                stack.append(prev.right)
            if prev.left:
                stack.append(prev.left)
~~~
{: #fold-body-python}
