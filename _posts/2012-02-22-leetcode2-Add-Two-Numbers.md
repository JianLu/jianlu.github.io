---
layout: post
title: "[LeetCode] Add Two Numbers"
date: 2016-02-22 12:49:14
description: Solution of LeetCode Question 2
categories: [Programming]
tags: [leetcode]
---

Question Link:

<https://leetcode.com/problems/add-two-numbers/>{: target="_blank"}

---

This is a pretty straightforward questions about the usage of the single linked list.
The only thing is to remember the last carry bit.


<div class="code-title">
<span class="code-fold" id="fold-btn-python" onclick="$use('fold-body-python', 'fold-btn-python')">[-]</span>
Python code accepted by LeetCode OJ
</div>

~~~ python
# Definition for singly-linked list.
# class ListNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution(object):
    def addTwoNumbers(self, l1, l2):
        """
        :type l1: ListNode
        :type l2: ListNode
        :rtype: ListNode
        """
        if l1 is None and l2 is None:
            return None

        h1 = l1
        h2 = l2
        s = 0
        rtn = ListNode(0)
        h = rtn

        while True:
            if h1 is not None:
                s += h1.val
                h1 = h1.next
            if h2 is not None:
                s += h2.val
                h2 = h2.next

            if s > 9:
                h.val = s - 10
                s = 1
            else:
                h.val = s
                s = 0

            if h1 is None and h2 is None:
                if s == 1:
                    h.next = ListNode(1)
                break
            h.next = ListNode(0)
            h = h.next

        return rtn
~~~


<div class="code-title">
<span class="code-fold" id="fold-btn-cpp" onclick="$use('fold-body-cpp', 'fold-btn-cpp')">[-]</span>
C++ code accepted by LeetCode OJ
</div>

~~~ cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    ListNode* addTwoNumbers(ListNode* l1, ListNode* l2) {
        if (l1 == NULL && l2 == NULL)
        {
            return NULL;
        }
        ListNode* p = new ListNode(0);
        int s = 0;
        ListNode* head = p;
        while (true)
        {
            if(l1) {
                s += l1->val;
                l1 = l1->next;
            }
            if (l2) {
                s += l2->val;
                l2 = l2->next;
            }
            if (s > 9)
            {
                p->val = s - 10;
                s = 1;
            }
            else {
                p->val = s;
                s = 0;
            }
            if (l1 || l2)
            {
                p->next = new ListNode(0);
                p = p->next;
            }
            else {
                if (s == 1)
                {
                    p->next = new ListNode(s);
                }
                break;
            }
        }
        return head;
    }
};
~~~