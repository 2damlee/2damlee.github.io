---
title: "Power, Bootstrapping, and Permutation tests"
categories:
  - Statistics
layout: archive
sidebar_main: true
author_profile: true
tag:
  - Statistics
---

## Power
Power is an important concept in hypothesis testing that refers to the ability of a statistical test to detect an effect when one exists. In other words, power is the probability of correctly rejecting a false null hypothesis. A higher power indicates a greater likelihood of detecting a true effect.

<br>

### Calculating Power
To calculate power, we need to know the effect size, the significance level (alpha), and the sample size. The effect size is the difference between the null hypothesis and the alternative hypothesis, and it can be expressed in different ways depending on the test being used. For example, in a t-test, the effect size would be the difference between the means divided by the standard deviation. In general, a larger effect size leads to a higher power.

The significance level, or alpha, is the probability of rejecting the null hypothesis when it is actually true. A common value for alpha is 0.05, which means there is a 5% chance of making a type I error (rejecting the null hypothesis when it is true). A smaller alpha leads to a lower chance of making a type I error, but it also decreases power.

<br>
The sample size is the number of observations in the sample. A larger sample size leads to a higher power, because it reduces the variability in the data and increases the precision of the estimate.

To calculate power, we can use statistical software or online calculators. For example, in R, we can use the pwr package to calculate power for a t-test. Here's an example:

````r
library(pwr)
power.t.test(n = 100, delta = 0.5, sd = 1, sig.level = 0.05, type = "one.sample")
````
<br>
This code calculates the power for a one-sample t-test with a sample size of 100, a true mean of 0.5, a standard deviation of 1, and a significance level of 0.05.

<br><br>

### Notes on Power

There are a few important things to keep in mind when thinking about power

- Power is affected by the effect size, alpha, and sample size. A larger effect size and sample size lead to higher power, while a smaller alpha leads to lower power.
Power is not affected by the null hypothesis. It only depends on the alternative hypothesis.

- A low power does not necessarily mean that the null hypothesis is true. It could simply mean that the sample size is too small or the effect size is too small to detect.

- Power analysis can be used to determine the sample size needed to achieve a desired power for a given effect size and alpha.

<br><br>

### T-test Power

One common use of power analysis is in the context of t-tests. In a t-test, we compare the means of two groups to see if they are significantly different. The power of a t-test depends on the effect size, sample size, and the difference between the means.

To calculate the power of a t-test, we can use the same formula as before, but with a few modifications. Instead of the effect size, we use Cohen's d, which is a standardized measure of the difference between the means. We also use the pooled standard deviation, which takes into account the variability in both groups.

Here's an example of calculating the power of a two-sample t-test in R:

````r
library(pwr)
power.t.test(n = 100, delta = 0.5, sd = 1, sig.level = 0.05, type = "two.sample", alternative = "two.sided")
````
<br>
This code calculates the power for a two-sample t-test with a sample size of 100 in each group, a true difference in means of 0.5, a pooled standard deviation

<br><br><br>

## Multiple Comparisons
Multiple comparisons is a common problem in statistical analysis, occurring when multiple hypothesis tests are performed simultaneously. The more hypothesis tests are performed, the greater the likelihood of false positives (Type I errors) occurring by chance alone. Therefore, it is important to take multiple comparisons into account when interpreting statistical results.

<br>

There are several methods for controlling the rate of false positives in multiple comparisons. One common method is the Bonferroni correction, which adjusts the significance level of each individual test based on the number of tests performed. For example, if 10 tests are performed at a significance level of 0.05, the Bonferroni correction would adjust the significance level to 0.005 for each individual test, in order to maintain an overall family-wise error rate of 0.05.

<br>

Another method for controlling the false positive rate is the False Discovery Rate (FDR) correction. The FDR is the expected proportion of false positives among the rejected null hypotheses, and can be controlled by adjusting the p-values of each individual test. The Benjamini-Hochberg procedure is a commonly used method for controlling the FDR.

It is important to note that multiple comparisons can also lead to false negatives (Type II errors), since the significance level of each individual test is reduced. This can lead to a lack of power to detect true effects. Therefore, it is important to balance the control of false positives with the need to maintain power.

In addition to controlling the false positive rate, it is also important to consider the effect size and clinical relevance of the results. A small effect size may be statistically significant with a large sample size, but may not be clinically relevant. Therefore, it is important to interpret statistical results in the context of the research question and prior knowledge.
<br><br>

In summary, multiple comparisons is a common problem in statistical analysis that can lead to false positives and false negatives. The Bonferroni correction and False Discovery Rate correction are two commonly used methods for controlling the false positive rate, but it is also important to consider the effect size and clinical relevance of the results.

<br><br><br>

## Resampling

Resampling is a statistical technique used to make inferences about a population using data from a sample. There are two main types of resampling: bootstrapping and permutation tests.

<br>

### Bootstrapping
Bootstrapping is a resampling method used to estimate the sampling distribution of a statistic from a single sample by repeatedly resampling from that sample. The main idea behind bootstrapping is that if the sample is representative of the population, then the distribution of the statistic calculated from the resampled datasets should be similar to the distribution of the statistic calculated from the original sample. 

<br>

### Example

 Suppose we have a sample of 50 values and we want to estimate the mean of the population from which the sample was drawn. We can use bootstrapping to estimate the standard error of the mean and construct a confidence interval for the population mean. To do this, we randomly sample with replacement from the original sample to create a new sample of the same size (i.e., 50 values). We calculate the mean of this new sample and record it. We repeat this process many times (e.g., 1000 times) to create a distribution of means. From this distribution, we can estimate the standard error of the mean and construct a confidence interval for the population mean.

<br>

### Notes

One important note about bootstrapping is that it assumes the sample is representative of the population. If the sample is biased or not representative, then bootstrapping may not provide accurate estimates of the population parameters.

Permutation tests, also known as randomization tests, are a resampling method used to test hypotheses by randomly permuting the data and recalculating the test statistic. The main idea behind permutation tests is to use the null hypothesis to generate a distribution of test statistics and compare the observed test statistic to this distribution to determine whether the null hypothesis should be rejected.

<br>

#### example  of how a permutation test works
Suppose we want to test whether there is a significant difference in means between two groups, A and B. We can use a permutation test to test this hypothesis. First, we calculate the difference in means between the two groups in the original sample. Then, we randomly permute the group labels and recalculate the difference in means. We repeat this process many times (e.g., 1000 times) to create a distribution of differences in means under the null hypothesis (i.e., no difference between the two groups). We compare the observed difference in means to this distribution to determine whether it is statistically significant.

<br><br>

In conclusion, resampling methods such as bootstrapping and permutation tests are powerful tools for making inferences about a population using data from a sample. They allow us to estimate population parameters and test hypotheses without making assumptions about the distribution of the data. However, it is important to ensure that the sample is representative of the population before using resampling methods, and to carefully interpret the results of these tests.