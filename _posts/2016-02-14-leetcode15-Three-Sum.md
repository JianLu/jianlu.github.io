---
layout: post
title: "[LeetCode 15] 3Sum"
date: 2016-02-14 21:00:14
description: Solution of Leetcode Question 15
categories: [Programming]
tags: [leetcode]
math: true
---

Question Link:

<https://leetcode.com/problems/3sum/>{: target="_blank"}

----

## Solution

The 3Sum problem can be solved by using the algorithm of 2Sum,
I have talked different cases of the 2Sum problem in my 
[old post](http://www.cs.uml.edu/~jlu1/doc/codes/findSum.html){: target="_blank"}.
In this problem, I would like to use the solution for the case where the given array is sorted.

The basic method is, we sort the given array first (because the requested triplet must be in non-descending order).
Suppose the array is $${e_1, e_2, ..., e_n}$$, for each element $$e_i$$, we find all pairs $$(e_j, e_k)$$
where $$e_k + e_j = - e_i$$ and $$k < j$$ by using 2Sum algorithm.

Since the array is sorted, the time cost of the algorithm is $$O(n^2)$$.

## Optimization

### Duplicate scan

We can skip the scanned number if there are many same numbers in the sorted array,
to reduce the number of scans.
Since the problem requires to remove the duplicated triplets and
each triplet is in non-descending order, we can have following conclusion:
for the triplet $$<a, b, c>$$, if we already scan the value for the position a,
we don't need to scan that value for the position a, and same to position b and c.

### Boundary trim

We can trim the sorted array to reduce the number of scans.
Suppose the array is $${e_1, e_2, ..., e_n}$$.
If $$e_1 + e_{n-1} + e_n < 0$$, then $$e_1$$ can be removed from the array because it is too small to be included in the result triplet.
Similarly, if $$e_1 + e_2 + e_n > 0$$, $$e_n$$ can be removed because it is too large.
The trim process can repeat recursively until neither of the boundary elements cannot be removed any more.

## Implementation

<div class="code-title">
<span class="code-fold" id="fold-btn-python" onclick="$use('fold-body-python', 'fold-btn-python')">[-]</span>
Python code accepted by LeetCode OJ
</div>

~~~ python
class Solution(object):
    def threeSum(self, nums):
        """
        :type nums: List[int]
        :rtype: List[List[int]]
        """
        # Sort the numbers first
        nums.sort()

        # Find the valid range
        # For the smallest number a1, a2, any number b > -(a1 + a2), cannot be in result
        # For the largest number b1, b2, any number a < -(b1 + b2), cannot be in result
        head = 0
        tail = len(nums) - 1

        while tail - head > 1:
            # Remove invalid largest numbers
            max_value = - (nums[head] + nums[head+1])
            new_tail = tail
            while nums[new_tail] > max_value and new_tail - head > 1: 
                new_tail -= 1
            # Remove invalid smallest numbers
            min_value = - (nums[tail] + nums[tail-1])
            new_head = head
            while nums[new_head] < min_value and tail - new_head > 1:
                new_head += 1
            # Stop when no numbers can be removed
            if new_tail == tail and new_head == head:
                break
            else:
                head = new_head
                tail = new_tail
        
        # Find a + b + c = 0
        res = []
        if tail - head < 2:
            return []
        # Make sure first ai will be checked
        a = nums[head] - 1
        for ai in xrange(head, tail - 1):
            # If number nums[ai] already checked
            if nums[ai] == a:
                continue
            # Find a + b + c = 0, where a <= b <= c
            a = nums[ai]
            # 2Sum solution for the sorted array case
            bi = ai + 1
            ci = tail
            b = nums[bi] - 1
            c = nums[ci] + 1
            while bi < ci:
                b = nums[bi]
                c = nums[ci]
                # Case 0: b = c
                if b == c:
                    if a + b + c == 0:
                        res.append([a, b, c])
                    break

                # Otherwise b < c
                print ai, bi, ci, " - ", a, b, c
                # Case 1: a + b + c = 0 found
                # Case 2: a + b + c < 0, b is too small
                # Case 3: a + b + c > 0, c is too large
                if a + b + c == 0:
                    res.append([a,b,c])
                    b += 1
                    while nums[bi] == b:
                        bi += 1
                    ci -= 1
                    while nums[ci] == c:
                        ci -= 1
                elif a + b + c < 0:
                    bi += 1
                    while nums[bi] == b:
                        bi += 1
                else:
                    ci -= 1
                    while nums[ci] == c:
                        ci -= 1

        return res
~~~
{:#fold-body-python}