---
title: Machine Learning | Logistic Regression
date: 2020-05-15 17:24:12
tags: [算法,机器学习,回归,判别式模型]
categories: 机器学习
math: true
mathjax: true
hide: true
notshow: true
---

<center>逻辑回归</center>
<!--more-->

# Logistic Regression
逻辑回归（**LR**，Logistic Regression），是一种<u>对数几率模型</u>（Logit Model），常用于二分类问题（binary classification problems）
- 逻辑回归和线性回归（Linear Regression）都是一种广义线性模型（GLM，Generalized Linear Model）
- 属于**有监督学习**（Supervised Learning）
- 是一种分类器（Classifier）
- 因变量是分类变量（Qualitative response）

> 其他分类器有：
- 判别分析（Discriminant Analysis）
- 决策树（Decision Tree）
- 随机森林（Random Forest）
- 支持向量机（Support Vector Machine）
- 神经网络（Neural Network）
- ……

## 逻辑分布
Logistic分布是一种连续型分布，其分布函数（Cumulative distribution function）为
$$F(x)=P(X\leq x)=\frac{1}{1+e^{-(x-\mu)/s}}=\frac{1}{2}+\frac{1}{2}\tanh\left(\frac{x-\mu}{2s}\right)$$


<meta name="referrer" content="no-referrer" />
{% asset_img LR-CDF.png 不同取值参数对应的CDF %}


概率密度函数为
$$
f(x)=F^\prime (x)=\frac{e^{-(x-\mu)/s}}{s \left( 1+e^{-(x-\mu)/s} \right)^2}=\frac{1}{4s} \mathrm{sech}^2 \left( \frac{x-2\mu}{2s} \right),\quad -\infty< x< +\infty
$$
其中
- $\mu$为位置参数（location parameter）
- $s$为尺度参数（scale parameter）
- 期望
 $$\mathrm{E(X)}=\mu$$
- 中位数
 $$\mathrm{Median(X)}=\mu$$
- 方差
 $$\mathrm{Var}(X)=\frac{s^2 \pi^2}{3}$$

<meta name="referrer" content="no-referrer" />
{% asset_img LR-pdf.png 不同取值参数对应的pdf %}

## 逻辑函数
一个简单的Logistic函数：
$$P(t)=\frac{1}{1+e^{-t}}$$

<meta name="referrer" content="no-referrer" />
{% asset_img LR-slog.png 标准Logistic函数 %}

广义Logistic曲线可以模拟一些人口增长的S形曲线：初始阶段大致是指数增长；然后开始变得饱和，增加变慢；最后，达到成熟时增加停止。

## 逻辑回归
逻辑回归通常用于二分类问题（因变量是分类变量），用来表示某件事发生的可能性。

Andrew Ng的CS229中的例子：根据肿瘤的大小（Tumor Size）$X$来判断肿瘤是否是恶性的（Malignant）$Y$。

考虑构建线性回归模型$h_\theta(x)$，$h_\theta(x)\geq 0.5$ 为恶性肿瘤，$h_\theta(x)<0.5$为良性肿瘤。

\begin{equation}
Y= \left\{
\begin{array}{ll}
1, & \mbox{如果} h_\theta(x) \geq 0.5\\
0, & \mbox{如果} h_\theta(x) <0.5\\
\end{array} \right.
\end{equation}

Logistic函数**Sigmoid函数**可以将任意实数值映射到(0,1)区间（不包括0和1，但可以无限接近）。因此，使用Sigmoid函数来将线性回归的结果按照一定的阈值分为2类（二分类问题）或更多类（多分类问题）。

<meta name="referrer" content="no-referrer" />
{% asset_img LR-heart.png 来自：机器之心-从原理到应用-简述Logistic回归算法 %}

令
$$h_\theta(x)=g(\theta^T x)=\frac{1}{1+e^{-\theta^T x}}=\frac{e^{\theta^T x}}{1+e^{\theta^T x}}$$
其中$z=\theta^Tx$（这里$x$包含截距项），Sigmoid函数为
$$g(z)=\frac{1}{1+e^{-z}}=\frac{e^z}{1+e^z}$$
Sigmoid保证$h_\theta(x)$取值在0和1之间（概率取值范围为$(0,1)$）。

**Log-odds**，也称**Logit**（Logit transformation / Link function）表示为
$$\log\frac{p}{1-p}=\log\frac{h_\theta(x)}{1-h_\theta(x)}=\theta^Tx$$
其中$p=h_\theta(x)$表示概率。

$h_\theta(x)$表示输入$x$后分类结果为1的概率
$$P(Y=1|x;\theta)=h_\theta(x)$$
$$P(Y=0|x;\theta)=1-h_\theta(x)$$
因此，因变量$Y$服从Bernoulli分布
$$Y|X=x_i\sim \mathrm{Bernoulli}\left(h_\theta(x_i)\right)$$

概率函数为
$$P(y|x;\theta)=\left[h_\theta(x) \right]^y\left[1-h_\theta(x) \right]^{1-y}$$
使用训练数据集$x=\{x_1,x_2,\cdots,x_n\}$（特征数据）和$y=\{y_1,y_2\cdots,y_n\}$（分类数据）构建回归模型。

### 损失函数
对于单个样本，其损失函数可以表示为

\begin{equation}
\mathrm{Cost}(h_\theta(X),y)=\left\{
  \begin{array}{ll}
  -\log{h_\theta(X)},& \mathrm{if}\quad y=1\\
  -\log{\left(1-h_\theta(X)\right)}, & \mathrm{if }\quad y=0\end{array}
  \right.
\end{equation}

即
$$\mathrm{Cost}(h_\theta(X),y)=-\left[y\log{h_\theta(X)}+(1-y)\log{\left(1-h_\theta(X)\right)} \right]$$
该式被称为**交叉熵代价函数**（cross entropy cost function）。

对于训练集的所有样本(样本量为$n$)来说，共同造成的损失函数的均值为

$$
\begin{aligned}
  H_{\theta(X)} &=\frac{1}{n} \sum_{i=1}^n \mathrm{Cost} \left( h_{\theta}(X^{(i)}),y^{(i)} \right)\\
  &= -\frac{1}{n} \sum_{i=1}^n \left[ y^{(i)} \log{h_\theta(X^{(i)})}+(1-y^{(i)}) \log{ \left(1-h_\theta(X^{(i)})\right) } \right]
\end{aligned}
$$


如何求解逻辑回归方程中的参数？通常使用极大似然法。

### 极大似然法
$n$个独立样本的似然函数为
$$L(\theta)=\Pi_{i=1}^nP(y_i|x_i;\theta)=\Pi_{i=1}^nh_\theta(x_i)^{y_i}\left[1-h_\theta(x_i) \right]^{1-y_i}$$
对数似然函数为
$$
\begin{aligned}
  l(\theta)=\log{L(\theta)}&=\sum_{i=1}^n\left\{y_i\log{h_\theta(x_i)}+(1-y_i)\log{\left[1-h_\theta(x_i) \right]}\right\}\\
  &= \sum_{i=1}^n\left\{y_i\theta^Tx_i-\log{\left[1+e^{\theta^Tx_i} \right]}\right\}
\end{aligned}
$$


其中
$$\log{h_\theta(x_i)}=\log{\frac{e^{\theta^Tx_i}}{1+e^{\theta^Tx_i}}}=\theta^Tx_i-\log{\left[1+e^{\theta^Tx_i}\right]}$$
$$\log{\left[1-h_\theta(x_i) \right]}=\log{\frac{1}{1+e^{\theta^Tx_i}}}=-\log{\left[1+e^{\theta^Tx_i} \right]}$$

$$\max{L(\theta)} \Leftrightarrow \max{l(\theta)} \Leftrightarrow \min \{ -l(\theta)\} $$
其中$-l(\theta)=-\log{L(\theta)}$称为**交叉熵误差函数**（cross-entropy error function）/交叉熵代价函数。

考虑$p+1$维的参数$\theta=(\theta_0,\theta_1,\cdots,\theta_p)^T$，其中$\theta_0$ 为线性回归方程$z=\theta^Tx$的截距。
$$
\theta=\left(
\begin{array}{c}
  \theta_0\\
  \theta_1\\
  \theta_2\\
  \vdots\\
  \theta_p
\end{array}
\right),
\quad
x_i=\left(
\begin{array}{c}
  1\\
  x_{i1}\\
  x_{i2}\\
  \vdots\\
  x_{ip}
\end{array}
\right)
$$

对数似然函数$l(\theta)$分别对$\theta_j\ (j=0,1,\cdots,p)$求偏导，并令导数为零
$$\frac{\partial{l(\theta)}}{\partial{\theta_j}}=\sum_{i=1}^n\left\{y_ix_{ij}-\frac{e^{\theta^Tx_i}}{1+e^{\theta^Tx_i}}x_{ij}\right\}=\sum_{i=1}^n\left\{x_{ij}\left[y_i-h_\theta(x_i) \right]\right\}=0$$
写成向量形式
$$\frac{\partial{l(\theta)}}{\partial{\theta}}=\sum_{i=1}^n\left\{x_i\left[y_i-h_\theta(x_i) \right]\right\}=0$$
该式也被称为**score equation**。

这里$x_i$的第一个元素为1，因此由上式可得
$$\sum_{i=1}^ny_i=\sum_{i=1}^nh_\theta(x_i)$$
表示$Y$取值为1的样本量（$n$个样本中得恶性肿瘤的人数）等于“事件”（如肿瘤为恶性肿瘤）发生的概率和。

当$p$较小时，score equation可得到解析解（精确解）；但当$p$较大时，score equation 往往得不到解析解。这时，可以考虑使用Newton-Raphson算法求得数值解（numerical solution）。

### Newton-Raphson算法
首先计算二阶偏导（Hessian Matrix）：
$$
\begin{aligned}
\frac{\partial^2 l(\theta)}{\partial{\theta_k} \partial{\theta_j}} &= \frac{\partial}{\partial{\theta_k}}\sum_{i=1}^n\left\{x_{ij}\left[ y_i-h_\theta(x_i) \right] \right\}\\
&=\frac{\partial}{\partial{\theta_k}} \sum_{i=1}^n \left\{x_{ij} \left[y_i-\frac{e^{\theta^Tx_i}}{1+e^{\theta^Tx_i}} \right]\right\}\\
&=\sum_{i=1}^nx_{ij} \left\{-\frac{e^{\theta^Tx_i}x_{ik}(1+e^{\theta^Tx_i})-e^{\theta^Tx_i}e^{\theta^Tx_i}x_{ik}}{ \left(1+e^{\theta^Tx_i} \right)^2} \right\}\\
&=-\sum_{i=1}^nx_{ij}x_{ik}\frac{e^{\theta^Tx_i}}{1+e^{\theta^Tx_i}} \left(1-\frac{e^{\theta^Tx_i}}{1+e^{\theta^Tx_i}} \right)\\
&=-\sum_{i=1}^nx_{ij}x_{ij}h_\theta(x_i) \left[1-h_\theta(x_i) \right] \\
\end{aligned}
$$

其中$j,k=0,1,\cdots,p$。

**Hessian Matrix**为
$$\frac{\partial^2l(\theta)}{\partial\theta\partial\theta^T}=-\sum_{i=1}^nx_ix_i^Th_\theta(x_i)[1- h_\theta(x_i)]$$

由$\theta^{old}$开始，Newton-Raphson的更新步骤（updating step）为
$$\theta^{new}=\theta^{old}-\left[\frac{\partial^2l(\theta)}{\partial\theta\partial\theta^T}\right]^{-1}\frac{\partial{l(\theta)}}{\partial\theta}$$
这里考虑步长为1的pure Newton's Method。即
$$\theta^{new}=\theta^{old}-\alpha\left[\frac{\partial^2l(\theta)}{\partial\theta\partial\theta^T}\right]^{-1}\frac{\partial{l(\theta)}}{\partial\theta}\quad \mbox{with}\ \alpha=1$$
用矩阵形式表示
$$
\mathbf{y}=\left(
\begin{array}{c}
  y_1\\
  y_2\\
  \vdots\\
  y_n
\end{array}
\right),\quad
\mathbf{X}=\left(
\begin{array}{c}
  x_1^T\\
  x_2^T\\
  \vdots\\
  x_n^T
\end{array}
\right)=
\left(
\begin{array}{ccccc}
  1 & x_{11} & x_{12} & \cdots & x_{1p}\\
  1 & x_{21} & x_{22} & \cdots & x_{2p}\\
  \vdots & \vdots & \vdots &\ddots & \vdots \\
  1 & x_{n1} & x_{n2} & \cdots & x_{np}
\end{array}
\right)
$$

\begin{equation}
\mathbf{P}=\left(
\begin{array}{c}
  h_{\theta^{old}}(x_1)\\
  h_{\theta^{old}}(x_2)\\
  \vdots\\
  h_{\theta^{old}}(x_n)
\end{array}
\right),\quad
\mathbf{W}=\mathrm{diag}\left\{
  h_{\theta^{old}}(x_i)(1-h_{\theta^{old}}(x_i))
\right\}
\end{equation}

则有
$$\frac{\partial{l(\theta)}}{\partial\theta}=\mathbf{X}^T(\mathbf{y}-\mathbf{P})$$
$$\frac{\partial^2l(\theta)}{\partial\theta\partial\theta^T}=-\mathbf{X}^T\mathbf{W}\mathbf{X}$$

因此
\begin{equation}
\begin{aligned}
\theta^{new}&=\theta^{old}-\left[\frac{\partial^2l(\theta)}{\partial\theta\partial\theta^T} \right]^{-1}\frac{\partial{l(\theta)}}{\partial\theta}\\
&= \theta^{old}+\left(\mathbf{X}^T\mathbf{W}\mathbf{X}\right)^{-1}\mathbf{X}^T(\bf{y}-\bf{P})\\
&= \left(\mathbf{X}^T\mathbf{W}\mathbf{X}\right)^{-1}\mathbf{X}^T\mathbf{W}\left[\mathbf{X}\theta^{old}+\mathbf{W}^{-1}(\mathbf{y}-\mathbf{P}) \right]\\
&= \left(\mathbf{X}^T\mathbf{W}\mathbf{X}\right)^{-1}\mathbf{X}^T\mathbf{W}\mathbf{z}
\end{aligned}
\end{equation}

其中$\mathbf{z}=\mathbf{X}\theta^{old}+\mathbf{W}^{-1}(\mathbf{y}-\mathbf{P})$。

每次迭代中，$\bf{P}$更新，$\bf{W}$和$\bf{z}$也随之更新。

$\theta^{new}$可以看作是$\bf{z}$关于$\bf{X}$回归的加权最小二乘（weighted least square）。

式
$$\theta^{new}=\left(\mathbf{X}^T\mathbf{W}\mathbf{X}\right)^{-1}\mathbf{X}^T\mathbf{W}\mathbf{z}$$
这种迭代法也称为<u>迭代重加权最小二乘</u>（iteratively reweighted least squares，IRLS）。

# 比较
## 优点
- 训练速度快
- 模型具有较好的解释性


## 缺点
- 不能用逻辑回归解决非线性问题
- 对多重共线性数据较为敏感
- 在不平衡的数据中表现较差
- 不能筛选特征（解释变量），当特征较多时，模型的解释性较差

## 逻辑回归 vs 线性回归

||**逻辑回归**|**线性回归**|
|:---:|:------:|:---------:|
||分类|回归|
|**变量分布**|无要求|要求服从正态分布|
|**因变量**|离散的变量|连续的变量|
|**自变量与因变量**|可以不符合线性关系|符合线性关系|
||无法直观表达变量关系|直观表达变量关系|
||分析因变量取某个值的概率与自变量的关系|直接分析因变量与自变量的关系|
||$\theta^TX=0$为决策边界|$\theta^TX$为预测值的拟合函数|
|**损失函数**|交叉熵损失函数|残差平方和|



# 参考资料
- [维基百科-逻辑回归](https://zh.wikipedia.org/wiki/%E9%82%8F%E8%BC%AF%E8%BF%B4%E6%AD%B8)
- [逻辑回归（Logistic Regression）（一）](https://zhuanlan.zhihu.com/p/28408516)
- [维基百科-逻辑分布](https://en.wikipedia.org/wiki/Logistic_distribution)
- [维基百科-逻辑函数](https://zh.wikipedia.org/wiki/%E9%82%8F%E8%BC%AF%E5%87%BD%E6%95%B8)
- [从原理到应用-简述Logistic回归算法](https://www.jiqizhixin.com/articles/2018-05-13-3)
- [逻辑回归模型基础](https://www.cnblogs.com/sparkwen/p/3441197.html)
- [逻辑回归](https://easyai.tech/ai-definition/logistic-regression/)
- [Logistic回归，梯度下降法，牛顿法，IRLS算法](https://zhuanlan.zhihu.com/p/67842740)
- [Logistic和牛顿法](https://blog.csdn.net/u012526120/article/details/48897135)
- [如何在R中执行Logistic回归？](https://www.afenxi.com/56409.html)
- [如何在R语言中使用Logistic回归模型](https://www.cnblogs.com/nxld/p/6170690.html)
- [Python3常用的数据清洗方法](https://blog.csdn.net/zllnau66/article/details/81742798)
- [Python_Sklearn机器学习库学习笔记（三）](https://www.cnblogs.com/wuchuanying/p/6243987.html)
- [线性回归于逻辑回归的区别](https://www.cnblogs.com/wmr95/articles/9698475.html)