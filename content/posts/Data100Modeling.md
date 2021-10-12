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
To determine optimal a and b, choose a loss function

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

## Questions to Ask