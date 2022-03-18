---
title: Python | 模块
date: 2020-05-03 11:57:08
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

# 模块

## \_\_name\_\_
一个模块中有`__name__`
> - 直接运行，`__name__`为`__main__`
- 调用该模块，`__name__`为被调用模块的“模块名”(文件名)

## \_\_new\_\_
- 是<u>静态方法</u>
- 返回一个创建的实例
- 只有在`__new__`返回一个cls的实例时，后面的`__init__`才能被调用
- 当创建一个新实例时，调用`__new__`


## \_\_init\_\_
- 是<u>实例方法</u>
- 什么都不返回
- 当初始化一个实例时，用`__init__`
- 为类的实例提供一些属性或完成一些动作




# 参考资料
- [牛客网试题](https://www.nowcoder.com/test/question/done?tid=33152813&qid=370530#summary)