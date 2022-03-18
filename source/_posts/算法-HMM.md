---
title: Machine Learning | 隐马尔可夫模型
date: 2020-06-03 17:43:09
tags: [机器学习,算法,生成式模型]
#index_img: /img/random_forest.jpg
categories: 机器学习
mathjax: true
toc: true
hide: true
notshow: true
---

<center>Hidden Markov Model</center>
<!--more-->

# HMM
隐马尔可夫模型是关于**时序**的概率模型。
- 用来描述一个含有隐含未知参数的马尔可夫过程
- 是可用于标注问题的统计学习模型
- {% post_link 生成式模型-判别式模型 生成式模型 %}
- 隐马尔可夫模型由**初始概率分布**、**状态转移概率分布**以及**观测概率分布**确定。
- HMM在语言识别、自然语言处理、模式识别等领域得到广泛的应用
- HMM适用的问题：
 1. 问题是基于序列的（如时间序列，或状态序列）
 2. 问题中有两类数据，一类序列数据是可以观测到的（**观测序列**），另一类数据是不可观测的（**隐藏状态序列**，简称**状态序列**）

隐马尔可夫模型描述由一个隐藏的马尔可夫链随机生成不可观测的状态随机序列，再由各个状态生成一个观测而产生观测随机序列的过程。
- **状态序列**（state sequence）：隐藏的马尔可夫链随机生成的状态的序列
- **观测序列**（observation sequence）：每个状态生成一个观测，而由此产的观测的随机序列
- 序列的每一个位置又可以看作是一个时刻

{% note warning %}
假设：
- $Q$是所有可能的隐藏状态的集合
 $$Q=\{q_1, q_2,\cdots, q_N\}$$
 其中，$N$是可能的隐藏状态数。
- $V$是所有可能的观测状态的集合
 $$V=\{v_1,v_2,\cdots,v_M\}$$
 其中，$M$是可能的观测状态数。

对于一个长度为$T$的序列，
- $I$是对应的状态序列
 $$I=\{i_1,i_2,\cdots, i_T\}$$
- $O$是对应的观测序列
 $$O=\{o_1,o_2,\cdots,o_T\}$$
其中，$\forall i_t\in Q,\quad \forall o_t\in V$。
{% endnote %}

隐马尔可夫模型由**初始概率分布**、**状态转移概率分布**以及**观测概率分布**确定。

{% note warning %}
在时刻$t=1$的隐藏状态概率分布为
$$\Pi=\left(\pi_i\right)_{N\times 1}$$
其中，
$$\pi_i=P(i_1=q_i)$$
$i=1,\cdots,N$。
{% endnote %}


## 基本假设
HMM作了两个基本假设
{% note info %}
### 齐次马尔可夫性假设
假设马尔可夫链在任意时刻$t$的隐藏状态只依赖于其前一时刻的状态，与其他时刻的状态及观测无关，也与时刻$t$无关。
{% endnote %}
基于该假设，有
{% note warning %}
如果在时刻$t$的隐藏状态为$i_t=q_i$，在时刻$t+1$的隐藏状态为$i_{t+1}=q_j$，则从时刻$t$到时刻$t+1$的HMM状态转移概率$a_{ij}$为
$$a_{ij}=P(i_{t+1}=q_j|i_t=q_i)$$
其中$i=1,\cdots,N;j=1,\cdots,N$。

马尔可夫链的状态转移概率矩阵为
$$A=\left[a_{ij} \right]_{N\times N}$$
{% endnote %}

{% note info %}
### 观测独立性假设
假设任意时刻的观测状态只依赖于该时刻的隐藏状态，与其他观测及状态无关。
{% endnote %}
基于该假设，有
{% note warning %}
如果在时刻$t$的隐藏状态为$i_t=q_j$，而对应的观测状态为$o_t=v_k$，则该时刻观测状态$v_k$在隐藏状态$q_j$下生成的概率为$b_j(k)$，满足
$$b_j(k)=P\left(o_t=v_k|i_t=q_j\right)$$
其中$k=1,\cdots,M;j=1,\cdots,N$。

则观测状态生成的概率矩阵为
$$B=\left[b_j(k) \right]_{N\times M}$$
{% endnote %}

隐马尔可夫模型由**初始状态概率向量**$\Pi$、**状态转移概率矩阵**$A$以及**观测概率矩阵**$B$确定。
- $\Pi$和$A$决定状态序列
- $B$决定观测序列

隐马尔可夫模型科用三元组表示：
$$\lambda=(A,B,\Pi)$$
$A,B,\Pi$称为隐马尔可夫模型的**三要素**。

{% note danger %}
## 算法：观测序列的生成
**输入**：
 - 隐马尔可夫模型$\lambda=(A,B,\Pi)$
 - 观测序列的长度$T$

**输出**：观测序列$O=\{o_1,\cdots,o_T\}$

1. 根据初始状态概率分布$\Pi$生成隐藏状态$i_1$
2. 对于$t=1,\cdots,T$，
    1. 按照隐藏状态$i_t$的观测状态分布$b_{i_t}(k)$生成观测状态$o_t$
    2. 按照隐藏状态$i_t$的状态转移概率分布$a_{i_t,i_{t+1}}$产生隐藏状态$i_{t+1}\quad (i_{t+1}=1,\cdots,N)$
{% endnote %}

## 基本问题
隐马尔可夫模型有三个经典的基本问题需要解决：
1. **概率计算问题**
 - 给定模型$\lambda=(A,B,\Pi)$和观测序列$O=\{o_1,\cdots,o_T\}$，计算在模型$\lambda$下观测序列$O$出现的概率$P(O|\lambda)$
 - 用**前向后向算法**
2. **模型参数学习问题**
 - 给定观测序列$O=\{o_1,\cdots,o_T\}$，估计模型$\lambda=(A,B,\Pi)$的参数，使得在该模型下观测序列的条件概率$P(O|\lambda)$最大
 - 用**极大似然估计**的方法估计参数
 - 用**基于EM算法的Baum-Welch算法**
3. **预测问题/解码（decoding）问题**
 - 给定模型$\lambda=(A,B,\Pi)$和观测序列$O=\{o_1,\cdots,o_T\}$，求给定观测序列条件该概率$P(I|O)$下，最可能出现的对应的状态序列$I=\{i_1,\cdots,i_T\}$
 - 用**基于动态规划的维特比算法**

# 概率计算问题
给定模型$\lambda=(A,B,\Pi)$和观测序列$O=\{o_1,\cdots,o_T\}$，计算观测序列$O$在模型$\lambda$下出现的条件概率$P(O|\lambda)$。

{% note danger %}
## 直接计算法
列举所有可能出现的长度为$T$的隐藏序列$I=\{i_1,i_2,\cdots,i_T\}$，分别求出各个状态序列$I$与观测序列$O=\{o_1,o_2,\cdots,o_T\}$的联合概率分布$P(O,I|\lambda)$，然后对所有可能的状态序列求和，得到边缘分布$P(O|\lambda)$。
{% endnote %}

任意一个隐藏序列$I=\{i_1,i_2,\cdots,i_T\}$出现的概率是
$$P(I|\lambda)=\pi_{i_1}a_{i_1i_2}a_{i_2i_3}\cdots a_{i_{T-1}i_T}$$

对固定的状态序列$I=\{i_1,i_2,\cdots,i_T\}$，（要求的）观测序列$O=\{o_1,o_2,\cdots,o_T\}$出现的概率是
$$P(O|I,\lambda)=b_{i_1}(o_1)b_{i_2}(o_2)\cdots b_{i_T}(o_T)$$

则$O$和$I$同时出现的联合概率为
\begin{aligned}
P(O,I|\lambda) &= P(I|\lambda)P(O|I,\lambda)\\
&= \pi_{i_1}b_{i_1}(o_1)a_{i_1i_2}b_{i_2}(o_2)\cdots a_{i_{T-1}i_T}b_{i_T}(o_T)
\end{aligned}

然后对所有可能的状态序列$I$求和，得到观测序列$O$的边缘概率
\begin{aligned}
P(O|\lambda) &= \sum_I P(O,I|\lambda)\\
&= \sum_{I}P(O|I,\lambda)P(I|\lambda)\\
&= \sum_{i_1,i_2,\cdots,i_T}\pi_{i_1}b_{i_1}(o_1)a_{i_1i_2}b_{i_2}(o_2)\cdots a_{i_{T-1}i_T}b_{i_T}(o_T)
\end{aligned}

- 预测状态有$N^T$种组合
- 算法的时间复杂度为$O(TN^T)$

对于一些隐藏状态数极少的模型，可以使用上述“暴力求解”法来求解观测序列出现的概率。但是，如果隐藏状态多，上述算法太过耗时，则应考虑**前向后向算法**。

{% note warning %}
**前向概率**：

给定隐马尔可夫模型$\lambda$，定义时刻$t$的隐藏状态为$q_i$，观测状态序列为$o_1,o_2,\cdots,o_t$的概率为前向概率。记为
$$\alpha_t(i)=P(o_1,o_2,\cdots,o_t,i_t=q_i|\lambda)$$
{% endnote %}

假设已经找到在时刻$t$时各个隐藏状态的前向概率，需要递推初时刻$t+1$时各个隐藏状态的前向概率。

基于时刻$t$时各个隐藏状态的前向概率$\alpha_t(j)$，乘以对应的状态转移概率$a_{ji}$，得到时刻$t$隐藏状态为$q_j$、时刻$t+1$隐藏状态为$q_i$的概率$\alpha_t(j)a_{ji}$；对所有$j$求和，得到时刻$t+1$观测序列为$o_1,o_2,\cdots,o_t,o_{t+1}$且隐藏状态为$q_i$的概率$\alpha_t(j)a_{ji}$。

因为观测状态$o_{t+1}$只依赖于$t+1$时刻的隐藏状态$q_i$，所以$\left[\sum_{j=1}^N\alpha_t(j)a_{ji} \right]b_i(o_{t+1})$为在时刻$t+1$观测到$o_1,o_2,\cdots,o_t,o_{t+1}$且时刻$t+1$的隐藏状态为$q_i$的前向概率。所以前向概率的递推关系式为
$$\alpha_{t+1}(i)=\left[\sum_{j=1}^N\alpha_t(j)a_{ji} \right]b_i(o_{t+1})$$

$\alpha_T(i)$表示在时刻$T$观测序列为$o_1,o_2,\cdots,o_T$，且时刻$T$隐藏状态为$q_i$的概率，所以$\sum_{i=1}^N\alpha_T(i)$表示在时刻$T$观测序列为$o_1,o_2,\cdots,o_T$的概率。

<meta name="referrer" content="no-referrer" />
{% asset_img forward.png 来自[刘建平Pinard-隐马尔科夫模型HMM（二）前向后向算法评估观察序列概率] %}

{% note danger %}
## 前向算法
**输入**：
 - 隐马尔可夫模型$\lambda$
 - 观测序列$O$

**输出**：观测序列概率$P(O|\lambda)$
 $$P(O|\lambda)=\sum_{i=1}^N\alpha_T(i)$$

1. 计算时刻$1$的各个隐藏状态前向概率$(i=1,2,\cdots,N)$
 $$\alpha_1(i)=\pi_ib_i(o_1)$$
2. 递推：对$t=1,2, \cdots, T-1$
 $$\alpha_{t+1}(i)=\left[\sum_{j=1}^N\alpha_t(j)a_{ji} \right]b_i(o_{t+1})$$
 $i=1,2,\cdots,N$。
{% endnote %}

- 前向算法本质上属于**动态规划**的算法
- 通过找到局部状态递推的公式，从子问题的最优解拓展到整个问题的最优解
- 前向算法的时间复杂度为$O(TN^2)$

{% note warning %}
**后向概率**：

给定隐马尔可夫模型$\lambda$，定义时刻$t$时隐藏状态为$q_i$，从时刻$t+1$到最后时刻$T$的观测状态序列为$o_{t+1},o_{t+2},\cdots,o_T$的概率为后向概率。记为
$$\beta_t(i)=P(o_{t+1},o_{t+2},\cdots,o_T|i_t=q_i, \lambda)$$
{% endnote %}

假设已经找到在时刻$t+1$时各个隐藏状态的后向概率$\beta_{t+1}(j)$，需要递推初时刻$t$的各个隐藏状态的后向概率。

可以计算出观测状态序列为$o_{t+2},o_{t+3},\cdots,o_T$、$t$时隐藏状态为$q_i$、$t+1$时隐藏状态为$q_j$的概率为$a_{ij}\beta_{t+1}(j)$；则观测状态序列为$o_{t+1},o_{t+2},o_{t+3},\cdots,o_T$、$t$时隐藏状态为$q_i$、$t+1$时隐藏状态为$q_j$的概率为$a_{ij}b_j(o_{t+1})\beta_{t+1}(j)$；将$t+1$时所有隐藏状态概率加总，得到观测状态序列为$o_{t+1},o_{t+2},o_{t+3},\cdots,o_T$、$t$时隐藏状态为$q_i$的概率为$\sum_{j=1}^Na_{ij}b_j(o_{t+1})\beta_{t+1}(j)$，即时刻$t$的后向概率。

<meta name="referrer" content="no-referrer" />
{% asset_img backward.png 来自[刘建平Pinard-隐马尔科夫模型HMM（二）前向后向算法评估观察序列概率] %}

{% note danger %}
## 后向算法
**输入**：
 - 隐马尔可夫模型$\lambda$
 - 观测序列$O$

**输出**：观测序列概率$P(O|\lambda)$
 $$P(O|\lambda)=\sum_{i=1}^N\pi_ib_i(o_1)\beta_1(i)$$

1. 初始化时刻$T$的各个隐藏状态后向概率
 $$\beta_T(i)=1$$
 $i=1,2,\cdots,N$。
2. 递推：对于$t=T-1,T-2,\cdots,1$，计算
 $$\beta_t(i)=\sum_{j=1}^Na_{ij}b_j(o_{t+1})\beta_{t+1}(j)$$
 $i=1,2,\cdots,N$。
{% endnote %}
- 后向算法本质上也是属于**动态规划**的算法
- 后向算法的时间复杂度为$O(TN^2)$

{% note warning %}
## 常用概率计算
给定隐马尔可夫模型$\lambda$和观测序列$O$，
- 在时刻$t$处于状态$q_i$的概率为

\begin{aligned}
\gamma_t(i)&=P(i_t=q_i|O,\lambda) \\
&= \frac{P(i_t=q_i,O|\lambda)}{P(O|\lambda)} \\
&= \frac{\alpha_t(i)\beta_t(i)}{\sum_{j=1}^N\alpha_t(j)\beta_t(j)} \\
\end{aligned}

- 在时刻$t$处于状态$q_i$、时刻$t+1$处于状态$q_j$的概率为

\begin{equation}
\begin{aligned}
\xi_t(i,j) &= P(i_t=q_i,i_{t+1}=q_j | O,\lambda)\\
&= \frac{P(i_t=q_i,i_{t+1}=q_j,O|\lambda)}{P(O|\lambda)}\\
&= \frac{\alpha_t(i)a_{ij} b_j(o_{t+1}) \beta_{t+1}(j)}{\sum_{r=1}^N \sum_{s=1}^N \alpha_t ( r ) a_{rs} b_s(o_{t+1})\beta_{t+1}(s)} \\
\end{aligned}
\end{equation}

- 在观测序列$O$下
 - 状态$i$出现的期望值为
  $$\sum_{t=1}^T\gamma_t(i)$$
 - 由状态$i$转移的期望值
  $$\sum_{t=1}^{T-1}\gamma_t(i)$$
 - 由状态$i$转移到状态$j$的期望值
  $$\sum_{t=1}^{T-1}\xi_t(i,j)$$
{% endnote %}

# 参数学习问题
隐马尔可夫模型的参数求解根据<u>已知条件</u>可分为两种情况：
1. 已知观测序列和对应的隐藏状态序列，可用**极大似然估计法**来求解模型参数
2. 只有观测序列，而无法得到对应的隐藏状态序列，可用**基于EM算法的Baum-Welch算法**（鲍姆-韦尔奇算法）来求解模型参数

对于**第一种情况**：
已知$D$个长度为$T$的观测序列和对应的隐藏序列
$$\left\{ (O_1,I_1,),(O_2,I_2), \cdots, (O_D,I_D) \right\}$$

{% note danger %}
## 极大似然估计
- 假设样本从隐藏状态$q_i$转移到$q_j$的频率计数是$A_{ij}$，则状态转移矩阵为
 $$A=\left[a_{ij} \right]_{N\times N}$$
 其中
 $$a_{ij}=\frac{A_{ij}}{\sum_{s=1}^NA_{is}}$$
- 假设样本隐藏状态为$q_j$且观测状态为$v_k$的频率计数是$B_{jk}$，则观测状态概率矩阵为
 $$B=\left[b_j(k) \right]_{N\times M}$$
 其中
 $$b_j(k)=\frac{B_{jk}}{\sum_{s=1}B_{js}}$$
- 假设所有样本种初始隐藏状态为$q_i$的频率计数为$C(i)$，则初始概率分布为
 $$\Pi=\left[\pi(i) \right]_{N\times 1}$$
 其中
 $$\pi(i)=\frac{C(i)}{\sum_{s=1}^NC(s)}$$

{% endnote %}

对于**第二种情况**：
只有$D$个长度为$T$的观测序列
$$\{(O_1),(O_2),\cdots,(O_D)\}$$

基于EM算法的Baum-Welch算法：
- E步：基于条件概率$P(I|O,\overline{\lambda})$求出联合分布$P(O,I|\lambda)$
 其中$\overline{\lambda}$为当前的模型参数
- M步：最大化联合分布$P(O,I|\lambda)$的期望，得到更新的模型参数$\lambda$

不停地进行EM迭代，直到模型参数的值收敛为止。

{% note danger %}
## Baum-Welch算法

{% endnote %}

# 预测问题


未完待续

# 笔试题目
{% note default %}
在HMM中,如果已知观察序列和产生观察序列的状态序列,那么可用以下哪种方法直接进行参数估计(D)
A. EM算法
B. 维特比算法
C. 前向后向算法
D. 极大似然估计

[<font face="宋体" color="grey">来自：[牛客试题广场](https://www.nowcoder.com/questionTerminal/dc4e7ad7e9634b65b56f2541a580eba0)</font>]
{% endnote %}
- EM算法：只有观测序列，无状态序列时，用于学习模型参数，即Baum-Welch算法
- 维特比算法：用动态规划解决HMM的预测问题，不是参数估计
- 前向后向算法：用于算概率
- 极大似然估计：预测序列和相应的状态序列都存在时的监督学习算法，用来估计参数



# 参考资料
- [李航-统计学习方法](https://book.douban.com/subject/10590856/)
- [一文搞懂HMM（隐马尔可夫模型）](https://www.cnblogs.com/skyme/p/4651331.html)
- [刘建平Pinard-隐马尔科夫模型HMM（一）HMM模型](https://www.cnblogs.com/pinard/p/6945257.html)
- [刘建平Pinard-隐马尔科夫模型HMM（二）前向后向算法评估观察序列概率](https://www.cnblogs.com/pinard/p/6955871.html)
- [隐马尔科夫模型HMM（三）鲍姆-韦尔奇算法求解HMM参数](https://www.cnblogs.com/pinard/p/6972299.html)