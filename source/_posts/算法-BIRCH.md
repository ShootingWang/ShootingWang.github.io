---
title: Machine Learning | BIRCH
date: 2020-06-03 14:22:23
tags: [机器学习,聚类,算法]
categories: 机器学习
mathjax: true
toc: true
hide: true
---

<center>Balanced Iterative Reducing and Clustering Using Hierarchies</center>
<!--more-->

利用层次方法的平衡迭代规约和聚类

# BIRCH
- 是一种针对大规模数据集的聚类算法
- 只需单遍扫描数据集就能进行聚类

## 聚类特征
聚类特征（CF，Clustering Feature）
- 是BIRCH增量聚类算法的核心
- 使用CF概括描述各簇的信息

{% note warning %}
设某簇中有$N$个$p$维数据点$\{\mathbf{x}_i\}\quad (i=1,\cdots,N)$，则该簇的**聚类特征（CF）**定义为三元组：
$$CF=(N, LS, SS)$$
其中：
- $N$：簇中点的数目
- 矢量$LS$：各点的线性求和，即
 $$\sum_{i=1}^N\mathbf{x}_i=\left(\sum_{i=1}^Nx_{i1},\cdots, \sum_{i=1}^Nx_{ip} \right)^T$$
- 标量$SS$：各点的平方和，即
 $$\sum_{i=1}^N\mathbf{x}_i^T\mathbf{x}_i=\sum_{i=1}^N\sum_{j=1}^px_{ij}^2$$
{% endnote %}


{% note success %}
聚类特征（CF）具有**可加性**。

若有$CF_1=(n_1,LS_1,SS_1)$, $CF_2=(n_2,LS_2,SS_2)$，

则$CF_1+CF_2=(n_1+n_2,LS_1+LS_2, SS_1+SS_2)$表示将两个不相交的簇合并成一个大簇的聚类特征。
{% endnote %}

聚类特征本质上是给定簇的统计汇总，可以有效地对数据进行压缩。

基于聚类特征，可以推导出簇的许多统计量和距离度量。

{% note warning %}
设某簇中有$N$个$p$维数据点$\{\mathbf{x}_i\}\quad (i=1,\cdots,N)$，则该簇的
- **簇质心**
 $$\mathbf{x}_0=\frac{1}{N}\sum_{i=1}^N\mathbf{x}_i=\frac{LS}{N}$$
- **簇半径**
 $$R=\sqrt{\frac{1}{N}\sum_{i=1}^N\left(\mathbf{x}_i-\mathbf{x}_0 \right)^2}=\sqrt{\frac{N\cdot SS-LS^2}{N^2}}$$
- **簇直径**
 $$D=\sqrt{\frac{\sum_{i=1}^N\sum_{j=1}^N\left(\mathbf{x}_i-\mathbf{x}_j \right)^2}{N(N-1)}}=\sqrt{\frac{2N\cdot SS-2LS^2}{N(N-1)}}$$
{% endnote %}
其中
- R 是簇中的点到质心的平均距离
- D 是簇中两两数据点的平均距离
- R和D都反映了<u>簇内紧实度</u>

不同簇之间的距离度量通常用**曼哈顿距离**：
$$D_0=\sqrt{\frac{\sum_{i=1}^{N_1}\sum_{j=N_1+1}^{N_1+N_2}\left(\mathbf{x}_i-\mathbf{x}_j \right)^2}{N_1N_2}}=\sqrt{\frac{SS_1}{N_1}+\frac{SS_2}{N_2}-2\cdot \frac{LS_1}{N_1}\cdot\frac{LS_2}{N_2}}$$


## 聚类特征树
聚类特征树（CF-tree，Clustering Feature Tree）存储了层次聚类的簇的特征，这棵树的每一个节点是由若干个聚类特征组成的。
- 每个节点（包括叶子节点）都有若干个CF
- 内部节点的CF有指向孩子节点的指针
- 所有叶子节点用一个双向链表连接起来



聚类特征树有三个参数：
- 枝平衡因子$\beta$
- 叶平衡因子$\lambda$
- 空间阈值$\tau$

未完待续


# 参考资料
- [BIRCH算法](https://blog.csdn.net/congnaahahei/article/details/78881128)
- [刘建平Pinard-BIRCH聚类算法原理](https://www.cnblogs.com/pinard/p/6179132.html)

