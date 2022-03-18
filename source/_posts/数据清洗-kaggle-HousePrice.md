---
title: Kaggle | House Prices:Advanced Regression Techniques
date: 2020-05-28 16:23:27
tags: [Data]
hide: true
math: true
toc: true
---

<center></center>
<!--more-->

{% cq %}
汇总请见：{% post_link Data-Cleaning-Projects 数据清洗合集 %}
{% endcq %}

# House Prices: Advanced Regression Techniques
数据来源：[Kaggle](https://www.kaggle.com/c/house-prices-advanced-regression-techniques/data)

代码参考：[COMPREHENSIVE DATA EXPLORATION WITH PYTHON](https://www.kaggle.com/pmarcelino/comprehensive-data-exploration-with-python)

进行数据分析前的主要工作：
1. 理解问题 Understanding the problem
   - 看每个变量的含义和重要性
2. 进行单变量探究 Univariate study
   - 关注因变量
3. 进行多变量探究 Multivariate study
   - 因变量与自变量间的关系
4. 简单数据清洗 Basic cleaning
   - 数据缺失
   - 异常点
   - 分类变量的处理
5. 检验假设 Test assumptions

## 载入数据


```python
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns
from scipy.stats import norm
from sklearn.preprocessing import StandardScaler
from scipy import stats
import warnings

warnings.filterwarnings("ignore")
%matplotlib inline
```


```python
df_train = pd.read_csv("train.csv")
df_train.columns
```




    Index(['Id', 'MSSubClass', 'MSZoning', 'LotFrontage', 'LotArea', 'Street',
           'Alley', 'LotShape', 'LandContour', 'Utilities', 'LotConfig',
           'LandSlope', 'Neighborhood', 'Condition1', 'Condition2', 'BldgType',
           'HouseStyle', 'OverallQual', 'OverallCond', 'YearBuilt', 'YearRemodAdd',
           'RoofStyle', 'RoofMatl', 'Exterior1st', 'Exterior2nd', 'MasVnrType',
           'MasVnrArea', 'ExterQual', 'ExterCond', 'Foundation', 'BsmtQual',
           'BsmtCond', 'BsmtExposure', 'BsmtFinType1', 'BsmtFinSF1',
           'BsmtFinType2', 'BsmtFinSF2', 'BsmtUnfSF', 'TotalBsmtSF', 'Heating',
           'HeatingQC', 'CentralAir', 'Electrical', '1stFlrSF', '2ndFlrSF',
           'LowQualFinSF', 'GrLivArea', 'BsmtFullBath', 'BsmtHalfBath', 'FullBath',
           'HalfBath', 'BedroomAbvGr', 'KitchenAbvGr', 'KitchenQual',
           'TotRmsAbvGrd', 'Functional', 'Fireplaces', 'FireplaceQu', 'GarageType',
           'GarageYrBlt', 'GarageFinish', 'GarageCars', 'GarageArea', 'GarageQual',
           'GarageCond', 'PavedDrive', 'WoodDeckSF', 'OpenPorchSF',
           'EnclosedPorch', '3SsnPorch', 'ScreenPorch', 'PoolArea', 'PoolQC',
           'Fence', 'MiscFeature', 'MiscVal', 'MoSold', 'YrSold', 'SaleType',
           'SaleCondition', 'SalePrice'],
          dtype='object')



'SalePrice'是因变量

## 描述性统计


```python
df_train['SalePrice'].describe()
```




    count      1460.000000
    mean     180921.195890
    std       79442.502883
    min       34900.000000
    25%      129975.000000
    50%      163000.000000
    75%      214000.000000
    max      755000.000000
    Name: SalePrice, dtype: float64



- 'SalePrice'最小值应该为正（$\surd$）
- 最小值和最大值相差较大


```python
## 直方图
sns.distplot(df_train['SalePrice'])
```




    <matplotlib.axes._subplots.AxesSubplot at 0x16f6ce68828>


<meta name="referrer" content="no-referrer" />
{% asset_img output_9_1.png 直方图 %}



- 'SalePrice'集中分布在300000以下
- 右偏


```python
## 计算‘SalePrice’的偏度和峰度
print('Skewness: %f' % df_train['SalePrice'].skew())  ## 偏度
print('Kurtosis: %f' % df_train['SalePrice'].kurt())  ## 峰度
```

    Skewness: 1.882876
    Kurtosis: 6.536282
    

- 偏度大于0——右偏
- 峰度大于3——厚尾

## 探究变量之间的关系


```python
## GrLivArea: Above grade (ground) living area square feet
## ‘GrLivArea’与‘SalePrice’的散点图
var = 'GrLivArea'
data = pd.concat([df_train['SalePrice'], df_train[var]],
                axis=1)  ## 按列合并
data.plot.scatter(x=var, y='SalePrice', ylim=(0, 800000))
```




    <matplotlib.axes._subplots.AxesSubplot at 0x16f6d629978>


<meta name="referrer" content="no-referrer" />
{% asset_img output_14_1.png 散点图 %}



'GriLivArea'与'SalePrice'之间具有明显的线性关系


```python
##  TotalBsmtSF：Total square feet of basement area
## ‘TotalBsmtSF’与‘SalePrice’的散点图
var = 'TotalBsmtSF'
data = pd.concat([df_train['SalePrice'], df_train[var]],
                axis=1)  ## 按列合并
data.plot.scatter(x=var, y='SalePrice', ylim=(0, 800000))
```




    <matplotlib.axes._subplots.AxesSubplot at 0x16f6d6eacc0>


<meta name="referrer" content="no-referrer" />
{% asset_img output_16_1.png 散点图 %}



- ‘TotalBsmtSF’与‘SalePrice’之间也具有明显的线性关系
- 一些样本中，‘TotalBsmtSF’为零


```python
## OverallQual: Rates the overall material and finish of the house
## 'OverallQual'与‘SalePrice’的箱线图
## 'OverallQual'是分类变量，有1~10共10个取值
var = 'OverallQual'
data = pd.concat([df_train['SalePrice'], df_train[var]], 
                axis=1)

f, ax = plt.subplots(figsize=(8, 6))
fig = sns.boxplot(x=var, y='SalePrice', data=data)
fig.axis(ymin=0, ymax=800000)
```




    (-0.5, 9.5, 0, 800000)



<meta name="referrer" content="no-referrer" />
{% asset_img output_9_1.png 箱线图 %}


- "OverallQual"越高，"SalePrice"的值相对越大
- "OverallQual"较高的"SalePrice"的分布较为分散


```python
## YearBuilt: Original construction date
var = 'YearBuilt'
data = pd.concat([df_train['SalePrice'], df_train[var]], 
                axis=1)

f, ax = plt.subplots(figsize=(16, 8))
fig = sns.boxplot(x=var, y='SalePrice', data=data)
fig.axis(ymin=0, ymax=800000)
plt.xticks(rotation=90)  ## x轴的刻度旋转90度（即垂直于x轴）
```




    (array([  0,   1,   2,   3,   4,   5,   6,   7,   8,   9,  10,  11,  12,
             13,  14,  15,  16,  17,  18,  19,  20,  21,  22,  23,  24,  25,
             26,  27,  28,  29,  30,  31,  32,  33,  34,  35,  36,  37,  38,
             39,  40,  41,  42,  43,  44,  45,  46,  47,  48,  49,  50,  51,
             52,  53,  54,  55,  56,  57,  58,  59,  60,  61,  62,  63,  64,
             65,  66,  67,  68,  69,  70,  71,  72,  73,  74,  75,  76,  77,
             78,  79,  80,  81,  82,  83,  84,  85,  86,  87,  88,  89,  90,
             91,  92,  93,  94,  95,  96,  97,  98,  99, 100, 101, 102, 103,
            104, 105, 106, 107, 108, 109, 110, 111]),
     <a list of 112 Text xticklabel objects>)



<meta name="referrer" content="no-referrer" />
{% asset_img output_20_1.png %}



不知“SalePrice”是否已剔除通胀影响的价格，无法对不同建造年份的房屋价格进行比较。


```python
## 相关系数阵
cor_m = df_train.corr()

f, ax = plt.subplots(figsize=(12, 9))
sns.heatmap(cor_m, vmax=.8, square=True)
```




    <matplotlib.axes._subplots.AxesSubplot at 0x16f6f3a2da0>



<meta name="referrer" content="no-referrer" />
{% asset_img output_22_1.png 相关系数矩阵热力图 %}



- 颜色越浅，表明正相关性越强
  - 'TotalBsmtSF'和'1stFlrSF'的正相关性很强（很显然）
  - 'GarageCars'和'GarageArea'的正相关性很强（也很显然）
- 上述两组正相关性很强的变量，导致数据存在多重共线性
  - 考虑剔除其中一个变量，因为正相关性强的两个变量包含的信息是相似的
- 看‘SalePrice’那一列，也可以通过热力图的颜色深浅，直观地发现与‘SalePrice’正相关性较强的变量，如‘OverallQual’、‘GriLivArea’

其中：
- 1stFlrSF: First Floor square feet
- TotalBsmtSF: Total square feet of basement area
- GarageCars: Size of garage in car capacity
- GarageArea: Size of garage in square feet


```python
## 放大相关系数矩阵热力图的部分
## 相关系数最大的10个变量
k = 10
cols = cor_m.nlargest(k, 'SalePrice')['SalePrice'].index
cm = np.corrcoef(df_train[cols].values.T)

sns.set(font_scale=1.25)
hm = sns.heatmap(cm, cbar=True, annot=True, square=True,
                fmt='.2f', annot_kws={'size': 10},
                yticklabels=cols.values, xticklabels=cols.values)
plt.show()
```

<meta name="referrer" content="no-referrer" />
{% asset_img output_24_0.png 相关性最强的10个变量的相关性热力图 %}




相关性较强的几对变量：
- ‘GarageCars’、‘GarageArea’
- ‘GriLivArea’、‘TotRmsAbvGrd’
- ‘TotalBsmtSF’、‘1stFlrSF’

考虑剔除每对中的一个


```python
## 散点图
sns.set()
cols = ['SalePrice', 'OverallQual', 'GrLivArea', 'GarageCars', 
        'TotalBsmtSF', 'FullBath', 'YearBuilt']
sns.pairplot(df_train[cols], size=2.5)
plt.show()
```

<meta name="referrer" content="no-referrer" />
{% asset_img output_26_0.png 两两散点图 %}



## 数据清洗

### 缺失


```python
## 统计每列的缺失数据个数，并按降序排列
total = df_train.isnull().sum().sort_values(ascending=False)
percent = (df_train.isnull().sum()/df_train.isnull().count()).sort_values(ascending=False)
missing_data = pd.concat([total, percent], axis=1,
                        keys=['Total', 'Percent'])
missing_data.head(20)  ## 缺失数据最多的20个列
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: center;">
      <th></th>
      <th>Total</th>
      <th>Percent</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>PoolQC</th>
      <td>1453</td>
      <td>0.995205</td>
    </tr>
    <tr>
      <th>MiscFeature</th>
      <td>1406</td>
      <td>0.963014</td>
    </tr>
    <tr>
      <th>Alley</th>
      <td>1369</td>
      <td>0.937671</td>
    </tr>
    <tr>
      <th>Fence</th>
      <td>1179</td>
      <td>0.807534</td>
    </tr>
    <tr>
      <th>FireplaceQu</th>
      <td>690</td>
      <td>0.472603</td>
    </tr>
    <tr>
      <th>LotFrontage</th>
      <td>259</td>
      <td>0.177397</td>
    </tr>
    <tr>
      <th>GarageCond</th>
      <td>81</td>
      <td>0.055479</td>
    </tr>
    <tr>
      <th>GarageType</th>
      <td>81</td>
      <td>0.055479</td>
    </tr>
    <tr>
      <th>GarageYrBlt</th>
      <td>81</td>
      <td>0.055479</td>
    </tr>
    <tr>
      <th>GarageFinish</th>
      <td>81</td>
      <td>0.055479</td>
    </tr>
    <tr>
      <th>GarageQual</th>
      <td>81</td>
      <td>0.055479</td>
    </tr>
    <tr>
      <th>BsmtExposure</th>
      <td>38</td>
      <td>0.026027</td>
    </tr>
    <tr>
      <th>BsmtFinType2</th>
      <td>38</td>
      <td>0.026027</td>
    </tr>
    <tr>
      <th>BsmtFinType1</th>
      <td>37</td>
      <td>0.025342</td>
    </tr>
    <tr>
      <th>BsmtCond</th>
      <td>37</td>
      <td>0.025342</td>
    </tr>
    <tr>
      <th>BsmtQual</th>
      <td>37</td>
      <td>0.025342</td>
    </tr>
    <tr>
      <th>MasVnrArea</th>
      <td>8</td>
      <td>0.005479</td>
    </tr>
    <tr>
      <th>MasVnrType</th>
      <td>8</td>
      <td>0.005479</td>
    </tr>
    <tr>
      <th>Electrical</th>
      <td>1</td>
      <td>0.000685</td>
    </tr>
    <tr>
      <th>Utilities</th>
      <td>0</td>
      <td>0.000000</td>
    </tr>
  </tbody>
</table>
</div>



- 当一个列的数据超过15%都是缺失，则应考虑删除该列
- 列'PoolQC', 'MiscFeature', 'Alley'等的缺失比例较大，考虑删除
- ‘GarageX’几个列的缺失个数一样，但是缺失比例都较小
- ‘BsmtX’几个列的缺失个数也一样


```python
## 考虑删除上述存在缺失值的列（除了‘Electrical’）
df_train = df_train.drop((missing_data[missing_data['Total'] > 1]).index, 1)
## 删除‘Electrical’缺失的样本
df_train = df_train.drop(df_train.loc[df_train['Electrical'].isnull()].index)
df_train.isnull().sum().max()  ## 检查是否还存在缺失
```




    0



### 异常点


```python
## 标准化
SalePrice_scaled = StandardScaler().fit_transform(df_train['SalePrice'][:, np.newaxis])
low_range = SalePrice_scaled[SalePrice_scaled[:, 0].argsort()][:10]
high_range = SalePrice_scaled[SalePrice_scaled[:, 0].argsort()][-10:]
print('Outer range (low) of the distribution:')
print(low_range)
print('\nOuter range (high) of the distribution:')
print(high_range)
```

    Outer range (low) of the distribution:
    [[-1.83820775]
     [-1.83303414]
     [-1.80044422]
     [-1.78282123]
     [-1.77400974]
     [-1.62295562]
     [-1.6166617 ]
     [-1.58519209]
     [-1.58519209]
     [-1.57269236]]
    
    Outer range (high) of the distribution:
    [[3.82758058]
     [4.0395221 ]
     [4.49473628]
     [4.70872962]
     [4.728631  ]
     [5.06034585]
     [5.42191907]
     [5.58987866]
     [7.10041987]
     [7.22629831]]
    

- 较低的10个值仅在-1.5到-1.9之间
- 而较高的10个值的跨度从3到7.5
- 要小心SalePrice标准化后值为7以上的样本点


```python
## 再来看看该图
## ‘GrLivArea’与‘SalePrice’的散点图
var = 'GrLivArea'
data = pd.concat([df_train['SalePrice'], df_train[var]],
                axis=1)  ## 按列合并
data.plot.scatter(x=var, y='SalePrice', ylim=(0, 800000))
```

    'c' argument looks like a single numeric RGB or RGBA sequence, which should be avoided as value-mapping will have precedence in case its length matches with 'x' & 'y'.  Please use a 2-D array with a single row if you really want to specify the same RGB or RGBA value for all points.
    




    <matplotlib.axes._subplots.AxesSubplot at 0x16f736e0ac8>


<meta name="referrer" content="no-referrer" />
{% asset_img output_35_2.png 散点图 %}



观察上图，可以发现右上方和右下方各有2个点较为异常。
- 右上方的两个点比较顺应整体趋势
- 而右下方的两个点并没有顺应整体趋势，考虑将其删去


```python
## 删除异常点
df_train.sort_values(by='GrLivArea', ascending=False)[:2]
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Id</th>
      <th>MSSubClass</th>
      <th>MSZoning</th>
      <th>LotArea</th>
      <th>Street</th>
      <th>LotShape</th>
      <th>LandContour</th>
      <th>Utilities</th>
      <th>LotConfig</th>
      <th>LandSlope</th>
      <th>...</th>
      <th>EnclosedPorch</th>
      <th>3SsnPorch</th>
      <th>ScreenPorch</th>
      <th>PoolArea</th>
      <th>MiscVal</th>
      <th>MoSold</th>
      <th>YrSold</th>
      <th>SaleType</th>
      <th>SaleCondition</th>
      <th>SalePrice</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1298</th>
      <td>1299</td>
      <td>60</td>
      <td>RL</td>
      <td>63887</td>
      <td>Pave</td>
      <td>IR3</td>
      <td>Bnk</td>
      <td>AllPub</td>
      <td>Corner</td>
      <td>Gtl</td>
      <td>...</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>480</td>
      <td>0</td>
      <td>1</td>
      <td>2008</td>
      <td>New</td>
      <td>Partial</td>
      <td>160000</td>
    </tr>
    <tr>
      <th>523</th>
      <td>524</td>
      <td>60</td>
      <td>RL</td>
      <td>40094</td>
      <td>Pave</td>
      <td>IR1</td>
      <td>Bnk</td>
      <td>AllPub</td>
      <td>Inside</td>
      <td>Gtl</td>
      <td>...</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>10</td>
      <td>2007</td>
      <td>New</td>
      <td>Partial</td>
      <td>184750</td>
    </tr>
  </tbody>
</table>
<p>2 rows × 63 columns</p>
</div>




```python
df_train = df_train.drop(df_train[df_train['Id'] == 1299].index)
df_train = df_train.drop(df_train[df_train['Id'] == 523].index)
```

## 核心
testing for assumptions underlying the statistical bases for multivariate analysis.

{% note default %}
根据[Multivariate Data Analysis-Hair et al (2013)](https://www.amazon.com/Multivariate-Data-Analysis-Joseph-Hair/dp/9332536503/ref=as_sl_pc_tf_til?tag=pmarcelino-20&linkCode=w00&linkId=5e9109fa2213fef911dae80731a07a17&creativeASIN=9332536503)，需要进行检验的假设有：
1. 正态性 Normality
  - 许多统计检验方法都基于正态分布的假设构建的
  - 当样本量大于200时，正态性就不太影响了
2. 同方差性 Homoskedasticity
  - 误差项通常是独立同方差的
3. 线性性 Linearity
  - 因变量与自变量之间呈线性关系
  - 如果不是线性关系，可能需要先进行数据变换（data transformation）
4. 序列自相关性
  - 通常存在于时间序列
{% endnote %}

### 正态性检验
- 直方图
- 概率密度图(Q-Q图)


```python
sns.distplot(df_train['SalePrice'], fit=norm)
fig = plt.figure()
res = stats.probplot(df_train['SalePrice'], plot=plt)
```

<meta name="referrer" content="no-referrer" />
{% asset_img output_41_0.png 密度曲线 %}


<meta name="referrer" content="no-referrer" />
{% asset_img output_41_1.png Q-Q图 %}



- 很显然，‘SalePrice’并不服从正态分布
 - 右偏
 - QQ图尾部（蓝点）与标准正态分布（红线）不一致


```python
## 对数据进行对数变换
df_train['SalePrice'] = np.log(df_train['SalePrice'])
```


```python
## 检验对数变换后数据的正态性
sns.distplot(df_train['SalePrice'], fit=norm)
fig = plt.figure()
res = stats.probplot(df_train['SalePrice'], plot=plt)
```

<meta name="referrer" content="no-referrer" />
{% asset_img output_44_0.png 密度曲线图 %}


<meta name="referrer" content="no-referrer" />
{% asset_img output_44_1.png Q-Q图 %}




Perfect！


```python
## 再来看看'GrLivArea'
sns.distplot(df_train['GrLivArea'], fit=norm)
fig = plt.figure()
res = stats.probplot(df_train['GrLivArea'], plot=plt)
```

<meta name="referrer" content="no-referrer" />
{% asset_img output_46_0.png 密度曲线图 %}


<meta name="referrer" content="no-referrer" />
{% asset_img output_46_1.png Q-Q图 %}




```python
## 进行对数变换
df_train['GrLivArea'] = np.log(df_train['GrLivArea'])
```


```python
sns.distplot(df_train['GrLivArea'], fit=norm)
fig = plt.figure()
res = stats.probplot(df_train['GrLivArea'], plot=plt)
```

<meta name="referrer" content="no-referrer" />
{% asset_img output_48_0.png 密度曲线图 %}


<meta name="referrer" content="no-referrer" />
{% asset_img output_48_1.png Q-Q图 %}




```python
## 'TotalBsmtSF'
sns.distplot(df_train['TotalBsmtSF'], fit=norm);
fig = plt.figure()
res = stats.probplot(df_train['TotalBsmtSF'], plot=plt)
```


<meta name="referrer" content="no-referrer" />
{% asset_img output_49_0.png 密度曲线图 %}


<meta name="referrer" content="no-referrer" />
{% asset_img output_49_1.png Q-Q图 %}




```python
## 'TotalBsmtSF'存在0值，无法直接进行对数变换
## 只将'TotalBsmtSF'非零值的数据进行对数变换
df_train['HasBsmt'] = pd.Series(len(df_train['TotalBsmtSF']), index=df_train.index)
df_train['HasBsmt'] = 0 
df_train.loc[df_train['TotalBsmtSF']>0,'HasBsmt'] = 1
df_train.loc[df_train['HasBsmt']==1,'TotalBsmtSF'] = np.log(df_train['TotalBsmtSF'])
```


```python
sns.distplot(df_train[df_train['TotalBsmtSF']>0]['TotalBsmtSF'], fit=norm);
fig = plt.figure()
res = stats.probplot(df_train[df_train['TotalBsmtSF']>0]['TotalBsmtSF'], plot=plt)
```


<meta name="referrer" content="no-referrer" />
{% asset_img output_51_0.png 密度曲线图 %}


<meta name="referrer" content="no-referrer" />
{% asset_img output_51_1.png Q-Q图 %}



### 同方差性（同质性）


```python
plt.scatter(df_train['GrLivArea'], df_train['SalePrice'])
```




    <matplotlib.collections.PathCollection at 0x16f7589eba8>


<meta name="referrer" content="no-referrer" />
{% asset_img output_53_1.png 散点图 %}




- 未进行对数变换前，'GrLivArea'和'SalePrice'散点图呈锥形
- 进行对数变换后，不再呈锥形（上图）


```python
plt.scatter(df_train[df_train['TotalBsmtSF']>0]['TotalBsmtSF'], df_train[df_train['TotalBsmtSF']>0]['SalePrice'])
```




    <matplotlib.collections.PathCollection at 0x16f7589ebe0>



<meta name="referrer" content="no-referrer" />
{% asset_img output_55_1.png 散点图 %}



We can say that, in general, 'SalePrice' exhibit equal levels of variance across the range of 'TotalBsmtSF'.

### 类别变量
需要将类别变量转化为哑变量（dummy）


```python
df_train = pd.get_dummies(df_train)
```


```python
df_train.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Id</th>
      <th>MSSubClass</th>
      <th>LotArea</th>
      <th>OverallQual</th>
      <th>OverallCond</th>
      <th>YearBuilt</th>
      <th>YearRemodAdd</th>
      <th>BsmtFinSF1</th>
      <th>BsmtFinSF2</th>
      <th>BsmtUnfSF</th>
      <th>...</th>
      <th>SaleType_ConLw</th>
      <th>SaleType_New</th>
      <th>SaleType_Oth</th>
      <th>SaleType_WD</th>
      <th>SaleCondition_Abnorml</th>
      <th>SaleCondition_AdjLand</th>
      <th>SaleCondition_Alloca</th>
      <th>SaleCondition_Family</th>
      <th>SaleCondition_Normal</th>
      <th>SaleCondition_Partial</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>60</td>
      <td>8450</td>
      <td>7</td>
      <td>5</td>
      <td>2003</td>
      <td>2003</td>
      <td>706</td>
      <td>0</td>
      <td>150</td>
      <td>...</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2</td>
      <td>20</td>
      <td>9600</td>
      <td>6</td>
      <td>8</td>
      <td>1976</td>
      <td>1976</td>
      <td>978</td>
      <td>0</td>
      <td>284</td>
      <td>...</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>3</td>
      <td>60</td>
      <td>11250</td>
      <td>7</td>
      <td>5</td>
      <td>2001</td>
      <td>2002</td>
      <td>486</td>
      <td>0</td>
      <td>434</td>
      <td>...</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>4</td>
      <td>70</td>
      <td>9550</td>
      <td>7</td>
      <td>5</td>
      <td>1915</td>
      <td>1970</td>
      <td>216</td>
      <td>0</td>
      <td>540</td>
      <td>...</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>5</td>
      <td>60</td>
      <td>14260</td>
      <td>8</td>
      <td>5</td>
      <td>2000</td>
      <td>2000</td>
      <td>655</td>
      <td>0</td>
      <td>490</td>
      <td>...</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 222 columns</p>
</div>


