---
layout: post
title: "[LeetCode 121] Best Time to Buy and Sell Stock"
date: 2014-05-06 22:46
description: Solution of LeetCode Question 121
categories: [Programming]
tags: [leetcode, dynamic programming]
---

Question Link:

<https://leetcode.com/problems/best-time-to-buy-and-sell-stock/>{: target="_blank"}

---

## DP Solution

We solve this problem by using DP, which only scans the prices list once.
So the algorithm is in `O(n)` where `n` is the length of prices list.

By given a list `P[1..n]`, we represent a transaction as `(b,s)` where `b` is the day to buy, `s` is the day to sell, and `0 <= b <= s <= n-1`.
Let `(B[i], S[i])` be the best transaction for the first `i` days, that is, `P[S[i]] - P[B[i]]` is maximum.

Suppose we already have the best transaction `(B[i], S[i])` for the first `i` days.
Then, for the first `i+1` days, there are only two cases that best transaction changes:
1) If `P[i+1] > P[S[i]]` then `i+1` should be the best day to sell;
2) If `P[i+1] - min(P[1..i]) > P[S[i]] - P[B[i]]`, then `i+1` and the lowest price day should be the best day to sell and buy.
Otherwise, `S[i+1] = S[i]` and `B[i+1] = B[i]`.

However, since we are only asked for the optimal solution for the n days.
We do not need the array `B` and `S` to store all local optimal solutions.
Therefore, we only need three variables:

* `l`, the day with the lowest value
* `b', the optimal day to buy
* `s`, the optimal day to sell

Initially, we have `l = b = s = 1`.

## Python Implementation

In the following Python implementation, we don't need to track the days (index), because the question asks for the optimal profit.
So the implementation could be more simple.

<div class="code-title">
<span class="code-fold" id="fold-btn-python" onclick="$use('fold-body-python', 'fold-btn-python')">[-]</span>
Python code accepted by LeetCode OJ
</div>

~~~ python
class Solution(object):
    def maxProfit(self, prices):
        """
        :type prices: List[int]
        :rtype: int
        """
        n = len(prices)

        # Specail case
        if n < 2:
            return 0

        # Initialization
        l = prices[0]
        profit = 0

        # Scan prices
        for i in xrange(1, n):
            # If prices[i] is the lowest, it cannot be the optimal sell day, just update the lowest
            if prices[i] < l:
                l = prices[i]
            elif prices[i] - l > profit:
                profit = prices[i] - l

        return profit
~~~
{: #fold-body-python}
