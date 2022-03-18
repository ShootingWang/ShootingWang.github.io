---
title: SQL Server
date: 2020-10-11 11:05:54
tags: [SQL]
categories: 数据库
mathjax: true
index_img: /img/SQL.jpg  ## 封面图片
---

<center>SQL Server相关知识</center>
<!--more-->

# 关键字
## INTO
`SELECT INTO FROM ...`和`INSERT INTO SELECT ... FROM ... `都是用于复制表，但二者存在区别：
- `SELECT INTO FROM`要求目标表不存在
  - 若目标表已存在，则会提示错误
  - 若目标表不存在，则语句执行成功，并在目标表中将原有的标识列自动设为标识列
- `INSERT INTO FROM`要求目标表存在

> MySQL不支持`SELECT INTO FROM`

- [相关题目](https://www.nowcoder.com/questionTerminal/5d6b5a80e83b4328938352423e7123c8)