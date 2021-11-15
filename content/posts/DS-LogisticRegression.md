---
title: Logistic Regression
date: "2021-10-21T23:46:37.121Z"
template: "post"
draft: false
slug: "cross-validation-and-regularization"
category: "Data Science Notes"
tags:
  - "Notes"
  - "Lecture"
  - "Data Science"

description: "Notes on Logistic Regression"
socialImage: "/media/image-2.jpg"
---

## Linear Regression

Goal: Predict a number from a set of features
Output: A real number
Process: determine optimal parameters by minimizing some average loss and something with a regularization penalty.

## Classification

Goal: Predict a categorical variable(ie win or lose, disease or no disease, spam or ham)
Types: Binary Classification(Two classes, ie spam or not spam), Multiclass classication(type of animal)

## Logistic Regression Model

P(Y=1|x) = 1 / (1 + e^((-x^t)_theta)) = sigma(x^T _ theta)
The probability of an observation belonging to class 1 given x(vector of features) = Linear regression function on linear transformation of a linear combination of x(vector of features)

Thus, the logistic regression is sometimes known as a generalized linear model since it is a non-linear transformation of a linear model.

## Pitfalls of Squared Loss functions for logitisic regression

Can get trapped in a flat region if the surface is not convex
Solution: Use Cross-entropy Loss!
Empirical Risk for logistic regression when using cross-entropy loss  
`R(theta) = -(1/n)sum from i=1 to n( y_i * log(logistic regression(X_i^T * theta)) + (1 - y_i) * (1 - logistic regression(X_i^T * theta)))`

Advantages of Cross-entropy loss

- Loss surface is guaranteed to be convex
- More strongly penalizes bad predictions
- Has roots in probability and information theory

