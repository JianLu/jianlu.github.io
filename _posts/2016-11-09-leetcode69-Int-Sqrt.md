---
layout: post
title: "[LeetCode 69] Sqrt(x)"
date: 2016-11-09 19:49:29
description: Solution of Leetcode Question 69
categories: [Programming]
tags: [leetcode, binary search]
---

Question Link:

<https://leetcode.com/problems/sqrtx/>{: target="_blank"}

---

## Solution

The solution is same to the binary search on the sorted array, with following differences:
 
 1. The target is to find sqrt(x) instead of x
 2. Instead of not found, we need to find the right smaller integer to the sqrt(x)
 
## Implementation

<div class="code-title">
<span class="code-fold" id="fold-btn-python" onclick="$use('fold-body-python', 'fold-btn-python')">[-]</span>
Python code accepted by LeetCode OJ
</div>

~~~ python
class Solution(object):
    def mySqrt(self, x):
        """
        :type x: int
        :rtype: int
        """
        if x < 2:
            return x
            
        low, high = 1, x
        while low <= high:
            mid = (low+high) >> 1
            v1 = mid * mid
            if v1 == x:
                return mid
            elif v1 < x:
                v2 = (mid + 1) * (mid + 1)
                if v2 == x:
                    return mid+1
                elif v2 > x:
                    return mid
                else:
                    low = mid + 1
            elif v1 > x:
                v2 = (mid - 1) * (mid - 1)
                if v2 == x:
                    return mid-1
                elif v2 < x:
                    return mid - 1
                else:
                    high = mid - 1
        
        # Return anyway
        return low                
~~~
{: #fold-body-python}


