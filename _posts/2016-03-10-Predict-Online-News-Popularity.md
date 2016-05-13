---
layout: post
title: Building Predictive Modeling with Decision Tree, Naïve Bayes, Random forest in R.
feature-img: "img/feature_images/news_header.jpg"
---

Online News Popularity data set has been selected from the UC Irvine Machine Learning Repository. The articles in this data set were published by Mashable. This data set is donated on January 8, 2015. The data set includes 39797 observations and 61 combined attributes. The goal of this project is to find non-trivial, potential useful knowledge and predict the popularity of the articles, which is the number of shares in social networks.


* The explanatory attributes for Online News Popularity data set include URL of the article, number of words in the title, number of words in the content, number of images, number of videos, polarity of positive words and polarity of negative words are numeric. These attributes can be used to create a predictive model.
* The response attribute is the number of shares in social network (popularity).

```R
###########Required Library##########
library(rpart.plot)
library(caret)
library(e1071)
library(klaR)
library(rattle)
library(rpart.plot)
library(RColorBrewer)
library(ggplot2)
```

## Data Preprocessing and Exploratory



```R
OnlineNewsPopularity = read.csv("OnlineNewsPopularity.csv", header = TRUE)

###########Data Preprpcessing step##########

#Duplicate dataset
df.onNews <- OnlineNewsPopularity

#Remove ID
df.onNews$url  = NULL
df.onNews$timedelta = NULL

#Change Dummy variables to factors
df.onNews$pubDay <- ifelse(OnlineNewsPopularity$weekday_is_monday == 1, "monday", 
                           ifelse((OnlineNewsPopularity$weekday_is_tuesday == 1), "tuesday", 
                                  ifelse((OnlineNewsPopularity$weekday_is_wednesday == 1), "wednesday", 
                                         ifelse((OnlineNewsPopularity$weekday_is_thursday == 1), "thursday", 
                                                ifelse((OnlineNewsPopularity$weekday_is_friday == 1), "friday", 
                                                       ifelse((OnlineNewsPopularity$weekday_is_saturday == 1), "saturday", "sunday"))))))
df.onNews$pubDay <- as.factor(df.onNews$pubDay)


df.onNews$weekday_is_monday    = NULL
df.onNews$weekday_is_tuesday   = NULL
df.onNews$weekday_is_wednesday = NULL
df.onNews$weekday_is_thursday  = NULL
df.onNews$weekday_is_friday    = NULL
df.onNews$weekday_is_saturday  = NULL
df.onNews$weekday_is_sunday    = NULL


df.onNews$dataChannel <- ifelse(OnlineNewsPopularity$data_channel_is_lifestyle == 1,"lifestyle",
                                ifelse(OnlineNewsPopularity$data_channel_is_entertainment == 1,"entertainment",
                                       ifelse(OnlineNewsPopularity$data_channel_is_bus == 1,"business",
                                              ifelse(OnlineNewsPopularity$data_channel_is_socmed == 1,"Social",
                                                     ifelse(OnlineNewsPopularity$data_channel_is_tech == 1,"tech", "world")))))

df.onNews$data_channel_is_lifestyle = NULL
df.onNews$data_channel_is_entertainment = NULL
df.onNews$data_channel_is_bus = NULL
df.onNews$data_channel_is_socmed = NULL
df.onNews$data_channel_is_tech = NULL
df.onNews$data_channel_is_world = NULL

df.onNews$dataChannel <- as.factor(df.onNews$dataChannel)


#Class distribution Bin data
df.onNews$share2bins <- ifelse(OnlineNewsPopularity$shares > 1300, "high", "low")
df.onNews$share2bins <- as.factor(df.onNews$share2bins)


```


In this data set, we'll define an 66%/34% for training and test data.
```R
set.seed(415)
# define an 66%/34% train/test split of the dataset
trainIndex <- createDataPartition(df.onNews$share2bins, p=0.66, list=FALSE)
data_train <- df.onNews[ trainIndex,]
data_test <- df.onNews[-trainIndex,]
```


## Model Fitting

## Performance Evaluation