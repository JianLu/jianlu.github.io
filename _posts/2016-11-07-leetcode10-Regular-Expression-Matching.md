---
layout: post
title: "[LeetCode 10] Regular Expression Matching"
date: 2016-11-07 22:15
description: Solution of Leetcode Question 10
categories: [Programming]
tags: [leetcode, dynamic programming]
---

Question Link:

<https://leetcode.com/problems/regular-expression-matching/>{: target="_blank"}

---

## Dynamic Programming

Let `M[i][j]` denote if string `s[0..i-1]` matches pattern `p[0..j-1]`. Then we have the recursive function of `M`:

1. If `p[j-1]` is a character other than `*` or `.`:
    `M[i][j] = p[j-1] == s[j-1] and M[i-1][j-1]`
2. If `p[j-1]` is `.`:
    `M[i][j] = M[i-1][j-1]`
3. If `p[j-1]` is `*`, there are three different cases on how `p[j-2, j-1]` matches `s`.
    1. `p[j-2,j-1]` matches 0 element, then `M[i][j] = M[i][j-2]`
    2. `p[j-2,j-1]` matches 1 element, then `M[i][j] = M[i][j-1]` (remove `*`)
    3. `p[j-2,j-1]` matches multiple elements, then `p[j-1]` should be `.` or `s[i-1]`, also `M[i-1][j]` is true (remove last character from `s`)

## Python implementation

The python code is as follows.

<div class="code-title">
<span class="code-fold" id="fold-btn-python" onclick="$use('fold-body-python', 'fold-btn-python')">[-]</span>
Python code accepted by LeetCode OJ
</div>

~~~ python
class Solution(object):
    def isMatch(self, s, p):
        """
        :type s: str
        :type p: str
        :rtype: bool

        DP solution:
        M[i][j] denotes if s[0,..,i-1] matches p[0,..,j-1]
        Then the recurisve function of M[i][j] is:
        1. If p[j-1] is a character other than "*" and ".":
            M[i][j] = M[i-1][j-1] && s[i-1] == p[j-1]
        2. If p[j-1] is ".":
            M[i][j] = M[i-1][j-1]
        3. If p[j-1] is "*":
            1) If "*" matches 0 p[j-2]:
                M[i][j] = M[i][j-2]
            2) If "*" matches 1 p[j-2]:
                M[i][j] = M[i][j-1]
            3) If "*" matches multiple p[j-2], M[i][j] is true iff:
                a. s[i-1] == p[j-2] or p[j-2] is ".", and
                b. M[i-1][j] is true
        """

        n = len(s)
        m = len(p)

        # Initialize the 2D array
        M = [[False for x in range(m+1)] for y in range(n+1)]
        M[0][0] = True
        # Initialize M[0][j] for j = 2,...,m
        for j in xrange(2, m+1):
            if p[j-1] == "*":
                M[0][j] = M[0][j-2]

        for i in xrange(1, n+1):
            for j in xrange(1, m+1):
                if p[j-1] == ".":
                    M[i][j] = M[i-1][j-1]
                elif p[j-1] == "*":
                    if M[i][j-2] or M[i][j-1] or ( (p[j-2] == "." or p[j-2] == s[i-1]) and M[i-1][j] ):
                        M[i][j] = True
                else:
                    M[i][j] = M[i-1][j-1] and p[j-1] == s[i-1]

        return M[n][m]

~~~
{: #fold-body-python}

## Character-by-character Matching

There is a character-by-character matching [here](http://articles.leetcode.com/regular-expression-matching){:target="_blank"}.
