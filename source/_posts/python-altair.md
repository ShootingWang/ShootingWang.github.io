---
title: Python | Altair
date: 2021-08-01 22:33:44
tags: [Python, 可视化]
categories: Python
mathjax: true
toc: true
hide: true
notshow: true
---

<center>A Grammar of Interactive Graphics</center>
<!--more-->

<meta name="referrer" content="no-referrer" />

# Altair
Altair是Python的一个可视化库，由华盛顿大学的数据科学家Jake Vanderplas编写。
> - 官网：https://altair-viz.github.io/
> - Example Gallery：https://altair-viz.github.io/gallery/index.html

安装：
```python
pip install -U altair
```

Altair中的基本对象是 Chart，它将数据框（DataFrame）作为单个参数。

Chart有三个基本方法：
- 数据data
- 标记mark ：可以让用户在图中以不同形状来表示数据点（如实心点、空心圆、方块等）
- 编码encode ：指定图像的具体内容

> 标记和编码决定着绘制图表的样式

常用的**编码encode**有：
- `x`：x轴数值
- `y`：y轴数值
- `color`：标记点的颜色
- `opacity`：标记点的透明度
- `shape`：标记点的形状
- `size`：标记点的大小
- `row`：按行分布图片
- `column`：按列分布图片

# 绘图
## 面积图 mark_area()
```python
import altair as alt
from vega_datasets import data

source = data.iowa_electricity()  ## 导入数据

alt.Chart(source).mark_area().encode(
    x="year:T",  ## T是指数据类型为Temporal，表示时间或日期数据
    y="net_generation:Q",  ## Q是指数据类型为Quantitative，表示连续/实数数据
    color="source:N"  ## N是指数据类型为Nominal，表示定性数据（离散、无序）
)
```

{% asset_img 210802面积图.png 面积图 %}

## 条形图 mark_bar()
mark_bar() ：绘制条形图

```python
import pandas as pd

data = pd.DataFrame({
    'a': list('AAABBBBDDDDEE'),
    'b': [2, 7, 4, 1, 2, 6, 8, 4, 7, 5, 12, 11, 10]
})  ## 创建一个数据框

import altair as alt
alt.Chart(data).mark_bar().encode(
    x='a',
    y='average(b)'
)
```

{% asset_img 210802条形图.png 条形图 %}


## 地图 mark_geoshape()
mark_geoshape() ：地理形状图

## mark_image()
mark_image() ：将图片绘制在坐标系中



## 线图 mark_line()

```python
import altair as alt
from vega_datasets import data

source = data.stocks()

alt.Chart(source).mark_line().encode(
    x='date',
    y='price',
    color='symbol'
)
```

{% asset_img 210802线图.png 线图 %}

## 散点图 mark_point()
`mark_point()` ：绘制散点图
- 如果只指定`mark_point()`，则画出来的只有一个点

```python
import pandas as pd

data = pd.DataFrame({
    'a': list('AAABBBBDDDDEE'),
    'b': [2, 7, 4, 1, 2, 6, 8, 4, 7, 5, 12, 11, 10]
})  ## 创建一个数据框

import altair as alt
chart = alt.Chart(data)  ## 定义Chart对象

alt.Chart(data).mark_point()  ## 画点图（没有指定x轴、y轴）
## 得到的图片仅一个点

alt.Chart(data).mark_point().encode(
    x='a' ##指定x轴
)
```

{% asset_img 210801散点图（仅指定x轴）.png 散点图（仅指定x轴） %}

```python
alt.Chart(data).mark_point().encode(
    x='a',  ##指定x轴为变量a
    y='b'  ##指定y轴为变量b
)
```

{% asset_img 210801散点图（指定xy轴）.png 散点图（指定xy轴） %}

```python
## 某一维度为指定变量的均值
alt.Chart(data).mark_point().encode(
    x='average(b)',  ## x轴数据为变量b的均值
    y='a'
)
```

{% asset_img 210801散点图（均值）.png 散点图（均值） %}

```python
## 参考：https://github.com/altair-viz/altair_notebooks
import altair as alt
from vega_datasets import data

cars = data.cars()  ## 载入汽车数据集cars

alt.Chart(cars).mark_point().encode(
    x='Horsepower',  ## Horsepower作为x轴数据
    y='Miles_per_Gallon',  ## Miles_per_Gallon作为y轴数据
    color='Origin',  ## 以变量Origin（原产国）作为第三维度-颜色
).interactive()
```

{% asset_img 210801散点图.png 散点图  %}

点击“…”可选择将图片保存为PNG图片格式；鼠标移至图片区域，向下滑动鼠标可缩小数据点，向上滑动鼠标可放大数据点。

## save()
save() ：将图片保存为指定格式

```python
## 将图片保存为html文件
chart = alt.Chart(data).mark_bar().encode(
    x='a',
    y='average(b)',
)
chart.save('chart.html')
```

## to_json()
to_json() ：输出为JSON格式
- 可使用该函数将图表输出为JSON格式，再结合Echarts在博客上绘制动态图表（Echarts可参考{% post_link 可视化-Echarts %}）

```python
chart = alt.Chart(data).mark_bar().encode(
    x='a',
    y='average(b)',
)
print(chart.to_json())
# {
#   "$schema": "https://vega.github.io/schema/vega-lite/v4.0.2.json",
#   "config": {
#     "view": {
#       "continuousHeight": 300,
#       "continuousWidth": 400
#     }
#   },
#   "data": {
#     "name": "data-f1e68a455420a2e3bea861e0a72caf7d"
#   },
#   "datasets": {
#     "data-f1e68a455420a2e3bea861e0a72caf7d": [
#       {
#         "a": "A",
#         "b": 2
#       },
#       {
#         "a": "A",
#         "b": 7
#       },
#       {
#         "a": "A",
#         "b": 4
#       },
#       {
#         "a": "B",
#         "b": 1
#       },
#       {
#         "a": "B",
#         "b": 2
#       },
#       {
#         "a": "B",
#         "b": 6
#       },
#       {
#         "a": "B",
#         "b": 8
#       },
#       {
#         "a": "D",
#         "b": 4
#       },
#       {
#         "a": "D",
#         "b": 7
#       },
#       {
#         "a": "D",
#         "b": 5
#       },
#       {
#         "a": "D",
#         "b": 12
#       },
#       {
#         "a": "E",
#         "b": 11
#       },
#       {
#         "a": "E",
#         "b": 10
#       }
#     ]
#   },
#   "encoding": {
#     "x": {
#       "field": "a",
#       "type": "nominal"
#     },
#     "y": {
#       "aggregate": "average",
#       "field": "b",
#       "type": "quantitative"
#     }
#   },
#   "mark": "bar"
# }
```


## X(), Y()
`X()` ：设置x轴的聚合函数aggregate、field、数据类型type
`Y()` ：设置y轴的聚合函数aggregate、field、数据类型type
- 参数`aggregate`：设置聚合函数
- 参数`type`：轴数据类型
- 参数`title`：轴标题

```python
import pandas as pd
import altair as alt

data = pd.DataFrame({
    'a': list('AAABBBBDDDDEE'),
    'b': [2, 7, 4, 1, 2, 6, 8, 4, 7, 5, 12, 11, 10]
})  ## 创建一个数据框

alt.Chart(data).mark_bar().encode(
    alt.Y('a', type='nominal'),    ## y轴为定量变量（quantitative）
    alt.X('b', type='quantitative', aggregate='average')  ## x轴为名义变量（nominal）
)
```

{% asset_img 210802设置轴.png 设置轴 %}


```python
import pandas as pd
data = pd.DataFrame({
    'a': list('AAABBBBDDDDEE'),
    'b': [2, 7, 4, 1, 2, 6, 8, 4, 7, 5, 12, 11, 10]
})  ## 创建一个数据框

alt.Chart(data).mark_bar(color='firebrick').encode(
    alt.Y('a', title='category'),
    alt.X('average(b)', title='avg(b) by category')  
)
## 设置条形图颜色为firebrick
## 设置y轴标题为category
## 设置x轴标题为“avg(b) by category”
```

{% asset_img 210802设置轴2.png 设置轴 %}

# 参考资料
- [Altair官网](https://altair-viz.github.io/)
- [比Excel制图更强大，Python可视化工具Altair入门教程](https://mp.weixin.qq.com/s?__biz=MzIzNjc1NzUzMw==&mid=2247512075&idx=3&sn=bcd88ceeaad7a89a4895982b1b61fb9f&chksm=e8d00779dfa78e6fb71c176dce490b4a69f85458d4d2eb0ed781749cc833c926aa2b98f8cdfd&scene=21#wechat_redirect)
