---
title: Algorithm | 堆排序
date: 2020-08-13 22:08:50
tags: [算法,基础算法]
categories: [基础算法]
hide: true
math: true
mathjax: true
mermaid: true
---

<center>Heap Sort</center>
<!--more-->

# 堆
堆是具有以下性质的{% post_link 树 完全二叉树 %}：
- 每个结点的值都大于或等于其左右孩子结点的值，称为**大顶堆**
- 或，每个结点的值都小于或等于其左右孩子结点的值，称为**小顶堆**

<meta name="referrer" content="no-referrer" />
{% asset_img heap.png 大顶堆和小顶堆 %}

对堆中的结点按层进行编号，并将这种逻辑结构映射到数组中，如下：

<meta name="referrer" content="no-referrer" />
{% asset_img array.png 映射成数组 %}

该数组即是一个堆结构。
- 大顶堆：
  $$array[i] \geq array[2i+1] 且 array[i] \geq array[2i+2]$$
- 小顶堆：
  $$array[i] \leq array[2i+1] 且 array[i] \leq array[2i+2]$$

# 堆排序
- 堆排序是一种选择排序
- 不稳定排序

## 运行时间
- 最坏、最好、平均时间复杂度均为$O(n\log{n})$


# 参考资料
- [图解排序算法(三)之堆排序](https://www.cnblogs.com/chengxiao/p/6129630.html)