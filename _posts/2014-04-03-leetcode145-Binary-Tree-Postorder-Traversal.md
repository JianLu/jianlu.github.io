---
layout: post
title: "[LeetCode 145] Binary Tree Postorder Traversal"
date: 2014-04-03 23:36
description: Solution of Leetcode Question 145
categories: [Programming]
tags: [leetcode, binary tree]
---

Question Link:

<https://leetcode.com/problems/binary-tree-postorder-traversal/>{: target="_blank"}

---

Trees can be traversed in
[pre-order](/2014/04/04/leetcode144-Binary-Tree-Preorder-Traversal/),
[in-order](/2014/04/04/leetcode94-Binary-Tree-Inorder-Traversal/),
or [post-order](/2014/04/03/leetcode145-Binary-Tree-Postorder-Traversal/).
These searches are referred to as depth-first search (DFS),
as the search tree is deepened as much as possible on each child before going to the next sibling.

## Post-Order Traversal

1. Traverse the left subtree by recursively calling the pre-order function.
2. Traverse the right subtree by recursively calling the pre-order function.
3. Visit the tree node (root or current node) and do something.

## Recursive Solution

To solve this problem with a recursive function,
we define a helper function which visit the node recursively with the post-order traversal described above.

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
    def postorderTraversal(self, root):
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
        Recursive postorder traversal method
        """
        # Traverse left child first
        if node.left:
            self.helper(node.left)
        # Then, traverse right child
        if node.right:
            self.helper(node.right)
        # Visit the node at last
        self.rtn.append(node.val)
~~~
{: #fold-body-python-recursive}

## Iterative Solution

We need a **stack** data structure for storing the nodes not visited, in the post-order traversal on a binary tree.

The post-order iterative traversal is the most hard one of the three different DFS traversal.
Because:

1. A child node cannot be put into stack before its parent.
This means, we can ONLY put a node while we are going down the tree, and we can ONLY pop a node while we are going up the tree.
2. For each iteration, we need to know we are going down or up.

In practice, I use two pointers to help to know the direction:

1. `go_down`: if go_down is NULL, it is "GoUp" case which means we are going up from the child to parent;
otherwise, it is "GoDown" case that we are visiting the node from its parent.
2. `prev_visit`: denotes the last visited node in go_up case, which can help us to identify if we just visit the left or right child of the current node.

The algorithm is like:

1. Let `S` be a empty stack, `go_down` be the root of the tree and `prev_visit` be NULL.
2. Do the following until `S` is empty and `go_down` is NULL

    1. For the case "GoDown" (`go_down` is not NULL), continue go down to the left subtree. Push `go_down` into `S` and let `go_down` be its left child.
    2. Otherwise, it is "GoUp" case:

        1. Let `parent` be the top of `S`
        2. If `parent` does not have right child or we came up from its right child, we continue go up (update `prev_visit` to `parent`)
        3. Otherwise, we traverse `parent`'s right child in "GoDown" mode (set `go_down` to the right child)

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
    def postorderTraversal(self, root):
        """
        :type root: TreeNode
        :rtype: List[int]
        """
        rtn = []

        S = []
        prev_visit = None
        go_down = root

        while S or go_down:
            if go_down:
                S.append(go_down)
                # Traverse left child first
                go_down = go_down.left
            else:
                # Cannot go left further, then go back up to traverse parent's right child
                parent = S[-1] # Do not pop this time
                if parent.right is None or parent.right == prev_visit:
                    # Subtree of parent has been traversed, keep going up
                    # Update the previous visited node
                    prev_visit = S.pop()
                    # Visit node at last
                    rtn.append(prev_visit.val)
                else:
                    # Parent's right subtree is not traversed yet
                    # Traverse the right child then
                    go_down = parent.right

        return rtn
~~~
{: #fold-body-python-iterative}
