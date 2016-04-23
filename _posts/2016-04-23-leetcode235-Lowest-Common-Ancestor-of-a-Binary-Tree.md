---
layout: post
title: "[LeetCode 236] Lowest Common Ancestor of a Binary Tree"
date: 2016-04-23 13:06
description: Solution of Leetcode Question 236
categories: [Programming]
tags: [leetcode, binary tree]
---

Question Link:

<https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-tree/>{: target="_blank"}

---

> 哈哈哈，好久没有90%以上的beat率了，先show off一下，虽然这个beat率很random……

![showoff](/images/showoff_20160423.PNG)

---

The solution of this problem is based on commonly-used techniques of DFS (depth-first search) on a binary tree.

There two steps to solve this problem:

1. Traverse the binary tree from the root to find the node `P` or `Q`, and record the path from the root to the node.
2. Let's say we found node `P` first, then we scan the nodes on the path from the last node to the root
 to check if any node is the ancestor of node `Q`, the first ancestor must be the lowest common ancestor of `Q` and `P`.

## Find first node and the path from root

To find the first node and the path from the root to this node,
we use a DFS method which is very similar to the iterative method of
[post-order traversal](/2014/04/03/leetcode145-Binary-Tree-Postorder-Traversal/).
Without loss of generality, we assume we find `P` and the path is `path[1..m]`
where `path[1]` is the root of the tree and `path[m]` is `P`.

## Find the common ancestor from the path node

Then, we only need to find the first node from `path[m]` to `path[1]` which is also the ancestor of the other node `Q`.
We scan the path in the reverse order, and for each node `path[i]`:

1. At least we do not need to check one of the subtree of its child `path[i+1]`, because we already checked it in the previous iteration.
2. If `path[i+1]` is the right child of `path[i]`, we do not need to check the subtree of `path[i]`'s left child either.
Because we already tried to find `P` or `Q` in this subtree in the first step.

To check if a node is the ancestor of `Q`, we use a recursive function `search` to check if a node `node` is in the subtree whose root is `root`.

## Time complexity

In the worst case, the time complexity of the algorithm is `O(n)` because each node is only visited once except that
the nodes on the path are visited twice.

## Python implementation

The python code is as follows.

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
    def lowestCommonAncestor(self, root, p, q):
        """
        :type root: TreeNode
        :type p: TreeNode
        :type q: TreeNode
        :rtype: TreeNode
        Use a "parent stack" to track the path from the root to the current node.
        The traversal logic is very similar to iterative PostOrderTraversal.
        But the visit logic is PreOrder, each time we check if the current node is p or q
        """

        # Special case 1: root is p or q
        if root == p or root == q:
            return root

        # Special case 2: p and q are the same node on the tree
        if p == q:
            return p

        # A preorder traversal with a stack to remember the path to the current node
        path = []
        go_down = root
        pre_visit = None

        while path or go_down is not None:
            if go_down is not None:
                path.append(go_down)
                if go_down == q:
                    return self.findLowestAncestorInPath(path, p)
                if go_down == p:
                    return self.findLowestAncestorInPath(path, q)
                # Neither p or q, conitnue go down
                go_down = go_down.left
            else:
                # go_down is null means we touch the leaf and need to go up
                # Get go_down's parent
                parent = path[-1]
                # Need to check parent's right child, we nned to continue go up if:
                # 1) parent has no right child
                # 2) We already traversed parent's right child
                if parent.right is None or parent.right == pre_visit:
                    # Conitnue go up (pop parent from path), and update pre_visit
                    pre_visit = path.pop()
                else:
                    # Continue go down from parent.right
                    go_down = parent.right

        # Not found p or q
        return None


    def findLowestAncestorInPath(self, path, node):
        """
        path is the path from root to one of two nodes (p and q), node is the other node
        This function will find the lowest ancestor of node on the path
        """
        n = len(path)
        if self.search(path[n-1], node):
            return path[n-1]

        # We don't need to check the node on the path, because there are neither p nor q
        n = n - 2
        while n >= 0:
            # If path[n+1] is the left child of path[n], the need to check path[n]'s right child
            # Otherwise continue, because path[n]'s children are both checked
            if path[n+1] == path[n].left and self.search(path[n].right, node):
                return path[n]
            n -= 1

        # Not found
        return None

    def search(self, root, node):
        """
        Recursive function to search a node in a subtree
        """
        if root is None:
            return False
        if root == node:
            return True
        return self.search(root.left, node) or self.search(root.right, node)

~~~
{: #fold-body-python}
