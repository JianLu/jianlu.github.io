---
layout: post
title: "[Statistics] Hypothesis Test 01"
date: 2016-05-23 14:57:29
description: Self learning on Hypothesis Test
categories: [Statistics, 文渊台]
tags: [hypothesis test, R]
math: true
---

* TOC
{:toc}

# Introduction

## Definition

When we finish a experiment and have some experimental results,
a natural question is whether the result could have occurred by chance?
A *statistical hypothesis test* (or *hypothesis test*) is a statistical procedure
for testing whether chance is a plausible explanation of the the experimental finding.

A *statistical hypothesis* is a hypothesis that is testable on the basis of observing a process that is modeled via a set of random variables.

## Example: God of Dice

Firstly, I will take an example of dice game, where we roll the number between 1 and 6, and guess what number will be rolled. Let's say I have the ability to predict the rolled number. Therefore, the hypothetical experiment to determine whether I can predict the rolled number. In each test, I provide a number between 1 and 6, and then roll the dice to check if it is same to what I provided. Let's say I was correct on 7 of 10 roll tests.

The experiment result does NOT prove that I can predict the number in the dice game. Maybe today is my lucky day, and I just guessed the right number 7 out of 10 times. Then the next question is how plausible is the explanation that I was just lucky?

Let's do a math about the hypothesis that I am just guessing 7 or more times out of 10 rolls. By guessing, each test, I have 1/6 probability to guess the right number, and probability of the experiment outcome be calculated from the binomial distribution. This probability can be computed by using the following R command:

~~~ r
# Calculate the probability of P[X>=7] (or P[X>6])
pbinom(7 - 1, size = 10, prob = 1/6, lower.tail = FALSE)
~~~

The result is `0.0002675215`, which is a pretty low probability,
and hence I could be very very lucky to guess correct 7 or more times out of 10 rolls.
We cannot prove that the hypothesis (I am just guessing) is false,
but we can reject it because the occurrence probability is too small.
Therefore, there is a strong evidence that I can predict the roll number in a dice game. :)

## The Probability Value

It is very important to understand precisely what the probability values (P-value) mean.
Note that in the example above, `0.0002675215` is the probability that I guess 7 or more times of 16 rolls.
It is NOT the probability that I cannot predict the rolled number.
The number is just the probability of a certain outcome (7 or more out of 16),
assuming a certain state of the world (I was just guessing).
It is NOT the probability that a state of the world is true.

In statistics, it is conventional to refer to possible states of the world as hypotheses since they are hypothesized states of the world.
Using this terminology, the P-value is the probability of an outcome given the hypothesis,
not the probability of the hypothesis given the outcome.
This is not to say that we ignore the probability of the hypothesis.
If the probability of the outcome given the hypothesis is sufficiently low,
we have evidence that the hypothesis is false. However, we do not compute the probability that the hypothesis is false.

Again, in the experiment above, the hypothesis is that I cannot predict the roll number and I was just guessing,
the probability value is low, thus providing evidence that I can predict the roll number.
I have not compute the probability that I can predict the roll number.

## Null Hypothesis and Alternative Hypothesis

Every hypothesis test contains two hypothesis: *null hypothesis* and *alternative hypothesis* (or *research hypothesis*).

The null hypothesis $$H_0$$ is a hypothesis which is due to chance, or "nothing" hypothesis,
whose interpretation could be that nothing has changed, there is nothing special taking place, etc..
The alternative hypothesis $$H_1$$ is researcher's own claim on the experiment.
Typically, the alternative hypothesis is the opposite of the null hypothesis.
If the null hypothesis is rejected, then the alternative (to the null) hypothesis is accepted.

For our example, the null hypothesis is:

> $$H_0$$: I cannot predict the dice game, just guess roll number.

And the alternative hypothesis is:

> $$H_1$$: I can predict the dice game.

# Significance Test

## Significance Level

We know that a low probability value casts doubt on the null hypothesis,
then the question is how low the probability value is to reject the null hypothesis?
It is conventional to reject the null hypothesis if the probability value is less than 0.05.
More conservative researchers may reject if the probability value is less than 0.01.
The probability value below which the null hypothesis is rejected is called the *significance level*,
denoted by $$\alpha$$ level.

When the null hypothesis is rejected, the effect is said to be *statistically significant*.
It is very important to keep in mind that statistical significance only means that the null hypothesis of exactly no effect is rejected;
it does not mean that the effect is important, which is what "significant" usually means.

## Type I and II Errors

Every time we make a decision, it is possible to be wrong. To reject a null hypothesis, we could make two types errors:

1. **Type I Error**: if we reject $$H_0$$ when in fact $$H_0$$ is true.
2. **Type II Error**: if we fail to reject $$H_0$$ when in fact $$H_1$$ is true.

A Type I error occurs when a significance test results in the rejection region of the null hypothesis,
so the Type I error rate is affected by the $$\alpha$$ level:
the lower the $$\alpha$$ level the lower the Type I error rate.
It might seem that $$\alpha$$ is the probability of a Type I error. However, this is not correct.
Instead, $$\alpha$$ is the probability of a Type I error given that the null hypothesis is true.
If the null hypothesis is false, then it is impossible to make a Type I error.

Unlike a Type I error, a Type II error is not really an error.
When a statistical test is not significant, it means that the data do not provide strong evidence that the null hypothesis is false.
Lack of significance does not support the conclusion that the null hypothesis is true.
A Type II error can only occur if the null hypothesis is false.

Type I errors are usually considered worse, and we design our statistical procedures to control the probability of making such a mistake.
Therefore, we want $$\alpha$$ to be small which conventionally means, say, $$\alpha = 0.05$$, or $$\alpha = 0.01$$.

## Approaches

One approach to conducting significance tests favored by R. Fisher uses probability value to reflect the strength of the evidence against the null hypothesis.
The strength is like:

1. Pr < 0.01: strong evidence that the null hypothesis is false;
2. 0.01 <= Pr < 0.05: still reject the null hypothesis with less confidence;
3. 0.05 <= Pr < 0.1: weak evidence to reject the null hypothesis;
4. Pr >= 0.1: failed to reject

The alternative approach (favored by Neyman and Pearson) is to specify an $\alpha$ level before analyzing data.
If the data analysis results in a probability value below the $\alpha$ level, then the null hypothesis is rejected;
Otherwise, the null hypothesis is not rejected.
According to this perspective, if a result is significant, then it does not matter how significant it is.
Moreover, if it is not significant, then it does not matter how close to being significant it is.

# Hypothesis Test Steps

There are four steps in a significance test:

1. Step 1: setting up the null hypothesis $$H_0$$ and alternative hypothesis $$H_1$$.
For a two-tailed test, the null hypothesis is typically that a parameter equals to zero
(e.g. $$ mu_1 - mu_2 = 0$$ or $$ mu_1 = mu_2 $$).
For a one-tailed test, the null hypothesis is either that a parameter is larger than or equal to zero ($$ mu_1 - mu_2 \ge 0$$)
or that a parameter is less than or equal to zero ($$ mu_1 - mu_2 \le 0 $$).
2. Step 2: Take a random sample of individuals from the population and calculate the sample statistics.
3. Step 3: computing p-value, which is the probability of obtaining sample statistic as different or more different from the parameter
specified in the null hypothesis given that the null hypothesis is true.
4. Step 4: comparing p-value with the $$\alpha$$ level and make your decision.
If the probability value is lower than the significance level, then reject the null hypothesis.
The lower the p-value, the more confidence to conclude the null hypothesis is false.
Failure to reject the null hypothesis does NOT support that null hypothesis is true, it just means that you cannot prove it is false.

# Misconception

The probability value is the probability that the null hypothesis if false.

> The probability value is the probability of a result as extreme or more extreme given that the nul hypothesis is true.
> It is the probability of data given the null hypothesis.

A low probability value indicates a large effect.

> A low probability value indicates that the sample outcome is very unlikely if the null hypothesis is true.
> A low probability value can occur with small effect sizes, particularly if the sample size is large.

A non-significant outcome means that the null hypothesis is probably true.

> A non-significant outcome means that the data do not prove that the null hypothesis is false.
