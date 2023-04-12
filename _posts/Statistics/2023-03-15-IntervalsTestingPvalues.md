---
title: "Intervals, Testing, and Pvalues"
categories:
  - Statistics
layout: archive
sidebar_main: true
author_profile: true
tag:
  - Statistics
---

## Confidence intervals

Confidence intervals are a statistical tool used to estimate a population parameter from a sample. In other words, they provide a range of values that the true parameter is likely to fall within. The confidence level of a confidence interval is the probability that the interval contains the true population parameter.

One of the most commonly used types of confidence intervals is the T confidence interval. This is used when the population standard deviation is unknown and the sample size is small (typically less than 30). The formula for a T confidence interval is:

`CI = x̄ ± t*(s/√n)`

Where CI is the confidence interval, x̄ is the sample mean, s is the sample standard deviation, n is the sample size, and t is the critical value from the T distribution with n-1 degrees of freedom and the desired confidence level.

- example

  Suppose we want to estimate the average height of a population of students. We take a random sample of 20 students and find that the sample mean height is 65 inches and the sample standard deviation is 3 inches. We want to construct a 95% confidence interval for the population mean height.

  Using the formula above, we can calculate the interval as: 

    CI = 65 ± 2.093 * (3/√20) = (62.57, 67.43)

  This means that we are 95% confident that the true population mean height falls within the range of 62.57 to 67.43 inches.

Another type of confidence interval is the Independent group T interval, which is used when comparing two means from independent samples. The formula for this interval is:

`CI = (x̄1 - x̄2) ± t*(√s1²/n1 + s2²/n2)`

Where CI is the confidence interval, x̄1 and x̄2 are the sample means, s1 and s2 are the sample standard deviations, n1 and n2 are the sample sizes, and t is the critical value from the T distribution with n1+n2-2 degrees of freedom and the desired confidence level.

- example

  For example, suppose we want to compare the mean exam scores of two classes. We take a random sample of 30 students from each class and find that the sample mean score for class 1 is 85 with a standard deviation of 4, and the sample mean score for class 2 is 82 with a standard deviation of 5. We want to construct a 95% confidence interval for the difference between the means.

  Using the formula above, we can calculate the interval as:

  CI = (85 - 82) ± 2.045 * (√4²/30 + 5²/30)
    = (0.13, 5.87)

  This means that we are 95% confident that the true difference in population mean exam scores falls within the range of 0.13 to 5.87.

<br>

It is worth noting that when the variances of the two populations being compared are not equal, the formula for the Independent group T interval must be modified to account for this. This is known as the Welch's T interval and involves adjusting the degrees of freedom used to calculate the critical value. confidence intervals are a useful tool in statistics for estimating population parameters from samples. 

## Hypothesis testing

Hypothesis testing is a statistical method used to determine whether an assumed hypothesis about a population is supported by sample data. The process involves testing a null hypothesis against an alternative hypothesis. The null hypothesis is the assumption that there is no significant difference between the population parameter and the sample statistic, while the alternative hypothesis is the assumption that there is a significant difference.

- Choosing a Rejection Region

  The first step in hypothesis testing is to choose a rejection region or a critical value. The rejection region is a range of values for the test statistic that would lead to the rejection of the null hypothesis. The critical value is the point beyond which the null hypothesis is rejected. The choice of the rejection region depends on the level of significance or alpha value, which is typically set at 0.05 or 0.01.

- T-test

  One of the most commonly used tests for hypothesis testing is the T-test. A T-test is used when the population standard deviation is unknown and the sample size is small (less than 30). The T-test is used to test the hypothesis about the population mean, where the null hypothesis assumes that the population mean is equal to a specific value. If the calculated T-statistic falls within the rejection region, the null hypothesis is rejected.

- Two Group testing

  Another type of hypothesis testing is the two-group testing. This method is used to test the hypothesis that there is no significant difference between two groups. The null hypothesis assumes that there is no significant difference, while the alternative hypothesis assumes that there is a significant difference. The two-group testing involves calculating a test statistic, such as the Z-test or the T-test, and comparing it with the critical value to determine whether the null hypothesis is rejected.


Hypothesis testing is a critical tool in statistics for making decisions based on sample data. The process involves testing a null hypothesis against an alternative hypothesis and choosing a rejection region based on the level of significance. The T-test is commonly used for testing the hypothesis about the population mean, while the two-group testing is used for testing the hypothesis about two groups.


<br><br>
## P-values

In statistics, p-value is a measure of evidence against a null hypothesis. It is the probability of observing a test statistic as extreme as or more extreme than the one observed, assuming the null hypothesis is true. The smaller the p-value, the stronger the evidence against the null hypothesis.

### What is P-values?

A P-value is the probability of observing a test statistic as extreme as, or more extreme than, the one observed, given that the null hypothesis is true. A small P-value indicates strong evidence against the null hypothesis, while a large P-value suggests that the null hypothesis cannot be rejected.

### How to calculate a P-values?
The calculation of a P-value depends on the statistical test used and the assumptions made about the distribution of the test statistic. In general, a P-value is calculated by comparing the test statistic to a probability distribution that reflects the null hypothesis.

<br>

For example, in a two-sample t-test, the P-value is calculated by comparing the observed difference in means to the distribution of differences in means that would be expected if the null hypothesis were true. The P-value is then calculated as the probability of observing a difference as large or larger than the observed difference.
<br>

### Interpreting P-values
 The interpretation of a P-value depends on the context of the hypothesis test and the chosen significance level (alpha). In general, a P-value less than the chosen significance level indicates that the null hypothesis can be rejected, while a P-value greater than the significance level suggests that the null hypothesis cannot be rejected.

 - example 1)
  
   if the significance level is set to 0.05, a P-value of 0.02 indicates that there is strong evidence against the null hypothesis, and it can be rejected. Conversely, a P-value of 0.10 suggests that the evidence is not strong enough to reject the null hypothesis.

- example 2)

  A pharmaceutical company claims that a new drug reduces cholesterol levels in patients by at least 10%. To test this claim, a researcher conducts a clinical trial with a sample of 100 patients and finds that the drug reduces cholesterol levels by an average of 8%. The researcher calculates a P-value of 0.07. This means that if the true reduction in cholesterol levels is 10%, there is a 7% chance of getting a sample with an average reduction of 8% or less. Since the P-value is greater than 0.05 (the typical threshold for statistical significance), the researcher cannot reject the null hypothesis and conclude that the drug does not have a significant effect on cholesterol levels.

<br>

### Misconceptions about P-values

It is important to note that P-values do not measure the size of an effect, the probability that the null hypothesis is true, or the clinical significance of a finding. Additionally, a significant P-value does not necessarily imply that the alternative hypothesis is true, as other factors such as sample size, study design, and data quality can also affect the outcome of a hypothesis test.