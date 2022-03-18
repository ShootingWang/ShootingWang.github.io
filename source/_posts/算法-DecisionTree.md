---
title: Machine Learning | 决策树
date: 2020-05-15 20:19:32
tags: [算法,机器学习,分类,回归,有监督学习]
categories: 机器学习
math: true
mathjax: true
hide: true
notshow: true
index_img: /img/DT.jpg
---

<center>Decision Tree</center>
<!--more-->

{% note default %}
基础阅读：
{% post_link Machine-Learning-熵 %}
{% endnote %}


# Decision Tree
- 是一种基本的**分类**与**回归**方法
- 决策树由节点和有向边组成
 - <u>内部节点</u>表示一个特征/属性
 - <u>叶子节点</u>表示一个分类
 - <u>有向边</u>表示一个划分规则
- 决策树从根节点到子节点的有向边代表了一条路径(一条分类规则)
 - 决策树的路径是互斥的且是完备的
   - **互斥**：每一个样本不会同时匹配上两条分类规则
   - **完备**：每一个样本都能在决策树中匹配上一条规则
- 决策树学习通常包括3个步骤：
 1. 特征选择
 2. 决策树生成
 3. 决策树剪枝
- 决策树表示给定特征条件下类的条件概率分布



决策树的组成：
- **根节点**：第一个选择点
- **非叶子节点与分支**：中间过程
- **叶子节点**：最终的决策结果

<meta name="referrer" content="no-referrer" />
{% asset_img 决策树.jpg 决策树 %}



## 原理

## 特征选择

特征选择指选择最大化所定义目标函数的特征。

为了衡量类别分布概率的倾斜程度，定义决策树节点node的**不纯度**（impurity）
- 不纯度越小，则类别的分布概率越倾斜

不纯度的三种度量：
1. 熵（Entropy）
 $$\mathrm{Entropy}(\mathrm{node})=-\sum_{k=1}^Kp_k\log{p_k}$$
2. 基尼不纯度（Gini Impurity）
 $$\mathrm{Gini}(\mathrm{node})=\sum_{k=1}^K p_k(1-p_k)$$
3. 分类误差率
 $$\mathrm{Classification\_error}(\mathrm{node})=1-\max_k\ p_k$$
 
其中$p_k$表示样本中类别$k$的占比；共有$K$个类别。

<meta name="referrer" content="no-referrer" />
{% asset_img three.png 熵、基尼不纯度、分类误差率之间的关系 %}

其中
- 为了更好地反映变量关系，曲线中的熵除以2
- 横轴是$p$
- $p$为0或1时，熵、基尼不纯度、分类误差率都为0，表示不存在不确定性
- 当$p=0.5$时，熵、基尼不纯度、分类误差率最大，表示不确定性最大


### 信息增益
{% note info %}
熵与条件熵：
{% post_link Machine-Learning-熵 %}
{% endnote %}

**信息增益**（Information Gain）：表示已知特征$X$的信息而使得类$Y$的信息的不确定性减少的程度。

{% note warning %}
**定义**：特征$A$对训练数据集$D$的信息增益$g(D,A)$，定义为数据集$D$的经验熵$H(D)$与特征$A$给定条件下$D$的经验条件熵$H(D|A)$之差。即
$$g(D,A)=H(D)-H(D|A)$$
{% endnote %}

一般地，熵$H(Y)$与条件熵$H(Y|X)$之差称为{% post_link Machine-Learning-熵 互信息（Mutual Information） %}。

- 决策树学习中的信息增益等价于训练数据集中类与特征的互信息
- 给定训练数据集$D$和特征$A$，经验熵$H(D)$表示对数据集$D$进行分类的不确定性
- $g(D,A)$表示由于特征$A$而使得对数据集$D$的分类的不确定性减少的程度
- 对于数据集$D$而言，信息增益依赖于特征
- 不同的特征往往具有不同的信息增益
- **信息增益大的特征具有更强的分类能力**

考虑训练数据集$D$，$n=|D|$为样本容量。
- 有$K$个类$C_k,\ (k=1,\cdots,K)$
- $|C_k|$为属于类$C_k$的样本个数
 $$\sum_{k=1}^K|C_k|=n$$
- 设特征$A$有$m$个不同的取值$a_1,\cdots,a_m$
- 根据特征$A$的取值将$D$划分为$m$个子集$D_1,\cdots,D_m$
- $|D_i|$为$D_i$的样本个数
 $$\sum_{i=1}^m|D_i|=|D|$$
- 记子集$D_i$中属于类$C_k$的样本的集合为$D_{ik}$
 $$D_{ik}=D_i\cap C_k$$
 $|D_{ik}|$为$D_{ik}$的样本个数

{% note danger %}
**信息增益算法**
输入：训练数据集$D$和特征$A$

输出：特征$A$对训练数据集$D$的信息增益$g(D,A)$
1. 计算训练集$D$的经验熵$H(D)$
 $$H(D)=-\sum_{k=1}^K\frac{|C_k|}{|D|}\log{\frac{|C_k|}{|D|}}$$
2. 计算特征$A$对数据集$D$的经验条件熵$H(D|A)$
 $$\begin{aligned} H(D|A)&=\sum_{i=1}^m\frac{|D_i|}{|D|}H(D_i)\\
 &=-\sum_{i=1}^m\frac{|D_i|}{|D|}\sum_{k=1}^K\frac{|D_{ik}|}{|D_i|}\log{\frac{|D_{ik}|}{|D_i|}}\\
 &= -\sum_{i=1}^m\sum_{k=1}^K\frac{|D_{ik}|}{|D|}\log{\frac{|D_{ik}|}{|D_i|}}
 \end{aligned}$$
3. 计算信息增益
 $$g(D,A)=H(D)-H(D|A)$$
{% endnote %}

- 可用信息增益来进行决策树的划分属性选择
 $$a^*=\arg\max_{a\in A} g(D,a)$$

{% note default %}
ID3决策树学习算法就是以<u>信息增益</u>为准则来选择划分属性
{% endnote %}



# 决策树算法
包括
- ID3
- C4.5
- CART

## ID3
- 以**信息增益**为准则来划分属性


{% note default %}
示例：
[数据来源](https://mp.weixin.qq.com/s?__biz=MzA4MjYwMTc5Nw==&mid=2648935296&idx=2&sn=cb77cd47a0189804ba9b16ba10415100&chksm=879419aab0e390bcf14a8eb3f56f1ce31750b5d9566263d93269d47e4674bc7600cc1d1636fb&mpshare=1&scene=24&srcid=&sharer_sharetime=1590479772805&sharer_shareid=b539221659d6ecf12200314308b58dd3&key=e224f213fcbe8475f9ef285bb7bf11ea118733d485a2e0a1bf5d71cd8212cc36e61414400a348229bd6398ffdf35e8ca97536ef45b6ba48e523107c1ff2b6bae4b84605a9a810ef0e8ccfb3381a96097&ascene=14&uin=MjAwNDUzMjgxNw%3D%3D&devicetype=Windows+10+x64&version=62090070&lang=zh_CN&exportkey=AXXj%2FLKyq7BwO492ReB6hMg%3D&pass_ticket=UotHILlosru6hQjR77K5QFOqbs0ufoaaTOgd804mZ9Zj6ebPR%2BPzibBO9ORmuQZQ)

共4个特征：
1. 天气
2. 温度
3. 湿度
4. 风

<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: center;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: center;">
      <th>特征</th>
      <th>特征的不同取值</th>
      <th>样本数</th>
      <th>`yes`样本数</th>
      <th>`no`样本数</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th rowspan="3">outlook</th>
      <td>sunny</td>
      <td>5</td>
      <td>2</td>
      <td>3</td>
    </tr>
    <tr>
      <td>overcast</td>
      <td>4</td>
      <td>4</td>
      <td>0</td>
    </tr>
    <tr>
      <td>rainy</td>
      <td>5</td>
      <td>3</td>
      <td>2</td>
    </tr>
    <tr>
      <th rowspan="3">temperature</th>
      <td>hot</td>
      <td>4</td>
      <td>2</td>
      <td>2</td>
    </tr>
    <tr>
      <td>mild</td>
      <td>6</td>
      <td>4</td>
      <td>2</td>
    </tr>
    <tr>
      <td>cool</td>
      <td>4</td>
      <td>3</td>
      <td>1</td>
    </tr>
    <tr>
      <th rowspan="2">humidity</th>
      <td>high</td>
      <td>7</td>
      <td>3</td>
      <td>4</td>
    </tr>
    <tr>
      <td>normal</td>
      <td>7</td>
      <td>6</td>
      <td>1</td>
    </tr>
    <tr>
      <th rowspan="2">windy</th>
      <td>false</td>
      <td>8</td>
      <td>6</td>
      <td>2</td>
    </tr>
    <tr>
      <td>true</td>
      <td>6</td>
      <td>3</td>
      <td>3</td>
    </tr>
    <tr>
  </tbody>
</table>
</div>


标签为“打球”`yes`或“不打球”`no`，原始数据共有14个样本，其中9个为`yes`、5个为`no`，原始数据集的熵为
$$H(D)=-\frac{9}{14}\log_2{\frac{9}{14}}-\frac{5}{14}\log_2{\frac{5}{14}}=0.940286$$

接着计算经验条件熵：
1. 天气：
 \begin{aligned}
 H(sunny)&=-\frac{2}{5}\log_2{\frac{2}{5}}-\frac{3}{5}\log_2{\frac{3}{5}}=0.970951\\
 H(overcast)&=0\\
 H(rainy)&=-\frac{3}{5}\log_2{\frac{3}{5}}-\frac{2}{5}\log_2{\frac{2}{5}}=0.970951\\
 H(D|outlook)&=\frac{5}{14}H(sunny)+\frac{4}{14}H(overcast)+\frac{5}{14}H(rainy)=0.693536
 \end{aligned}
2. 温度
 \begin{aligned}
 H(hot)&=-\frac{2}{4}\log_2{\frac{2}{4}}-\frac{2}{4}\log_2{\frac{2}{4}}=1\\
 H(mild)&=-\frac{4}{6}\log_2{\frac{4}{6}}-\frac{2}{6}\log_2{\frac{2}{6}}=0.918296\\
 H(cool)&=-\frac{3}{4}\log_2{\frac{3}{4}}-\frac{1}{4}\log_2{\frac{1}{4}}=0.811278\\
 H(D|temperture)&=\frac{4}{14}H(hot)+\frac{6}{14}H(mild)+\frac{4}{14}H(cool)=0.911063
 \end{aligned}
3. 湿度
 \begin{aligned}
 H(high)&=\cdots=0.985228\\
 H(normal)&=\cdots=0.591673\\
 H(D|humidity)&=\frac{7}{14}H(high)+\frac{7}{14}H(normal)=0.788451
 \end{aligned}
4. 风
 \begin{aligned}
 H(false)&=\cdots=0.811278\\
 H(true)&=\cdots=1\\
 H(D|windy)&=\frac{8}{14}H(false)+\frac{6}{14}H(true)=0.892159
 \end{aligned}

接着计算信息增益：
\begin{aligned}
H(D,outlook)&=0.940286-0.693536=0.24675\\
H(D,temperature)&=0.940286-0.911063=0.029223\\
H(D,humidity)&=0.940286-0.788451=0.151835\\
H(D,windy)&=0.940286-0.892159=0.048127
\end{aligned}

其中outlook的信息增益最大，所以应选择特征`outlook`为根节点。
{% endnote %}

## ID4

## C4.5
- 基于ID3
- ID3使用的是<u>信息增益</u>，而C4.5使用的是**信息增益比**


## CART
**C**lassification **a**nd **R**egression **T**ree / Regression Tree
分类和回归树

- 与C4.5不同，CART的决策树由二元逻辑问题生成，每个树节点只有两个分支



> regression tree is a function that maps the attributes to the score. —— from Tianqi Chen - Introduction to Boosted Tree


### CHAID
Chi-squared Automatic Interaction Detector

### MARS

### 条件推断树




## 过拟合

当训练误差（training error）很小，泛化误差（generalization error / test error）较大时，说明决策树模型发生了过拟合。

发生过拟合的根本原因是决策树模型过于复杂，可能的原因有：
- 训练数据集中有异常点（噪音样本点），异常点影响了模型的分类效果
- 决策树的叶节点中缺乏有分类价值的样本记录（需要进行剪枝）


如何避免决策树过拟合？
- 限制树的深度max_depth
- 剪枝prune
- 限制叶子节点数量
- 正则化项
- 增加样本数据
- bagging（subsample、subfeature、低维空间投影）
- 数据增加（加入有噪声的数据）
- early stopping


# 总结
## 优点
决策树的优点：
- 可读性强
- 分类速度快
- 推理过程完全依赖于属性变量的取值特点
- 可自动忽略对目标变量没有贡献的属性变量

## 缺点


# 笔试题目
{% note default %}
当在一个决策树中划分一个节点时，以下关于“信息增益”的论述正确的是（2、3）
1.较不纯的节点需要更多的信息来描述总体
2.信息增益可以通过熵来推导
3.信息增益偏向于选择大量值的属性

[<font face="宋体" color="grey">来自：[牛客试题广场](https://www.nowcoder.com/questionTerminal/c8af2b7b74d74c65b6e5d58b5c607dc3)</font>]
{% endnote %}
- 不纯度指的是基尼系数

{% note default %}
以下机器学习中，在数据预处理时，不需要考虑归一化处理的是：
- 树形模型

[<font face="宋体" color="grey">来自：[牛客试题广场](https://www.nowcoder.com/test/question/done?tid=36064816&qid=370007#summary)</font>]
{% endnote %}
- 不需要归一化处理的模型：决策树、随机森林
  - 因为它们不关心变量的值，而是关心变量的分布和变量之间的条件概率

# 参考资料
- [Tianqi Chen - Introduction to Boosted Trees](https://homes.cs.washington.edu/~tqchen/data/pdf/BoostedTree.pdf)
- [决策树](http://www.huaxiaozhuan.com/%E7%BB%9F%E8%AE%A1%E5%AD%A6%E4%B9%A0/chapters/4_decision_tree.html)
- [24个「数据分析师」岗位面试题和答案解析](https://mp.weixin.qq.com/s?__biz=MzUzODYwMDAzNA==&mid=2247490797&idx=2&sn=e4488e6ae078fd04992ee19c3ea43e0d&chksm=fad46be0cda3e2f606f3faffe800c819c2cacb5ded2c4bb6e1b0b061bc2c06d276bd18a4a9f4&mpshare=1&scene=24&srcid=&sharer_sharetime=1589846441823&sharer_shareid=b539221659d6ecf12200314308b58dd3&key=391433877af29848d80eb268dfdb67e70c94dbc8461e647ef4fd9cf863b97fcbf05fe3cc098469dd0eed236ecaa0ce9b4afa6518cb6c2e116156d348ec25c79484217b352893ff4b5a26e8f3fd8d7f35&ascene=14&uin=MjAwNDUzMjgxNw%3D%3D&devicetype=Windows+10+x64&version=62090070&lang=zh_CN&exportkey=AYJS0mHna0hF6l9GD1%2B%2BuCY%3D&pass_ticket=hSPy%2FUU8oxUcSxTs%2F5nB1TD0Hf4oMyJJiFtHAk4RLQ6enp8U%2FMNrC0H1aJAXQF7t)
- [决策树算法的原理（接地气版）](https://mp.weixin.qq.com/s?__biz=MzA4MjYwMTc5Nw==&mid=2648935296&idx=2&sn=cb77cd47a0189804ba9b16ba10415100&chksm=879419aab0e390bcf14a8eb3f56f1ce31750b5d9566263d93269d47e4674bc7600cc1d1636fb&mpshare=1&scene=24&srcid=&sharer_sharetime=1590479772805&sharer_shareid=b539221659d6ecf12200314308b58dd3&key=e224f213fcbe8475f9ef285bb7bf11ea118733d485a2e0a1bf5d71cd8212cc36e61414400a348229bd6398ffdf35e8ca97536ef45b6ba48e523107c1ff2b6bae4b84605a9a810ef0e8ccfb3381a96097&ascene=14&uin=MjAwNDUzMjgxNw%3D%3D&devicetype=Windows+10+x64&version=62090070&lang=zh_CN&exportkey=AXXj%2FLKyq7BwO492ReB6hMg%3D&pass_ticket=UotHILlosru6hQjR77K5QFOqbs0ufoaaTOgd804mZ9Zj6ebPR%2BPzibBO9ORmuQZQ)
- [数据挖掘|决策树](https://divinerhjf.github.io/2019/05/24/shu-ju-wa-jue-jue-ce-shu/#toc-heading-8)
- [从决策树到随机森林：树型算法的原理与实现](https://www.jiqizhixin.com/articles/2017-07-31-3)
- [【十大经典数据挖掘算法】C4.5](https://www.cnblogs.com/en-heng/p/5013995.html)
- [李航-统计学习方法](https://book.douban.com/subject/10590856/)
- [熵、基尼系数、分类误差率之间的关系-图片来源](https://zhuanlan.zhihu.com/p/76667156)