---
title: Machine Learning | 汇总
date: 2020-05-02 11:00:00
tags: [算法,机器学习]
categories: 
  - 机器学习
math: true
mathjax: true
index_img: /img/ml.jpg
top: 10 ##true ## 置顶
---

<center>Machine Learning 算法汇总</center>
<!--more-->


# 基础
- {% post_link Machine-Learning-分类模型评估 分类模型评估 %}
- {% post_link Machine-Learning-聚类模型评估 聚类模型评估%}
- {% post_link 算法-过拟合和欠拟合 过拟合和欠拟合 %}
- {% post_link Machine-Learning-熵 熵 %}
- {% post_link 算法-特征归一化 归一化、特征缩放 %}



# Machine Learning 机器学习
- Classical Learning
 - Supervised Learning
 - Unsupervised Learning
- Ensemble Methods
 - Bagging
 - Boosting
 - Stacking
- Reinforcement Learning
- Neural Networks and Deep Learning

## Classical Learning 分类
- {% post_link 算法-SupervisedLearning Supervised Learning %}
 - Classification
 - Regression
- Unsupervised Learning
 - Clustering
 - Pattern Search
 - Dimension Reduction

### Supervised Learning 有监督学习
#### Classification
主要分类算法：
- {% post_link 算法-KNN KNN %}
- {% post_link 算法-朴素贝叶斯 Naive Bayes 朴素贝叶斯 %}
- {% post_link 算法-SVM SVM 支持向量机 %}
- {% post_link 算法-DecisionTree Decision Tree 决策树 %}
- {% post_link 算法-LogisticRegression Logistic Regression 逻辑回归 %}

相关文章：
- {% post_link Machine-Learning-分类模型评估 分类模型评估 %}

从**使用的主要技术**上看，可以把分类方法归结为4种类型：
1. 基于距离的分类方法
   - 最邻近方法
2. 决策树分类方法
   - ID3
   - C4.5
   - VFDT
3. 贝叶斯分类方法
   - 朴素贝叶斯
   - EM算法
4. 规则归纳方法
   - AQ算法
   - CN2算法
   - FOIL算法



#### Regression
主要回归算法：
- {% post_link 算法-线性回归 Linear Regression 线性回归 %}
- Ridge Regression 岭回归
- Lasso Regression


### Unsupervised Learning
#### Clustering
- 分割聚类算法：
 - {% post_link 算法-K-Means K-Means K均值聚类 %}
 - K-Medoids
 - {% post_link 算法-CLARANS CLARANS %}
- 层次聚类算法：
 - {% post_link 算法-DBSCAN DBSCAN %}
 - OPTICS
 - {% post_link 算法-BIRCH BIRCH %}
 - CURE
- Mean-Shift
- Agglomerative
- Fuzzy C-Means

相关文章：
- {% post_link Machine-Learning-聚类模型评估 聚类模型评估%}
- {% post_link  %}

<u>表：各种聚类算法对比</u>
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
      <th>算法</th>
      <th>算法效率</th>
      <th>适合的数据类型</th>
      <th>能够发现的聚类类型</th>
      <th>对异常值的敏感性</th>
      <th>对数据输入顺序的敏感性</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>CLARANS</th>
      <td>较低</td>
      <td>数值</td>
      <td>凸形/球形</td>
      <td>不敏感</td>
      <td>非常敏感</td>
    </tr>
    <tr>
      <th>BIRCH</th>
      <td>高</td>
      <td>数值</td>
      <td>凸形/球形</td>
      <td>不敏感</td>
      <td>不太敏感</td>
    </tr>
    <tr>
      <th>DBSCAN</th>
      <td>一般</td>
      <td>数值</td>
      <td>任意形状</td>
      <td>敏感</td>
      <td>敏感</td>
    </tr>
    <tr>
      <th>CURE</th>
      <td>较高</td>
      <td>数值</td>
      <td>任意形状</td>
      <td>不敏感</td>
      <td>不太敏感</td>
    </tr>
    <tr>
      <th>K-poto</th>
      <td>一般</td>
      <td>数值/符号</td>
      <td>凸形/球形</td>
      <td>敏感</td>
      <td>一般</td>
    </tr>
    <tr>
      <th>CUQUE</th>
      <td>较低</td>
      <td>数值</td>
      <td>凸形/球形</td>
      <td>一般</td>
      <td>不敏感</td>
    </tr>
  </tbody>
</table>
</div>


#### Pattern Search
- Euclat
- {% post_link 算法-Apriori Apriori %}
- FP-Growth


#### Dimension Reduction
- {% post_link 算法-t-SNE t-SNE %}
- PCA
- LSA
- SVD
- LDA

## Ensemble Methods
{% post_link 算法-集成方法 集成方法 %}

- Bagging
- Boosting
- Stacking


### Bagging
- {% post_link 算法-随机森林 Random Forest 随机森林 %}


### Boosting
- {% post_link 算法-AdaBoost AdaBoost %}
- {% post_link 算法-XGBoost XGBoost %}
- LightGBM
- CatBoost
- {% post_link 算法-BoostingTree Boosting Tree 提升树 %}

{% note default %}
Boosting is a method of converting a set of weak learners into strong learners.
{% endnote %}

Boosting是一种将弱学习器转化成强学习器的方法。

假设进行的是二分类任务：
- 弱学习器的分类错误率仅略小于0.5（分类效果只比扔硬币好一点）
- 强学习器的分类错误率接近0
将弱学习器转化为强学习器——将多个弱学习器[^1]联合起来，通过投票得到最后分类结果

[^1]: 这里的弱学习器之间相关性要小

{% note primary %}
为实现弱学习器互补，则需要解决两个问题：
1. 怎样获得不同的弱学习器？
2. 怎样组合弱学习器？
{% endnote %}


### Stacking


## Reinforcement Learning
- Genetic Algorithm
- A3C
- SARSA
- Q-Learning
- DQN


## Neural Nets and Deep Learning
- CNN
- RNN
- GAN
- Autoencoders
- MLP

其他:
- {% post_link 算法-感知机 感知机Perceptron %}
- {% post_link 算法-神经网络 神经网络的激活函数 %}





### CNN
### RNN
- LSM
- LSTM
- GRU

#### LSM
#### LSTM
#### GRU
- GAN
- Autoencoders
- Perceptrons (MLP)

### GAN
Generative Adversarial Nerworks
### Autoencoders
seq2seq

### Perceptrons (MLP)




# 参考资料
- [首页缩略图](https://cn.bing.com/images/search?view=detailV2&ccid=k5LiTi5l&id=95DF77DEC29B1CD7083F9435F1AE55043B0C0081&thid=OIP.k5LiTi5lt_ND02kaBT_SAAHaE7&mediaurl=https%3a%2f%2fwww.smartdatacollective.com%2fwp-content%2fuploads%2f2018%2f11%2fMachine-learning-1024x682.jpg&exph=682&expw=1024&q=machine+learning&simid=608018054240865990&selectedIndex=3)
- [聚类分析中几种算法的比较](http://cda.pinggu.org/view/20203.html)