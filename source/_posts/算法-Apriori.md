---
title: Machine Learning | Apriori算法
date: 2020-06-05 14:19:10
tags: [算法,]
categories: 机器学习
hide: true
notshow: true
mermaid: true
mathjax: true
math: true
---

<center></center>
<!--more-->

# Apriori
- 常用于挖掘**数据关联规则**
- 其他**关联规则挖掘算法**有：PrefixSpan、CBA

> 在超市购物数据集、电商购物数据集，找出了频繁出现的数据集，则可以优化超市产品的位置摆放、优化电商商品所在的仓库位置，从而节约成本，增加经济效益

{% note warning %}
**项集**（Itemset）：包含0个或多个项的集合。

如果包含$k$个项，则称为$k-$**项集**。
{% endnote %}
- **空集**是不包含任何项的项集

{% note default %}
以经典的“啤酒-尿布”案例为例，某位顾客到超市购买的物品清单
<center>{牛奶，尿布，可乐，啤酒}</center>
就是一个项集。其中{牛奶}、{尿布}等都是数据项。该项集的长度为4，称为4-项集。
{% endnote %}

## 频繁项集
常见的频繁项集(Frequent Itemset)的评估标准：
1. 支持度
2. 置信度
3. 提升度

一般情况下，要选择一个数据集合中的频繁项集，则需要自定义评估标准。如：
- 自定义的支持度
- 自定义支持度和置信度的一个组合

**强规则**：同时满足最小支持度阈值（minsup）和最小置信度阈值（minconf）的规则。

{% note warning %}
### 支持度Support
- 几个关联的数据在数据集中出现的次数占总数据集的比重
- 或，几个数据关联出现的概率
- 或，同时包含事务$A$和事务$B$的事务占所有事务的比例
 $$Support=P(A\&B)$$
{% endnote %}
- 支持度高的数据不一定构成频繁项集，但是支持度太低的数据肯定不构成频繁项集

{% note default %}
考虑多个顾客，每个顾客的购物清单是一个项集
<center>{牛奶，可乐，啤酒，纸巾}</center>
<center>{面包，牛奶，鸡蛋}</center>
<center>{啤酒，鸡蛋，可乐，尿布}</center>
<center>{牛奶，啤酒，尿布}</center>
<center>{面包，牛奶，尿布，啤酒}</center>
<center>{尿布，啤酒，可乐，纸巾}</center>
{尿布}和{啤酒}同时出现4次，一共有6个项集，则{尿布，啤酒}的支持度为
$$Support(尿布\&啤酒)=\frac{4}{6}=\frac{2}{3}$$
{% endnote %}

{% note warning %}
### 置信度Confidence
- 体现一个数据出现后，另一个数据出现的概率
 $$Confidence(X\rightarrow Y)=\frac{\{X,Y\}同时出现}{出现X}$$
- 或，数据的条件概率
- 或，同时包含$A$和$B$的事务占包含$A$事务的比例
 $$Confidence(A\rightarrow B)=\frac{P(A\&B)}{P(A)}$$
{% endnote %}

{% note default %}
$$Confidence(啤酒\rightarrow尿布)=\frac{Support(尿布\&啤酒)}{P(啤酒)}=\frac{2}{3}/\frac{5}{6}=\frac{4}{5}$$
{% endnote %}

{% note warning %}
### 提升度Lift
- 表示含有$X$的条件下，同时含有$Y$的概率，与$Y$总体发生的概率之比
 $$Lift(X\rightarrow Y)=\frac{P(Y|X)}{P(Y)}=\frac{P(XY)}{P(X)P(Y)}$$
- 或，“包含事务$A$的事务中同时包含事务$B$的比例”与“包含事务$B$的比例”的比值
 $$Lift(A\rightarrow B)=\frac{Confidence(A\rightarrow B)}{P(B)}=\frac{P(A\&B)}{P(A)P(B)}$$
{% endnote %}
{% note default %}
$$Lift(啤酒\rightarrow尿布)=\frac{Confidence(啤酒\rightarrow尿布)}{P(尿布)}=\frac{4}{5}/\frac{4}{6}=\frac{6}{5}>1$$
{% endnote %}
- 提升度反映了关联规则中$A$与$B$的**相关性**
 - 提升度$>1$且越高表明正相关性越高，则$A\rightarrow B$是有效的强关联规则
 - 提升度$<1$且越低表明负相关性越高，则$A\rightarrow B$是无效的强关联规则
 - 提升度$=1$表明没有相关性，此时
  $$P(B|A)=P(B)$$


Apriori算法的目标是<u>找到最大的$k$项频繁集</u>
- 首先找到符合支持度标准的频繁项集
- 其次从中找到最大个数的频繁项集


Apriori算法采用**迭代**的方法，先搜索出候选1-项集及对应的支持度

未完待续


# 参考资料
- [Apriori算法原理总结](https://www.cnblogs.com/pinard/p/6293298.html)
- [机器学习之关联规则（支持度和置信度、Apriori算法）](https://blog.csdn.net/Pizza_great/article/details/101224098)
- [关联规则、支持度（support）、置信度（confidence）](https://blog.csdn.net/DD18203614685/article/details/98057386)
- [数据挖掘关联分析中的支持度、置信度和提升度](https://www.jianshu.com/p/dc053deb94f2)