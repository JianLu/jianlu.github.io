---
layout: post
title: "[LeetCode] Evaluate Reverse Polish Notation"
date: 2014-01-09 11:10
description: Solution of Leetcode Question 150
categories: [Programming]
tags: [leetcode, C++]
---

Question Link:

<a href="https://leetcode.com/problems/evaluate-reverse-polish-notation/" target="_blank">
https://leetcode.com/problems/evaluate-reverse-polish-notation/</a>

---

## Pseudo-code

According to the wiki page provided in the problem description,
the algorithm for evaluating any postfix expression is fairly straightforward:

~~~
while there are input tokens left
     read the next token from input
        if the token is a value (operand)
            push it into the stack
        else (the token is an operator)
            let n be the number of the arguments of the operator
            if there are fewer than n values in the stack
                return ERROR (non-sufficient operands in the expression)
            else
                pop the top n values from the stack
                evaluate the expression with the operator and operands
                push the result, if any, back into the stack
if there is only one value in the stack
    return the value as the result
else
    return ERROR (too many operands in the expression)
~~~

## Implement

The problem is much simpler than the pseudo-code, because there are only four operators.
The only issue is to identify the operators and numbers (integer in this case).
Therefore, what we need is as follows:

1. A stack structure;
2. some functions on string that can determine the string is an integer or an operator.

The C++ code is as follows:

<div class="code-title">
<span class="code-fold" id="fold-btn-cpp" onclick="$use('fold-body-cpp', 'fold-btn-cpp')">[-]</span>
C++ code of LeetCode 150
</div>

~~~ cpp
#include <stack>
#include <ctype.h>

class Solution {
public:
    int evalRPN(vector<string> &tokens) {
        // The stack to store operators (int values) only
        std::stack<int> res;
        int x,y;
        // Iterate the vector from left to right
        for(std::vector<string>::iterator it = tokens.begin(); it != tokens.end(); it++)
        {
            // If operator, pop and evaluate
            if ( (*it).length() == 1 && !isdigit((*it)[0]) ) {
                y = res.top();
                res.pop();
                x = res.top();
                res.pop();
                if (*it == "+") x += y;
                else if (*it == "-") x -= y;
                else if (*it == "*") x *= y;
                else if (*it == "/") x /= y;
                res.push(x);
            }
            // If operand, push it into the stack
            else res.push( atoi((*it).c_str()) );
        }
        // The only value left in the stack is the final result
        return res.top();
    }
};
~~~
{: #fold-body-cpp}

## C++ Basics

The standard library for stack is `std::stack<T> S`.
The function `stack::pop()` does not return the value but void value,
to get and pop, you need to use `stack::top()` first to get the value and then use `stack::pop()` to pop.

Use vector iterator to traverse all elements in the vector:

~~~
    // Assume list is a variable of vector<T>
    for(std::vector<T>::iterator it = list.begin(); it != list.end(); it++)
    {
       /*
        * Do something on each element *it,
        * where it is a pointer type
       */
    }
~~~

C++ string functions:

* `isdigit(char)`: check if a character is a number
* `string::c_str()`: convert a string to `char*`
* `atoi(char*)`: convert a list of char to a integer
