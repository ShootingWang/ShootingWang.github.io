---
title: Machine Learning | Boosting Tree
date: 2020-05-15 17:04:15
tags: [算法,机器学习,集成学习,Boosting,分类,回归]
categories: 机器学习
math: true
mathjax: true
hide: true
notshow: true
---

<center>提升树</center>
<!--more-->

Tree Ensemble methods
- 不需要归一化（normalization)
- 能够学习到特征之间的高阶交互
- 可用于分类、回归、排序、……


# Boosting Tree
- 提升树是以分类树或回归树为基本分类器的提升方法
- 模型：加法模型[^1]与前向分步算法
- 基学习器：决策树，即决策树桩[^2]

1. **二叉分类树**：分类问题决策树
2. **二叉回归树**：回归问题决策树


[^1]: 加法模型即基学习器的线性组合
[^2]: 决策树桩（decision stump）：由一个根结点直接连接两个叶节点的简单决策树


{% note danger %}
## 前向分步算法

**输入**: 
 - 样本特征数据$x_1,\cdots,x_N$
 - 样本因变量数据$y_1,\cdots,y_N$
 - 初始提升树$H_0(x)=h_0(x)=0$

**输出**： 提升树
 $$H(x)=\sum_{t=1}^Th_t(x;\theta_t)$$

- 对于$t=1,2,\cdots,T$，
  1. 要训练一棵决策树$h_t(x;\theta_t)$
  2. 提升树为
    $$H_t(x)=H_{t-1}(x)+h_t(x;\theta_t)$$
  3. 通过经验风险最小化确定该棵决策树的参数$\theta_t$
   $$\hat{\theta}_t=\arg{\min_{\theta_t}{\sum_{i=1}^NL\left(y_i, H_{t-1}(x_i)+h_t(x_i;\theta_t) \right)}}$$
{% endnote %}

## 损失函数
针对不同问题的提升树学习算法，使用的损失函数不同
- 回归问题：平方误差损失函数
- 分类问题：指数损失函数
- 一般决策问题：一般损失函数

{% note default %}
对于二分类问题，若将{% post_link 算法-AdaBoost AdaBoost算法 %}中的基分类器限定为**二类决策树**，则为提升树。
此时，Boosting Tree是AdaBoost的特殊情况。
{% endnote %}


{% note danger %}
## 回归提升树
**输入**：训练数据集$\{(x_1,y_1),\cdots, (x_N,y_N)\}$
  $x_i\in X\subseteq \mathbb{R}^p,\quad y_i\in Y\subseteq \mathbb{R}$

**输出**：提升树
  $$H(x)=\sum_{t=1}^Th_t(x)$$

1. 初始化提升树$H_0(x)=0$
2. 对$t=1,\cdots,T$($T$为迭代次数)，
  1. 计算上一轮迭代的残差
    $$r_i^{(t)}=y_i-H_{t-1}(x_i),\quad i=1,\cdots,N.$$
  2. 拟合残差$r_i^{(t)}$学习得到一棵回归树
    $$h_t(x;\theta_t)$$
  3. 更新提升树
    $$H_t(x)=H_{t-1}(x)+h_t(x;\theta_t)$$
{% endnote %}


## Objective
$$Obj=\sum_{i=1}^nl(y_i,\hat{y}_i)+\sum_{k=1}^K\Omega(f_k)$$

- 如何定义树的复杂度$\Omega(f_k)$？
 - 树的结点数、树的深度
 - 叶子权重的$l2$范数
 - ……

- 信息增益information gain → 训练误差
- 剪枝prunning → 由结点数定义的正则化
- 最大深度max depth → 函数空间的约束
- smoothing leaf values → 叶子权重的$l2$正则

## 类别
- **Gradient Boosted Machine**：使用平方损失$l(y_i,\hat{y}_i)=(y_i-\hat{y}_i)^2$
- **LogitBoost**：使用Logistic损失
$$l(y_i,\hat{y}_i)=y_i\ln{(1+e^{-\hat{y}_i})}+(1-y_i)\ln{(1+e^{\hat{y}_i})}$$

## Pre-stopping
- stop split if the best split have negative gain
- but maybe a split can benefit future splits

## Post-Prunning
事后剪枝
- grow a tree to maximum depth, recursively prune all the leaf splits with negative gain


# 参考资料
- [Tianqi Chen - Introduction to Boosted Trees](https://homes.cs.washington.edu/~tqchen/data/pdf/BoostedTree.pdf)
- [李航-统计学习方法](https://book.douban.com/subject/10590856/)
- []()
