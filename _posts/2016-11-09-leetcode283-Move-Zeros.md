---
layout: post
title: "[LeetCode 283] Move Zeroes"
date: 2016-11-09 15:02:29
description: Solution of Leetcode Question 283
categories: [Programming]
tags: [leetcode]
---

Question Link:

<https://leetcode.com/problems/move-zeroes/>{: target="_blank"}

---

## Solution

We utilize the algorithm behind bubble sorting, we move the zero segment from the beginning to the end of the array.
We do not need to swap 0 back number by number, but move the zero segment by swapping the first 0 to the non-zero number next to the zero segment.

## Implementation

<div class="code-title">
<span class="code-fold" id="fold-btn-python" onclick="$use('fold-body-python', 'fold-btn-python')">[-]</span>
Python code accepted by LeetCode OJ
</div>

~~~ python
class Solution(object):
    def moveZeroes(self, nums):
        """
        :type nums: List[int]
        :rtype: void Do not return anything, modify nums in-place instead.
        """
        n = len(nums)
        if n <= 1:
            return
        
        # Find the first zero segment
        s0 = 0
        while s0 < n and nums[s0] != 0:
            s0 += 1
            
        e0 = s0 + 1
        while e0 < n:
            if nums[e0] == 0:
                e0 += 1
            else:
                nums[s0] = nums[e0]
                nums[e0] = 0
                s0 += 1
                e0 += 1
                
~~~
{: #fold-body-python}


