---
title: LaTeX
date: 2020-12-19 18:23:41
tags: [LaTeX]
categories: Everything
mathjax: true
math: true
---

<center></center>
<!--more-->

# 安装
## MacOS
在MacOS可以安装MacTeX，但是完整版较大，安装后显示为TeXShop；可以安装较小的[BasicTeX](http://www.tug.org/mactex/morepackages.html)，并安装编辑器TeXStudio（博主打不开官网）。
- TeXShop默认编码不是Unicode，需点击TeXShop——偏好设置，将编码改为“Unicode（UTF-8）”，以便能够支持中文编译。

## Windows
安装[CTEX](http://www.ctex.org/HomePage)，Windows对应的是[MiKTeX](https://miktex.org/)。





# 自定义命令
- `\newcommand{新命令}[参数个数]{原命令}`
- `\newcommand*{新命令}[参数个数]{原命令}`
- `\renewcommand{新命令}[参数个数]{原命令}`
- `\renewcommand*{新命令}[参数个数]{原命令}`

- 参数个数不能超过9个
- 带星号的称为“短命令”（不能出现段落）
- LATEX编译时用原命令内容替换新命令，同时将相应的参数代入
- 自定义命令本质上就是命令替换

自定义命令的作用范围：
1. 如果放在导言区，则全局有效
2. 如果放在正文中的某个环境或分组中定义，则仅在其所在环境或分组内部有效

# 表格tabular

```tex
\begin{tabular}[竖向位置]{列格式}
第一行 \\
... \\
第末行 \\
\end{tabular}
```

- 竖向位置：表格在竖直方向与外部文本行的相对位置，取值可以是t或b，缺省为居中
- 表格每行由若干列组成，列与列之间用&分隔，每行必须用 `\\` 结束
- 列格式：由若干项组成，用于指定各列的格式，如`\begin{tabular}{|c|c|c|}`
  - `l`：左对齐
  - `c`：居中
  - `r`：右对齐
  - `|`：边界线
- 横线：
  - `\hline`：与表格同宽的水平线
  - `\cline{m-n}`：从第m列开始到第n列结束的水平线
- 竖线：
  - `\vline`：在当前位置画一与行等高的竖线
- 合并相邻多列：`\multicolumn{列数}{列格式}{文本内容}`
  - 当列数=1时，可以改变当前列的对齐方式
  - 改变各行之间的间隔（改变行高）：`\\[长度]`


- [Tables Generator](https://www.tablesgenerator.com/latex_tables)：在线编辑表格，生成LaTex语法，可直接复制





# 幻灯片Beamer
> {% post_link LaTeX-Beamer %}

MacOS可以从“文件”中的“从模板新建”中选择“Beamer”新建幻灯片模板。

主题超市：
- [Beamer theme gallery](https://deic-web.uab.cat/~iblanes/beamer_gallery/)
- [OverLeaf Beamer主题Gallery](https://www.overleaf.com/gallery/tagged/presentation#.Wv7RVIiFPIU)

# 文档
## 摘要Abstract
- 在 `\maketitle` 之后用 `abstract` 环境生成

```tex
\begin{abstract}
摘要内容
\end{abstract}
```

## 目录
### 导引线

|符号|TeX|示例|
|:---|:------|:---:|
|省略号导引线|`\dotfill`|$\dotfill$|
||`\hrulefill`|$\hrulefill$|



## 脚注footnote
`\footnote{脚注内容}`
- 该脚注命令应紧接在需要注释的文字后面，排版后会在所在处显示一个脚注标记，并将脚注内容显示在当前页的底部，并带有相同的脚注标记
- 脚注标记是上标形式的数字，并自动编号
- 在`article`文档类中，整篇文章的脚注统一编号
- 在`book`和`report`文档类中，每章的脚注统一编号
- 脚注只能位于普通文本中，不能位于数学模式，表格，LR盒子等中

# 页面布局
## 页高度
增加当前页高度：有时可以避免难看的分页
1. `\enlargethispage{尺寸}` ：可增加的最大高度
2. `\enlargethispage*{尺寸}` ：严格指定增加高度

## 分段


## 分页
- 自动分页
- 强制分页：`\newpage`
- 建议分页：
  - `\pagebreak[n]`
  - `\nopagebreak[n]`

# 数学
## 公式
行间公式：
- 独占一行（单行公式）或多行（多行公式）
- 行间公式可以编号，也可以不编号
- 编号可以人工编号，也可以自动编号
- 行间公式中不能出现空行

单行公式
1. 不带编号：
```tex
\begin{displaymath}
...
\end{displaymath}
```
2. 上面的简化形式：`\[...\]`
3. 自动编号：
```tex
\begin{equation}
...
\end{equation}
```
4. 不自动编号：`$$...$$`；可使用`\eqno`或`leqno`人工编号

### equation环境
```tex
\begin{equation}
...
\end{equation}
```

- 公式自动编号
- equation环境中的公式可以是普通的**单行公式**，也可以是作为一个整体处理的**环境或盒子**
- 若要改变公式自动编号的值，可在公式前插入`\setcounter{equation}{整数}`，这里的equation为公式计数器，其后面的公式的编号自动为计数器的值加1
- 标记：`\label{公式标记}`，公式标志唯一
- 引用：`\ref{公式标记}`和`\eqref{公式标记}`

### eqnarray环境
```tex
%% 自动编号
\begin{equation}
...
\end{equation}

%% 不自动编号
\begin{equation*}
...
\end{equation*}
```

- eqnarray环境中每行公式都自动编号
- 若某行公式无需编号，可在该行公式后面的换行符前插入`\nonumber`
- eqnarray环境中每行的格式为`左边公式 & 中间公式 & 右边公式`
  - 总是按三列排版公式，每行至多三列
  - 三列的对齐方式分别为：左对齐，居中，右对齐
  - 同一行中各列之间用 & 隔开
- eqnarray环境内部修改计数器equation的值
- 紧跟在行间公式后面的文本不会自动缩进
- 每行公式以 `\\` 结束（最后一行除外）
- 修改单个行间距：`\\[高度]`

## 角标

|符号|TeX|示例|
|:---|:------|:---:|
|上标|`^{...}`|`$x^2$`即$x^2$|
|下标|`_{...}`|`$x_i$`即 $x_i$|

- 上标、小标角标命令必须在数学模式中使用
- 多层角标需要使用分组符号
- 一级角标字体大小约7pt，二级及以上角标字体大小约为5pt
- **中文角标**要放入盒子，并指定字体大小`$x^{\mbox{\scriptsize 平方}}$`
- 可以根据需要手工改变角标字体大小或层次

## 字体

|符号|TeX|示例|
|:---|:------|:---:|
|实数R（空心）|`\mathbb{R}`|$\mathbb{R}$|

## 运算符号
|符号|TeX|示例|
|:---|:------|:---:|
|求和（求和上下标在右边）|`\sum_{ }^{ }`|`\sum_{i=1}^{100}` 即 $\sum_{i=1}^{100}$|
|求和（人工指定上下限的位置）|`\limits`、`\nolimits`||
|求和（上下标在求和符号上下方）|`\sum\nolimits{ }\limits{ }`|`\sum\nolimits{i=1}\limits{n}` 即 $\sum\nolimits{i=1}\limits{n}$|

## 几何符号
|符号|TeX|示例|
|:---|:------|:---:|
|垂直|`\perp`|`x\perp y` 即 $x\perp y$|
|单条竖线|`\mid`|`\mid x\mid` 即 $\mid x\mid$|
|双条竖线|`\parallel`|`\parallel x\parallel` 即 $\parallel x\parallel$|


## 常用数学符号

|符号|TeX|示例|
|:---|:------|:---:|
|导数符号|`\prime` 或 单引号`'`|`$f^\prime(x)$` 即 $f^\prime(x)$|



  - ：


# 其他

## 长度单位
常用的长度单位：

|单位|含义|
|:--:|:-----|
|mm|毫米|
|cm|厘米|
|in|英寸|
|pt|点|
|em|大写字母M的宽度|
|ex|小写字母x的高度|

- TeX长度由十进制小数加长度单位表示
- em、ex与当前字体尺寸相关
- 1 in = 2.54 cm = 72.27 pt

# 资源
## 字体
- [LaTex Font Catalogue](https://tug.org/FontCatalogue/)


```tex

```

```tex

```

```tex

```

```tex

```

```tex

```

```tex

```

# 参考资料
