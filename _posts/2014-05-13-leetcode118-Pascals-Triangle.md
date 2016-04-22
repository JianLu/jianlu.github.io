---
layout: post
title: "[LeetCode 118] Pascal's Triangle"
date: 2014-05-13 08:52
description: Solution of LeetCode Question 118
categories: [Programming, dynamic programming]
tags: [leetcode]
math: true
---

Question Link:

<https://leetcode.com/problems/pascals-triangle/>{: target="_blank"}

---

没啥可说的，直接上code吧……

<div class="code-title">
<span class="code-fold" id="fold-btn-python" onclick="$use('fold-body-python', 'fold-btn-python')">[-]</span>
Python code accepted by LeetCode OJ
</div>

~~~ python
class Solution:
    # @return a list of lists of integers
    def generate(self, numRows):
        # Initialize the triangle
        res = []
        for i in xrange(numRows):
            res.append([1] * (i+1))
        # Compute the triangle row by row
        for row in xrange(1, numRows):
            for i in xrange(1, row):
                res[row][i] = res[row-1][i] + res[row-1][i-1]
        # Return
        return res
~~~
{: #fold-body-python}