---
title: Kaggle | Medical Cost Personal可视化
date: 2020-05-24 14:10:03
tags: [Data]
hide: true
math: true
toc: true
---

<center></center>
<!--more-->

# 数据说明

- 数据来源：[Kaggle](https://www.kaggle.com/mirichoi0218/insurance)
- 可视化代码来源：[EDA l Data Visualization](https://www.kaggle.com/rockbt1189/eda-l-data-visualization?tdsourcetag=s_pctim_aiomsg)
- 使用Kaggle在线Jupyter Notebook实现


# 载入数据
```python
# This Python 3 environment comes with many helpful analytics libraries installed
# It is defined by the kaggle/python Docker image: https://github.com/kaggle/docker-python
# For example, here's several helpful packages to load

import numpy as np # linear algebra
import pandas as pd # data processing, CSV file I/O (e.g. pd.read_csv)

# Input data files are available in the read-only "../input/" directory
# For example, running this (by clicking run or pressing Shift+Enter) will list all files under the input directory

import os
print(os.listdir("../input"))

# You can write up to 5GB to the current directory (/kaggle/working/) that gets preserved as output when you create a version using "Save & Run All" 
# You can also write temporary files to /kaggle/temp/, but they won't be saved outside of the current session
```

    ['insurance']
    


```python
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns
import warnings

warnings.filterwarnings("ignore")
```


```python
df = pd.read_csv("../input/insurance/insurance.csv")  ## 加载数据
df.head()  ## 预览前5行
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
      <th>age</th>
      <th>sex</th>
      <th>bmi</th>
      <th>children</th>
      <th>smoker</th>
      <th>region</th>
      <th>charges</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>19</td>
      <td>female</td>
      <td>27.900</td>
      <td>0</td>
      <td>yes</td>
      <td>southwest</td>
      <td>16884.92400</td>
    </tr>
    <tr>
      <th>1</th>
      <td>18</td>
      <td>male</td>
      <td>33.770</td>
      <td>1</td>
      <td>no</td>
      <td>southeast</td>
      <td>1725.55230</td>
    </tr>
    <tr>
      <th>2</th>
      <td>28</td>
      <td>male</td>
      <td>33.000</td>
      <td>3</td>
      <td>no</td>
      <td>southeast</td>
      <td>4449.46200</td>
    </tr>
    <tr>
      <th>3</th>
      <td>33</td>
      <td>male</td>
      <td>22.705</td>
      <td>0</td>
      <td>no</td>
      <td>northwest</td>
      <td>21984.47061</td>
    </tr>
    <tr>
      <th>4</th>
      <td>32</td>
      <td>male</td>
      <td>28.880</td>
      <td>0</td>
      <td>no</td>
      <td>northwest</td>
      <td>3866.85520</td>
    </tr>
  </tbody>
</table>
</div>


```python
print(df.shape)  ## 查看数据的维度
print(df.describe())  ## 查看数据的描述统计
print("Total number of NULL value in the dataset:", df.isnull().sum())  ## 数据集中缺失数据的个数
```

    (1338, 7)
                   age          bmi     children       charges
    count  1338.000000  1338.000000  1338.000000   1338.000000
    mean     39.207025    30.663397     1.094918  13270.422265
    std      14.049960     6.098187     1.205493  12110.011237
    min      18.000000    15.960000     0.000000   1121.873900
    25%      27.000000    26.296250     0.000000   4740.287150
    50%      39.000000    30.400000     1.000000   9382.033000
    75%      51.000000    34.693750     2.000000  16639.912515
    max      64.000000    53.130000     5.000000  63770.428010
    Total number of NULL value in the dataset: age         0
    sex         0
    bmi         0
    children    0
    smoker      0
    region      0
    charges     0
    dtype: int64
    

# 新建特征
BMI:
- Normal: bmi <= 24
- OverWeight: 24 < bmi <30
- Obese: bmi >= 30


```python
## 按照bmi划分
df['risk_type'] = np.where(df.bmi<=24, "Normal", 
                          (np.where(df.bmi<30, "OverWeight", "Obese")))
## 按照age划分区组
df['Age_group'] = np.where(df.age<25, "Age below 25 year", 
                          (np.where(df.age<35, "Age 25 to 34 year",
                                   (np.where(df.age<55, "Age 35 to 54 year",
                                            (np.where(df.age<75, "Age 55 to 74 year",
                                                     "Age more than 75 year")))))))
df.head()
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
      <th>age</th>
      <th>sex</th>
      <th>bmi</th>
      <th>children</th>
      <th>smoker</th>
      <th>region</th>
      <th>charges</th>
      <th>risk_type</th>
      <th>Age_group</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>19</td>
      <td>female</td>
      <td>27.900</td>
      <td>0</td>
      <td>yes</td>
      <td>southwest</td>
      <td>16884.92400</td>
      <td>OverWeight</td>
      <td>Age below 25 year</td>
    </tr>
    <tr>
      <th>1</th>
      <td>18</td>
      <td>male</td>
      <td>33.770</td>
      <td>1</td>
      <td>no</td>
      <td>southeast</td>
      <td>1725.55230</td>
      <td>Obese</td>
      <td>Age below 25 year</td>
    </tr>
    <tr>
      <th>2</th>
      <td>28</td>
      <td>male</td>
      <td>33.000</td>
      <td>3</td>
      <td>no</td>
      <td>southeast</td>
      <td>4449.46200</td>
      <td>Obese</td>
      <td>Age 25 to 34 year</td>
    </tr>
    <tr>
      <th>3</th>
      <td>33</td>
      <td>male</td>
      <td>22.705</td>
      <td>0</td>
      <td>no</td>
      <td>northwest</td>
      <td>21984.47061</td>
      <td>Normal</td>
      <td>Age 25 to 34 year</td>
    </tr>
    <tr>
      <th>4</th>
      <td>32</td>
      <td>male</td>
      <td>28.880</td>
      <td>0</td>
      <td>no</td>
      <td>northwest</td>
      <td>3866.85520</td>
      <td>OverWeight</td>
      <td>Age 25 to 34 year</td>
    </tr>
  </tbody>
</table>
</div>



也可应用Pandas模块中的`cut()`函数进行分组，具体可见{% post_link python-pandas模块 Pandas模块 %}。


```python
## 分组也可按如下操作
bins1 = [0, 24, 29.99, 100]
df['risk_bins'] = df.cut(df['bmi'], bins1, labels=['Normal', 'OverWeight', 'Obese'])

bins2 = [0, 25, 35, 55, 75, 100]
df['age_bins'] = df.cut(df['age'], bins2, 
                        labels=["Age below 25 year", "Age 25 to 34 year", "Age 35 to 54 year", 
                               "Age 55 to 74 year", "Age more than 75 year"])
```


```python
df["Charge"] = df["charges"]/1000
```

# 数据可视化

```python
## 相关图
sns.pairplot(df, height=1.8)
```




    <seaborn.axisgrid.PairGrid at 0x7f6074b5ced0>



<meta name="referrer" content="no-referrer" />
{% asset_img output_10_1.png 相关图 %}


- 对角线上的是特征的样本数据直方图
- 非对角线的是两两特征之间的相关图（以两特征分别为横纵轴）
- `charges`是`Charge`除以1000得到的，二者完全正相关，因此相关图呈一条45度的直线
- `bmi`与`age`没有什么相关性
- 不同`charges`区间中，`charges`以及`Charge`与`age`呈正相关

```python
plt.rcParams["figure.figsize"] = (20, 14)
plt.subplot(421)  ## 将画布裁成4行2列  ## 第1个子图
df['age'].value_counts().sort_index().plot.line(color="k")  ## 折线图
plt.title("Age distribution in the Data")  ## 标题

plt.subplot(422)  ## 第2个子图
df['bmi'].sort_index().plot.hist(color="g")  ## 直方图
plt.title("bmi distribution in the Data")

plt.subplot(423)  ## 第3个子图
df['children'].value_counts().plot.line(color="b")  ## 折线图
plt.title("child distribution in the Data")

plt.subplot(424)
df['charges'].plot.hist(color="c")
plt.title("charges distribution in the Data")

plt.subplot(425)
df["risk_type"].value_counts().plot.bar()  ## 条形图
plt.title("risk_type distribution in the Data")

plt.subplot(426)
df['smoker'].value_counts().plot.bar()
plt.title("smoker distribution in the data")

plt.subplot(427)
df['Age_group'].value_counts().plot.bar()
plt.title("Age_group distribution in the Data")
```




    Text(0.5, 1.0, 'Age_group distribution in the Data')


<meta name="referrer" content="no-referrer" />
{% asset_img output_12_1.png %}

- 样本中的年龄主要集中分布在“小于20岁”区间
- 样本中肥胖的样本较多
- 样本中，抽烟的样本较少，不抽样的样本较多


```python
plt.rcParams['figure.figsize'] = (18, 8)
plt.subplot(221)  ## 将画布分成2行2列
sns.lineplot(x='age', y='bmi', data=df, color="b")

plt.subplot(222)
sns.lineplot(x='age', y='Charge', data=df, color="g")  ## 折线图

plt.subplot(223)
sns.scatterplot(x="Charge", y="bmi", data=df, color="k")  ## 散点图
```




    <matplotlib.axes._subplots.AxesSubplot at 0x7f6069f93d90>



<meta name="referrer" content="no-referrer" />
{% asset_img output_13_1.png %}

- `bmi`有随`age`上升而上升的趋势
- `charges`有随`age`上升而上升的趋势

下面按年龄段绘制`bmi`、`charges`与`age`的折线图：
```python
plt.rcParams['figure.figsize'] = (18, 8)
plt.subplot(221)
## 横轴是age，纵轴是bmi，按Age_group分段绘制
sns.lineplot(x='age', y='bmi', data=df, hue="Age_group")

plt.subplot(222)
sns.lineplot(x='age', y='Charge', data=df, hue='Age_group')
```




    <matplotlib.axes._subplots.AxesSubplot at 0x7f6069d0fad0>


<meta name="referrer" content="no-referrer" />
{% asset_img output_14_1.png %}





```python
plt.rcParams["figure.figsize"]=(18,8)
plt.subplot(221)
sns.lineplot(x="age", y="bmi", data=df, color="b", hue="smoker")

plt.subplot(222)
sns.lineplot(x="age", y="Charge", data=df, color="g", hue="smoker")
```




    <matplotlib.axes._subplots.AxesSubplot at 0x7f6069c22150>


<meta name="referrer" content="no-referrer" />
{% asset_img output_15_1.png %}

- 不吸烟的人的`Charge`整体比吸烟的人低


```python
## region
southwest = df[df["region"]=="southwest"]
southeast = df[df["region"]=="southeast"]
northwest = df[df["region"]=="northwest"]
northeast = df[df["region"]=="northeast"]

plt.rcParams["figure.figsize"]=(18,8)
plt.subplot(421)
sns.lineplot(x="age", y="bmi", data=southwest, hue="risk_type", hue_order=["Normal","OverWeight","Obese"])
plt.title("southwest region")

plt.subplot(422)
sns.lineplot(x="bmi", y="Charge", data=southwest, hue="risk_type", hue_order=["Normal","OverWeight","Obese"])
plt.title("southwest region")

plt.subplot(423)
sns.lineplot(x="age", y="bmi", data=southeast, hue="risk_type", hue_order=["Normal","OverWeight","Obese"])
plt.title("southeast region")

plt.subplot(424)
sns.lineplot(x="bmi", y="Charge", data=southeast, hue="risk_type", hue_order=["Normal","OverWeight","Obese"])
plt.title("southeast region")

plt.subplot(425)
sns.lineplot(x="age", y="bmi", data=northwest, hue="risk_type", hue_order=["Normal","OverWeight","Obese"])
plt.title("northwest region")

plt.subplot(426)
sns.lineplot(x="bmi", y="Charge", data=northwest, hue="risk_type", hue_order=["Normal","OverWeight","Obese"])
plt.title("northwest region")

plt.subplot(427)
sns.lineplot(x="age", y="bmi", data=northeast, hue="risk_type", hue_order=["Normal","OverWeight","Obese"])
plt.title("northeast region")

plt.subplot(428)
sns.lineplot(x="bmi", y="Charge", data=northeast, hue="risk_type", hue_order=["Normal","OverWeight","Obese"])
plt.title("northeast region")
```




    Text(0.5, 1.0, 'northeast region')


<meta name="referrer" content="no-referrer" />
{% asset_img output_16_1.png %}





```python
plt.rcParams["figure.figsize"]=(16,6)
plt.subplot(2,2,1)
sns.violinplot(x="sex", y="bmi", data=df)  ## 小提琴图

plt.subplot(2,2,2)
sns.violinplot(x="smoker", y="bmi", data=df)

plt.subplot(2,2,3)
sns.violinplot(x="risk_type", y="bmi", data=df)

plt.subplot(2,2,4)
sns.violinplot(x="Age_group", y="bmi", data=df)
```




    <matplotlib.axes._subplots.AxesSubplot at 0x7f60692f6590>


<meta name="referrer" content="no-referrer" />
{% asset_img output_17_1.png %}




```python
plt.rcParams["figure.figsize"]=(16,6)
plt.subplot(2,2,1)
sns.violinplot(x="sex", y="Charge", data=df)

plt.subplot(2,2,2)
sns.violinplot(x="smoker", y="Charge", data=df)

plt.subplot(2,2,3)
sns.violinplot(x="risk_type", y="Charge", data=df)

plt.subplot(2,2,4)
sns.violinplot(x="Age_group", y="Charge", data=df)
```




    <matplotlib.axes._subplots.AxesSubplot at 0x7f6069128bd0>



<meta name="referrer" content="no-referrer" />
{% asset_img output_18_1.png %}



```python
plt.rcParams["figure.figsize"]=(28,10)
plt.subplot(2,2,1)
sns.violinplot(x="children", y="bmi", data=df, hue="smoker")

plt.rcParams["figure.figsize"]=(28,10)
plt.subplot(2,2,3)
sns.violinplot(x="children", y="Charge", data=df, hue="smoker")
```




    <matplotlib.axes._subplots.AxesSubplot at 0x7f6069054ad0>



<meta name="referrer" content="no-referrer" />
{% asset_img output_19_1.png %}
