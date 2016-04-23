---
layout: post
title: "[LeetCode 144] Binary Tree Preorder Traversal"
date: 2014-04-04 02:03
description: Solution of Leetcode Question 144
categories: [Programming]
tags: [leetcode, binary tree]
---

Question Link:

<https://leetcode.com/problems/binary-tree-preorder-traversal/>{: target="_blank"}

---

Trees can be traversed in
[pre-order](/2014/04/04/leetcode144-Binary-Tree-Preorder-Traversal/),
[in-order](/2014/04/04/leetcode94-Binary-Tree-Inorder-Traversal/),
or [post-order](/2014/04/03/leetcode145-Binary-Tree-Postorder-Traversal/).
These searches are referred to as depth-first search (DFS),
as the search tree is deepened as much as possible on each child before going to the next sibling.

## Pre-Order Traversal

1. Visit the tree node (root or current node) and do something.
2. Traverse the left subtree by recursively calling the pre-order function.
3. Traverse the right subtree by recursively calling the pre-order function.

## Recursive Solution

To solve this problem with a recursive function,
we define a helper function which visit the node recursively with the pre-order traversal described above.

The python code is as follows.

<div class="code-title">
<span class="code-fold" id="fold-btn-python-recursive" onclick="$use('fold-body-python-recursive', 'fold-btn-python-recursive')">[-]</span>
Recursive traversal Python code accepted by LeetCode OJ
</div>

~~~ python
# Definition for a binary tree node.
# class TreeNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution(object):
    def preorderTraversal(self, root):
        """
        :type root: TreeNode
        :rtype: List[int]
        """
        if root is None:
            return []

        self.rtn = []
        self.helper(root)
        return self.rtn

    def helper(self, node):
        """
        Recursive preorder traversal method
        """
        # Visit the node first
        self.rtn.append(node.val)
        # Then traverse left child
        if node.left:
            self.helper(node.left)
        # Traverse right child at last
        if node.right:
            self.helper(node.right)
~~~
{: #fold-body-python-recursive}

## Iterative Solution

We need a **stack** data structure for storing the nodes not visited,
in the in-order traversal on a binary tree.
The pseudo-code is like:

1. Let `S`be an stack initially containing the root of the tree.
2. Do the following until `S` is empty
    1) Let `N = S.pop()` and visit `N`.
    2) If `N` has right child, then push `N`'s right child into `S`
    3) If `N` has left child, then push `N`'s left child into `S`

The python code is as follows.

<div class="code-title">
<span class="code-fold" id="fold-btn-python-iterative" onclick="$use('fold-body-python-iterative', 'fold-btn-python-iterative')">[-]</span>
Iterative traversal Python code accepted by LeetCode OJ
</div>

~~~ python
# Definition for a binary tree node.
# class TreeNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution(object):
    def preorderTraversal(self, root):
        """
        :type root: TreeNode
        :rtype: List[int]
        """
        if root is None:
            return []

        S = [root]
        rtn = []
        while S:
            N = S.pop()
            # Visit the node first
            rtn.append(N.val)
            # Push right child first which will be traversed last
            if N.right:
                S.append(N.right)
            # Then push left child which will be traverse earlier
            if N.left:
                S.append(N.left)

        return rtn
~~~
{: #fold-body-python-iterative}
