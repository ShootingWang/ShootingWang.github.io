---
title: 检验 | t检验
date: 2020-05-04 14:29:06
tags: [概率统计基础,检验]
categories: Statistics
math: true
mathjax: true
hide: true
notshow: true
index_img: /img/hypothesis.jpg
---

<center>t-test</center>
<!--more-->

- 适用于样本量小、总体方差未知的样本
- 研究两组数据是否存在差异

# $t$分布
Student's t-distribution[^1]

从均值为$\mu$、方差为$\sigma^2$的正态总体种抽取容量为$n$的随机样本，随机样本的
- 均值：$\bar{x}$
- 方差：$s^2=\frac{1}{n-1}\sum_{i=1}^n(x_i-\bar{x})^2$

则随机变量T
$$T=\frac{\bar{x}-\mu}{s/\sqrt{n}}$$
服从自由度为$n-1$的$t$分布，即$T\sim t(n-1)$。

- $t$分布是一种典型的长尾分布

[^1]: 'Student'是William Sealy Gosset的笔名

# $t$检验
## 单样本$t$检验
设$x_1,\cdots,x_n$是来自$N(\mu,\sigma^2)$的样本，考虑如下三种关于$\mu$的检验问题：
1. $$H_0:\mu\leq\mu_0\quad vs \quad H_1:\mu>\mu_0$$
2. $$H_0:\mu\geq\mu_0\quad vs \quad H_1:\mu<\mu_0$$
3. $$H_0:\mu=\mu_0\quad vs \quad H_1:\mu\neq\mu_0$$

对于上述检验问题.,若$\sigma$未知，则可用$t$检验统计量
$$t=\frac{\bar{x}-\mu_0}{s/\sqrt{n}}=\frac{\sqrt{n}(\bar{x}-\mu_0)}{s}$$
来进行检验。

其中$s$为样本标准差
$$s^2=\frac{1}{n-1}\sum_{i=1}^n(x_i-\bar{x})^2$$

对于给定的样本观测值计算得到的检验统计量$t$的值为
$$t_0=\frac{\sqrt{n}(\bar{x}-\mu_0)}{s}$$

在零假设$H_0:\mu=\mu_0$下，有
$t\sim t(n-1)$

在显著性水平为$\alpha$的情况下：
- 检验问题1：$H_0:\mu\leq\mu_0\quad vs \quad H_1:\mu>\mu_0$
 - 拒绝域为
  $$C_1=\{t\geq t_{1-\alpha}(n-1)\}$$
 - $p$值为
  $$p_1=P(t\geq t_0)$$
  其中$t$是服从自由度为$n-1$的$t$分布的随机变量
- 检验问题2：$H_0:\mu\geq\mu_0\quad vs \quad H_1:\mu<\mu_0$
 - 拒绝域为
  $$C_2=\{t\leq t_{\alpha}(n-1)\}$$
 - $p$值为
  $$p_2=P(t\leq t_0)$$
  其中$t$是服从自由度为$n-1$的$t$分布的随机变量
- 检验问题3：$H_0:\mu=\mu_0\quad vs \quad H_1:\mu\neq\mu_0$
 - 拒绝域为
  $$C_3=\{|t|\geq t_{1-\alpha/2}(n-1)\}$$
 - $p$值为
  $$p_3=P(|t|\geq |t_0|)$$
  其中$t$是服从自由度为$n-1$的$t$分布的随机变量

## 独立样本$t$检验
设$x_1,\cdots,x_m$是来自正态总体$N(\mu_1,\sigma_1^2)$的样本，$y_1,y_2,\cdots,y_n$是来自另一个正态总体$N(\mu_2,\sigma_2^2)$的样本，两个样本相互独立，其中
$$\sigma_1=\sigma_2=\sigma但未知$$
考虑如下三类检验问题：
1. $$H_0:\mu_1-\mu_2 \leq 0 \quad vs \quad H_1:\mu_1-\mu_2 > 0$$
2. $$H_0:\mu_1-\mu_2 \geq 0 \quad vs \quad H_1:\mu_1-\mu_2 < 0$$
3. $$H_0:\mu_1-\mu_2 = 0 \quad vs \quad H_1:\mu_1-\mu_2 \neq 0$$

已知$\bar{x}\sim N(\mu_1,\frac{1}{m}\sigma^2)$,$\bar{y}\sim N(\mu_2,\frac{1}{n}\sigma^2)$，且两个样本相互独立，则有
$$\bar{x}-\bar{y}\sim N\left(\mu_1-\mu_2, \left(\frac{1}{m}+\frac{1}{n} \right)\sigma^2\right)$$
所以有
$$\frac{(\bar{x}-\bar{y})-(\mu_1-\mu_2)}{\sqrt{\frac{1}{m}+\frac{1}{n}}\sigma}\sim N(0,1)$$


因为
$$\frac{(m-1)}{\sigma^2}s_x^2=\frac{1}{\sigma^2}\sum_{i=1}^m(x_i-\bar{x})^2\sim\chi^2(m-1)$$
$$\frac{(n-1)}{\sigma^2}s_y^2=\frac{1}{\sigma^2}\sum_{i=1}^n(y_i-\bar{y})^2\sim\chi^2(n-1)$$
所以有
$$\frac{(m+n-2)}{\sigma^2}s_w^2=\frac{1}{\sigma^2}\left(\sum_{i=1}^m(x_i-\bar{x})^2+\sum_{j=1}^n(y_j-\bar{y})^2 \right)\sim\chi^2(m+n-2)$$
其中
$$s_w^2=\frac{1}{m+n-2}\left(\sum_{i=1}^m(x_i-\bar{x})^2+\sum_{j=1}^n(y_j-\bar{y})^2 \right)$$

所以
\begin{aligned}
t&=\frac{N(0,1)}{\sqrt{\frac{\chi^2(m+n-2)}{m+n-2}}}\sim t(m+n-2)\\
&=\frac{\frac{(\bar{x}-\bar{y})-(\mu_1-\mu_2)}{\sqrt{\frac{1}{m}+\frac{1}{n}}\sigma}}{\sqrt{\frac{(m+n-2)}{\sigma^2}s_w^2/(m+n-2)}}\\
&=\frac{(\bar{x}-\bar{y})-(\mu_1-\mu_2)}{s_w\sqrt{\frac{1}{m}+\frac{1}{n}}} \sim t(m+n-2)
\end{aligned}

配对样本$t$检验的统计量为
$$t=\frac{\bar{x}-\bar{y}}{s_w\sqrt{\frac{1}{m}+\frac{1}{n}}} \sim t(m+n-2)$$

对给定样本，计算得到统计量的值为
$$t_0=\frac{\bar{x}-\bar{y}}{s_w\sqrt{\frac{1}{m}+\frac{1}{n}}}$$

在显著性水平为$\alpha$的情况下：
- 检验问题1：$H_0:\mu_1-\mu_2 \leq 0 \quad vs \quad H_1:\mu_1-\mu_2 > 0$
 - 拒绝域为
  $$W_1=\{t\geq t_{1-\alpha(m+n-2)}\}$$
 - $p$值为
  $$p_1=P(t\geq t_0)$$
- $H_0:\mu_1-\mu_2 \geq 0 \quad vs \quad H_1:\mu_1-\mu_2 < 0$
 - 拒绝域为
  $$W_2=\{t\leq t_{\alpha}(m+n-2)\}$$
 - $p$值为
  $$p_2=P(t\leq t_0)$$
- $H_0:\mu_1-\mu_2 = 0 \quad vs \quad H_1:\mu_1-\mu_2 \neq 0$
 - 拒绝域为
  $$W_3=\{|t|\geq t_{1-\alpha/2}(m+n-2)\}$$
 - $p$值为
  $$p_3=P(|t|\geq |t_0|)$$

## 配对样本

设配对样本为
$$\mathbf{X}=\{X_1,X_2,\cdots,X_n\}$$
$$\mathbf{Y}=\{Y_1,Y_2,\cdots,Y_n\}$$
令
$$\mathbf{Z}=\{Z_1,Z_2,\cdots,Z_n\}$$
其中
$$Z_i=X_i-Y_i$$
$i=1,2,\cdots,n$。

$\mu$是$Z$的总体均值，设假设为
$$H_0:\mu=0\quad \mathrm{vs} \quad H_1:\mu\neq0$$

检验统计量为
$$T=\frac{\overline{Z}}{S_z/\sqrt{n}}\sim t(n-1)$$
拒绝域为
$$W=\left\{|T|\geq t_{1-\frac{\alpha}{2}}(n-1) \right\}$$

p值为
$$p=2\cdot t_{n-1}(|T|)$$

# $t$检验 vs $u$检验
## 单样本

<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: center;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: center;">
      <th>检验法</th>
      <th>$H_0$</th>
      <th>$H_1$</th>
      <th>检验统计量</th>
      <th>拒绝域</th>
      <th>$p$值</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th rowspan="3">$u$检验<br>($\sigma$已知)</th>
      <td>$\mu\leq\mu_0$</td>
      <td>$\mu>\mu_0$</td>
      <td rowspan="3">$u=\frac{\bar{x}-\mu_0}{\sigma/\sqrt{n}}$</td>
      <td>$\{u\geq u_{1-\alpha}\}$</td>
      <td>$1-\Phi(u_0)$</td>
    </tr>
    <tr>
      <td>$\mu\geq\mu_0$</td>
      <td>$\mu < \mu_0$ </td>
      <td>$\{u\leq u_{\alpha}\}$</td>
      <td>$\Phi(u_0)$</td>
    </tr>
    <tr>
      <td>$\mu=\mu_0$</td>
      <td>$\mu\neq\mu_0$</td>
      <td>$\{|u|\geq u_{1-\alpha/2}\}$</td>
      <td>$2(1-\Phi(|u_0|))$</td>
    </tr>
    <tr>
      <th rowspan="3">$t$检验<br>($\sigma$未知)</th>
      <td>$\mu\leq\mu_0$</td>
      <td>$\mu>\mu_0$</td>
      <td rowspan="3">$t=\frac{\bar{x}-\mu_0}{s/\sqrt{n}}$</td>
      <td>$\{t\geq t_{1-\alpha}(n-1)\}$</td>
      <td>$P(T\geq t_0)$</td>
    </tr>
    <tr>
      <td>$\mu\geq\mu_0$</td>
      <td>$\mu < \mu_0$ </td>
      <td>$\{t\leq t_{\alpha}(n-1)\}$</td>
      <td>$P(T\leq t_0)$</td>
    </tr>
    <tr>
      <td>$\mu=\mu_0$</td>
      <td>$\mu\neq\mu_0$</td>
      <td>$\{|t|\geq t_{1-\alpha/2}(n-1)\}$</td>
      <td>$P(|T|\geq|t_0|)$</td>
    </tr>
    <tr>
  </tbody>
</table>
</div>

> - $u_0=\frac{\sqrt{n}(\bar{x}-\mu_0)}{\sigma}$
- $T$是服从自由度为$n-1$的$t$分布的随机变量
- $t_0=\frac{\sqrt{n}(\bar{x}-\mu_0)}{s}$

## 独立样本

<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: center;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: center;">
      <th>检验法</th>
      <th>$H_0$</th>
      <th>$H_1$</th>
      <th>检验统计量</th>
      <th>拒绝域</th>
      <th>$p$值</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th rowspan="3">$u$检验<br>($\sigma_1,\sigma_2$已知)</th>
      <td>$\mu_1\leq\mu_2$</td>
      <td>$\mu_1>\mu_2$</td>
      <td rowspan="3">$u=\frac{\bar{x}-\bar{y}}{\sqrt{\frac{\sigma^2_1}{m}+\frac{\sigma_1^2}{n}}}$</td>
      <td>$\{u\geq u_{1-\alpha}\}$</td>
      <td>$1-\Phi(u_1)$</td>
    </tr>
    <tr>
      <td>$\mu_1\geq\mu_2$</td>
      <td>$\mu_1<\mu_2$ </td>
      <td>$\{u\leq u_{\alpha}\}$</td>
      <td>$\Phi(u_1)$</td>
    </tr>
    <tr>
      <td>$\mu_1=\mu_2$</td>
      <td>$\mu_1\neq\mu_2$</td>
      <td>$\{|u|\geq u_{1-\alpha/2}\}$</td>
      <td>$2(1-\Phi(|u_1|))$</td>
    </tr>
    <tr>
      <th rowspan="3">$t$检验<br>($\sigma_1=\sigma_2$未知)</th>
      <td>$\mu_1\leq\mu_2$</td>
      <td>$\mu_1>\mu_2$</td>
      <td rowspan="3">$t=\frac{\bar{x}-\bar{y}}{s_w\sqrt{\frac{1}{m}+\frac{1}{n}}}$</td>
      <td>$\{t\geq t_{1-\alpha}(m+n-2)\}$</td>
      <td>$P(T_1\geq t_1)$</td>
    </tr>
    <tr>
      <td>$\mu_1\geq\mu_2$</td>
      <td>$\mu_1<\mu_2$ </td>
      <td>$\{t\leq t_{\alpha}(m+n-2)\}$</td>
      <td>$P(T_1\leq t_1)$</td>
    </tr>
    <tr>
      <td>$\mu_1=\mu_2$</td>
      <td>$\mu_1\neq\mu_2$</td>
      <td>$\{|t|\geq t_{1-\alpha/2}(m+n-2)\}$</td>
      <td>$P(|T_1|\geq|t_1|)$</td>
    </tr>
    <th rowspan="3">大样本$u$检验<br>($m,n$充分大)</th>
      <td>$\mu_1\leq\mu_2$</td>
      <td>$\mu_1>\mu_2$</td>
      <td rowspan="3">$u=\frac{\bar{x}-\bar{y}}{\sqrt{\frac{s_x^2}{m}+\frac{s_y^2}{n}}}$</td>
      <td>$\{u\geq u_{1-\alpha}\}$</td>
      <td>$1-\Phi(u_2)$</td>
    </tr>
    <tr>
      <td>$\mu_1\geq\mu_2$</td>
      <td>$\mu_1<\mu_2$ </td>
      <td>$\{u\leq u_{\alpha}\}$</td>
      <td>$\Phi(u_2)$</td>
    </tr>
    <tr>
      <td>$\mu_1=\mu_2$</td>
      <td>$\mu_1\neq\mu_2$</td>
      <td>$\{|u|\geq u_{1-\alpha/2}\}$</td>
      <td>$2(1-\Phi(|u_2|))$</td>
    </tr>
    <tr>
      <th rowspan="3">近似$t$检验<br>($m,n$没有很大)</th>
      <td>$\mu_1\leq\mu_2$</td>
      <td>$\mu_1>\mu_2$</td>
      <td rowspan="3">$t=\frac{\bar{x}-\bar{y}}{\sqrt{\frac{s_x^2}{m}+\frac{s_y^2}{n}}}$</td>
      <td>$\{t\geq t_{1-\alpha}(l)\}$</td>
      <td>$P(T_2\geq t_2)$</td>
    </tr>
    <tr>
      <td>$\mu_1\geq\mu_2$</td>
      <td>$\mu_1<\mu_2$ </td>
      <td>$\{t\leq t_{\alpha}(l)\}$</td>
      <td>$P(T_2\leq t_2)$</td>
    </tr>
    <tr>
      <td>$\mu_1=\mu_2$</td>
      <td>$\mu_1\neq\mu_2$</td>
      <td>$\{|t|\geq t_{1-\alpha/2}(l)\}$</td>
      <td>$P(|T_2|\geq|t_2|)$</td>
    </tr>
    <tr>
  </tbody>
</table>
</div>

> - $u_1=\frac{\bar{x}-\bar{y}}{\sqrt{\frac{\sigma_1^2}{m}+\frac{\sigma_2^2}{n}}}$
- $t_1=\frac{\bar{x}-\bar{y}}{s_w\sqrt{\frac{1}{m}+\frac{1}{n}}}$
- $T_1$是服从自由度为$m+n-2$的$t$分布的随机变量
- $u_2=\frac{\bar{x}-\bar{y}}{\sqrt{\frac{s_x^2}{m}+\frac{s_y^2}{n}}}$
- $t_2=\frac{\bar{x}-\bar{y}}{\sqrt{\frac{s_x^2}{m}+\frac{s_y^2}{n}}}$
- $T_2$是服从自由度为$l$的$t$分布的随机变量，其中$l$为
 $$l=\frac{s_x^2/m+s_y^2/n}{\frac{s_x^2}{m^2(m-1)}+\frac{s_y^2}{n^2(n-1)}}$$
 $l$一般不为整数，可以取与$l$最接近的整数代替之。


# $t$检验 vs $\chi^2$检验

<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: center;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: center;">
      <th></th>
      <th>$t$检验</th>
      <th>$\chi^2$检验</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>用途</th>
      <td>检验两组样本之间是否存在统计意义上的差异</td>
      <td>检验两个变量之间是否有关系</td>
    </tr>
    <tr>
      <th>原假设<br>Null Hypothesis</th>
      <td>两组样本的均值不存在统计意义上的差异<br>There is no statistical difference between the means of the two groups</td>
      <td>两个变量之间没有关系<br>There is no relationship between the two variables</td>
    </tr>
    <tr>
      <th>拒绝原假设意味着</th>
      <td>两组样本的均值具有显著差异<br>the means are statistically different</td>
      <td>两个变量之间存在一定关系<br>There is a relationship between the two variables.<br>(But, it does not tell you the direction or the size of the relationship.)</td>
    </tr>
    <tr>
  </tbody>
</table>
</div>



# 参考资料
- [Statistical Analysis: Identifying Patterns](https://www.skillsyouneed.com/num/statistics-identifying-patterns.html)
- [概率论与数理统计教程](https://book.douban.com/subject/5998092/)
- [数分面试-AB实验篇](https://zhuanlan.zhihu.com/p/116002163)
- [Differences between a T-Test and Chi Square](https://pages.ucsd.edu/~lspangle/CourseDocs/Ttest-v-ChiSquare.pdf)