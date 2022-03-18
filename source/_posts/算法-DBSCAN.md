---
title: Machine Learning | DBSCAN
date: 2020-05-17 23:41:52
tags: [机器学习,聚类,算法]
categories: 机器学习
mathjax: true
toc: true
hide: true
---

<center>Density-Based Spatial Clustering of Applications with Noise</center>
<!--more-->

DBSCAN
具有噪声的基于密度的聚类方法


# DBSCAN
DBSCAN是一种基于密度的空间聚类算法。该算法将具有足够密度的区域划分为簇，并在具有噪声的空间数据库中发现任意形状的簇，将簇定义为“密度相连的点的最大集合”。

> Martin Ester等人于1996年提出。
原文：[A density-based algorithm for discovering clusters in large spatial databases with noise.](https://www.researchgate.net/publication/44250717_A_Density_Based_Algorithm_for_Discovering_Density_Varied_Clusters_in_Large_Spatial_Databases)
（截至2020年01月9日，被引用次数已达16602次）

- 聚类的时候不需要事先指定簇的个数
- 最终的簇的个数不确定

## 参数

DBSCAN算法首先要确定两个参数：
1. 半径（Eps或epsilon）
 在一个点周围邻近区域的半径。若两点之间的距离小于或等于该值，则这些点被认为是相邻的。如果选择的eps值太小，则很大一部分数据不会聚集，将被视为异常点；如果选择的eps太大，则群集会被合并，会造成大多数对象处于同一群集中。因此，应该根据数据集的距离来选择eps；一般，eps的取值尽量取小。
2. MinPts
 邻近区域内至少包含点的个数。对于具有噪声的数据集，应考虑较大的MinPts；MinPts的最小值必须为3。数据集越大，对应选择的MinPts应越大。

# 参考资料
- [DBSCAN聚类算法——机器学习（理论+图解+python代码）](https://blog.csdn.net/huacha__/article/details/81094891)
- [DBSCAN方法及应用](https://www.cnblogs.com/bonelee/p/8692336.html)
- [DBSCAN密度聚类](https://mp.weixin.qq.com/s?__biz=MzU5NzkxODMxOA==&mid=2247485595&idx=1&sn=830a5f9ac8d3e9966ae36859fbcdab0b&chksm=fe4d5f9ac93ad68cae44a066ab689b9f6178618d2b2420473de316fe95c85ebb8789c5f5abb8&scene=21#wechat_redirect)