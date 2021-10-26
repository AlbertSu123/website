---
title: Cross Validation and Regularization
date: "2021-10-21T23:46:37.121Z"
template: "post"
draft: false
slug: "cross-validation-and-regularization"
category: "Data Science Notes"
tags:
  - "Notes"
  - "Lecture"
  - "Data Science"

description: "Notes on Cross Validation and Regularization"
socialImage: "/media/image-2.jpg"
---

# The train-test Split
- **Training Data:** Used to fit model
- **Test Data:** Used to check generalization error
- How to split? Randomly, Temporally, Geographically (Usually Random)
- What size? Larger training set = more complex models, Larger test set = better estimate of generalization error (Usually 90%-10% or 75%-25%)

# Recipe for Successful Generalization
1. Split data into training(90%) and testing set(10%)
2. Use only training data when training, use cross validation to test generalization(do not look at testing data!)
3. Commit to the model and train once more using only training data
4. Test the model using testing data. If accuracy is not acceptable, return to step 2
5. Train on all available data and ship it

# Regularization
Parametrically controlling the model complexity
**Naive Solution:** Find the best value of theta which uses less than B features. Unfortunately, this is an NP hard combinatorial search problem.
- **L0 Norm Ball** Ideal for feature selection but combinatorically difficult to optimize
- **L1 Norm Ball** Encourages sparse solutions and is convex!
- **L2 Norm Ball** Spreads weight over features(robust) and does not encourage sparsity
- **L1 + L2 Norm(Elastic Net)** Compromise, need to tune two regularization parameters

**Standardization:** Ensure each dimension has the same scale and is centered around 0
**Intercept Terms:** Typically doesn't regularize the intercept term 

# Ridge Regression
**Model:** Y = X * theta
**Loss:** Squared Loss
**Regularization:** L2 regularization
**Objective Function** Squared Loss + added penalty
![Ridge Regression](/media/CrossValidationAndRegularization/RidgeRegression.JPG)

Note that there is always a unique optimal parameter vector for Ridge Regression!

# Lasso Regression
**Model:** Y = X * theta
**Loss:** Squared Loss
**Regularization:** L2 regularization
**Objective Function** Average Squared Loss + added penalty
![Ridge Regression](/media/CrossValidationAndRegularization/LassoRegression.JPG)

Note that there is NO closed form solution for the optimal parameter vector for LASSO so we must use numerical methods like gradient descent

![Summary](/media/CrossValidationAndRegularization/SummaryRegression.JPG)
# Questions