---
title: JAVA
date: 2020-05-08 11:15:00
tags: [JAVA,编程]
categories: JAVA
toc: true
math: true
mathjax: true
mermaid: true
index_img: /img/java.jpg
---

<center>JAVA小白的学习之路</center>
<!--more-->



# 环境配置
- [Java 开发环境配置](https://www.runoob.com/java/java-environment-setup.html)
- [JAVA JDK 安装配置](https://mp.weixin.qq.com/s?__biz=MzU5NzkxODMxOA==&mid=2247484931&idx=1&sn=711689c6734cf40856639baae8fb375c&chksm=fe4d5102c93ad8146c09e3a9d001db254899668f77b0f4cedd8537758d4089b4aab0e9e1a24d&token=1150466167&lang=zh_CN#rd)


## 安装IDE
- [IntelliJ IDEA下载](https://www.jetbrains.com/idea/download/download-thanks.html?platform=windows&code=IIC)

# 基础语法
## 一些概念
- **对象**：是类的一个实例，有状态和行为。
- **类**：类是一个模板，描述一类对象的行为和状态。
- **方法**：就是行为，一个类可以有很多方法。
 - 逻辑运算
 - 数据修改
 - ……
- **实例变量**：每个对象都有独特的实例变量，对象的状态由这些实例变量的值决定。

## 基本语法
- 大小写敏感
- 类名
 - 类名的首字母应该大写
 - 若类名由多个单词组成，则每个单词的首字母应该大写
- 方法名
 - 所有方法名都应该以小写字母开头
 - 若方法名由多个单词组成，则后面的每个单词首字母大写
- 源文件名
 - 源文件名必须与类名相同
 - 后缀为`.java`
- 主方法入口
 - 所有的JAVA程序由 public static void main(String []args) 方法开始执行

## 标识符
- Java的所有组成部分都需要名字
- 类名、变量名及方法名都被称为**标识符**

注意事项：
- 标识符应以字母、美元符或下划线开始
- 首字符后可以是字母、美元符、下划线或数字的任何字符组合
- 关键字不能用作标识符
- 标识符是大小写敏感的

## 修饰符
用于修饰类中方法和属性。

主要有两类：
1. 访问控制修饰符
 - `default`
 - `public`
 - `protected`
 - `private`
2. 非访问控制修饰符
 - `final`
 - `abstract`
 - `static`
 - `synchronized`


## 变量
主要有三类：
1. 局部变量
2. 类变量/静态变量
3. 成员变量/非静态变量

## 关键字
### 访问控制
|关键字|说明|
|:---|:----|
|`private`|私有的|
|`protected`|受保护的|
|`public`|公共的|
|`default`|默认|


### 类、方法和变量修饰符
|关键字|说明|
|:---|:----|
|`abstract`|声明抽象|
|`class`|类|
|`extends`|扩充，继承|
|`final`|最终值，不可改变的|
|`implements`|实现（接口）|
|||

未完待续


### 程序控制句
|关键字|说明|
|:---|:----|
|||
|||

### 错误处理
|关键字|说明|
|:---|:----|
|||
|||

### 包相关
|关键字|说明|
|:---|:----|
|||
|||

### 基本类型
|关键字|说明|
|:---|:----|
|||
|||


# 参考资料
- [首页缩略图](https://cn.bing.com/images/search?view=detailV2&ccid=zThJuq%2bC&id=474DFD30EC584147A5C8FC0F10952D615CF2795A&thid=OIP.zThJuq-CIVq3uElNx7mDVQHaEK&mediaurl=https%3a%2f%2fwww.vizteams.com%2fwp-content%2fuploads%2f2015%2f07%2fjava-logo.png&exph=576&expw=1024&q=java&simid=608020279042638133&selectedIndex=2)
- [Java 基础语法](https://www.runoob.com/java/java-basic-syntax.html)