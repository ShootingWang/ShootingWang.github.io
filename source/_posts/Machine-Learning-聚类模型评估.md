---
title: Machine Learning | 聚类模型评估
date: 2020-05-11 08:59:07
tags: [算法,机器学习,聚类]
categories: 机器学习
math: true
mathjax: true
index_img: /img/ml.jpg
hide: true
notshow: true
---

<center>Clustering</center>
<!--more-->

**模型评估/评价**：对于已经建立的一个或多个模型，根据模型的类别，选择不同的指标评价其性能优劣的过程。

# 聚类评估

关于聚类分析，往往是希望聚类结果中，组内（同类别）的相似性（稠密程度）越大、组间（不同类别）的相似性（离散程度）越小。

聚类模型的评估指标/方法：
- ARI评价法（兰德系数）
- AMI评价法（互信息）
- V-measure评分
- FMI评价法
- 轮廓系数
- Calinski-Harabasz分数
- ……

## ARI评价法
### 兰德系数RI
**兰德系数/兰德指数**（RI，Rand Index）：用于聚类模型的性能评估。
- 需要真实的类别
- 最佳值为1
- RI的取值范围为 [0,1]；值越大意味着聚类结果与真实情况越吻合

> - 样本个数$n$
- $n$个样本集合$S=\{x_1,\cdots,x_n\}$
- 样本集合的真实划分$U=\{u_1,\cdots,u_R\}$
- 样本集合的聚类结果$V=\{v_1,\cdots,v_C\}$
- $a$：在$U$中为同一类且在$V$中也为同一类别的数据点对数（即TP）
- $b$：在$U$中为同一类但在$V$中却是不同类别的数据点对数（即FN）
- $c$：在$U$中不是同一类但在$V$中却是同一类的数据点对数（即FP）
-  $d$：在$U$中不是同一类且在$V$中也是不同类别的数据点对数（即TN）

|Class\Cluster|Same Cluster|Different Cluster| Sum(U) |
|:-----------:|:----------:|:---------------:|:---:|
|Same Class| a | b | a+b|
|Different Class | c | d| c+d|
|Sum(V)|a+c|b+d|a+b+c+d|

则兰德指数为
$$\mathrm{RI}=\frac{a+d}{C_n^2}=\frac{TP+TN}{TP+FP+TN+FN}$$
其中，
$$C_n^2=
\left(\begin{array}{c}
n\\
2
\end{array}\right)=\frac{n(n-1)}{2}
$$
是样本集中能够组成的元素对数。

### 调整兰德系数ARI
对于随机聚类结果，RI并不能保证接近零。为了实现“在聚类结果随机产生的情况下，指标应该接近零”，有人提出了**调整兰德系数**（ARI，Adjusted Rand Index），具有更高的区分度
- ARI的取值范围为 [-1,1] ；值越大意味着聚类结果与真实情况越吻合
- ARI实际是RI去均值归一化
- 从广义的角度来看，ARI衡量的是两个数据分布的吻合程度（相似度）

$$\mathrm{ARI}=\frac{\mathrm{RI}-E(\mathrm{RI})}{\max{(\mathrm{RI})}-E(\mathrm{RI})}$$

|Class\Cluster | $v_1$ | $v_2$ | $\cdots$ | $v_C$ | SUM |
|:------------:|:-----:|:-----:|:--------:|:-----:|:---:|
|$u_1$ | $n_{11}$ | $n_{12}$ | $\cdots$ | $n_{1C}$ | $n_{1.}$|
|$u_2$ | $n_{21}$ | $n_{22}$ | $\cdots$ | $n_{2C}$ | $n_{2.}$|
|$\cdots$ | $\cdots$ | $\cdots$ | $\cdots$ | $\cdots$ | $\cdots$|
|$u_R$ | $n_{R1}$ | $n_{R2}$ | $\cdots$  | $n_{RC}$ |  $n_{R.}$|
|SUM  | $n_{.1}$ | $n_{.2}$ | $\cdots$ | $n_{. C}$ | $n_{..}=n$|

RI中的$a+d$可表示为
$$\sum_{i,j}\left(\begin{array}{c}
n_{ij}\\
2
\end{array}\right)
$$
且
$$E(\mathrm{RI})=E(\sum_{i,j}\left(\begin{array}{c}
n_{ij}\\
2
\end{array}\right))=\frac{\sum_i\left(\begin{array}{c}
n_{i.}\\
2
\end{array}\right)\sum_j\left(\begin{array}{c}
n_{.j}\\
2
\end{array}\right)
}{
\left(\begin{array}{c}
n\\2
\end{array}\right)
}$$

\begin{equation}
\max{(\mathrm{RI})}=\frac{1}{2}[\sum_i\left(\begin{array}{c}
n_{i.}\\
2
\end{array}\right)+\sum_j\left(\begin{array}{c}
n_{.j}\\
2
\end{array}\right)]
\end{equation}

```Python
# Python实现
from sklearn.metrics import adjust_rand_score

ari = adjusted_rand_score(labels_true, labels_pred)
# labels_true：真实的分类标签
# label_pred：模型聚类的结果
# ARI = (RI - Expected_RI) / (max(RI) - Expected_RI)
# adjusted_rand_score(a, b) == adjusted_rand_score(b, a)
```

## AMI评价法
**互信息**（MI，Mutual Information）：衡量两个数据分布的吻合程度。
- 需要真实类别
- 最佳值为1
- MI和NMI的取值范围为 [0,1]；AMI的取值范围为 [-1,1]；都是值越大，意味着聚类结果与真实情况越吻合

### 互信息MI
两个离散随机变量$X$和$Y$的互信息定义为
$$I(X,Y)=\sum_{y\in Y}\sum_{x\in X}p(x,y)\log{\frac{p(x,y)}{p(x)p(y)}}$$
其中，$p(x,y)$是$X$和$Y$的联合概率分布函数，$p(x)$、$p(y)$分别是$X$和$Y$的边缘概率分布函数。
两个连续随机变量的互信息定义为
$$I(X,Y)=\int_Y\int_Xp(x,y)\log{\frac{p(x,y)}{p(x)p(y)}}\mathrm{d}x\mathrm{d}y$$
其中，$p(x,y)$是$X$和$Y$的联合概率密度函数，$p(x)$、$p(y)$分别是$X$和$Y$的边缘概率密度函数。
- 互信息是互信息量$I(x_i,y_j)$在联合概率空间中的统计平均值
- （平均）互信息克服了互信息量$I(x_i,y_j)$的随机性，是一个确定的量
- 互信息是$X$和$Y$的联合分布相对于假定$X$和$Y$独立情况下的联合分布之间的内在依赖性
- $I(X,Y)=0$当且仅当$X$和$Y$独立时成立
- 当$X$和$Y$独立时，$p(x,y)=p(x)p(y)$。因此
$$\log{\frac{p(x,y)}{p(x)p(y)}}=\log1=0$$

用互信息来评估聚类效果：
> - 样本个数$n$
- $n$个样本集合$S=\{x_1,\cdots,x_n\}$
- 样本集合的真实划分$U=\{u_1,\cdots,u_R\}$
- 样本集合的聚类结果$V=\{v_1,\cdots,v_C\}$
- $P(i)$为$u_i$在$U$中的概率（比率）
 $$P(i)=\frac{||u_i||}{n}=\frac{n_{i.}}{n}$$
- $P^\prime(j)$为$v_j$在$V$中的概率（比率）
 $$P^\prime(j)=\frac{||v_j||}{n}=\frac{n_{.j}}{n}$$
- $U$的熵为
 $$H(U)=\sum_{i=1}^RP(i)\log{\left(P(i)\right)}$$
- $V$的熵为
 $$H(V)=\sum_{j=1}^CP^\prime(j)\log{\left(P^\prime(j)\right)}$$

$U$和$V$之间的**互信息**（MI）为
$$\mathrm{MI}(U,V)=\sum_{i=1}^R\sum_{j=1}^CP(i,j)\log{\left(\frac{P(i,j)}{P(i)P^\prime(j)}\right)}$$
其中$P(i,j)=n_{ij}/n$。

### 标准化的互信息NMI
**标准化后的互信息**（NMI，Normalized Mutual Information）为
$$\mathrm{NMI}(U,V)=\frac{\mathrm{MI}(U,V)}{\sqrt{H(U)H(V)}}$$

### 调整互信息AMI
**调整互信息**（AMI，Adjusted Mutual Information）为

$$\mathrm{AMI}=\frac{\mathrm{MI}-E[\mathrm{MI}]}{\max{\left(H(U),H(V)\right)}-E[\mathrm{MI}]}$$

\begin{equation}
\begin{aligned}
E[\mathrm{MI}(U,V)]=&\sum_{i=1}^R\sum_{j=1}^C\sum_{n_{ij}=(n_{i.}+n_{.j}-n)^+}^{\min{(n_{i.},n_{.j})}}\frac{n_{ij}}{n}
\log{\left(\frac{n\cdot n_{ij}}{n_{i.}n_{.j}}\right)}\times\\
&\frac{n_{i.}!n_{.j}(n-n_{i.})!(n-n_{.j})!}{n!n_{ij}!(n_{i.}-n_{ij})!(n_{.j-n_{ij}})!(n-n_{i.}-n_{.j}+n_{ij})!}
\end{aligned}
\end{equation}


- $(n_{i.}+n_{.j}-n)^+=\max\{1, n_{i.}+n_{.j}-n\}$
- $n_{i.}=\sum_{j=1}^Cn_{ij}$
- $n_{.j}=\sum_{i=1}^Rn_{ij}$


```Python
# Python实现
from sklearn.metrics import adjusted_mutual_info_score

## AMI
ami = adjusted_mutual_info_score(labels_true, labels_pred)
# labels_true：真实的分类标签
# label_pred：模型聚类的结果
# Python的sklearn中的AMI的公式为
# AMI(U,V)=[MI(U,V) - E(MI(U,V))] / [avg(H(U),H(V)) - E(MI(U,V))]
# 其中用的是avg而不是max

## NMI
from sklearn.metrics import normalized_mutual_info_score

nmi = normalized_mutual_info_score(labels_true, labels_pred)
```

## V-measure评价法
V-measure评价法：同质性（Homogeneity）和完整性（Completeness）的调和平均值。
- 需要真实值
- 最佳值为1
- 对簇结构不作假设，可以比较两种聚类算法的结果（比如k均值算法和谱聚类算法）
- 随机聚类的V-measure不会为零
- 当样本数较大（>1000） 或 聚类数较小（<10）时，可以忽略“V-measure不为零”的问题；当样本数较小或聚类数较大时，使用AMI或ARI会更可靠

### 同质性
同质性（Homogeneity）：每个簇(cluster)只包含单个类成员
- 在一个簇内包含的类别越少，则聚类效果越好
- 取值范围为 [0,1]


$$h=1-\frac{H(U|V)}{H(U)}$$
其中

$$H(U|V)=-\sum_{i=1}^R\sum_{j=1}^C\frac{n_{ij}}{n}\log{\left(\frac{n_{ij}}{n_{.j}}\right)}$$
$$H(U)=-\sum_{i=1}^R\frac{n_{i.}}{n}\log{\left(\frac{n_{i.}}{n}\right)}$$

### 完整性
完整性（Completeness）：给定类的所有成员分配给同一簇类
$$c=1-\frac{H(V|U)}{H(V)}$$

其中
$$H(V|U)=-\sum_{i=1}^R\sum_{j=1}^C\frac{n_{ij}}{n}\log{\left(\frac{n_{ij}}{n_{i.}}\right)}$$
$$H(V)=-\sum_{j=1}^C\frac{n_{.j}}{n}\log{\left(\frac{n_{.j}}{n}\right)}$$

### V-measure
V-measure是同质性和完整性的调和平均数，公式为
$$V=\frac{2\cdot h\cdot c}{h+c}$$

```Python
# Python实现 V-measure
from sklearn.metrics.cluster import v_measure_score

vs = v_measure_score(labels_true, labels_pred, beta=1.0)
# labels_true：真实的分类标签
# label_pred：模型聚类的结果
# v = (1 + beta) * homogeneity * completenes
#     / (beta * homogeneity + completeness)

# homogeneity
from sklearn.metrics.cluster import homogeneity_score

h = homogeneity_score(labels_true, labels_pred)

# completeness
from sklearn.metrics import completeness_score

c = completeness_score(labels_true, labels_pred)
```

## FMI评价法
**FMI评价法**（Fowlkes-Mallows score FMI）：精确率（Precision）和召回率（Recall）的几何平均值
- 需要真实值
- 最佳值为1
$$\mathrm{FMI}=\sqrt{\mathrm{Precision\cdot Recall}}=\frac{TP}{\sqrt{(TP+FP)(TP+FN)}}$$

```Python
# Python实现
from sklearn.metrics import fowlkes_mallows_score

fmi = fowlkes_mallows_score(labels_true, labels_pred)
```

## 轮廓系数
**轮廓系数**（Silhouette Coefficient）：适用于实际类别信息未知的情况。
- 轮廓系数的取值范围为 [-1,1]
- 同类别样本距离越相近且不同类别样本距离越远，值越高，表明聚类效果越好

计算单个样本的轮廓系数：
1. 计算样本$x_i$到同簇其他样本的平均距离$a_i$（$a_i$ 称为样本$x_i$的**簇内不相似度**）。 $a_i$ 越小，说明样本$x_i$ 越应该被聚类到该簇。
2. 计算样本$x_i$到其他簇$C_j$的所有样本的平均距离$b_{ij}$，称为样本$x_i$与簇$C_j$的不相似度。定义样本$x_i$的**簇间不相似度**为$b_i=\min\{b_{ij},j\neq i\}$。$b_i$越大，说明样本$x_i$越不属于其他簇。
3. 根据样本$x_i$的簇内不相似度$a_i$和簇间不相似度$b_i$，定义样本$x_i$的**轮廓系数**
 $$s_i=\frac{b_i-a_i}{\max\{a_i,b_i\}}=\left\{
 \begin{array}{ll}
  1-\frac{a_i}{b_i} & a_i<b_i\\
  0 & a_i=b_i\\
  \frac{b_i}{a_i}-1 & a_i>b_i
 \end{array}
 \right.$$
4. 判断：
 - $s_i$接近1，说明样本$x_i$聚类合理
 - $s_i$接近-1，说明样本$x_i$更应该分类到另外的簇
 - 若$s_i$约等于0，则说明样本$x_i$在两个簇的边界上

样本集合的轮廓系数是其所有样本轮廓系数的平均值。

```Python
# Python
from sklearn.metrics import silhouette_score

ss = silhouette_scor(X, labels, metric='euclidean')
```

## Calinski-Harabasz指数
**Calinski-Harabasz Index**（Variance Ratio Criterion）：用簇内的稠密程度和簇间的离散程度来评估聚类的效果。
- 主要用于k-均值聚类
- 类别内部数据的协方差越小越好，类别之间的协方差越大越好，则Calinski-Harabasz分数会越高，表明聚类效果越好

\begin{equation}
  \begin{aligned}
s(k) &= \frac{\mathrm{tr}(B_k)/(k-1)}{\mathrm{tr}(W_k)/(n-k)}= \frac{\mathrm{tr}(B_k)}{\mathrm{tr}(W_k)}\times\frac{n-k}{k-1}\\
&= \frac{SS_B/(k-1)}{SS_W/(n-k)}=\frac{SS_B}{SS_W}\times\frac{n-k}{k-1}
  \end{aligned}
\end{equation}
其中
- $B_k$是类别之间的协方差矩阵（$SS_B$ is the overall between-cluster variance）
$$SS_B=TSS-SS_W$$
 total sum of squares TSS
$$TSS=\sum_{i=1}^n(x_i-\bar{x})^2$$
- $W_k$是类别内部数据的协方差矩阵（$SS_W$ is the overall within-cluster variance）
- $\mathrm{tr}$ 是矩阵的迹

```Python
# Python实现
from sklearn.metrics import calinski-harabasz_score

ch = calinski_harabasz_score(X, labels)
```


# 参考资料
- [聚类模型评估](https://mp.weixin.qq.com/s?__biz=MzU5NzkxODMxOA==&mid=2247486297&idx=1&sn=0a2c549de4e1cfe54694e8364acfceb9&chksm=fe4d5c58c93ad54e7b45b5f644870f011f5e7ffc1d6cb0698f53d15d38e1419bc8cc7c0e1cd7&token=1168016353&lang=zh_CN#rd)
- [机器学习之模型评估详解](https://www.lagou.com/lgeduarticle/109119.html)
- [机器学习评价指标大汇总](https://zhaokv.com/machine_learning/2016/03/ml-metric.html)
- [ 维基百科-Rand Index](https://en.wikipedia.org/wiki/Rand_index#Adjusted_Rand_index)
- [维基百科-调整互信息](https://en.wikipedia.org/wiki/Adjusted_mutual_information)
- [兰德系数、调整兰德系数](https://blog.csdn.net/sinat_30203515/article/details/82634778)
- [方法名称最优值sklearn函数](https://www.wandouip.com/t5i267906/)
- [聚类模型评估](https://www.jianshu.com/p/b9528df2f57a)
- [聚类︱python实现六大分群质量评估指标（兰德系数、互信息、轮廓系数）](https://cloud.tencent.com/developer/article/1010857)
- [用scikit-learn学习K-Means聚类](https://www.cnblogs.com/pinard/p/6169370.html)
- [Calinski-Harabasz Index and Bootstrap Evaluation with Clustering Methods](https://ethen8181.github.io/machine-learning/clustering_old/clustering/clustering.html)
- [sklearn.metrics.adjusted_mutual_info_score](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.adjusted_mutual_info_score.html)
- [sklearn.metrics.adjusted_rand_score](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.adjusted_rand_score.html)
- [sklearn.metrics.v_measure_score](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.v_measure_score.html)
- [sklearn.metrics.homogeneity_score](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.homogeneity_score.html#sklearn.metrics.homogeneity_score)
- [sklearn.metrics.completeness_score](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.completeness_score.html)
- [sklearn.metrics.silhouette_score](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.silhouette_score.html)
- [sklearn.metrics.calinski_harabasz_score](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.calinski_harabasz_score.html)

# 推荐阅读
- {% post_link Machine-Learning-分类模型评估 分类模型评估 %}