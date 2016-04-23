---
layout: post
title: "[LeetCode 235] Lowest Common Ancestor of a Binary Search Tree"
date: 2016-04-22 17:58
description: Solution of Leetcode Question 235
categories: [Programming]
tags: [leetcode, binary search tree]
---

Question Link:

<https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-search-tree/>{: target="_blank"}

---

## Assumption: duplicate excluded

We would like to assume that there is no two nodes with the same key in the BST,
which means the BST is defined with the following strict recursive condition: `left < root < right`.

If the duplicate is allowed (there are more than one nodes with the same key),
this problem becomes the binary tree version.

## Algorithm

With the assumption above, we can solve this problem in more efficient way.
Suppose the given two nodes are `P` and `Q`.
Without loss of generality, we let `P.val < Q.val`.
We can find the lowest common ancestor of `P` and `Q` by traversing the BST from the root and for each visited node `N`:

1. if `N == P`, then `N` should be the answer;
2. if `N == Q`, then `N` should be the answer;
3. if `P.val < N.val < Q.val` (duplicate excluded), then `N` should be the answer;
4. Otherwise, the answer should be in one of `N`'s subtree:
    * Traverse `N = N.left`  if `N` is larger than `P` and `Q`, or
    * Traverse `N = N.right` if `N` is smaller than `P` and `Q`.

However, in the worst case, the time complexity is still `O(n)`, but it could be `O(lgn)` in average especially when the tree is balanced.

## C++ Implement

The C++ code is as follows

<div class="code-title">
<span class="code-fold" id="fold-btn-cpp" onclick="$use('fold-body-cpp', 'fold-btn-cpp')">[-]</span>
C++ code accepted by LeetCode OJ
</div>

~~~ cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
        while (root != p && root != q)
        {
            if (root->val > p->val) {
                if (root->val > q->val) root = root->left;
                else break;
            }
            else {
                if (root->val < q->val) root = root->right;
                else break;
            }
        }
        return root;
    }
};
~~~
{: #fold-body-cpp}

