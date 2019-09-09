---
layout: post
title: Customer Churn Prediction with PySpark
subtitle: Exploratory analysis and Machine Learning
gh-repo: https://github.com/andrew-siu12/Customer-Churn-Prediction-with-PySpark/tree/master
# gh-badge: [star, fork]
tags: [data science]
comments: true
---


![music](https://images.pexels.com/photos/1626481/pexels-photo-1626481.jpeg?auto=compress&cs=tinysrgb&dpr=2&h=750&w=1260)
*Source: [Stas Knop](www.pinterest.ru/stasknop/pins/)*

# Overview

In this project, we will be creating a predictive model for customer churn prediction of a fictional music streaming service: Sparkify (similar to Spotify and Pandora). Churn prediction is about making use of customer data to predict the likelihood of customers discontinuing their subscription in the future. Predicting churn rate is important because it can help Sparkify to identify and improve areas where customer services is lacking. So churn prediction helps to prevent individuals from discontinuing their subscription. According to [ClickZ](https://www.clickz.com/are-ecommerce-customer-retention-strategies-improving/105454/), the probability of selling to an existing customer is 60 - 70%. However, the probability of selling to a new prospect is just 5-20%.
So it is very important to keep the existing customer around.

# Data
There are two dataset made avaliable, a full dataset (12GB) and a tiny dataset (240Mb) sample from the full dataset. We will use the subset for data exploration, feature engineering and model selection on local machine. Its more time and computationally efficient to do all the modelling work in a small dataset than working on full dataset. If the subset of data is indeed representative of the full dataset, feature engineering and hyperparameters tuned in the sample dataset should applicable to the full dataset. Once the feature enginnering and the best model are identified, we will use the model for modelling the full dataset on Amazon AWS EMR cluster. There are 18 features in both the full dataset and 240mb dataset.
```
root
 |-- artist: string (nullable = true)
 |-- auth: string (nullable = true)
 |-- firstName: string (nullable = true)
 |-- gender: string (nullable = true)
 |-- itemInSession: long (nullable = true)
 |-- lastName: string (nullable = true)
 |-- length: double (nullable = true)
 |-- level: string (nullable = true)
 |-- location: string (nullable = true)
 |-- method: string (nullable = true)
 |-- page: string (nullable = true)
 |-- registration: long (nullable = true)
 |-- sessionId: long (nullable = true)
 |-- song: string (nullable = true)
 |-- status: long (nullable = true)
 |-- ts: long (nullable = true)
 |-- userAgent: string (nullable = true)
 |-- userId: string (nullable = true)
```
Column `firstName`, `gender`, `lastName`, `registration`, `location`, `userAgent` and `userId` have the same amount of missing values. We shall remove all these null values as we cannot identity their `userId`.

For the missing values in artist, length, song column, we fill with None or 0 as some people may just login the app and not listen to any songs.

Finally, we define the churn label before performaing EDA on the dataset. The churn label is defined as the users who confirmed the cancellation of subscription. 



# Exploratory Analysis

> ## What is the churn rate of Sparkify?

![churn_rate](https://i.imgur.com/2TQEB19.png)

From the chart above, we can see that around 77.9% of them are non-churn users and 22.1% of them are churn-users. In the full dataset, the statistics is almost the same. This is a sign to suggest that the sample is representative of the 12GB dataset. Also, this is an imbalanced dataset, we have select an appropriate metrics to evaluate the precition model.

> ## What is the distribution of gender and level?
<p float="left">
  <img src="https://i.imgur.com/oFSm5ka.png" width="400" /> 
  <img src="https://i.imgur.com/h5qp9pe.png" width="400" />
</p>
