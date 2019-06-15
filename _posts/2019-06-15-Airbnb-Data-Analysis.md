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

> ## How is the demand for Airbnb changing in London?

![Imgur0](https://i.imgur.com/yyEVCyu.png)

There is no available data on the number of bookings over the years. Instead we estimate the demand by looking at the number of reviews across years. According to [Inside Airbnb](http://insideairbnb.com/about.html), around 50% of the guests leave reviews to listings, so the number of reviews is a good estimator of the demand.  The "Number of reviews across years" graph shows an upward trend of the number of reviews which indicates increase in demand of rental listings. It also shows seasonal pattern with some months has reviews less than the others.

<img src="https://i.imgur.com/4PXvH0A.png" width="500" />
<img src="https://i.imgur.com/sTNu3kI.png" width="500" /> 

The other two graphs show monthly reviews in 2017 and 2018. Both graphs show deamand peaked at around August and slowly decreasing until the end of the year. 

>  ## Which month is more expensive to travel on?

![boxplot](https://i.imgur.com/P0j31Zx.png)
The average prices tends to increases along the year except for May which is signifactly lower than the other months. In comparsion to the 'Number of reviews' plots in previous sections, the pattern is similar except for May and December. 

December is more expensive compared to the other months. This may be because december is the holiday season and people are more likely to travel. May is the cheapest month to make a booking in Airbnb, perhaps it's closer to exam period. Moreover, the data is scraped on May, and many people would book at last minute to get a cheaper price.


> ## Whcih boroughs are more expensive, and which areas have the best reviews?

The London boroughs are the 32 local authority districts that make up Greater London; each is governed by a London borough council. The detailed list of London boroughs can be found in this [link](https://en.wikipedia.org/wiki/List_of_London_boroughs).
![location](https://i.imgur.com/zaDakuw.png)
It is not surprised that Kensington and Chelsea borough is the most expensive borough to book a property. This borough is closed to many of the museums and department stores such as Harrods, Peter Jones and Harvey Nichols. Moreover, many of the most expensive residential properties in the world lie in this brough. The second one is the City of Westminster. This borough closes to many of the famous London landmarks (blue marker on the map), shopping areas and night-time entertainment area Soho. In general, inner london is more expensive than than outer london area.

![location1](https://i.imgur.com/w3G784r.png)
All of the london boroughs has average score ratings higher than 90 out of 100. As opposed to the price, outer london has higher review score ratings than inner london. Richmond upon Thames is the highest scores rating borough. The borough is home to open space such as Richmond Park, Kew Gardens, Bushy Park and Old Deer Park.

> ## What are the main factors that influence pricing on Airbnb listings?

![feature](https://i.imgur.com/QRe4iWm.png=300x)

The features that contribute most to the variance in prices are:

* accommodates             
* room type                 
* Number of bedrooms                   
* cleaning fee               
* the borough the listing in   
* The number of listings the host has 
* The number of nights are avaliable to be booked in the next 90 days                        
* listing id   
* bathrooms  
* fee for extra person    

The top 2 features the number of people the property accommodates and room type contribute almost 43% to the price. This make snese because more people a property can live in, the higher the price is. It is the first thing we look at when we make a booking. Obviously if the room type is a private room, the price tends to be lower than the entire home/apartment.  

Cleaning fee and the borough the listing in are also important features that influence the price. Interestingly the number of listings the host has is in the top 10 features. Maybe because these hosts are professional property management that is more realiable than individuals. Hence, the price is more expensive.  

## Summary

* The demand for Airbnb increased exponentially over the years.
* Demand increases along the year until August and slowly decrease.
* The cheapest month to book Airbnb is May.
* Kensington and Chelsea borough is the most expensive borough to book a property. City of Westminster comes second. Both of the     b  boroughs are closed to famous London Landmarks.
* Richmond upon Thames borough has the highest average review score ratings. 
* Two main factors that influence the pricing of listings are accommodates and room type.  
* Our best model to predict the price is using Catboost which has a r2 score of 73.9%. This means that the model explain 73.9% of the     variability in listing price. The result was not good enough. However, price is very different to predict correcly, and the remaining   26.1% may be explained by features that are not in the data.


