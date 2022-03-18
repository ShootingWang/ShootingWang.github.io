---
title: 检验 | Mann-Whitney U检验
date: 2020-05-04 14:34:46
tags: [概率统计基础,检验]
categories: Statistics
math: true
mathjax: true
hide: true
notshow: true
index_img: /img/hypothesis.jpg
---

<center></center>
<!--more-->

# Mann-Whitney U 检验
- 非参数检验方法
- Mann-Whitney U 检验是用得最广泛的两独立样本秩和检验，是与独立样本t检验相对应的方法，当样本数据的正态性、方差齐性等不能达到t检验的要求时，可使用该检验
- 该检验的原假设假设从一个总体中随机选择的值小于或大于另一个总体中随机选择的值的可能性相同

不同叫法：
- Mann-Whitney U test
- Mann-Whitney-Wilcoxon（MWW）
- Wilcoxon rank-sum test
- Wilcoxon-Mann-Whitney test


Mann-Whitney U 检验的双边检验（two-tailed）的临界值表：
- [Critical Values of the Mann-Whitney Test (Two-Tailed Testing)](http://ocw.umb.edu/psychology/psych-270/other-materials/RelativeResourceManager.pdf)
- [Mann-Whitney Table](http://www.real-statistics.com/statistics-tables/mann-whitney-table/)

检验两组样本$\{X_1,X_2,\cdots,X_{n_1}\}$，$\{Y_1,Y_2,\cdots,Y_{n_2}\}$的均值是否有显著差别（假设这两组样本分别来自除了总体均值以外完全相同的两个总体）。
- 原假设下，$P(X_i>Y_j)=\frac{1}{2}$
- 备择假设下，$P(X_i>Y_j)\neq \frac{1}{2}$

1. 将两组数据混合，并求出每个数据的秩（按大小顺序排列）
     > 序列$\{ 3,5,2,4,5,8,9 \}$
     排序得到$\{2,3,4,5,5,8,9\}$，则数字2的秩为1，数字3的秩为2，数字4的秩为3；数字5排序有“第4”和第“5”，则数字5的秩为$\frac{4+5}{2}=4.5$；数字8的秩为6，数字9的秩为7。
     所以序列$\{3,5,2,4,5,8,9\}$的秩对应为$\{2,4.5,1,3,4.5,6,7\}$
2. 分别求出两组样本的秩和$R_x$、$R_y$
     > $R_x+R_y=\frac{N(N+1)}{2}=n_1n_2+\frac{n_1(n_1+1)}{2}+\frac{n_2(n_2+1)}{2}$
     其中$N=n_1+n_2$
     - 两组数据分别为$\{ 13,7,8,4,5 \}$和$\{3,9,2,12,4,10\}$
     - 混合排序后为$\{2,3,4,4,5,7,8,9,10,12,13\}$
     - 排序后的序列对应的秩为$\{1,2,3.5,3.5,5,6,7,8,9,10,11\}$
     - 原序列$\{13,7,8,4,5\}$在混合序列中的秩为$\{11,6,7,3.5,5\}$，秩和$R_x=11+6+7+3.5+5=32.5$
     - 原序列$\{3,9,2,12,4,10\}$在混合序列中的秩为$\{2,8,1,10,3.5,9\}$，秩和$R_y=2+8+1+10+3.5+9=33.5$
3. 计算Mann-Whitney U检验统计量
 $$U_x=\sharp \left\{ X_i > Y_j \right\}=R_x-\frac{n_1(n_1+1)}{2}=n_1n_2+\frac{n_2(n_2+1)}{2}-R_y$$

 $$U_y=\sharp \{ X_i < Y_j \}=R_y-\frac{n_2(n_2+1)}{2}=n_1n_2+\frac{n_1(n_1+1)}{2}-R_x$$
 其中$n_1$是第一组样本的样本量，$n_2$是第二组样本的样本量。
 取$U=\min\{W_1,W_2\}$；与临界值$U_\alpha$进行比较。当$U < U_\alpha$时，拒绝原假设$H_0$；否则，接受$H_0$。
     > - 当样本量足够大时（$n_1$和$n_2$都不小于10 时，或者$n_1n_2>20$时），随机变量$U$近似服从正态分布
     - $E(U)=\frac{n_1n_2}{2}$, $D(U)=\frac{n_1n_2(n_1+n_2+1)}{12}$
     - $U_x=32.5-\frac{5*6}{2}=17.5$
     - $U_y=33.5-\frac{6*7}{2}=14.5$
     - $U=\min\{17.5,14.5\}=14.5$
4. 假设第一个总体的均值为$\mu_1$，第二个总体的均值为$\mu_2$，临界值$U_\alpha$，则
     1. $H_0:\mu_1\leq \mu_2,\quad H_1:\mu_1>\mu_2$
     如果$U<-U_\alpha$，则拒绝$H_0$
     2. $H_0:\mu_1\geq \mu_2,\quad H_1:\mu_1<\mu_2$
     如果$U>U_\alpha$，则拒绝$H_0$
     3. $H_0:\mu_1\geq \mu_2,\quad H_1:\mu_1<\mu_2$
     如果$U>U_\alpha$，则拒绝$H_0$

## 性质
$$E(U)=\frac{n_1n_2}{2},\quad D(U)=\frac{n_1n_2(n_1+n_2+1)}{12}$$
证明：
定义$x_i$为$X_i\in\{X_1,X_2,\cdots,X_{n_1}\}$ 在序列$\{X_1,X_2,\cdots,X_{n_1},Y_1,Y_2,\cdots,Y_{n_2}\}$中的秩（$i=1,\cdots,n_1$;$N=n_1+n_2$），则有

$$
\begin{aligned}
E(x_i)&=\frac{1}{N}\sum_{i=1}^{N}i=\frac{1}{N}\frac{N(N+1)}{2}=\frac{N+1}{2}=\frac{n_1+n_2+1}{2}\\
\sum_{i=1}^Ni^2&=\frac{N(N+1)(2N+1)}{6}\\
\sum_{i=1}^N\sum_{j=1}^Nij&=\left(\sum_{i=1}^Ni\right)\left(\sum_{j=1}^Nj\right)=\frac{N^2(N+1)^2}{4}\\
\sum_{i\neq j}^Nij&=\sum_{i=1}^N\sum_{j=1}^Nij-\sum_{i=1}^Ni^2=\frac{N^2(N+1)^2}{4}-\frac{N(N+1)(2N+1)}{6}\\
E(x_i^2)&=\frac{1}{N}\sum_{i=1}^Ni^2=\frac{(N+1)(2N+1)}{6}\\
E(x_ix_j)&=\frac{1}{N(N-1)}\sum_{i\neq j}ij\\
&=\frac{N(N+1)^2}{4(N-1)}-\frac{(N+1)(2N+1)}{6(N-1)} \quad (\mathrm{for}\ i\neq j)\\
\mu_x=E(R_x)&=E\left(\sum_{i=1}^{n_1}x_i\right)=\sum_{i=1}^{n_1}E(x_i)=\frac{n_1(n_1+n_2+1)}{2}\\
Var(x_i)&=E(x_i^2)-E^2(x_i)\\
&=\frac{(N+1)(2N+1)}{6}-\left(\frac{N+1}{2}\right)^2=\frac{N^2-1}{12}\\
Cov(x_i,x_j)&=E(x_ix_j)-E(x_i)E(x_j)\\
&=\frac{N(N+1)^2}{4(N-1)}-\frac{(N+1)(2N+1)}{6(N-1)}-\left(\frac{N+1}{2}\right)^2=-\frac{N+1}{12}\\
\sigma^2_x=Var(R_x)&=Var\left(\sum_{i=1}^{n_1}x_i\right)=\sum_{i=1}^{n_1}Var(x_i)+\sum_{i\neq j}^{n_1}Cov(x_i,x_j)\\
&=\sum_{i=1}^{n_1}\frac{N^2-1}{12}+2\sum_{i<j}^{n_1}\left(-\frac{N+1}{12}\right)\\
&=n_1\frac{N^2-1}{12}+n_1(n_1-1)\left(-\frac{N+1}{12}\right)\\
&=\frac{n_1n_2}{12}(n_1+n_2+1) \\ 
\end{aligned}
$$

同理，有
$$\mu_y=E(R_y)=\frac{n_2(n_1+n_2+1)}{2},\quad \sigma_y^2=Var(R_y)=\frac{n_1n_2}{12}(n_1+n_2+1)$$
那么
\begin{equation}
  \begin{aligned}
E(U_1)&=E\left(R_x-\frac{n_1(n_1+1)}{2}\right)=\frac{n_1n_2}{2}\\
E(U_2)&=E\left(R_y-\frac{n_2(n_2+1)}{2}\right)=\frac{n_1n_2}{2}\\
Var(U_1)&=Var\left(R_x-\frac{n_1(n_1+1)}{2}\right)=Var(R_x)\\
Var(U_2)&=Var\left(R_y-\frac{n_2(n_2+1)}{2}\right)=Var(R_y)\\
E(U)&=E\left(\min\{U_1,U_2\}\right)=\frac{n_1n_2}{2}\\
Var(U)&=\frac{n_1n_2}{12}(n_1+n_2+1)
\end{aligned}
\end{equation}




# 参考资料
- [Statistical Analysis: Identifying Patterns](https://www.skillsyouneed.com/num/statistics-identifying-patterns.html)
- [Wiki: Mann–Whitney U test](https://en.wikipedia.org/wiki/Mann%E2%80%93Whitney_U_test)
- [The Mann-Whitney U Test](http://www.statstutor.ac.uk/resources/uploaded/mannwhitney.pdf)
- [Mann-whitney 检验算法学习](https://blog.csdn.net/qq_34734303/article/details/80296316)
- [Mann-Whitney Test for Independent Samples](http://www.real-statistics.com/non-parametric-tests/mann-whitney-test/)
- [Mann-Whitney Test – Advanced](http://www.real-statistics.com/non-parametric-tests/mann-whitney-test/mann-whitney-test-advanced/)