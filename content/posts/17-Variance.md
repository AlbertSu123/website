---
title: Data 100 Variance
date: "2021-10-21T23:46:37.121Z"
template: "post"
draft: false
slug: "variance"
category: "Data Science Notes"
tags:
  - "Notes"
  - "Lecture"
  - "Data Science"

description: "Notes on variance"
socialImage: "/media/image-2.jpg"
---

**Random Variable:** A numerical function of a random sample, a statistic.
**Expectation:** Weighted average of the values of X, where the weights are the probabilities of the values
**Variance:** Expected squared deviation from the expectation of X, the units of variances are the units of X squared.

## Interpretation of Variance

- Var(X) = E[X^2] - (E[X])^2
- The main use of variance is to quantify chance error
- **Chebyshev's inequality:** The vast majority of the probability is around the expectation plus minus a few standard deviations
- If X is centered, ie E[X] = 0, then Var(X) = E[X^2]

## Linear Transformations

- E[aX + b] = aE[X] + b
- Var(aX+b)= a^2 \* Var(X)
- SD(aX+b) = |a|SD(X)
- Var(aX+b)=Var(aX)
- A shift by b units does not affect spread
- The multiplication by a does affect spread

## Standardization of random variables

- X in standard units = (X-E[X]) / SD(X)
- X in standard units measures the number of SDs from expectation
- It is a linear transformation of X
- E[X_su] = 0, SD[X_su] = 1
- E[X_su^2] = Var[X_su] = 1

## Variance of a sum -> Covariance

- Var[X+Y] = Var[X] + Var[Y] + 2\*Covariance
- Covariance = 2E[(x-E[X])(Y-E[Y])] =
- Covariance is 0 if X and Y are independent
- To get right of units for covariance, scale it by the standard deviation to get correlation
- Correlation = Cov[X,Y] / (SD[X] \* SD[Y])
- Uncorrelated random variables is when covariance = 0

## I.I.D. sample sum

- independent and identically distributed
- draws at random with replacement from a population are i.i.d.
- E[S_n] = n _ sample mean, Var[S_n]=n_(std dev)^2, SD[S_n] = sqrt(n) \* std dev

## Model Risk

- Mean squared error of prediction
- Model risk = E[(Y-Y(x))^2], Y(x) is a model that we are using
- **Chance Error:** Due to randomness alone in the new observations
- **Bias:** Non random error due to model being different from the true underlying function

## Bias and Overfitting

- Overfitting: small differences in random samples which leads to large differences in the fitted model
- Overfitting solution: Reduce model complexity
- Model risk = std dev^2 + (model bias)^2 + model variance

# Questions
