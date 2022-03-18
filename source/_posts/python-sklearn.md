---
title: Python | sklearn
date: 2020-05-13 16:33:22
tags: [Python]
categories: Python
mathjax: true
toc: true
index_img: /img/Python.png  ## 封面图片
hide: true
notshow: true
---

<center>Scikit-learn</center>
<!--more-->

# Scikit-learn

> 常用使用手册：
> - [Scikit-Learn User Guide](https://scikit-learn.org/stable/user_guide.html)

# datasets

## fetch_<name>()
获取较大型的数据集

|函数名|详情|用途|
|:----|:-----|:----|
|`fetch_olivetti_faces`|Olivett脸部图片|分类|
|`fetch_20newsgroups`|20个新闻组文本（文本）|分类|
|`fetch_20newsgroups_vectorized`|20个新闻组文本（向量）|分类|
|`fetch_lfw_people`|LFW（人脸比对数据集）|分类|
|`fetch_lfw_pairs`|LFW（人脸匹配数据集）|分类|
|`fetch_covtype`|森林覆盖类型|分类|
|`fetch_rcv1`|RCV1新闻报导|分类|
|`fetch_kddcup99`|KDD竞赛在1999年举行比赛时采用的数据集|分类|
|`fetch_california_housing`|加利福尼亚州房价|回归|

## load_<name>()
`datasets`含有若干自带的小数据集，使用`load_<name>`可以载入名称为name的数据集
- 数据集列表及信息：[https://scikit-learn.org/stable/datasets/toy_dataset.html#toy-datasets](https://scikit-learn.org/stable/datasets/toy_dataset.html#toy-datasets)

|函数名|详情|用途|
|:----|:-----|:----|
|`load_boston`|[Boston房价数据](https://scikit-learn.org/stable/modules/generated/sklearn.datasets.load_boston.html#sklearn.datasets.load_boston)|回归|
|`load_iris`|经典的鸢尾花数据|分类|
|`load_diabetes`|糖尿病患者数据|回归|
|`load_digits`|手写数字数据|多分类|
|`load_linnerud`|||
|`load_wine`||分类|
|`load_breast_cancer`||分类|

```python
from sklearn.datasets import load_boston

X, y = load_boston(return_X_y=True)
print(X.shape)
# (506, 13)

from sklearn import datasets
import pandas as pd

dat = dataset.load_iris()  ## 载入鸢尾花数据集
print(" ".join(dat.keys()))     ## 数据集包含的信息项
print(dat['DESCR'])         ## 数据集描述信息
dat.sample(8)   ## 随机抽取8行数据
dat.describe()  ## 数据集特征描述信息
```

## make_blobs
生成多类单标签数据集，为每个类分配一个或多个正态分布的点集

## make_classification
生成多类单标签数据集，为每个类分配一个或多个正态分布的点集


# liner_model
## LinearRegression()
LinearRegression()：用于拟合线性回归模型。在sklearn库中的linear_model模块中。
- `.fit()`：拟合模型
- `.predict()`：预测结果
- `.coef_[0]`：一元线性回归中，特征变量的参数估计值
- `.coef_[:]` ：多元线性回归中，特征变量的参数估计值
- `.intercept_` ：一元线性回归中，截距项的估计值
- `.score()`：看测试集的表现得分

```python
###导入线性回归模型
import numpy as np
import matplotlib.pyplot as plt
from sklearn.linear_model import LinearRegression

%matplotlib inline

X = [[1], [4]]   ###两个点的横坐标
y = [3, 5]   ###两个点的纵坐标

lr = LinearRegression().fit(X, y)
z = np.linspace(0, 5, 20)
plt.scatter(X, y, s= 50)   ###参数s设置点的大小
plt.plot(z, lr.predict(z.reshape(-1, 1)), c='green')
plt.title("Straight Line")
plt.show()
```

<meta name="referrer" content="no-referrer" />
{% asset_img output_0_0.png %}

```python
print("y = {:.4f}".format(lr.coef_[0]),"x","+{:.4f}".format(lr.intercept_))
```

    y = 0.6667 x +2.3333

考虑多个数据点的线性回归：

```python
import numpy as np
import matplotlib.pyplot as plt

###首先生成数据集
from sklearn.datasets import make_regression

X ,y = make_regression(n_samples=100, n_features=1, 
                      n_informative=1, noise=50, random_state=1)

from sklearn.linear_model import LinearRegression
###拟合
reg = LinearRegression()
reg.fit(X, y)
z = np.linspace(-3, 3, 200).reshape(-1, 1)

plt.scatter(X, y, c='blue', s=50)
plt.plot(z, reg.predict(z), c='k')
plt.title("Linear Regression")
plt.show()
```

<meta name="referrer" content="no-referrer" />
{% asset_img output_2_0.png %}


糖尿病数据，线性回归：

```python
###加载糖尿病数据集
from sklearn.datasets import load_diabetes

X, y = load_diabetes().data, load_diabetes().target

from sklearn.model_selection import train_test_split
X_train, X_test, y_train, y_test = train_test_split(X, y, random_state=8)

from sklearn.linear_model import LinearRegression
lr = LinearRegression().fit(X_train, y_train)

print("训练数据集得分：{:.3f}".format(lr.score(X_train, y_train)))
print("测试数据集得分：{:.3f}".format(lr.score(X_test, y_test)))
```

    训练数据集得分：0.530
    测试数据集得分：0.459
    

- **损失函数**（Loss Function），也称**成本函数**（Cost Function），用于定义模型预测值与观测值的误差。
- **残差**（residuals）/**训练误差**（training errors）：模型预测值与训练集数据的差异。
- **预测误差**（prediction errors）/**测试误差**（test errors）：模型预测值与测试集数据的差异。

考虑带损失函数的模型拟合结果评估（标注残差、计算残差平方和）：

```python
X = [[6], [8], [10], [14], [18]]
y = [[7], [9], [13], [17.5], [18]]

from sklearn.linear_model import LinearRegression
model = LinearRegression()
model.fit(X, y)
X2 = [[0], [10], [14], [25]]
y2 = model.predict(X2)

###残差
yr = model.predict(X)


%matplotlib inline
import matplotlib.pyplot as plt

plt.plot(X, y, 'k.')
plt.plot(X2, y2, 'g-')
for idx, x in enumerate(X):
    plt.plot([x, x], [y[idx], yr[idx]], 'r-')

plt.xlabel("X")
plt.ylabel("y")
plt.axis([0, 25, 0, 25])
plt.grid(True)
plt.show()
```

<meta name="referrer" content="no-referrer" />
{% asset_img output_4_0.png %}


# preprocessing
sklearn模块中的preprocessing包含许多用于encoding的函数。

## LabelBinarizer()
使用One-Hot Encoding将类别数据二进制化

```Python
from sklearn import preprocessing
lb = preprocessing.LabelBinarizer()

c_list = ['Quanzhou', 'Xiamen', 'Zhangzhou', 'Quanzhou']

lb.fit(c_list)
lb.classes_
#array(['Quanzhou', 'Xiamen', 'Zhangzhou'], dtype='<U9')

c_list_lb = lb.transform(c_list)  ##Encoding
c_list_lb
#array([[1, 0, 0],
#       [0, 1, 0],
#       [0, 0, 1],
#       [1, 0, 0]])

c_list_new = lb.inverse_transform(c_list_lb)  ##Decoding
c_list_new
#array(['Quanzhou', 'Xiamen', 'Zhangzhou', 'Quanzhou'], dtype='<U9')
```


## LabelEncoder()
进行Label encoding。

```Python
from sklearn import preprocessing
le = preprocessing.LabelEncoder()
le.fit(["paris", "paris", "tokyo", "amsterdam"])  ##进行编码
# LabelEncoder()

list(le.classes_)  ##编码类别；paris重复；
# ['amsterdam', 'paris', 'tokyo']

le.transform(["tokyo", "tokyo", "paris"])
# array([2, 2, 1], dtype=int64)
```

```Python
from sklearn.preprocessing import LabelEncoder
from collections import defaultdict
import pandas as pd

d = defaultdict(LabelEncoder)

df = pd.DataFrame({
    'pets':['cat', 'dog', 'cat', 'monkey', 'dog', 'cat'],
    'owner':['Champ', 'Ron', 'Jane', 'Champ', 'Veronica', 'Ron'],
    'location':['San_Diego', 'New_York', 'New_York', 'San_Diego', 'San_Diego', 'New_York']
})
df
#     pets     owner   location
#0     cat     Champ  San_Diego
#1     dog       Ron   New_York
#2     cat      Jane   New_York
#3  monkey     Champ  San_Diego
#4     dog  Veronica  San_Diego
#5     cat       Ron   New_York

## Encoding
fit = df.apply(lambda x: d[x.name].fit_transform(x))  ##按列操作
fit
#   pets  owner  location
#0     0      0         1
#1     1      2         0
#2     0      1         0
#3     2      0         1
#4     1      3         1
#5     0      2         0

## Decoding 
dec = fit.apply(lambda x: d[x.name].inverse_transform(x))
dec
#     pets     owner   location
#0     cat     Champ  San_Diego
#1     dog       Ron   New_York
#2     cat      Jane   New_York
#3  monkey     Champ  San_Diego
#4     dog  Veronica  San_Diego
#5     cat       Ron   New_York

### Using the dictionary to label future data
df.apply(lambda x: d[x.name].transform(x))
```

### inverse_transform()
对LabelEncoder()进行解码

```Python
##解码decode
list(le.inverse_transform([2, 2, 1]))
# ['tokyo', 'tokyo', 'paris']
```

## OneHotEncoder()
One-Hot Encoding（独热编码）
`OneHotEncoder(n_values=None, categorical_features=None, 
    categories=None, sparse=True, dtype=<class 'numpy.float64'>, 
    handle_unknown='error')`

```Python
from sklearn import preprocessing
ohe = preprocessing.OneHotEncoder()
ohe.fit([[0, 0, 3],
        [1, 1, 0],
        [0, 2, 1],
        [1, 0, 2]])  ##学习Encoding
ohe.transform([[0, 1, 3]]).toarray()  ## Encoding
#array([[1., 0., 0., 1., 0., 0., 0., 0., 1.]])
```
观察学习的4×3数据矩阵，共有4个数据，3个特征。
- 第一个特征维度取值仅{0,1}，对应二维One-Hot编码向量{(1,0) , (0,1)}；
- 第二个特征维度取值为{0,1,2}，对应三维One-Hot编码向量{(1,0,0) , (0,1,0) , (0,0,1)}；
- 第三个特征维度取值为{0,1,2,3}，对应四维One-Hot编码向量{(1,0,0,0) , (0,1,0,0) , (0,0,1,0) , (0,0,0,1)}。

因此，关于(0,1,3)的编码结果应该是{(1,0) , (0,1,0) , (0,0,0,1)}，所以最后输出结果为 [1,0,0,1,0,0,0,0,1]。


```Python
from sklearn import preprocessing
import numpy as np

oh = preprocessing.OneHotEncoder(sparse=False)
data = ([1, 3, 2],
       [2, 1, 1],
       [4, 2, 2])
oh.fit_transform(data)
#array([[1., 0., 0., 0., 0., 1., 0., 1.],
#       [0., 1., 0., 1., 0., 0., 1., 0.],
#       [0., 0., 1., 0., 1., 0., 0., 1.]])
```




```Python

```


```Python

```


```Python

```

```Python

```

# 参考资料
- [Sklearn学习：线性回归、岭回归、Lasso、Elastic Net](https://mp.weixin.qq.com/s?__biz=MzU5NzkxODMxOA==&mid=2247484190&idx=1&sn=e7484e733ae84785a8940270cdf289b1&chksm=fe4d541fc93add09a7d2730ec0ec9aeb04bc1d1fe38e9c662087e368b20bb20c087611191956&token=1683818096&lang=zh_CN#rd)