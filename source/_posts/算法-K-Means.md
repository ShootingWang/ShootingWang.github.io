---
title: Machine Learning | K-Means
date: 2020-05-13 15:15:46
tags: [机器学习,聚类,算法]
#index_img: /img/random_forest.jpg
categories: 机器学习
mathjax: true
toc: true
hide: true
---

<center>K均值聚类</center>
<!--more-->

# K-Means
- 无监督学习
- $K$值无法自动获取
- 初始聚类中心随机选择
- 迭代（iterative）算法

{% note danger %}
## 算法
**输入**：
- 类的个数：$K$
- 训练集：$\{ x^{(1)}, x^{(2)}, \cdots, x^{(n)} \}$（其中，$x^{(i)}\in \mathbb{R}^{n}$）

**训练**：
1. 随机初始化$K$个簇（cluster）的质心（centroid）：$\mu_1, \mu_2, \cdots, \mu_K \in \mathbb{R}^n$；
2. 对于每个训练样本$x^{(i)}$（$i=1,2,\cdots,n$） ，用 $c^{(i)}$ 表示最接近 $x^{(i)}$ 的簇的质心（簇分配步骤）；其中，$c^{(i)} \in \{ 1,2,\cdots,K \}$
  $$c^{(i)} := \arg{\min_j || x^{(i)} - \mu_j ||}$$
3. 对于每个簇$j$（$j=1,2,\cdots,K$），更新簇的质心$\mu_j$（簇中所有点的坐标均值）
  $$\mu_j := \frac{\sum_{i=1}^n 1 \{ c^{(i)} = j \} x^{(i)} }{\sum_{i=1}^n 1 \{ c^{(i)} = j \} } $$
4. 重复步骤2和3，直到收敛

**输出**：

{% endnote %}

## 收敛 converge
记
$$\mu_{c^{(i)}} = \mbox{样本} x^{(i)} \mbox{被分配的簇的质心} $$

畸变函数/失真函数（distortion function）：
$$J(c, \mu)=\sum_{i=1}^n || x^{(i)} - \mu_{c^{(i)}} ||^2 $$

目标函数（optimization objective）/ 代价函数（cost function）为：
$$J \left( c^{(1)}, \cdots, c^{(n)}; \mu_1, \cdots, \mu_K \right) = \frac{1}{n}\sum_{i=1}^n || x^{(i)} - \mu_{c^{(i)}} ||^2 $$
$$\min_{ c^{(1)}, \cdots, c^{(n)} \atop \mu_1, \cdots, \mu_K } J \left( c^{(1)}, \cdots, c^{n}; \mu_1, \cdots, \mu_K \right) $$

K-means聚类的内循环（inner loop，步骤2和3）重复最小化$J$
- 固定$c$，$J$针对


# 确定K的方法
## 按需选择
根据建模的需求和目的来选择聚类的个数

## 观察法
直接看数据的散点图，看数据点大致聚成几堆
- 原始数据维数要低，一般是两维或三维，否则无法通过散点图观察
- 对于高维数据，可以先利用PCA（主成分分析法）降维，然后进行观察

## 肘部法
肘部法（Elbow Method），以聚类的数量为x轴，以WSS为y轴，绘制折线图，拐弯最明显的点对应的值就是比较合适的k值。
- x轴为聚类的数量
- y轴为WSS（within cluster sum of squares），即各个点到cluster（簇）中心的距离的平方和


## Gap Statistics方法



# 对比
## 优点
- 原理简单
- 容易实现
- 对大数据集有较高的学习效率，且具有可伸缩性

## 缺点
- 收敛速度较慢
- 算法的时间复杂度较高$O(nKt)$
 - $n$：样本点个数
 - $K$：聚类类别数
 - $t$：迭代次数
- 不能发现非凸形状的簇
- 对噪声和离群点敏感
- 结果不一定是全局最优，只能保证局部最优


## K-Means vs K-Medoids
- 二者的区别主要在于：质心的选择

||K-Means|K-Medoids|
|:---:|:-----:|:------:|
|质心的选择|样本点均值|从样本点中选取|
|计算质心的时间复杂度||$O(n^2)$<br>运行速度较慢|
|对噪声|敏感|相对较不敏感，比较稳健（robust）|

## K-Means vs KNN

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
      <th>K-Means</th>
      <th>KNN</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>分类</th>
      <td>聚类算法</td>
      <td>分类/回归算法</td>
    </tr>
    <tr>
      <th>监督</th>
      <td>无监督学习</td>
      <td>有监督学习</td>
    </tr>
    <tr>
      <th>训练集</th>
      <td>无标签数据</td>
      <td>有标签数据<br>无序变有序</td>
    </tr>
    <tr>
      <th>相似点</th>
      <td colspan="2">都包含——给定一个点，在数据集中找离它最近的点——的过程；即，二者都使用了NN（Nearest Neighbor）算法，一般用KD树实现NN</td>
    </tr>
    <tr>
  </tbody>
</table>
</div>

## K-Means vs DBSCAN


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
      <th>K-Means</th>
      <th>DBSCAN</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>聚类类型</th>
      <td>基于划分</td>
      <td>基于密度</td>
    </tr>
    <tr>
      <th rowspan="2">对象</th>
      <td colspan="2">都是将每个对象指派到单个簇的划分聚类算法</td>
    </tr>
    <tr>
      <td>一般聚类所有对象</td>
      <td>丢弃被它识别为噪声的对象</td>
    </tr>
    <tr>
      <th>簇</th>
      <td>很难处理非球形的簇和不同大小的簇；<br>可以发现不是明显分离的簇</td>
      <td>可以处理不同大小或形状的簇；<br>会合并有重叠的簇</td>
    </tr>
    <tr>
      <th>噪声/离群点</th>
      <td></td>
      <td>不太受噪声和离群点的影响</td>
    </tr>
    <tr>
      <th>时间复杂度</th>
      <td>$O(n)$</td>
      <td>$O(n^2)$</td>
    </tr>
    <tr>
      <th>结果可重复性</th>
      <td>通常随机初始化质心，不会产生相同结果</td>
      <td>多次运行产生相同的结果</td>
    </tr>
    <tr>
      <th>稀疏数据</th>
      <td>可用于稀疏的高维数据</td>
      <td>在稀疏的高维数据上的表现很差</td>
    </tr>
    <tr>
  </tbody>
</table>
</div>

其中：
- $n$是样本点个数
- 聚类可分为<u>基于划分、层次、密度、图形和模型</u>五大类

# 参考资料
- [机器学习：K-means和K-medoids对比](https://blog.csdn.net/databatman/article/details/50445561)
- [KNN与K-Means的区别](https://www.cnblogs.com/nucdy/p/6349172.html)
- [机器学习 之k-means和DBSCAN的区别](https://www.cnblogs.com/hugechuanqi/p/10509307.html)
- [CS229 Lecture Notes: K-Means Clustering Algorithm](http://cs229.stanford.edu/notes2020spring/cs229-notes7a.pdf)