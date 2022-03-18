---
title: R语言编程
date: 2020-05-11 11:26:55
tags: [R]
categories: R语言
mathjax: true
toc: true
top: true
index_img: /img/rstudio.jpg  ## 封面图片
---


<center>R语言</center>

<!--more-->

# base

## class
变量的类别


## merge
合并两个数据框（DataFrame）
```R
merge(x, y, by = intersect(names(x), names(y)),
      by.x = by, by.y = by, all = FALSE, all.x = all, all.y = all,
      sort = TRUE, suffixes = c(".x",".y"), no.dups = TRUE,
      incomparables = NULL, ...)
```
- `all=FALSE`：相当于`natural join`，仅返回匹配的行
- `all=TRUE`：相当于`full outer join`
- `all.x=TRUE`：相当于`left outer join`
- `all.y=TRUE`：相当于`right outer join`

```R
x <- data.frame(k1 = c(NA,NA,3,4,5), k2 = c(1,NA,NA,4,5), data = 1:5)
x
#  k1 k2 data
#1 NA  1    1
#2 NA NA    2
#3  3 NA    3
#4  4  4    4
#5  5  5    5
y <- data.frame(k1 = c(NA,2,NA,4,5), k2 = c(NA,NA,3,4,5), data = 1:5)
y
#  k1 k2 data
#1 NA NA    1
#2  2 NA    2
#3 NA  3    3
#4  4  4    4
#5  5  5    5

merge(x, y, by = c("k1","k2")) # NA's match
#  k1 k2 data.x data.y
#1  4  4      4      4
#2  5  5      5      5
#3 NA NA      2      1

merge(x, y, by = "k1") # NA's match, so 6 rows
#  k1 k2.x data.x k2.y data.y
#1  4    4      4    4      4
#2  5    5      5    5      5
#3 NA    1      1   NA      1
#4 NA    1      1    3      3
#5 NA   NA      2   NA      1
#6 NA   NA      2    3      3

merge(x, y, by = "k2", incomparables = NA) # 2 rows
#  k2 k1.x data.x k1.y data.y
#1  4    4      4    4      4
#2  5    5      5    5      5
```

## mode
返回对象在内存中的存储类型
- `numeric`：integer、double
- 

```r
mode(NA)
# [1] "logical"
```


## paste
组合字符串
- 可以将任意数量的参数组合在一起
```R
paste(..., sep = " ", collapse = NULL)
```
- `...`：要组合的任何数量的参数
- `sep`：表示参数之间的分隔符
- `collapse`：用于消除两个字符串之间的空间；但不是在一个字符串的两个词的空间

```R
(nth <- paste0(1:12, c("st", "nd", "rd", rep("th", 9))))
# [1] "1st"  "2nd"  "3rd"  "4th"  "5th"  "6th"  "7th"  "8th"  "9th"  "10th" "11th" "12th"

month.abb
# [1] "Jan" "Feb" "Mar" "Apr" "May" "Jun" "Jul" "Aug" "Sep" "Oct" "Nov" "Dec"
paste(month.abb, "is the", nth, "month of the year.")
# [1] "Jan is the 1st month of the year."  "Feb is the 2nd month of the year." 
# [3] "Mar is the 3rd month of the year."  "Apr is the 4th month of the year." 
# [5] "May is the 5th month of the year."  "Jun is the 6th month of the year." 
# [7] "Jul is the 7th month of the year."  "Aug is the 8th month of the year." 
# [9] "Sep is the 9th month of the year."  "Oct is the 10th month of the year."
#[11] "Nov is the 11th month of the year." "Dec is the 12th month of the year."

letters
# [1] "a" "b" "c" "d" "e" "f" "g" "h" "i" "j" "k" "l" "m" "n" "o" "p" "q" "r" "s" "t" "u" "v"
#[23] "w" "x" "y" "z"
paste(month.abb, letters)
# [1] "Jan a" "Feb b" "Mar c" "Apr d" "May e" "Jun f" "Jul g" "Aug h" "Sep i" "Oct j" "Nov k"
#[12] "Dec l" "Jan m" "Feb n" "Mar o" "Apr p" "May q" "Jun r" "Jul s" "Aug t" "Sep u" "Oct v"
#[23] "Nov w" "Dec x" "Jan y" "Feb z"
```

## typeof
对变量类型的细分

# stats
## r+分布
产生特定分布的随机数（random）

|||


# Packages

## Amelia 
{% post_link R语言-Amelia Amelia包 %}
- 可视化缺失值


## ggplot2
{% post_link R语言-ggplot2 ggplot2包 %}
- 绘图

## grpreg
{% post_link R语言-grpreg grpreg包 %}
- 组变量选择

## installr
更新RGui

```R
## 在RGui中输入下列命令
install.packages('installr')
library(installr)
updateR()
```

## MICE
{% post_link R语言-MICE MICE包 %}（Multivariate Imputation by Chained Equations）

- 填补缺失值
- 可视化缺失值



# 其他
## 快捷键

||Windows|MacOS|
|:--|:----|:----|
|注释|`Ctrl` + `Shift` + `C` | `Cmd` + `Shift` + `C`|
|运行|`Ctrl` + `Enter` |`Shift` + `Enter`|



```R

```


```R

```

```R

```


# 参考资料
- [首页缩略图](https://cn.bing.com/images/search?view=detailV2&ccid=y7PQyqXC&id=617BD652FC2012861BB5E27035D4C092C9467682&thid=OIP.y7PQyqXC-XCIFzc0BC5JawHaHa&mediaurl=https%3a%2f%2fwww.rstudio.com%2fwp-content%2fuploads%2f2019%2f02%2frstudio-og.png&exph=1200&expw=1200&q=Rstudio&simid=607993942360785821&selectedIndex=18)
- [使用R中merge()函数合并数据](https://blog.csdn.net/neweastsun/article/details/79435271)
- [R语言paste函数](https://www.cnblogs.com/csguo/p/7294057.html)
- [R语言学习笔记：mode，class，typeof的区别](https://www.cnblogs.com/xihehe/p/7306449.html)
- [Windows下更新R版本及Rstudio](https://blog.csdn.net/weixin_41859179/article/details/97570369)
- []()