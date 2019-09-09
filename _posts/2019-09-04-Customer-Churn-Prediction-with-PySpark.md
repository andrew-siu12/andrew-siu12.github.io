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
There are two dataset made avaliable, a full dataset (12GB) and a tiny dataset (240Mb) sample from the full dataset. We will use the subset for data exploration, feature engineering and model selection on local machine. Its more time and computationally efficient to do all the modelling work in a small dataset than working on full dataset. If the subset of data is indeed representative of the full dataset, feature engineering and hyperparameters tuned in the sample dataset should applicable to the full dataset. Once the feature enginnering and the best model are identified, we will use the model for modelling the full dataset on Amazon AWS EMR cluster. 
