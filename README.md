---
title: 'Logistic Regression: Feature Selection Methods'
author: "Daniel Cho"
output:
  html_document:
    highlight: tango
fontsize: 12pt
header-includes:
- \renewcommand*\familydefault{\sfdefault} %% this picks a sans serif font
- \usepackage[T1]{fontenc}
---

```{r setup, echo=F}
knitr::opts_chunk$set(cache = F)
```

```{r, eval = F, echo=F}
install.packages('tinytex')
tinytex::install_tinytex()
```


 
<font size= "5">**INTRODUCTION**</font>

In the traditional statistical sense, logistic regression is a way to estimate the parameters of a logistic model, where logistic models are commonly used to predict a dichotomous outcome by the linear combination of independent variables. Feature selection method is an automatic process that selects a subset of the independent variables(features) from the existing set of features to create a model. The premise of this method is the removal of features that are redundant and irrelevant without losing too much information. In some cases, these methods can provide models that are easier to interpret, can handle model complexity, and are less computationally intensive. 

For this presentation, we will be utilizing the ["Pima Indians Diabetes"](https://www.kaggle.com/datasets/uciml/pima-indians-diabetes-database) data set to demonstrate the application of feature selection methods for logistic models. Using these methods, our goal is to create a model that can accurately predict our dichotomous outcome(diagnosis of Type 2 diabetes) from a selection of provided health information of patients. In addition, a demonstration of model interpretation for logistic regression will be provided to evaluate the effects of the provided health information of patients to the diagnosis of Type 2 diabetes.

It is important to note that the goal of this presentation is the application of logistic modeling with R. Therefore, many statistical concepts that are used in this presentation will be only be introduced, but links with additional information will be provided throughout the vignette.

### I. Data Cleaning
```{r,echo = F}
diabetes <- read.csv("/Users/dancho/Downloads/diabetes.csv")
```
Summary of the diabetes data set:
```{r}
summary(diabetes)
```
Let's filter out the missing values that are denoted as 0.

```{r, message = F}
library(dplyr)
```
```{r}
diabetes <- filter(diabetes, Glucose > 0, BloodPressure > 0, SkinThickness > 0, Insulin > 0, BMI > 0)
head(diabetes)
```

In this dataset, there are eight candidates for our explanatory/predictor variables which include: Pregnancies, Glucose, BloodPressure, SkinThickness, Insulin, BMI, DiabetesPedigreeFunction, and Age. Our dichotomous response variable is Outcome, which has the values 0 or 1 to denote whether the patient was diagnosed with Type 2 diabetes or not.

### II. Logistic Regression

To fit a logistic regression model in R, we can use the glm function and family = binomial. Instead of the least squares method used in linear regression, this function for logistic regression uses [maximum likelihood estimation and log likelihood function](https://arunaddagatla.medium.com/maximum-likelihood-estimation-in-logistic-regression-f86ff1627b67) to estimate the coefficients, and find the best fitting line.

```{r}
model_test <- glm(Outcome ~ ., data = diabetes)
summary(model_test)
```
