---
layout: post
title: "[LeetCode 43] Multiply Strings"
date: 2016-11-07 11:03:31
description: Solution of Leetcode Question 43
categories: [Programming]
tags: [leetcode]
---

Question Link:

<https://leetcode.com/problems/multiply-strings/>{: target="_blank"}

Given two numbers represented as strings, return multiplication of the numbers as a string.

* The numbers can be arbitrarily large and are non-negative.
* Converting the input string to integer is NOT allowed.
* You should NOT use internal library such as BigInteger.

---

## Solution

The solution is very straightforword, just simulate the multiplication operations.

1. From low digit to high digit in multiplier, calculate multiplcation of multiplicand and the digit.
2. Add the result to corresponding position. `s1[i]` and `s2[j]` should be added into `res[i+j]`.
3. Need to take care the carry, especially it is possible that there is a new carry at the highest digit for each pass of the multiplication of multiplicand and the mulitplier digit.
4. Because we need to calculate from low to high digit, one trick is to reverse `s1` and `s2`, and return the reverse of the multiplication result.


## Python implementation

The python code is as follows.

<div class="code-title">
<span class="code-fold" id="fold-btn-python" onclick="$use('fold-body-python', 'fold-btn-python')">[-]</span>
Python code accepted by LeetCode OJ
</div>

~~~ python
class Solution(object):
    def multiply(self, num1, num2):
        """
        :type num1: str
        :type num2: str
        :rtype: str
        """
        if len(num1) == 0 or len(num2) == 0:
            return ""

        n1 = len(num1)
        n2 = len(num2)

        # Reverse the string so we can scan strings from the left to right
        # T(n) = O(n)
        num1 = num1[::-1]
        num2 = num2[::-1]

        # Create an empty ret string
        # S(n) = O(n)
        ret = [0] * (n1 + n2)

        # For each digits in s2
        for j in xrange(n2):
            carry = 0
            d2 = int(num2[j])

            # Calculate s1 * d2
            for i in xrange(n1):
                d1 = int(num1[i])
                res = ret[i+j] + d1 *d2 + carry
                carry = int(res / 10)
                ret[i+j] = res - carry * 10

            # After calculation of s1*d2, we need to check if s1 * d2 has a carry at the highest position
            # We can assume that it is the first time to visit ret[n1 + j]
            if carry != 0:
                ret[n1 + j] = carry

        # Convert result from int to string, and reverse the digits
        ret = "".join(str(x) for x in ret[::-1])
        i = 0
        while i < len(ret) and ret[i] == '0':
            i += 1
        ret = ret[i:]
        if len(ret) == 0: return '0'
        return ret
~~~
{: #fold-body-python}
