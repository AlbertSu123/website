---
title: Inference
date: "2021-10-21T23:46:37.121Z"
template: "post"
draft: false
slug: "inference"
category: "Data Science Notes"
tags:
  - "Notes"
  - "Lecture"
  - "Data Science"

description: "Notes on Inference"
socialImage: "/media/image-2.jpg"
---

# Inference
- Moving from premises to logical consequences
- Induction is inference from a particular premise to a universal conclusion
- Statistical inference: Using data analysis to deduce properties of underlying distribution

## Prediction vs Inference
- Prediction: Using our model to make predictions for unseen data. (Given attributes of house, how much is it worth?) We don't care about how
- Inference: Using our model to draw conclusions about the underlying true relationships between our features and response. (How much extra will a house be worth if it has a view of the river?) We care about model parameters that are interpretable and meaningful

## Statistical Inference
- Draw conclusions about a population parameters given only a random sample
- Parameter: some function of population, ie population mean
- Estimator: some function of a sample, whose goal is to estimate a population parameter, ie sample mean. Estimators are random variables
    - Bias of an estimator: difference between estimator's expected value and the true value of the parameter being estimated.
    - Variance of an estimator: Expected squared deviation of an estimator from its mean

## Bootstrapping
- Idea: treat our random sample as a population and resample from it
- Psuedocode
    ```
    collect random sample of size n(aka bootstrap population)
    initialize list of estimates
    repeat 10,000 times:
        resample with replacement from bootstrap population
        apply estimator f to resample
        store in list
    list of estimates is the bootstrapped sampling distribution of f
    ```
- The median cannot be accurately drawn from bootstrapping
- If the sample is too small, bootstrapping won't work

## Confidence Interval
- What does a confidence interval p% mean?
    - If we take a sample from the population and compute P% confidence interval for the true population parameter, and repeat this many times, our population parameter will be in our interval P% of the time.
- to compute confidence interval(s,f,P), approximate sampling distribution of f using sample s. Choose middle P% of samples from this approximate distribution
- A 95% confidence interval does not mean that there is a 95% chance that the population parameter is in the interval, either the population parameter is in or isn't

## Bootstrapping Model Parameters
- Our estimate for theta depends on what our training data was.
- We want to think about all of the different ways that our training data and our parameter estimate could have come out
- We want to test whether a feature has any effect on the outcome. This works for linear and logistic regression models with any number of features.
```
Estimate theta1 each time
Make confidence interval for theta 1 and see if 0 is in the interval
    If yes, theta 1 is not significantly different than 0
    If no, theta 1 is significantly different than 0
```

## Multicollinearity
- If features are related to one another, it might not be possible to have a change in one while holding the others constant
- Multicollinearity: Where a feature can be predicted fairly accurately by a lienar combination of other features.
    - Doesn't impact model predictability, only interpretability
- Perfect Multicollinearity: one feature can be written exactly as linear combination of other features

## Summary
- Estimators are functions that provide estimates of true population parameters
- We can bootstrap to estimate the sampling distribution of an estimator
- Using the bootstrapped sampling distribution, we can compute a confidence interval for our estimator
    - This gives a rough idea of how uncertain we are about the true population parameter
    - Only valid if the original random sample is representative
- The assumption when performing linear regression is that there is some true parameter theta that defines the linear relationship between features X and response Y
    - We can use bootstrap to determine whether or not an individual feature is significant
- Multicollinearity arises when features are correlated with one another 
- Supervised Learning = We have an X and Y
- Unsupervised Learning = We only have x, want to learn about X
# Questions
What is confidence interval


