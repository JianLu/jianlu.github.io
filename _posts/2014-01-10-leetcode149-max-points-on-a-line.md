---
layout: post
title: "[LeetCode 149] Max Points on a Line"
date: 2014-01-10 10:53
description: Solution of Leetcode Question 149
categories: [Programming]
tags: [leetcode, C++]
math: true
---

Question Link:

<a href="https://leetcode.com/problems/max-points-on-a-line/" target="_blank">
https://leetcode.com/problems/max-points-on-a-line/</a>

---

## Brute-force Solution

By given $$n$$ points, there are most $$n^2$$ different lines in the 2D space.
To solve the problem, we just check all possible lines, and for each line count the points on it.
This native approach takes $$O(n^3)$$ time.

## Using Hash Table

We can use a hash table to store the lines to improve the brute-force solution,
where the time complexity can be $$O(n^2)$$.

## More Fancy Algorithm

There is another way to solve the problem by converting the 2D space to its dual space.
In a work, map the (point, line)-space into a (line, point)-space.
The $$n$$ points are converted into $$n$$ lines in the dual space.
Then, the intersection of lines in the dual space corresponds to the lines passing through those points in the original space.
Therefore, instead of "finding the line pass through most points by given $$n$$ points",
we are asked to solve problem of "finding the intersection points that most lines pass through by given $$n$$ lines".
However, this algorithm is still in $$O(n^2)$$ time.


## Dynamic Programming

Finally, I decide to solve the problem using **Dynamic Programming**.
Let $$M[n]$$ be the solution to the problem with points $$P_1, P_2, \dots, P_n$$.
Then we have the recursive function as follows:

$$
\begin{align}
M[n] &= n, & n <= 2 \\
M[n] &= max(BestSolutionPassingPoint(P_n, \{P_1, \dots, P_{n-1} \})), & otherwise
\end{align}
$$

Suppose we already have points $$P_1, \dots, P_{n-1}$$ and the solution line with $$M[n-1]$$ points,
there are only two non-overlapped cases: (1) The solution line passes through the point $P_n$;
or (2) The solution line does not pass through the point $$P_n$$.
For case (1), we only need to find the number of points lie on the line that also passes through $$P_n$$
(function `BestSolutionPassingPoint`, which is of $$O(n^2)$$ time by using hash technique).
For case (2), the solution of $$P_1, \dots, P_{n-1}$$ is just the solution for $$P_1, \dots, P_n$$.

### Line representation and Hash

The key of the algorithm is how to represent the line passing through two points and hash it.
Let $$X(x_1, y_1)$$ and $$Y(x_2, y_2)$$ be two points, then we can represent the line $$XY$$ as $$kx + y = d$$.

> Notice that this representation has a drawback that it cannot cover the case of $$x1==x2$$.
> Another representation is to use ax+by=c, but personally I do not like it since it requires gcd and more values to hash.

Therefore, the pair $$(k,d)$$ is unique to a line, which can be used as the hash key.
In DP approach, all lines we concern are passing through a same point.
Then we can consider this point as the origin, and all lines can represented as $$y=kx$$ or $$x=c$$ for lines parallel to the x-axis.
So we can use the value of $k$ only as the hash key. Note that the line $$x=c$$ should be considered separately.

### Duplicate points

We have to consider the case that there exist same points in the point set.
If we use brute-force approach, we need to scan the points and count the duplicates first.
For DP approach, it becomes much easier to handle the duplicates.
To calculate `BestSolutionPassingPoint(P, S)`, if there exist same point to the point $$P$$ in points set $$S$$, lets say $$Q$$.
What we need to do is just to add 1 to every count since all lines must pass through $$Q$$ since they passes through $$P$$.

### C++ Implement

The C++ code should like:

<div class="code-title">
<span class="code-fold" id="fold-btn-cpp" onclick="$use('fold-body-cpp', 'fold-btn-cpp')">[-]</span>
C++ code of LeetCode 150
</div>

~~~ cpp
/**
 * Definition for a point.
 * struct Point {
 *     int x;
 *     int y;
 *     Point() : x(0), y(0) {}
 *     Point(int a, int b) : x(a), y(b) {}
 * };
 */
#include <unordered_map>

class Solution {
public:
    static const int EPSILON = 1000000;  // float precision purpose
    std::unordered_map<int, int> lines;  // We use the unordered_map, key is the slope of the line and value is the count of points
    int find_sub_max_points(vector<Point> &points, int last) {
        lines.clear();  // Clear the hash table
        int num_same_x = 0;  // count the number of points with same x-coordinate of Point[last]
        int num_same_point = 1;
        const int x0 = points[last].x;
        const int y0 = points[last].y;
        int x,y,tmp;
        int res = 0;
        for (int i = last-1; i >= 0; i--) {
            x = points[i].x;
            y = points[i].y;
            if (x == x0) {
                if (y == y0) num_same_point++;
                else if(++num_same_x > res)
                    res = num_same_x;
            }
            else {
                tmp = int( (y-y0) * EPSILON ) / (x-x0);
                if (++lines[tmp] > res) res = lines[tmp];
                /* the line above is equal to following lines
                std::unordered_map<int,int>::const_iterator got = lines.find (tmp);
                if (got == lines.end())
                    lines[tmp] = 1;
                else
                    lines[tmp]++;
                if (lines[tmp]>res) res = lines[tmp];
                */
            }
        }
        return res+num_same_point; // Point[last] itself should be counted
    }
    int maxPoints(vector<Point> &points) {
        int n = points.size();
        if (n <= 2) return n;
        int res = 2;
        int tmp = 0;
        for(int i=2; i<n; i++) {
            tmp = find_sub_max_points(points, i);
            if (tmp > res) res = tmp;
        }
        return res;
    }
};
~~~
{: #fold-body-cpp}

## C++ Basics

In C++, we can use `<unordered_map>` as hash table data structure.
And there is a very quick way to do the following update:

~~~ cpp
if (++lines[tmp] > res) res = lines[tmp];
~~~

if the key is not in contained in the hash table, then set the key with value 1;
otherwise, add 1 to the key's value.
This update can be done in one line since the `HashTable[key]` is set to 0 if the key does not exist.