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

### Advantages of Cross-entropy loss

- Loss surface is guaranteed to be convex
- More strongly penalizes bad predictions
- Has roots in probability and information theory

## Logistic Regression
- Good for outliers since we wrap our data into a logistic function call
- L2 and L1 Loss Motivation: We want to measure the size of the error(difference between prediction and data), and for it to be indifferent to sign.
- To measure the difference between two probability distributions, the cross entropy loss is useful
- Use a threshold to classify a point on the logistic curve into a graph, this turns our decision rule into a classifier

## Evaluating Classifiers
- The most basic evaluation for a classifier is accuracy. This is widely used, but in the presence of class imbalance it doesn't mean much(since if we classify all the pictures as dog and dogs compose of 95% of the dataset our accuracy is 95%)
- Types of classification errors: True positive and True negatives(correct when classify observations as positive or negative), False positive(False alarm), False negative(We failed to detect)
- Confusion matrix: Gives us true + false positives and negatives for a given classifier

## Precision and Recall
- `Accuracy = (True positive + True negative) / number of data points`
  - This tells us what proportion of points did our classifier classify correctly
- `Precision = True positive / (True positive + False positive)`
  - Of all observations that were predicted to be 1, what proportion is actually 1? Penalized false positives
- `Recall = True positive / (True positive + false negative)` 
  - Of all observations that were actually 1, what proportion did we predict to be 1? Penalizes false negatives
  - Can achieve 100% recall by making the classifier output 1 regardless of input, but makes precision low
- Generally, a higher classification threshold for positives equals fewer false positives, increases precision.
- On the other hand, a lower classification threshold has fewer false negatives, increases recall.

## Visual Metrics
- The only thing we can change after getting our model is to play around with our classification threshold
- Accuracy vs threshold: `Threshold too high = false negatives`, `Threshold too low = false positives`
- Precision vs threshold: `Threshold increases = fewer false positives`, precision tends to increase(not always)
- Recall vs threshold: `Threshold increases = more false negatives`, recall tends to decrease

## Other Metrics
- `False Positive Rate = False Positives / (False Positives + True Negative)`
  - What proportion of innocent people did I convict
- `True Positive Rate = True Positives / (True Positives + False Negatives)`
  - What proportion of guilty people did I convict? aka recall
- ROC Curve(Receiver Operating Characteristic): plots the false positive rate vs true positive rate
- AUC(Area under curve): Area under ROC curve, best possible = 1, worst possible = 0.5(randomly guessing)
# Questions
1. Can a threshold be a sloped line os is this useless?
 