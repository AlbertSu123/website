---
title: Data 100 Modeling
date: "2021-10-10T23:46:37.121Z"
template: "post"
draft: false
slug: "data-modeling"
category: "Data Science Notes"
tags:
  - "Notes"
  - "Data Science"
  
description: "Notes on the different models"
socialImage: "/media/image-2.jpg"
---

## Motivation
Predict unknown values based on known values

## Modeling Process
Choose a Model:** Constant model
Choose a Loss Function:** Squared Loss, Absolute Loss
Minimize average loss across entire dataset to determine optimal parameters

## Correlation
**Correlation Coefficient (r):** Measures the strength of the LINEAR association between two variables
  - r is unitless and ranges between -1 and 1
    - if r = 1, x = y and all points fall exactly on the line
    - if r = -1, -x = y and all points fall exactly on the line
    - if r = 0, there is no linear association between x and y
  - r says nothing about causation or non-linear association. Remember correlation does not imply causation!

r = average of the product of x and y, both measured in standard units
x_i in standard units = (x_i - x) / o_x

**Covariance:** r * std_x * std_y

## Simple Linear Regression
Motivation: Want to predict value of y for and given x. A naive attempt for getting children heights given parent heights is to compute the average value of y for each x value, then use those as predicts.

Simple linear regression: y = a + bx
To determine optimal a and b, choose a loss function. If the loss function is squared loss, the objective function is mean squared error(MSE).

To solve for the optimal parameters, we use the objective function and minimize the mean squared error by hand using calculus.

b = r * (o_y/o_x)
a = y_bar - b * x_bar
This gives us parameter estimates for x and y.

## Loss Surfaces
Usually 3d, with axes being a, b and loss(y = a + bx)

## Model Interpretation
Slope - measured in units of y per unit of x
New data needs to be similar to original data - you cannot predict the weight of a chihuahua's weight given a model for golden retrievers
Visualize, then quantify - watch out for anscombe's quartet
![AnscombesQuartetGraphs](/media/DataScienceNotes/Anscombes quartet.JPG)

## Terminology
**Names for the x variable:**
  - Feature
  - Covariate
  - Independent variable
  - Explanatory variable
  - Predictor
  - Input
  - Regressor

**Names for the y variable:**
  - Output
  - Outcome
  - Response
  - Dependent variable

## Adding independent variables
Use a weighted sum of coefficients and input variables.

## Evaluating Models
  - Look at Mean Squared Error(MSE) or Root Mean Squared Error(RMSE)
  - Look at the correlations
  - Look at a residual plot

Root Mean Squared Error: Square root of the mean squared error. RMSE is in the same units as y. A lower RMSE indicates more accurate predictions. It is impossible to lower the RMSE just by adding features using the same data

**R squared**: Used to measure the strength of the linear association between our actual y and predicted y. aka coefficient of determination.

R^2 = variance of fitted values / variance of y

## Ordinary Least Squares

### Multiple regression using matrix multiplication

Multiple regression is of the form
`y = theta_0 + theta_1 * x_1 + theta_2 * x_2 + ... + theta_p * x_p`
We can restate this as a dot product
`y = x^T * theta`

### Design Matrix
Motivation: the mean squared error involves all observations at once, it would be nice to express our model in terms of all observations, not just one. We can put them into a design matrix.

**Rows:** Correspond to observations. e.g. all features for data point 3
**Columns:** Correspond to features. e.g. feature 1, for all data points

## Residuals
Residuals are the difference between an actual and predicted value, in the regression context. We use the letter `e` to denote residuals, `e_i = y_i - yhat_i`

The mean squared error is equal to the mean of the squares of its residuals.
We can stack all n residuals into a vector, called the residual vector.
`residual vector = true y values - predicted y values`

Residuals are orthogonal to the span of X.
If our model has an intercept term(when our design matrix has a column of all 1s)
  - The sum and mean of the residuals is 0
  - The average true y value is equal to the average predicted y value

**Residual Plots:**
  - With simple linear regression with only 1 independent variables, we plot residuals vs x
  - In the general case, use residuals on y axis vs fitted values on x
  - A good residual plot has no pattern, if there is a curve, this is a sign that transformations or additional variables can help
  - A residual plot should have a similar vertical spread throughout the entire plot. If it doesn't there are probably issues with the accuracy of the predictions

## Unique Solutions
  - There is always at least one model parameter that minimizes average loss.
  - Constant models with a squared loss: a unique solution always exists
  - Simple linear model with a squared loss: Any non constant value has unique mean, SD, correlation coefficient
  - Constant model with absolute loss: Unique when there is an odd number y values, if there is an even number of y values, there are infinitely many solution.

## Invertability of X transpose * X
  - Invertible iff it is full rank
  - X transpose * X and X have the same rank
  - Thus, X^T * X is invertible iff X has rank p + 1 (full column rank)

# Real World Example - Fairness in Housing Appraisal

## Situation
The cook county assessor's office is in charge of assessing property values in order to determine property taxes. 

## Problem
The biased property value assessment resulted in a regressive tax, where rich people paid less and poor people paid more. In addition, rich people appealed more often than poor people, resulting in an even greater reduction of property tax. 

## Solution
1. **Ask a Question:** What do we want to know? How to fairly value things for tax purposes. What are our metrics for success? Have both fairness and transparency in projections.
2. **Data Acquisition and Cleaning:** What data do we have and what do we need? Housing Sales data between 2013-2019, Property Characteristics-ie age, bedrooms, baths, etc.How will we sample more data? Is our sample representative?
3. **Exploratory Data Analysis and Visualization:** What attributes are most predictive of sales price? Which are potentially problematic? Is the data predictive of sales price? 

## Takeaways
1. Accuracy is a necessary, but not sufficient condition of a fair system.
2. Fairness and transparency are context-dependent
3. Learn to work with contexts and consider how your data analysis will reshape them
4. Keep in mind the power and limits of data analysis?

## Questions to Ask