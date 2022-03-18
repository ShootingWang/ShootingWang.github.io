---
title: Machine Learning | AdaBoost
date: 2020-05-27 14:30:00
tags: [算法,机器学习,集成学习,分类,回归,Boosting]
categories: 机器学习
hide: true
notshow: true
mermaid: true
mathjax: true
math: true
---

<center>Adaptive Boosting</center>
<!--more-->

自适应增强

**自适应**在于：前一个分类器分错的样本会被用来训练下一个分类器——AdaBoost能够适应弱分类器各自的训练误差率

# AdaBoost
由Yoav Freund和Robert Schapire提出

{% note default %}
Freund, Yoav , and R. E. Schapire . "[A Decision-Theoretic Generalization of On-Line Learning and an Application to Boosting](https://www.cis.upenn.edu/~mkearns/teaching/COLT/adaboost.pdf)." *Proceedings of the Second European Conference on Computational Learning Theory*, 1995, pp. 23-37.
{% endnote %}

- 模型：加法模型
- AdaBoost是损失函数为**指数损失**（exponential error loss）的Boosting算法
- 学习算法：前向分步算法的二分类学习算法
- 可用于分类，也可用于回归

## 基学习器
在分类模型中称为**基分类器**（classifier）

可以是
- 决策树
 - AdaBoost分类用CART分类树
 - AdaBoost回归用CART回归树
- 神经网络（neural networks）
- 线性判别（linear discriminants）
- ……

其中使用最广泛的是决策树和神经网络。

## 原理
- 训练数据集特征$x_i\ (x_i\in \mathbb{R}^p;\ i=1,\cdots,N)$
- 训练数据集标签$y_i=\{-1,+1\}$

在第$t$次迭代时，前面已经训练得到$t-1$个弱分类器$h_j(x)\quad(j=1,\cdots,t-1)$，且对应的重要性为$\alpha_j$。

此时已得到$t-1$个弱分类器的组合分类器
$$H_{t-1}(x_i)=\alpha_1 h_1(x_i)+\alpha_2 h_2(x_i)+\cdots+\alpha_{t-1} h_{t-1}(x_i)$$
想要将该组合分类器继续扩展成
$$H_{t}(x_i)=H_{t-1}(x_i)+\alpha_th_t(x_i)$$

AdaBoost的损失函数是指数损失：
- 若分类器$h(\cdot)$将第$i$个样本正确分类（即$h(x_i)=y_i$），则损失为
 $$e^{-\alpha}$$
- 若分类器$h(\cdot)$将第$i$个样本错误分类（即$h(x_i)\neq y_i$），则损失为
 $$e^{\alpha}$$

即
$$e^{-\alpha y_ih(x_i)}$$
其中$\alpha>0$保证“错误分类”的样本受到的“惩罚”更重。

则$H_{t}(x_i)$的损失函数为
\begin{aligned}
\mathrm{Loss}&=\sum_{i=1}^Ne^{-y_iH_t(x_i)}\\
&= \sum_{i=1}^N\exp\left\{-y_i\left(H_{t-1}(x_i)+\alpha_th_t(x_i) \right) \right\}\\
&= \sum_{i=1}^N\exp\left\{-y_iH_{t-1}(x_i)\right\}\exp\left\{-y_i\alpha_th_t(x_i)\right\}\\
&=\sum_{i=1}^N w_i^{(t)}\exp\left\{-y_i\alpha_th_t(x_i)\right\}\\
&= \sum_{y_i=h_t(x_i)}w_i^{(t)}e^{-\alpha_t}+\sum_{y_i\neq h_t(x_i)}w_i^{(t)}e^{\alpha_t}\\
&= \sum_{i=1}^Nw_i^{(t)}\left(\frac{\sum_{y_i=h_t(x_i)}w_i^{(t)}}{\sum_{i=1}^Nw_i^{(t)}}e^{-\alpha_t}+\frac{\sum_{y_i\neq h_t(x_i)}w_i^{(t)}}{\sum_{i=1}^Nw_i^{(t)}}e^{\alpha_t} \right)\\
&= \sum_{i=1}^Nw_i^{(t)}\left((1-e_t)e^{-\alpha_t}+e_te^{\alpha_t}\right)
\end{aligned}
其中
- $w_i^{(t)}=e^{-y_iH_{t-1}(x_i)}$为第$t$次迭代中样本的权重，依赖于前一轮的迭代重分配
- $\frac{\sum_{y_i\neq h_t(x_i)}w_i^{(t)}}{\sum_{i=1}^Nw_i^{(t)}}$为分类误差率$e_t$

损失函数Loss关于$\alpha_t$求偏导并令导数为零，即$\frac{\partial{\mathrm{Loss}}}{\partial{\alpha_t}}=0$，得
$$\alpha_t=\frac{1}{2}\ln\frac{1-e_t}{e_t}$$

{% note default %}
\begin{aligned}
\frac{\partial{\mathrm{Loss}}}{\partial{\alpha_t}}&=0\\
\sum_{i=1}^Nw_i^{(t)}\left((e_t-1)e^{-\alpha_t}+e_te^{\alpha_t} \right)&=0\\
(e_t-1)e^{-\alpha_t}+e_te^{\alpha_t}&=0\\
e_t-1+e_te^{2\alpha_t}&=0\\
e^{2\alpha_t}&=\frac{1-e_t}{e_t}\\
\alpha_t&=\frac{1}{2}\ln\frac{1-e_t}{e_t}
\end{aligned}
{% endnote %}

{% note danger %}
## AdaBoost算法
**输入**：
 - 训练数据集$D=\{(x_1,y_1),\cdots,(x_N,y_N)\}$
  
  其中$x_i\in X\subseteq \mathbb{R}^p,\quad y_i\in Y=\{-1,+1\}$
 - 迭代次数$T$
 - 初始化训练样本的权值分布
 $$D_1=(w_1^{(1)},w_2^{(1)},\cdots, w_N^{(1)})$$
 其中$w_i^{(1)}=\frac{1}{N},\quad i=1,2,\cdots,N$。

**输出**：最终（强）分类器
 $$H(x)=\mathrm{sign}\left(\sum_{t=1}^T\alpha_t h_t(x)\right)$$

对于$t=1,\cdots,T$:
1. 使用具有权值分布$D_t$的训练数据集进行学习，得到弱分类器$h_t(x)$
2. 计算$h_t(x)$在训练集上的分类误差率
 $$e_t=\sum_{i=1}^Nw_i^{(t)}\cdot \mathbf{I}\left(h_t(x_i)\neq y_i \right)=\sum_{h_t(x_i)\neq y_i}w_i^{(t)}$$
3. 计算$h_t(x)$在强分类器中所占的比重
 $$\alpha_t=\frac{1}{2}\ln{\frac{1-e_t}{e_t}}$$
4. 更新训练数据集的权值分布$w_i^{(t+1)}$
 \begin{aligned}
 w_i^{(t+1)}&=\frac{w_i^{(t)}}{Z_t}e^{-\alpha_t y_i h_t(x_i)}\\
 &=\left\{\begin{array}{ll}
 \frac{w_i^{(t)}}{Z_t}e^{-\alpha_t} & \mathrm{if} \quad h_t(x_i)=y_i\\
 \frac{w_i^{(t)}}{Z_t}e^{\alpha_t} & \mathrm{if} \quad h_t(x_i)\neq y_i
 \end{array}\right.\\
 &=\left\{\begin{array}{ll}
 \frac{w_i^{(t)}}{Z_t}\sqrt{\frac{e_t}{1-e_t}} & \mathrm{if} \quad h_t(x_i)=y_i\\
 \frac{w_i^{(t)}}{Z_t}\sqrt{\frac{1-e_t}{e_t}} & \mathrm{if} \quad h_t(x_i)\neq y_i
 \end{array}\right.
 \end{aligned}
 其中$Z_t$是归一化因子（为了使样本的概率分布和为1）
 $$Z_t=\sum_{i=1}^Nw_i^{(t)}e^{-\alpha_t y_i h_t(x_i)}$$
 $$\sum_{i=1}^Nw_i^{(t+1)}=1$$
{% endnote %}

- $e_t=0$：完美分类器，分类器的权重为正无穷大
- $e_t=\frac{1}{2}$：比随机猜的分类效果还差的分类器，分类器的权重为0
- $e_t=1$：完美骗子（perfect liar），分类器的权重为负无穷大


## 正则化
为了防止AdaBoost过拟合，通常会在迭代过程中加入正则化项，称之为步长（learning rate）。

在前面的第$t$次迭代中，得到的组合学习器为
$$H_t(x)=H_{t-1}(x)+\alpha_t h_t(x)$$
加入正则化项，为
$$H_t(x)=H_{t-1}(x)+\nu \alpha_t h_t(x)$$
其中$0< \nu \leq 1$为步长。

对于同一训练集，较小的$\nu$意味着需要更多的弱学习器的迭代次数（$T$需要更大）。

通常结合**步长**和**迭代最大次数**一起决定算法的拟合效果。


# 训练误差分析

{% note success %}
AdaBoost算法最终分类器的训练误差界为
$$\frac{1}{N}\sum_{i=1}^N\mathbf{I}\left(H(x_i)\neq y_i \right)\leq \frac{1}{N}\sum_{i=1}^N \exp\left\{-y_i\sum_{t=1}^T\alpha_th_t(x_i) \right\}=\prod_{t=1}^TZ_t$$
{% endnote %}
{% note default %}
**证明**：
\begin{aligned}
\frac{1}{N}\sum_{i=1}^N \exp\left\{-y_i\sum_{t=1}^T\alpha_th_t(x_i) \right\} &= \frac{1}{N}\sum_{i=1}^N\prod_{t=1}^T\exp\left\{-\alpha_ty_ih_t(x_i)\right\}\\
&= \sum_{i=1}^N w_i^{(1)}\prod_{t=1}^T\exp\left\{-\alpha_ty_ih_t(x_i)\right\} \quad (w_1^{(1)}=\frac{1}{N})\\
&= Z_1 \sum_{i=1}^N w_i^{(2)}\prod_{t=2}^T\exp\left\{-\alpha_ty_ih_t(x_i)\right\}\quad (Z_tw_i^{(t+1)}=w_i^{(t)}e^{-\alpha_ty_ih_t(x_i)})\\
&= Z_1Z_2 \sum_{i=1}^N w_i^{(3)}\prod_{t=3}^T\exp\left\{-\alpha_ty_ih_t(x_i)\right\}\\
&=\cdots\\
&= Z_1Z_2\cdots Z_{T-1}\sum_{i=1}^N w_i^{(T)}e^{-\alpha_Ty_ih_T(x_i)}\\
&=\prod_{t=1}^T Z_t
\end{aligned}
{% endnote %}

{% note success %}
二分类问题AdaBoost的训练误差界为：
$$\prod_{t=1}^T Z_t=\prod_{t=1}^T \left[2\sqrt{e_t(1-e_t)} \right]=\prod_{t=1}^T \sqrt{1-4\gamma_t^2}\leq \exp\left(-2\sum_{t=1}^T \gamma_t^2 \right)$$
其中$\gamma_t=\frac{1}{2}-e_t$
{% endnote %}
{% note default %}
**证明**：
因为$\alpha_t=\frac{1}{2}\ln{\frac{1-e_t}{e_t}}$，所以
$$e^{\alpha_t}=\sqrt{\frac{1-e_t}{e_t}}$$
$$e^{-\alpha_t}=\sqrt{\frac{e_t}{1-e_t}}$$

\begin{aligned}
Z_t &= \sum_{i=1}^N w_i^{(t)}e^{-\alpha_ty_ih_t(x_i)}\\
&= \sum_{y_i=h_t(x_i)}w_i^{(t)}e^{-\alpha_t}+\sum_{y_i\neq h_t(x_i)}w_i^{(t)}e^{\alpha_t}\\
&= (1-e_t)e^{-\alpha_t}+e_te^{\alpha_t}\\
&= 2\sqrt{e_t(1-e_t)}\\
&= \sqrt{1-4\gamma_t^2}
\end{aligned}

$\sqrt{1-2x}$在$x=0$的泰勒展开为
$$\sqrt{1-2x}=1-x-x^2+o(x^3)$$

$e^{-x}$在$x=0$的泰勒展开为
$$e^{-x}=1-x+\frac{1}{2}x^2+o(x^3)$$
所以有$\sqrt{1-2x}\leq e^{-x}$。令这里$x=2\gamma_t^2$，则有
$$\sqrt{1-4\gamma_t^2}\leq \exp(-2\gamma_t^2)$$
{% endnote %}

{% note success %}
**训练误差指数下降**

如果存在$\gamma>0$，对所有的$t$有$\gamma_t\geq \gamma$，则
$$\frac{1}{N}\sum_{i=1}^N\mathbf{I}\left(H(x_i)\neq y_i \right)\leq \exp\left(-2T\gamma^2 \right)$$
AdaBoost算法不需要知道下界$\gamma$。
{% endnote %}

# Python实现
```python
from sklearn.ensemble import AdaBoostClassifier

clf = AdaBoostClassifier()
## n_estimators = 50 (默认)
## base_estimator = DecisionTreeClassifier  (默认)
clf.fit(X_train, y_train)  ## 训练
clf.predict(X_test)  ## 预测
```


# Problems

{% note primary %}
## 过拟合？
AdaBoost算法会尝试尽量拟合好每一点，这样不会导致过拟合吗？
The algorithm tries to fit every point, doesn't it overfit?
{% endnote %}
{% note default %}
No, it does not. The answer has been found out through experimental results, there has been speculations but no concrete reasoning available.

From: *[Boosting Algorithms: AdaBoost, Gradient Boosting and XGBoost](https://hackernoon.com/boosting-algorithms-adaboost-gradient-boosting-and-xgboost-f74991cad38c)*
{% endnote %}

{% note primary %}
## 抗噪能力？
AdaBoost等几种基本机器学习算法哪个抗噪能力最强，哪个对重采样不敏感？
{% endnote %}
{% note default %}
1. 抗噪能力强——对异常点不敏感
2. 对重采样不敏感——对数据不均衡不敏感

- 聚类算法一般抗噪能力强；集成算法对重采样不敏感
- AdaBoost对异常点敏感
- K-Means对异常点敏感
{% endnote %}

# 总结
## 优点
- AdaBoost作为分类器时，分类精度很高
- 在AdaBoost的框架下，可以使用各种回归分类模型来构建弱学习器，非常灵活
- 作为简单的二分类器时，构造简单，结果可理解
- 不容易发生过拟合

## 缺点
- 对异常样本敏感
 - 异常样本在迭代中可能会获得较高的权重，影响最终的强学习器的预测准确性

# AdaBoost vs GBDT
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
      <th></th>
      <th>AdaBoost</th>
      <th>GBDT</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>共同点</th>
      <td colspan="2">目标都是优化偏差（Bias）</td>
    </tr>
    <tr>
      <th>每轮学习新的学习器</th>
      <td>通过改变样本的权值<br>关注上轮分类错误的样本的权值</td>
      <td>通过改变输出值<br>每轮拟合的值为真实值与已有的加法模型的差值（即残差）</td>
    </tr>
    <tr>
      <th>异常点</th>
      <td>敏感</td>
      <td>一定程度上优化了AdaBoost异常点敏感的问题</td>
    </tr>
    <tr>
      <th>树</th>
      <td></td>
      <td>CART树</td>
    </tr>
    <tr>
  </tbody>
</table>
</div>


# 参考资料
- [维基百科-AdaBoost](https://zh.wikipedia.org/wiki/AdaBoost)
- [AdaBoost入门详解](https://louisscorpio.github.io/2017/11/28/AdaBoost%E5%85%A5%E9%97%A8%E8%AF%A6%E8%A7%A3/)
- [AdaBoost and the Super Bowl of Classifiers A Tutorial Introduction to Adaptive Boosting](http://www.inf.fu-berlin.de/inst/ag-ki/adaboost4.pdf)
- [Boosting Algorithms: AdaBoost, Gradient Boosting and XGBoost](https://hackernoon.com/boosting-algorithms-adaboost-gradient-boosting-and-xgboost-f74991cad38c)
- [A Short Introduction to Boosting](https://cseweb.ucsd.edu/~yfreund/papers/IntroToBoosting.pdf)
- [李航-统计学习方法](https://book.douban.com/subject/10590856/)
- [机器学习树模型对比总结](https://blog.csdn.net/qq_37792144/article/details/89374530)
- [牛客交流贴-在你所知的机器学习算法中，哪个抗噪能力最强？哪个对采样不敏感？](https://pre.nowcoder.com/discuss/100334?type=1)
- [刘建平Pinard-集成学习之Adaboost算法原理小结](https://www.cnblogs.com/pinard/p/6133937.html)