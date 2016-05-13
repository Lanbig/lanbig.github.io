---
layout: post
title: Building Predictive Modeling with Decision Tree, Naïve Bayes, Random forest in R.
feature-img: "img/feature_images/newsheader.jpg"
---

Online News Popularity data set has been selected from the UC Irvine Machine Learning Repository [Link](https://archive.ics.uci.edu/ml/datasets/Online+News+Popularity). The articles in this data set were published by Mashable. This data set is donated on January 8, 2015. The data set includes 39797 observations and 61 combined attributes. The goal of this project is to find non-trivial, potential useful knowledge and predict the popularity of the articles, which is the number of shares in social networks.


* The explanatory attributes for Online News Popularity data set include URL of the article, number of words in the title, number of words in the content, number of images, number of videos, polarity of positive words and polarity of negative words are numeric. These attributes can be used to create a predictive model.
* The response attribute is the number of shares in social network (popularity).

The full source code is in [Github Gist](https://gist.github.com/Lanbig/9c365cbd372c0d7aed81011338893e91)

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

## Data Exploratory
```R
library(ggplot2)
theme_set(theme_bw(base_size = 28))

qplot(df.onNews$dataChannel, geom="histogram",ylab = "count",binwidth = 30,
      main = "Histogram for data channel", xlab = "Channel", fill=df.onNews$share2bins ) 

qplot(df.onNews$pubDay, geom="histogram",ylab = "count",binwidth = 30,
      main = "Histogram for publication day", xlab = "Day", fill=df.onNews$share2bins ) 
```
![Decision Tree](http://{{ site.url }}/img/content_images/pvn4.png)

Most positive cases (high popularity) are captured in Lifestyle, social and tech channel. These channels seem to be much popular than business, entertainment and word channel. For example, Social channel had 76 percent of positive cases (high popularity) and technology channel had 64 percent of positive cases. 


![Decision Tree](http://{{ site.url }}/img/content_images/pvn5.png)
In reviewing positive cases (high popularity) on the publication day, most news shared on weekend seems to be in the “high popularity” group. About 75 percent of the news published on Sunday was popular. The news published on weekday tent to be in “low popularity” group. For example, the majority of the news shared on Wednesday and Tuesday were unpopular.


## Model Fitting

In this data set, we'll define an 66%/34% for training and test data.

```R
set.seed(415)
# define an 66%/34% train/test split of the dataset
trainIndex <- createDataPartition(df.onNews$share2bins, p=0.66, list=FALSE)
data_train <- df.onNews[ trainIndex,]
data_test <- df.onNews[-trainIndex,]
```

**Decision Tree Analysis**

```
fit.rpart8= train(share2bins ~ . ,data=data_train , method= "rpart", tuneLength = 8, trControl=trainControl(method="cv",number=10))

fancyRpartPlot(fit.rpart8$finalModel)
```
![Decision Tree](http://{{ site.url }}/img/content_images/pvn1.png)


**Naïve Bayes Analysis**

```R
> fit.nb= train(share2bins ~ . ,data=data_train , method= "nb", trControl=trainControl(method="cv",number=10))

> fit.nb
Naive Bayes 

26166 samples
   47 predictor
    2 classes: 'high', 'low' 

No pre-processing
Resampling: Cross-Validated (10 fold) 
Summary of sample sizes: 23549, 23550, 23550, 23550, 23549, 23549, ... 
Resampling results across tuning parameters:

  usekernel  Accuracy   Kappa       Accuracy SD  Kappa SD  
  FALSE      0.5198759  0.08688965  0.03318579   0.05532158
   TRUE      0.5998991  0.21353378  0.02759259   0.04304326

```

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
![Random Forest](http://{{ site.url }}/img/content_images/pvn2.png)



## Performance Evaluation
As a result, Naïve Bayes does not work very well with the dataset. Decision tree is one the useful modeling technique since this technique is easy to interpret and understand the result. Moreover, random forest analysis is the best modeling technique with the dataset because it gets the highest accuracy in both training and testing dataset.

![Performance comparision](http://{{ site.url }}/img/content_images/pvn3.png)

