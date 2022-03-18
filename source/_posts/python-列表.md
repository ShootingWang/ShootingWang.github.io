---
title: Python | List 列表
date: 2020-12-13 09:29:00
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


统计列表中重复项出现的次数：
1. `collections.Counter()`
2. `list.count()`

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