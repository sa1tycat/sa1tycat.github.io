---
title: 快速幂
author: saltycat
date: 2022-08-21 20:05:17 +0800
categories: [算法]
tags: [快速幂,矩阵快速幂]
math: true
comments: true
mermaid: true
typora-root-url: ../../sa1tycat.github.ioyu
---





## 引言

在计算指数如 $7^5$ 时，最朴素的做法就是逐个累乘，即：

```c++
long long ans = 1;
for (int i = 1; i <= 5; i++) {
    ans *= 7;
}
```

显然算法的时间复杂度为 $\mathcal{O}(n)$，效率虽然也是可以接受的，但显然其中有很多部分是属于重复操作的。

例如我们计算出了 $7^2$，显然这个结果还可以继续使用，即$7^4=(7^2)^2$，从而计算 $7^5=(7^2)^2\times7$。

再举一个指数稍微大一点的例子，如 $7^8$，要计算 $7^8$，我们可以先计算出 $7^4$ 然后再平方；要计算 $7^4$，我们可以先计算 $7^2$，然后再平方；...，如此往复直到幂指数为 0，直接返回结果为 1。

## 递归版

从而可以总结出如下递归式：

 $$ \textup{power}(a,n)=\left\{\begin{matrix}
1 & (n=0)\\ 
\textup{power}(a, \lfloor n/2 \rfloor)^2 & (n>0 \textup{ and } n \textup{ is even}) \\ 
\textup{power}(a, \lfloor n/2 \rfloor)^2 \times a & (n>0 \textup{ and } n \textup{ is odd})
\end{matrix}\right. $$

上述过程实际就是在对指数进行二分，因此可知该算法的时间复杂度为 $\mathcal{O}(\log n)$ 

```c++
long long power(int a, int n) {
  if (n == 0) { // 递归基
    return 1;
  }
  // n >> 1，相当于 n / 2
  long long tmp = power(a, n >> 1);
  // 二进制最后一位为1(即+1*2^0)，与1按位与得1
  return (n & 1) ? tmp * tmp * a : tmp * tmp;
}
```



## 非递归版

我们将取幂的任务按照指数的 **二进制表示** 来分割成更小的任务。

首先我们将 $n$ 表示为 2 进制，举一个例子：

$$
3^{13} = 3^{(1101)_2} = 3^8 \cdot 3^4 \cdot 3^1
$$

因为 $n$ 有 $\lfloor \log_2 n \rfloor + 1$ 个二进制位，因此当我们知道了 $a^1, a^2, a^4, a^8, \dots, a^{2^{\lfloor \log_2 n \rfloor}}$ 后，我们只用计算 $\Theta(\log n)$ 次乘法就可以计算出 $a^n$。

于是我们只需要知道一个快速的方法来计算上述 3 的 $2^k$ 次幂的序列。这个问题很简单，因为序列中（除第一个）任意一个元素就是其前一个元素的平方。举一个例子：

$$
\begin{align}
3^1 &= 3 \\
3^2 &= \left(3^1\right)^2 = 3^2 = 9 \\
3^4 &= \left(3^2\right)^2 = 9^2 = 81 \\
3^8 &= \left(3^4\right)^2 = 81^2 = 6561
\end{align}
$$

因此为了计算 $3^{13}$，我们只需要将对应二进制位为 1 的整系数幂乘起来就行了：

$$
3^{13} = 6561 \cdot 81 \cdot 3 = 1594323
$$

将上述过程说得形式化一些，如果把 $n$ 写作二进制为 $(n_tn_{t-1}\cdots n_1n_0)_2$，那么有：

$$
n = n_t2^t + n_{t-1}2^{t-1} + n_{t-2}2^{t-2} + \cdots + n_12^1 + n_02^0
$$

其中 $n_i\in\{0,1\}$。那么就有

$$
\begin{aligned}
a^n & = (a^{n_t 2^t + \cdots + n_0 2^0})\\\\
& = a^{n_0 2^0} \times a^{n_1 2^1}\times \cdots \times a^{n_t2^t}
\end{aligned}
$$

根据上式我们发现，原问题被我们转化成了形式相同的子问题的乘积，并且我们可以在常数时间内从 $2^i$ 项推出 $2^{i+1}$ 项。

这个算法的复杂度是 $\Theta(\log n)$ 的，我们计算了 $\Theta(\log n)$ 个 $2^k$ 次幂的数，然后花费 $\Theta(\log n)$ 的时间选择二进制为 1 对应的幂来相乘。

考虑到递归的开销较大，可以转化为以下非递归方式：

简而言之，就是将指数 $n$，转化成二进制形式 $(n_tn_{t-1}\cdots n_1n_0)_2$，从右到左依次编号为 0，1，...，t。对于其中任意第 k 位，若 $n_k=1$，则需要相乘 $a^{2^k}$。

即抽离出公式：


$$
\begin{aligned}
a^n & = (a^{n_t 2^t + \cdots + n_0 2^0})\\\\
& = a^{n_0 2^0} \times a^{n_1 2^1}\times \cdots \times a^{n_t2^t}
\end{aligned}
$$


中 $n_k=1$ 的部分。

```c++
long long power(int a, int n) {
  long long tmp = 1;
  while (n > 0) { // 一共 log n 次
    if (n & 1) { // 当前位为 1
      tmp *= a;
    }
    a = a * a; // 依次为 a^1, a^2, a^4, ..., a^(n/2)
    n >>= 1; // n 右移
  }
  return tmp;
}
```

