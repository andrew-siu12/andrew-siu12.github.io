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

From the chart above, we can see that around 77.9% of them are non-churn users and 22.1% of them are churn-users. In the full dataset, the distribution is almost the same. This is a sign to suggest that the sample is representative of the population. Also, this is an imbalanced dataset, we have to select an appropriate metrics to evaluate the prediction model.

> ## What is the distribution of gender and level?
<p float="left">
  <img src="https://i.imgur.com/oFSm5ka.png" width="350" /> 
  <img src="https://i.imgur.com/h5qp9pe.png" width="350" />
</p>
More male users use sparkify than female users. The discrepancy between male users and female users is smaller. 52.2% are male users and 47.8% are female users. The subscription level is pretty much the same in full dataset and the sample.  

> ## Churn vs gender and Churn rate vs subscription level
<p float="left">
  <img src="https://i.imgur.com/tKWeTnQ.png" width="350" /> 
  <img src="https://i.imgur.com/ZcHHyw0.png" width="350" />
</p>
As expected both non churn users of male and female are much higher than churn users. The sames goes to subscription level. 

> ## Distribution of page and churn rate

![page](https://i.imgur.com/5hhe1KM.png)
![page_churn](https://i.imgur.com/BredsTw.png)

Many users visit `NextSong` page which is very good for the music streaming business. Thumbs up is another important factor that suggest users like this app. Next, we look at the distribution of page and churn users. As we use  cancellation confirmation page to determine the churn label. We should not include the number of cancellation confirmation as part of our feature. Pages such as `Thumbs up`, `Add to playlist`, `Add Friend` has higher proportion of non-churn users. We can count the number of times users visit such pages to determine if the users are likely to churn or not.

> ## Is there any particular time of the week/day has higher churn rate?
![week_churn](https://i.imgur.com/rCoSu0g.png)
![day_churn](https://i.imgur.com/X3eaRGC.png)

It seems like less users using Sparkify during the weekends. Thus the number of churning users are less than the other days. Moreover, Thursday and Friday have lower churn users than other weekdays. We should create a feature to extract the weekday of a particular record. So machine learning model can capture this relationship. More users using Sparkify from 14:00 to 23:00. But there is no clear relationship between hour and churn rate.

![daily_churn](https://i.imgur.com/H655gvY.png)
Both full dataset and samples start from 2018-10-01 nad end at 2018-12-01. We can look at the trend of the churning status of  users over the period of the dataset. The trend suggest that weekend user counts are lower in both churn users and non-churn users which confrom with the weekday churn plot above. The daily plot also suggests that the number of churn users decrease over the two months period. We may need some extra data to understand why such decrease happens. Due to computational cost, we were not able to compute such plot in the full dataset. It would be interesting to see if such pattern exist in the full dataset as well.


## Does the user device affect the churn rate?
<p float="left">
  <img src="https://i.imgur.com/6Teset3.png" width="400" /> 
  <img src="https://i.imgur.com/fzh8220.png" width="400" />
</p>
By using regex in python, we were able to extract the users' device, os and browser version. In the browser plot, one can see that mobile phone user have significantly higher churning rate than the other browers. The churning rate is 60%, firefox comes second with 26%.  The OS plot also suggest that IPhone users churn more than other OS. These two features should be useful for machine learning models to identify churn users. We shall include them in our features.

# Feature engineering
With the help of EDA, we are able to hand-crafted 16 features. These features includes:
* **total number of artist listened** - This feature identify how many artist each user listens. The more artists listen may indicate how active the user is.
* **Gender (binary), Level (binary)** - Binarize the gender and level feature for machine learning algorithms.
* **total number of downgrades page** -  Since the downgrade page is related to the cancel subsription. The higher the number of downgrade page visit by users, the more likely the user will churn.
* **add to playlist** - Frequent users are more likely to add to playlist than users. So this may be a good feature to identify the frequent users.
* **thumbs down and thumbs up** - Users putting thumbs up may be less likely to churn versus thumbs down users
* **add friend** - Users that add many friends may be less likely to churn
* **total length of time listened** - the length of time users listen indicate how active they are
* **number of songs listened** - similar to total length of time listened
* **number of sessions per user** - more sessions relate to how active users are
* **average songs per session** - aggregations of songs and session feature
* **average session per day/month** - aggregations of songs and session feature
* **location, os and browser features** - As mentioned in previous sessions, os and browser features are useful for modelling. We also use regex to extract the state where users use sparkify. This may be useful to identify which state are more likely to churn.

# Modeling
## Pipeline
``` Python
num_cols= ["total_artist", "gender", "level", "num_downgrade", "num_advert", "num_song_playlist",
           "num_thumbs_down", "num_thumbs_up", "num_friend", "total_length", "num_song",
           "session", "avg_songs", "avg_daily_sessions", "avg_monthly_sessions"]


indexer_os = StringIndexer(inputCol="os", outputCol="os_index")
indexer_browser = StringIndexer(inputCol="browser", outputCol="browser_index")
indexer_browser_ver = StringIndexer(inputCol="browser_ver", outputCol="browser_ver_index")
assemblerCat = VectorAssembler(inputCols=["os_index", "browser_index", "browser_ver_index"], outputCol="cat")

pipelineCat = Pipeline(stages = [indexer_os, indexer_browser, indexer_browser_ver, assemblerCat])
transformed_event = pipelineCat.fit(transformed_event).transform(transformed_event)



assemblerNum = VectorAssembler(inputCols=num_cols, outputCol="Num")
scaler = StandardScaler(inputCol="Num", outputCol="scaled_num")
pipelineNum = Pipeline(stages=[assemblerNum, scaler])
transformed_event = pipelineNum.fit(transformed_event).transform(transformed_event)

assembler = VectorAssembler(inputCols=["cat", "scaled_num"], outputCol="features")

features_pipeline = Pipeline(stages=[assembler])

transformed_event = features_pipeline.fit(transformed_event).transform(transformed_event)
```
We first label encode os, browser and browser version features and use pipeline to process these features. Then we handle numerical features by concatenting to feature vectors and scale all these columns to have mean 0 and standard deviation 1. Avoid numerical instabilities due to large values. We then concatenate the categorical features and numerical features into one feature column.
