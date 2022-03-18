---
title: Markdown语法
date: 2021-02-12 11:35:48
tags: Markdown
categories: Everything
mathjax: true
math: true
---

<center></center>
<!--more-->

# 标题
井号键`#`（N个代表N级标题） +` `（空格）+ `标题内容`

```markdown
# 一级标题
## 二级标题
### 三级标题
```

## 二级标题
示范`## 二级标题的效果`

# 正文
## 加粗
```markdown
**加粗的内容**
```

效果：**加粗的内容**

## 下划线
```markdown
<u>需要下划线的内容</u>
```

效果：<u>需要下划线的内容</u>

## 删除线
```markdown
~~需要加删除线的内容~~
```

效果：~~需要加删除线的内容~~

## 引用
```markdown
> 引用的内容
```

效果：
> 引用的内容

## 超链接
```markdown
[显示的名称](链接)
```

> 点击即可跳转

效果：[点击即可跳转到首页](https://shootingwang.github.io/)


## 列表
### 有序列表
`1.` + 空格 + 内容

```markdown
1. 第一个item内容
2. 第二个item内容
```

效果：
1. 第一个item内容
2. 第二个item内容


### 无序列表
`-` + 空格 + 内容 或`*` + 空格 + 内容

```markdown
- 第一个内容
- 第二个内容
* 第三个内容
```

效果：
- 第一个内容
- 第二个内容
* 第三个内容

> 通过缩进可以嵌套多层列表


# 公式
## 行内公式

```markdown
$...$
```

```markdown
From [Wiki: Lasso](https://en.wikipedia.org/wiki/Lasso_(statistics)):
Consider a sample consisting of $N$ cases, each of which consists of $p$ covariates and a single outcome. Let $y_{i}$ be the outcome and $x_{i}:=(x_{1},x_{2},\ldots ,x_{p})^{T}$ be the covariate vector for the ith case. Then the objective of lasso is to solve
$$\min_{\beta_0, \beta} \left\{ \sum_{i=1}^N (y_i - \beta_0 - x_i^T \beta)^2 \right\} \quad \mathrm{subject\, to } \sum_{j=1}^p |\beta_j|\leq t.$$
```

效果：
From [Wiki: Lasso](https://en.wikipedia.org/wiki/Lasso_(statistics)):
Consider a sample consisting of $N$ cases, each of which consists of $p$ covariates and a single outcome. Let $y_{i}$ be the outcome and $x_{i}:=(x_{1},x_{2},\ldots ,x_{p})^{T}$ be the covariate vector for the ith case. Then the objective of lasso is to solve

$$\min_{\beta_0, \beta} \left\{ \sum_{i=1}^N (y_i - \beta_0 - x_i^T \beta)^2 \right\} \quad \mathrm{subject\, to } \sum_{j=1}^p |\beta_j|\leq t.$$


```markdown

```

```markdown

```

```markdown

```


```markdown

```

# 参考资料