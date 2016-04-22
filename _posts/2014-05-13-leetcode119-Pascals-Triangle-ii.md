---
layout: post
title: "[LeetCode 119] Pascal's Triangle II"
date: 2014-05-13 09:19
description: Solution of LeetCode Question 119
categories: [Programming, dynamic programming]
tags: [leetcode]
math: true
---

Question Link:

<https://leetcode.com/problems/pascals-triangle-ii/>{: target="_blank"}

---

Let `T[i][j]` be the `j`-th element of the `i`-th row in the triangle,
for `0 <= j <= i`, `i = 0, 1, ...`

And we have the recursive function for `T[i][j]`:

`T[i][j] = 1, if j == 0 or j == i`

`T[i][j]　= T[i-1][j] + T[i-1][j-1]`

Simple enough, the python code is as follows.

<div class="code-title">
<span class="code-fold" id="fold-btn-python" onclick="$use('fold-body-python', 'fold-btn-python')">[-]</span>
Python code accepted by LeetCode OJ
</div>

~~~ python
class Solution:
    # @return a list of integers
    def getRow(self, rowIndex):
        # Spcecial case
        if rowIndex < 2:
            return [1] * (rowIndex + 1)
        # Initial two lists for update
        res = []
        res.append([1]*(rowIndex+1))
        res.append([1]*(rowIndex+1))
        current = 0
        # Update row by row
        for row in xrange(2, rowIndex+1):
            for i in xrange(1, row):
                res[1-current][i] = res[current][i] + res[current][i-1]
            current = 1 - current
        # Return the current list
        return res[current]
~~~
{: #fold-body-python}