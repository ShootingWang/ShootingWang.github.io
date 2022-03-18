---
title: R语言 | Amelia
date: 2021-02-06 10:28:41
tags: [R,缺失值]
categories: R语言
mathjax: true
toc: true
---

<center></center>
<!--more-->

# Amelia包
Amelia对缺失值的假设为：
- 缺失值是随机缺失的
- 数据中所有变量都满足多元正态分布（MVN，Multivariate Normal Distribution），可以用均值和协方差来描述数据
- 利用Bootstrap生成多组填补值

可视化缺失值

```r
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

install.packages('Amelia')
library(Amelia)

Amelia::missmap(iris.miss)
```

<meta name="referrer" content="no-referrer" />
{% asset_img miss2.png 可视化缺失值 %}

# Amelia vs MICE

||Amelia|MICE|
|:---:|:-----|:------|
|拟合|依赖整体数据服从“多元正态分布”的假设|一个变量一个变量分别拟合|
||只能处理正态分布或者经转换后近似正态分布的变量|可以处理多种类型数据|
||不能在数据子集上处理缺失值|能在数据子集上处理缺失值|

# 参考资料
- [R 中缺失值的简单处理—— MICE 和 Amelia 篇](https://jiangjun.netlify.app/post/r-missing-data/)