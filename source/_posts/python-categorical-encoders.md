---
title: Python | Categorical_encoders
date: 2020-06-12 17:07:59
tags: [Python]
categories: Python
mathjax: true
toc: true
index_img: /img/Python.png  ## 封面图片
hide: true
notshow: true
---

<center> </center>
<!--more-->

# Categorical_encoders
categorical_encoders包有多种不同的编码技术可以把类别变量转换为数值型变量。

使用Anaconda Prompt进行**安装**：

```shell
pip install categorical_encoders
```

## BinaryEncoder()
进行0-1转换

```Python
import category_encoders as ce
from sklearn.datasets import load_boston

bunch = load_boston()
y = bunch.target
X = pd.DataFrame(bunch.data, columns=bunch.feature_names)
X.head()
#      CRIM    ZN  INDUS  CHAS    NOX  ...  RAD    TAX  PTRATIO       B  LSTAT
#0  0.00632  18.0   2.31   0.0  0.538  ...  1.0  296.0     15.3  396.90   4.98
#1  0.02731   0.0   7.07   0.0  0.469  ...  2.0  242.0     17.8  396.90   9.14
#2  0.02729   0.0   7.07   0.0  0.469  ...  2.0  242.0     17.8  392.83   4.03
#3  0.03237   0.0   2.18   0.0  0.458  ...  3.0  222.0     18.7  394.63   2.94
#4  0.06905   0.0   2.18   0.0  0.458  ...  3.0  222.0     18.7  396.90   5.33
#
#[5 rows x 13 columns]

ohe = ce.BinaryEncoder(cols=['CHAS', 'RAD'], handle_unknown='indicator').fit(X, y)
numeric_dataset = ohe.transform(X)
numeric_dataset.info()
#<class 'pandas.core.frame.DataFrame'>
#RangeIndex: 506 entries, 0 to 505
#Data columns (total 19 columns):
#CRIM       506 non-null float64
#ZN         506 non-null float64
#INDUS      506 non-null float64
#CHAS_0     506 non-null int64
#CHAS_1     506 non-null int64
#CHAS_2     506 non-null int64
#NOX        506 non-null float64
#RM         506 non-null float64
#AGE        506 non-null float64
#DIS        506 non-null float64
#RAD_0      506 non-null int64
#RAD_1      506 non-null int64
#RAD_2      506 non-null int64
#RAD_3      506 non-null int64
#RAD_4      506 non-null int64
#TAX        506 non-null float64
#PTRATIO    506 non-null float64
#B          506 non-null float64
#LSTAT      506 non-null float64
#dtypes: float64(11), int64(8)
#memory usage: 75.2 KB

numeric_dataset[['CHAS_0', 'CHAS_1', 'CHAS_2']].head()
#   CHAS_0  CHAS_1  CHAS_2
#0       0       0       1
#1       0       0       1
#2       0       0       1
#3       0       0       1
#4       0       0       1

numeric_dataset[['RAD_0', 'RAD_1', 'RAD_2', 'RAD_3', 'RAD_4']].head()
#   RAD_0  RAD_1  RAD_2  RAD_3  RAD_4
#0      0      0      0      0      1
#1      0      0      0      1      0
#2      0      0      0      1      0
#3      0      0      0      1      1
#4      0      0      0      1      1
```


## OneHotEncoder()
进行One-Hot Encoding
`OneHotEncoder(verbose=0, cols=None, drop_invariant=False, 
    return_df=True, handle_missing='value', 
    handle_unknown='value', use_cat_names=False)`

```Python
import category_encoders as ce
from sklearn.datasets import load_boston

bunch = load_boston()
y = bunch.target
X = pd.DataFrame(bunch.data, columns=bunch.feature_names)
X.head()
#      CRIM    ZN  INDUS  CHAS    NOX  ...  RAD    TAX  PTRATIO       B  LSTAT
#0  0.00632  18.0   2.31   0.0  0.538  ...  1.0  296.0     15.3  396.90   4.98
#1  0.02731   0.0   7.07   0.0  0.469  ...  2.0  242.0     17.8  396.90   9.14
#2  0.02729   0.0   7.07   0.0  0.469  ...  2.0  242.0     17.8  392.83   4.03
#3  0.03237   0.0   2.18   0.0  0.458  ...  3.0  222.0     18.7  394.63   2.94
#4  0.06905   0.0   2.18   0.0  0.458  ...  3.0  222.0     18.7  396.90   5.33
#
#[5 rows x 13 columns]

ohe = ce.OneHotEncoder(cols=['CHAS', 'RAD'], handle_unknown='indicator').fit(X, y)
numeric_dataset = ohe.transform(X)
numeric_dataset.info()
#<class 'pandas.core.frame.DataFrame'>
#RangeIndex: 506 entries, 0 to 505
#Data columns (total 24 columns):
#CRIM       506 non-null float64
#ZN         506 non-null float64
#INDUS      506 non-null float64
#CHAS_1     506 non-null int64
#CHAS_2     506 non-null int64
#CHAS_-1    506 non-null int64
#NOX        506 non-null float64
#RM         506 non-null float64
#AGE        506 non-null float64
#DIS        506 non-null float64
#RAD_1      506 non-null int64
#RAD_2      506 non-null int64
#RAD_3      506 non-null int64
#RAD_4      506 non-null int64
#RAD_5      506 non-null int64
#RAD_6      506 non-null int64
#RAD_7      506 non-null int64
#RAD_8      506 non-null int64
#RAD_9      506 non-null int64
#RAD_-1     506 non-null int64
#TAX        506 non-null float64
#PTRATIO    506 non-null float64
#B          506 non-null float64
#LSTAT      506 non-null float64
#dtypes: float64(11), int64(13)
#memory usage: 95.0 KB

numeric_dataset.head()
#      CRIM    ZN  INDUS  CHAS_1  CHAS_2  ...  RAD_-1    TAX  PTRATIO       B  LSTAT
#0  0.00632  18.0   2.31       1       0  ...       0  296.0     15.3  396.90   4.98
#1  0.02731   0.0   7.07       1       0  ...       0  242.0     17.8  396.90   9.14
#2  0.02729   0.0   7.07       1       0  ...       0  242.0     17.8  392.83   4.03
#3  0.03237   0.0   2.18       1       0  ...       0  222.0     18.7  394.63   2.94
#4  0.06905   0.0   2.18       1       0  ...       0  222.0     18.7  396.90   5.33
#
#[5 rows x 24 columns]
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
- [Python类别变量处理](https://blog.csdn.net/weixin_42422585/article/details/83542722)