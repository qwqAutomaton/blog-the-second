---
title: 死鸡问题：解析组合初探？
date: 2026-01-23 00:36:06
tags:
- 数学
description: 乎上看到的有趣问题。
---

# 死鸡问题

## 原问题

[原问题](https://www.zhihu.com/question/1950568727587317333) 转述一下：

> 有 $n=10^6$ 只鸡，重复 $m=10^6$ 次操作，每次操作等可能地取一只有腿的鸡，砍掉一条腿。问最终有腿鸡数的期望。每只鸡当然是两条腿。

用 ODT 自然轻松解决。然而这并不能处理 $n\not\sim m$ 的情况。因此考虑朴素的求解。这里的求解参考了 [Equation 的回答](https://www.zhihu.com/question/1950568727587317333/answer/1997461316059804240)，似乎是一种很新的解析组合？

为了可扩展性，下面刻意不将 $n,m$ 视作相同的数。

## 建模

不妨设最终腿数为 $0,1,2$ 的鸡数量分别为 $a,b,c$，那么容易根据题设得到关系：

$$
\begin{cases}
a+b+c&=n&&\text{总数为 }n\\
2a+b&=m&&\text{总共砍掉的腿数为 }m
\end{cases}
$$

相减得到 $a-c=m-n=0$ 即完好（两条腿）鸡和死（没腿）鸡数量相等。这是源于题设中 $n=m$ 的性质。因此我们只需要研究死鸡数（实际上本来也只需要研究死鸡数）。为了对称，不妨再令 $k:=a=c$. 则最终的结果就是 $n-\mathbb Ek=n-\sum kp_k$，且

$$
\begin{cases}
a&=k\\
b&=m-2k\\
c&=k-m+n=k
\end{cases}
$$

接下来怎么做？不妨关注整条路径，也就是记录 $m$ 次杀的鸡的编号得到的一个序列 $\mathcal C=\lang c_1,c_2\cdots,c_m\rang$（懒了，接下来用 `mathcal` 字体表示路径），表示第 $i$ 次砍的是 $c_i$ 号鸡的腿。

## 路径相关

有了路径，然后呢？

并不是所有的路径都合法。你不能砍一只死鸡。因此我们要求合法的路径满足集合 $\{i|c_i=t\}$ 的大小对所有的 $1\le t\le m$ 而言都不大于 $2$，即 $t$ 号鸡最多砍两次。下文中同样偷懒，用 $|S|$ 表示集合 $S$ 的大小（元素个数）。

对于合法的路径 $\mathcal C$，它发生的概率可以写作：

$$
\mathbb P(\mathcal C)=\prod_{i=1}^m \dfrac{1}{L_i}
$$

其中 $L_i$ 表示按照路径 $\mathcal C$ 的砍法，第 $i$ 次砍腿 **前** 还活着（腿数 $>0$、能被砍）的鸡的个数。

我们注意到，腿被砍光或 **死亡** 是一个很重要的事情。由于我们假设最终死了 $k$ 只鸡，我们再关注一个死亡序列 $\mathcal K=\lang k_1,k_2,\cdots,k_k\rang$，按顺序记录鸡的死亡情况（第 $k_i$ 步中砍死了一只鸡）。显然 $k_i\in\{1,\cdots,m\}$. 不过为了方便描述，我们人为地在 $\mathcal K$ 头尾添加两项 $k_0=0$ 和 $k_{k+1}=m+1$，即让 $\mathcal K=\lang 0,k_1,\cdots,k_k,m+1\rang$. 

那么如果一条路径 $\mathcal C$ 描述的砍鸡腿过程中的死亡序列是 $\mathcal K$，就简称这个 $\mathcal C$ 服从 $\mathcal K$，记作 $\mathcal C\sim \mathcal K$. 

不妨将所有的路径按服从的死亡集合分组。对于某条 $\mathcal C\sim \mathcal K$，不难得到上面需要求出的每个 $L_i$ 实际上就是 $N-i\text{ 之前死掉的个数}$，于是就变成了

$$
\mathbb P(\mathcal C)=\left (n^{k_1}\times(n-1)^{k_2}\times\cdots(n-k)^{k_k}\right )^{-1}
$$

这样就可以很方便地求出所有服从某一组 $K$ 的路径总概率，即 

$$
\mathbb P\{\mathcal C:\mathcal C\sim K\}=\sum_{\mathcal C\sim \mathcal K}\mathbb P(\mathcal C)=\dfrac{\left|\{\mathcal C\mid\mathcal C\sim K\}\right|}{\displaystyle\prod_{i=0}^k (n-i)^{k_{i+1}-k_i}}
$$

于是我们的任务就变成求分子这个集合的元素个数了。概率问题变成了计数问题。

## 数数

不难发现，每一个死亡步 $k_i$ 都必须要有某只鸡被砍死。~~废话...~~

然后考虑每只鸡。如果这只鸡最终死了，那么它一定经历过恰好 $2$ 次砍腿，且最后一次砍腿一定在某一个死亡步上。

这其实很像括号匹配，在右括号（死亡步）已知时，求左括号（砍掉第一条腿）的分配情况。也就是说，我们总共由 $k$ 个右括号，于是就有 $m-k$ 个左括号。而且，我们要要求第 $r$ 个右括号（即第 $k_r$ 步）之前必须有至少 $r$ 个左括号（即使已经被匹配过）。不管怎么样，最终一定有 $k$ 死、$m-2k$ 瘸（一条腿）、$n-m+k$ 完好。

由于最终砍哪只鸡都无所谓（无标号），因此前面还要乘以一个人为标号导致的排列系数。这个因子是（那一坨分式是将三类鸡编号的方案数）：

$$
\dfrac{n!}{k!(m-2k)!(k-m+n)!}\times \text{接下来导致的编号}
$$

接下来就是数左右括号了。

### 数左括号

先算左括号，也就是砍一条腿的情况。

我们先把整个操作区间 $\{1,2,\cdots,m\}$ 按照 $K$ 集合分成 $k+1$ 段，也就是：

- 第 $0$ 段：$1\sim k_1-1$ 即 $k_0+1\sim k_1-1$;
- 第 $1$ 段：$k_1+1\sim k_2-1$;
- $\vdots$
- 第 $k$ 段：$k_k+1\sim m$ 即 $k_k+1\sim k_{k+1}-1$.

记第 $i$ 段的长度为 $l_i=k_{i+1}-k_{i}-1$.

接下来分类。对于第 $i$ 段而言，此时砍的鸡一定是完好鸡。如果砍的鸡：

- 将来会被砍死：设在这一段中出现 $a_i$ 次（天哪拉丁字母好少）。
- 将来不会被砍死（只是砍一条）：这一段出现 $l_i-a_i$ 次。

这里因为砍不砍死很难统计，就直接设一组 $a_i$ 解决。那么在给定这一组 $\mathcal A=\lang a_0,\cdots,a_{k}\rang$ 的情况下，选择所有左括号的方案数（注意不是总 $\mathcal C\sim\mathcal K$ 的数量）为

$$
\prod_{i=0}^k{l_i\choose a_i}=\prod_{i=0}^k {k_{i+1}-k_i-1\choose a_i}
$$

而 $a_i$ 需要满足的条件则是：

- 不能让某一次死亡“无鸡可死”： $\sum_{i=0}^ra_i\ge r$ 对所有 $1\le r\le k$ 成立；
- 总共恰死了 $k$ 只鸡：$\sum_{i=0}^ka_i=k$;
- 粗的范围： $0\le a_i\le l_i$.

此时的总左括号方案数为

$$
\sum_{\substack{
    \sum a=k\\
    a_i\ge 0\\
    a_0+\cdots+a_r\ge r
    }
}\prod_{i=0}^k{l_i\choose a_i}
$$

这里用的是组合数，所以需要额外乘以一个编号导致的系数（也就是前面提到的“接下来导致的编号”）。同样将 $a_i$ 和 $l_i-a_i$ 分别用死鸡和瘸鸡编号，得到缺的系数为 $(m-2k)!\times k!$. 前面需要补的系数也就变成 $\dfrac{n!}{(n-m+k)!}$.

### 数右括号

右括号（砍最后一条腿）比较简单。

由于第 $i$ 个右括号一定在 $k_i$ 处，并且前面一定有 $a_0+\cdots+a_{i-1}-(i-1)$ 个 **未匹配的** 左括号（总数为部分和，减掉匹配的 $i-1$ 个）。因此在固定 $\mathcal A$ 时，右括号的取值总数为

$$
\prod_{i=1}^k(a_0+\cdots+a_{i-1}-(i-1))
$$

由于我们这里已经将标号算在内了，不需要额外乘什么系数。

## 计算

根据上面的推导，某个路径 $\mathcal C$ 属于一个特定死亡序列 $\mathcal K$ 的概率是

$$
\begin{aligned}
&\mathbb P\{\mathcal C:\mathcal C\sim \mathcal K\}\\
=&\dfrac{\dfrac{n!}{(n-m+k)!}\times\displaystyle\sum_{\substack{
    \sum a=k\\
    a_i\ge 0\\
    a_0+\cdots+a_r\ge r
    }
}\left[\left(\prod_{i=0}^k{l_i\choose a_i}\right)\left(\prod_{i=1}^k(a_0+\cdots+a_{i-1}-(i-1))\right)\right]}{\displaystyle\prod_{i=0}^k (n-i)^{k_{i+1}-k_i}}\\
\end{aligned}
$$

所以总的概率（用各段长 $\ell_i=k_{i+1}-k_i$ 替代 $k$，那么 $l_i=\ell_i-1$，且诸 $\ell$ 为正整数、总和为 $m$）就是

$$
\begin{aligned}
&\mathbb P_{n,m}\{\text{死了 }k\text{ 只鸡}\}\\
=&\sum_{\mathcal K:|\mathcal K|=k}\mathbb P\{\mathcal C:\mathcal C\sim\mathcal K\}\\
\\
=&\sum_{\mathcal L:|\mathcal L|=k+1}\dfrac{\dfrac{n!}{(n-m+k)!}\times\displaystyle\sum_{\substack{
    \sum a=k\\
    a_i\ge 0\\
    a_0+\cdots+a_r\ge r
    }
}\left[\left(\prod_{i=0}^k{l_i\choose a_i}\right)\left(\prod_{i=1}^k(a_0+\cdots+a_{i-1}-(i-1))\right)\right]}{\displaystyle\prod_{i=0}^k (n-i)^{\ell_i}}\\
\end{aligned}
$$

所求的期望就是

$$
\mathbb Ek=\sum_{k=0}^nk\times \mathbb P_{n,m}\{\text{死了 }k\text{ 只鸡}\}=\text{吓哭了}
$$

这一坨都是什么...

硬做吧。

## 解析组合？

我们需要一些注意力。先用各区间的长度代替分割点 $k$，即：

- $\ell_i=k_{i+1}-k_i>0$
- $\sum \ell=m$

则 $l_i=\ell_i-1$.

**注意到** 这里需要使用某种生成函数，并将那一大坨求和看成是两个生成函数系数之比（不过对于概率问题，这倒也合理）。

先处理那一大坨对 $\mathcal A$ 的求和。我们 **注意到** 这个 $a$ 的限制很 Catalan（实际上在推导的时候也是类比了左右括号，因此 Catalan 不难注意），因此尽量往 Catalan 的生成函数上考虑。

于是单独把这个和拎出来：

$$
\boxed{A(k)=\sum_{\substack{
    \sum a=k\\
    a_i\ge 0\\
    a_0+\cdots+a_r\ge r
    }
}\left[\left(\prod_{i=0}^k{\ell_i-1\choose a_i}\right)\left(\prod_{i=1}^k(a_0+\cdots+a_{i-1}-(i-1))\right)\right]}
$$

后面这一大坨很难受。因此考虑令 $b_i=a_0+\cdots+a_i-i$ 为部分和减去标号的差。那么翻译一下限制：

- $b_k=0$;
- $b_r\ge 0$ 对所有的 $r=0\cdots k$;
- $b_r-b_{r-1}=a_r-1\ge -1$.
- $b_0=a_0$ 显然。

并用之前的 $l_i$ 替换 $\ell_i-1$（后面会换回来的），那么被求和的柿子就变成

$$
\begin{aligned}
A=&\sum_{\mathcal B}\left[\left(\prod_{i=1}^k b_i\right)\prod_{i=0}^k{l_i\choose b_i-b_{i-1}+1}\right]\\
=&\sum_{\mathcal B}{l_0\choose b_0}\prod_{i=1}^k\left(b_i\cdot{l_i\choose b_i-b_{i-1}+1}\right)
\end{aligned}
$$

这里的一条路径 $\mathcal B$ 就是一条：从 $b_0\ge 1$ 出发、每次前进任意步或至多后退一步、第 $k$ 步之后回到 $0$ 的路径。

> 闲话：这种路径应该叫 Dyck 路径，性质似乎不错（而且非常 Catalan）。

**注意到** 被求和的考虑求一下 $A(k)$ 关于 $k$ 的递归式。将整条路径按照第一次到达 $0$ 前后划分（设 $b_m=0$ 且 $b_{m'}>0$ 对所有的 $m'<m$），那么此时这条路径对总和的贡献就是

$$
\begin{aligned}
&{\ell_0-1\choose a_0}\prod_{i=1}^k\left(b_i\cdot{\ell_i-1\choose b_i-b_{i-1}+1}\right)\\
=&{\ell_0-1\choose a_0}\left[\prod_{i=1}^m\left(b_i\cdot{\ell_i-1\choose b_i-b_{i-1}+1}\right)\right]\left[\prod_{i=m+1}^k\left(b_i\cdot{\ell_i-1\choose b_i-b_{i-1}+1}\right)\right]
\end{aligned}
$$

... 算了求不出来

还是看看渐进吧。

记

$$
Z_{n,m}(k)=\mathbb P\{死了k只鸡\}=系数\times\sum_{\ell}\dfrac{\displaystyle\sum_a\left[(整式)\times\prod{\cdot\choose\cdot}\right]}{\displaystyle\prod_{0\le i\le k}(n-i)^{\ell_i}}
$$

那么死鸡数期望归一化后（理解为某鸡砍腿之后死的概率？）就是

$$
\overline {Z_{n,m}}=\dfrac{\sum_{k\ge 0}Z_{n,m}(k)\cdot k}{\sum_{k\ge 0}Z_{n,m}(k)}
$$

由于原题目 $n=m=10^6$ 很大，看看渐进倒也不影响结果。

接下来就开始解析了。由于关注的是渐进，我们可以预料到它之和极点有关；因此可以忽略上面的整式，只关注分母和二项式系数。

先处理分母。构造一组生成函数

$$
\begin{aligned}
f_r(z)=\sum_{j\ge 0}(\dfrac z{n-r})^j=\sum_{j\ge 0}(n-r)^{-j}z^j=\dfrac 1{1-\dfrac{z}{n-r}}
\end{aligned}
$$

收敛区间分别为 $|z| < |n-r|$. 那么原来分母中各项就可以表示为 $[z^{\ell_r}]f_r(z)$.

**注意到** 诸 $\ell$ 之和是定值 $m$，很自然地想到将这一组函数乘起来，然后直接比较 $z^m$ 项系数。那么

$$
\begin{aligned}
F(z)=&\prod_{r=0}^k\dfrac 1{1-\dfrac{z}{n-r}}=\prod_r f_r\\
=&\prod_r\sum_j(n-r)^{-j}z^j\\
=&\sum_j\left(\sum_{w_0+w_1+\cdots+w_k=j}\prod_r(n-r)^{w_r}\right)z^j
\end{aligned}
$$

不难看出分母就是 $[z^m]F(z)$.

利用 $z=0$ 处的 Taylor 结合 Cauchy 积分公式就有

$$
\begin{aligned}
[z^m]F(z)&=\dfrac{F^{(m)}(z)}{m!}=\dfrac 1{2i\pi}\oint_\gamma\dfrac{F(\xi)}{\xi^{m+1}}\text d\xi\\
&=\dfrac1{2\pi i}\oint_\gamma\dfrac {\text d\xi}{\xi^{m+1}}\prod_{r=0}^k\dfrac1{1-\dfrac \xi{n-r}}
\end{aligned}
$$

其中积分路径 $\gamma: |z|<\rho$，并任取半径 $\rho<|n-k|$ 即可。

分母做完了做二项式系数。首先是一般的二项式系数的积分表示（Egorychev 法？）

$$
{n\choose r}=\dfrac 1{2\pi i}\oint_{\gamma'}\dfrac{(1+z)^n}{z^{r+1}}\text dz
$$

被积式的奇点为 $0$ 和 $-1$，因此取 $\gamma':|z|<\eta<1$ 即可。那么

$$
\begin{aligned}
&\sum_{\substack{ a0+\cdots+a_k=k\\ a_i\ge 0\\ a_0+\cdots+a_r\ge r } }\left[\left(\prod_{i=0}^k{
\ell_i-1\choose a_i}\right)\left(\prod_{i=1}^k(a_0+\cdots+a_{i-1}-(i-1))\right)\right]\\
=&\sum_{\mathcal A}W(\mathcal A)\cdot\dfrac 1{(2\pi i)^{k+1}}\oint\cdots\oint\dfrac{\prod (1+z_i)^{\ell-1}}{\prod z_i^{a_i+1}}\text d{\bf z}
\end{aligned}
$$

代入！

$$
\begin{aligned}
&Z_{n,m}(k)\\
=&\sum_{\sum \ell=m}\dfrac{\dfrac{n!}{(n-m+k)!}\times\displaystyle\sum_{\substack{ a0+\cdots+a_k=k\\ a_i\ge 0\\ a_0+\cdots+a_r\ge r } }\left[\left(\prod_{i=0}^k{
\ell_i-1\choose a_i}\right)\left(\prod_{i=1}^k(a_0+\cdots+a_{i-1}-(i-1))\right)\right]}{\displaystyle\prod_{i=0}^k (n-i)^{\ell_i}}\\
=&\dfrac{n!}{(n-m+k)!}\times\dfrac1{2\pi i}\oint_\gamma\dfrac {\text d\xi}{\xi^{m+1}}\prod_{r=0}^k\dfrac1{1-\frac \xi{n-r}}\\
&\times\sum_\ell\left(\sum_{\mathcal A}W(\mathcal A)\cdot\dfrac 1{(2\pi i)^{k+1}}\oint\cdots\oint\dfrac{\prod (1+z_i)^{\ell-1}}{\prod z_i^{a_i+1}}\text d{\bf z}\right)\\
=&\dfrac{n!}{(n-m+k)!}\cdot\dfrac1{(2\pi i)^{k+2}}\cdot\oint\cdots\oint\dfrac1{t^{m+1}}\left(\sum_{\mathcal A}\dfrac{W(\mathcal A)}{\prod_{r=0}^k z_r^{a_r+1}}\right)\left(\prod_{r=0}^k\left(1-\dfrac{1+z_r}{n-r}t\right)^{-1}\right)\text d{\bf z}\text dt\\
=&\dfrac{(2\pi i)^{-k-2}\cdot n!}{(n-m+k)!}\cdot\boxed{\oint\cdots\oint\dfrac{\mathcal R_{n,m,k}(z)}{t^{m+1}}\left(\prod_{r=0}^k\left(1-\dfrac{t}{n-r}\right)^{-1}\right)\text d{\bf z}\text dt}
\end{aligned}
$$

我们只关心 $t$ 上的奇点 $\to$ 渐进行为。再说了

## 推广

不妨推广一下原来的死鸡问题：

> 设有 $n$ 个人，每个人初始都有 $k$ 滴血。共有 $m$ 次操作，每次操作从血量活人（大于 $0$ 的人）中等概率地随机抽取一人，扣一滴血。不难得知每个人最终的血量是独立同分布的。求这个分布 $\mathbb P\{\text{HP}=k\}$.

GPT 说某人某次被抽到的概率永远是 $\dfrac 1n$. 这显然是错误的（好吧，是 $k\ge m$ 才成立）。它把这个问题当作自助抽样了。

推广留给以后吧。
