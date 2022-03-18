---
title: Python | re模块
date: 2020-05-03 13:59:22
tags: [Python]
categories: Python
mathjax: true
toc: true
index_img: /img/Python.png  ## 封面图片
hide: true
notshow: true
---

<center></center>
<!--more-->

# re

## 正则表达式
### ()
包住的数据为要提取的数据，通常与`.group()`函数连用
### .
匹配单个任意字符
### *
匹配前一个字符出现0次或无限次

### ?
匹配前一个字符出现0次或1次

## .group()
- `.group(0)`：输出的是匹配正则表达式整体结果
- `.group(1)`：列出第一个括号匹配部分
- `.group(2)`：列出第二个括号匹配部分

## re.I
使匹配对大小写不敏感

## re.M
多行匹配

## 

## re.match()