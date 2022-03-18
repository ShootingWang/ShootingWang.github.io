---
title: Machine Learning | GBDT
date: 2020-05-28 17:00:00
tags: [算法,机器学习,集成学习,Boosting,分类,回归]
categories: 机器学习
hide: true
notshow: true
mermaid: true
mathjax: true
math: true
---

<center>Gradient Boosting Decision Tree</center>
<!--more-->

梯度提升树

不同简称：
- **GBDT** (Gradient Boosting Decision Tree)
- **GBT** (Gradient Boosting Tree)
- **GTB** (Gradient Tree Boosting)
- **GBRT** (Gradient Boosting Regression Tree)
- **MART** (Multiple Additive Regression Tree)

# GBDT


{% note default %}
Breiman, L. (June 1997). "[Arcing The Edge](https://statistics.berkeley.edu/sites/default/files/tech-reports/486.pdf)". Technical Report 486. Statistics Department, University of California, Berkeley.
{% endnote %}




<meta name="referrer" content="no-referrer" />
{% asset_img GBDT.png GBDT训练过程(图片来自:Boosting框架) %}




## 原理

GBDT进行$T$次迭代，第$t$次（$t=1,\cdots,T$）迭代产生一个弱分类器$h_t(x)$；下一次迭代时，会根据上一次迭代计算得到的残差进行学习，并生成一个弱分类器。

最终得到的强分类器为
$$H(x)=\sum_{t=1}^Th_t(x)$$

在GBDT的第$t$次迭代，假设在前一轮迭代得到的组合强学习器是$H_{t-1}(x)$，损失函数是$L(y,H_{t-1}(x))$


本轮迭代的目标是找到一个CART回归树模型的弱学习器$h_t(x)$使得本轮的损失函数最小
$$L\left(y, H_{t}(x) \right)=L\left(y, H_{t-1}(x)+h_t(x) \right)$$

- GBDT通过经验风险最小化来确定下一个弱分类器的参数
- 损失函数$L$的选择有多种
 - 平方损失函数
 - 0-1损失函数
 - 对数损失函数
 - ……
 
 通常使用平方损失函数（即残差）

到目前为止，上述方法描述的是{% post_link 算法-BoostingTree 提升树(Boosting Tree) %}。

当损失函数为平方损失函数时，每一步优化的实现较为简单，当损失函数为一般损失函数时，每一步优化的实现较为困难。

针对该问题，Freidman提出了梯度提升（Gradient Boosting）算法——用损失函数的负梯度来拟合本轮迭代（优化）的近似值，进而拟合一棵CART回归树。

第$t$轮迭代的第$i$个样本的损失函数的负梯度为
$$r_i^{(t)}=-\left[\frac{\partial{L(y_i,f(x_i))}}{\partial{f(x_i)}} \right]_{f(x)=H_{t-1}(x)}$$

{% note danger %}
## GBDT回归算法
**输入**：训练数据集$\{(x_1,y_1),\cdots, (x_N,y_N)\}$
    
  $x_i\in X\subseteq \mathbb{R}^p,\quad y_i\in Y\subseteq \mathbb{R}$

**输出**：提升树
  $$H(x)=H_0(x)+\sum_{t=1}^T\sum_{j=1}^Jc_j^{(t)}\mathbf{I}\left(x\in R_j^{(t)} \right)$$

1. 初始化 
  $$H_0(x)=\arg{\min_c{\sum_{i=1}^NL(y_i, c)}}$$
2. 对于$t=1,\cdots, T$，
     1. 对$i=1,\cdots,N$，计算
      $$r_i^{(t)}=-\left[\frac{\partial{L(y_i,f(x_i))}}{\partial{f(x_i)}} \right]_{f(x)=H_{t-1}(x)}$$
     2. 对$r_i^{(t)}$拟合一棵回归树，得到第$t$棵树的叶结点区域$R_j^{(t)}\quad(j=1,\cdots,J)$，其中$J$为叶结点的个数
     3. 对$j=1,\cdots,J$，计算
      $$c_j^{(t)}=\arg{\min_c{\sum_{x_i\in R_j^{(t)}}L\left(y_i, H_{t-1}(x_i)+c \right)}}$$
     4. 更新
     $$H_t(x)=H_{t-1}(x)+\sum_{j=1}^Jc_j^{(t)}\mathbf{I}\left(x\in R_j^{(t)} \right)$$
{% endnote %}

再看GBDT分类算法。

当损失函数为指数损失函数时，此时二分类GBDT退化为{% post_link 算法-AdaBoost AdaBoost算法 %}。

下面讨论对数似然损失函数的情况。

损失函数为
$$L(y, H(x))=\log\left(1+\exp{\left(-yH(x) \right)} \right)$$

{% note danger %}
## GBDT二分类算法

**输入**：训练数据集$\{(x_1,y_1),\cdots, (x_N,y_N)\}$
    
  $x_i\in X\subseteq \mathbb{R}^p,\quad y_i\in \{-1,+1\}$

**输出**：提升树
  $$H(x)=H_0(x)+\sum_{t=1}^T\sum_{j=1}^Jc_j^{(t)}\mathbf{I}\left(x\in R_j^{(t)} \right)$$

1. 初始化 
  $$H_0(x)=\arg{\min_c{\sum_{i=1}^NL(y_i, c)}}$$
2. 对于$t=1,\cdots, T$，
     1. 对$i=1,\cdots,N$，计算
      $$r_i^{(t)}=-\left[\frac{\partial{L(y_i,f(x_i))}}{\partial{f(x_i)}} \right]_{f(x)=H_{t-1}(x)}=\frac{y_i}{1+\exp\left(y_iH_{t-1}(x_i) \right)}$$
     2. 对$r_i^{(t)}$拟合一棵决策树，得到第$t$棵树的叶结点区域$R_j^{(t)}\quad(j=1,\cdots,J)$，其中$J$为叶结点的个数
     3. 对$j=1,\cdots,J$，计算
      $$c_j^{(t)}=\arg{\min_c{\sum_{x_i\in R_j^{(t)}}\log{\left\{1+\exp{\left[-y_i \left(H_{t-1}(x_i)+c \right)\right]} \right\}}}}$$
      一般使用下面这个近似值代替
      $$c_j^{(t)}=\frac{\sum_{x_i\in R_j^{(t)}}r_i^{(t)}}{\sum_{x_i\in R_j^{(t)}}|r_i^{(t)}|\left(1- |r_i^{(t)}|\right)}$$
     4. 更新
     $$H_t(x)=H_{t-1}(x)+\sum_{j=1}^Jc_j^{(t)}\mathbf{I}\left(x\in R_j^{(t)} \right)$$
{% endnote %}

考虑多元GBDT分类。

假设类别数为$K$，对应的对数似然函数为
$$L(y, f(x))=-\sum_{k=1}^Ky_k\log{p_k(x)}$$
其中
$$y_k=\left\{
  \begin{array}{ll}
  1,&样本输出类别为k\\
  0,&否则
  \end{array}
  \right.$$
第$k$类的概率$p_k(x)$
$$p_k(x)=P(y=k|x)=\frac{\exp\{f_k(x)\}}{\sum_{l=1}^K\exp\{f_l(x)\}}$$
其中$f_1,\cdots,f_K$是$K$个不同的CART回归树。 
{% note default %}
每一轮的训练，实际上是训练了$K$棵树，去拟合Softmax的每一个分支模型的负梯度。
{% endnote %}

单个样本的损失函数为
$$L(y,x)=-\sum_{k=1}^Ky_k\log{\frac{\exp\{f_k(x)\}}{\sum_{l=1}^K\exp\{f_l(x)\}}}$$

负梯度为
$$-\frac{\partial{L}}{\partial{f_k(x)}}=y_k-\frac{\exp\{f_k(x)\}}{\sum_{l=1}^K\exp\{f_l(x)\}}=y_k-p_k(x)$$

> Python中`sklearn`的GBDT不支持多因变量，可以针对每个因变量单独运行GBDT，建立多个GBDT模型

{% note danger %}
## GBDT多分类算法

**输入**：训练数据集$\{(x_1,y_1),\cdots, (x_N,y_N)\}$
    
  $x_i\in X\subseteq \mathbb{R}^p,\quad y_i\in Y\subseteq \mathbb{R}^K$
  $$y_i=(y_{i1},\cdots,y_{iK})^T, \quad y_{ij}=\left\{\begin{array}{ll}
  1, & 如果第i个样本属于第k个类别\\
  0, & j\neq k
  \end{array} \right.$$
  

**输出**：提升树
  $$H(x)=\left(f_1(x),\cdots,f_K(x) \right)$$
  其中
  $$f_k(x)=\sum_{t=1}^T\sum_{j=1}^Jc_{jk}^{(t)}\mathbf{I}\left(x\in R_{jk}^{(t)} \right)$$

1. 初始化 
  $$H_0(x)=\left(f_1^{(0)}(x),\cdots,f_K^{(0)}(x) \right)^T, 其中f_j^{(0)}(x)=0(j=1,\cdots,K)$$
2. 对$t=1,\cdots,T$，

  1. 对$k=1,\cdots,K$，计算
    $$p_k^{(t)}(x)=\frac{\exp{f_k^{(t)}(x)}}{\sum_{l=1}^K\exp{f_l^{(t)}(x)}}$$
    1. 对$i=1,\cdots,N$，计算
      $$r_{ik}^{(t)}=y_{ik}-p_k^{(t)}(x_i)$$
    2. 对$r_{ik}^{(t)}$拟合一棵回归树，得到第$t$次迭代的第$k$棵树的叶结点区域$R_{jk}^{(t)}\quad(j=1,\cdots,J)$，其中$J$为叶结点的个数
    3. 对$j=1,\cdots,J$，计算
      $$c_{jk}^{(t)}=\frac{K-1}{K}\frac{\sum_{x_i\in R_{jk}^{(t)}}r_{ik}^{(t)}}{\sum_{x_i\in R_{jk}^{(t)}}|r_{ik}^{(t)}|(1-|r_{ik}^{(t)}|)}$$
    4. 更新
      $$f_k^{(t)}(x)=f_k^{(t-1)}(x)+\sum_{j=1}^Jc_{jk}^{(t)}\mathbf{I}\left(x\in R_{jk}^{(t)} \right)$$

{% endnote %}

## 正则化
为了防止过拟合，可以对GBDT进行正则化。
1. 与{% post_link 算法-AdaBoost AdaBoost %}类似的正则化项，即**步长**（learning rate）$\nu$，在弱学习器的迭代
  $$H_{t}(x)=H_{t-1}(x)+h_t(x)$$
 中添加正则化项，即
  $$H_{t}(x)=H_{t-1}(x)+\nu h_t(x)$$
 其中$0<\nu\leq1$
 - 对于同一训练集，较小的$\nu$意味着需要更多的弱学习器的迭代次数（$T$需要更大）。
 - 通常结合**步长**和**迭代最大次数**一起决定算法的拟合效果。
2. 子采样比例（subsample），取值为$(0,1]$
 - 不放回抽样（而随机森林是有放回抽样）
 - 小于1的比例可以减少方差（防止过拟合），但是会增加样本拟合的偏差，因此取值不能太低
 - 推荐在$[0.5, 0.8]$之间
 - 程序可以通过采样分发到不同的任务去做Boosting的迭代过程，最后形成新树（减少弱学习器难以并行学习的弱点）
 - 采用了子采样的GBDT也称**随机梯度提升树**（SGBT，Stochastic Gradient Boosting Tree）
3. 对弱学习器（CART回归树）进行正则化剪枝


# 损失函数
分类算法：
 - 对数损失函数
 - 指数损失函数

回归算法：
 - 平方损失函数（误差平方和）
 - 绝对损失函数（绝对误差和）
 - Huber损失
 - Quantile损失（分位数损失）

## 对数损失
二元
$$L(y, H(x))=\log\left\{1+\exp{\left[-yH(x) \right]} \right\}$$

多元
$$L(y, H(x))=-\sum_{k=1}^Ky_k\log{p_k(x)}$$

## 指数损失
$$L(y, H(x))=\exp{\left[-yH(x) \right]}$$

## 平方损失
$$L(y, H(x))=\left(y-H(x) \right)^2$$

## 绝对损失
$$L(y, H(x))=|y-H(x)|$$
对应负梯度误差为
$$\mathrm{sign}\left(y_i-H(x_i) \right)$$

## Huber损失
- 对于远离中心的异常点，采用绝对损失
- 中心附近的点，采用平方损失
- 这个界限一般用分位数点度量

$$L(y, H(x))=\left\{
  \begin{array}{ll}
  \frac{1}{2}(y-H(x))^2 ,& |y-H(x)|\leq \delta\\
  \delta \left(|y-H(x)|-\frac{\delta}{2} \right), & |y-H(x)|> \delta 
  \end{array}
  \right.$$

对应的负梯度误差为
$$r(y_i, H(x_i))=\left\{
  \begin{array}{ll}
  y_i-H(x_i), & |y_i-H(x_i)|\leq \delta\\
  \delta\mathrm{sign}\left(y_i-H(x_i) \right), & |y_i-H(x_i)|> \delta
  \end{array}
  \right.$$

## 分位数损失
对应的是分位数回归的损失函数
$$L(y,H(x))=\sum_{y\geq H(x)}\theta|y-H(x)|+\sum_{y< H(x)}(1-\theta)|y-H(x)|$$
其中$\theta$是分位数，需要提前指定。

对应的负梯度误差为
$$r(y_i, H(x_i))=\left\{
  \begin{array}{ll}
  \theta, & y_i\geq H(x_i)\\
  \theta-1, & y_i< H(x_i)
  \end{array}
  \right.$$


# 总结
## 优点
- 可以灵活处理各种类型的数据
 - 连续值
 - 离散值
- 使用Huber损失函数、Quantile损失函数，对异常值不敏感
- （相对{% post_link 算法-SVM SVM %}来说）在相对少的调参时间情况下，预测的准确率也可以比较高

## 缺点
- 弱学习器之间存在依赖关系，难以并行训练数据
  - 可通过自采样的SGBT来达到部分并行

# 比较
## GBDT vs RF
GBDT vs {% post_link 算法-随机森林 随机森林 %}

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
      <th>GBDT</th>
      <th>随机森林</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>集成思想</th>
      <td>Boosting</td>
      <td>Bagging</td>
    </tr>
    <tr>
      <th>基树</th>
      <td>只能是回归树</td>
      <td>分类树或回归树</td>
    </tr>
    <tr>
      <th>生成</th>
      <td>串行生成</td>
      <td>并行生成</td>
    </tr>
    <tr>
      <th>最终模型</th>
      <td>直接累加或加权累加</td>
      <td>多数投票</td>
    </tr>
    <tr>
      <th>异常点</th>
      <td>非常敏感</td>
      <td>不敏感</td>
    </tr>
    <tr>
      <th>训练集</th>
      <td>训练集样本具有权值</td>
      <td>对训练集一视同仁</td>
    </tr>
    <tr>
      <th>variance-bias</th>
      <td>减少模型的偏差（bias）</td>
      <td>减少模型的方差（variance）</td>
    </tr>
    <tr>
  </tbody>
</table>
</div>

## GBDT vs XGBoost

GBDT vs {% post_link 算法-XGBoost XGBoost %}


||GBDT|XGBoost|
|:--:|:----:|:------:|
|**基分类器**|CART|还支持线性分类器|
|**优化**|梯度下降法[^1]|牛顿法[^2]|
|**正则项**[^3]|没有|有|
|**Learning rate**||Shrinkage|
|**列抽样**||支持列抽样[^4]|
|**缺失值**||可以自动学习出缺失值的分裂方向|

[^1]: 在优化时只用到一阶导数信息
[^2]: XGBoost对代价函数进行了二阶泰勒展开，同时使用了一阶导数和二阶导数的信息；XGBoost还支持自定义代价函数
[^3]: 正则项降低了模型的Variance，学习得到的模型复杂度更低（防止过拟合）
[^4]: XGBoost借鉴了随机森林的做法，支持列抽样（column sampling），既能降低过拟合，还能减少计算

## GBDT vs AdaBoost
GBDT vs {% post_link 算法-AdaBoost AdaBoost %}

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
      <th>GBDT</th>
      <th>AdaBoost</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>共同点</th>
      <td colspan="2">目标都是优化偏差（Bias）</td>
    </tr>
    <tr>
      <th>每轮学习新的学习器</th>
      <td>通过改变输出值<br>每轮拟合的值为真实值与已有的加法模型的差值（即残差）</td>
      <td>通过改变样本的权值<br>关注上轮分类错误的样本的权值</td>
    </tr>
    <tr>
      <th>异常点</th>
      <td>一定程度上优化了AdaBoost异常点敏感的问题</td>
      <td>敏感</td>
    </tr>
    <tr>
      <th>树</th>
      <td>CART树</td>
      <td></td>
    </tr>
    <tr>
  </tbody>
</table>
</div>



# 参考资料
- [GBDT（MART） 迭代决策树入门教程 | 简介](https://blog.csdn.net/suranxu007/article/details/49910323)
- [刘建平Pinard-梯度提升树(GBDT)原理小结](https://www.cnblogs.com/pinard/p/6140514.html)
- [随机森林和GBDT的区别](http://www.17bigdata.com/%e9%9a%8f%e6%9c%ba%e6%a3%ae%e6%9e%97%e5%92%8cgbdt%e7%9a%84%e5%8c%ba%e5%88%ab/)
- [Boosting框架](https://www.zybuluo.com/vivounicorn/note/446479#24-bagging-and-boosting%E6%A1%86%E6%9E%B6)
- [李航-统计学习方法](https://book.douban.com/subject/10590856/)
- [深入理解GBDT多分类算法](https://zhuanlan.zhihu.com/p/91652813?utm_source=wechat_session)