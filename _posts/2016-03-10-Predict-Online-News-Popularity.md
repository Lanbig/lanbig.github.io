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
######### check it out from my github. ##############


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
**Decision Tree Analysis**
```
fit.rpart8= train(share2bins ~ . ,data=data_train , method= "rpart", tuneLength = 8, trControl=trainControl(method="cv",number=10))

fancyRpartPlot(fit.rpart8$finalModel)
```
![Decision Tree](http://{{ site.url }}/img/content_images/viz1.png)


**Naïve Bayes Analysis**

**Random Forest Analysis**
```R
> fit.rf = train(share2bins ~ . ,data_train , method= "rf", ntree=501 , tuneGrid = data.frame(mtry = 4), 
+                allowParallel=TRUE,trControl=trainControl(method="cv",number=10) )
> 
 
Random Forest 

26166 samples
   47 predictors
    2 classes: 'high', 'low' 

No pre-processing
Resampling: Cross-Validated (3 fold) 
Summary of sample sizes: 17444, 17444, 17444 
Resampling results

  Accuracy   Kappa      Accuracy SD  Kappa SD  
  0.6651762  0.3222725  0.006759968  0.01355711

Tuning parameter 'mtry' was held constant at a value of 4
```
![Random Forest](http://{{ site.url }}/img/content_images/viz2.png)



## Performance Evaluation
As a result, Naïve Bayes does not work very well with the dataset. Decision tree is one the useful modeling technique since this technique is easy to interpret and understand the result. Moreover, random forest analysis is the best modeling technique with the dataset because it gets the highest accuracy in both training and testing dataset.
![Performance comparision](http://{{ site.url }}/img/content_images/viz3.png)

