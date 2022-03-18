---
title: Machine Learning | 熵
date: 2020-05-15 20:29:18
tags: [算法,机器学习]
categories: 机器学习
math: true
mathjax: true
hide: true
notshow: true
---

<center>Entropy</center>
<!--more-->

# 信息量
信息量是对信息的度量；有时也叫作**自信息**（self-information）。
- 信息的大小与随机事件发生的概率有关
 - 概率越小的事件发生了，产生的信息量越大
 > 如：处于非地震带的地方发生了地震

 - 概率越大的事件发生了，产生的信息量越小
 > 如太阳从东边升起

因此，事件的信息量随着其发生概率而递减，且不为负

**信息量**的公式：
$$I(x)=-\log{p(x)}$$
其中，$p(x)$为随机事件$x$发生的概率；负号保证了信息量一定不为负。

- **联合自信息量**
$$I(x_i,y_j)=-\log{p(x_i,y_j)}$$
- **条件自信息量**
$$I(y_j|x_i)=-\log{p(y_j|x_i)}$$




# 信息熵/香农熵
Entropy
- 熵是表示随机变量不确定性的度量

|**信息量**|**信息熵**|
|:--------:|:--------:|
|度量一个具体事件发生了所带来的信息|在事件发生之前对其结果可能产生的信息量求期望|
|对事件不确定性的度量|事件所有可能结果的自信息的期望值|

**信息熵**的公式如下：
$$H(X)=-\sum_{i=1}^np(x_i)\log{p(x_i)}$$
其中$p(x_i)$表示随机事件$x_i\ (i=1,\cdots,n)$的概率。

假设一组数据由$D=\{d_1,d_2,\cdots,d_n\}$构成，则这组数据的信息熵为
$$H(D)=-\sum_{i=1}^n\frac{d_i}{S_D}\log{\frac{d_i}{S_D}}$$
其中，$S_D=\sum_{i=1}^nd_i$。



## 对数的底
信息熵公式中的对数的底的选择是任意的
- 信息论中，一般选取**2**作为对数的底，则信息的单位为<u>比特（bits）</u>
- 机器学习中，一般选取**自然常数e**作为对数的底，则单位为<u>奈特（nats）</u>

## 性质
信息熵的一个性质为：
{% note success %}
$$0\leq H(X)\leq\log{n}$$
这里$n$表示事件的个数。
> 即：当随机分布为均匀分布时，熵最大
{% endnote %}
{% note default %}
证明：使用拉格朗日乘子法
因为$p(x_1)+p(x_2)+\cdots+p(x_n)=1$，所以目标函数为
$$f\left(p(x_1),\cdots,p(x_n)\right)=-\left[p(x_1)\log{p(x_1)}+p(x_2)\log{p(x_2)}+\cdots+p(x_n)\log{p(x_n)} \right]$$
约束条件为
$$g\left(p(x_1),\cdots,p(x_n)\right)=p(x_1)+p(x_2)+\cdots+p(x_n)-1=0$$
定义拉格朗日函数：
\begin{equation}
\begin{aligned}
L(p(x_1),\cdots,p(x_n);\lambda)&=-\left[p(x_1)\log{p(x_1)}+p(x_2)\log{p(x_2)}+\cdots+p(x_n)\log{p(x_n)} \right] \\
& +\lambda\left[p(x_1)+p(x_2)+\cdots+p(x_n)-1 \right] \\ 
\end{aligned}
\end{equation}

$L(p(x_1),\cdots,p(x_n);\lambda)$分别对$p(x_1),p(x_2),\cdots,p(x_n),\lambda$求偏导，并令偏导数为0，得到
\begin{equation}
\left\{
\begin{array}{l}
\lambda-\log p(x_1)-1=0\\
\lambda-\log p(x_2)-1=0\\
\cdots\\
\lambda-\log p(x_n)-1=0\\
p(x_1)+p(x_2)+\cdots+p(x_n)-1=0
\end{array}\right.
\end{equation}

解上述方程组，可得
$$p(x_1)=p(x_2)=\cdots=p(x_n)=\frac{1}{n}$$
所以目标函数的极大值为
$$f\left(\frac{1}{n},\frac{1}{n},\cdots,\frac{1}{n}\right)=-\left[\frac{1}{n}\log\frac{1}{n}+\frac{1}{n}\log\frac{1}{n}+\cdots+\frac{1}{n}\log\frac{1}{n} \right]=-\log\frac{1}{n}=\log n$$
即：当随机分布为均匀分布时，熵最大。
{% endnote %}


# 联合熵/复合熵
联合熵/复合熵：度量两个随机变量$X$和$Y$在一起时的不确定性

假设
- 离散情况：随机变量$(X,Y)$的联合概率分布为
$$P(X=x_i,Y=y_j)=p_{ij},\ i=1,\cdots,n;\ j=1,\cdots,m$$
- 连续情况：随机变量$(X,Y)$的联合密度函数为
$$f(x,y)\quad (x\in \Omega_X;\ y\in \Omega_Y)$$

**复合熵/联合熵**为
$$H(X,Y)=-\sum_{i=1}^n\sum_{j=1}^mp_{ij}\log{p_{ij}}$$
$$H(X,Y)=-\int_{\Omega_X}\int_{\Omega_Y}f(x,y)\log{f(x,y)}\mathrm{dx}\mathrm{dy}$$

## 性质
{% note success %}
联合熵大于独立熵的和
\begin{aligned}
  H(X,Y)&\geq\max{\left[H(X),H(Y) \right]}\\
  H(X_1,\cdots,X_N)&\geq\max{\left[H(X_1),\cdots,H(X_N) \right]}
\end{aligned}
{% endnote %}

{% note success %}
联合熵小于各独立熵的和
\begin{aligned}
H(X,Y)&\leq H(X)+H(Y)\\
H(X_1,\cdots,X_N)&\leq H(X_1)+\cdots+H(X_N)
\end{aligned}
{% endnote %}




# 条件熵
条件熵：代表在某一个条件$X$下，随机变量$Y$的复杂度（不确定度）
> 即：在给定条件X下，Y的条件概率分布的熵关于X的数学期望

**条件熵**$H(Y|X)$表示在已知随机变量$X$的条件下、随机变量$Y$ 的不确定性。
\begin{equation}
\begin{aligned}
  H(Y|X) &= \sum_{i=1}^np(x_i)H(Y|X=x_i)\\
&= -\sum_{i=1}^np(x_i)\sum_{j=1}^mp(y_j|x_i)\log{p(y_j|x_i)}\\
&= -\sum_{i=1}^n\sum_{j=1}^mp(x_i,y_j)\log{p(y_j|x_i)}\\
H(Y|X)&= \int_{x\in\Omega_X}f(x)H(Y|X=x)\mathrm{dx}\\
&= -\int_{x\in\Omega_X}f(x)\int_{y\in\Omega_Y}f(y|x)\log{f(y|x)}\mathrm{dy}\\
&= -\int_{x\in\Omega_X}\int_{y\in\Omega_Y}f(x)f(y|x)\log{f(y|x)}\mathrm{dx}\mathrm{dy}
\end{aligned}
\end{equation}

当信息熵和条件熵中的概率由样本数据估计而得时，所对应的信息熵与条件熵称为**经验熵**（empirical entropy）和**经验条件熵**（empirical conditional entropy）。

## 性质
{% note success %}
$$H(Y|X)=H(X,Y)-H(X)$$
{% endnote %}
{% note default %}
证明：（仅证明离散情况）
\begin{equation}
\begin{aligned}
H(Y|X)&= -\sum_{i=1}^n\sum_{j=1}^mp(x_i,y_j)\log{p(y_j|x_i)}\\
&= -\sum_{i=1}^n\sum_{j=1}^mp(x_i,y_j)\log{\frac{p(x_i,y_j)}{p(x_i)}}\\
&= -\sum_{i=1}^n\sum_{j=1}^mp(x_i,y_j)\left(\log{p(x_i,y_j)}-\log{p(x_i)}\right)\\
&= -\sum_{i=1}^n\sum_{j=1}^m\left(p(x_i,y_j)\log{p(x_i,y_j)}-p(x_i,y_j)\log{p(x_i)}\right)\\
&= -\sum_{i=1}^n\sum_{j=1}^mp(x_i,y_j)\log{p(x_i,y_j)}-\left(-\sum_{i=1}^n\sum_{j=1}^mp(x_i,y_j)\log{p(x_i)}\right)\\
&= H(X,Y)-H(X)
\end{aligned}
\end{equation}
同理可得
$$H(X|Y)=H(X,Y)-H(Y)$$
合并上述两个公式可得
$$H(Y|X)+H(X)=H(X,Y)=H(X|Y)+H(Y)$$

体现了熵的对称性。
{% endnote %}

# 相对熵/KL散度
**相对熵**，又称互熵、鉴别信息、Kullback熵、**Kullback-Leible散度**（KL散度），是两个概率分布间差异的非对称性度量；常常用来度量两个随机变量的“距离”。在信息论中，相对熵等价于两个概率分布的信息熵的差值。如果其中一个概率分布为真实分布，另一个为理论分布/拟合分布，则相对熵等于交叉熵与真实分布的信息熵之差，表示使用理论分布拟合真实分布时的信息损耗。

设$p(x)$和$q(x)$是两个概率分布，则$p$对$q$的相对熵为
$$D_{KL}(p||q)=E_{p(x)}\left(\log{\frac{p(x)}{q(x)}}\right)=\sum_{i=1}^np(x_i)\log{\frac{p(x_i)}{q(x_i)}}$$
相对熵不具有对称性：
$$D(p||q)\neq D(q||p)$$

## 性质
{% note success %}
相对熵不为负：
$$D(p||q)\geq0,\ D(q||p)\geq0$$
且相对熵公式只有在$p(x_i)$等于$q(x_i)$时等于0。
{% endnote %}
{% note default %}
证明：要证明
$$D_{KL}(p||q)=\sum_{i=1}^n\left(p(x_i)\log{p(x_i)}-p(x_i)\log{q(x_i)}\right)\geq0$$
即证
$$\sum_{i=1}^np(x_i)\log{\frac{q(x_i)}{p(x_i)}}\leq0$$
因为$\ln{x}\leq x-1$当且仅当$x=1$时等号成立，所以
$$\sum_{i=1}^np(x_i)\log{\frac{q(x_i)}{p(x_i)}}\leq\sum_{i=1}^np(x_i)\left(\frac{q(x_i)}{p(x_i)}-1\right)=\sum_{i=1}^n\left(q(x_i)-p(x_i)\right)$$
当且仅当$p(x_i)=q(x_i)$（对所有的$i,\ i=1,2,\cdots,n$）时有
$$\sum_{i=1}^np(x_i)\log{\frac{q(x_i)}{p(x_i)}}=\sum_{i=1}^n\left[q(x_i)-p(x_i)\right]=0$$
{% endnote %}

# 交叉熵
交叉熵（Cross entropy）：是一种损失函数/代价函数，用于描述模型预测值与真实值的差距大小。

真实概率分布$p(x)$和预测概率分布$q(x)$之间的交叉熵为
$$H(p,q)=-\sum_{i=1}^np(x_i)\log{q(x_i)}$$

交叉熵在分类问题中，常常与softmax搭配使用，softmax将输出的结果进行处理，使多个分类类别的预测值的和为1，再使用交叉熵计算损失。

将KL散度公式拆开：
\begin{equation}
\begin{aligned}
  D_{KL}(p||q)&=\sum_{i=1}^np(x_i)\log{\frac{p(x_i)}{q(x_i)}}\\
&= \underline{\sum_{i=1}^np(x_i)\log{p(x_i)}}-\sum_{i=1}^np(x_i)\log{q(x_i)}\\
&= \underline{-H\left(p(x)\right)}+\left[-\sum_{i=1}^np(x_i)\log{q(x_i)} \right]
\end{aligned}
\end{equation}
其中，$H\left(p(x)\right)$表示真实分布的信息熵，后者即为交叉熵。

$$KL散度=交叉熵-信息熵$$
KL散度（相对熵）衡量的是真实概率分布与预测概率分布之间的差异；KL散度越小，表明预测的效果越好。在机器学习训练模型时，输入数据（Input）与标签（Label）通常已确定，那么真实概率分布$p$也已确定，信息熵$H\left(p(x)\right)$就是一个常量。因此，最小化KL散度等价于最小化交叉熵。所以，在机器学习中常常使用交叉熵作为损失函数。

# 互信息
互信息（Mutual Information）：也是信息论中的一种信息度量，是一个随机变量中包含的关于另一个随机变量的信息量，或者说，是一个随机变量由于已知另一个随机变量而减少的不确定性。

- 两个**离散**随机变量$X$和$Y$的互信息定义为
$$I(X,Y)=\sum_{y\in Y}\sum_{x\in X}p(x,y)\log{\frac{p(x,y)}{p(x)p(y)}}$$
其中，$p(x,y)$是$X$和$Y$的联合概率分布函数，$p(x)$、$p(y)$分别是$X$和$Y$的边缘概率分布函数。
- 两个**连续**随机变量的互信息定义为
$$I(X,Y)=\int_Y\int_Xp(x,y)\log{\frac{p(x,y)}{p(x)p(y)}}\mathrm{d}x\mathrm{d}y$$
其中，$p(x,y)$是$X$和$Y$的联合概率密度函数，$p(x)$、$p(y)$分别是$X$和$Y$的边缘概率密度函数。

- 互信息是互信息量$I(x_i,y_j)$在联合概率空间中的统计平均值。
- （平均）互信息克服了互信息量$I(x_i,y_j)$的随机性，是一个确定的量。
- 互信息是$X$和$Y$的联合分布相对于假定$X$和$Y$独立情况下的联合分布之间的内在依赖性。
- $I(X,Y)=0$当且仅当$X$和$Y$独立时成立。
- 当$X$和$Y$独立时，$p(x,y)=p(x)p(y)$。因此
 $$\log{\frac{p(x,y)}{p(x)p(y)}}=\log1=0$$

{% note success %}
互信息和条件熵的关系为
$$I(X,Y)=H(X)-H(Y|X)=H(Y)-H(Y|X)$$
{% endnote %}

{% note default %}
证明:
\begin{aligned}
H(X)-H(X,Y) &= -\sum_{x\in X}\underline{p(x)}\log{p(x)}+\sum_{x\in X}\sum_{y\in Y}p(x,y)\log{p(x|y)}\\
&= \sum_{x\in X}\sum_{y\in Y}p(x,y)\log{p(x|y)}-\sum_{x\in X}\underline{\sum_{y\in Y}p(x,y)}\log{p(x)}\\
&= \sum_{x\in X}\sum_{y\in Y}p(x,y)\left(\log{p(x|y)}-\log{p(x)} \right)\\
&= \sum_{x\in X}\sum_{y\in Y}p(x,y)\log{\frac{p(x|y)}{p(x)}}\\
&= \sum_{x\in X}\sum_{y\in Y}\log{\frac{p(x,y)}{p(x)p(y)}}\\
&= I(X,Y)
\end{aligned}
{% endnote %}




<meta name="referrer" content="no-referrer" />
{% asset_img mi.png 信息熵、联合熵、条件熵、互信息的关系 %}


# 参考资料
- [通俗理解信息熵](https://zhuanlan.zhihu.com/p/26486223)
- [ID3算法](http://blog.sina.com.cn/s/blog_6e85bf420100ohma.html)
- [“熵”的通俗解释](https://ask.julyedu.com/question/6897)
- [通俗理解条件熵](https://zhuanlan.zhihu.com/p/26551798)
- [机器学习笔记十：各种熵总结](https://blog.csdn.net/xierhacker/article/details/53463567)
- [相对熵（KL散度）](https://blog.csdn.net/weixinhum/article/details/85064685)
- [互信息的深度理解](https://blog.csdn.net/BigData_Mining/article/details/81279612)
- [机器学习中各种熵](https://blog.csdn.net/NeilGY/article/details/98216164)
- [详解机器学习中的熵、条件熵、相对熵和交叉熵](https://www.cnblogs.com/kyrieng/p/8694705.html)
- [交叉熵](https://www.jiqizhixin.com/graph/technologies/1786086f-5b63-4eee-b9ed-dad4d64cdc86)
- [交叉熵损失函数原理详解](https://blog.csdn.net/b1055077005/article/details/100152102)
- [为什么交叉熵可以用于计算代价](https://www.zhihu.com/question/65288314/answer/244557337)