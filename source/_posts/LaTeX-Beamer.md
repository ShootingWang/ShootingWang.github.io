---
title: LaTeX | Beamer幻灯片
date: 2021-04-18 23:03:20
tags: LaTeX
categories: Everything
mathjax: true
math: true
---

<center>使用LaTeX制作幻灯片</center>
<!--more-->

# 基本
## 编译
使用`XeLaTeX`程序编译

## 中文
使用`ctex`宏包
```tex
\usepackage[UTF8]{ctex}
```

- 可以在载入`ctex`宏包时加上`noindent`选项，取消段落的缩进
- 在WinEdt编辑器中，对中文默认不是`UTF8`编码，需要在新建文件后，在第一行写上`% -*- coding: utf-8 -*-`，保存并关闭文件，再打开文件

## 主体结构
幻灯片主体结构：
```tex
\section{章名}
\subsection{节名}
\begin{frame}[选项]
\frametitle{幻灯片标题}
幻灯片内容
\end{frame}

```

## 对齐
在Beamer的每张幻灯片中，正文内容（不包括幻灯片标题）默认为“竖直居中”的
- `t`：竖直居上（top）
- `c`：竖直居中（center）
- `b`：竖直居下（bottom）

```tex
% 将全局对齐格式改为“竖直居上”
\documentclass[t]{beamer}

% 将某张幻灯片的正文内容修改为竖直居下
\begin{frame}[b]
幻灯片内容
\end{frame}
```

# 主题

> - [Beamer主题Gallery](https://deic-web.uab.cat/~iblanes/beamer_gallery/)

## 演示主题
```tex
\usetheme{theme name}
```

`theme name`可以在[https://deic-web.uab.cat/~iblanes/beamer_gallery/index_by_theme.html](https://deic-web.uab.cat/~iblanes/beamer_gallery/index_by_theme.html)上查看各主题风格预览，选择你想要的主题名称填入

|类别|theme name|
|:---|:-----|
|无导航栏|`default`、`boxes`、`Bergen`、`Pittsburgh`、`Rochester`|
|带顶部导航栏|`Antibes`、`Darmstadt`、`Frankfurt`、`JuanLesPins`、`Montpellier`、`Singapore`|
|带底部导航栏|`Boadilla`、`Madrid`|
|带顶部&底部导航栏|`AnnArbor`、`Berlin`、`CambridgeUS`、`Copenhagen`、`Dresden`、`Ilmenau`、`Luebeck`、`Malmoe`、`Szeged`、`Warsaw`|
|带侧边栏|`Berkeley`、`Goettingen`、`Hannover`、`Marburg`、`PaloAlto`|

## 色彩主题
设置幻灯片各部分、各结构、各元素的配色
- [Beamer color theme gallery](https://deic-web.uab.cat/~iblanes/beamer_gallery/index_by_color.html)（预览各颜色主题样式）

```tex
\usecolortheme{color theme name}
```

|类别|color theme name|
|:----|:---------|
|基本颜色|`default`、`sidebartab`、`structure`|
|完整颜色|`albatross`、`beaver`、`beetle`、`crane`、`dove`、`fly`、`seagull`、`wolverine`|
|内部颜色|`lily`、`orchid`、`rose`|
|外部颜色|`dolphin`、`seahorse`、`whale`|

```tex
%% 设置Beamer元素的颜色
\setbeamercolor{Beamer_element}{color}

%% 例：
\setbeamercolor{frametitle}{fg=blue,bg=yellow}     % 修改幻灯片标题颜色
\setbeamercolor*{item projected}{fg=white, bg=itemizecolor}    % 目录中编号字体颜色为fg，编号图标颜色为bg
\setbeamercolor{itemize item}{fg=itemizecolor}  	% 修改无序列表颜色
\setbeamercolor{enumerate item}{fg=itemizecolor}  	% 修改有序列表颜色
\setbeamercolor{description item}{fg=itemizecolor}  		% 修改描述列表“描述项”颜色
\setbeamercolor{section in toc}{fg=black}		% 设置目录中标题的颜色
```



## 字体主题
设置幻灯片的字体
- [Beamer font theme gallery](https://deic-web.uab.cat/~iblanes/beamer_gallery/index_by_font.html)（预览各字体主题样式）

```tex
\usefonttheme{font theme name}
```

|font theme name|描述|
|:---|:-----|
|`default`||
|`serif`||
|`structurebold`||
|`structureitalicserif`||
|`structuresmallcapsserif`||


## 内部主题
设置幻灯片正文内容（标题、列表、定理等）的样式
```tex
\useinnertheme{inner theme name}
```

|inner theme name|描述|
|:----|:---------|
|`default`||
|`circles`||
|`rectangles`||
|`rounded`||


## 外部主题
设置幻灯片是否有顶部导航栏、底部导航栏、侧边栏，以及他们的结构
```tex
\useoutertheme{outer theme name}
```

|outer theme name|描述|
|:----|:---------|
|`default`||
|`infolines`||
|`miniframes`||
|`sidebar`||
|`smoothbars`||
|`smoothtree`||
|`split`||
|`shadow`||
|`tree`||








# 封面/标题页
## 封面信息
```tex
\title[导航区报告标题]{封面报告标题}
\author[导航区作者名]{封面作者名}
\institute[导航区院系]{封面院系}
\date{日期}
% \date{\today}：今天
% 例：\date{\small \vskip -10pt \today} 或 \date{2021年04月}
```

## 标题页
- 用`\titlepage`生成标题页
- 一般是第一张幻灯片

```tex
\begin{frame}[plain]
    \titlepage
\end{frame}
% plain：不显示顶部导航区、侧边栏、底部导航区等外部元素
```

# 目录
使用`\tableofcontents`生成目录页：
```tex
\section*{目录}  % *表示章“目录”不出现在目录中
\frame{
    \frametitle{\secname}  % 幻灯片标题
    % \secname：与章（section）同名
    \tableofcontents[hideallsubsection]  % 目录
    % hideallsubsection：隐藏所有子标题（节）
}
```

```tex
\setbeamertemplate{section in toc}[sections numbered]  	% 目录显示标题的编号
\setbeamertemplate{section in toc}[square]   	% 标题前图标为square（方块）
```

# 正文
## 新建幻灯片
```tex
% 方式一：
\begin{frame}{幻灯片主题}{幻灯片副标题}
幻灯片内容
\end{frame}

% 方式二：
\frame{
    \frametitle{幻灯片主题}
    \framesubtitle{幻灯片副标题}
    幻灯片内容
}

% 方式三：
\begin{frame}
    \frametitle{幻灯片主题}
    \framesubtitle{幻灯片副标题}
    幻灯片内容
\end{frame}
```

## 列表 list
### 无序列表
```tex
\begin{itemize}
    \item 项
    \item 项
\end{itemize}
```

```tex
\setbeamertemplate{itemize item}[triangle]  		% 修改无序列表图标
\setbeamercolor{itemize item}{fg=itemizecolor}  	% 修改无序列表颜色
% default
% triangle：三角形
% circle：小圆点
% square：正方形
% ball：球形
```


### 有序列表
```tex
\begin{enumerate}
    \item 第一项
    \item 第二项
    \item 第三项
\end{enumerate}
```

## 描述列表 description
```tex
\begin{description}
    \item[红色] 热情、活泼、温暖、幸福
    \item[绿色] 新鲜、平静、安逸、柔和
\end{description}
```

## 区块环境 block

```tex
\begin{block}{重要内容}
内容
\end{block}
```

## 提醒环境 alertblock

```tex
\begin{alertblock}{重要提醒}
内容
\end{alertblock}
```

## 例子环境 exampleblock

```tex
\begin{exampleblock}{重要例子}
内容
\end{exampleblock}
```

## 定理环境

## 图片 figure
```tex
\begin{figure}
    \includegraphics[width=0.6\linewidth]{图片名称}
    \caption{图片标题}
\end{figure}
```


# 汇总
```tex
% 设置文档类别为 beamer
\documentclass[16pt]{beamer} 
%\\\ []中选项：
% 16pt：全局字体大小
% compress：尽量压缩导航栏


% ----------------- 主题 ------------------- %
\usetheme{theme name}   % 幻灯片主题



% ------------------ 主体 ------------------- %
% 开始写文章
\begin{document}


% ----------------- 封面信息 ------------------- %
\title[导航区报告标题]{封面报告标题}
\author[导航区作者名]{封面作者名}
\institute[导航区院系]{封面院系}
\date{日期}
% \date{\today}：今天
% 例：\date{\small \vskip -10pt \today}


% ----- 封面 ----- %
\begin{frame}
    \maketitle  % 或 \titlepage
\end{frame}


% ----------------- 目录页 ------------------- %
\section*{目录}  % *表示章“目录”不出现在目录中
\frame{
    \frametitle{\secname}  % 幻灯片标题
    % \secname：与章（section）同名
    \tableofcontents[hideallsubsection]  % 目录
    % hideallsubsection：隐藏所有子标题（节）
}


% ----------------- 正文 ------------------- %
\section{绪论}
\subsection{研究背景和意义}

\begin{frame}{幻灯片标题}
    内容
\end{frame}

\begin{frame}{幻灯片标题2}
    \begin{figure}
        \includegraphics[width=0.6\linewidth]{图片名称}
        \caption{图片标题}
    \end{figure}
\end{frame}




\end{document}




```


# 参考资料
- [HitSzBeamer](https://mirror.las.iastate.edu/tex-archive/macros/latex/contrib/beamer-contrib/themes/hitszbeamer/hitszbeamer.pdf)
- [Beamer演示学习笔记](https://bbs.pku.edu.cn/attach/cb/40/cb401e254626b3f9/beamerlog-1112.pdf)
- [Beamer v3.0 指南](http://static.latexstudio.net/article/2019/0623/beamer_guide-zh-cn-byl00l.pdf)