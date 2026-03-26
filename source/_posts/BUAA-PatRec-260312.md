---
title: 模式识别笔记（2）：线性分类器
date: 2026-03-12 19:02:01
tags:
  - 学习笔记
  - 模式识别
---

# 线性分类器

## 线性判别函数

一般形式：

$$
g(x) = \sum w_i x_i + w_0 = \mathbf{w}^T\mathbf{x} + w_0
$$

$w_i$ 为加权因子，$w_0$ 为阈值权。

常用 $g=\mathbf{w}^T\mathbf{x}+w_0$ 简化表示。

二分类中用线性判别函数 $g$，规则为：

- $g<0\to\omega_1$.

- $g>0\to\omega_2$.

则判别平面即 $g=0$.


或采用增广矩阵：$\mathbf y=\lang 1\mid\mathbf x\rang$，因此可以把它简化成 $a^Ty$.

$a=\lang w_0\mid\mathbf{w}\rang$ 为增广权向量。在增广空间中 $a^Ty=0$ 确定一个过原点的分类面。

多分类转二分类：$A$ vs $\bar A$，或者做 $O(n^2)$ 个分类器一一做二分类，最后投票。

## 垂直平分分类器

是一种线性分类器。

分类面取两类均值连线的中垂线。即

$$
\begin{aligned}
\mathbf m_1 &=\frac 1{n_1}\sum_{P\sim\omega_1} P\\
\mathbf m_2 &=\frac 1{n_2}\sum_{Q\sim\omega_2} Q\\
\mathbf w   &=\mathbf m_1-\mathbf m_2\\
w_0         &=-w^T\frac{\mathbf m_1+\mathbf m_2}2
\end{aligned}
$$

然后以 $\mathbf w^Tx+w_0$ 为分类面。

## Fisher 投影

这里只讲 $\mathbb R^D\to\mathbb R$.

用 Fisher 降维。

思想：将二分类的样本投影到直线 $y=w^Tx$ 上，最大化均值之差、最小化类内方差。

具体做法：

先定义类内离散度矩阵

$$
S_1 = \sum_{P\sim\omega_1} (P-\mathbf m_1)(P-\mathbf m_1)^T.\\
S_2 = \sum_{Q\sim\omega_2} (Q-\mathbf m_2)(Q-\mathbf m_2)^T.\\
S_W=S_1+S_2
$$

然后是类间的离散度矩阵

$$
S_B=(m_1-m_2)(m_1-m_2)^T
$$

接下来最小化 Rayleigh 商：

$$
J(\mathbf w)=\frac{w^TS_Bw}{w^TS_ww}
$$

用 Lagrange 乘子法得到

$$
\mathbf w^\ast=S_w^{-1}(m_1-m_2).
$$

此时将样本点先投影到 $y=w^{\ast T}x$，然后在直线上取一个分割点（通常取 $(m_1+m_2)/2$ 即中点）。

## 感知机

唯一真神。

$$
y=f(w^Tx+\theta)
$$

用 $f$ 激活函数引入非线性部分。

由于类 2 的判据是内层 $<0$，为了方便将类 2 的分量都乘以 $-1$，这样 $a^Ty$ 都会落在第一象限（大于 $0$）。

用惩罚函数 $J$.

最小化 $J$，通过梯度下降。

