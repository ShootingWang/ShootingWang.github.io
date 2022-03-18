---
title: Python | pyecharts
date: 2020-05-08 09:40:20
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

# pyecharts
- [pyecharts官网](https://pyecharts.org/)


## 安装
在Anaconda Prompt中输入：
```bash
pip install pyecharts
```

安装过程可能会出现多种问题：
1. 缺少相关的库
2. 安装MarkupSafe出错
3. 提示找不到相应版本

[解决方法](https://blog.csdn.net/qq_37954088/article/details/88876791)

## 渲染图片
仅运行demo，并不能直接获得图片，要想获得图片输出（即使用pyecharts将输出渲染成图片），需要进行一点操作，根据官网的说明，共有3种方式：

- [渲染图片](https://pyecharts.org/#/zh-cn/render_images)

1. selenium
2. phantomjs
3. pyppeteer

### selenium
安装snapshot-selenium（在Anaconda Prompt中运行）：
```md
pip install snapshot-selenium
```

使用selenium[^2]，需要配置browser driver（浏览器驱动程序），详情可参见:
https://selenium-python.readthedocs.io/installation.html#drivers

选择浏览器对应的driver，下面以Edge浏览器[^1]为例：
[Edge的WebDriver下载地址](https://developer.microsoft.com/en-us/microsoft-edge/tools/webdriver/)
1. 首先查看Edge的版本信息（version）：点击Edge浏览器右上角的“…”，选择“设置”，“常规”的最下方有Edge版本信息
 <meta name="referrer" content="no-referrer" />
 {% asset_img oldedge.png 旧Edge版本信息 %}
 - 查看全新Edge的版本信息：点击右上角的“…”，选择“帮助与反馈”中的“关于Microsoft Edge”，即可查看相应的版本信息
 <meta name="referrer" content="no-referrer" />
 {% asset_img newedgev.png 新Edge版本信息 %}
2. 接着下载[版本79.0.309.68的Edge对应的WebDriver](https://msedgewebdriverstorage.z22.web.core.windows.net/?prefix=79.0.309.68/)，从[各版本WebDriver](https://msedgewebdriverstorage.z22.web.core.windows.net/)中找到相应版本
3. 下载后解压，将driver的路径添加到系统路径中：
 - 右键单击“此电脑/我的电脑”
 - 选择“属性”
 - 选择“高级系统设置”—“环境变量”
 - 在”系统变量“中的”Path“添加driver的路径

[^1]: 2020年01月16日，基于Chromium内核的全新Microsoft Edge正式上线微软官网，可自行下载进行更新。[全新Microsoft Edge](https://www.microsoft.com/en-us/edge)
<meta name="referrer" content="no-referrer" />
{% asset_img newedge.png 全新Edge %}
[^2]: selenium目前支持Chrome和Safari

Chrome的Driver下载可见：
https://sites.google.com/a/chromium.org/chromedriver/home

使用selenium渲染图片，需要在运行python代码时添加以下代码（举例）：
```python
from pyecharts.render import make_snapshot
from snapshot_selenium import snapshot

make_snapshot(snapshot, calendar_base().render(), "calendar0.png")
```
总是报错(ノへ￣、)

将Chrome Driver应用程序直接放在项目文件所在的文件夹中即可正常运行（惊了

```python
import datetime
import random

from pyecharts import options as opts
from pyecharts.charts import Calendar

from pyecharts.render import make_snapshot
from snapshot_selenium import snapshot

def calendar_base() -> Calendar:
    begin = datetime.date(2019, 1, 1)
    end = datetime.date(2019, 12, 31)
    data = [
        [str(begin + datetime.timedelta(days=i)), 
        random.randint(1000, 25000)] 
        for i in range((end - begin).days + 1)
    ]
    c = (
        Calendar()
        .add("", data, calendar_opts=opts.CalendarOpts(range_="2019"))
        .set_global_opts(
            title_opts=opts.TitleOpts(title="Calendar-2019年每日步数情况"),
            visualmap_opts=opts.VisualMapOpts(
                max_ = 20000,
                min_ = 500,
                orient = "horizontal",
                is_piecewise = True,
                pos_top = "230px",
                pos_left = "100px",
            ),
        )
    )
    return c

make_snapshot(snapshot, calendar_base().render(), "calendar0.png")
```


### phantomjs
安装snapshot-phantomjs（在Anaconda Prompt中运行）：
```shell
pip install snapshot-phantomjs
```

使用snapshot-phantomjs需要安装phantom.exe，[下载地址](https://phantomjs.org/download.html)

下载后解压即可（需要将phantomjs所在路径添加到系统变量的PATH中）

仍是报错(ノへ￣、)

将Phantom.exe直接放在项目文件所在的文件夹内即可正常运行
> [简单认识PhantomJS](https://phantomjs.org/quick-start.html)

```python
from pyecharts import options as opts
from pyecharts.charts import Bar
from pyecharts.render import make_snapshot

from snapshot_phantomjs import snapshot

def bar_chart() -> Bar:
    c = (
        Bar()
        .add_xaxis(["衬衫", "毛衣", "领带", "裤子", "风衣", "高跟鞋", "袜子"])
        .add_yaxis("商家A", [114, 55, 27, 101, 125, 27, 105])
        .add_yaxis("商家B", [57, 134, 137, 129, 145, 60, 49])
        .reversal_axis()
        .set_series_opts(label_opts=opts.LabelOpts(position="right"))
        .set_global_opts(title_opts=opts.TitleOpts(title="Bar-测试渲染图片"))
    )
    return c

make_snapshot(snapshot, bar_chart().render(), "bar0.png")
```

### pyppeteer
安装snapshot-pyppeteer（在Anaconda Prompt中运行）：
```shell
pip install snapshot-pyppeteer
```

并执行chromium安装命令（在Anaconda Prompt中运行）：
```shell
pyppeteer-install
```

### 最直接的渲染方式
最简单直接的方式：
```python
xxx().render()
```
其中`xxx()`是相应的pyecharts对象的执行命令

输出的对象为html文件，打开html文件，单击右键可保存图片

```python
import json
import os

from pyecharts import options as opts
from pyecharts.charts import Graph, Page


def graph_base() -> Graph:
    nodes = [
        {"name": "结点1", "symbolSize": 10},
        {"name": "结点2", "symbolSize": 20},
        {"name": "结点3", "symbolSize": 30},
        {"name": "结点4", "symbolSize": 40},
        {"name": "结点5", "symbolSize": 50},
        {"name": "结点6", "symbolSize": 40},
        {"name": "结点7", "symbolSize": 30},
        {"name": "结点8", "symbolSize": 20},
    ]
    links = []
    for i in nodes:
        for j in nodes:
            links.append({"source": i.get("name"), "target": j.get("name")})
    c = (
        Graph()
        .add("", nodes, links, repulsion=8000)
        .set_global_opts(title_opts=opts.TitleOpts(title="Graph-基本示例"))
    )
    return c

graph_base().render()
```

# 图表类型




# 其他
## 将echart插入到PPT
1. 使用pyecharts画图并生成html文件
2. 查看html的源代码
3. 把相关的源代码复制到PPT插件中，点击运行即可

> 需要安装PPT插件`Office Apps Fiddle for PowerPoint`

参考[官方源码](http://gallery.pyecharts.org/#/Sankey/sankey_base)生成html文件：
```Python
from pyecharts import options as opts
from pyecharts.charts import Sankey

nodes = [
    {"name": "category1"},
    {"name": "category2"},
    {"name": "category3"},
    {"name": "category4"},
    {"name": "category5"},
    {"name": "category6"},
]

links = [
    {"source": "category1", "target": "category2", "value": 10},
    {"source": "category2", "target": "category3", "value": 15},
    {"source": "category3", "target": "category4", "value": 20},
    {"source": "category5", "target": "category6", "value": 25},
]
c = (
    Sankey()
    .add(
        "sankey",
        nodes,
        links,
        linestyle_opt=opts.LineStyleOpts(opacity=0.2, curve=0.5, color="source"),
        label_opts=opts.LabelOpts(position="right"),
    )
    .set_global_opts(title_opts=opts.TitleOpts(title="Sankey-基本示例"))
    .render("sankey_base.html")
)
```
**查看html源代码**：
二选一
1. 用浏览器打开`sankey_base.html`，右键选择“查看源代码”或`Ctrl+U`
2. 用Visual Sudio Code打开`sankey_base.html`
查看源代码
<meta name="referrer" content="no-referrer" />
{% asset_img code1.png html源代码 %}

**安装PPT插件**：
打开PowerPoint，选择“插入”——“获取加载项”——搜索“html”，添加“Office Apps Fiddle for PowerPoint”。
<meta name="referrer" content="no-referrer" />
{% asset_img pic2.png 安装PPT插件 %}

将html的源代码粘贴到该插件的“HTML”中
<meta name="referrer" content="no-referrer" />
{% asset_img pic.png 粘贴html源代码 %}

点击“Run Fiddle!”按钮，会报错

<meta name="referrer" content="no-referrer" />
{% asset_img pic3.png 报错 %}

点击“×”，并点击齿轮“Setting”；再次点击“Run Fiddle!”按钮，即可成功生成交互式echarts。

<meta name="referrer" content="no-referrer" />
{% asset_img pic4.png 成功的交互式桑基图 %}

非第一次使用该插件，可从“插入”——“我的加载项”中找到该插件。

# 参考资料
- [pyecharts官方文档](https://pyecharts.org/#/)
- [Pyecharts-Gallary](http://gallery.pyecharts.org/#/)
- [如何把pyecharts的炫酷延续到PPT里？](https://campus.alibaba.com/traineePositions.htm?spm=a1z3e1.11874847.0.0.5d8d4928Tx32T7&refno=12394)
- [百度官方Echarts](https://echarts.apache.org/examples/zh/)