---
title: Linux Learning
date: 2020-08-11 01:01:31
tags: [Linux]
math: true
mathjax: true
mermaid: true
hide: true
notshow: true
---

<center>Linux小白成长之旅</center>
<!--more-->

# Linux
## 文件基本权限
Linux文件基本权限有9个，owner/User, Group, Others、三种身份对应各自read,write,execute三种权限。
- `r`：可读取；具有读取目录结构列表的权限
- `w`：可写入
- `x`：可执行；用户能否进入该目录成为工作目录；没有x则无法且换到该目录，也就无法执行该目录下的任何命令

文件权限字符：“-rwxrwxrwx”三个一组。
数字分别表示User、Group、Other的权限。
$$r:4 w:2 x:1 $$
- 若要`rwx`属性，则4+2+1=7
- 若要`rw-`属性，则4+2=6
- 若要`r-x`属性，则4+1=5

{% note default %}
文件目录data当前权限为rwx --- ---，只需要增加用户组可读权限，但不允许写操作，具体方法为：`chmod+050data`
{% endnote %}

# 常用命令

## cat
用于连接文件并打印到标准输出设备上（文本输出命令）

## chomd
改变文件权限


## echo
用于查看`export`是否成功

## env
显示所有的环境变量

## export
用于设置环境变量

## ls
列出目录内容（List Directory Contents）
- `ls -l`：以详情模式（long listing fashion）列出文件夹的内容
- `ls -a`：列出文件夹里的所有内容，包括以“.”开头的隐藏文件
- 

## touch
用于修改文件或目录的时间属性，包括存取时间和更改时间。若文件不存在，系统会建立一个新的文件



## >
- `>`为创建
- `>>`为追加

当文件不存在时，`>`和`>>`都会默认创建

{% note default %}
执行以下shell语句，可以生成/test文件的是（假定执行前没有/test文件）：
- `touch /test`
- a = \`touch / test\`
- `>/test`
{% endnote %}

## -a
表示and


# 参考资料
- [linux常用基本命令](https://blog.csdn.net/xiaoguaihai/article/details/8705992)
- [对Linux新手非常有用的20个命令](https://www.oschina.net/translate/useful-linux-commands-for-newbies)
- []()