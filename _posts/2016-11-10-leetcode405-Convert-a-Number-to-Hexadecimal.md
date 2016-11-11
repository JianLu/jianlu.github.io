---
layout: post
title: "[LeetCode 405] Convert a Number to Hexadecimal"
date: 2016-11-10 21:25:05
description: Solution of Leetcode Question 405
categories: [Programming]
tags: [leetcode]
---

Question Link:

<https://leetcode.com/problems/convert-a-number-to-hexadecimal/>{: target="_blank"}

---

## Solution

We convert each 4 bits of the number from lowest bits to higher.
The only thing is how to convert a negative integer.
According to [Two's Complement](https://en.wikipedia.org/wiki/Signed_number_representations){:target="_blank"},
we can convert negative by coding the negation of a positive number:

1. Invert all the bits of the number
2. Add one

## Implementation

<div class="code-title">
<span class="code-fold" id="fold-btn-python" onclick="$use('fold-body-python', 'fold-btn-python')">[-]</span>
Python code accepted by LeetCode OJ
</div>

~~~ python
class Solution(object):
    def toHex(self, num):
        """
        :type num: int
        :rtype: str
        """
        if num == 0: 
            return "0" 
            
        code = "0123456789abcdef"
        res = ""
        
        # Tow's complement negation
        if num < 0:
            num = (-num ^ 0xffffffff) + 1
        
        # Convert binary to hex
        while num != 0:
            x = num & 0xf
            res = code[x] + res
            num = num >> 4
        
        # Return
        return res
~~~
{: #fold-body-python}


