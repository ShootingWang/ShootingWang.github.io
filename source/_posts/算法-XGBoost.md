---
title: Machine Learning | XGBoost
date: 2020-05-15 15:11:38
tags: [算法,机器学习,集成学习,Boosting]
categories: 机器学习
math: true
mathjax: true
hide: true
notshow: true
---

<center>eXtreme Gradient Boosting</center>
<!--more-->



# XGBoost
> 作者：陈天奇
论文：[XGBoost: A Scalable Tree Boosting System](http://dmlc.cs.washington.edu/data/pdf/XGBoostArxiv.pdf)
- 是梯度提升树框架下的一种机器学习算法


## 实例
- [Awesome XGBoost](https://github.com/dmlc/xgboost/blob/master/demo/README.md)

# XGBoost vs GBDT

||GBDT|XGBoost|
|:--:|:----:|:------:|
|**基分类器**|CART|还支持线性分类器|
|**优化**|梯度下降法[^1]|牛顿法[^2]|
|**正则项**[^3]|没有|有|
|**Learning rate**||Shrinkage|
|**列抽样**||支持列抽样[^4]|
|**缺失值**||可以自动学习出缺失值的分裂方向|

[^1]: 在优化时只用到一阶导数信息
[^2]: XGBoost对代价函数进行了二阶泰勒展开，同时使用了一阶导数和二阶导数的信息；XGBoost还支持自定义代价函数
[^3]: 正则项降低了模型的Variance，学习得到的模型复杂度更低（防止过拟合）
[^4]: XGBoost借鉴了随机森林的做法，支持列抽样（column sampling），既能降低过拟合，还能减少计算



# 参考资料
- [Story and Lessons Behind the Evolution of XGBoost](https://homes.cs.washington.edu/~tqchen/2016/03/10/story-and-lessons-behind-the-evolution-of-xgboost.html)
- [机器学习算法中 GBDT 和 XGBOOST 的区别有哪些？](https://www.zhihu.com/question/41354392)
- []()