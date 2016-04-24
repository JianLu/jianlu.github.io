---
layout: post
title: "[LeetCode 122] Best Time to Buy and Sell Stock II"
date: 2014-05-06 22:58
description: Solution of LeetCode Question 122
categories: [Programming]
tags: [leetcode, greedy algorithm]
---

Question Link:

<https://leetcode.com/problems/best-time-to-buy-and-sell-stock-ii/>{: target="_blank"}

---

## Greedy Algorithm

We solve this problem using Greedy Algorithm which only scan the prices list once, so the time complexity is `O(n)`.

According to the problem description, we know that the transaction operations must be like:

buy, sell, buy, sell, ..., buy, sell

For each day, we have two status, holding stocks or not.
And we have no stocks in day 0.

Then for each day i, we decide to buy or sell according to the prices of the following day:

* If we have no stock: we buy stock if prices[i] < prices[i+1]; otherwise, we do nothing.
* If we have stock: we sell stock if prices[i] > prices[i+1]; otherwise, we do nothing.
Note that we need to sell the stock in the last day if we have stock after scanning prices[0:n-1].

You can prove this greed choice will make your profit maximum.

## Python Implementation

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
        We solve this problem using Greedy Algorithm, for each day i:
        1 - I hold stock, I need to decide sell or not: sell if prices[i] > prices[i+1]
        2 - I don't hold stock, I need to decide buy or not: buy if prices[i] < prices[i+1]
        """
        # Initialization
        total_profit = 0
        buy_price = None

        # Scan
        for i in xrange(len(prices)-1):
            if buy_price is None:
                # No stock
                if prices[i] < prices[i+1]:
                    buy_price = prices[i]
            elif prices[i] > prices[i+1]:
                total_profit += prices[i] - buy_price
                buy_price = None

        # If holding stock, sell them to get the maximum profit
        if buy_price is None:
            return total_profit
        else:
            return total_profit + prices[-1] - buy_price
~~~
{: #fold-body-python}
