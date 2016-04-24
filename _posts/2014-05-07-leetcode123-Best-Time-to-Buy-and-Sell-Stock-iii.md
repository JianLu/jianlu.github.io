---
layout: post
title: "[LeetCode 123] Best Time to Buy and Sell Stock III"
date: 2014-05-07 01:21
description: Solution of LeetCode Question 123
categories: [Programming]
tags: [leetcode, dynamic programming]
---

Question Link:

<https://leetcode.com/problems/best-time-to-buy-and-sell-stock-iii/>{: target="_blank"}

---

人生新高，99.22%，亲啊，这是真的么……

![showoff](/images/showoff_20140507.PNG)

---
## Linear Time Solution

We try to solve this problem in `O(n)` time by using the solution of
[Best Time to Buy and Sell Stock](/2014/05/06/leetcode121-Best-Time-to-Buy-and-Sell-Stock/),
which finds the optimal profit of one transaction.

Suppose the transaction `(b, s)` is the optimal for price list `P[1..n]`. Then we have:

1. `P[i] >= P[b]`, for `i = 1, ..., s`
1. `P[i] <= P[s]`, for `i = b, ..., n`

Let `find_best_transaction` is the function to find optimal transaction,
the following algorithm solves the problem:

1. By given the price list `P[1..n]`, find the best transaction `(b,s) = find_best_transaction(P)`
2. Find the maximum profit for the following three parts divided by b and s:
    * P1: the maximum profit for the price list `P[1..b-1]`
    * P2: the maximum profit for the price list `P[b+1..s-1].inverse()`
    * P3: the maximum profit for the price list `P[s+1..n]`
3. The answer should be `P[s] - P[b] + max(P1,P2,P3)`.

## Correctness

Now we prove the algorithm above is correct.
Let `(b,s)` be the optimal transaction of `P[1..n]`, then there are two cases:
1) `(b,s)` is one of the two transactions; or 2) `(b,s)` is not in the result.

For case 1), according to the problem description, you must sell the stock before you buy again,
so the other transaction must happen in `P[1...b+1]` or `P[s+1..n]`.
Then the answer is `(P[s] - P[b]) + max(P1, P3)`.

For case 2), let `(b1, s1)` and `(b2, s2)` be the first and second transactions of the solution,
where `1 <= b1 < s1 < b2 < s2 <= n`. We can have following cases:

### Case 2-1: s < b2

The optimal answer could be `(b, s)` and `(b2, s2)`, because we can replace `(b1, s1)` with `(b, s)`.
Therefore the answer of this case could be `P[s] - P[b] + P1`

### Case 2-2: b > s1

The optimal answer could be `(b1, s1)` and `(b, s)`, because we can replace `(b2, s2)` with `(b, s)`.
Therefore the answer of this case could be `P[s] - P[b] + P2`

### Case 2-3: b <= s1 and s >= b2

This means `b <= s1 < b2 <= s`, then we have:

1. We can replace `b1` with `b` because `P[b]` is the minimum of `P[1..s]`;
2. We can replace `s2` with `s` because `P[s]` is the maximum of `P[b..n]`.

The optimal solution could be like: `--- b --- s1 --- b2 -- s ---` for this case,
where the profit is `P[s1] - P[b] + P[s] - P[b2]`, that is, `P[s] - P[b] + P[s1] - P[b2]`.
And `P[s1] - P[b2]` is just the answer of find the optimal transaction of `P[s-1 .. b+1]`.
Therefore the answer of this case could be `P[s] - P[b] + P2`

Considering all cases above, the optimal profit should be maximum of all four cases, which is
`P[s] - P[b] + max(P1, P2, P3)`.

## Python Implement

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
        if n < 2:
            return 0

        # Find the best transaction
        b, s, profit = self.find_best_transaction(prices)

        # Find max(P1, P2, P3)
        profit1 = 0
        if b > 1:
            profit1 = max(profit1, self.find_best_transaction(prices[:b])[2])
        if s > b + 2:
            profit1 = max(profit1, self.find_best_transaction(prices[b+1:s][::-1])[2])
        if s < n - 2:
            profit1 = max(profit1, self.find_best_transaction(prices[s+1:])[2])

        # Return
        return profit + profit1

    def find_best_transaction(self, prices):
        """
        Return the optimal transaction (b, s) of prices.
        """
        b = s = l = 0
        for i in xrange(1, len(prices)):
            if prices[i] <= prices[l]:
                l = i
            elif prices[i] - prices[l] > prices[s] - prices[b]:
                b = l
                s = i
        return b, s, prices[s] - prices[b]
~~~
{: #fold-body-python}
