---
title: Python | heapq
date: 2020-08-13 21:54:14
tags: [Python]
categories: Python
mathjax: true
math: true
hide: true
notshow: true
---

<center></center>
<!--more-->

# 堆队列
堆队列，也称为优先级队列。

堆是一个二叉树，它的每个父节点的值都只会小于或大于所有孩子节点（的值），原理与{% post_link Algo-堆排序 %}极为相似。

## 特点
- 最小的元素总是在根结点。
```Python
nums = [2, 3, 5, 1, 54, 23, 132]
heap = []
for num in nums:
    heapq.heappush(heap, num)  # 加入堆


heap[0]  ## 获取最小值
# 1
```
- 堆的值也可以是元组，可以实现对带权值的元素进行排序
```python
h = []
heapq.heappush(h, (5, 'write code'))
heapq.heappush(h, (7, 'release product'))
heapq.heappush(h, (1, 'write spec'))
heapq.heappush(h, (3, 'create tests'))
h
"""
[(1, 'write spec'),
 (3, 'create tests'),
 (5, 'write code'),
 (7, 'release product')]
"""
heapq.heappop(h)
# (1, 'write spec')
```

# 函数

## heapify()
将列表转换为堆结构
```python
nums = [2, 3, 5, 1, 54, 23, 132]
heapq.heapify(nums)
nums
# [1, 2, 5, 3, 54, 23, 132]
```

## heapmerge()
合并多个排序后的序列成为一个排序后的序列，返回排序后的值的迭代器。

```python
num1 = [32, 3, 5, 34, 54, 23, 132]
num2 = [23, 2, 12, 656, 324, 23, 54]
num1 = sorted(num1)
num2 = sorted(num2)

res = heapq.merge(num1, num2)
print(list(res))
# [2, 3, 5, 12, 23, 23, 23, 32, 34, 54, 54, 132, 324, 656]
```

## heappop()
将最小值弹出

```Python
print([heapq.heappop(nums) for _ in range(len(nums))])  # 堆排序结果
# [1, 2, 3, 5, 23, 54, 132]

nums = [2, 3, 5, 1, 54, 23, 132]
heapq.heapify(nums)  ## 将列表转换为堆结构
heapq.heappop(nums)  ## 将堆结构的最小值弹出
# 1
```

```Python
## 实现堆排序
def heapsort(iterable):
    h = []
    for value in iterable:
        heappush(h, value)
    return [heappop(h) for i in range(len(h))]

heapsort([1, 3, 5, 7, 9, 2, 4, 6, 8, 0])
# [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
```

## heappush()
将值加入堆中
```Python
nums = [2, 3, 5, 1, 54, 23, 132]
heap = []
for num in nums:
    heapq.heappush(heap, num)  # 加入堆

heap
# [1, 2, 5, 3, 54, 23, 132]
```

## heapreplace()
删除堆中的最小元素并加入一个元素

```Python
nums = [2, 3, 5, 1, 54, 23, 132]
heapq.heapify(nums)  ## 将列表转换为堆结构
heapq.heapreplace(nums, 55)  ## 删除最小元素，并加入55
# 1

nums
# [2, 3, 5, 55, 54, 23, 132]
```

## nlargest()
`nlargest(n, lst)`：返回列表lst中前n个最大元素

```python
lst = [3, 42, 34, 12, 2, 1, 5]

import heapq

heapq.nlargest(3, lst)
# [42, 34, 12]

import heapq
from pprint import pprint
portfolio = [
    {'name': 'IBM', 'shares': 100, 'price': 91.1},
    {'name': 'AAPL', 'shares': 50, 'price': 543.22},
    {'name': 'FB', 'shares': 200, 'price': 21.09},
    {'name': 'HPQ', 'shares': 35, 'price': 31.75},
    {'name': 'YHOO', 'shares': 45, 'price': 16.35},
    {'name': 'ACME', 'shares': 75, 'price': 115.65}
]
cheap = heapq.nsmallest(3, portfolio, key=lambda s: s['price'])
expensive = heapq.nlargest(3, portfolio, key=lambda s: s['price'])
pprint(cheap)
pprint(expensive)

"""
输出：
[{'name': 'YHOO', 'price': 16.35, 'shares': 45},
 {'name': 'FB', 'price': 21.09, 'shares': 200},
 {'name': 'HPQ', 'price': 31.75, 'shares': 35}]
[{'name': 'AAPL', 'price': 543.22, 'shares': 50},
 {'name': 'ACME', 'price': 115.65, 'shares': 75},
 {'name': 'IBM', 'price': 91.1, 'shares': 100}]
"""
```

## nsmallest()
`heapq.nsmallest(n, lst)`：返回列表lst中前n个最小元素

```Python
lst = [3, 42, 34, 12, 2, 1, 5]

import heapq

heapq.nsmallest(4, lst)
# [1, 2, 3, 5]
```



# 参考资料
- [Python 优先级队列](https://mp.weixin.qq.com/s/tMok3YAKvMWnI3eW46OYlg)
- [Python标准库模块之heapq](https://www.jianshu.com/p/801318c77ab5)