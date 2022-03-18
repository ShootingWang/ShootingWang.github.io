---
title: Algorithm | 广度优先搜索
date: 2020-05-05 11:32:43
tags: [算法,基础算法,搜索算法]
categories: [基础算法]
hide: true
notshow: true
math: true
mathjax: true
mermaid: true
---

<center>Breadth-First Search</center>
<!--more-->




# BFS
- 最简便的图的搜索算法之一
- 用于解决**最短路径问题**（shortest-path problem）

> - 编写拼写检查器：计算最少编辑多少个地方就可以把所有拼错的单词改成正确的单词
- 迷宫问题：从起点最少需要走多少步可以走到出口
- 从厦大白城出发到厦门北站，找出换乘最少的公交乘车路线
- ……

## 原理
- 搜索范围从起点开始逐渐向外延伸——先检查一度关系，再检查二度关系

## 运行时间
图的顶点（vertice）数为$V$，边数为$E$，则广度优先搜索的运行时间为$O(V+E)$


# 参考资料
[1] [图解算法](https://book.douban.com/subject/26979890/)