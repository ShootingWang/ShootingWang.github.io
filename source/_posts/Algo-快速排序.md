---
title: Algorithm | 快速排序
date: 2020-05-05 10:08:01
tags: [算法,基础算法]
categories: [基础算法]
hide: true
math: true
mathjax: true
mermaid: true
---

<center>Divide and Conquer</center>
<!--more-->

# D&C
Divide and Conquer，分而治之
1. 找出基线条件
  基线条件尽可能简单
2. 不断将问题分解，直到符合基线条件


# 快速排序
- 一种著名的<u>递归式</u>问题解决方法
- 分而治之（D&C, divide and conquer）

> C语言标准库中的函数qsort实现的是快速排序

1. 基线条件：数组为空或只包含一个元素

## 原理
1. 从数组中选择一个元素——**基准值**（pivot）
2. 找出比基准值小的元素以及比基准值大的元素——**分区**（partitioning）
     - 一个由小于基准值的数字组成的子数组
     - 基准值
     - 一个由所有大于基准值的数字组成的子数组
3. 对左右两个子数组进行快速排序

## 运行时间
- 最糟的情况下：$O(n^2)$
- 平均情况下：$O(n\log{n})$
 > 最佳的情况也是平均情况，只要每次都随机地选择一个数组元素作为基准值

算法的运行时间通常有个**常量**$c$，表示算法所需的固定时间
> 如：简单查找的运行时间为10ms$\*n$，二分查找的运行时间为1s$\*\log{n}$。其中，10ms和1s都是常量

- 通常情况下，常量几乎没什么影响
- 但有时候，常量的影响可能很大
> 如：运行时间都为$O(n\log{n})$的快速排序和合并排序，快速排序将会快很多




## Python实现
```Python
def quick_sort(arr):
    """
    使用快速排序将数组从小到大排序
    """
    if len(arr) < 2:
        return arr  ## 若数组arr为空或只包含一个元素，直接返回
    else:
        pivot = arr[0]
        less = [i for i in arr[1:] if i <= pivot]
        greater = [j for j in arr[1:] if j > pivot]
        return quick_sort(less) + [pivot] + quick_sort(greater)
    
print(quick_sort([3, 5, 1, 19, 15, 2]))
## [1, 2, 3, 5, 15, 19]
```



# 参考资料
[1] [图解算法](https://book.douban.com/subject/26979890/)