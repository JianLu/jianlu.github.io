---
layout: post
title: "[LeetCode 167] Two Sum II - Input array is sorted"
date: 2016-11-09 13:50:18
description: Solution of Leetcode Question 167
categories: [Programming]
tags: [leetcode]
---

Question Link:

<https://leetcode.com/problems/two-sum-ii-input-array-is-sorted/>{: target="_blank"}

---

> I have talked about different versions of the "Two Sum" problem in my 
> [university web page](http://www.cs.uml.edu/~jlu1/doc/codes/findSum.html){: target="_blank"}

## Related Problems

[[LeetCode 1] Two Sum with unsorted array](http://127.0.0.1:4000/2016/02/13/leetcode1-Two-Sum-Unsorted/){:target="_blank"}

## Solution

The solution is just the simpler version of the $O(n)$ method with sorted array in my post above,
where we scan the array from the both ends.

## Implementation

<div class="code-title">
<span class="code-fold" id="fold-btn-python" onclick="$use('fold-body-python', 'fold-btn-python')">[-]</span>
Python code accepted by LeetCode OJ
</div>

~~~ python
class Solution(object):
    def twoSum(self, numbers, target):
        """
        :type numbers: List[int]
        :type target: int
        :rtype: List[int]
        """
        low = 0
        high = len(numbers) - 1
        
        while low < high:
            s = numbers[low] + numbers[high]
            if s == target:
                return [low + 1, high + 1]
            elif s < target:
                # Need to increase low
                tmp = numbers[low]
                low += 1
                while numbers[low] == tmp and low < high:
                    low += 1
            elif s > target:
                # Need to decrease high
                tmp = numbers[high]
                high -= 1
                while numbers[high] == tmp and low < high:
                    high -= 1
        # Not found
        return None
~~~
{: #fold-body-python}


