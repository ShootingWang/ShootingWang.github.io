---
title: Machine Learning | CLARANS
date: 2020-05-14 13:43:39
tags: [机器学习,聚类,算法]
categories: 机器学习
mathjax: true
toc: true
hide: true
---


<center>A Clustering Algorithm based in Randomized Search</center>
<!--more-->

CLARANS


基于随机选择的聚类算法/随机搜索聚类算法

# CLARANS
- 一种分割聚类算法
- 在CLARA算法的基础上提出的
- 时间复杂度大约是$O(n^2)$
- 计算效率较低
- 对数据输入顺序敏感
- 只能聚类凸形或球形边界

## 原理
1. 随机选择一个点作为当前点
2. 随机检查它周围不超过MaxNeighbor个的邻接点



# 参考资料
- [聚类分析中几种算法的比较](https://zhuanlan.zhihu.com/p/22417979)
- [文本挖掘之聚类算法之CLARANS（基于随机选择的聚类算法）](https://blog.csdn.net/u011955252/article/details/50805095)