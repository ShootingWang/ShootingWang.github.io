---
title: Python | statistics
date: 2020-05-30 16:14:28
tags: [Python]
categories: Python
mathjax: true
toc: true
index_img: /img/Python.png  ## 封面图片
hide: true
notshow: true
---

<center> </center>
<!--more-->

# statistics

## median()
求中位数
```python
lst = [2, 3, 2, 3, 5, 6, 8]
from statistics import median
median(lst)
# 3
```

## median_high()
当元素个数为偶数时，求中位数，返回中间两个元素的较大的那个作为中位数
```python
lst = [2, 3, 2, 3, 5, 6, 8, 10]
import statistics as s
s.median(lst)
# 4.0
s.median_high(lst)
# 5
```

## median_low()
当元素个数为偶数时，求中位数，返回中间两个元素的较小的那个作为中位数
```python
lst = [2, 3, 2, 3, 5, 6, 8, 10]
import statistics as s
s.median(lst)
# 4.0
s.median_low(lst)
# 3
```

# 参考资料
- [统计模块：Python3.7的statistics模块](https://blog.csdn.net/weixin_41084236/article/details/81482984)