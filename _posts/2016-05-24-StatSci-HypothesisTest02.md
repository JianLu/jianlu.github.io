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

## Assumptions

1. Each value is sampled independently from each other value;
2. The values are sampled from a normal distribution.

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

# Two-sample Testing

It is much common for a researcher to be interested in the difference between two means
than in the specific values of the means themselves.
This section covers how to test for differences between means of two separate groups of observations.
That is, we have two samples $$ X \sim norm(mean = \mu_X, sd = \sigma_X) $$ and $$ Y \sim norm(mean = \mu_Y, sd = \sigma_Y) $$, distributed independently.
We would like to know whether $$X$$ and $$Y$$ come from the same population distribution.

## Assumptions

1. The two samples have the same variance.
2. The two samples are normally distributed.
3. Each value is sampled independently from each other value.

The equal variance assumption can be relaxed as long as both sample sizes $$n$$ and $$m$$ are large.
The normality assumption can be relaxed as long as the population distributions are not highly skewed.
However, if one (or both) samples is small, then the test does not perform well.

## Hypothesis

The hypothesis testing is defined as:

$$ H_0: \mu_X = \mu_Y $$

$$ H_1: \mu_X \ne \mu_Y $$

## Test Statistic

The test statistic is:

$$ t = \frac{ \text{statistic} - \text{hypothesized value}} {\text{standard error of the statistic}} $$

If the variances of the populations are known, then the test statistic is:

$$ Z = \frac { (\bar{X} - \bar{Y}) - 0 } {\sqrt{ {\sigma_X}^2 / n + {\sigma_Y}^2 / m }}  \sim norm(mean = 0, sd = 1) $$

The test statistic should follow the standard normal distribution under $$ H_0 $$.

In the most case, the variances are not known.

$$ T = \frac { (\bar{X} - \bar{Y}) - 0 } {\sqrt{ {S_X}^2 / n + {S_Y}^2 / m }} $$

If either of the sample sizes is small (generally less than 30),
the test value should follow the t-distribution with $$ (n - 1) + (m - 1) $$ degrees of freedom
instead of the standard normal distribution.


## Examples

Suppose we have two samples:

~~~ R
# Generate data
g1 <- c(2, 3, 4, 5)
g2 <- c(2, 4, 6)
~~~

It is usually good to construct side-by-side boxplots,
which gives a visual comparison of the samples and helps to identify departures from the test’s assumptions.

~~~ R
df <- data.frame(group = c(rep(1, length(g1)), rep(2, length(g2))), value = c(g1, g2))
boxplot(value ~ group, df)
~~~

Then, we calculate the test statistic:

~~~ R
# Test statistic: difference of means
v <- mean(g1) - mean(g2)
# [1] -0.5
~~~

Next step is to calculate the standard error of the statistic:

~~~ R
# Standard error of the statistic
s <- sqrt(sd(g1) / length(g1) + sd(g2) / length(g2))
# [1] 0.9946936
~~~

Finally, calculate the probability:

~~~ R
tt <- v / s
## [1] -0.5026674
# Probablity of getting tt
dt(tt, df =5)
# [1] 0.3274179
# One-tail test: Pr(t < -0.5026674)
pt(tt, df =5)
# [1] 0.3182754
# Two-tail test: Pr(t < -0.502664 or t > 0.502664)
pt(tt, df = 5) + pt(-tt, df = 5, lower.tail = FALSE)
# [1] 0.6365508
~~~

### Computation of Standard Error

However, there is another way to estimate the standard error of statistic $$ S $$, especially when the sample sizes are not equal.
We can use MSE, the estimate of variance, with the following formula:

$$
\text{SSE} = \sum{(X-M_1)^2} + \sum{(X-M_2)^2} \\

\text{MSE} = \frac{\text{SSE}}{\text{df}} \\

S = \sqrt{\frac{2 \text{MSE}} {n}}
$$

# Paired Samples Testing

Note that one of the assumption is that each value is sampled independently,
which means each subject must provide only one scores.
If a subject provides two scores, then the scores are not independent.
In this section, we study the test where the samples are either paired up in some way or the same sample are used twice.

## Example

Suppose a researcher wants to see whether teaching students to read using a computer game gives better results than teaching with a tried-and-true phonics method.
She randomly selects 20 students and puts them into 10 pairs according to their reading readiness level, age, IQ, and so on.
She randomly selects one student from each pair to learn to read via the computer game,
and the other learns to read using the phonics method.
At the end of the study, each student takes the same reading test.

The data are in pairs, but you’re really interested only in the difference in reading scores
(computer reading score – phonics reading score) for each pair, not the reading scores themselves.
So, you take the difference between the scores for each pair,
and those paired differences make up your new dataset to work with.
If the two reading methods are the same, the average of the paired differences should be 0.
If the computer method is better, the average of the paired differences should be positive.

## Hypothesis

$$ H_0: \mu_d = 0 $$

$$ H_1: \mu_d \ne 0 $$

$$ \mu_d $$ is the mean of the paired difference.

## Test Statistic

The formula for the test statistic for paired differences is:

$$ T = \frac {\bar{d} - \mu_d} {S_d / \sqrt{n}} $$

When calculating probability value, if the dataset is small,
we should use t-distribution with $$n-1$$ degrees of freedom instead of the standard normal distribution.
