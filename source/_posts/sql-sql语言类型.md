---
title: SQL | SQL语言类型
date: 2020-06-02 17:02:14
tags: [SQL,数据库]
categories: 数据库
mathjax: true
toc: true
#index_img: /img/Python.png  ## 封面图片
hide: true
notshow: true
mermaid: true
---

<center></center>
<!--more-->


# SQL语言

SQL有四种语言（也有的说法将TCL考虑为第五种）：
- DDL  数据定义语言
- DQL  数据查询语言
- DML  数据库操纵语言
- DCL  数据库控制语言
- TCL  事务控制语言

## DDL
DDL（Data Definition Language），数据定义语言，用于定义数据库的结构（structure）或架构（schema）。用于定义数据库的三级结构，包括外模式、概念模式、内模式及其相互之间的映像；用于定理数据的完整性、安全控制等约束。
- 执行后会自动提交，不需要commit
- 相关命令有：CREATE，ALTER，DROP，TRUNCATE，COMMENT，RENAME

{% note default %}
- CREATE：在数据库内创建对象
- ALTER：更改数据库对象
- DROP：删除数据库里的对象
- TRUNCATE：删除数据表中的所有记录并还原该表至初始设置
- COMMENT：注释
- RENAME：重命名表名或列名
{% endnote %}

## DQL
DQL（Data Query Language），数据查询语言
- 相关命令有：SELECT

## DML
DML（Data Manipulation Language），数据操作语言，实现对数据库中数据的操作。
- 根据语言的级别，DML可分为过程性DML和非过程性DML。
- 执行之后需要commit（所有DML都是显式提交的）
- 相关命令有：INSERT，UPDATE，DELETE，MERGE，CALL，EXPLAIN PLAN，LOCK TABLE

## DCL
DCL（Data Control Language），数据控制语言；负责数据库的权限管理、角色控制等。
- 相关命令有：GRANT，REVOKE

{% note default %}
- GRANT：授权
- REVOKE：取消授权
{% endnote %}

## TCL
TCL（Transaction Control Language），事务控制语言
- 相关命令有：SAVEPOINT，ROLLBACK，SET TRANSACTION，COMMIT

{% note default %}
- SAVEPOINT：设置保存点
- ROLLBACK：回滚
- SET TRANSACTION：
- COMMIT：提交事务
{% endnote %}

<meta name="referrer" content="no-referrer" />
{% asset_img Types-of-SQL-Commands.jpg SQL语言 %}

{% note default %}
哪个不是DDL(数据库定义语言)语句？
A. ALTER
B. CREATE
C. RENAME
D. GRANT

答案：D

[<font face="宋体" color="grey">来自：[网易2018校招数据分析师笔试卷](https://www.nowcoder.com/questionTerminal/29cb393ca36146e78230f07f096247e5)</font>]
{% endnote %}

{% note default %}
**DDL**和**DML**完成对数据库数据的建表与更新。
{% endnote %}

# 参考资料
- [SQL | DDL, DQL, DML, DCL and TCL Commands](https://www.geeksforgeeks.org/sql-ddl-dql-dml-dcl-tcl-commands/)
- [SQL四种语言：DDL,DML,DCL,TCL](https://www.cnblogs.com/henryhappier/archive/2010/07/05/1771295.html)
- [SQL SERVER – Example of DDL, DML, DCL and TCL Commands](https://blog.sqlauthority.com/2009/05/02/sql-server-example-of-ddl-dml-dcl-and-tcl-commands/)
- [MySql是否需要commit详解](https://www.jb51.net/article/160797.htm)