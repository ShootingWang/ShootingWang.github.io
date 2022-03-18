---
title: Data Cleaning Projects
date: 2020-04-30 15:08:01
tags: [Data,Python,R,Projects]
categories: Data Scientist
index_img: /img/data_cleaning.jpg
mathjax: true
math: true
toc: true
#top: true
---

<center>Data Cleansing Projects 汇总</center>

<!--more-->
# Projects
- {% post_link 数据清洗-Titanic Kaggle | Titanic  %}
- {% post_link 数据清洗-天池-二手车交易数据 天池 | 二手车交易数据 %}
- {% post_link 数据清洗-kaggle-MedicalCostPersonal Kaggle | Medical Cost Personal %}
- {% post_link 数据清洗-kaggle-HousePrice Kaggle | House Prices:Advanced Regression Techniques %}

# 基本操作
## 缺失值
### 可视化缺失值
- 直接统计

```Python
import pandas as pd

## 读取数据
train_data = pd.read_csv("used_car_train_20200313.csv", sep=" ")
missing = train_data.isnull().sum()  ## 统计每列的缺失值样本数
missing = missing[missing > 0]
missing.sort_values(inplace=True)
missing.plot.bar()
```

<meta name="referrer" content="no-referrer" />
{% asset_img na_pic.png 将缺失值数量可视化 %}

- {% post_link python-missingno missingno %}

```Python
import missingno as msno

msno.matrix(train_data.sample(250))
```

<meta name="referrer" content="no-referrer" />
{% asset_img na_pic2.png 将缺失值数量可视化 %}

# 参考资料
- [首页缩略图](https://cn.bing.com/images/search?view=detailV2&ccid=gpCSzLGU&id=1898D7F14E27BFCC00B895A307ECC5F04313DE94&thid=OIP.gpCSzLGUdIzn2Pkx-1iVFAHaEN&mediaurl=https%3a%2f%2fpublic.tableau.com%2fs%2fsites%2fdefault%2ffiles%2fmedia%2fdata-cleaning-thumb2_20.jpg&exph=766&expw=1349&q=data+cleaning&simid=608010194433082183&selectedIndex=175)
- [数据分析](https://tianchi.aliyun.com/notebook-ai/detail?spm=5176.12281978.0.0.68021b43hxXs9w&postId=95457)