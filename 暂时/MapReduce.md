---
title: MapReduce分布式算法
date: 2020-05-06 00:25:09
tags: [算法]
#index_img: /img/random_forest.jpg
categories: 算法
mathjax: true
toc: true
hide: true
notshow: true
---

<center></center>
<!--more-->

# MapReduce
MapReduce术语来自两个基本的数据转换操作：
**Map**：映射；将集合中的元素从一种形式转换成另一种形式。
**Reduce**：归并

- MapReduce是一种**计算模型**，可将大型数据处理任务分解成很多单个的、可以在服务器集群中**并行执行**的任务
- 用于在短时间内完成海量工作
- MapReduce编程模型是由Google开发的

- MapReduce任务（job）的启动过程需要消耗较长的时间
- 如果用户需要对大规模数据使用OLTP（联机事务处理，Online Transaction Processing）功能，则应该选择使用一个NoSQL数据库，如
 - 和Hadoop结合使用的HBase和Cassandra
 - 如果用户使用的是Amazon弹性MapReduce计算系统（EMR）或弹性计算云服务（EC2），则也可使用DynamoDB



## 工具
- 开源工具：Apache Hadoop

## 映射函数map
接受一个数组，并对其中的每个元素执行同样的处理

```Python
arr1 = [1, 2, 3, 4, 5]
arr2 = map(lambda x: x**2, arr1)
list(arr2)
# [1, 4, 9, 16, 25]
```

如果有多台计算机，map就能将任务（如这里的数组arr1）分配给不同的计算机完成，加快处理的速度

- map操作会将集合中的元素从一种形式转换成另一种形式
 - 输入的键-值对会被转换成零到多个键-值对输出
 - 输入和输出的键<u>必须完全不同</u>
 - 输入和输出的值<u>可能完全不同</u>

## 归并函数Reduce
Reduce过程的**目的**是：将值的集合<u>转换成一个值</u>或<u>转换成另一个集合</u>


- 在MapReduce计算框架中，某个键的所有键-值对都会被分发到同一个Reduce操作中
- 如果job不需要reduce过程，也是可以无reduce过程的


```Python

```

# Word Count算法
- 相当于MapReduce计算框架中的`Hello World`程序
- Word Count会返回在语料库（单个或多个文件）中出现的所有单词以及单词出现的次数
 - 输出内容会显示每个单词和它的频数，每行显示一条
 - 通常，单词（输出的键）和频数（输出的值）使用**制表符**`\t`进行分割

# 参考资料
- [图解算法](https://book.douban.com/subject/26979890/)
- [HIVE编程指南]()