---
title: Python | Set 集合
date: 2020-12-13 09:34:45
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

# 集合

```python
set(("Rank"))
#{'R', 'a', 'k', 'n'}
set("Rank")
#{'R', 'a', 'k', 'n'}
set(["Rank"])
#{'Rank'}
```

# 函数
## .add()
往集合中添加元素
```python
s = set(['a', 'b', 1, 2])
s
#{1, 2, 'a', 'b'}

s.add(('c', 'a'))
s
#{('c', 'a'), 1, 2, 'a', 'b'}
```

## .difference()
求差集
```python
a = {1, 2, 3, 4}
b = {4, 5, 6, 7, 8}
a.difference(b)
#{1, 2, 3}

a = {1, 2, 3, 4}
b = {4, 5, 6, 7, 8}
b.difference(a)
#{5, 6, 7, 8}

a.difference(b) == b.difference(a)
#False

a = {1, 2, 3, 4}

b = {1, 2, 3, 4}
a.difference(b) == b.difference(a)
#True
```

## .difference_update()
取差集，并更新原集合
- 等价于`-=`

```python
s1 = set("Hello")
s2 = set("World")
s1.difference(s2)  ## 不改变s1、s2
#{'H', 'e'}
s1
#{'H', 'e', 'l', 'o'}
s1.difference_update(s2)
s1
#{'H', 'e'}

s1 = set("Hello")
s2 = set("World")
s1 -= s2
s1
#{'H', 'e'}
```

## .discard()
从集合中剔除元素
- 如果元素不存在，则无变化（不会报错）
```python
s
#{('c', 'a'), 1, 2, 3, 4, 5, 6, 7, 'a', 'b'}

s.discard(6)
s
#{('c', 'a'), 1, 2, 3, 4, 5, 7, 'a', 'b'}

s.discard(8) ## 删除不存在的元素，无事发生
# 没有报错
s
#{('c', 'a'), 1, 2, 3, 4, 5, 7, 'a', 'b'}
```

## .intersection()
求交集
- 等价于`&`
- 不改变集合本身

```python
a = {1, 2, 3, 4}
b = {4, 5, 6, 7, 8}
a.intersection(b)
#{4}

a.intersection(b) == b.intersection(a)
#True

s = set("Rank")
s
#{'R', 'a', 'k', 'n'}
s.intersection("Hacker")
#{'a', 'k'}
## 等价于
s & set("Hacker")
#{'a', 'k'}
```

## .intersection_update()
- 等价于`&=`

```python
s1 = set("Hello")
s2 = set("World")
s1.intersection_update(s2)
s1
#{'l', 'o'}

s1 = set("Hello")
s2 = set("World")
s1 &= s2
s1
#{'l', 'o'}
```

## .pop()
从集合中随意删除并返回一个元素
- 如果集合为空，则会报错

```python
s = set(['a', 1, 2, 3, 5, 'b'])
s
#{1, 2, 3, 5, 'a', 'b'}
s.pop()
#1
s
#{2, 3, 5, 'a', 'b'}

s2 = set()
s2.pop()
#KeyError: 'pop from an empty set'
```

## .remove()
从集合中剔除元素
- 如果元素不存在，则会报错
```python
s
#{('c', 'a'), 1, 2, 3, 4, 5, 7, 'a', 'b'}
s.remove('b')
s
#{('c', 'a'), 1, 2, 3, 4, 5, 7, 'a'}

s.remove(10) ## 删除不存在的元素，会报错
#KeyError: 10
```


## .symmetric_difference()
对称差
- 不改变集合本身
- 等价于`^`
- `s1.symmetric_difference(s2)`等价于`s1 ^ s2` 或 `(s1 - s2).union(s2 - s1)` 或 `(s1 - s2) | (s2 - s1)` 或 `s2.symmetric_difference(s1)`


```python
s1 = set("Hello")
s2 = set("World")
s1.symmetric_difference(s2)
#{'H', 'W', 'd', 'e', 'r'}

s2.symmetric_difference(s1)
#{'H', 'W', 'd', 'e', 'r'}

(s1 - s2).union(s2 - s1)
#{'H', 'W', 'd', 'e', 'r'}

(s1 - s2) | (s2 - s1)
#{'H', 'W', 'd', 'e', 'r'}

s1 ^ s2
#{'H', 'W', 'd', 'e', 'r'}
```

## .symmetric_difference_update()
取对称差，并更新原集合
- 等价于`^=`

```python
s1 = set("Hello")
s2 = set("World")
s1.symmetric_difference(s2)
#{'H', 'W', 'd', 'e', 'r'}
s1
#{'H', 'e', 'l', 'o'}
s1.symmetric_difference_update(s2)
s1
#{'H', 'W', 'd', 'e', 'r'}
```

## .union()
合并集合
- 等价于`|`
- 不改变集合本身


```python
a = {1, 2, 3, 4}
b = {4, 5, 6, 7, 8}
a.union(b)
#{1, 2, 3, 4, 5, 6, 7, 8}
a
#{1, 2, 3, 4}
b
#{4, 5, 6, 7, 8}

a.union(b) == b.union(a)
#True

set1 = set(['apple', 'banana', 'tomato'])
set2 = set(['banana', 'tomato', 'carrots'])
set1 | set2  ## 并集
# {'apple', 'banana', 'carrots', 'tomato'}

s = set("Rank")
s
#{'R', 'a', 'k', 'n'}
s.union("Hello")
#{'H', 'R', 'a', 'e', 'k', 'l', 'n', 'o'}

s = set("Rank")
s
#{'R', 'a', 'k', 'n'}
s.union(enumerate(['H', 'e', 'l', 'l', 'o']))
#{(0, 'H'), (1, 'e'), (2, 'l'), (3, 'l'), (4, 'o'), 'R', 'a', 'k', 'n'}

s = set("Rank")
s
#{'R', 'a', 'k', 'n'}
s.union({"Hello":1})
#{'Hello', 'R', 'a', 'k', 'n'}
s.union({"Hello":2})
#{'Hello', 'R', 'a', 'k', 'n'}

s = set("Rank")
s
#{'R', 'a', 'k', 'n'}
s | set("Hello") ##等价于 s.union("Hello")
#{'H', 'R', 'a', 'e', 'k', 'l', 'n', 'o'}
```

## .update()
批量往集合中添加元素（要求可迭代）；对集合取并集，并改变原集合
- 等价于`|=`


```python
s
#{('c', 'a'), 1, 2, 'a', 'b'}
s.update([1,2,3,4,5,6,7])
s
#{('c', 'a'), 1, 2, 3, 4, 5, 6, 7, 'a', 'b'}

s1 = set("Hello")
s2 = set("World")
s1.union(s2)
#{'H', 'W', 'd', 'e', 'l', 'o', 'r'}
s1
#{'H', 'e', 'l', 'o'}
s2
#{'W', 'd', 'l', 'o', 'r'}
s1.update(s2)
s1
#{'H', 'W', 'd', 'e', 'l', 'o', 'r'}

s1 = set("Hello")
s2 = set("World")
s1 |= s2
s1
#{'H', 'W', 'd', 'e', 'l', 'o', 'r'}
```
