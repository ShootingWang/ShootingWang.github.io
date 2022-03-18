---
title: Python | 循环
date: 2020-05-03 09:58:15
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

# for循环
- 在`if...elif...else`的多个语句块中只会执行一个语句块
- `for`和`while`都可以有`else`语句

## for...else
- `for`循环正常执行完的情况下，执行`else`输出
- 当`for`循环中执行了跳出循环的语句（如`break`），将不执行`else`代码块的内容


```Python
for condition:
    statement1
else:
    statement2
```

# while循环

## while...else
- `while`循环正常执行完的情况下，执行`else`输出
- 当`while`循环中执行了跳出循环的语句（如`break`），将不执行`else`代码块的内容


```Python
while condition:
    statement1
else:
    statement2
```

```Python

```

# break语句
- `break`语句用于终止当前循环
- 通常与`if`、`if...else`和`if...elif...else`语句一起使用

# continue语句
- `continue`语句用于跳过当前剩余要执行的代码，执行下一次循环
- 通常与`if`、`if...else`和`if...elif...else`语句一起使用

# pass语句
- `pass`不做任何事情，一般用作<u>占位</u>语句

# 参考资料
- [牛客Python测试题](https://www.nowcoder.com/test/question/done?tid=33151324&qid=618873#summary)
- [Python while...else... 和 for...else...](https://blog.csdn.net/weixin_42595012/article/details/91569770)