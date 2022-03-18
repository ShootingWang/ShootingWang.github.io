---
title: Python | SciPy
date: 2020-05-29 20:36:14
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

# scipy
## stats
### mode()
求众数

```Python
## 求矩阵/数组的众数
import numpy as np

from scipy.stats import mode

a = np.array([[2, 2, 2, 1],
              [1, 2, 2, 2],
              [1, 1, 3, 3]])
print("# Print mode(a):", mode(a))
print("# Print mode(a.transpose()):", mode(a.transpose()))
print("# a的每一列中最常见的成员为：{}，分别出现了{}次。".format(mode(a)[0][0], mode(a)[1][0]))
print("# a的第一列中最常见的成员为：{}，出现了{}次。".format(mode(a)[0][0][0], mode(a)[1][0][0]))
print("# a的每一行中最常见的成员为：{}，分别出现了{}次。".format(mode(a.transpose())[0][0], mode(a.transpose())[1][0]))
print("# a中最常见的成员为：{}，出现了{}次。".format(mode(a.reshape(-1))[0][0], mode(a.reshape(-1))[1][0]))
# Print mode(a): ModeResult(mode=array([[1, 2, 2, 1]]), count=array([[2, 2, 2, 1]]))
# Print mode(a.transpose()): ModeResult(mode=array([[2, 2, 1]]), count=array([[3, 3, 2]]))
# a的每一列中最常见的成员为：[1 2 2 1]，分别出现了[2 2 2 1]次。
# a的第一列中最常见的成员为：1，出现了2次。
# a的每一行中最常见的成员为：[2 2 1]，分别出现了[3 3 2]次。
# a中最常见的成员为：2，出现了6次。
```

```Python
## 求列表的众数
from scipy import stats

lst = [2, 3, 5, 2, 3, 3, 6, 8, 6]
print(int(stats.mode(lst)[0]))
3
```

# 参考资料
- [Python中的scipy.stats.mode函数](https://blog.csdn.net/kane7csdn/article/details/84795405)