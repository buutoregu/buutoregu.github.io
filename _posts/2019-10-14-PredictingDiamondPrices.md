---
layout:     post   				   
title:      Predicting Diamond Prices 				
subtitle:   R Project 
date:       2019-10-14 				
author:     Yvonne Liu						
header-img: img/background.jpg 	
catalog: true 						
tags:								
    - Project
    - R
---

The original dataset can be found on The original dataset can be found on [Kaggle](https://www.kaggle.com/shivam2503/diamonds).

## Business Understanding
The business problem we wanted to address was to be able to quickly, efficiently, and consistently price new diamonds coming into the stock of diamond retailer. We also wanted to determine which characteristics of diamonds contribute the most to the price. 

More specifically, we assumed that we were a group of data analysts in a luxury brand jewelry company who want to build a model for predicting the price of diamonds. Our task was to utilize the information and data on hand to come up with a formula that uses the attributes of diamonds to easily and accurately calculate the value of each diamond. In order to stay competitive in the market, goods must be reasonably priced to compete with competitors, but must also maximize firm profits. Our company can use the model to identify good quality diamonds and fully understand which variables contribute most to diamond price. 

## Data Understanding

The dataset contains **53,940 observations** and **11 variables**. The variables include carat, cut, color, clarity, depth, and price as well as an identifier variable.

We decided to first investigated the correlation between each variable by performing exploratory analysis and plotting. We would then generate several models to predict how certain variables can affect the price of diamonds. This data mining solution, consisting of five models, addressed the defined business problem by helping the company better understand how the characteristics of a diamond related to price. 

## Data Preparation

* Null Values: While there were no variables with explicitly null values, there were several observations in which the variables of x, y, and z, the measurements of diamonds, were classified as zero, which is impossible. These zero values would negatively impact our analysis, and there were very few of these observations, so we dropped the observation if any of those variables equalled zero. 

* Outliers: Next, we generated histograms and plots to investigate individual variables. Outliers were identified and dropped: we excluded observations when depth of diamond is less than 50 or greater than 75 or when table is less than 50 or greater than 90. Finally, we excluded the observations where y is greater than 20 or z is greater than 30. These changes resulted in a total of 36 dropped observations and a final cleaned data set of 53,904 observations. 

* In order to prepare for the modeling, we decided to randomly divide the data into 2 subsets: train and test. We split the data 60% - 40% to the train set and the test set respectively, in order to provide out-of-sample performance results for each of the models we developed. 

## Data Visualization

![Diamond_viz_1](https://user-images.githubusercontent.com/78829814/110559700-8e724e80-80f9-11eb-99eb-45d58ac6c1fd.jpg)

![diamond_viz_2](https://user-images.githubusercontent.com/78829814/110559785-b5c91b80-80f9-11eb-95aa-708f9fa2d3eb.jpg)

