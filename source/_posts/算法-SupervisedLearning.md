---
title: Machine Learning | Supervised Learning
date: 2020-05-15 17:08:41
tags: [算法,机器学习]
categories: 机器学习
math: true
mathjax: true
hide: true
notshow: true
---
<center>有监督学习</center>
<!--more-->



# Supervised Learning

## Model

## Parameters

## Objective Function
目标函数
$$Obj(\Theta)=L(\Theta)+\Omega(\Theta)$$
$$Training Loss + Regularization$$

||**Training Loss**|**Regularization**|
|:--:|:--|:----|
|**measure**|how well model fit on training data|complexity of model|
|optimize...to encourages|predictive models[^1]|simple models[^2]|

[^1]: Fitting well in training data at least get you close to training data which is hopefully close to the underlying distribution. ——from [Tianqi Chen - Introduction to Boosted Trees](https://homes.cs.washington.edu/~tqchen/data/pdf/BoostedTree.pdf)
[^2]: Simpler models tends to have smaller variance in future predictions, making prediction stable. ——from [Tianqi Chen - Introduction to Boosted Trees](https://homes.cs.washington.edu/~tqchen/data/pdf/BoostedTree.pdf)


### Training Loss
训练误差**Training Loss**: measures <u>how well model fit on training data</u>
$$L(\Theta)=\sum_{i=1}^nl(y_i,\hat{y}_i)$$
- Square Loss
$$l(y_i,\hat{y}_i)=(y_i-\hat{y}_i)^2$$
- Log-Loss
$$l(y_i,\hat{y}_i)=y_i\ln{(1+e^{-\hat{y}_i})}+(1-y_i)\ln{(1+e^{\hat{y}_i})}$$

### Regularization
正则化**Regularization**: measures <u>complexity of model</u>
$$\Omega(\Theta)$$
- $l1$-norm 
$$\Omega(\omega)=\lambda||w||_1$$
- $l2$-norm
$$\Omega(\omega)=\lambda||w||^2$$




# 参考资料
- [Tianqi Chen - Introduction to Boosted Trees](https://homes.cs.washington.edu/~tqchen/data/pdf/BoostedTree.pdf)
- []()
- []()