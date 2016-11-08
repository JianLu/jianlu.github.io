---
layout: post
title: "[LeetCode 38] Count and Say"
date: 2016-11-07 20:56
description: Solution of Leetcode Question 38
categories: [Programming]
tags: [leetcode]
---

Question Link:

<https://leetcode.com/problems/count-and-say/>{: target="_blank"}

---

## Note

Do NOT use the recursive function, the performance is very bad.
Just use two slot array to do the iterations.

## Python implementation

The python code is as follows.

<div class="code-title">
<span class="code-fold" id="fold-btn-python" onclick="$use('fold-body-python', 'fold-btn-python')">[-]</span>
Python code accepted by LeetCode OJ
</div>

~~~ python
class Solution(object):
    def countAndSay(self, n):
        """
        :type n: int
        :rtype: str
        """
        if n == 0:
            return ""

        # Two-slot array for interation loops
        s = ["1", ""]
        curr = 0

        # Iterations
        for _ in xrange(1, n):
            s[1-curr] = ""
            count, c = 1, s[curr][0]
            for nn in s[curr][1:]:
                if nn == c:
                    count += 1
                else:
                    s[1-curr] += str(count) + c
                    c = nn
                    count = 1
            s[1-curr] += str(count) + c
            curr = 1 - curr

        # Return
        return s[curr]

~~~
{: #fold-body-python}
