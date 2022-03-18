---
title: Python | String 字符串
date: 2020-12-13 09:31:55
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

# 函数
## .center()
```python
width = 20
print('HackerRank'.center(width,'-'))
#-----HackerRank-----
```

## .endswith()
判断字符串是否以指定字符或子字符串结尾，常用于判断文件类型
```Python
str1 = 'Hello, world'
str2 = 'rld'
str1.endswith(str2)  ## str1是否以str2结尾
# True

str1.endswith('wor', 3)  ## （从左数）从索引为3的字符开始检测是否以"wor"结尾
# False

str1.endswith('wor', 3, 10)  
## （从左数）从索引为3的字符开始、到索引为10(不包括10)检测是否以"wor"结尾
## str1中逗号后含空格（算一个字符）
# True

str1.endswith(('a', 'b', 'd'))  ## 匹配元组，只要有一个满足就行
# True
```

## .islower()
字符串是否都是小写字母

## .isupper()
字符串是否都是大写字母

## .ljust()
将字符串按左对齐并填充字符直到字符串达到目标长度
```python
width = 20
print('HackerRank'.ljust(width,'-'))
#HackerRank----------
```

## .lower()
将字符转换为小写

## .rjust()
```python
width = 20
print('HackerRank'.rjust(width,'-'))
#----------HackerRank
```

## str()


## .upper()
将字符转换为大写


```python
## 将字符串s中的字母大小写互换
def swap_case(s):
    s1 = [i.upper() if i.islower() else i.lower() for i in s]
    return "".join(s1)
```
