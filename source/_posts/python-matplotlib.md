---
title: Python | matplotlib
date: 2021-07-24 22:01:01
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

# Matplotlib
> [Matplotlib Tutorials](https://matplotlib.org/stable/tutorials/index.html)


# .axes.Axes 轴格式
## .set_major_formatter()
设置数据标签的显示的文本格式

```python
ticks = np.arange(0., 8.1, 2.)
ax.xaxis.set_ticks(ticks)
ax.xaxis.set_major_formatter('{x:1.1f}')  ## 显示为1位小数

ax.xaxis.set_major_formatter('±{x}°') ## 数字前带±号、数字后带°
```

## .set_xlim()
设置x轴的数据范围


## .set_ticks()
设置轴的刻度线
- `.xaxis.set_ticks()`：设置x轴刻度线
- `.yaxis.set_ticks()`：设置y轴刻度线

## .set_ticklabels()
设置轴的数据标签格式

```python
ticks = np.arange(0., 8.1, 2.)
ax.xaxis.set_ticks(ticks)
ax.xaxis.set_ticklabels([f'{tick:1.2f}' for tick in ticks])
```

## .set_title()
设置图表标题
- 参数`loc`：标题位置
    - `left`：靠左
    - `center`：居中
    - `right`：靠右
- 参数`pad`：标题相对图表偏移（默认为6）

```python
fig, axs = plt.subplots(3, 1, figsize=(5, 6), tight_layout=True)
locs = ['center', 'left', 'right']
for ax, loc in zip(axs, locs):
    ax.plot(x1, y1)
    ax.set_title('Title with loc at '+loc, loc=loc)
plt.show()
```

{% asset_img 修改图表标题.png 修改图表标题  %}


## .set_xlabel()
修改$x$轴标签
> [matplotlib.axes.Axes.set_xlabel](https://matplotlib.org/stable/api/_as_gen/matplotlib.axes.Axes.set_xlabel.html#matplotlib.axes.Axes.set_xlabel)

- 参数`fontsize`：设置标签字体大小
- 参数`fontweight`：设备标签字体粗细
    - `fontweight='bold'`：粗体
- 参数`fontproperties`：设置字体
- 参数`horizontalalignment`：设置标签水平对齐位置
    - `horizontalalignment=left`：水平向左对齐
- 参数`labelpad`：标签相对轴偏移
- 参数`position=(a, b)`：标签位置
    - 在`.set_xlabel()`中，仅`a`起作用，`b`随便赋值
    - 在`.set_ylabel()`中，仅`b`起作用，`a`随便赋值
- 标签换行：`\n`
- 标签含公式：`r'$公式内容$'`

```python
import matplotlib.pyplot as plt
import numpy as np

%matplotlib inline

x1 = np.linspace(0.0, 5.0, 100)
y1 = np.cos(2 * np.pi * x1) * np.exp(-x1)

fig, ax = plt.subplots(figsize=(5, 3))
fig.subplots_adjust(bottom=0.15, left=0.2)
ax.plot(x1, y1)
ax.set_xlabel('time [s]')
ax.set_ylabel('Damped oscillation [V]')

plt.show()
```

{% asset_img 修改xy轴标签文字.png 修改xy轴标签文字  %}

```python
fig, ax = plt.subplots(figsize=(5, 3))
fig.subplots_adjust(bottom=0.15, left=0.2)
ax.plot(x1, y1*10000)
ax.set_xlabel('time [s]')
ax.set_ylabel('Damped oscillation [V]', labelpad=18)

plt.show()
```

{% asset_img 修改xy轴标签文字_文本偏移.png 修改xy轴标签文字（文本偏移）  %}

```python
from matplotlib.font_manager import FontProperties

font = FontProperties()
font.set_family('serif')
font.set_name('Times New Roman')
font.set_style('italic')

fig, ax = plt.subplots(figsize=(5, 3))
fig.subplots_adjust(bottom=0.15, left=0.2)
ax.plot(x1, y1)
ax.set_xlabel('time [s]', fontsize='large', fontweight='bold')
ax.set_ylabel('Damped oscillation [V]', fontproperties=font)

plt.show()
```

{% asset_img 修改xy轴标签文字（字体格式）.png 修改xy轴标签文字（字体格式）  %}


```python
fig, ax = plt.subplots(figsize=(5, 3))
fig.subplots_adjust(bottom=0.2, left=0.2)
ax.plot(x1, np.cumsum(y1**2))
ax.set_xlabel('time [s] \n This was a long experiment')
ax.set_ylabel(r'$\int\ Y^2\ dt\ \ [V^2 s]$')
plt.show()
```

{% asset_img 修改xy轴标签文字（换行+含公式）.png 修改xy轴标签文字（换行+含公式）  %}

## .set_ylabel()
修改$y$轴标签

> 例子见`.set_xlabel()`

## .tick_params()
旋转刻度线标签，使得文本之间互相重叠
- 参数`rotation`：逆时针旋转的度数

```python
import datetime

fig, ax = plt.subplots(figsize=(5, 3), tight_layout=True)
base = datetime.datetime(2017, 1, 1, 0, 0, 1)
time = [base + datetime.timedelta(days=x) for x in range(len(x1))]

ax.plot(time, y1)
ax.tick_params(axis='x', rotation=70)
plt.show()
```

{% asset_img 旋转刻度线标签.png 旋转刻度线标签  %}


```python

```

```python

```

# 参考资料