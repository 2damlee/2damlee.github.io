---
title: "Variability, Distribution, and Asymptotics"
categories:
  - Statistics
layout: archive
sidebar_main: true
author_profile: true
tag:
  - Statistics
---

## Variability

Variability is a concept in statistics that refers to the amount of dispersion or spread in a set of data. When working with data, it is important to understand the amount of variability present in order to make accurate inferences and draw meaningful conclusions.

Variability is often described using measures of dispersion, which include the range, variance, and standard deviation. The range is the difference between the largest and smallest values in a dataset, while the variance and standard deviation are measures of how far the data points are from the mean.
<br><br>

#### Variance Simulation Examples

 Suppose we have a dataset of 100 observations and we want to calculate the variance of the dataset. We can do this using the var() function in R.


````r
# Create a dataset with 100 observations
data <- rnorm(100)

# Calculate variance using the var() function
variance <- var(data)

# Print the variance
print(variance)
````

The output of the above code will give us the variance of the dataset.
<br>

### Standard error of the mean

The standard error of the mean (SEM) is a measure of the variability of sample means. It tells us how much the sample means vary from the true population mean. The formula for SEM is

`SEM = SD / sqrt(n)`

where SD is the standard deviation of the sample and n is the sample size.

For example, 
We can use the sd() function to calculate the standard deviation and the length() function to get the sample size.

````r
# Create a dataset with 100 observations
data <- rnorm(100)

# Calculate SEM using the sd() and length() functions
SEM <- sd(data) / sqrt(length(data))

# Print the SEM
print(SEM)
````

The output of the above code will give us the SEM of the dataset.
<br>

### Variance data example

Suppose we have a dataset of 1000 observations and we want to calculate the variance of the dataset. We can do this using the var() function in R.

````r
# Create a dataset with 1000 observations
data <- rnorm(1000)

# Calculate variance using the var() function
variance <- var(data)

# Print the variance
print(variance)
````
The output of the above code will give us the variance of the dataset. We can also visualize the distribution of the data using a histogram.
````r
hist(data)
````
<br>
<br>

## Distributions

Probability distributions are an essential concept in statistics and data science. A probability distribution is a function that describes the likelihood of obtaining different values of a random variable. In other words, it is a way to represent the possible outcomes and their probabilities for a given experiment.

There are many different types of probability distributions, but we will focus on three of the most commonly used: the binomial distribution, the normal distribution, and the Poisson distribution.

### Binomial Distribution
The binomial distribution is a discrete probability distribution that describes the number of successes in a fixed number of independent trials. It is often used in experiments where there are only two possible outcomes (e.g., success or failure) and the trials are independent of each other.

The probability mass function (PMF) of the binomial distribution is given by

`P(X=k) = nCk * p^k * (1-p)^(n-k)`

where X is the random variable, k is the number of successes, n is the total number of trials, p is the probability of success, and nCk is the binomial coefficient, which represents the number of ways to choose k objects from a set of n objects.

- example of how to use R to simulate a binomial distribution with 10 trials and a success probability of 0.5
    ````r
    # Generate 1000 random samples from a binomial distribution
    samples <- rbinom(n=1000, size=10, prob=0.5)

    # Plot the histogram of the samples
    hist(samples, breaks=seq(-0.5, 10.5, by=1), freq=FALSE, main="Binomial Distribution (n=10, p=0.5)")

    ````

<br>
### Normal Distribution
The normal distribution, also known as the Gaussian distribution, is a continuous probability distribution that is often used to model real-world phenomena such as height, weight, and IQ scores. It is characterized by its mean and standard deviation.

<br>
The probability density function (PDF) of the normal distribution is given by

`f(x) = (1 / (sigma * sqrt(2*pi))) * exp(-(x - mu)^2 / (2 * sigma^2))`

where x is the random variable, mu is the mean, sigma is the standard deviation, and pi is the mathematical constant.

<br>

### Poisson Distribution
The Poisson distribution is a discrete probability distribution that is often used to model the number of events that occur in a fixed interval of time or space. It is characterized by its rate parameter, which represents the average number of events per interval.

The probability mass function (PMF) of the Poisson distribution is given by

`P(X=k) = (lambda^k * exp(-lambda)) / k!`

where X is the random variable, k is the number of events, lambda is the rate parameter, and k! is the factorial function, which represents the number of ways to arrange k objects.

Here's an example of how to use R to simulate a Poisson distribution with a rate parameter of 2:
````r
# Generate 1000 random samples from a Poisson distribution
samples <- rpois(n=1000, lambda=2)
````

<br><br>

## Asymptotics
Asymptotics is the study of the behavior of statistical estimators as the sample size grows infinitely large.

The Law of Large Numbers (LLN) states that as the sample size increases, the sample mean converges to the true mean of the population from which the sample was drawn. In other words, if we take a large enough sample, the sample mean will be very close to the true mean. This is an important concept in statistics because it allows us to estimate population parameters with increasing accuracy as the sample size grows. The LLN can be illustrated using R as follows

````r
set.seed(123)
n <- 1000
p <- 0.5
x <- rbinom(n, size = 1, prob = p)
cumulative_mean <- cumsum(x) / (1:n)
plot(cumulative_mean, type = "l", ylab = "Cumulative Mean", xlab = "Sample Size")
abline(h = p, col = "red")

````

The above code generates a sequence of Bernoulli random variables with probability of success `p = 0.5`, and calculates the cumulative mean at each sample size. The plot shows that as the sample size grows, the cumulative mean converges to the true mean `p`.

<br>

The Central Limit Theorem (CLT) states that the distribution of the sample mean approaches a normal distribution as the sample size grows. This is true regardless of the underlying distribution of the population from which the sample was drawn, as long as the sample size is large enough. The CLT can be illustrated using R as follows:

````r
set.seed(123)
n <- 1000
x <- rexp(n, rate = 1)
sample_mean <- replicate(10000, mean(sample(x, size = n, replace = TRUE)))
hist(sample_mean, freq = FALSE, main = "Sample Means", xlab = "Sample Mean")
curve(dnorm(x, mean = mean(x), sd = sd(x) / sqrt(n)), add = TRUE, col = "red")

````

The above code generates a sequence of exponential random variables, and takes 10,000 samples of size n = 1000, with replacement. The histogram shows the distribution of the sample means, which is approximately normal, as predicted by the CLT. The red curve shows the theoretical normal distribution with mean and standard deviation calculated from the original exponential distribution.

Finally, we can use the LLN and the CLT to construct confidence intervals for population parameters, such as the mean or proportion. A confidence interval is a range of values that is likely to contain the true value of the parameter, with a certain level of confidence. The level of confidence is determined by the chosen significance level and the sample size. The formula for a confidence interval depends on the estimator and the underlying distribution of the data. As an example, we can calculate a 95% confidence interval for the mean of a normal distribution with unknown variance, using the t-distribution

````r
set.seed(123)
n <- 100
x <- rnorm(n, mean = 5, sd = 2)
alpha <- 0.05
t_value <- qt(1 - alpha / 2, df = n - 1)
se <- sd(x) / sqrt(n)
ci <- mean(x) + t_value * se * c(-1, 1)
ci

````