---
layout: post
title: "[LeetCode 3] Longest Substring Without Repeating Characters"
date: 2016-02-22 16:34:14
description: Solution of LeetCode Question 3
categories: [Programming]
tags: [leetcode]
---

Question Link:

<https://leetcode.com/problems/longest-substring-without-repeating-characters/>{: target="_blank"}

---

## Qualified Suffix

Assuming the string is `s[0,1, ...n-1]`, instead of finding the qualified substring, we find the qualified suffix.
However, the longest suffix strings for each `s[i]` will give the correct result to the original problem.

Lets define `<a, b>` is a duplicate pair where `s[a] == s[b]`, then it is not hard to show that,
for the character `s[i]`, the longest suffix strings should be `s[j..i]`
where `j` is the largest `b` value for all duplicate pairs before `s[i]`.

## Implementation

Then we can have following python code.
We use hash table to record the last occurrence of each character to keep track the duplicates.
The code is very neat and efficient (first time my code beats more than 90%... :p).


<div class="code-title">
<span class="code-fold" id="fold-btn-python" onclick="$use('fold-body-python', 'fold-btn-python')">[-]</span>
Python code accepted by LeetCode OJ
</div>

~~~ python
class Solution(object):
    def lengthOfLongestSubstring(self, s):
        """
        :type s: str
        :rtype: int
        """
        n = 0
        last_occurrence = {}
        last_duplicate = -1
        for i in xrange(len(s)):
            j = last_occurrence.get(s[i], -1)
            if last_duplicate < j:
                last_duplicate = j
            if n < i - last_duplicate:
                n = i - last_duplicate
            last_occurrence[s[i]] = i

        return n
~~~
{: #fold-body-python}