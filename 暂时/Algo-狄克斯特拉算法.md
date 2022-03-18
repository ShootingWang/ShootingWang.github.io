---
title: Algorithms | 狄克斯特拉算法
date: 2020-05-05 16:43:05
tags: [算法,基础算法,搜索算法]
categories: [基础算法]
hide: true
notshow: true
math: true
mathjax: true
mermaid: true
---

<center>Dijkstra's Algorithm</center>
<!--more-->

# 狄克斯特拉算法

- 加权图的最快路径查找
- 只适用于**有向无环图**（directed acyclic graph，DAG）

> 绕环的路径不可能是最短的路径
> 无向图意味着两个节点彼此指向对方，其实是环

## 加权图
- 图的每条边都有关联的数字，这些数字称为**权重**（weight）
- 带权重的图称为**加权图**（weighted graph）
- 不带权重的图称为**非加权图**（unweighted graph）
- 计算非加权图中的最短路径，可使用广度优先搜索
- 计算加权图中的最短路径，可使用狄克斯特拉算法


# 参考资料
- [图解算法](https://book.douban.com/subject/26979890/)