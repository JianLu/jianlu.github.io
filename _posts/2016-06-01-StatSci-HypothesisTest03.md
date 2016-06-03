---
layout: post
title: "[Statistics] Hypothesis Test 03 - ANOVA"
date: 2016-06-01 15:22:20
description: Self learning on Hypothesis Test
categories: [Statistics, 文渊台]
tags: [hypothesis test, ANOVA, R]
math: true
---

* TOC
{:toc}

# Introduction

Analysis of Variance (ANOVA) is a statistical method used to test differences between two or more means.
It may seem odd that the technique is called “Analysis of Variance” rather than “Analysis of Means”.
As you will see, the name is appropriate because inferences about means are made by analyzing variance.

# ANOVA Design

In describing an ANOVA design, the term *factor* is a synonym of independent variable.
An ANOVA conducted on a design in which there is only one factor is called a *one-way ANOVA*.
If an experiment has two factors, then the ANOVA is called a *two-way ANOVA*.
The number of *levels* measure how many different values for each factor.

For example, suppose an experiment on the effects of age and gender on reading speed were conducted using
three age groups (8 years, 10 years, and 12 years) and the two genders (male and female).
The factors would be age and gender.
Age would have three levels and gender would have two levels.

So in the design, there would be a total of six different groups as show in the table below.

|----
| Group | Gender | Age |
|:-----:|:------:|:---:|
| 1 | Female | 8 |
| 2 | Female | 10 |
| 3 | Female | 12 |
| 4 | Male | 8 |
| 5 | Male | 10 |
| 6 | Male | 12 |
|----

# One Way ANOVA

## Hypothesis

The null hypothesis tested by ANOVA is that the population means for all $$k$$ conditions are the same.
This can be expressed as follows:

$$ H_0: \mu_1 = \mu_2 = \dots = \mu_k $$

And the alternative hypothesis could be:

$$ H_1: \text{at least one of the population mean is different from at least one other mean} $$

## Assumptions

The analysis of variance can be presented in terms of a linear model,
which makes the following assumptions about the probability distribution of the responses:

* Independence of observations – this is an assumption of the model that simplifies the statistical analysis.
* Normality – the distributions of the residuals are normal.
* Equality (or "homogeneity") of variances, called homoscedasticity — the variance of data in groups should be the same.

## Why "Variance"?

Recall that analysis of variance is a method for testing differences among means by analyzing variance.
The test is based on estimating and comparing the population variance ($$ \sigma^2 $$).
ANOVA computes two estimates of the population variance.

The first estimate is called **mean square error (MSE)**.
MSE is based on difference among scores within the groups,
which estimates $$\sigma^2$$ regardless of whether $$H_0$$ is true.

The second estimate is called **mean square between (MSB)**.
MSB is based on difference among sample means, which only estimate $$\sigma^2$$ when $$H_0$$ is true.
If the population means are not equal, then MSB estimates a quantity larger than $$\sigma^2$$.

Therefore, if MSB is much larger than MSE, then the population means are unlikely to be equal.
On the other hand, if the MSB is about the same as MSE,
then the data are consistent with the hypothesis that the population means are equal.

## Computing Estimates

Suppose we have 34 subjects in each of the four conditions:

| Condition | Mean  | Variance  |
|-----------+-------+-----------|
| Cond1     | 5.3676    | 3.3380    |
| Cond2     | 4.9118    | 2.8253    |
| Cond3     | 4.9118    | 2.1132    |
| Cond4     | 4.1176    | 2.3191    |

### Sample Sizes

So we have samples with equal size.
We refer to the number of observations in each group as $$n$$ and the total number of observations as $$N$$.
In this example, we have 4 groups, $$n = 34$$ and $$N = 136$$.

### MSE

According to the assumption homogeneity of variance, MSE is computed as the mean of the sample variances:

$$ MSE = (3.3380 + 2.8253 + 2.1132 + 2.3191) / 4 = 2.6489

### MSB

The formula for MSB is based on the fact that the variance of the sampling distribution of the mean is:

$$ \sigma_M^2 = \frac {\sigma^2} {n} $$

where $n$ is the sample size of each group. So we can compute the population variance as:

$$ \sigma^2 = n \sigma_M^2 $$

Although we don't know the variance of the sampling distribution of mean,
we can estimate it with the variance of the sample means, then

$$ MSB = var(c(5.3676, 4.9118, 4.9118, 4.1176)) * 34 = 9.1786 $$

## Test by Comparing MSE and MSB

Recall that, MSE estimates σ2 whether or not the population means are equal,
whereas MSB estimates σ2 only when the population means are equal and estimates a larger quantity when they are not equal.
Therefore, we can test the sample means are equal by check if the MSB estimates a larger quantity than MSE.

However since MSB could be larger than MSE by chance even if the sample means are equal,
how much larger must MSB be in order to justify the conclusion that the sample means differ?

The mathematics necessary to answer this question were worked out by the statistician R. Fisher,
which is based on the ratio of MSB to MSE, is named after Fisher, called *F ratio*.
The ratio should follow an **F distribution**.

The shape of the F distribution depends on the sample size.
More precisely, it depends on two degrees of freedom (df): numerator degrees of freedom and denominator degrees of freedom.
That is,

$$ F = \frac {V_1 / m_1} {V_2/m_2} \sim F(m_1, m_2) $$

You can use the following R command to draw a F distribution with (5, 2) degrees of freedom.

~~~ R
curve(df(x, 5, 2), from = 0, to = 5)
~~~

## Partition of Variance

One of the important characteristics of ANOVA is that it partitions the variation into its various sources.
ANOVA divides two variances and comparing the ratio to a handbook value to determine statistical significance.

The definitional equation of variance is $$ \sigma^2 = \frac{1}{n-1} \sum{(X_i - \bar(X))^2},
where the divisor is called the degrees of freedom, the summation is called the **sum of squares (SS)**.
In ANOVA, we can use SS to indicate variation, and ANOVA estimates 3 sample variances:

1. $$SS_{total}$$: a total variance based on all the observations from the grand mean (GM, the mean of all observations).
$$SS_{total}$$ is defined as:
$$ SS_{total} = \sum{(X_i - {GM}^2)} $$

2. $$SS_{error}$$: an error variance based on all the observation deviations from its group mean,
$SS_{error} is also called *sum of square within group (SSW)*. SSE is defined as:
$$ SS_{error} = \sum{(X_{1i} - M_1)^2} + \sum{(X_{2i} - M_2)^2} + \dots + \sum{(X_{ki} - M_k)^2} $$

3. $$SS_{condition}$$: a conditional variance based on the deviations of group means from the grand mean,
the result being multiplied by the number of observations in each group.
This is also called *sum of sqaure between (SSB)*, defined as:
 $$ SS_{condition} = n( (M_1 - GM)^2 + (M_2 - GM)^2 + \dots + (M_k - GM)^2) $$
If there are unequal sample sizes, use the weighted sum:
$$ SS_{condition} = n_1 (M_1 - GM)^2 + n_2 (M_2 - GM)^2 + \dots + n_k (M_k - GM)^2 $$

The sum of squares error can also be computed by substraction:

$$ SS_{error} = SS_{total} - SS_{condition} $$

Also, the number of degrees of freedom $$df$$ can be partitioned in a similar way:

$$
{df}_{total} = {df}_{error} + {df}_{condition} \\
N - 1 = (N - k) + (k - 1) \\
$$


## F Test

Then we can apply the F distribution formula to calculate F ratio:

$$ F = \frac {V_1 / m_1} {V_2/m_2}$$

where $$V_1 = SS_{condition} $$ and numerator degrees of freedom $$ m_1 = {df}_{condition} = k - 1$$,
and $$ V_2 == SS_{error} $$ and denominator degrees of freedom $$ m_2 = {df}_{error} = N - k $$.

In another word, we can calculate MSB and MSE estimates from sum of squares, and calculate F ratios as:

$$ F = \frac {\text{variance between groups}} { \text{variance within groups} } = \frac {MSB} {MSE} $$

The formula for computing $$ MSB $$ is:

$$ MSB = \frac {SS_{condition}} {df_{condition}} $$

and the formula for computing $$ MSE $$ is:

$$ MSE = \frac {SS_{error}} {df_{error}} = \frac {SS_{total} - SS_{error}} {df_{error}} $$


## R Example

We extract our test data from `mtcars` dataset:

~~~ R
head(mtcars)
~~~

~~~
                   mpg cyl disp  hp drat    wt  qsec vs am gear carb
Mazda RX4         21.0   6  160 110 3.90 2.620 16.46  0  1    4    4
Mazda RX4 Wag     21.0   6  160 110 3.90 2.875 17.02  0  1    4    4
Datsun 710        22.8   4  108  93 3.85 2.320 18.61  1  1    4    1
Hornet 4 Drive    21.4   6  258 110 3.08 3.215 19.44  1  0    3    1
Hornet Sportabout 18.7   8  360 175 3.15 3.440 17.02  0  0    3    2
Valiant           18.1   6  225 105 2.76 3.460 20.22  1  0    3    1
~~~

We only want to research three factors: mpg, cylinder number and horse power.

~~~ R
df <- mtcars[, c("mpg", "cyl", "hp")]
~~~

And we want to convert `cyl` from numbers to factor levels:

~~~ R
df$cyl <- as.factor(df$cyl)
levels(df$cyl)
# [1] "4" "6" "8"
~~~

So my research question is, does `cyl` have a significant impact on `mpg`?
Then we can define the hypothesis:

$$
H_0: \mu_{cyl = 4} = \mu_{cyl = 6} = \mu_{cyl = 8} //
H_1: \text{at least one mean differs to one another}
$$

By using condition `cyl`, the population can be divided into three groups:

~~~ R
> table(df$cyl)
#
#  4  6  8
# 11  7 14
~~~

The total number of observations is $$N = 32$$, the number of groups is $$k=3$$, and the sample sizes are not equal.

We compute $$ SS_{total} $$ first,

~~~ R
SST <- sum( (df$mpg - mean(df$mpg))^2 )
# [1] 1126.047
~~~

and then compute $$ SS_{condition} $$ (or sum of square between):

~~~ R
# Compute grand mean
GM <- mean(df$mpg)
# Compute sample group means
M <- sapply(levels(df$cyl), function(i) { mean(df$mpg[which(df$cyl == i)]) })
# Compute sum of squares between
SSB <- sum( as.numeric(table(df$cyl)) * (M - GM)^2 )
# [1] 824.7846
~~~

Finally, we can compute $$ SS_{error} $$:

~~~ R
SSE <- SST - SSB
[1] 301.2626
~~~

Then we can calculate the F ratio:

~~~ R
# DF total: N - 1 = 31
dft <- nrow(df) - 1
# DF condition: k - 1 = 2
dfb <- length(levels(df$cyl)) - 1
# DF error: N - k = 29
dfe <- nrow(df) - length(levels(df$cyl))
# Mean square error
MSE <- SSE / dfe
# [1] 10.38837
# Mean square condition (between)
MSB <- SSB / dfb
# [1] 412.3923
# F ratio = MSB / MSE
# [1] 39.69752
~~~

Use F distribution with degree of freedom `(2, 29)` to get the probability value of F ratio:

~~~ R
# Probability of F = F.ratio
Pr1 <- df(F.ratio, dfb, dfe)
# [1] 1.33206e-09
# Probability of F > F.ratio
Pr2 <- pf(F.ratio, dfb, dfe, lower.tail = FALSE)
# [1] 4.978919e-09
~~~

Therefore, we have the following ANOVA summary table:

| Source    | df    | SS        | MS        | F ratio   | Pr(>F)    |
|-----------+-------+-----------+-----------+-----------+-----------|
|Condition  | 2     | 824.7846  | 412.3923  | 39.69752  | 4.978919e-09  |
|Error      | 29    | 301.2626  | 10.38837  |           |           |
|Total      | 31    | 1126.047  |           |           |           |

R has an ANOVA function `aov`, and we can compare the result with our calculation:

~~~ R
fit1 <- aov(mpg ~ cyl, data = df)
summary(fit1)
~~~

~~~
            Df Sum Sq Mean Sq F value   Pr(>F)
cyl          2  824.8   412.4    39.7 4.98e-09 ***
Residuals   29  301.3    10.4
---
Signif. codes:  0 ‘***’ 0.001 ‘**’ 0.01 ‘*’ 0.05 ‘.’ 0.1 ‘ ’ 1
~~~

They are exactly same result except rounding.
From the ANOVA summary table, the probability under $$ H_0 $$ is very small,
so we can reject $$ H_0 $$ and conclude that `cyl` have a significant impact on `mpg`.
