---
title: Python | NumPy
date: 2020-05-10 20:26:22
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

# NumPy

## argmax()
返回矩阵/数组的最大值的索引

```Python
import numpy as np

a = np.arange(12).reshape(3,4)  ## 3行4列
a
#array([[ 0,  1,  2,  3],
#       [ 4,  5,  6,  7],
#       [ 8,  9, 10, 11]])

np.argmax(a)  ## 返回展平的a的最大值的索引
# 11

np.argmax(a, axis=0)  ## 返回每列的最大值的索引
# array([2, 2, 2, 2], dtype=int64)

np.argmax(a, axis=1)  ## 返回每行的最大值的索引
# array([3, 3, 3], dtype=int64)
```

## argmin()
返回矩阵/数组的最小值的索引
```Python
import numpy as np

a = np.arange(12).reshape(3,4)  ## 3行4列
a
#array([[ 0,  1,  2,  3],
#       [ 4,  5,  6,  7],
#       [ 8,  9, 10, 11]])

np.argmin(a, axis=0)  ## 返回每列的最小值的索引
# array([0, 0, 0, 0], dtype=int64)

np.argmin(a, axis=1)  ## 返回每行的最小值的索引
# array([0, 0, 0], dtype=int64)

a[2, 2] = 2
a
#array([[ 0,  1,  2,  3],
#       [ 4,  5,  6,  7],
#       [ 8,  9,  2, 11]])
np.argmin(a, axis=0)  ## 多个最小值，只返回第一个最小值的索引
# array([0, 0, 0, 0], dtype=int64)
```

## mean()
求均值
```Python
import numpy as np

lst = [2,3,4,5,6]
np.mean(lst)
# 4.0
```

## median()
求中位数
```Python
import numpy as np

lst = [2,1,4,5,2,5,7,8]
np.median(lst)
# 4.5
```

## newaxis
用于扩展数组的维度
https://stackoverflow.com/questions/29241056/how-does-numpy-newaxis-work-and-when-to-use-it
中的图片简单易懂

## ptp()
最大值与最小值的差

## reshape()
改变矩阵/数组的维数

```Python
import numpy as np

a = np.arange(12).reshape(3, 4)  ## 3行4列
a
#array([[ 0,  1,  2,  3],
#       [ 4,  5,  6,  7],
#       [ 8,  9, 10, 11]])
```
### reshape(m,-1)
将数组形状变为m行（列数相应确定）；元素个数必须是m的整数倍

```Python
a = np.arange(12).reshape(1, -1)  ## 1行（不指定列数）
a
# array([[ 0,  1,  2,  3,  4,  5,  6,  7,  8,  9, 10, 11]])

a = np.arange(12).reshape(3, -1)  ## 3行
a
# array([[ 0,  1,  2,  3],
#       [ 4,  5,  6,  7],
#       [ 8,  9, 10, 11]])
```
### reshape(-1,n)
将数组形状变为n列（行数相应确定）；元素个数必须是n的整数倍
```Python
a = np.arange(12).reshape(-1, 4)  ## 4列
a
# array([[ 0,  1,  2,  3],
#       [ 4,  5,  6,  7],
#       [ 8,  9, 10, 11]])
```

## tile()
`tile(A, B)`：重复A B次

```python
import numpy as np

np.tile([1, 2],5)#在列方向上重复[1, 2]5次，默认行1次
# array([1, 2, 1, 2, 1, 2, 1, 2, 1, 2])

np.tile([3, 4],(2,1))#在列方向上重复[3, 4]1次，行2次
# array([[3, 4],
#        [3, 4]])

np.tile([1, 2], (3, 4)) # 在行方向上重复[1, 2] 3次、列4次
# array([[1, 2, 1, 2, 1, 2, 1, 2],
#        [1, 2, 1, 2, 1, 2, 1, 2],
#        [1, 2, 1, 2, 1, 2, 1, 2]])
```


```Python

```

```Python

```

```Python

```

# 参考资料
- [Numpy.argmin的使用](https://blog.csdn.net/weixin_41770169/article/details/80714461)
- [numpy中的tile函数](https://blog.csdn.net/ksearch/article/details/21388985)