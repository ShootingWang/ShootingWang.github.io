---
title: Python | cmath
date: 2020-11-25 22:58:33
tags: [Python]
categories: Python
mathjax: true
math: true
hide: true
notshow: true
---

<center></center>
<!--more-->

# cmath
进行复数相关的计算

复数：
$$z = x + y \mathrm{i}$$
- $x$：实部（real part）
- $y$：虚部（imaginary part）

复数$z$可以写成：
$$z = r(\cos \phi+\mathrm{i}\sin \phi)$$
- 模（modulus）$r = |z| = \sqrt{x^2 + y^2}$
- 辐角$\phi$（记作$\mathrm{Arg}(z)$）
 - 任意一个不为零的复数$z = x + y \mathrm{i}$的辐角有无限多个值，且这些值相差$k\cdot 2\pi$（$k$为任意整数）
- 辐角主值$\mathrm{arg}(z)$：在区间$[-\pi, \pi]$的辐角。
 - 辐角的主值是唯一的

复数还可写为指数形式：
$$z = r(\cos \phi+\mathrm{i}\sin \phi) = re^{\mathrm{i}\phi}$$

## abs()
计算复数的模
```python
abs(complex(-1, 0))
#1.0
```


## phase()
计算辐角

```python
import cmath

cmath.phase(complex(-1, 0))  ## 辐角为pi
#3.141592653589793
```

## polar()
计算复数的模、辐角

```python
import cmath

cmath.polar(complex(1, 2))
#(2.23606797749979, 1.1071487177940904)
print(*cmath.polar(complex(1, 2)), sep='\n')
#2.23606797749979
#1.1071487177940904
```