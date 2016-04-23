---
layout: post
title: "[LeetCode 94] Binary Tree Inorder Traversal"
date: 2014-04-04 01:03
description: Solution of Leetcode Question 94
categories: [Programming]
tags: [leetcode, binary tree]
---

Question Link:

<https://leetcode.com/problems/binary-tree-inorder-traversal/>{: target="_blank"}

---

Trees can be traversed in
[pre-order](/2014/04/04/leetcode144-Binary-Tree-Preorder-Traversal/),
[in-order](/2014/04/04/leetcode94-Binary-Tree-Inorder-Traversal/),
or [post-order](/2014/04/03/leetcode145-Binary-Tree-Postorder-Traversal/).
These searches are referred to as depth-first search (DFS),
as the search tree is deepened as much as possible on each child before going to the next sibling.


## In-Order Traversal

1. Traverse the left subtree by recursively calling the in-order function.
2. Visit the tree node (root or current node) and do something.
3. Traverse the right subtree by recursively calling the in-order function.

## Recursive Solution

To solve this problem with a recursive function,
we define a helper function which visit the node recursively with the in-order traversal described above.

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
    def inorderTraversal(self, root):
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
        Recursive inorder traversal method
        """
        # Traverse left child first
        if node.left:
            self.helper(node.left)
        # Then visit the node
        self.rtn.append(node.val)
        # Traverse right child at last
        if node.right:
            self.helper(node.right)
~~~
{: #fold-body-python-recursive}

## Iterative Solution

We need a **stack** data structure for storing the nodes not visited,
in the in-order traversal on a binary tree.
The pseudo-code is like:

1. Let `S`be an empty stack and `current_node` be the root of the tree.
2. Do the following for each `current_node`:
    1) if `current_node` is NULL, then pop a node from `S` and visit the node;
    2) otherwise continue traverse the left child of `current_node` by pushing `current_node` into `S` and
     and setting `current_node` as its left child
3. Terminate when `S` is empty and `current_node` is NULL.

The python code is as follows.

<div class="code-title">
<span class="code-fold" id="fold-btn-python-iterative" onclick="$use('fold-body-python-iterative', 'fold-btn-python-iterative')">[-]</span>
Iterative traversal Python code accepted by LeetCode OJ
</div>

~~~ python
class Solution(object):
    def inorderTraversal(self, root):
        """
        :type root: TreeNode
        :rtype: List[int]
        """
        S = []
        rtn = []
        while S or root:
            if root:
                S.append(root)
                # Traverse left child
                root = root.left
            else:
                root = S.pop()
                # Visit the node now
                rtn.append(root.val)
                # Then, traverse right child
                root = root.right

        return rtn
~~~
{: #fold-body-python-iterative}
