---
title: Jupyter Notebook使用技巧
date: 2020-04-30 00:59:06
tags: [Jupyter,Python]
categories: Python
mathjax: true
toc: true
index_img: /img/jupyter.png  ## 封面图片
---

<center>Jupyter Tips!</center>

<!--more-->
# 输出
## 输出结果显示图片
```Python
%matplotlib inline
```

## 同时输出多个命令的结果

1. 方法一：在文件开头添加以下命令（仅对当前文件有效）
 ```Python
 from IPython.core.interactiveshell import InteractiveShell
 InteractiveShell.ast_node_interactivity = "all"
 ```
2. 方法二：直接添加配置文件（对所有文件有效）
 ```Python
 vi ~/.ipython/profile_default/ipython_config.py
 ```
配置文件内容为：
 ```python
 c = get_config()
 #Run all nodes interactively
 c.InteractiveShell.ast_node_interactivity = "all"
 ```


## 禁止输出警告（warnings）

运行以下代码：
```Python
import warnings
warnings.filterwarnings("ignore")
```


# Kernel
## 安装R kernel

先在R中安装以下几个package：
```R
install.packages(c('repr', 'IRdisplay', 'evaluate', 'crayon', 
                   'pbdZMQ', 'devtools', 'uuid', 'digest'))
devtools::install_github('IRkernel/IRkernel')
```
在Anaconda Prompt中输入以下命令：
第一步：输入R，调用R
```
R
```
第二步：安装R kernel，有2种方式（自由选择，个人建议第二种）：
1. 直接安装在当前用户中：
 ```r
 IRkernel::installspec()
 ```
2. 安装在系统中：
 ```r
 IRkernel::installspec(user = FALSE)
 ```


# 其他
## 运行时间

在cell中添加 `%%time` ，返回结果中会含有cell单次运行的时间。
在cell中添加 `%timeit` ，会运行该cell 100,000次（默认），然后以运行最快的3次结果的平均值作为结果。
```Python
import numpy
%timeit numpy.random.normal(size = 10)
# 5.91 µs ± 258 ns per loop (mean ± std. dev. of 7 runs, 100000 loops each)
%%time
import time
for _ in range(100):
    time.sleep(0.01)
# Wall time: 1.73 s
```

## 插件管理器

Nbextensions相当于Jupyter的插件管理器。
安装：（在Anaconda Prompt中输入）
```bash
conda install -c conda-forge jupyter_contrib_nbextensions  
```
安装过程需要选择[y/n]，输入y。

该选项卡下方罗列了大量Jupyter可用的插件；点击某个插件的名称，即可在列表下方显示该插件的说明文档；勾选某个插件前面的方框，则系统会加载启用该插件。

## Jupyter Lab！

超好用！是Project Jupyter的下一代用户界面，具体安装和使用说明请见《[Jupyter：超强的下一代Jupyter Notebook](https://mp.weixin.qq.com/s/T-Afq0vAw0lVB9Bave-aNQ)》（原文作者Parul Pandey，EarlGrey翻译，[原文网址](https://towardsdatascience.com/jupyter-lab-evolution-of-the-jupyter-notebook-5297cacde6b?gi=dec00114bf03)）。

```tex
%% Jupyter Notebook的本地访问地址为
http://localhost:8888/tree

%% Jupyter Lab的本地访问地址为
http://localhost:8888/lab
```



## 切换Code和Markdown

要快捷地切换Cell的形式（Code或Markdown），可按如下操作：
选中Cell（光标在Cell中闪烁），点击“Esc”，进入命令模式（光标不再Cell中闪烁，但Cell左侧仍有蓝色粗线条）；然后键盘按“Y”将Cell切换为Code模式，键盘按“M”则将Cell切换为Markdown模式。即
- Esc+Y：code模式
- Esc+M：Markdown模式
- Esc+B：快速添加新的Cell




# 参考资料
- [首页缩略图](https://www.google.com/Jupyter)
- [Jupyter Notebook 如何让一个Cell 可以同时输出多个语句的值？](https://www.cnblogs.com/bella1102/p/11032519.html)
- [Jupyter Notebook/Lab中添加R Kernel的详细步骤](https://www.sohu.com/a/219989263_774914)
- [Jupyter Notebook的27个窍门，技巧和快捷键](https://blog.csdn.net/create115721/article/details/79243641)
- [九大神招，让Python里数据分析神器Jupyter，完美升华](https://mp.weixin.qq.com/s/5sFkpI4eEodVQuLya3K5rw)