---
layout: post
title: "[LeetCode 138] Copy List with Random Pointer"
date:  2014-04-06 12:03
description: Solution of Leetcode Question 138
categories: [Programming]
tags: [leetcode, pointer]
---

Question Link:

<https://leetcode.com/problems/copy-list-with-random-pointer/>{: target="_blank"}

A linked list is given such that each node contains an additional random pointer which could point to any node in the list or null.

Return a deep copy of the list.

---

## Solution

### Deep Copy

Deep copy a linked list, means you need to create new nodes (each copy with a new memory address)
and linked up all these new nodes corresponding to the relationships of their original copies.

### Random Pointers

Without "random pointers", it is really simple problems to duplicate a new linked list.
It could be done by duplicating the head node and duplicate the next node nodes, and go on.

However, with the "random pointers", the problem is that, when we get a pointer, it is very possible that
we have not crate the node which the pointer points to yet.
So we cannot deep copy the linked list in one pass. We need to deep copy the linked list without assigning
the random pointers in the first pass, and then fill in  the random pointers in the second scan of the list.
To speed up the second pass, we may need the hash map to store the mapping from original node to its copy.



## Python Code

![showoff](/images/showoff_20160406.PNG)

The python code is as follows.

<div class="code-title">
<span class="code-fold" id="fold-btn-python" onclick="$use('fold-body-python', 'fold-btn-python')">[-]</span>
Recursive traversal Python code accepted by LeetCode OJ
</div>

~~~ python
# Definition for singly-linked list with a random pointer.
# class RandomListNode(object):
#     def __init__(self, x):
#         self.label = x
#         self.next = None
#         self.random = None

class Solution(object):
    def copyRandomList(self, head):
        """
        :type head: RandomListNode
        :rtype: RandomListNode
        """

        # Special case
        if head is None:
            return None

        #
        # First pass: duplicate the linked list without random pointers
        #

        # Hash table: new_node[original_node] = new_node
        new_node = {}

        # Create the new head node
        new_head = RandomListNode(head.label)
        new_node[head] = new_head

        # Cursors
        p1 = head
        p2 = new_head

        # Scan & copy
        while p1.next is not None:
            # Create a copy
            p2.next = RandomListNode(p1.next.label)
            new_node[p1.next] = p2.next
            # Go on
            p2 = p2.next
            p1 = p1.next

        #
        # Second pass: add random pointers
        #
        p1 = head
        while p1 is not None:
            if p1.random is not None:
                new_node[p1].random = new_node[p1.random]
            p1 = p1.next

        #
        # Return
        #
        return new_head
~~~
{: #fold-body-python}
