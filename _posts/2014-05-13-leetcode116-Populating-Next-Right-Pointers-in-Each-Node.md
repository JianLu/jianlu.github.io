---
layout: post
title: "[LeetCode 116] Populating Next Right Pointers in Each Node"
date: 2014-05-13 10:10
description: Solution of LeetCode Question 116
categories: [Programming]
tags: [leetcode, binary tree]
math: true
---

Question Link:

<https://leetcode.com/problems/populating-next-right-pointers-in-each-node/>{: target="_blank"}

---

Just traverse the tree from the root, level by level, easy enough...

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
#         self.next = None

class Solution:
    # @param root, a tree node
    # @return nothing
    def connect(self, root):
        """
        We use a BFS from the root, so that we can traverse the tree level by level.
        We need two queues, to store the nodes of this level and next.
        """
        if root is None:
            return
        q = [root]
        while q:
            new_q = []
            for i in xrange(len(q)-1):
                q[i].next = q[i+1]
            q[-1].next = None
            for node in q:
                if node.left:
                    new_q.append(node.left)
                if node.right:
                    new_q.append(node.right)
            q = new_q

~~~
{: #fold-body-python}
