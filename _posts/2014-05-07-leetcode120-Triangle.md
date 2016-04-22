---
layout: post
title: "[LeetCode 120] Triangle"
date: 2014-05-07 04:29
description: Solution of LeetCode Question 120
categories: [Programming]
tags: [leetcode, dynamic programming]
math: true
---

Question Link:

<https://leetcode.com/problems/triangle/>{: target="_blank"}

---

Let `R[][]` be a 2D array where `R[i][j]` (`j <= i`) is the minimum sum of the path from
`triangle[0][0]` to `triangle[i][j]`.

We initialize `R[0][0] = triangle[0][0]`, and update `R[][]` from `i = 1` to `n-1`:

`R[i][0] = triangle[i][0] + R[i-1][0]`

`R[i][i] = triangle[i][i] + R[i-1][i-1]`

`R[i][j] = triangle[i][i] + min(R[i-1][j-1], R[i-1][j]) for 1 < j < i`

After scan all triangle elements, we just return the minimum value of `R[n-1][..]`

We note that `R[i][..]` is only related to `R[i-1][..]`, so we do not need keep all rows of `R[][]`.
Instead, we use two arrays of length `n`, and each time we update one with the other.

The following is a python implementation, where the time complexity is `O(n^2)` and space complexity is `O(n)`.

<div class="code-title">
<span class="code-fold" id="fold-btn-python" onclick="$use('fold-body-python', 'fold-btn-python')">[-]</span>
Python code accepted by LeetCode OJ
</div>

~~~ python
class Solution:
    # @param triangle, a list of lists of integers
    # @return an integer
    def minimumTotal(self, triangle):
        """
        We scan the triangle from the first row to the last row,
        and we maintian an array s[0..n-1] where s[i] is the minimum path sum
        if we pick i-th number as the path element in the current row
        After scan all rows, we return the minimum value of s[].
        """
        n = len(triangle)
        if n == 0:
            return 0
        s = [[2**32] * n, [2**32] * n]
        s[0][0] = triangle[0][0]
        current = 0
        row = 1
        for i in xrange(1,n):
            # Scan the i-th row, whose length is i+1
            # Compute the sum reaching the first element of this row
            s[1-current][0] = triangle[i][0] + s[current][0]
            # Compute the sum reaching the last element of this row
            s[1-current][i] = triangle[i][i] + s[current][i-1]
            # Compute others
            for j in xrange(1,i):
                s[1-current][j] = triangle[i][j] + min(s[current][j], s[current][j-1])
            # Go to next row and swith s[0] and s[1]
            current = 1 - current
        # Return the maximum value of S[current]
        return min(s[current])
~~~
{: #fold-body-python}