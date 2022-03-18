---
title: Python | Pandas
date: 2020-05-12 20:51:22
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



# Pandas
Pandas是Python中基于提供高性能易用数据类型和分析工具的第三方库。

- 其基于Numpy实现
- 名称来源于面板数据（Panel Data）和数据分析（Data Analysis）
- 提供2种最基本的数据类型：Series（一维数组）和DataFrame（二维数组）

安装pandas库：（在Anaconda Prompt中实现）
```python
pip install pandas
```

查看Pandas的版本：
```python
import pandas as pd 
pd.__version__ ## 注意version前后各有2个下划线
```

> 常用参考手册：
> - [Pandas用户手册（英文版）](https://pandas.pydata.org/docs/user_guide/index.html)
> - [Pandas: Powerful Python Data Analysis Toolkit（PDF版）](https://pandas.pydata.org/docs/pandas.pdf)




# concat()
拼接DataFrame；将数据根据不同的轴作简单的融合
`pd.concat(objs, axis=0, join='outer', join_axes=None, ignore_index=False,
       keys=None, levels=None, names=None, verify_integrity=False)`
- `axis`：需要合并的轴
  - `=0`是行（列对齐，合并行）
  - `=1`是列（行对齐，合并列）
- `join`：连接的方式
 - `inner`：得到多表的交集
 - `outer`：得到多表的并集
- `ignore_index`：无视表的索引，会自动根据列字段对齐，然后进行合并
- `keys`：说明数据源（分组键）

```python
df1 = pd.DataFrame({'A': ['A0', 'A1', 'A2', 'A3'],
                    'B': ['B0', 'B1', 'B2', 'B3'],
                    'C': ['C0', 'C1', 'C2', 'C3'],
                    'D': ['D0', 'D1', 'D2', 'D3']})
df2 = pd.DataFrame({'A': ['A4', 'A5', 'A6', 'A7'],
                    'B': ['B4', 'B5', 'B6', 'B7'],
                    'C': ['C4', 'C5', 'C6', 'C7'],
                    'D': ['D4', 'D5', 'D6', 'D7']})
df3 = pd.DataFrame({'A': ['A8', 'A9', 'A10', 'A11'],
                    'B': ['B8', 'B9', 'B10', 'B11'],
                    'C': ['C8', 'C9', 'C10', 'C11'],
                    'D': ['D8', 'D9', 'D10', 'D11']})
frames = [df1, df2, df3]
result = pd.concat(frames)  ## 默认按列对齐，拼接行
result
#     A    B    C    D
#0   A0   B0   C0   D0
#1   A1   B1   C1   D1
#2   A2   B2   C2   D2
#3   A3   B3   C3   D3
#0   A4   B4   C4   D4
#1   A5   B5   C5   D5
#2   A6   B6   C6   D6
#3   A7   B7   C7   D7
#0   A8   B8   C8   D8
#1   A9   B9   C9   D9
#2  A10  B10  C10  D10
#3  A11  B11  C11  D11
```

添加参数`keys`，说明数据源（分组键）
```python
result = pd.concat(frames, keys=['x', 'y', 'z'])
result
#       A    B    C    D
#x 0   A0   B0   C0   D0
#  1   A1   B1   C1   D1
#  2   A2   B2   C2   D2
#  3   A3   B3   C3   D3
#y 0   A4   B4   C4   D4
#  1   A5   B5   C5   D5
#  2   A6   B6   C6   D6
#  3   A7   B7   C7   D7
#z 0   A8   B8   C8   D8
#  1   A9   B9   C9   D9
#  2  A10  B10  C10  D10
#  3  A11  B11  C11  D11
```

也可通过字典来传入分组键
```python
pieces = {'x': df1, 'y': df2, 'z': df3}
pd.concat(pieces)
#       A    B    C    D
#x 0   A0   B0   C0   D0
#  1   A1   B1   C1   D1
#  2   A2   B2   C2   D2
#  3   A3   B3   C3   D3
#y 0   A4   B4   C4   D4
#  1   A5   B5   C5   D5
#  2   A6   B6   C6   D6
#  3   A7   B7   C7   D7
#z 0   A8   B8   C8   D8
#  1   A9   B9   C9   D9
#  2  A10  B10  C10  D10
#  3  A11  B11  C11  D11
```


```python
df1 = pd.DataFrame({'A': ['A0', 'A1', 'A2', 'A3'],
                    'B': ['B0', 'B1', 'B2', 'B3'],
                    'C': ['C0', 'C1', 'C2', 'C3'],
                    'D': ['D0', 'D1', 'D2', 'D3']})
df4 = pd.DataFrame({'B': ['B2', 'B3', 'B6', 'B7'],
                    'D': ['D2', 'D3', 'D6', 'D7'],
                    'F': ['F2', 'F3', 'F6', 'F7']})
pd.concat([df1, df4], axis=1)  ## 行对齐，合并列
#    A   B   C   D   B   D   F
#0  A0  B0  C0  D0  B2  D2  F2
#1  A1  B1  C1  D1  B3  D3  F3
#2  A2  B2  C2  D2  B6  D6  F6
#3  A3  B3  C3  D3  B7  D7  F7
```

`join=inner`：返回index的交集
```python
df4 = pd.DataFrame({'B': ['B2', 'B3', 'B6', 'B7'],
                    'D': ['D2', 'D3', 'D6', 'D7'],
                    'F': ['F2', 'F3', 'F6', 'F7']})
df4.index = [2,3,6,7]
df1
#    A   B   C   D
#0  A0  B0  C0  D0
#1  A1  B1  C1  D1
#2  A2  B2  C2  D2
#3  A3  B3  C3  D3
df4
#    B   D   F
#2  B2  D2  F2
#3  B3  D3  F3
#6  B6  D6  F6
#7  B7  D7  F7
pd.concat([df1, df4], axis=1, join='inner')
#    A   B   C   D   B   D   F
#2  A2  B2  C2  D2  B2  D2  F2
#3  A3  B3  C3  D3  B3  D3  F3
```

`join_axes`指定用于对齐的轴
```python
df1
#    A   B   C   D
#0  A0  B0  C0  D0
#1  A1  B1  C1  D1
#2  A2  B2  C2  D2
#3  A3  B3  C3  D3
df4
#    B   D   F
#2  B2  D2  F2
#3  B3  D3  F3
#6  B6  D6  F6
#7  B7  D7  F7
pd.concat([df1, df4], axis=1, join_axes=[df1.index])
#    A   B   C   D    B    D    F
#0  A0  B0  C0  D0  NaN  NaN  NaN
#1  A1  B1  C1  D1  NaN  NaN  NaN
#2  A2  B2  C2  D2   B2   D2   F2
#3  A3  B3  C3  D3   B3   D3   F3

pd.concat([df1, df4], axis=1, join_axes=[df4.index])
#     A    B    C    D   B   D   F
#2   A2   B2   C2   D2  B2  D2  F2
#3   A3   B3   C3   D3  B3  D3  F3
#6  NaN  NaN  NaN  NaN  B6  D6  F6
#7  NaN  NaN  NaN  NaN  B7  D7  F7
```

`ignore_index`：无视表的索引，会自动根据列字段对齐，然后进行合并
```python
df1
#    A   B   C   D
#0  A0  B0  C0  D0
#1  A1  B1  C1  D1
#2  A2  B2  C2  D2
#3  A3  B3  C3  D3
df4
#    B   D   F
#2  B2  D2  F2
#3  B3  D3  F3
#6  B6  D6  F6
#7  B7  D7  F7
pd.concat([df1, df4], axis=1, ignore_index=True)
#     0    1    2    3    4    5    6
#0   A0   B0   C0   D0  NaN  NaN  NaN
#1   A1   B1   C1   D1  NaN  NaN  NaN
#2   A2   B2   C2   D2   B2   D2   F2
#3   A3   B3   C3   D3   B3   D3   F3
#6  NaN  NaN  NaN  NaN   B6   D6   F6
#7  NaN  NaN  NaN  NaN   B7   D7   F7
```


# cut()
将数组按区间划分

```Python
cut(数据数组, 面元数组)  # 将数据数组按区间划分
cut(数据数组, 面元个数, precision=2)  # 将数据数组均分；每个面元的间距一致
# precision=2表示保留2位小数

import pandas as pd

ages = [23, 45, 21, 15, 24, 54, 36, 48, 57, 86, 61, 45]  # 数据数组
cat2 = pd.cut(ages, 5)  # 不指定面元划分，只指定面元个数
cat2
# [(14.929, 29.2], (43.4, 57.6], (14.929, 29.2], (14.929, 29.2], (14.929, 29.2], ..., (43.4, 57.6], (43.4, 57.6], (71.8, 86.0], (57.6, 71.8], (43.4, 57.6]]
# Length: 12
# Categories (5, interval[float64]): [(14.929, 29.2] < (29.2, 43.4] < (43.4, 57.6] < (57.6, 71.8] < (71.8, 86.0]]
```

指定面元标签：
```Python
import numpy as np
import pandas as pd

ages = np.random.randint(18, 89, size = 30)
ages
# array([75, 66,  1, 25, 38, 53, 29, 50, 88, 54, 87, 93, 53, 16, 51, 34, 69,
#        60, 70, 58, 95, 76, 52,  8, 20, 48, 61,  1, 11, 18])
df = pd.DataFrame()
df['age'] = ages
# 一般来说：0（初生）-6岁为婴幼儿；7-12岁为少儿；
# 13-17岁为青少年；18-45岁为青年；46-69岁为中年；>69岁为老年。
bins = [0, 6, 12, 18, 45, 69, 100]

df['age_bin'] = pd.cut(df['age'], bins)
df.head()
#    age    age_bin
# 0   75  (69, 100]
# 1   66   (45, 69]
# 2    1     (0, 6]
# 3   25   (18, 45]
# 4   38   (18, 45]

# 给区间添加标签
df['age_category'] = pd.cut(df['age'], bins, 
  labels=['婴幼儿', '少儿', '青少年', '青年', '中年', '老年'])

df.head().append(df.tail())  # 查看前5行和最后5行
#     age    age_bin age_category
# 0    75  (69, 100]           老年
# 1    66   (45, 69]           中年
# 2     1     (0, 6]          婴幼儿
# 3    25   (18, 45]           青年
# 4    38   (18, 45]           青年
# 25   48   (45, 69]           中年
# 26   61   (45, 69]           中年
# 27    1     (0, 6]          婴幼儿
# 28   11    (6, 12]           少儿
# 29   18   (12, 18]          青少年
```

## .codes
查看数据对应的面元编号
```Python
import pandas as pd

ages = [23, 45, 21, 15, 24, 54, 36, 48, 57, 86, 61, 45]  # 数据数组
bins = [0,18, 30, 40, 50, 65,100]  # 面元数组

cat = pd.cut(ages, bins)
cat  # 返回每个数据对应的面元
# [(18, 30], (40, 50], (18, 30], (0, 18], (18, 30], ..., (40, 50], (50, 65], (65, 100], (50, 65], (40, 50]]
# Length: 12
# Categories (6, interval[int64]): [(0, 18] < (18, 30] < (30, 40] < (40, 50] < (50, 65] < (65, 100]]
```

## .value_counts()
不同面元里含有的数据个数
```Python
cat.value_counts()  # 不同面元里含有的数据个数
# (0, 18]      1
# (18, 30]     3
# (30, 40]     1
# (40, 50]     3
# (50, 65]     3
# (65, 100]    1
# dtype: int64
```

# DataFrame()
DataFrame是二维数组（即数据框），是表格型数据结构
- 包含一组有序的列，每列可以是不同的值类型
- DataFrame有行索引和列索引
- 可以看作是由多个Series组成的字典
- 若没有指定索引，会自动加上索引（从0开始）

`DataFrame()`：创建二维数组（数据框）。
`DataFrame(data=None, index=None, columns=None, dtype=None, copy=False)`
- `index` ：指定行索引（可以缺省）
- `columns`：指定列名

```python
## 结合字典创建一个DataFrame：
import numpy as np
import pandas as pd
from pandas import DataFrame

data1 = {'Name':['Jane', 'John', 'Mike', 'Jack'],
        'Gender':['F', 'M', 'M', 'M'],
        'Age':[18, 19, 21, 17],
        'Grade':[89, 90, 87, 76]}
df1 = DataFrame(data1)
```

```python
## 使用嵌套字典创建DataFrame：
## 嵌套字典：外层字典的键作为列名（columns）；内层字典的键作为行索引（index）
dict2 = {'Neth':{2010:3.4, 2013:5.2},
        'Norway':{2010:2.5, 2011:3.4, 2012:2.8, 2013:4.3}
        }
df4 = DataFrame(dict2)
```

```python
## 若指定参数columns，则按columns有序排列：
df2 = DataFrame(data1, columns=['Name', 'Grade', 'Gender', 'Age'])
```

访问某一列：
```python
## 访问DataFrame的某一列（2种方法）：（访问df1的“Age”列）
df1['Age']
#0    18
#1    19
#2    21
#3    17
#Name: Age, dtype: int64

df1.Age ## 结果同上
```

访问某几列：
```python
## 访问DataFrame的某几列：
df1[['Name', 'Age', 'Gender']]
```

## at[]
通过**行标签**和**列标签**，选取DataFrame中的某个元素
```python
dict1 = {'Name':['Mike', 'John', 'Maria', 'Jack'],
        'Gender':['M', 'M', 'F', 'M'],
        'Age':[21, 19, 24, 18]}
df1 = DataFrame(data=dict1)
df1.at[2, 'Name']
#'Maria'
```

## .columns
查看DataFrame的列名
```python
df1.columns
#Index(['Name', 'Gender', 'Age'], dtype='object')
```

```python

```

## del
删除列数据
```python
del df3['Female']
```

## .describe
查看数据概况
```python
df3.describe
#<bound method NDFrame.describe of        Name Gender   Age  Grade  Class  Female
#one    Jane      F  16.0     91      2    True
#two    John      M   NaN     93      3   False
#three  Mike      M  25.0     89      1   False
#four   Jack      M   NaN     79      2   False>
```

## .describe()
数据DataFrame的描述性统计
- `count`：计数
- `mean`：均值
- `std`：标准差
- `min`：最小值
- `25%`：25\%分位数
- `50%`：中位数
- `75%`：75\%分位数
- `max`：最大值

```python
df.describe()
```

## .drop()
删除行或列
- 默认按行删除（即axis=0）
- axis=1 或 axis="columns" 表示按列删除
- 若要直接修改DataFrame，不保持原样，则令参数 inplace=True。

按行删除：
```python
df1.drop([2])  ## 删除行索引为2的行，返回被删除行后的DataFrame
## df1并未改变

## 删除多行：
df1.drop([1,3])
```

按列删除：
```python
df1.drop(['Gender', 'Age'], axis=1)
#或
df1.drop(['Gender', 'Age'], axis='columns')
## df1并未改变
```

```python
## 直接对DataFrame操作
df1.drop(['Gender'], axis=1, inplace=True)
```


## dropna()
删除含缺失值的行

## fillna()
填补缺失值
```python
df.fillna(0) ## 用0填补缺失值
```

## head()
预览前5行
- 可以通过赋值`df.head(n)`中n来调整预览的行数

```python
df.head()
```

## .iloc[]
通过**行号**索引行数据
```python
## 在创建DataFrame时指定行标签：
data1 = {'Name':['Jane', 'John', 'Mike', 'Jack'],
        'Gender':['F', 'M', 'M', 'M'],
        'Age':[18, 19, 21, 17],
        'Grade':[89, 90, 87, 76]}
df3 = DataFrame(data=data1, index=['one', 'two', 'three', 'four']
df3.iloc[2]
#Name      Mike
#Gender       M
#Age         21
#Grade       87
#Name: three, dtype: object

df3.iloc[[0,2]]
```

获取多行数据：
- 使用行号索引时，左闭右开
```python
## 下例中仅获取0、1、2行
df3.iloc[0:3]
```

获取单列数据：
```python
df3.iloc[:, 3]
#one      89
#two      90
#three    87
#four     76
#Name: Grade, dtype: int64
```

获取多列数据：
```python
df3.iloc[:, 1:3]
```

## .index
查看DataFrame的行索引/标签
```python
df3.index
#Index(['one', 'two', 'three', 'four'], dtype='object')
```

## isin()
用于选择DataFrame中符合条件的行

```python
import pandas as pd
import numpy as np

dates = pd.date_range("20130101", periods=6)
dates
# DatetimeIndex(['2013-01-01', '2013-01-02', '2013-01-03', '2013-01-04',
#                '2013-01-05', '2013-01-06'],
#               dtype='datetime64[ns]', freq='D')
df = pd.DataFrame(np.random.randn(6, 4), index=dates, columns=list("ABCD"))
df["E"] = ["one", "one", "two", "three", "four", "three"]
df.head()
#     A   B   C   D   E
# 2013-01-01 0.522782 -0.908932 0.503061 -1.133839 one
# 2013-01-02 -0.137954 -0.801103 0.416220 1.085829 one
# 2013-01-03 0.782363 0.044841 0.106608 1.169290 two
# 2013-01-04 1.143327 -2.648602 -1.410805 -0.639302 three
# 2013-01-05 -0.174496 0.810790 0.566061 -0.230608 four
df[df["E"].isin(["two", "four"])]
#     A   B   C   D   E
# 2013-01-03 0.782363 0.044841 0.106608 1.169290 two
# 2013-01-05 -0.174496 0.810790 0.566061 -0.230608 four
```


## isna()
判断数据是否缺失

```python
pd.isna(df2["one"])
df.isna()
```

注意：
```python
None == None
# True

np.nan == np.nan
# False
# 因此，不能使用 == np.nan来判断数据是否缺失
```

对于`datetime64`类型数据，`NaT`表示缺失值。

## .ix[]
通过**行号** / **行标签** 索引行数据。是`.loc[]`和`.iloc[]`的结合。
- 已弃用

```python
df3.ix[3]
#Name      Jack
#Gender       M
#Age         17
#Grade       76
#Name: four, dtype: object

df3.ix['four']
#Name      Jack
#Gender       M
#Age         17
#Grade       76
#Name: four, dtype: object
```


## .loc[]
通过**行标签**索引行数据。
```python
df3.loc['three']  ## 获取行标签为“three”的行数据
#Name      Mike
#Gender       M
#Age         21
#Grade       87
#Name: three, dtype: object

df3.loc[['two', 'four']]  ## 获取行标签为“two”和“four”的行数据
```

获取多行数据：
- 使用行标签索引时，两边都是闭的
```python
## 获取多行数据：
df3.loc['one':'three']
```

获取单列数据：
```python
df3.loc[:, 'Age']  ##不显示列名
df3.loc[:, ['Age']]  ##显示列名
```

获取多列数据：
```python
df3.loc[:, 'Age':]
```


## melt()
`pivot_table()`的反向操作
- 参数`id_vars`：不变的列
- 参数`var_name`：“列转行”的列名

```python
cheese = pd.DataFrame(
    {
        "first": ["John", "Mary"],
        "last": ["Doe", "Bo"],
        "height": [5.5, 6.0],
        "weight": [130, 150],
    }
)
cheese
# 	first	last	height	weight
# 0	John	Doe	5.5	130
# 1	Mary	Bo	6.0	150
cheese.melt(id_vars=["first", "last"])
# 	first	last	variable	value
# 0	John	Doe	height	5.5
# 1	Mary	Bo	height	6.0
# 2	John	Doe	weight	130.0
# 3	Mary	Bo	weight	150.0

cheese.melt(id_vars=["first", "last"], var_name="quantity")
#  first	last	quantity	value
# 0	John	Doe	height	5.5
# 1	Mary	Bo	height	6.0
# 2	John	Doe	weight	130.0
# 3	Mary	Bo	weight	150.0
```





## merge()
合并DataFrame
- 交集、并集

```python
## 取交集
import pandas as pd

df1 = pd.DataFrame([['a', 10, '男'], ['b', 11, '女'], ['a', 10, '女']], 
                   columns=['name', 'age', 'gender'])
df1
# name age gender
# 0 a 10 男
# 1 b 11 女
# 2 a 10 女

df2 = pd.DataFrame([['a', 10, '男']],
                   columns=['name', 'age', 'gender'])
df2
# name age gender
# 0 a 10 男

df = pd.merge(df1, df2, on=['name', 'age'])
df
#  name age gender_x gender_y
# 0 a 10 男 男
# 1 a 10 女 男

df = pd.merge(df1, df2, on=['name', 'age', 'gender'])
df
#  name age gender
# 0 a 10 男

## 取并集
df = pd.merge(df1, df2, on=['name', 'age', 'gender'], how='outer')
df
#  name age gender
# 0 a 10 男
# 1 b 11 女
# 2 a 10 女
```



## notna()
判断数据是否非缺失

```python
df1['B'].notna()
```


## sort_index()
按照轴排序
- 默认升序

```python
df.sort_index(axis=1, ascending=False) ## 按列名降序排序

df.sort_index(by='B') ## 按照B列的值升序排序
```

## .T
转置

## tail()
预览最后5行
- 可以通过赋值`df.tail(n)`中n来调整预览的行数

```python
df.tail()
```

## to_numpy()
将DataFrame转换为数组Array
- 输出不包括DataFrame的列名、索引

## .values
查看DataFrame的数据
```python
df3.values
#array([['Jane', 'F', 16.0, 91, 2, True],
#       ['John', 'M', nan, 93, 3, False],
#       ['Mike', 'M', 25.0, 89, 1, False],
#       ['Jack', 'M', nan, 79, 2, False]], dtype=object)
```

## 修改列值
```python
df3['Grade'] = (91, 93, 89, 79)

## 结合Series精准插补数据/修改列值：
df3['Age'] = pd.Series([16, 25], index=['one', 'three'])
```

## 添加新列
```python
## 直接为不存在的列赋值：
df3['Class'] = (2, 3, 1, 2)

df3['Female'] = df3.Gender=='F'
```

```python

```

```python

```

# date_range()
取一定的日期范围
- 参数`freq`：
  - `freq='D'`：按天
  - `freq='H'`：按小时
  - `freq=Q-DEC`：季度的最后一天

```python
import pandas as pd

dt = pd.date_range('2021-08-01', periods=5, freq='D')
dt
# DatetimeIndex(['2021-08-01', '2021-08-02', '2021-08-03', '2021-08-04',
#                '2021-08-05'],
#               dtype='datetime64[ns]', freq='D')

hours = pd.date_range('2021-08-01', periods=5, freq='H')
hours
# DatetimeIndex(['2021-08-01 00:00:00', '2021-08-01 01:00:00',
#                '2021-08-01 02:00:00', '2021-08-01 03:00:00',
#                '2021-08-01 04:00:00'],
#               dtype='datetime64[ns]', freq='H')

quarts = pd.date_range('2021-08-01', periods=5, freq='Q')
quarts
# DatetimeIndex(['2021-09-30', '2021-12-31', '2022-03-31', '2022-06-30',
#                '2022-09-30'],
#               dtype='datetime64[ns]', freq='Q-DEC')

qa = pd.date_range('2021-08-01', periods=5, freq='2M')
qa
# DatetimeIndex(['2021-08-31', '2021-10-31', '2021-12-31', '2022-02-28',
#                '2022-04-30'],
#               dtype='datetime64[ns]', freq='2M')
```



# factorize()
将类别变量/特征数字化。

Pandas中的`factorize()`可以将类别型变量转换为一组数字，相同的类别对应相同的数字。
`factorize()`函数的返回值是一个元组（tuple），元组中含有两个元素:
- 第一个元素是array，其中的元素是类别型变量对应的数字；
- 第二个元素是Index，是所有没有重复的类别元素。

以iris鸢尾花数据为例：
- iris数据下载网址：https://archive.ics.uci.edu/ml/datasets/Iris
- 鸢尾花数据共有3个品种

```Python
import pandas as pd
from urllib.request import urlretrieve

urlretrieve(url="https://archive.ics.uci.edu/ml/machine-learning-databases/iris/iris.data",
           filename="iris.data")
iris = pd.read_csv("iris.data", header=None)
iris.columns = ['Sepal_length', 'Sepal_width', 'Petal_length', 'Petal_width', 'class']
iris.head()
#   Sepal_length  Sepal_width  Petal_length  Petal_width        class
#0           5.1          3.5           1.4          0.2  Iris-setosa
#1           4.9          3.0           1.4          0.2  Iris-setosa
#2           4.7          3.2           1.3          0.2  Iris-setosa
#3           4.6          3.1           1.5          0.2  Iris-setosa
#4           5.0          3.6           1.4          0.2  Iris-setosa

iris['class'] = pd.factorize(iris['class'])[0]
iris.head()
#   Sepal_length  Sepal_width  Petal_length  Petal_width  class
#0           5.1          3.5           1.4          0.2      0
#1           4.9          3.0           1.4          0.2      0
#2           4.7          3.2           1.3          0.2      0
#3           4.6          3.1           1.5          0.2      0
#4           5.0          3.6           1.4          0.2      0

iris.tail()
#     Sepal_length  Sepal_width  Petal_length  Petal_width  class
#145           6.7          3.0           5.2          2.3      2
#146           6.3          2.5           5.0          1.9      2
#147           6.5          3.0           5.2          2.0      2
#148           6.2          3.4           5.4          2.3      2
#149           5.9          3.0           5.1          1.8      2
```


# get_dummies()
将类别变量one-hot encoding

```python
import pandas as pd
import numpy as np

df = pd.DataFrame({'A': ['F', 'F', 'M', 'F', 'M'],
                   'B': ['one', 'three', 'two', np.nan, 'one'],
                   'C': ['1', '2', '1', '3', '2']})
print(df)
#   A      B  C
#0  F    one  1
#1  F  three  2
#2  M    two  1
#3  F    NaN  3
#4  M    one  2

print(pd.get_dummies(df))  ## 默认na值不作为一类
#   A_F  A_M  B_one  B_three  B_two  C_1  C_2  C_3
#0    1    0      1        0      0    1    0    0
#1    1    0      0        1      0    0    1    0
#2    0    1      0        0      1    1    0    0
#3    1    0      0        0      0    0    0    1
#4    0    1      1        0      0    0    1    0

## 参数prefix指定新列名的前缀
print(pd.get_dummies(df, prefix=['col1', 'col2', 'col3']))
#   col1_F  col1_M  col2_one  col2_three  col2_two  col3_1  col3_2  col3_3
#0       1       0         1           0         0       1       0       0
#1       1       0         0           1         0       0       1       0
#2       0       1         0           0         1       1       0       0
#3       1       0         0           0         0       0       0       1
#4       0       1         1           0         0       0       1       0

## 参数dummy_na表示是否将na作为单独的一类
print(pd.get_dummies(df['B'], dummy_na=True))
#   one  three  two  NaN
#0    1      0    0    0
#1    0      1    0    0
#2    0      0    1    0
#3    0      0    0    1
#4    1      0    0    0
```

# isnull()
判断目标中是否是缺失值/缺失数据
- 若为缺失数据，则返回True；否则返回False。
```Python
## 检查Series中是否存在缺失值/缺失数据（即NaN/NA）
import pandas as pd

dict1 = {'new':2.3, 'old':3.5, 'then':6.5, 'obj':8.4}
ind1 = ['new', 'after', 'old', 'then', 'obj', 'six']
s4 = Series(dict1, index=ind1)
pd.isnull(s4)
#new      False
#after     True
#old      False
#then     False
#obj      False
#six       True
#dtype: bool
```

# notnull()
判断是否不是缺失数据
- 若为缺失数据，则返回False；否则返回True。
- 与`isnull()`结果完全相反。
```Python
import pandas as pd

dict1 = {'new':2.3, 'old':3.5, 'then':6.5, 'obj':8.4}
ind1 = ['new', 'after', 'old', 'then', 'obj', 'six']
s4 = Series(dict1, index=ind1)
pd.notnull(s4)
#new       True
#after    False
#old       True
#then      True
#obj       True
#six      False
#dtype: bool
```


# pivot_table()
绘制透视表，进行分组统计。
`pivot_table(data, values=None, index=None, columns=None, aggfunc='mean', fill_value=None, margins=False, dropna=True, margins_name='All')`
- 参数`aggfunc`：决定统计类型
  - `mean`：计算平均值
- 参数`fill_value`：缺失值填补
- 参数`index`：指定“行”维度
- 参数`columns`：指定“列”维度
- 使用`reset_index()`可以将MultiIndex变换为每行Index

```python
import pandas as pd
import numpy as np

df = pd.DataFrame({"A": ["foo", "foo", "foo", "foo", "foo",
                         "bar", "bar", "bar", "bar"],
                   "B": ["one", "one", "one", "two", "two",
                         "one", "one", "two", "two"],
                   "C": ["small", "large", "large", "small",
                         "small", "large", "small", "small",
                         "large"],
                   "D": [1, 2, 2, 3, 3, 4, 5, 6, 7],
                   "E": [2, 4, 5, 5, 6, 6, 8, 9, 9]})
df.head()
#   A	B	C	D	E
# 0	foo	one	small	1	2
# 1	foo	one	large	2	4
# 2	foo	one	large	2	5
# 3	foo	two	small	3	5
# 4	foo	two	small	3	6
table = pd.pivot_table(df, values='D', index=['A', 'B'],
                    columns=['C'], aggfunc=np.sum)
## 行按照A、B列聚合；列按照C列聚合；
## 统计D列的和（sum）
table
# 	  C	  large	small
# A	  B		
# bar	one	4.0	  5.0
#     two	7.0	  6.0
# foo	one	4.0	  1.0
#     two	NaN	  6.0

table = pd.pivot_table(df, values=['D', 'E'], index=['A', 'C'],
                    aggfunc={'D': np.mean,
                             'E': np.mean})
## D、E两列分别对应不同的计算方式
table
# 		      D	        E
# A	  C		
# bar	large	5.500000	7.500000
#     small	5.500000	8.500000
# foo	large	2.000000	4.500000
#     small	2.333333	4.333333
```


# qcut()

```Python
qcut(数据数组, 面元个数)  # 将数据数组均分；每个面元的数据个数相等
```

```Python
cat3 = pd.qcut(ages, 5)
cat3
# [(14.999, 23.2], (39.6, 46.8], (14.999, 23.2], (14.999, 23.2], (23.2, 39.6], ..., (46.8, 56.4], (56.4, 86.0], (56.4, 86.0], (56.4, 86.0], (39.6, 46.8]]
# Length: 12
# Categories (5, interval[float64]): [(14.999, 23.2] < (23.2, 39.6] < (39.6, 46.8] < (46.8, 56.4] < (56.4, 86.0]]
cat3.value_counts()  # qcut()会尽可能保证每个面元包含的数据个数相等
# (14.999, 23.2]    3
# (23.2, 39.6]      2
# (39.6, 46.8]      2
# (46.8, 56.4]      2
# (56.4, 86.0]      3
# dtype: int64

cat4 = pd.qcut(ages, 3)  # 将ages均分成3份，每个面元有4个数据
cat4
# [(14.999, 32.0], (32.0, 50.0], (14.999, 32.0], (14.999, 32.0], (14.999, 32.0], ..., (32.0, 50.0], (50.0, 86.0], (50.0, 86.0], (50.0, 86.0], (32.0, 50.0]]
# Length: 12
# Categories (3, interval[float64]): [(14.999, 32.0] < (32.0, 50.0] < (50.0, 86.0]]
cat4.value_counts()
# (14.999, 32.0]    4
# (32.0, 50.0]      4
# (50.0, 86.0]      4
# dtype: int64
```

# Series()
Series是一维数组（即向量）。
- 由一组数组（Numpy数据类型）和一组与之相关的数据标签（索引）组成
- 表现形式：索引在左边，值在右边
- 索引可以不用指定，会自动创建一个从0开始、到n-1结束的整数型索引。（这里n是数据的长度）
- 可以通过Series的values属性和index属性获取其表现形式和索引对象
- Series对象本身及其索引（index）都有一个name属性
-  索引可以通过赋值进行修改。

`Series()`：创建一维数组。
`Series(data=None, index=None, dtype=None, name=None, copy=False, fastpath=False)`
- `data`：指定数据
- `index`：指定索引（可以缺省）
- `name`：指定数组名称

```Python
## 创建Series
import numpy as np
import pandas as pd
from pandas import Series, DataFrame

#s = Series([1, 3, 4, np.nan, 6.5, 8.9])
#print(s)
#0    1.0
#1    3.0
#2    4.0
#3    NaN
#4    6.5
#5    8.9
#dtype: float64
```

```Python
## 创建Series的时候指定特殊的index：
s2 = Series([2, 3.5, 6.3, 6, 7, np.nan], 
           index=['td', 'ad', 'rm', 'ed','way', 'moon'])
print(s2)
#td      2.0
#ad      3.5
#rm      6.3
#ed      6.0
#way     7.0
#moon    NaN
#dtype: float64
```

```Python
## 结合字典创建Series：
dict1 = {'new':2.3, 'old':3.5, 'then':6.5, 'obj':8.4}
s3 = Series(dict1)
s3
#new     2.3
#old     3.5
#then    6.5
#obj     8.4
#dtype: float64
```

```Python
## 将字典dict1按照索引ind1创建Series：
## 若ind1的索引比字典dict1的索引多，则与dict1相匹配的值会找到，并放置到相应的位置中；ind1多出的索引，则填充NaN。
ind1 = ['new', 'after', 'old', 'then', 'obj', 'six']
s4 = Series(dict1, index=ind1)
s4
#new      2.3
#after    NaN
#old      3.5
#then     6.5
#obj      8.4
#six      NaN
#dtype: float64
```

## .index
查看Series的索引
```Python
s.index
# RangeIndex(start=0, stop=6, step=1)

## 可以根据索引查看索引对应的值：
s2['rm']
#6.3
s2[['ed', 'way', 'moon']]
#ed      6.0
#way     7.0
#moon    NaN
#dtype: float64
s2[s2 > 4.3]  ## 查看s2中大于4.3的值
#rm     6.3
#ed     6.0
#way    7.0
#dtype: float64

## 判断是否存在某个索引：
'way' in s2

## 修改索引：
s4.index = ['one', 'two', 'three', 'four', 'five', 'six']
s4
#one      2.3
#two      NaN
#three    3.5
#four     6.5
#five     8.4
#six      NaN
#Name: New_name, dtype: float64
```

## .name
```Python
## name属性：
s4
#new      2.3
#after    NaN
#old      3.5
#then     6.5
#obj      8.4
#six      NaN
#dtype: float64
s4.name = 'New_name'
s4.index.name = 'Index_name'
s4
#Index_name
#new      2.3
#after    NaN
#old      3.5
#then     6.5
#obj      8.4
#six      NaN
#Name: New_name, dtype: float64
```


## .values
查看Series的值
```Python
s.values
# array([1. , 3. , 4. , nan, 6.5, 8.9])

## 判断是否存在某个值：
6.3 in s2.values
```


```Python

```



```Python

```






```Python

```



# 参考资料
- [python数据拼接: pd.concat](https://www.cnblogs.com/RB26DETT/p/11555099.html)
- [【运筹OR帷幄｜数据科学】Pandas实战教程系列电子书](https://github.com/zhouyanasd/or-pandas)