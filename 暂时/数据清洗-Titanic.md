---
title: Kaggle | Titanic
date: 2020-04-30 15:18:16
tags: [Data Cleaning]
hide: true
math: true
toc: true
---

<center></center>
<!--more-->

# Titanic数据集
数据来源：Kaggle
[点击下载](https://www.kaggle.com/c/titanic/data)

## 数据预览
```Python
import os

os.chdir("你的路径")  ##设置工作路径
os.getcwd()  ##读取当前工作路径
import pandas as pd

train_data = pd.read_csv("train.csv")
train_data.head()  ##查看前5行
```

```Python
train_data.describe()  ##变量的描述统计
```

```Python
train_data.isnull().sum().sort_values(ascending=False)  ##统计每列的缺失值个数
```

```Python
train_data = train_data.drop(columns=["Cabin", "Name", "PassengerId", "Ticket"])  ##去除缺失值较多的变量Cabin以及其他无用信息
train_data.head()
```

```Python
train_data = train_data.dropna(axis=0) ##保留没有空值的行
train_data.shape
```

未完待续
