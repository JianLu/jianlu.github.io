---
layout: post
title: "[LeetCode 133] Clone Graph"
date: 2014-04-07 01:42:57
description: Solution of Leetcode Question 133
categories: [Programming]
tags: [leetcode, BFS, DFS, python]
---

Question Link:

<http://oj.leetcode.com/problems/clone-graph/>{: target="_blank"}

---

## Solution

The problem is very similar to 
[[LeetCode 138] Copy List with Random Pointer](/2014/04/06/leetcode138-Copy-List-With-Random-Pointer/){:target="_blank"},
we just traverse every node in the Graph and use hashtable to keep track the visited node and its new copy.
In the second pass, we scan all original nodes, and link the new created nodes based on the original edge information.
We can use either BFS or DFS to traverse the node.
 
## Implementation

I implemented both BFS and DFS approaches here.
I did BFS iteratively which is always my choice,
because I think the iterative solutions are always faster than recursive ones.
I implemented DFS recursively because the code is very clean.

<div class="code-title">
<span class="code-fold" id="fold-btn-python" onclick="$use('fold-body-python', 'fold-btn-python')">[-]</span>
Python code accepted by LeetCode OJ
</div>

~~~ python
# Definition for a undirected graph node
# class UndirectedGraphNode:
#     def __init__(self, x):
#         self.label = x
#         self.neighbors = []

class Solution:
    # @param node, a undirected graph node
    # @return a undirected graph node
    def cloneGraph(self, node):
        # return self.recursiveDFS(node, [])
        return self.iterativeBFS(node)
    
    def iterativeBFS(self, node):
        if node is None:
            return None
        
        # Hashtable to map original nodes and new copy nodes
        seen = {}
        
        # Queue for BFS
        q = [node]
        while q:
            n = q.pop()
            # Create a new copy of n
            seen[n] = UndirectedGraphNode(n.label)
            # BFS children
            q += [x for x in n.neighbors if x not in seen]
        
        # Fill in the neighbors of new nodes based on the hashtable
        for k,v in seen.iteritems():
            v.neighbors = [seen[x] for x in k.neighbors]
        
        # Return
        return seen[node]
        
    def recursiveDFS(self, node, seen):
        if node is None:
            return None
        
        # Recurisve DFS
        if node not in seen:
            d[node] = UndirectedGraphNode(node.label)
            d[node].neighbors += [self.dfs(n, seen) for n in node.neighbors]
            
        # Return
        return d[node]
~~~
{: #fold-body-python}


However, I found that there is no big difference between iterative and recursive methods in term of LeetCode evaluation.

The iterative BFS approach beats 99.58% submissions, but it is just a little better than the recursive DFS which beats 96.28%.

![Iterative](/images/showoff_20140407_iterative.PNG)

![Recursive](/images/showoff_20140407_recursive.PNG)

## Effective Python

One more thing I want to mention here is:

> Avoid to use for loop on the list as possible as you can.
> Use python list comprehension instead which is much much faster than for loop on list.

Here is a comparison: 

[Loop vs Map vs List Comprehension](http://leadsift.com/loop-map-list-comprehension/){:target="_blank"}

[Python Speed Peformance Tips](https://wiki.python.org/moin/PythonSpeed/PerformanceTips){:target="_blank"}

For my case here, I have an initial BFS implementation which beats ONLY 4%.
All the logic is same, but I use for loop on the list and append the elements one by one.
The performance is terrible!

<div class="code-title">
<span class="code-fold" id="fold-btn-bad" onclick="$use('fold-body-bad', 'fold-btn-bad')">[-]</span>
Slow Implementation with For Loop on List
</div>

~~~ python
# Definition for a undirected graph node
# class UndirectedGraphNode:
#     def __init__(self, x):
#         self.label = x
#         self.neighbors = []

class Solution:
    # @param node, a undirected graph node
    # @return a undirected graph node
    def cloneGraph(self, node):
        if node is None:
            return node
            
        # Use a hashtable to map original nodes to new nodes
        mapping = {}
        
        # BFS from the given node
        Q = [node]
        while Q:
            n = Q.pop()
            # Create a new copy of n for the first time
            mapping[n] = UndirectedGraphNode(n.label)
            # BFS n's children
            for x in n.neighbors:
                if x not in mapping.keys():
                    Q.append(x)
        
        # Add neighbors for the new created nodes
        for k, v in mapping.items():
            for oc in k.neighbors:
                v.neighbors.append(mapping[oc])
        
        # Return
        return mapping[node]
~~~
{: #fold-body-bad}

Yoy may notice that I have two for loops, one is used to add nodes into queue during BFS,
the other is add neighbor nodes to the new created nodes.
Both of them will drop the performance very much!