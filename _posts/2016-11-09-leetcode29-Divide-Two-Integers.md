---
layout: post
title: "[LeetCode 29] Divide Two Integers"
date: 2016-11-09 22:35:29
description: Solution of Leetcode Question 29
categories: [Programming]
tags: [leetcode, bit operation]
---

Question Link:

<https://leetcode.com/problems/divide-two-integers/>{: target="_blank"}

---

## Solution

Use bit shift operation to simulate `x2` or `/2`
 
## Implementation

<div class="code-title">
<span class="code-fold" id="fold-btn-python" onclick="$use('fold-body-python', 'fold-btn-python')">[-]</span>
Python code accepted by LeetCode OJ
</div>

~~~ python
class Solution(object):
    def divide(self, dividend, divisor):
        """
        :type dividend: int
        :type divisor: int
        :rtype: int
        """
        #python 3 have removed this constant
        MAX_INT = 2147483647 
        MIN_INT = -2147483648
        
        if divisor == 0 or (divisor == -1 and dividend == MIN_INT):
            return MAX_INT
        elif divisor == 1:
            return dividend
        elif divisor == -1:
            return -dividend
            
        positive = True
        if dividend < 0:
            dividend = -dividend
            positive = not positive
        if divisor < 0:
            divisor = - divisor
            positive = not positive
        
        if dividend < divisor:
            return 0
        
        res = 0
        while dividend >= divisor:
            m = 1
            d = divisor
            while (d << 1) < dividend:
                d = d << 1
                m = m << 1
            res += m
            dividend -= d
        
        if positive:
            return res
        else:
            return -res
~~~
{: #fold-body-python}


