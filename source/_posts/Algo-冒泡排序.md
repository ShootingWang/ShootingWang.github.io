---
title: Algorithm | 冒泡排序
date: 2020-05-22 22:27:32
tags: [算法,基础算法]
categories: [基础算法]
hide: true
math: true
mathjax: true
mermaid: true
---

<center>Bubble Sort</center>
<!--more-->

# 冒泡排序
- 冒泡排序详解：
  - [Bubble Sort in Python](https://stackabuse.com/bubble-sort-in-python/)
- 动态演示冒泡排序：[Bubble Sort](https://www.cs.usfca.edu/~galles/visualization/ComparisonSort.html)

## 原理
> 以从小到大排序为例

重复地走访要排序的元素列，依次比较两个相邻的元素，如果第一个元素比第二个元素大，就交换两个元素的位置。
- 元素列$\{x_1,x_2,\cdots,x_n\}$的长度为$n$
- 第1步，遍历$i=1,2,\cdots,n-1$
  - 比较$x_i$和$x_{i+1}$，如果$x_i>x_{i+1}$，则交换$x_i$和$x_{i+1}$的位置
  - 需要比较前$n$个元素
  - 遍历后的元素列记为$\{x_1,x_2,\cdots,x_n\}$
- 第2步，遍历$i=1,2,\cdots,n-2$
  - 比较$x_i$和$x_{i+1}$，如果$x_i>x_{i+1}$，则交换$x_i$和$x_{i+1}$的位置
  - 需要比较前$n-1$个元素
  - 遍历后的元素列记为$\{x_1,x_2,\cdots,x_n\}$
- $\cdots$
- 第$k$步，遍历$i=1,2,\cdots,n-k$
  - 比较$x_i$和$x_{i+1}$，如果$x_i>x_{i+1}$，则交换$x_i$和$x_{i+1}$的位置
  - 需要比较前$n-k+1$个元素
  - 遍历后的元素列记为$\{x_1,x_2,\cdots,x_n\}$
- $\cdots$
- 第$n-1$步，比较$x_1$和$x_{2}$，如果$x_1>x_{2}$，则交换$x_1$和$x_{2}$的位置
  - 需要比较前2个元素

## 复杂度
### 时间复杂度$O(n^2)$
- **最好的情况**：元素列是正序的
  - 则只需要遍历一遍元素列即可完成排序
  - 所需的比较次数为$n-1$
  - 所需的变换元素位置测次数为0
  - 冒泡排序在最好的情况下的时间复杂度为$O(n)$
- 最坏的情况：元素列正好是完全反序的
  - 则需要进行$n-1$次遍历
  - 其中第$k$次遍历需要进行$n-k$次比较，每次比较都需要变换元素位置3次
  - 所需的比较次数为$\frac{n(n-1)}{2}=O(n^2)$
  - 所需的变换元素位置测次数为$\frac{3n(n-1)}{2}=O(n^2)$
  - 冒泡排序在最坏的情况下的时间复杂度为$O(n^2)$
- 所以，冒牌排序的平均时间复杂度为$O(n^2)$

### 空间复杂度$O(1)$

## 代码实现
Python实现冒泡排序
```python
def BubbleSort(lst):
    for i in range(len(lst)):
        for j in range(len(lst) - i -1):
            if lst[j] > lst[j+1]:
                lst[j], lst[j+1] = lst[j+1], lst[j]
```

使用该函数对列表进行升序排序：
```python
lst0 = [2, 4, 1, 18, 25, 13, 11, 3]
BubbleSort(lst0)
lst0
# [1, 2, 3, 4, 11, 13, 18, 25]
```


# 参考资料
- [三分钟彻底理解冒泡排序](https://www.cnblogs.com/bigdata-stone/p/10464243.html)