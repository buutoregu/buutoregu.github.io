---
layout:     post
title:      Model Selection
subtitle:   OOS performance and cross validation
date:       2019-10-08
author:     Aly Hu
header-img: img/post-bg-ios9-web.jpg
catalog: 	 true
tags:
    - OOS
    - Cross Validation
---

# Model Selection

Now we have several models to make prediction for us. But how do we evaluate them? 


## Out of Sample(OOS) Metrics

In-sample metrics, such as in-sample R square, always increase with more coefficients. Models have tendency to fit as many records as they can to get a perfect in-sample score. This will lead to **overfitting**(Use LASSO to deal with overfitting). 

That's why we need to use out of sample metrics to measure models performance. 

![OOS explaination](https://tva1.sinaimg.cn/large/006y8mN6ly1g7swgskevgj30ht06bjs5.jpg)

For predictive models, we can use:

* OOS R Square

* Mean Absolute Percentage Error (MAPE)

For classification models, we can use:

* Classification Accuracy(FPR & TPR)

## K-fold Cross-Validation

Given a training dataset, we split it into K pieces(K folds). We use K-1 folds to train the model, and use the other one to test the accuracy. Repeat this for each fold(which is K times), and calculate the average OOS performance score.

We always use ***for loop*** to do this in R.

**Step 1**, we split the data into k folds by creating a random fold id index.

```
set.seed(1)
nfold <- 3
n <- nrow(train_data)
foldid <- rep(1:nfold,each=ceiling(n/nfold))[sample(1:n)]
```
**Step 2**, we create a *m* * *k* data frame for *m* models and *k* folds.

```
LM.OOS <- data.frame(LM=rep(NA,nfold)) 
LMS.OOS <- data.frame(LMS=rep(NA,nfold))
```

**Step 3**, we use for loop to train the data for k times, and record the OOS score for each fold and each model in the data frame we created in step two.

```
for(k in 1:nfold){ 
  train <- which(foldid!=k)
  
  ### Cross Validation for Linear regression model
  glm_train <- glm(Obama_margin_percent~., data=train_data[train,])
  predglm <- predict(glm_train, newdata = train_data[-train,])
  LM.OOS$LM[k] <- R2(y=y_var[-train], pred=predglm)
  
  ### Cross Validation for Stepwise Linear regression model
  glm_train_full <- glm(Obama_margin_percent~., data=train_data[train,])
  glm_train_step <- stepAIC(glm_train_full, direction = "both", trace = FALSE)
  predstepglm <- predict(glm_train_step, newdata = train_data[-train,], type="response")
  LMS.OOS$LMS[k] <- R2(y=y_var[-train], pred=predstepglm)
}
```

**Step 4**, plot the result and choose the model with the highest OOS performance score!



