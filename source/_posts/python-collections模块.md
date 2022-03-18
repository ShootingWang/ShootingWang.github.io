---
title: Python | collections
date: 2020-05-05 13:36:41
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

# collections

## Counter()
统计列表中重复项出现的次数

```python
from collections import Counter

lst = ['a', 'a', 'v', 'c', 'b', 'a', 'b', 'c', 'b']
Counter(lst)
# Counter({'a': 3, 'v': 1, 'c': 2, 'b': 3})


lst = ['a', 'a', 'v', 'c', 'b', 'a', 'b', 'c', 'b']
for item in set(lst):
    print('the %s appears %d times' %(item, lst.count(item)))
#the c appears 2 times
#the v appears 1 times
#the a appears 3 times
#the b appears 3 times
```

## deque
**双边队列**（double-ended queue），具有队列和栈的性质，在列表（list）的基础上增加了移动、旋转和增删等

```Python
from collections import deque

d = deque()  ## 创建一个队列
print(d)
# deque([])

d1 = deque(range(10))
print(d1)
# deque([0, 1, 2, 3, 4, 5, 6, 7, 8, 9])
```

如果指定了参数`maxlen`，则会生成固定长度的队列；插入元素时，若队列已满，
- 从右侧插入，会“挤掉”最左侧的元素
- 从左侧插入，会“挤掉”最右侧的元素
```Python
d = deque(maxlen=3)
d.append(1)
d.append(2)
d.append(3)
d
# deque([1, 2, 3])
d.append(4)
d
# deque([2, 3, 4])
d.appendleft(5)
d
# deque([5, 2, 3])
```




### .append()
在队列右侧添加元素

### .appendleft()
在队列左侧添加元素
```Python

```

### .count()
统计队列中某元素的个数

```Python

```


### .extend()
在队列右边依次添加所有元素
```Python

```
### .extendleft()
在队列左侧依次添加所有元素

```Python
d = deque(['a', 'b', 'c'])

d
#  deque(['a', 'b', 'c'])

d.extendleft(['d', 'e', 'f'])

d
# deque(['f', 'e', 'd', 'a', 'b', 'c'])
```

### .pop()
从队列的右侧删除（取出）一个元素

```Python

```
### .popleft()
从队列的左侧删除（取出）一个元素
```Python

```

### .remove()
将队列中的某元素删除
```Python

```

### .reverse()
将队列倒序
```Python

```

### .rotate()
`.rotate(n)`
- $n > 0$：将队列最右边的n个数据，移到队列的左边
- $n < 0$：将队列最左边的n个数据，移到队列的右边

```Python
d = deque(range(10))
d
# deque([0, 1, 2, 3, 4, 5, 6, 7, 8, 9])

d.rotate(3)
d
#  deque([7, 8, 9, 0, 1, 2, 3, 4, 5, 6])

d = deque(range(10))
d
# deque([0, 1, 2, 3, 4, 5, 6, 7, 8, 9])

d.rotate(-3)
d
# deque([3, 4, 5, 6, 7, 8, 9, 0, 1, 2])
```




# 参考资料
- [collections.deque介绍](https://blog.csdn.net/happyrocking/article/details/80058623)
- [deque 双向队列](https://www.cnblogs.com/LouisZJ/p/8118637.html)