---
layout: post
title: "[LeetCode 109] Convert Sorted List to Binary Search Tree"
date: 2014-05-14 01:06
description: Solution of LeetCode Question 109
categories: [Programming]
tags: [leetcode, binary search tree]
---

Question Link:

<https://leetcode.com/problems/convert-sorted-list-to-binary-search-tree//>{: target="_blank"}

---

We design a help function that convert a linked list to a node with following properties:

1. The node is the mid-node of the linked list.
2. The node's left child is the list consisting of the list nodes before the mid-node.
3. The node's right child is the list consisting of the list nodes after the mid-node.

The algorithm will convert the linked lists and create the tree nodes level by level
until there is no linked list existing as the new-created node's children.

The python code is as follows.

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
#
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution:
    # @param head, a list node
    # @return a tree node
    def sortedListToBST(self, head):
        if not head:
            return None
        root = self.find_mid(head)
        q = [root]
        while q:
            new_q = []
            for n in q:
                if n.left:
                    n.left = self.find_mid(n.left)
                    new_q.append(n.left)
                if n.right:
                    n.right = self.find_mid(n.right)
                    new_q.append(n.right)
            q = new_q
        return root

    def find_mid(self, head):
        prev = None
        slow = head
        fast = head
        while True:
            # Fast go one step
            if fast.next:
                fast = fast.next
            else:
                break
            # Slow go one step
            prev = slow
            slow = slow.next
            # Fast go another step
            if fast.next:
                fast = fast.next
            else:
                break
        # Create a TreeNode for the mid node pointed by 'slow'
        node = TreeNode(slow.val)
        # Set left and right children
        if prev:
            node.left = head
            prev.next = None
        else:
            node.left = None
        node.right = slow.next
        # Return the node
        return node
~~~
{: #fold-body-python}
