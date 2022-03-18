---
title: R语言 | MICE
date: 2021-02-06 09:31:08
tags: [R,缺失值]
categories: R语言
mathjax: true
toc: true
---

<center>处理缺失值</center>

<!--more-->

# 缺失值
缺失值总体分为两类：
1. 随机缺失（MAR，Missing at Random）
2. 非随机缺失（MNAR，Missing NOT at Random）

> 一般来说，认为缺失5%以内的缺失值可以接受。如果哪个变量或观测（样本）的缺失超过5%，可能就需要考虑将其删除。

# MICE包
全名：Multivariate Imputation by Chained Equations
- 假定数据的缺失是MAR（随机缺失）
- 随机缺失意味着某个值的缺失可能是依赖于其他值的
- MICE填补缺失值：使用其他变量对缺失变量$X_1$进行回归，用回归结果填补$X_1$的缺失

默认情况下，对连续型数据采用线性回归进行拟合填补，对分类型变量采用逻辑回归进行拟合填补。

MICE使用的拟合方法：
- 数值型变量：PMM（Predictive Mean Matching）
- 二分类变量：logreg（Logistic Regression）
- 多分类变量：polyreg（Bayesian Polytomous Regression）
- 有序分类变量：（Proportional Odds Model）

```r
install.packages("mice")  ## 安装
```


```r
install.packages('mice')
library(mice)

install.packages('missForest')
library(missForest)

data(iris)
summary(iris)  ## 描述性统计
# Sepal.Length    Sepal.Width     Petal.Length    Petal.Width          Species  
# Min.   :4.300   Min.   :2.000   Min.   :1.000   Min.   :0.100   setosa    :50  
# 1st Qu.:5.100   1st Qu.:2.800   1st Qu.:1.600   1st Qu.:0.300   versicolor:50  
# Median :5.800   Median :3.000   Median :4.350   Median :1.300   virginica :50  
# Mean   :5.843   Mean   :3.057   Mean   :3.758   Mean   :1.199                  
# 3rd Qu.:6.400   3rd Qu.:3.300   3rd Qu.:5.100   3rd Qu.:1.800                  
# Max.   :7.900   Max.   :4.400   Max.   :6.900   Max.   :2.500

install.packages("dplyr")
library(dplyr)  ## 为了使用 %>%

## 随机产生10%的缺失值
set.seed(2021)
iris.miss <- missForest::prodNA(iris, noNA = 0.1) %>% 
  select(-Species)  ## 先去掉分类变量Species
summary(iris.miss)
# Sepal.Length    Sepal.Width     Petal.Length    Petal.Width  
# Min.   :4.300   Min.   :2.000   Min.   :1.000   Min.   :0.10  
# 1st Qu.:5.100   1st Qu.:2.800   1st Qu.:1.500   1st Qu.:0.30  
# Median :5.700   Median :3.000   Median :4.400   Median :1.30  
# Mean   :5.788   Mean   :3.045   Mean   :3.744   Mean   :1.22  
# 3rd Qu.:6.400   3rd Qu.:3.300   3rd Qu.:5.100   3rd Qu.:1.90  
# Max.   :7.900   Max.   :4.200   Max.   :6.900   Max.   :2.50  
# NA's   :12      NA's   :9       NA's   :17      NA's   :18 

## 可视化缺失
md.pattern(iris.miss)
```

<meta name="referrer" content="no-referrer" />
{% asset_img miss1.png 可视化缺失值 %}

> - 每一行是一种缺失模式
> - 左边的数字表示有多少个样本是这种缺失模式
> - 下方的数字表示每列（每个变量）的缺失值个数
> - 右边的数字表示每行（每个样本）的缺失值个数


## mice 


```r
data_imputed <- mice(iris.miss, m = 5, maxit = 50, method = 'pmm', seed = 206)
## m = 5：生成5个填补好的数据
## maxit = 50：每次产生填补数据的迭代次数为50次
## method = 'pmm'：填补方式选择Predictive Mean Matching
summary(data_imputed)
# Class: mids
# Number of multiple imputations:  5 
# Imputation methods:
#   Sepal.Length  Sepal.Width Petal.Length  Petal.Width 
# "pmm"        "pmm"        "pmm"        "pmm" 
# PredictorMatrix:
#   Sepal.Length Sepal.Width Petal.Length Petal.Width
# Sepal.Length            0           1            1           1
# Sepal.Width             1           0            1           1
# Petal.Length            1           1            0           1
# Petal.Width             1           1            1           0

data_imputed$imp$Sepal.Width ## 生成的Sepal.Width值
# 1   2   3   4   5
# 5   3.4 3.8 3.0 2.7 3.4
# 14  3.8 2.5 2.9 2.8 3.1
# 16  3.8 3.9 3.5 4.2 4.2
# 38  3.1 3.4 3.0 3.0 3.6
# 41  3.4 3.5 3.7 3.7 3.5
# 42  3.1 2.9 2.9 3.4 3.2
# 124 3.4 2.5 2.5 2.8 3.3
# 144 2.6 2.5 2.9 3.0 2.7
# 150 3.0 3.1 3.2 3.9 3.3
dim(data_imputed$imp$Sepal.Width)
# [1] 9 5
# Sepal.Width有9个缺失，这里生成了5组数据

data_complete <- mice::complete(data_imputed, 3)
## 取5组中的第3组
sum(is.na(data_complete))
# [1] 0

head(data_complete)
# Sepal.Length Sepal.Width Petal.Length Petal.Width
# 1          5.1         3.5          1.4         0.2
# 2          4.9         3.0          1.4         0.1
# 3          4.7         3.2          1.3         0.2
# 4          4.6         3.1          1.5         0.2
# 5          5.0         3.0          1.4         0.2
# 6          5.4         3.9          1.7         0.2
```



```r

```

# 参考资料
- [R 中缺失值的简单处理—— MICE 和 Amelia 篇](https://jiangjun.netlify.app/post/r-missing-data/)
