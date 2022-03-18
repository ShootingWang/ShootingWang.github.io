---
title: Algorithm | 选择排序
date: 2020-05-05 08:00:00
tags: [算法,基础算法]
categories: [基础算法]
hide: true
math: true
mathjax: true
mermaid: true
---

<center>Selection Sort</center>
<!--more-->

# 选择排序
将长度为$n$的列表按从大到小（从小到大）的顺序排列
- 遍历该列表，找出最大的那个元素，添加到另一个新列表中

对于下面这个列表，想按照“play times”按从大到小排序：

|music|play times|
|--:|:-:|
|Ref:rain|240|
|Silly|130|
|remember|163|
|Prayer X |287|
|Angels|132|
|Let it Out|211|
|My Sweetest One|126|

遍历列表，找出播放次数最大的歌曲——“Prayer X”（287次），添加到新列表中。运行时间为$O(n)$（最坏的情况下，需要n个元素都找遍了才找到）。

|music|play times|
|--:|:-:|
|Prayer X|287|
|||
|||
|||
|||
|||
|||

再次遍历列表，找到播放次数第二多的音乐。运行时间为$O(n)$。

|music|play times|
|--:|:-:|
|Prayer X|287|
|Ref:rain|240|
|||
|||
|||
|||
|||

重复进行下去，直到将$n$个元素都添加到新列表。
每次运行时间为$O(n)$，共需要进行$n$次，所以选择排序的运行时间为$O(n^2)$。

> 实际上，第一次需要检查的元素有$n$个，第二次要检查的元素个数有$n-1$个，，随后要检查的元素个数依次为$n-2,\cdots,2,1$，平均每次检查的元素个数为$\frac{1}{2}n$，因此运行时间为$O(\frac{1}{2}n^2)$，即$O(n^2)$

## 稳定性
- 用数组实现的选择排序是不稳定的
- 用链表实现的选择排序是稳定的

通常使用数组实现选择排序，认为选择排序是不稳定的

## Python实现

使用Python实现选择排序：
```Python
def find_smallest(arr):
    """
    找到数组arr中最小元素的索引
    """
    smallest = arr[0] ## 用于存储最小值
    smallest_index = 0  ## 用于存储最小值的索引
    for i in range(1, len(arr)):
        if arr[i] < smallest:
            smallest = arr[i]
            smallest_index = i
    return smallest_index
    
def selection_sort(arr):
    """
    对数组arr进行选择排序
    从小到大
    """
    new_arr = []
    for i in range(len(arr)):
        smallest = find_smallest(arr)
        new_arr.append(arr.pop(smallest))
    return new_arr

print(selection_sort([3, 5, 1, 19, 15, 2]))
## [1, 2, 3, 5, 15, 19]
```


# 参考资料
- [图解算法](https://book.douban.com/subject/26979890/)