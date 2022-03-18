---
title: Data Balancing | SMOTE
date: 2020-05-29 22:27:24
tags: [算法,机器学习,数据不平衡]
categories: 机器学习
hide: true
notshow: true
mermaid: true
mathjax: true
math: true
---

<center>Synthetic Minority Oversampling Technique</center>
<!--more-->

合成少数类过采样技术

# 类不平衡
类不平衡（Class-imbalance）是指在训练分类器时所使用的训练数据的类别分布不均。

> 一个二分类问题，训练数据集有1000个样本
- 理想情况：正类和负类样本的数量相差不多
- 类不平衡：如果正类样本有990个，而负类样本只有10个

- 把样本数量过少的类别称为**少数类**
- 把样本数量较多的类别称为**多数类**

类不平衡的解决方案有：
1. 过采样（Oversampling）：对少数类进行过采样，合成新的样本来缓解类不平衡
2. 欠采样（Undersampling）：对多数类进行欠采样，抛弃一些样本来缓解类不平衡



# SMOTE
合成少数类过采样技术（SMOTE，Synthetic Minority Oversampling Technique），基于随机过采样算法的一种数据不平衡改进方案。

{% note default %}
Chawla N V , Bowyer K W , Hall L O , et al. [SMOTE: Synthetic Minority Over-sampling Technique](https://arxiv.org/pdf/1106.1813.pdf)[J]. Journal of Artificial Intelligence Research, 2002, 16(1):321-357.
{% endnote %}



# 参考资料
- [机器学习 —— 类不平衡问题与SMOTE过采样算法](https://www.cnblogs.com/Determined22/p/5772538.html)
