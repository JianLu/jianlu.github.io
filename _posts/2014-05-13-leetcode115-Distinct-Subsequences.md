---
layout: post
title: "[LeetCode 115] Distinct Subsequences"
date: 2014-05-13 11:07
description: Solution of LeetCode Question 115
categories: [Programming]
tags: [leetcode, string]
math: true
---

Question Link:

<https://leetcode.com/problems/distinct-subsequences/>{: target="_blank"}

---

A classic problem using Dynamic Programming technique.

Let `m` and `n` be the length of the strings `T` and `S`.
Let `R[i][j]` be the count of distinct subsequence of `T[0..i]` in `S[0..j]`.
Obviously, we have `R[i][j] = 0` for `i > j`.

We initialize `R[0][..]`: for `j = 1 .. n-1`,
if `S[j] == T[0]`, then `R[0][j] = R[0][j-1] + 1`; otherwise `R[0][j] = R[0][j-1]`.

Then we use the following recursive function to update `R[i][j]` bottom-up (from `i = 1` to `m-1` and `j = i` to `n-1`):


`R[i][j] = R[i][j-1] + R[i-1][j-1], if S[j] == T[i]`

`R[i][j] = R[i][j-1], otherwise`

The python code is as follows.

<div class="code-title">
<span class="code-fold" id="fold-btn-python" onclick="$use('fold-body-python', 'fold-btn-python')">[-]</span>
Python code accepted by LeetCode OJ
</div>

~~~ python
class Solution:
    # @return an integer
    def numDistinct(self, S, T):
        """
        Suppose two string S[0..n-1] and T[0..m-1], with n >= m
        DP method. Let R[i][j] be the count of distinct subsequences of T[0..i] in S[0..j].
        Obviously, R[i][j] = 0, for i > j.
        Initialization: R[0][j] from j = 0 to n-1
        Recursive Function:
            R[i][j] = R[i-1][j-1] + R[i][j-1], if T[i] == T[j]
            R[i][j] = R[i][j-1], otherwise
        """
        n = len(S)
        m = len(T)
        # Special case
        if n < m:
            return 0
        # Create the 2D array R
        R = []
        for _ in xrange(m):
            R.append([0]*n)
        # Initial R[1][0..n-1]
        if T[0] == S[0]:
            R[0][0] = 1
        for j in xrange(1,n):
            if T[0] == S[j]:
                R[0][j] = R[0][j-1] + 1
            else:
                R[0][j] = R[0][j-1]
        # Update R from i = 1 to m-1, j = 0
        for i in xrange(1, m):
            for j in xrange(i, n):
                if T[i] == S[j]:
                    R[i][j] = R[i-1][j-1] + R[i][j-1]
                else:
                    R[i][j] = R[i][j-1]
        # Return R[m-1][n-1]
        return R[m-1][n-1]
~~~
{: #fold-body-python}
