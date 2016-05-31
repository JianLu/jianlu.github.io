---
layout: post
title: "[Statistics] Hypothesis Test 02 - Testing Means"
date: 2016-05-24 16:23:00
description: Self learning on Hypothesis Test
categories: [Statistics, 文渊台]
tags: [hypothesis test, R]
math: true
---

* TOC
{:toc}

# One Sample Testing

This section shows how to test the null hypothesis that the population mean is equal to some hypothesized value.
For example, suppose we have the quiz scores of students.

## Hypothesis

The question is whether the population mean of scores suggested that the quiz score is equal to 60.
So the hypothesis testing is defined as:

$$ H_0: \mu = 60 $$

$$ H_1: \mu \ne 60 $$

## Test Statistic and Formula

In the null hypothesis, we use the paired difference $$ \bar{x} - \mu_0 $$ as the test statistic,
and reject the null hypothesis if it is large.

There are two cases: 1) If $$ \sigma^2 $$ is known, samples should follow normal distribution (full population) under $$ H_0 $$:

$$ Z = \frac {\bar{X} - \mu_0} {\sigma / \sqrt{n}} \sim norm(mean = 0, sd = 1) $$

2) If $$ \sigma^2 $$ is not known, then samples should follow student's t-distribution under $$ H_0 $$:

$$ T = \frac {\bar{X} - \mu_0} {S / \sqrt{n}} \sim t(df = n-1) $$

where $$ S $$ is the standard deviation of the sample, which is also called *standard error*.

## Examples

Assume our sample is dataset of 30 student from class 1.
We generate the scores in normal distribution with mean 60 and standard deviation 10.

~~~ r
x <- as.integer(rnorm(30, mean = 60, sd = 15))
# [1] 59 15 57 56 83 66 48 63 44 58 45 62 54 53 73 73 59 57 49 38 67 55 90 44 60 63 61 67 42 44
~~~

Let's first calculate the p-value under case 1:

~~~ r
x.mean <- mean(x)
# Calculate Z value
z <- (60 - x.mean) / (15 / sqrt(30))
# Calculate the probability value using normal distribution density function
p <- dnorm(z)
# [1] 0.2044448
~~~

The probability value 0.2044 is not small enough so we fail to reject $$ H_0 $$.

However, in the real-world data analyses it is very rare that you would know the variance of the population $$ \sigma $$.
And the case 2 is the common case:

~~~ r
x.mean <- mean(x)
x.sd <- sd(x)
# Calculate T value
xT <- (60 - x.mean) / (x.sd / sqrt(30))
# Calculate the probability value using t-distribution density function
p <- dt(xT, df = (30 - 1))
# [1] 0.186752
~~~

We get the similar result that the probability is not small enough to reject $$H_0$$.

If we generate the sample following normal distribution with mean 30, the probability of $$ H_0 $$ is very small,
then we can reject the $$H_0$$ and conclude that the mean of the scores does not equal to 60.

~~~ r
x <- as.integer(rnorm(30, mean = 30, sd = 15))
x.mean <- mean(x)
x.sd <- sd(x)
xT <- (60 - x.mean) / (x.sd / sqrt(30))
p <- dt(xT, df = (30 - 1))
# [1] 8.632443e-10
~~~