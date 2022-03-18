---
title: Python | Dython
date: 2021-08-22 22:33:16
tags: [Python]
categories: Python
mathjax: true
toc: true
index_img: /img/Python.png  ## 封面图片
hide: true
notshow: true
---

<center></center>
<!--more-->

# Dython 
Dython是一款数据建模库
- 依赖`numpy`、`pandas`、`seaborn`、`scipy`、`matplotlib`、`sklearn`、`scikit-plot`
- 包含的子模块：
    - `data_utils`：基础的数据探索性分析
    - `nominal`：特征相关性度量
    - `model_utils`：机器学习性能评估工具
    - `sampling`：数据采样

安装：
```python
pip install dython
## or
pip install git+https://github.com/shakedzy/dython.git
```

```python
## 载入数据集，用于示例
from sklearn import datasets
import pandas as pd
import numpy as np

dat = datasets.load_diabetes()
print(" ".join(dat.keys()))     ## 数据集包含的信息项
# data target DESCR feature_names data_filename target_filename
df = pd.DataFrame(dat['data'], columns=dat['feature_names'])
df.head()
# age	sex	bmi	bp	s1	s2	s3	s4	s5	s6
# 0	0.038076	0.050680	0.061696	0.021872	-0.044223	-0.034821	-0.043401	-0.002592	0.019908	-0.017646
# 1	-0.001882	-0.044642	-0.051474	-0.026328	-0.008449	-0.019163	0.074412	-0.039493	-0.068330	-0.092204
# 2	0.085299	0.050680	0.044451	-0.005671	-0.045599	-0.034194	-0.032356	-0.002592	0.002864	-0.025930
# 3	-0.089063	-0.044642	-0.011595	-0.036656	0.012191	0.024991	-0.036038	0.034309	0.022692	-0.009362
# 4	0.005383	-0.044642	-0.036385	0.021872	0.003935	0.015596	0.008142	-0.002592	-0.031991	-0.046641
```

# data_utils
## identify_columns_with_na()
`identify_columns_with_na()`：数据集的缺失情况，输出每列的数据缺失个数

```python
## 将部分值替换为“缺失值nan”
df.loc[4:5, 'bmi'] = np.nan
df.loc[1:4, 's3'] = np.nan

from dython import data_utils
data_utils.identify_columns_with_na(df)
# column	na_count
# 6	s3	4
# 2	bmi	2
### 6是列s3的索引（s3为第7列）
### 2是列bmi的索引（bmi为第3列）
### 按照缺失值个数降序排序
```

## identify_columns_by_type()
`identify_columns_by_type()`：按列的类型查找列


## split_hist()
`split_hist()`：快速绘制分组直方图

# nominal
## associations()
计算数据集中变量的相关系数。
- 相关系数包括：
    - `Pearson`
    - `Cramer's V`
    - `Theil's U`
    - 条件熵
- 参数`nom_nom_assoc='cramer'`：名义变量之间的相关系数计算方法，默认为`cramer`
- 参数`num_num_assoc='pearson'`：数值变量之间的相关系数计算方法，默认为Pearson相关系数

```python
from dython.nominal import associations
associations(df)
```


## cluster_correlations()
绘制基于层次聚类的相关系数矩阵图（热力图）


# model_utils
## ks_abc()


## metric_graph()
- 参数`metric`：
    - `roc`：绘制ROC曲线

# sampling 
## boltzmann_sampling()

## weighted_sampling()


# 参考资料
- [Dython主页](http://shakedzy.xyz/dython/)
- [GitHub: Dython](https://github.com/shakedzy/dython)