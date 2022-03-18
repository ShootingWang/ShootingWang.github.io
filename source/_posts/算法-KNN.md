---
title: Machine Learning | KNN
date: 2020-05-06 00:10:47
tags: [机器学习,分类,生成式模型,算法]
#index_img: /img/random_forest.jpg
categories: 机器学习
mathjax: true
toc: true
hide: true
---

<center>K最近邻算法</center>
<!--more-->

KNN

K-nearest neighbours

# KNN
**k最邻近算法**（kNN, k-Nearest Neighbors Algorithm），是一种<u>lazy learning</u>（消极学习法）
- 不需要使用训练集进行训练，训练的时间复杂度为0
- kNN分类的计算复杂度和训练集中的样本数目成正比
- kNN是一种可用于分类和回归的方法

> In machine learning, **lazy learning** is a learning method in which generalization of the training data is, in theory, delayed until a query is made to the system, as opposed to in eager learning, where the system tries to generalize the training data before receiving queries.
**维基百科：Lazy Learning**

> In AI, **eager learning** is a learning method in which the system tries to construct a general, input-independent target function during training of the system, as opposed to lazy learning, where generalization beyond the training data is delayed until a query is made to the system.
**维基百科：Eager Learning**

## 原理
假设有$m$类不同的样本数据，要判断某个待分类点A属于哪个类别，计算这个待分类点A与其他所有已知分类的样本点的距离（一般是计算<u>欧式距离</u>或<u>曼哈顿距离</u>）；距离点A最近的k个样本点中，属于同一类别的样本点数最多的那个类别记为 $i$ ，则待分类点A从属于类别 $i$ 。

考虑点$(x_i,y_i)$和$(x_j,y_j)$
- **欧氏距离**
 $$d=\sqrt{(x_i-x_j)^2+(y_i-y_j)^2}$$
- **曼哈顿距离**
 $$d=|x_i-x_j|+|y_i-y_j|$$

### K
- k值的选取非常重要
 - 如果k值太小，则该分类器容易受到训练数据的噪声而产生过拟合影响
 - 如果k值太大，则分类器容易欠拟合；最邻近列表中可能包含远离其近邻的数据点
- k值一般选取1，3，5，7等较小的值
- 可以通过交叉验证来选取最优的k值

{% note default %}
在large-scale且sparse的数据分析中，KNN的k个最近邻应该如何选择？
A. 随机选择
B. L1-norm最近的
C. L2-norm最近的
D.不用KNN

答案：D. 不用KNN

- KNN需要存储所有的样本
- KNN需要进行繁重的距离计算量

所以不适合大规模的稀疏的数据
[题目来源](https://www.nowcoder.com/questionTerminal/e744eccb59194771916a15b772851d73?source=relative)
{% endnote %}


## 用途
- 分类
- 回归

应用：
- 创建推荐系统
- 光学字符识别（OCR，Optical Character Recognition）：自动识别图片中的文字
 - OCR算法提取线段、点和曲线等特征
- 创建垃圾邮件（spam）分类器

## 总结
适用数据：
- 数值型
- 标称型

### 优点
- 精度高
- 对异常值不敏感
- 无数据输入假定

### 缺点
- 时间复杂度高
- 空间复杂度高





## 实现

### Python
使用Python的`scikit-learn`库。
scikit-learn库中有若干个数据集Toy datasets。

#### 二分类

```Python
##导入数据集生成器
from sklearn.datasets import make_blobs
##导入kNN分类器
from sklearn.neighbors import KNeighborsClassifier

##导入绘图工具
import matplotlib.pyplot as plt
###为了在Jupyter Notebook显示图片，加入以下代码行
%matplotlib inline

##导入数据集拆分工具
from sklearn.model_selection import train_test_split

##生成样本数n=200，分类为2的数据集
data = make_blobs(n_samples=200, centers=2, random_state=8)
X, y = data

##可视化
plt.scatter(X[:, 0], X[:, 1], c=y, cmap=plt.cm.spring, edgecolors='k')
plt.show()
```

补充图片

```Python
## 用kNN进行二分类

import numpy as np
kclassifier = KNeighborsClassifier()
kclassifier.fit(X, y)
%matplotlib inline
##横纵轴坐标范围
x_min, x_max = X[:, 0].min() - 1, X[:, 0].max() + 1
y_min, y_max = X[:, 1].min() - 1, X[:, 1].max() + 1
xx, yy = np.meshgrid(np.arange(x_min, x_max, .02),
                    np.arange(y_min, y_max, .02))
Z = kclassifier.predict(np.c_[xx.ravel(), yy.ravel()])
Z = Z.reshape(xx.shape)
plt.pcolormesh(xx, yy, Z, cmap=plt.cm.Pastel1)
plt.scatter(X[:, 0], X[:, 1], c=y, cmap=plt.cm.spring, edgecolors='k')
plt.xlim(xx.min(), xx.max())
plt.ylim(yy.min(), yy.max())
plt.title("Classifier:kNN")

plt.show()
```

补充图片

```Python
##对新数据点分类进行判断
print("新数据点的分类是：", kclassifier.predict([[6.75, 4.82]]))
#新数据点的分类是： [1]
```

#### 多分类


```Python
from sklearn.datasets import make_blobs
from sklearn.neighbors import KNeighborsClassifier
import matplotlib.pyplot as plt

##生成样本数为500，分类数为5的数据集
data2 = make_blobs(n_samples=500, centers=5, random_state=8)
X2, y2 = data2

%matplotlib inline

plt.scatter(X2[:, 0], X2[:,1], c=y2, cmap=plt.cm.spring, edgecolors='k')
plt.show()
```

补充图片

```Python
clf = KNeighborsClassifier()
clf.fit(X2, y2)

x_min, x_max = X2[:, 0].min() - 1, X2[:, 0].max() + 1
y_min, y_max = X2[:, 1].min() - 1, X2[:, 1].max() + 1

import numpy as np
%matplotlib inline

xx, yy = np.meshgrid(np.arange(x_min, x_max, .02),
                    np.arange(y_min, y_max, .02))
Z = clf.predict(np.c_[xx.ravel(), yy.ravel()])
Z = Z.reshape(xx.shape)

plt.pcolormesh(xx, yy, Z, cmap = plt.cm.Pastel1)
plt.scatter(X2[:, 0], X2[:, 1], c=y2, cmap=plt.cm.spring, edgecolors='k')
plt.xlim(xx.min(), xx.max())
plt.ylim(yy.min(), yy.max())
plt.title("Classifier:kNN")
plt.show()
```

补充图片

```Python
##模型的评分
print("模型正确率：{:.2f}".format(clf.score(X2, y2)))
#模型正确率：0.96
```

#### 回归
kNN的n_neighbors默认为5。

```Python
##导入数据生成器make_regression
from sklearn.datasets import make_regression

##生成特征数量为1，噪音为50的数据集
X, y = make_regression(n_features=1, n_informative=1, noise=50, random_state=8)

##可视化
%matplotlib inline
plt.scatter(X, y, c='orange', edgecolors='k')
plt.show()
```

补充图片


```Python
##导入用于回归分析的kNN模型
from sklearn.neighbors import KNeighborsRegressor
reg = KNeighborsRegressor()

##用kNN模型拟合数据
reg.fit(X, y)

##可视化预测结果
import numpy as np
z = np.linspace(-3, 3, 200).reshape(-1, 1)
%matplotlib inline
plt.scatter(X, y, c='orange', edgecolors='k')
plt.plot(z, reg.predict(z), c='k', linewidth=3)
plt.title("kNN Regressor")
plt.show()
```

补充图片

```Python
##模型评分
print("模型评分：{:.2f}".format(reg.score(X, y)))
#模型评分：0.77
```

修改模型的`n_estimators`参数

```Python
reg2 = KNeighborsRegressor(n_neighbors=2)
reg2.fit(X, y)

%matplotlib inline
plt.scatter(X, y, c='orange', edgecolors='k')
plt.plot(z, reg2.predict(z), c='k', linewidth=3)
plt.title("kNN Regressor:n_neighbors=2")
plt.show()
```

补充图片


```Python
print("模型评分为：{:.2f}".format(reg2.score(X, y)))
#模型评分为：0.86
```

模型评分从0.77提升到0.86，有显著的提升。



## 实例
### 酒的分类


```Python
##导入数据集
from sklearn.datasets import load_wine
wine_dataset = load_wine()
###使用load_wine()导入的酒数据集，是一种Bunch对象，包括键（keys）和数值（values）

###查看该数据集里的键
print("红酒数据集中的键：\n{}".format(wine_dataset.keys()))
```

     红酒数据集中的键：
     dict_keys(['data', 'target', 'target_names', 'DESCR', 'feature_names'])

数据集中包括数据data、目标分类target、目标分类名称target_names、数据描述DESCR和特征变量的名称features_names。

下面查看数据的样本量和变量数目：

```Python
print("数据概况：{}".format(wine_dataset['data'].shape))
#数据概况：(178, 13)
```

共有178个样本，每个样本有13个特征变量。

下面查看该数据集的描述（超长）：

```Python
print(wine_dataset['DESCR'])
```
     .. _wine_dataset:
      
     Wine recognition dataset
     ------------------------

     **Data Set Characteristics:**

     :Number of Instances: 178 (50 in each of three classes)
     :Number of Attributes: 13 numeric, predictive attributes and the class
     :Attribute Information:
         - Alcohol
         - Malic acid
         - Ash
        - Alcalinity of ash  
         - Magnesium
        - Total phenols
         - Flavanoids
         - Nonflavanoid phenols
         - Proanthocyanins
        - Color intensity
         - Hue
         - OD280/OD315 of diluted wines
         - Proline

     - class:
            - class_0
            - class_1
            - class_2

     :Summary Statistics:

     ============================= ==== ===== ======= =====
                                   Min   Max   Mean     SD
     ============================= ==== ===== ======= =====
     Alcohol:                      11.0  14.8    13.0   0.8
     Malic Acid:                   0.74  5.80    2.34  1.12
     Ash:                          1.36  3.23    2.36  0.27
     Alcalinity of Ash:            10.6  30.0    19.5   3.3
     Magnesium:                    70.0 162.0    99.7  14.3
     Total Phenols:                0.98  3.88    2.29  0.63
     Flavanoids:                   0.34  5.08    2.03  1.00
     Nonflavanoid Phenols:         0.13  0.66    0.36  0.12
     Proanthocyanins:              0.41  3.58    1.59  0.57
     Colour Intensity:              1.3  13.0     5.1   2.3
     Hue:                          0.48  1.71    0.96  0.23
     OD280/OD315 of diluted wines: 1.27  4.00    2.61  0.71
     Proline:                       278  1680     746   315
     ============================= ==== ===== ======= =====

     :Missing Attribute Values: None
     :Class Distribution: class_0 (59), class_1 (71), class_2 (48)
     :Creator: R.A. Fisher
     :Donor: Michael Marshall (MARSHALL%PLU@io.arc.nasa.gov)
     :Date: July, 1988

     This is a copy of UCI ML Wine recognition datasets.
     https://archive.ics.uci.edu/ml/machine-learning-databases/wine/wine.data

     The data is the results of a chemical analysis of wines grown in the same
     region in Italy by three different cultivators. There are thirteen different
     measurements taken for different constituents found in the three types of
     wine.

     Original Owners: 

     Forina, M. et al, PARVUS - 
     An Extendible Package for Data Exploration, Classification and Correlation. 
     Institute of Pharmaceutical and Food Analysis and Technologies,
     Via Brigata Salerno, 16147 Genoa, Italy.

     Citation:

     Lichman, M. (2013). UCI Machine Learning Repository
     [http://archive.ics.uci.edu/ml]. Irvine, CA: University of California,
     School of Information and Computer Science. 

     .. topic:: References

     (1) S. Aeberhard, D. Coomans and O. de Vel, 
     Comparison of Classifiers in High Dimensional Settings, 
     Tech. Rep. no. 92-02, (1992), Dept. of Computer Science and Dept. of  
     Mathematics and Statistics, James Cook University of North Queensland. 
     (Also submitted to Technometrics). 

      The data was used with many others for comparing various 
     classifiers. The classes are separable, though only RDA 
     has achieved 100% correct classification. 
     (RDA : 100%, QDA 99.4%, LDA 98.9%, 1NN 96.1% (z-transformed data)) 
     (All results using the leave-one-out technique) 

     (2) S. Aeberhard, D. Coomans and O. de Vel, 
     "THE CLASSIFICATION PERFORMANCE OF RDA" 
     Tech. Rep. no. 92-01, (1992), Dept. of Computer Science and Dept. of 
     Mathematics and Statistics, James Cook University of North Queensland. 
     (Also submitted to Journal of Chemometrics).


划分训练集和测试集（使用scikit-learn中的train_test_split函数）：

```Python
##导入数据拆分工具
from sklearn.model_selection import train_test_split

X_train, X_test, y_train, y_test = train_test_split(wine_dataset['data'],
                                                   wine_dataset['target'],
                                                   random_state=0)
###这里random_state即设置seed，固定seed则能保证每次产生的伪随机数不变；
###若random_state设置为0或者缺省时，则每次生成的伪随机数不同。

print("X_train shape:{}".format(X_train.shape))
print("X_test shape:{}".format(X_test.shape))
print("y_train shape:{}".format(y_train.shape))
print("y_test shape:{}".format(y_test.shape))
```
     X_train shape:(133, 13)
     X_test shape:(45, 13)
     y_train shape:(133,)
     y_test shape:(45,)

训练集样本数为133个，测试集样本数为45个。

```Python
##导入kNN分类器
from sklearn.neighbors import KNeighborsClassifier

##指定模型的n_neighbors参数为1
knn = KNeighborsClassifier(n_neighbors=1)

knn.fit(X_train, y_train)
print(knn)
```

score()：计算模型的得分（得分越高，则表示吻合度越高，模型表现越好）：

```Python
knn.score(X_test, y_test)
```
     0.7555555555555555

模型对新的样本数据做出正确分类预测的概率为76%。

对新的红酒分类：

```Python
import numpy as np
X_new = np.array([[13.2, 2.77, 2.51, 18.5, 96.6, 1.04,2.55, 0.57, 1.47, 6.2, 1.05, 3.33, 820]])

##对新的数据点进行预测
prediction = knn.predict(X_new)
print("预测新红酒的分类为：{}".format(wine_dataset['target_names'][prediction]))
```
     预测新红酒的分类为：['class_2']



```Python

```

## KNN vs K-Means
- {% post_link 算法-K-Means K-Means算法 %}

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

# kNN图
k-Nearest Neighbour Graph

假设空间中有$n$个节点，对任一节点$v_i$，找出距离[^1]它最近的$k$个邻点$v_1,\cdots,v_k$，分别将$v_i$与这$k$个邻点连结起来，形成$k$条有向边。

对空间中的所有顶点都按此方法进行，最后得到的图便是kNN图。

[^1]: 欧氏距离、马氏距离、……

- 可用于**异常点检测**
> 在大量高维数据点中，一般正常的数据点会聚集成一个个簇，而异常数据点与正常数据点的簇距离较远

## 构建kNN图
1. 空间分割树算法（Space-Partitioning Trees）
2. 局部敏感哈希算法（Locality Sensitive Hashing）
3. 邻居搜索算法（Neighbor Exploring Techniques)





# 参考资料
- [图解算法](https://book.douban.com/subject/26979890/)
- [维基百科-Lazy Learning](https://en.wikipedia.org/wiki/Lazy_learning)
- [维基百科-Eager Learning](https://en.wikipedia.org/wiki/Eager_learning)
- 段小手.[深入浅出 Python 机器学习](https://baike.baidu.com/item/%E6%B7%B1%E5%85%A5%E6%B5%85%E5%87%BAPython%E6%9C%BA%E5%99%A8%E5%AD%A6%E4%B9%A0/22799668?fr=aladdin)[M]. 清华大学出版社, 2018.
- [机器学习算法-K最近邻从原理到实现（Python）](https://blog.csdn.net/Dream_angel_Z/article/details/45896449)
- [机器学习（一）——K-近邻（KNN）算法](https://www.cnblogs.com/ybjourney/p/4702562.html)
- [从SNE到t-SNE再到LargeVis](http://bindog.github.io/blog/2016/06/04/from-sne-to-tsne-to-largevis/)