---
title: "Human Activity Recognition Study"
author: "David Freifeld"
date: "Thursday, May 21, 2015"
output: html_document
---

## Preprocessing

We first read in our data sets (training and testing), and then extract only the variables we want to use. These variables are just the readings from the fitness device, not including the "summary" variables. 


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
#modFit <- train(classe ~ ., data = train, method = "rf")
#modFit
```

