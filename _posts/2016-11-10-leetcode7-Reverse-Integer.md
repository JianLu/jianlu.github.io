---
layout: post
title: "[LeetCode 7] Reverse Integer"
date: 2016-11-10 11:36:05
description: Solution of Leetcode Question 7
categories: [Programming]
tags: [leetcode]
---

Question Link:

<https://leetcode.com/problems/reverse-integer/>{: target="_blank"}

---

## Solution

Use integer division and mod operations.
The other thing is to take care the signed integer overflow.
 
## Implementation

<div class="code-title">
<span class="code-fold" id="fold-btn-python" onclick="$use('fold-body-python', 'fold-btn-python')">[-]</span>
Python code accepted by LeetCode OJ
</div>

~~~ python
class Solution(object):
    def reverse(self, x):
        """
        :type x: int
        :rtype: int
        """
        # Signed int32 overflow
        MAX_SIGNED_INT = 2147483647

        if x < 0:
            n = -x
        else:
            n = x
        
        res = 0
        while n > 0:
            res = res * 10 + n % 10
            n = int(n * 0.1)
            if res > MAX_SIGNED_INT:
                return 0
                
        if x < 0:
            return -res
        else:
            return res
~~~
{: #fold-body-python}


