---
title: Algorithm | 希尔排序
date: 2020-07-20 13:35:29
tags: [算法,基础算法]
categories: [基础算法]
hide: true
math: true
mathjax: true
mermaid: true
---

<center>Shell's Sort</center>
<!--more-->

# 希尔排序
Shell's Sort是Donald Shell于1959年提出的一种排序算法。
- 希尔排序是一种**分组插入排序**
- 也称为**缩小增量排序**
- 动态演示希尔排序：[Shell Sort](https://www.cs.usfca.edu/~galles/visualization/ComparisonSort.html)

## 原理

先将整个待排序元素序列分割成若干个子序列（由相隔某个“增量”的元素组成的），分别进行直接插入排序；然后依次缩减增量再进行排序；待整个序列中的元素基本有序时，再对全体元素进行一次直接插入排序。

{% note default %}
例：对序列$\{55，26，33，80，70，90，6，30，40，20\}$进行增量为5的希尔排序。
1. 从55开始每隔5个距离取值分为1组，共分为5组，
分别为$\{55, 90\}\{26,6\}\{33,30\}\{80,40\}\{70,20\}$
2. 组内进行排序：
 $$\{55, 90\}\{6,26\}\{30,33\}\{40,80\}\{20,70\}$$
3. 然后增量取$5//2=2$，数据被分为2组
 $$\{55, 6,30,40,20\},\{90,26,33,80,70\}$$
4. 组内进行直接插入排序：
 $$\{6,20,30,40,55\},\{26,33,70,80,90\}$$
 即
 $$\{6,26,20,33,30,70,40,80,55,90\}$$
5. 再缩小增量$2//2=1$，整个数组为1组
 $$\{6,20,26,30,33,40,55,70,80,90\}$$
{% endnote %}

## 复杂度
### 时间复杂度
- 最坏的情况：$O(n^2)$
- - Hibbard优化后，最坏的实践复杂度为$O(n^{3/2})$

### 空间复杂度

## 代码实现

```python
def shell_sort(lst):
    n = len(lst)  ## 序列长度
    gap = n >> 1  ## n divided by 2**1
    while gap > 0:
        for i in range(gap, n):
            for j in range(i, 0, -gap):
                if lst[j] < lst[j-gap]:
                    lst[j], lst[j-gap] = lst[j-gap], lst[j]
                else:
                    break
        gap >>= 1  ## 增量gap减半

if __name__ == '__main__':
    lst = [10, 4, 3, 1, 6, 20, 30, 1, 40, 30, 20]
    print(lst)
    shell_sort(lst)
    print(lst)
# [10, 4, 3, 1, 6, 20, 30, 1, 40, 30, 20]
# [1, 1, 3, 4, 6, 10, 20, 20, 30, 30, 40]

```

# 参考资料
- [图解排序算法(二)之希尔排序](https://www.cnblogs.com/chengxiao/p/6104371.html)
- [用python实现希尔排序(shell_sort)](https://blog.csdn.net/u012468376/article/details/78233124)
