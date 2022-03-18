---
title: Python | TextWrap
date: 2020-11-18 00:27:24
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

# TextWrap
文本自动换行与填充
```python
import textwrap as tw 
```

## dedent()
移除 text 中每一行的任何相同前缀空白符。

## fill()

## indent()

## wrap()
`textwrap.wrap(text, width=70, **kwargs)`：对 text (字符串) 中的单独段落自动换行以使每行长度最多为 width 个字符。 返回由输出行组成的列表，行尾不带换行符。

# 参考资料
- [textwrap --- 文本自动换行与填充](https://docs.python.org/zh-cn/3.7/library/textwrap.html#textwrap.wrap)