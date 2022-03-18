---
title: R语言 | grpreg
date: 2021-02-10 09:06:17
tags: R
categories: R语言
mathjax: true
toc: true
---

<center></center>
<!--more-->

# grpreg包
包含
- 组选择方法（group selection methods）：
  - group lasso
  - group MCP
  - group SCAD
- 双层变量选择（bi-level selection methods）：
  - group exponential lasso
  - composite MCP
  - group bridge

```r
install.package('grpreg')  ## 安装
```

支持以下多种惩罚函数（penalities）：
- `grLasso`：Group Lasso（[Yuan & Lin, 2006](http://www.columbia.edu/~my2550/papers/glasso.final.pdf)）
- `grMCP`：Group MCP
- `grSCAD`：Group SCAD
- `cMCP`：Composite MCP（[Breheny & Huang, 2009](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC2904563/)）
- `gel`：Group Exponential Lasso（[Breheny, 2015](https://onlinelibrary.wiley.com/doi/abs/10.1111/biom.12300)）
- `gBridge`：Group Bridge（[Huang etal., 2009](https://www.jstor.org/stable/27798828)）


## cv.grpreg()
- 使用交叉验证方法确定最优的 $\lambda$ 值

```r
library(grpreg)


```

## grpreg()

```r

```

## plot.grpreg()

```r

```

# 参考资料
- [Package 'grpreg'](https://cran.r-project.org/web/packages/grpreg/grpreg.pdf)