---
title: SQL | 范式
date: 2020-05-03 10:18:37
tags: [SQL,数据库]
categories: 数据库
mathjax: true
toc: true
#index_img: /img/Python.png  ## 封面图片
hide: true
notshow: true
---

<center>Normal Form</center>
<!--more-->


# 范式
数据库范式：
1. 第一范式 1NF：无重复的列
2. 第二范式 2NF：消除部分函数依赖
3. 第三范式 3NF：消除传递函数依赖
4. BCNF：消除对主码自己的依赖
5. 第四范式 4NF：所有非平凡的多值依赖都是函数依赖
6. 第五范式 5NF：连接依赖均由候选码所蕴含

- 通过数据范式的引入，可以减少数据冗余（第三范式），消除数据操作异常
- 高层范式满足低层范式

1. 如果一个关系模式R的所有属性都是不可分的基本数据项，则$R\in 1NF$（R符合第一范式）
2. 若关系模式$R\in 1NF$（R符合第一范式），并且每一个非主属性都完全依赖于R的键（码），则$R\in 2NF$（R符合第二范式）
3. 若关系模式R每一个非主属性既不部分依赖于键（码）也不传递依赖于键（码），则$R\in 3NF$（R符合第三范式）


## 第一范式 1NF
字段具有原子性，不可再分。
> 无重复的列
- 所有关系型数据库系统都满足第一范式，数据库表中的字段都是单一属性的，不可再分
- 在这种范式下，一个数据列只能有一个值
- 一个表满足1NF，如果表的每一行都是唯一的并且任何行都没有包含多个值的列
- 在任何一个关系型数据库中，第一范式是对关系模式的基本要求，不满足第一范式的数据库就不是关系型数据库



# 第二范式 2NF
要求数据库表中的每个实例（instance）或行必须可以被唯一地区分。
> 非主属性<u>部分依赖</u>于主关键字
- 满足第二范式必须先满足第一范式
- <u>部分依赖</u>是指组合主码的一部分可以函数确定关系的一列
-  如果一个关系有单列的主码，那么这个关系中就不可能存在部分函数依赖
- 通常要含有一个唯一属性列，该列被称为主键（主关键字）
- 第二范式要求实体（instance）的属性完全依赖于主关键字


## 第三范式
要求一个数据库表中不包含已在其他表中已包含的非主关键字信息
> 消除冗余
- 非主关键字段不能出现<u>传递依赖</u>
- <u>传递依赖</u>是指一个关系的非码列确定另一个非码列
- 如果一个关系有传递依赖，那么它不满足3NF，需要被规范化到3NF
- 满足第四范式必然满足第三范式，满足第三范式必然满足第二范式

{% note default %}
如：存在一个部门信息表tbl_dept，其中每个部门有部门编号dept_id（主键）、部门名称dept_name、部门简介dept_desc等信息，那么在员工信息表employees中列出部门编号dept_id后就不能再将表tbl_dept中的非主关键字部门名称、部门简介等加入到表employees中。如果不存在部门信息表，则根据第三范式应该构建部门信息表，否则会有大量的信息冗余。
{% endnote %}

第三范式的**特征**：
- 每一列只有一个值
- 每一行都能被区分
- 每一个表都不包含其他表已包含的非主关键字信息



## 巴斯-科德范式 BCNF
BCNF在3NF的基础上消除对主码子集的依赖。
- BCNF是3NF的一个子集，即满足BCNF必须满足3NF
- 消除了删除异常、插入异常、更新异常
- BCNF实际是对3NF的修正，使数据库冗余度更小

## 第四范式 4NF

## 第五范式 5NF


# 参考资料
- [数据库三范式](https://mp.weixin.qq.com/s?__biz=MzU5NzkxODMxOA==&tempkey=MTA1OV84Y1hhdGQ4czVBNkVGOEhMMWQ1aXA0VW95VG14UjRKYWhpeUwzZm4yUnYyV0loMU9nekN6cmp6TTQ0LTdpeXNfempxSTlKSktSU2ZKdlBTM2w4REhBdGRQVXBXcDJzZW5XQnJrS3A4a0hpX3Q2ejRXMXV4eS1EOTFMSFIxTDhHa2s1aUp2QTdDWHgzTi1yZ21NS2toR2lLSVo4OGhrVHhObXRnOVZRfn4%3D&chksm=7e4d524e493adb5865db235ecd5b893bf9916fb11b6c00e5ee583c6c70add792cafe9c81c76f#rd)
- [第一、第二、第三范式之间的理解和比较](https://www.cnblogs.com/ktao/p/7775100.html)