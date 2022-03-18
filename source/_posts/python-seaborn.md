---
title: Python | Seaborn
date: 2020-12-19 10:11:39
tags: [Python,可视化]
categories: Python
mathjax: true
toc: true
index_img: /img/Python.png  ## 封面图片
hide: true
notshow: true
---

<center></center>
<!--more-->

<meta name="referrer" content="no-referrer" />

# Seaborn

> 常用使用手册：
> - [Seaborn User Guide](http://seaborn.pydata.org/tutorial.html)

Seaborn是Python中一个基于Matplotlib的数据可视化库。

功能：
- 面向数据集的API，用于检查多个变量之间的关系。
- 支持使用分类变量来显示样本或汇总统计数据。
- 可使用单变量或而二分量来比较数据子集。
- 针对不同类型的因变量进行线性回归模型的估计和绘制。
- 便于观察复杂数据集的整体结构。
- 用于构建多网格绘图，便于创造复杂的可视化图形。
- 简洁地控制matplotlib图形样式和内置主题。
- 可以选择调色板。

安装：（在Anaconda Prompt中运行安装命令）

```python
pip install seaborn
```

```python
## 一个简单的例子：
##该例中，以“total_bill”为横坐标，“tip”为纵坐标，“time”为分面依据，“smoker”为形状依据，“size”为大小依据。
import seaborn as sns   ## 加载seaborn库
sns.set()      ## 应用默认的seaborn主题、缩放、调色板
tips = sns.load_dataset("tips")  ##载入需要的数据tips

%matplotlib inline  
##为了使图片在jupyter中显示
##绘制简单的分面散点图
sns.relplot(x="total_bill", y="tip", col="time",
            hue="smoker", style="smoker", size="size",
            data=tips)
## 如果要保存图片：
#fig = sns.relplot(x="total_bill", y="tip", col="time",
#            hue="smoker", style="smoker", size="size",
#            data=tips)
#fig.savefig('分面散点图.png', dpi=400)
## dpi：每英寸点数 dots per inch
```

<meta name="referrer" content="no-referrer" />
{% asset_img 分面散点图.png 分面散点图  %}

## 中文乱码

```python
import matplotlib.pyplot as plt 
plt.rcParams['font.sans-serif'] = ['Arial Unicode MS']
```

# 高级函数
seaborn四大高级绘图函数：
1. catplot()
2. distplot()
3. lmplot()
4. relplot()

## catplot()
`catplot()` ：分类数据分布图（categorical）。
`catplot(x=None, y=None, hue=None, data=None, row=None, 
    col=None, col_wrap=None, estimator=<function mean at 0x000001AB0E436EA0>, 
    ci=95, n_boot=1000, units=None, order=None, 
    hue_order=None, row_order=None, col_order=None, 
    kind='strip', height=5, aspect=1, orient=None, 
    color=None, palette=None, legend=True, 
    legend_out=True, sharex=True, sharey=True, 
    margin_titles=False, facet_kws=None, **kwargs)`
- 参数 `kind` 可取值有：
 - `strip`：散点图，默认
 - `swarm`：分簇散点图
 - `violin`：小提琴图
 - `box`：箱线图
 - `boxen`：增强箱型图
 - `point`：点图
 - `bar`：条形图
 - `count`：计数条形图
- `strip`和`swarm`之间存在细微差异（见下面的例子），swarm图会区分位置较为相近的点（使点不重叠）。
- 参数 `aspect`：控制坐标轴的横纵比，默认为1
- 参数 `col_wrap`：表示输出的图形中每行panel的个数
- `.add_legend(title='')`：添加图例，并修改图例的标题
- `.set_axis_labels("横轴标签", "纵轴标签")`：修改横纵轴标签；若为空字符串，表示不显示轴标签

```python
ex = sns.load_dataset("exercise")
ex.head()

sns.catplot(x="time", y="pulse", hue="kind",
           col="diet", data=ex)
```

<meta name="referrer" content="no-referrer" />
{% asset_img 默认strip散点图.png 默认strip散点图  %}


```python
## 参数 aspect：控制坐标轴的横纵比，默认为1
## 若参数aspect=0.8（改变的是坐标轴的横纵轴比）：
sns.catplot(x="time", y="pulse", hue="kind",
           col="diet", data=ex, aspect=0.8)
```

<meta name="referrer" content="no-referrer" />
{% asset_img 修改坐标轴横纵比-默认strip散点图.png 修改坐标轴横纵比-默认strip散点图  %}

```python
ex = sns.load_dataset("exercise")
sns.catplot(x="time", y="pulse", hue="kind",
           col="kind", data=ex)
```

<meta name="referrer" content="no-referrer" />
{% asset_img 散点图.png 散点图  %}

```python
## 参数 col_wrap：表示输出的图形中每行panel的个数
ex = sns.load_dataset("exercise")
sns.catplot(x="time", y="pulse", hue="kind",
           col="kind", data=ex, col_wrap=2)
```

<meta name="referrer" content="no-referrer" />
{% asset_img 限制每行panel的个数-散点图.png 限制每行panel的个数-散点图  %}


```python
## 自定义图形的属性：
import seaborn as sns
import matplotlib.pyplot as plt

%matplotlib inline

tips = sns.load_dataset("tips")    ## 导入数据
g = sns.catplot(x="total_bill", y="day", hue="time", 
               height=4, aspect=1.5, kind="box",
               legend=False, data=tips)
##加上图例，并修改图例的标题
g.add_legend(title="Meal")
##设置横轴和纵轴的标签
g.set_axis_labels("Total bill($)", "")
##设置x轴的取值范围、y轴的刻度标签
g.set(xlim=(0, 60), 
      yticklabels=["Tuesday", "Friday", "Saturday", "Sunday"])
##设置图片尺寸（单位：英寸）
g.fig.set_size_inches(6.5, 3.5)
g.ax.set_xticks([5 ,15, 25, 35, 45, 55], minor=True);
plt.setp(g.ax.get_yticklabels(), rotation=30);
```

<meta name="referrer" content="no-referrer" />
{% asset_img 自定义图形属性.png 自定义图形属性 %}


### 散点图strip
```python
## catplot绘制默认strip：
import seaborn as sns

tips = sns.load_dataset("tips")   ## 加载数据
%matplotlib inline
sns.catplot(x="day", y="total_bill", hue="smoker",
           data=tips)
```

{% asset_img 散点图strip.png 散点图strip %}

### 散点图swarm
```python
## catplot绘制swarm：
import seaborn as sns

tips = sns.load_dataset("tips")
%matplotlib inline
sns.catplot(x="day", y="total_bill", hue="smoker",
           kind="swarm", data=tips)
```

{% asset_img 散点图swarm.png 散点图swarm %}

### 小提琴图violin

```python
## 绘制violin（小提琴图）：
import seaborn as sns

tips = sns.load_dataset("tips")
%matplotlib inline
sns.catplot(x="day", y="total_bill", hue="smoker",
           data=tips, kind="violin")
```

{% asset_img 小提琴图.png 小提琴图 %}

```python
## 参数split为真，表示
import seaborn as sns

tips = sns.load_dataset("tips")
%matplotlib inline
sns.catplot(x="day", y="total_bill", hue="smoker",
           data=tips, kind="violin", split=True)
```

<meta name="referrer" content="no-referrer" />
{% asset_img 小提琴图split.png 小提琴图split %}


### 箱线图box

```python
## catplot绘制box（箱线图）：
import seaborn as sns

tips = sns.load_dataset("tips")
%matplotlib inline
sns.catplot(x="day", y="total_bill", hue="smoker",
           data=tips, kind="box")
```

<meta name="referrer" content="no-referrer" />
{% asset_img 箱线图.png 箱线图 %}

```python
## catplot绘制boxen（增强箱型图/箱线图）：
import seaborn as sns

tips = sns.load_dataset("tips")
%matplotlib inline
sns.catplot(x="day", y="total_bill", hue="smoker",
           data=tips, kind="boxen")
```

<meta name="referrer" content="no-referrer" />
{% asset_img 增强箱线图.png 增强箱线图 %}

## distplot()
`distplot()` ：数据集分布图（distribution）。

`distplot(a, bins=None, hist=True, kde=True, rug=False, 
    fit=None, hist_kws=None, kde_kws=None, rug_kws=None, 
    fit_kws=None, color=None, vertical=False, 
    norm_hist=False, axlabel=None, label=None, ax=None)`

- `distplot`是hist（直方图）、rugplot（地毯图）、kdeplot（核密度估计图）的高级封装。
- 参数`fit` ：指定拟合相应的分布（可缺省）。

### 直方图


### 地毯图


### 核密度估计图


## lmplot()
`lmplot()` ：回归曲线图。
`lmplot(x, y, data, hue=None, col=None, row=None,
    palette=None, col_wrap=None, height=5, aspect=1, 
    markers='o', sharex=True, sharey=True, hue_order=None, 
    col_order=None, row_order=None, legend=True, 
    legend_out=True, x_estimator=None, x_bins=None, 
    x_ci='ci', scatter=True, fit_reg=True, ci=95, 
    n_boot=1000, units=None, order=1, logistic=False, 
    lowess=False, robust=False, logx=False, 
    x_partial=None, y_partial=None, truncate=False, 
    x_jitter=None, y_jitter=None, scatter_kws=None, 
    line_kws=None, size=None)`
- 与`regplot()`的区别是：`lmplot()`必须指定`data`参数
- 参数`lowess = True`：进行局部加权线性回归（LOWESS）
- 参数`logx = True`：进行对数线性回归，估计$y \sim \log{(X)}$形式的线性回归（但是$x$必须为正）
- 参数`robust = True`：进行稳健线性回归
- 参数`logistic = True`：进行逻辑回归


```python
## 使用数据tips绘制回归拟合曲线图并绘制置信区间：
tips = sns.load_dataset("tips")

sns.lmplot(x="total_bill", y="tip", col="time", hue="smoker",
           data=tips)
```

### 线性回归
指定自变量`x`和因变量`y`即可
- `hue`：分组进行线性回归

```python
import seaborn as sns

tips = sns.load_dataset("tips")  ##载入需要的数据tips
sns.lmplot(x="total_bill", y="tip", hue="smoker", data=tips)
```

<center>
{% asset_img 线性回归1.png 线性回归 %}
</center>

### 局部加权线性回归
局部加权线性回归（LOWESS，Locally Weighted Scatterplot Smoothing），一种非参数回归拟合的方式。主要思想是选取一定比例的局部数据，拟合多项式回归曲线，以便观察到数据的局部规律和趋势。
- 设置参数`lowess = True`
- 弥补了普通线性回归模型欠拟合或过拟合的问题
- 局部加权中的权重，是根据要预测的点与数据集中的点的距离来为数据集中的点赋权值。当某点离要预测的点越远，其权重越小，否则越大
- 局部加权线性回归的优势：处理非线性关系的异方差问题

```python
import seaborn as sns

tips = sns.load_dataset("tips")  ##载入需要的数据tips
sns.set_style('white')
f = plt.figure(figsize=(6, 4))
g = sns.lmplot(x="total_bill", y="tip", hue="smoker", lowess = True, data=tips)
g.savefig('局部加权线性回归.png')
```

<center>
{% asset_img 局部加权线性回归.png 局部加权线性回归 %}
</center>

### 对数线性回归
进行$y \sim \log{(x)}$形式的线性回归
- 令参数`logx = True`

```python 
import seaborn as sns

tips = sns.load_dataset("tips")  ##载入需要的数据tips
sns.set_style('white')
f = plt.figure(figsize=(6, 4))
g = sns.lmplot(x="total_bill", y="tip", hue="smoker", logx = True, data=tips)
g.savefig('对数线性回归.png')
```

<center>
{% asset_img 对数线性回归.png 对数线性回归 %}
</center>

### 稳健线性回归
在数据存在异常值的情况下，使用不同的损失函数来减少相对较大的残差，拟合一个稳健的回归模型
- 令参数`robust = True`
- 常见的稳健回归：最小中位平方法、M估计法等

```python 
import seaborn as sns

tips = sns.load_dataset("tips")  ##载入需要的数据tips
sns.set_style('white')
f = plt.figure(figsize=(6, 4))
g = sns.lmplot(x="total_bill", y="tip", hue="smoker", col="smoker", 
               robust = True, data=tips)
g.savefig('稳健线性回归.png')
```

<center>
{% asset_img 稳健线性回归.png 稳健线性回归 %}
</center>

### 多项式回归
- 令参数`order = n`，其中`n`是指多项式回归的阶数

```python
import seaborn as sns

tips = sns.load_dataset("tips")  ##载入需要的数据tips
sns.set_style('white')
f = plt.figure(figsize=(6, 4))
g = sns.lmplot(x="total_bill", y="tip", hue="smoker", col="smoker", 
               order=3, data=tips)
g.savefig('（三阶）多项式回归.png')
```

<center>
{% asset_img （三阶）多项式回归.png （三阶）多项式回归 %}
</center>


### 逻辑回归
逻辑回归（LR，Logistic Regression）是一种广义线性回归。因变量可以是二分类的，也可以是多分类的。
- 令参数`logistic = True`

```python

```

<center>
{% asset_img （三阶）多项式回归.png （三阶）多项式回归 %}
</center>




## relplot()
`relplot()` ：可视化变量间的关系（relationship）。可绘制散点图和曲线图。
- 参数 `hue` ：分组变量
- 参数 `style` ：在某一维度上，用线的不同表现形式区分
- 参数 `size` ：控制数据点的大小或线条的粗细
- 参数 `facet_kws`：设置坐标是否共享

### 散点图
```python
relplot(x=None, y=None, hue=None, size=None, style=None, 
 data=None, row=None, col=None, col_wrap=None, 
 row_order=None, col_order=None, palette=None, hue_order=None,
 hue_norm=None, sizes=None, size_order=None, size_norm=None, 
    markers=None, dashes=None, style_order=None, legend='brief',
    kind='scatter',      ## 散点图
    height=5, aspect=1, facet_kws=None, **kwargs)
```

### 曲线图
```python
relplot(x=None, y=None, hue=None, size=None, style=None, 
 data=None, row=None, col=None, col_wrap=None, 
 row_order=None, col_order=None, palette=None, hue_order=None, 
 hue_norm=None, sizes=None, size_order=None, size_norm=None, 
    markers=None, dashes=None, style_order=None, legend='brief', 
    kind='line',        ## 曲线图
    height=5, aspect=1, facet_kws=None, **kwargs)
```


```python
## 使用数据dots绘制曲线图：
import seaborn as sns

dots = sns.load_dataset("dots")
dots.head()
```

```python
sns.relplot(x="time", y="firing_rate", col="align",
           hue="choice", size="coherence", style="choice",
           facet_kws=dict(sharex=False), kind="line",
           legend="full", data=dots)
```



```python
sns.relplot(x="time", y="firing_rate", col="align",
            hue="choice", size="coherence", style="choice",
            height=4.5, aspect=2 / 3,
            facet_kws=dict(sharex=False),
            kind="line", legend="full", data=dots)
##参数aspect控制横纵比
##参数facet_kws设置坐标是否共享
```

```python
## 使用数据fmri绘制曲线图，并且绘制置信区间：
fmri = sns.load_dataset("fmri")
sns.relplot(x="timepoint", y="signal", col="region",
            hue="event", style="event",
            kind="line", data=fmri);
```



```python
sns.relplot(x="total_bill", y="tip", col="time",
            hue="size", style="smoker", size="size",
            palette="YlGnBu", markers=["D", "o"], sizes=(10, 125),
            edgecolor=".2", linewidth=.5, alpha=.75,
            data=tips)
```

# 其他绘图函数
## FacetGrid()
绘制分面图
- 可与绘图函数、`map()`函数结合使用

```python
## http://seaborn.pydata.org/examples/many_facets.html
import numpy as np
import pandas as pd
import seaborn as sns
import matplotlib.pyplot as plt

sns.set_theme(style="ticks")

## 使用随机游走（random walk）创建数据
rs = np.random.RandomState(4)
pos = rs.randint(-1, 2, (20, 5)).cumsum(axis=1)  ## 生成-1到2之间的维度为20*5的数据
pos -= pos[:, 0, np.newaxis]
step = np.tile(range(5), 20)    ## 在列方向上重复[0, 1, 2, 3, 4]20次，行方向默认1次
walk = np.repeat(range(20), 5)  ## 将[0, 1, ..., 19]重复5次
df = pd.DataFrame(np.c_[pos.flat, step, walk],
                  columns=["position", "step", "walk"])
df.head()
#  position step walk
# 0 0   0  0
# 1 1   1  0
# 2 1   2  0
# 3 1   3  0
# 4 0   4  0

grid = sns.FacetGrid(df, col="walk", hue="walk", palette="tab20c",
                     col_wrap=4, height=1.5)  ## col_wrap列维度限制
grid.map(plt.axhline, y=0, ls=":", c=".5")
## plt.axhline：绘制平行于x轴的水平参考线
## ls参数：linestyle

## 绘制折线图，展示随机游走的路径
grid.map(plt.plot, "step", "position", marker="o")

## 调整x轴刻度、y轴刻度，x轴、y轴坐标轴取值范围
grid.set(xticks=np.arange(5), yticks=[-3, 3],
         xlim=(-.5, 4.5), ylim=(-3.5, 3.5))

grid.fig.tight_layout(w_pad=1)
grid.savefig('分面折线图.png', dpi=400)
```

<meta name="referrer" content="no-referrer" />
{% asset_img 分面折线图.png 分面折线图 %}

```python
import seaborn as sns

tips = sns.load_dataset("tips")  ## 载入数据
g = sns.FacetGrid(tips, col="day", height=4, aspect=.5)   ## 以day为分面的列
g.map(sns.barplot, "sex", "total_bill", order=["Male", "Female"])  ## 绘制直方图
g.savefig('分面直方图.png', dpi=400)
```

<meta name="referrer" content="no-referrer" />
{% asset_img 分面直方图.png 分面直方图 %}

```python
## http://seaborn.pydata.org/examples/kde_ridgeplot.html
import numpy as np
import pandas as pd
import seaborn as sns
import matplotlib.pyplot as plt
sns.set_theme(style="white", rc={"axes.facecolor": (0, 0, 0, 0)})

# 构造数据
rs = np.random.RandomState(2021)
x = rs.randn(500)  ## 随机生成500个数据
g = np.tile(list("ABCDEFGHIJ"), 50)  ## 按列重复[A,B,...,I,J]50次
df = pd.DataFrame(dict(x=x, g=g))
m = df.g.map(ord)
df["x"] += m
df.head()
#  x   g
# 0 66.488609 A
# 1 66.676011 B
# 2 66.581549 C
# 3 67.193479 D
# 4 69.555876 E

pal = sns.cubehelix_palette(n_colors=10, rot=-.25, light=.7)  ## 生成序列调色盘；light：最浅颜色的强度
pp = sns.FacetGrid(df, row="g", hue="g", aspect=15, height=.5, palette=pal)
pp.map(sns.kdeplot, "x",
       bw_adjust=.5, clip_on=False,
       fill=True, alpha=1, linewidth=1.5)
pp.map(sns.kdeplot, "x", clip_on=False, color="w", lw=2, bw_adjust=.5)
pp.map(plt.axhline, y=0, lw=2, clip_on=False)  ## plt.axhline：绘制平行于x轴的水平参考线

def label(x, color, label):  ## 用于添加x轴刻度文本
    ax = plt.gca()
    ax.text(0, .2, label, fontweight="bold", color=color,
            ha="left", va="center", transform=ax.transAxes)

pp.map(label, "x")

pp.fig.subplots_adjust(hspace=-.25)  ## 调整子图以重叠
pp.set_titles("")  ## 去除轴标题
pp.set(yticks=[])  ## 去除y轴刻度线
pp.despine(bottom=True, left=True)  ## 去除底部、左边的坐标脊
pp.savefig('重叠分面密度曲线图.png', dpi=400)
```

<meta name="referrer" content="no-referrer" />
{% asset_img 重叠分面密度曲线图.png 重叠分面密度曲线图 %}

## heatmap()
绘制热力图。
```python
from string import ascii_letters
import numpy as np
import pandas as pd
import seaborn as sns
import matplotlib.pyplot as plt

## 生成数据
rs = np.random.RandomState(33)
dat = pd.DataFrame(data=rs.normal(size=(100, 26)),
                  columns=list(ascii_letters[26:]))

## 计算相关系数矩阵
corr = dat.corr()

mask = np.triu(np.ones_like(corr, dtype=bool))

f, ax = plt.subplots(figsize=(11, 9))
## 设置颜色
cmap = sns.diverging_palette(230, 20, as_cmap=True)
fig = sns.heatmap(corr, mask=mask, cmap=cmap, vmax=.3, center=0, square=True, 
                 linewidths=.5, cbar_kws={'shrink': .5})
fig.savefig('相关系数矩阵图.png')
```

<meta name="referrer" content="no-referrer" />
{% asset_img 相关系数矩阵图.png 相关系数矩阵图 %}

```python
## http://seaborn.pydata.org/examples/spreadsheet_heatmap.html
import matplotlib.pyplot as plt
import seaborn as sns
sns.set_theme()

flights_long = sns.load_dataset("flights")  ## 载入数据
flights_long.head()
#  year month passengers
# 0 1949 Jan  112
# 1 1949 Feb  118
# 2 1949 Mar  132
# 3 1949 Apr  129
# 4 1949 May  121

## 变换数据形式
flights = flights_long.pivot("month", "year", "passengers")
flights.head()
# year 1949 1950 1951 1952 1953 1954 1955 1956 1957 1958 1959 1960
# month            
# Jan 112 115 145 171 196 204 242 284 315 340 360 417
# Feb 118 126 150 180 196 188 233 277 301 318 342 391
# Mar 132 141 178 193 236 235 267 317 356 362 406 419
# Apr 129 135 163 181 235 227 269 313 348 348 396 461
# May 121 125 172 183 229 234 270 318 355 363 420 472
f, ax = plt.subplots(figsize=(9, 6))
g = sns.heatmap(flights, annot=True, fmt="d", linewidths=.5, ax=ax)
fig = g.get_figure()
fig.savefig('热力图.png', dpi=400)
```

<meta name="referrer" content="no-referrer" />
{% asset_img 热力图.png 热力图 %}

## 柱状图histplot()
绘制柱状图

```python
## http://seaborn.pydata.org/examples/histogram_stacked.html
import seaborn as sns
import matplotlib as mpl
import matplotlib.pyplot as plt

sns.set_theme(style="ticks")

diamonds = sns.load_dataset("diamonds")  ## 载入数据
diamonds.head()
#  carat cut color clarity depth table price x y z
# 0 0.23 Ideal E SI2 61.5 55.0 326 3.95 3.98 2.43
# 1 0.21 Premium E SI1 59.8 61.0 326 3.89 3.84 2.31
# 2 0.23 Good E VS1 56.9 65.0 327 4.05 4.07 2.31
# 3 0.29 Premium I VS2 62.4 58.0 334 4.20 4.23 2.63
# 4 0.31 Good J SI2 63.3 58.0 335 4.34 4.35 2.75

f, ax = plt.subplots(figsize=(7, 5))
sns.despine(f)  ## 移除上部、右边的坐标脊

g = sns.histplot(
    diamonds,
    x="price", hue="cut",  ## 根据cut区分颜色
    multiple="stack",
    palette="light:m_r",   ## 调色盘
    edgecolor=".3",        ## 柱状边的颜色
    linewidth=.5,          ## 线条宽度
    log_scale=True,        ## Set a log scale on the data axis
)
ax.xaxis.set_major_formatter(mpl.ticker.ScalarFormatter())  ## 设置x轴主刻度格式
ax.set_xticks([500, 1000, 2000, 5000, 10000])  ## 设置x轴刻度文本

fig = g.get_figure()
fig.savefig('柱状图.png', dpi=400)
```

<meta name="referrer" content="no-referrer" />
{% asset_img 柱状图.png 柱状图 %}

## JointGrid()
两个变量的关系+单变量边际分布

```python
## http://seaborn.pydata.org/examples/marginal_ticks.html
import seaborn as sns
sns.set_theme(style="white", color_codes=True)  ## 设置主题
mpg = sns.load_dataset("mpg")  ## 载入数据mpg
mpg.head()
#  mpg cylinders displacement horsepower weight acceleration model_year origin name
# 0 18.0 8 307.0 130.0 3504 12.0 70 usa chevrolet chevelle malibu
# 1 15.0 8 350.0 165.0 3693 11.5 70 usa buick skylark 320
# 2 18.0 8 318.0 150.0 3436 11.0 70 usa plymouth satellite
# 3 16.0 8 304.0 150.0 3433 12.0 70 usa amc rebel sst
# 4 17.0 8 302.0 140.0 3449 10.5 70 usa ford torino

g = sns.JointGrid(data=mpg, x="mpg", y="acceleration", space=0, ratio=17)
g.plot_joint(sns.scatterplot, size=mpg["horsepower"], sizes=(30, 120),  ## 根据horsepower调整散点的大小
             color="g", alpha=.6, legend=False)
g.savefig('0104散点图.png', dpi=400)
```

<meta name="referrer" content="no-referrer" />
{% asset_img 0104散点图.png 散点图 %}

```python
g = sns.JointGrid(data=mpg, x="mpg", y="acceleration", space=0, ratio=17)
g.plot_joint(sns.scatterplot, size=mpg["horsepower"], sizes=(30, 120),
             color="g", alpha=.6, legend=False)
g.plot_marginals(sns.rugplot, height=1, color="g", alpha=.6)
g.savefig('散点图+刻度线.png', dpi=400)
```

<meta name="referrer" content="no-referrer" />
{% asset_img 散点图+刻度线.png 散点图+刻度线 %}

## jointplot()
在图形边上绘制单变量的直方分布图

```python
## http://seaborn.pydata.org/examples/regression_marginals.html
import seaborn as sns
sns.set_theme(style="darkgrid")  ## 设置主题

tips = sns.load_dataset("tips")  ## 载入数据 tips
g = sns.jointplot(x="total_bill", y="tip", data=tips,
                  kind="reg", truncate=False,  ## 画回归线
                  xlim=(0, 60), ylim=(0, 12),
                  color="m", height=7)
g.savefig('jointplot边际直方分布图.png', dpi=400)
```

<meta name="referrer" content="no-referrer" />
{% asset_img jointplot边际直方分布图.png jointplot边际直方分布图 %}

## 核密度估计图kdeplot()
核密度估计图
- 可以比较直观地看出数据样本本身的分布特征
- 参数`cumulative`：是否绘制累积分布，默认False
- 参数`shade`：是否绘制曲线下的阴影区域
- 参数`color`：控制曲线、阴影的颜色（曲线颜色比阴影颜色深）
- 参数`vertical`：以X轴进行绘制还是以Y轴进行绘制（已弃用），将数据赋给`y`即可
- 参数`cbar`：当绘制二维核密度估计图且`shade=True`时，令`cbar=True`则图片显示色阶


```python
import numpy as np
import seaborn as sns 

x=np.random.randn(100)  #随机生成100个符合正态分布的数
g = sns.kdeplot(x)  ## 简单的一维核密度估计图
fig = g.get_figure()
fig.savefig('简单的一维核密度估计图.png', dpi=400)
```

<meta name="referrer" content="no-referrer" />
{% asset_img 简单的一维核密度估计图.png 简单的一维核密度估计图 %}

```python
g = sns.kdeplot(x, cumulative=True)
fig = g.get_figure()
fig.savefig('一维核密度估计累积图.png', dpi=400)
```

<meta name="referrer" content="no-referrer" />
{% asset_img 一维核密度估计累积图.png 一维核密度估计累积图 %}

```python
g = sns.kdeplot(x, shade=True, color='g')  ## 颜色为绿色 green
fig = g.get_figure()
fig.savefig('一维核密度估计图-绘制阴影.png', dpi=400)
```

<meta name="referrer" content="no-referrer" />
{% asset_img 一维核密度估计图-绘制阴影.png 一维核密度估计图-绘制阴影 %}

```python
g = sns.kdeplot(y=x)  ## 以y轴进行绘制
fig = g.get_figure()
fig.savefig('一维核密度估计图-以y轴进行绘制.png', dpi=400)
```

<meta name="referrer" content="no-referrer" />
{% asset_img 一维核密度估计图-以y轴进行绘制.png 一维核密度估计图-以y轴进行绘制 %}

```python
x = np.random.randn(100)
y = np.random.randn(100)
g = sns.kdeplot(x, y, shade=True, cbar=True)
fig = g.get_figure()
fig.savefig('二维核密度估计图.png', dpi=400)
```

<meta name="referrer" content="no-referrer" />
{% asset_img 二维核密度估计图.png 二维核密度估计图 %}

## lineplot()
绘制曲线图

```python
## http://seaborn.pydata.org/examples/wide_data_lineplot.html
import numpy as np
import pandas as pd
import seaborn as sns
sns.set_theme(style="whitegrid")

rs = np.random.RandomState(365)
values = rs.randn(365, 4).cumsum(axis=0)  ## 随机生成365*4的数据
dates = pd.date_range("1 1 2020", periods=365, freq="D")  ## 从2020.01.01开始、按天分配日期
data = pd.DataFrame(values, dates, columns=["A", "B", "C", "D"])
data.head()
#     A   B   C   D
# 2020-01-01 0.167921 0.523505 0.817376 1.703846
# 2020-01-02 -1.979026 1.237704 0.057230 2.743267
# 2020-01-03 -2.945478 1.094025 1.628355 2.359988
# 2020-01-04 -2.307479 0.749367 1.624072 2.518347
# 2020-01-05 -3.270573 0.333310 1.867085 2.866550
data = data.rolling(7).mean()
g = sns.lineplot(data=data, palette="tab10", linewidth=2.5)
fig = g.get_figure()
fig.savefig('线形图.png', dpi=400)
```

<meta name="referrer" content="no-referrer" />
{% asset_img 线形图.png 线形图 %}


## pairplot()
绘制变量两两之间的散点图
- 对角线绘制变量的分布图
- 非对角线绘制两两变量的散点图

```python
## http://seaborn.pydata.org/examples/scatterplot_matrix.html
import pandas as pd

url = 'https://raw.githubusercontent.com/mwaskom/seaborn-data/master/penguins.csv'

df = pd.read_csv(url, error_bad_lines=False)  ## 加载penguins数据
df.head()
#  species island bill_length_mm bill_depth_mm flipper_length_mm body_mass_g sex
# 0 Adelie Torgersen 39.1 18.7 181.0 3750.0 MALE
# 1 Adelie Torgersen 39.5 17.4 186.0 3800.0 FEMALE
# 2 Adelie Torgersen 40.3 18.0 195.0 3250.0 FEMALE
# 3 Adelie Torgersen NaN NaN NaN NaN NaN
# 4 Adelie Torgersen 36.7 19.3 193.0 3450.0 FEMALE

import seaborn as sns

sns.set_style('ticks')  ## 设置主题；set_theme()函数已弃用
fig = sns.pairplot(df, hue="species")  ## species决定色彩
fig.savefig('两两配对散点图.png', dpi=400)
```

<meta name="referrer" content="no-referrer" />
{% asset_img 两两配对散点图.png 两两配对散点图 %}

# 其他函数
## axes_style()
坐标轴属性设置

```python
import seaborn as sns

sns.axes_style()  ## 查看目前的各属性定义
# {'axes.facecolor': 'white',
#  'axes.edgecolor': '.15',
#  'axes.grid': False,
#  'axes.axisbelow': True,
#  'axes.labelcolor': '.15',   ## 轴标签颜色
#  'figure.facecolor': 'white',  ## 画布颜色
#  'grid.color': '.8',    ## 网格颜色
#  'grid.linestyle': '-',   ## 网格线型
#  'text.color': '.15',
#  'xtick.color': '.15',   ## x轴刻度颜色
#  'ytick.color': '.15',     ## y轴刻度颜色
#  'xtick.direction': 'out',    ## x轴刻度方向
#  'ytick.direction': 'out',    ## y轴刻度方向
#  'lines.solid_capstyle': 'round',
#  'patch.edgecolor': 'w',
#  'patch.force_edgecolor': True,
#  'image.cmap': 'rocket',
#  'font.family': ['sans-serif'],  ## 字体类型
#  'font.sans-serif': ['Arial',
#   'DejaVu Sans',
#   'Liberation Sans',
#   'Bitstream Vera Sans',
#   'sans-serif'],
#  'xtick.bottom': False,
#  'xtick.top': False,
#  'ytick.left': False,
#  'ytick.right': False,
#  'axes.spines.left': True,   ## 左边坐标脊
#  'axes.spines.bottom': True,  ## 底部坐标脊
#  'axes.spines.right': True,  ## 右边坐标脊
#  'axes.spines.top': True}     ## 顶部坐标脊
```

## despine()
去除坐标脊
- 对`white`和`ticks`风格有效

```python
import numpy as np
import seaborn as sns
import matplotlib.pyplot as plt

sns.set_style('white')

f = plt.figure(figsize=(6, 4))

def sinplot(flip=1):
    x = np.linspace(0, 14, 100)
    for i in range(1, 7):
        plt.plot(x, np.sin(x + i * .5) * (7 - i) * flip)

sinplot()
sns.despine()
f.savefig('去除坐标脊.png', dpi=400)
```

<meta name="referrer" content="no-referrer" />
{% asset_img 去除坐标脊.png 去除坐标脊 %}

## get_dataset_names()
获取所有Seaborn自带数据集的名称（需要联网）。

```python
from seaborn import get_dataset_names

get_dataset_names()
```

|数据名称|数据详情|用途|
|:---|:---|:----|
|'anagrams'|||
|'anscombe'|||
|'attention'|||
|'brain_networks'|||
|'car_crashes'|||
|'diamonds'|||
|'dots'|||
|'exercise'|||
|'flights'|||
|'fmri'|||
|'gammas'|||
|'geyser'|||
|'iris'|[数据详情](http://archive.ics.uci.edu/ml/datasets/Iris)||
|'mpg'|||
|'penguins'|||
|'planets'|||
|'tips'|||
|'titanic'|[数据详情](https://www.kaggle.com/heptapod/titanic)||


## load_dataset()
加载自带的数据集（需要联网）。

```python
import seaborn as sns

df = sns.load_dataset('titanic')
df.sample(5)
```

## set_context()
设置情境的size
- 可选项：`paper`、`talk`、`poster`、`notebook`
- 参数`font_scale`：控制字体的size倍数
- 参数`rc`：如`rc={"lines.linewidth": 5.0}`

```python
sns.set_context("notebook", font_scale=5.0, rc={"lines.linewidth": 5.0})
```

## set_theme()
修改主题
- 如果提示`AttributeError: module 'seaborn' has no attribute 'set_theme'`，则表明当前的seaborn版本过低，应使用`pip install seaborn==0.11.0`或`pip install seaborn==0.11.1`安装0.11版本的seaborn
- 查看当前的seaborn版本：
```python
import seaborn
seaborn.__version__
#'0.11.0'
```


## set_style()
调整绘图style。
- 第一个参数是整体风格，第二个参数是具体某个element的参数，如：`sns.set_style("darkgrid", {"axes.facecolor": ".2"})`。可使用`axes_style()`查看目前的elements定义

有5种预设的style：
- `darkgrid`
- `whitegrid`
- `dark`
- `white`
- `ticks`





# 调色板
## choose_colorbrewer_palette()
从Color Brewer中选择调色板。
`choose_colorbrewer_palette(data_type, as_cmap=False)`
- 该函数只能在Jupyter Notebook中使用
- 参数 `data_type` ：可选值有'sequential', 'diverging', 'qualitative'
- 其中 `Set` 可选择不同的调色板

```python
sns.palplot(sns.choose_colorbrewer_palette(data_type="qualitative"))
```

```python
sns.palplot(sns.choose_colorbrewer_palette(data_type="diverging"))
```

## choose_cubehelix_palette()
`choose_cubehelix_palette(as_cmap=False)`
- `cubehelix()`函数对应的交互式小部件
- 该函数只能在Jupyter Notebook中使用

```python
sns.palplot(sns.choose_cubehelix_palette())
```

## choose_dark_palette()
`choose_dark_palette(input='husl', as_cmap=False)`
- `dark_palette()`函数对应的交互式小部件
- 该函数只能在Jupyter Notebook中使用
- 参数 `input` ：可选值有`husl`, `hls`, `rgb`


```python
sns.palplot(sns.choose_dark_palette())
```

## choose_diverging_palette()
`choose_diverging_palette(as_cmap=False)`
- `diverging_palette()`函数对应的交互式小部件
- `center`部件可选择：light或dark

```python
sns.palplot(sns.choose_diverging_palette())
```


## choose_light_palette()
`choose_light_palette(input='husl', as_cmap=False)`
- `light_palette()`函数对应的交互式小部件
- 参数 `input` ：可选值有`husl`, `hls`, `rgb`
- 该函数只能在Jupyter Notebook中使用

```python
sns.palplot(sns.choose_light_palette())
```

## color_palette()
调色板
- seaborn有6种不同的默认主题：deep，muted，pastel，bright，dark，colorblind
- 若有任意类别的变量，但不突出任何一个类别时，可使用连续均匀间隔的颜色。
- 分类调色板：Color Brewer
- 可见[seaborn.color_palette](http://seaborn.pydata.org/generated/seaborn.color_palette.html#seaborn.color_palette)，有颜色示例。

```python
## seaborn默认调色盘
current_palette = sns.color_palette()
sns.palplot(current_palette)
```

```python
## 默认调色板（muted）
sns.palplot(sns.color_palette("muted"))
```

```python
## 默认调色板（deep）
sns.palplot(sns.color_palette("deep"))
```

```python
## 默认调色板（pastel）
sns.palplot(sns.color_palette("pastel"))
```


# 参考资料
- [seaborn主页](http://seaborn.pydata.org/)
- [seaborn Example Gallery](http://seaborn.pydata.org/examples/index.html)
- [displot例子](http://seaborn.pydata.org/examples/distplot_options.html)
- [lmplot例子](http://seaborn.pydata.org/examples/anscombes_quartet.html)
- [python绘图-seaborn(sns)的主题风格](https://blog.csdn.net/weixin_42468475/article/details/109556785)
- [Seaborn5分钟入门(一)——kdeplot和distplot](https://zhuanlan.zhihu.com/p/33977558)
- [太厉害了！Seaborn也能做多种回归分析，统统只需一行代码](https://mp.weixin.qq.com/s/jk6iXDbOlo4csojSAcox_g)