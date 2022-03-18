---
title: SQL | 模式
date: 2020-05-03 10:31:52
tags: [SQL,数据库]
categories: 数据库
mathjax: true
toc: true
#index_img: /img/Python.png  ## 封面图片
hide: true
notshow: true
---

<center>Schema</center>
<!--more-->

数据库有三级模式：
- 模式
- 外模式
- 内模式

# 模式/逻辑模式/概念模式
**模式**（Schema），也称**逻辑模式**、**概念模式**，是数据库中<u>全体数据</u>的逻辑结构和特征的描述，是所有用户的公共数据视图；用来叙述现实生活中的实体，以及它们之间的关系，从而定义记录数据项的完整性约束条件及记录之间的联系。
- 一个数据库只有一个模式
- 模式是数据库数据在逻辑级上的视图
- 数据库模式以某一种数据类型为基础

# 外模式/子模式/用户模式
**外模式**（External Schema），也称**子模式**（Subschema）、**用户模式**，是数据库用户能够看见和使用的<u>局部数据</u>的的逻辑结构和特征的描述，是数据库用户的数据视图，是与某一应用有关的数据的逻辑表示。
- 一个数据库可以有任意多个外模式
- 外模式就是<u>用户视图</u>
- 外模式是保证数据安全性的一个有力措施



# 内模式
**内模式**（Internal Schema），也称**存储模式**（Storage Schema），是数据物理结构和存储方式的描述，是数据在数据库内部的表示方式。
- 一个数据库只有一个内模式
- 一个表可能由多个文件组成
     > 如：数据文件、索引文件
- 内模式是所有模式中的最底层的表示

<meta name="referrer" content="no-referrer" />
{% asset_img schema.png 图片来自参考资料[2] %}


# 参考资料
- [数据库模式](https://blog.csdn.net/qq_37808895/article/details/94394123)
- [浅谈数据库三大模式](https://blog.csdn.net/qq_15950325/article/details/52654924)