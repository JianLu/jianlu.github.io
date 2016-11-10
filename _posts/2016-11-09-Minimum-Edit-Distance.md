---
layout: post
title: "Minimum Edit Distance"
date: 2016-11-09 20:36:29
description: Dynamic programming solution of minimum edit distance
categories: [Programming]
tags: [leetcode, edit distance]
---

Question Link:

<http://www.geeksforgeeks.org/dynamic-programming-set-5-edit-distance/>{:target="_blank"}

Given two strings str1 and str2 and below operations that can performed on str1. 
Find minimum number of edits (operations) required to convert ‘str1’ into ‘str2’.

* Insert
* Remove
* Replace

All of the above operations are of equal cost.

---

## Related Problems

[[LeetCode 161] One Edit Distance](/2016/11/09/leetcode161-One-Eidt-Distance/){:target="_blank"}

## Solution

We use dynamic programming to calculate the minimum edit distance between string `s1[0,..,n-1]` and `s2[0,..,m-1]`.

Let `M[i][j]` denote the minimum edit distance between `s1[0,...,i-1]` and `s2[0,...,j-1]`,
and `S[i]` denotes `s[0,..,j-1]`.
The recursive functions of `M[i+1][j+1]` is:
 
1. If `s1[i+1] == s2[j+1]`, then `M[i+1][j+1] = M[i][j]`.
2. Otherwise, there are three ways to convert `S1[i+1]` to `S2[j+1]`:

    1) Remove `s[i]` from `S1[i+1]`, and convert `S1[i]` to `S2[j+1]`. In this case, `M[i+1][j+1] = 1 + M[i][j+1]`
    
    2) Convert `S1[i+1]` to `S2[j]`, and insert `s1[i]`. In this case, `M[i+1][j+1] = M[i+1][j] + 1`
    
    3) Convert `S1[i]` to `S2[j]`, and replace `s1[i]` with `s2[j]`. In this case, `M[i+1][j+1] = M[i][j] + 1`
    
    And we only need to select the way with the smallest cost.
 
## Implementation

<div class="code-title">
<span class="code-fold" id="fold-btn-python" onclick="$use('fold-body-python', 'fold-btn-python')">[-]</span>
Python code accepted by LeetCode OJ
</div>

~~~ python
class Solution(object):
    def minEditDistance(self, s1, s2):
        m, n = len(s1), len(s2)

        # Create a table to store results of subproblems
        # M[i][j] denotes the minimum edit distance from s1[0,..,i-1] to s2[0,..,j-1]
        d = [[0 for _ in range(n + 1)] for _ in range(m + 1)]

        # Initialize boundaries
        # If s1 == "", then d(s1, s2) = len(s2), add characters to s1
        d[0] = [x for x in range(n+1)]
        # If s2 == "", then d(s1, s2) = len(s1), remove characters from s1
        for i in range(m+1):
            d[i][0] = i

        # Fill in the table bottom-up
        for i in range(m):
            for j in range(n):
                if s1[i] == s2[j]:
                    d[i+1][j+1] = d[i][j]
                else:
                    d[i+1][j+1] = 1 + min(d[i][j+1], d[i+1][j], d[i][j])

        return d[m][n]

if __name__ == "__main__":
    sol = Solution()
    s1 = "sunday"
    s2 = "saturday"
    print(sol.minEditDistance(s1, s2))
~~~
{: #fold-body-python}


