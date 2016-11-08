---
layout: post
title: "Convert a Binary Tree to a Circular Doubly Link List"
date: 2016-11-08 13:09:34
description: Convert a binary tree to doubly link list in-place
categories: [Programming]
tags: [leetcode, binary tree]
---

Given a Binary Tree, convert it to a Circular Doubly Linked List (In-Place):

* The left and right pointers in nodes are to be used as previous and next pointers respectively in converted Circular Linked List.
* The order of nodes in List must be same as Inorder of the given Binary Tree.
* The first node of Inorder traversal must be head node of the Circular List.

---

## Solution

By following the framework of
[[LeetCode 144] Binary Tree Preorder Traversal](/2014/04/04/leetcode144-Binary-Tree-Preorder-Traversal/){:target="_blank"},
we can solve the problem by modifying the visit operations.

Moreover, we need to link the last node (last prev_visit node) and the first node (head.right) in the final step.


## Python implementation

The python code is as follows.

<div class="code-title">
<span class="code-fold" id="fold-btn-python" onclick="$use('fold-body-python', 'fold-btn-python')">[-]</span>
Python code accepted by LeetCode OJ
</div>

~~~ python
# A Binary Tree node
class MyNode:
    # Constructor to create a new tree node
    def __init__(self, x):
        self.val = x
        self.left = None
        self.right = None

    def to_str(self):
        l = "x"
        r = "x"
        if self.left is not None:
            l = self.left.val
        if self.right is not None:
            r = self.right.val
        return "[%s | %s | %s]" % (l, self.val, r)

class BT2DLL:
    @staticmethod
    def sample():
        root = MyNode(10)
        root.left = MyNode(5)
        root.right = MyNode(15)
        root.left.left = MyNode(1)
        root.left.right = MyNode(6)
        root.left.right.right = MyNode(8)
        root.right.left = MyNode(12)
        root.right.right = MyNode(20)
        return root

    def inorder(self, root):
        """
        :type root: MyNode object
        """
        if root is not None:
            self.inorder(root.left)
            print("%d" % root.val, end=" ")
            self.inorder(root.right)

    def bt2dll(self, root):
        # DDL head and cursor
        head = MyNode(None)

        # Prev node and stack for inorder tree traverse
        prev_visit = head
        S = []
        while root or S:
            if root:
                # Suppose to be visited after traverse its left subtree
                S.append(root)
                # Go deeper to left child
                root = root.left
            else:
                # Go up and visit the parent node
                root = S.pop()
                # Visit the node: link it with previous node
                prev_visit.right = root
                root.left = prev_visit
                prev_visit = root
                # Go down to the right
                root = root.right

        # Link the first and last nodes
        first = head.right
        first.left = prev_visit
        prev_visit.right = first

        return head

    @staticmethod
    def print_ddl(HEAD):
        first = HEAD.right
        print(first.to_str(), end=" ")
        p = first.right
        while p and p != first:
            print(p.to_str(), end=" ")
            p = p.right


if __name__ == "__main__":
    bt = BT2DLL()
    ROOT = bt.sample()
    bt.inorder(ROOT)

    print()

    h = bt.bt2dll(ROOT)
    bt.print_ddl(h)
~~~
{: #fold-body-python}
