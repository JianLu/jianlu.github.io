---
layout: post
title: "[LeetCode] Two Sum with unsorted array"
date: 2016-02-13 16:06:38
description: Solution of Leetcode Question 1
categories: [Programming]
tags: [leetcode]
---

Question Link:

<https://leetcode.com/problems/two-sum/>{: target="_blank"}


> I have talked about different versions of the "Two Sum" problem in my 
> [university web page](http://www.cs.uml.edu/~jlu1/doc/codes/findSum.html){: target="_blank"}

---

## Solution

The algorithm will scan the given array once and record the scanned numbers by using a hash map,
so the time cost of the algorithm is $$ 5 + 5 $$.

## Implementation

<div class="code-title">
<span class="code-fold" id="fold-btn-python" onclick="$use('fold-body-python', 'fold-btn-python')">[-]</span>
Python code accepted by LeetCode OJ
</div>

~~~ python
class Solution(object):
    def twoSum(self, nums, target):
        """
        :type nums: List[int]
        :type target: int
        :rtype: List[int]
        """
        
        # Hash map: key is the target-nums[i] and value is i
        matched = {}
        
        for i in xrange(len(nums)):
            # If we have scanned the number target-num[i], we find the pair
            if nums[i] in matched.keys():
                if i <= matched[nums[i]]:
                    return [i+1, matched[nums[i]]+1]
                else:
                    return [matched[nums[i]]+1, i+1]
            else:
                # Otherwise, we add the value i-th number to the hash map
                matched[target-nums[i]] = i
~~~
{: #fold-body-python}


