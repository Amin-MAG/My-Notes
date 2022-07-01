# Machine Learning

## Introduction

It is a subset of AI which is more statistical.

The process of machine learning consists of

- Aggregation of data

- Cleaning data

- Select the training algorithm and create models

- Training and testing the model 

- Predict new cases

### Types of predictions

- Regression - $CO_2$ emission

- Classification - Cancer detection

- Clustering - Bank loans

- Credit card fraud - Anormaly detection

- Recommender - Netflix recommendations

### Supercised VS Unsupervised

- Supervised - Teaching the model by labeling. (Classification, Regression)

- Unsuperviesed - It works on its own to discover the information without no labeling. (Clustering)

## Regression

Regression is the process of predicting a continuous value. They can be Linear or Non-Linear.

- Simple - Only on independent X

- Multiple - Multiple independant Xs

### Applications

Each scenario that you give the inputs and the result is a continuous value.

- Household Price

- Customer Satisfaction

- Sales Forecast

- Employment Income

### Algorithms

- Ordinal

- Poisson

- Fast Forest quantile

- Linear, Polynominal, Lasso, Stepwise, Ridege

- Bayesian Linear

- Nerual Network

- Decision Forest

- Boosted decision tree

- K-nearest neighbors

### Simple Linear Regression

$$
y=\theta_0 + \theta_1x_1
$$

## Model Evaluation

The goal is to build a model to predict an unknown scenario. There are two main methods

- Train and Test are on same data
  
  - Not always good
  
  - It causes overfitting

- Train and Test are splited

### MAE - Mean Absolute Error

It's so simple.

$$
MAE=\frac{1}{n}\Sigma |y_i-\hat{y}_i|
$$

### MSE - Mean squared Error

It exaggerates the errors.

$$
MSE=\frac{1}{n}\Sigma(y_i-\hat{y}_i)^2
$$

### RMSE

It has the same unit with the first one.

$$
MSE=\sqrt{\frac{1}{n}\Sigma(y_i-\hat{y}_i)^2}
$$

### RAE

$$
RAE=\frac{\Sigma|y_i-\hat{y}_i|}{\Sigma|y_i-\bar{y}_i|}
$$

### RSE

$$
RAE=\frac{\Sigma(y_i-\hat{y}_i)^2}{\Sigma(y_i-\bar{y}_i)^2}
$$

### R2

$$
R^2=1-RSE
$$
