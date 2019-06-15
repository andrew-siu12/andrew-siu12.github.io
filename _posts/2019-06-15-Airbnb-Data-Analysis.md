---
layout: post
title: Exploring Airbnb Listings in London
subtitle: Exploratory analysis and Machine Learning
gh-repo: andrew-siu12/Airbnb-data-analysis
# gh-badge: [star, fork]
tags: [data science]
comments: true
---

![airbnb](https://a0.muscache.com/im/pictures/91c33d06-c95b-46e5-819d-f05671225bc6.jpg?aki_policy=xx_large)
*Source: [Airbnb](https://www.airbnb.co.uk/rooms/17569968?location=London%2C%20United%20Kingdom&_set_bev_on_new_domain=1559511782_3PA3AEi93KyZ9PKJ&source_impression_id=p3_1560609696_TLDyoWw94YlR44Ry)*

# Introduction

Airbnb is one of the most popular online community marketplace for people ('hosts) to list properties, book experiences and discover places. Hosts are reponsible for setting prices for the listings. It is hard for newcomers to set an accurate price to compete with other experience hosts. 

As one of the most popular cities in Europe, London has over 80,000 listings as of May 2019. In such fierce competition environments, it is important to know which factors driving the price of listings. In this post, we will perform data analysis to extract useful insights about rental landscape in London. And applying machine learning models to predict the price for listings in London. 


# Dataset

[Inside Airbnb](http://insideairbnb.com/get-the-data.html) has provided data that is sourced from public available infomration from Airbnb webiste. The data we used for this project is compiled on 05 May, 2019. The dataset comprised of three tables and a geojson file of London boroughs:
* `listings` - Deatailed listings data for London
* `calendar` - Deatailed bookings for the next calendar year of listings
* `reviews` - Detailed reviews data for listings in London.
* `neigbourhoods` - geojson file of boroughs of London.

# Exploratory Analysis

> **How is the demand for Airbnb changing in London?**

![Imgur0](https://i.imgur.com/yyEVCyu.png)

There is no available data on the number of bookings over the years. Instead we estimate the demand by looking at the number of reviews across years. According to [Inside Airbnb](http://insideairbnb.com/about.html), around 50% of the guests leave reviews to listings, so the number of reviews is a good estimator of the demand.  The "Number of reviews across years" graph shows an upward trend of the number of reviews which indicates increase in demand of rental listings. It also shows seasonal pattern with some months has reviews less than the others.

<img src="https://i.imgur.com/4PXvH0A.png" width="500" />
<img src="https://i.imgur.com/sTNu3kI.png" width="500" /> 

The other two graphs show monthly reviews in 2017 and 2018. Both graphs show deamand peaked at around August and slowly decreasing until the end of the year. 

> **Which month is more expensive to travel on?**

![boxplot](https://i.imgur.com/P0j31Zx.png)
The average prices tends to increases along the year except for May which is signifactly lower than the other months. In comparsion to the 'Number of reviews' plots in previous sections, the pattern is similar except for May and December. 

December is more expensive compared to the other months. This may be because december is the holiday season and people are more likely to travel. May is the cheapest month to make a booking in Airbnb, perhaps it's closer to exam period. Moreover, the data is scraped on May, and many people would book at last minute to get a cheaper price.
