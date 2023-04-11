---
title: "Probability & Expected Values"
categories:
  - Statistics
layout: archive
sidebar_main: true
author_profile: true
tag:
  - Statistics
---
<br>

## Probability

Probability is a field of mathematics that deals with the study of random events. In simple terms, probability is the measure of the likelihood of an event occurring. Probability is an essential tool in various fields, including science, engineering, economics, and finance, among others. 


<br>

### Probability Mass Functions
Probability mass functions (PMFs) are used to describe the probability distribution of discrete random variables. Discrete random variables are random variables that can take on a countable number of values. For example, the number of heads obtained when tossing a coin three times is a discrete random variable, as it can take on values of 0, 1, 2, or 3. A PMF assigns probabilities to all possible values of a discrete random variable.

- The PMF is defined as follows:
 Suppose X is a discrete random variable that takes on values x1, x2, ..., xn, then the PMF of X is defined by the function p(x), where p(xi) = P(X = xi) for all i = 1,2,...,n. The PMF must satisfy the following two properties:<br>
    1. 0 <= p(xi) <= 1 for all i = 1,2,...,n
    2. The sum of all probabilities is equal to 1. That is, p(x1) + p(x2) + ... + p(xn) = 1
<br>

### Probability Density Functions
Probability density functions (PDFs) are used to describe the probability distribution of continuous random variables. Continuous random variables are random variables that can take on any value within a specified range. For example, the height of a person is a continuous random variable, as it can take on any value within a specified range.
- The PDF is defined as follows:
    Suppose X is a continuous random variable with a probability density function f(x), then the probability that X takes on a value between a and b is given by the integral of f(x) over the interval [a,b]. That is, P(a <= X <= b) = integral of f(x) dx from a to b. The PDF must satisfy the following two properties:<br>
    1. f(x) >= 0 for all x in the specified range
    2. The integral of f(x) over the entire range is equal to 1. That is, integral of f(x) dx over the entire range = 1
<br><br><br>

## Conditional Probability
In probability theory, conditional probability is the probability of an event happening, given that another event has occurred. This concept is important in many areas of mathematics and statistics, especially in machine learning and data science.<br><br>
For example, <br>
Let A and B be two events. The conditional probability of A given B is denoted by P(A | B) and is defined as:

`P(A | B) = P(A and B) / P(B)`

where P(A and B) is the probability of A and B happening together, and P(B) is the probability of B happening.
<br><br>

### Bayes' Rule
Bayes' rule is a formula that relates conditional probabilities. It is used to update the probability of an event based on new information or evidence. Let A and B be two events, then Bayes' rule states:

`P(A | B) = P(B | A) * P(A) / P(B)` <br>

where P(A) and P(B) are the probabilities of A and B happening, respectively, and P(B | A) is the conditional probability of B given A.
<br><br>

### Independence
Two events A and B are said to be independent if the occurrence of one event does not affect the probability of the other event. In other words, if A and B are independent, then

`P(A and B) = P(A) * P(B)`

Conditional probability and independence are related. If A and B are independent, then

`P(A | B) = P(A)` 

and

`P(B | A) = P(B)`

<br><br>
Conditional probability, Bayes' rule, and independence are important concepts in probability theory. They are used to model the uncertainty and randomness of real-world events. 


<br><br><br>
## Expected Values

Expected values are a fundamental concept in probability theory and are used to calculate the long-term average of a random variable. The expected value is a measure of central tendency that helps us understand the behavior of a random variable over time.

An expected value is the long-term average value of a random variable. To calculate the expected value, we multiply the probability of each possible outcome by the corresponding value of that outcome, and then sum up the results. Mathematically, we can express the expected value of a random variable X as:

`E(X) = ∑x P(X=x)`

Where X is a random variable, x is a possible outcome of X, and P(X=x) is the probability of X taking the value x.

#### Simple Examples
- rolling a fair six-sided die example

    Let's consider a simple example of rolling a fair six-sided die. We can define the random variable X as the outcome of the die roll. The possible outcomes are 1, 2, 3, 4, 5, and 6, each with a probability of 1/6. The expected value of X is

    E(X) = 1/6 × 1 + 1/6 × 2 + 1/6 × 3 + 1/6 × 4 + 1/6 × 5 + 1/6 × 6 = 3.5

    So the expected value of rolling a fair six-sided die is 3.5.


- Flipping a fair coin example

    Let Y be the random variable that represents the number of heads in two flips. The possible outcomes are 0, 1, and 2 heads, each with a probability of 1/4. The expected value of Y is

    E(Y) = 0 × 1/4 + 1 × 1/2 + 2 × 1/4 = 1

    So the expected value of the number of heads in two coin flips is 1.
<br><br>

### Expected Values for Probability Density Functions
In some cases, we deal with continuous random variables that have a probability density function (PDF) instead of a probability mass function (PMF). In these cases, the expected value is calculated using the integral of the PDF over the possible values of the random variable. Mathematically, we can express the expected value of a continuous random variable X as

`E(X) = ∫x f(x) dx`

Where f(x) is the probability density function of X.

Let's consider an example of a continuous random variable X that follows a uniform distribution between 0 and 1. The PDF of X is

`f(x) = 1, if 0 ≤ x ≤ 1`

0, otherwise

The expected value of X is

`E(X) = ∫0¹ x × 1 dx = 1/2`

So the expected value of a random variable X that follows a uniform distribution between 0 and 1 is 1/2.
<br><br>
Expected values are a fundamental concept in probability theory that help us calculate the long-term average of a random variable. We can use expected values to understand the behavior of a random variable over time and make predictions about future outcomes. By applying the concept of expected values to probability density functions, we can also calculate the expected value of continuous random variables.
