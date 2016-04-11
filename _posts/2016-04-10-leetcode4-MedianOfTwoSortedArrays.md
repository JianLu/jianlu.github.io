---
layout: post
title: "[LeetCode] Median of Two Sorted Arrays"
date: 2016-04-10 20:57:11
description: Solution of LeetCode Question 4
categories: [Programming]
tags: [leetcode]
---

Question Link:

<https://leetcode.com/problems/median-of-two-sorted-arrays/>{: target="_blank"}

---

## Conquer-and-divide

Because the arrays are sorted, intuitively I thought the problem should be very similar to **Binary Search**,
which uses the "Conquer-and-divide" idea.
I tried to compare the medians of two arrays, and trim left and right with the same number of elements until each array has at most 1 elements.
However, I failed to provide a good solution because the logic of deciding trim size is too complicated.

## Find K-th Value of Two Sorted Arrays

Still inspired by "Conquer-and-divide", this time, I transfer the problem into another problem.
Instead of find the median of the two sorted arrays, I try to find the *K*-th value of two sorted arrays
where *K = (N1 + N2) / 2* and *N1* and *N2* are lengths of two arrays.


## Solution

If (N1 + N2) is odd, the answer is the solutino of finding $K+1$-th value of the two sorted arrays.
Otherwise, the answer is the average of *K*-th and *K+1*-th values (*K = (N1 + N2) / 2*).

We should also take care of some boundary conditions. 
For each iteration, the length of the shorter array could be smaller than *K/2*,
then the trim size should be `min(n1, k/2)`.

The python implement is as follows.

<div class="code-title">
<span class="code-fold" id="fold-btn-python" onclick="$use('fold-body-python', 'fold-btn-python')">[-]</span>
Python code accepted by LeetCode OJ
</div>

~~~ python
class Solution(object):

    def findKthSortedArrays(self, nums1, start1, n1, nums2, start2, n2, k):
        """
        Find the K-th number in two sorted arrays nums1[low1...high1] and nums2[low2...high2],
        where high1 = low1 + n1 - 1 and high2 = low2 + n2 - 1
        """
        if n1 == 0:
            return nums2[start2 + k - 1]
        if n1 > n2:
            return self.findKthSortedArrays(nums2, start2, n2, nums1, start1, n1, k)
        if k == 1:
            return min(nums1[start1], nums2[start2])
        k1 = min(n1, k >> 1)
        k2 = k - k1
        m1 = start1 + k1 - 1
        m2 = start2 + k2 - 1
        if nums1[m1] == nums2[m2]:
            return nums1[m1]
        elif nums1[m1] < nums2[m2]:
            return self.findKthSortedArrays(nums1, start1 + k1, n1 - k1, nums2, start2, n2, k - k1)
        else:
            return self.findKthSortedArrays(nums1, start1, n1, nums2, start2 + k2, n2 - k2, k - k2)

    def findMedianSortedArrays(self, nums1, nums2):
        """
        :type nums1: List[int]
        :type nums2: List[int]
        :rtype: float
        """
        n1 = len(nums1)
        n2 = len(nums2)
        
        # Only used for beat more submissions...
        if n1 == 1 and n2 == 1:
            return (nums1[0] + nums2[0]) / 2.0
            
        if (n1 + n2) & 1:
            # ood
            k = ((n1 + n2) >> 1) + 1
            return self.findKthSortedArrays(nums1, 0, n1, nums2, 0, n2, k)
        else:
            k = (n1 + n2) >> 1
            v1 = self.findKthSortedArrays(nums1, 0, n1, nums2, 0, n2, k)
            v2 = self.findKthSortedArrays(nums1, 0, n1, nums2, 0, n2, k + 1)
            return (v1 + v2) / 2.0

~~~
{: #fold-body-python}
