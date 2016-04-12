---
layout: post
title: "[LeetCode 1] Two Sum with unsorted array"
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
so the time cost of the algorithm is *O(1)*.

### Add number

For each scanned number *n*, the hash map stores the key-value-pair,
where the key is the matched number *n'* (*n + n' = target*), and the value is the number index.

### Match number

While scanning a new number *n1*, if *n* is in the hash map key,
that means there is scanned number matching with *n*, 
then we get the value of key *n1* which is the index of that scanned number.
If *n* is not in the hash map keys, just add *n* to the hash map for the further matchign check.


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


