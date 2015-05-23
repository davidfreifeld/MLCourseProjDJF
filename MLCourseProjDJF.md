---
title: "Human Activity Recognition Study"
author: "David Freifeld"
date: "Thursday, May 21, 2015"
output: html_document
---

## Preprocessing

We first read in our data sets (training and testing), and then extract only the variables we want to use. These variables are just the readings from the fitness device, not including the "summary" variables. We decide to omit the user_name variable because the goal of this study is to be able to predict incorrect weightlifting behaviors, regardless of who is performing them. Similarly, we decided that the timestamp of each observation is not helpful in our analysis.


```r
training <- read.csv("pml-training.csv", stringsAsFactors=F)
testing <- read.csv("pml-testing.csv", stringsAsFactors=F)

colNums <- c(8:11,37:49,60:68,84:86,102,113:124,140,151:160)

train <- training[,colNums]
test <- testing[,colNums]

for (i in 1:52) {
    train[[i]] <- as.numeric(train[[i]])
    test[[i]] <- as.numeric(test[[i]])
}

train$classe <- as.factor(train$classe)
```

## Building the Model

Now we will build our model. We will use the caret package to perform cross validation of a random forest model:


```r
library(caret)
modFit <- train(classe ~ ., data = train, method = "rf")
modFit
```

```
## Random Forest 
## 
## 19622 samples
##    52 predictor
##     5 classes: 'A', 'B', 'C', 'D', 'E' 
## 
## No pre-processing
## Resampling: Bootstrapped (25 reps) 
## 
## Summary of sample sizes: 19622, 19622, 19622, 19622, 19622, 19622, ... 
## 
## Resampling results across tuning parameters:
## 
##   mtry  Accuracy   Kappa      Accuracy SD  Kappa SD   
##    2    0.9928369  0.9909359  0.001338007  0.001691376
##   27    0.9923227  0.9902853  0.001415289  0.001788122
##   52    0.9846846  0.9806194  0.003482837  0.004404650
## 
## Accuracy was used to select the optimal model using  the largest value.
## The final value used for the model was mtry = 2.
```

The train function uses random subsampling to perform a cross validation of the model. That is - it splits the data randomly into a training and a test set, creates a model using the training set, and then records the accuracy on the predictions of the test set. We can see that the cross validation of the random forest model settled on an mtry parameter of 2. And it estimated an out-of-sample accuracy of 0.993. Therefore we would expect an error of (best-case) approximately 0.007 when making predictions on our test set.


