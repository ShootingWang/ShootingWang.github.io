---
title: Python | missingno
date: 2020-05-20 13:43:49
tags: [Python]
categories: Python
mathjax: true
toc: true
hide: true
notshow: true
---
<center>用于可视化缺失值</center>
<!--more-->


# missingno
安装：
```shell
pip install missingno
```

首先下载样本数据：
```shell
pip install quilt
quilt install ResidentMario/missingno_data
```

加载数据：
```python
import numpy as np
from quilt.data.ResidentMario import missingno_data

collisions = missingno_data.nyc_collision_factors()
collisions = collisions.replace('nan', np.nan)
collisions.head()
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
      <th>Unnamed: 0</th>
      <th>DATE</th>
      <th>TIME</th>
      <th>BOROUGH</th>
      <th>ZIP CODE</th>
      <th>LATITUDE</th>
      <th>LONGITUDE</th>
      <th>LOCATION</th>
      <th>ON STREET NAME</th>
      <th>CROSS STREET NAME</th>
      <th>OFF STREET NAME</th>
      <th>NUMBER OF PERSONS INJURED</th>
      <th>NUMBER OF PERSONS KILLED</th>
      <th>NUMBER OF PEDESTRIANS INJURED</th>
      <th>NUMBER OF PEDESTRIANS KILLED</th>
      <th>NUMBER OF CYCLISTS INJURED</th>
      <th>NUMBER OF CYCLISTS KILLED</th>
      <th>CONTRIBUTING FACTOR VEHICLE 1</th>
      <th>CONTRIBUTING FACTOR VEHICLE 2</th>
      <th>CONTRIBUTING FACTOR VEHICLE 3</th>
      <th>CONTRIBUTING FACTOR VEHICLE 4</th>
      <th>CONTRIBUTING FACTOR VEHICLE 5</th>
      <th>VEHICLE TYPE CODE 1</th>
      <th>VEHICLE TYPE CODE 2</th>
      <th>VEHICLE TYPE CODE 3</th>
      <th>VEHICLE TYPE CODE 4</th>
      <th>VEHICLE TYPE CODE 5</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0</td>
      <td>11/10/2016</td>
      <td>16:11:00</td>
      <td>BROOKLYN</td>
      <td>11208.0</td>
      <td>40.662514</td>
      <td>-73.872007</td>
      <td>(40.6625139, -73.8720068)</td>
      <td>WORTMAN AVENUE</td>
      <td>MONTAUK AVENUE</td>
      <td>NaN</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>Failure to Yield Right-of-Way</td>
      <td>Unspecified</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>TAXI</td>
      <td>PASSENGER VEHICLE</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1</td>
      <td>11/10/2016</td>
      <td>05:11:00</td>
      <td>MANHATTAN</td>
      <td>10013.0</td>
      <td>40.721323</td>
      <td>-74.008344</td>
      <td>(40.7213228, -74.0083444)</td>
      <td>HUBERT STREET</td>
      <td>HUDSON STREET</td>
      <td>NaN</td>
      <td>1</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>Failure to Yield Right-of-Way</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>PASSENGER VEHICLE</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2</td>
      <td>04/16/2016</td>
      <td>09:15:00</td>
      <td>BROOKLYN</td>
      <td>11201.0</td>
      <td>40.687999</td>
      <td>-73.997563</td>
      <td>(40.6879989, -73.9975625)</td>
      <td>HENRY STREET</td>
      <td>WARREN STREET</td>
      <td>NaN</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>Lost Consciousness</td>
      <td>Lost Consciousness</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>PASSENGER VEHICLE</td>
      <td>VAN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>3</th>
      <td>3</td>
      <td>04/15/2016</td>
      <td>10:20:00</td>
      <td>QUEENS</td>
      <td>11375.0</td>
      <td>40.719228</td>
      <td>-73.854542</td>
      <td>(40.7192276, -73.8545422)</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>67-64 FLEET STREET</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>Failure to Yield Right-of-Way</td>
      <td>Failure to Yield Right-of-Way</td>
      <td>Failure to Yield Right-of-Way</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>PASSENGER VEHICLE</td>
      <td>PASSENGER VEHICLE</td>
      <td>PASSENGER VEHICLE</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>4</th>
      <td>4</td>
      <td>04/15/2016</td>
      <td>10:35:00</td>
      <td>BROOKLYN</td>
      <td>11210.0</td>
      <td>40.632147</td>
      <td>-73.952731</td>
      <td>(40.6321467, -73.9527315)</td>
      <td>BEDFORD AVENUE</td>
      <td>CAMPUS ROAD</td>
      <td>NaN</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>Failure to Yield Right-of-Way</td>
      <td>Failure to Yield Right-of-Way</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>PASSENGER VEHICLE</td>
      <td>PASSENGER VEHICLE</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
</div>

## bar()
```python
msno.bar(collisions.sample(1000))
```

<meta name="referrer" content="no-referrer" />
{% asset_img output_4_1.png 条形图 %}


## Dendrogram()
谱系图/系统树图

```python
msno.dendrogram(collisions)
```

<meta name="referrer" content="no-referrer" />
{% asset_img output_6_1.png 谱系图 %}

- `NUMBER OF CYCLISTS INJURED`,`NUMBER OF CYCLISTS SKILLED`,`CONTRIBUTING FACTOR VEHICLE 1`,`NUMBER OF PEDESTRIANS SKILLED`,`NUMBER OF PEDESTRIANS INJURED`,`NUMBER OF PERSONS KILLED`等数据完整，没有缺失值，他们的距离为零，聚为一类。
- `BOROUGH`,`ZIP CODE`缺失相关性为1（同时缺失），距离为零；且缺失数据最少（除完整数据外），所以在完整数据后聚为一类。
- ……


## heatmap()
热力图
体现一个变量的存在或不存在如何强烈影响另一个变量的存在

```python
msno.heatmap(collisions)
```

<meta name="referrer" content="no-referrer" />
{% asset_img output_5_1.png 热力图 %}

`ZIP CODE`与`BOROUGH`的缺失相关性为1，说明：只要`BOROUGH`发生了缺失，`ZIP CODE`也会缺失。



## matrix()
```python
import missingno as msno
%matplotlib inline

msno.matrix(collisions.sample(250))
```

<meta name="referrer" content="no-referrer" />
{% asset_img output_1_2.png 矩阵图 %}

白色的为缺失

```python
import pandas as pd

null_pattern = (np.random.random(1000).reshape((50, 20)) > 0.5).astype(bool)
null_pattern = pd.DataFrame(null_pattern).replace({False: None})
msno.matrix(null_pattern.set_index(pd.period_range('1/1/2011', '2/1/2015', freq='M')), freq='BQ')
```

<meta name="referrer" content="no-referrer" />
{% asset_img output_3_1.png 矩阵图 %}



```Python

```


```Python

```


# 参考资料
- [GitHub-missingno](https://github.com/ResidentMario/missingno)
- [缺失值可视化处理--missingno](https://blog.csdn.net/Andy_shenzl/article/details/81633356)