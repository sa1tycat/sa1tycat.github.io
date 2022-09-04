---
title: 时间复杂度
author: saltycat
date: 2022-08-31 19:26:17 +0800
categories: [算法]
tags: [时间复杂度]
math: true
comments: true
mermaid: true
typora-root-url: ../../sa1tycat.github.ioyu
---



## 记号

### 大 O 记号

用于描述一个函数的渐进上界。

具体地，若存在正的常数 $c$ 和函数 $f(n)$，使得对任何 $n \gg 2$ 都有 $T(n) \leq c\cdot f(n)$ 则可认为在 $n$ 足够大之后，$f(n)$ 给出了 $T(n)$ 增长速度的一个渐进上界。此时，记之为： $T(n) = \mathcal{O}(f(n))$。

其性质：

- 对于任一常数 $c > 0$，有 $\mathcal{O}(f(n)) = \mathcal{O}(c\cdot f(n))$
- 对于任意常数 $a > b > 0$，有 $\mathcal{O}(n^a + n^b) = \mathcal{O}(n^a)$

从而可知大 O 记号将正的常数认定为 1，并且忽略除最高次项的其他项。

由于大 O 记号用于刻画的是一个函数的渐进上界，对于时间复杂度为 $T(n)=4n^3+2n$ 的函数，也可以被记作 $\mathcal{O}(n^5)$。但通常就直接取次数最高项。



### 大 Ω 记号

若存在正的常数 $c$ 和函数 $f(n)$，使得对任何 $n \gg 2$ 都有 $T(n) \geq c\cdot f(n)$ 则可认为在 $n$ 足够大之后，$f(n)$ 给出了 $T(n)$ 增长速度的一个渐进下界。此时，记之为： $T(n) = \textup{Ω}(f(n))$。

不同于大 O 记号，大 Ω 记号用于刻画一个算法运行的最好情况，即算法运行时间都不低于 $\textup{Ω}(f(n))$。



### 大 Θ 记号

$T(n)$ 介于 $\Omega(g(n))$与 $\mathcal{O}(f(n))$ 之间。若恰巧出现 $g(n) = f(n)$ 的情况，则可以使用另一记号来表示。

如果存在正的常数 $c_1 < c_2$ 和函数 $h(n)$，使得对于任何 $n \gg 2$ 都有 $c_1\cdot h(n) \leq T(n) \leq c_2\cdot h(n)$ 就可以认为在 $n$ 足够大之后，$h(n)$ 给出了 $T(n)$ 的一个确界。此时，我们记之为： $T(n) = \Theta(h(n))$。

大 Θ 记号是对算法复杂度的准确估计，对于规模为 $n$ 的任何输入，算法的运行时间 $T(n)$ 都与 $\Theta(h(n))$ 同阶。

![大O记号、大Ω记号和大Θ记号](/assets/blog_res/2022-08-31-time-complexity.assets/image-20220831203614023.png)

由于大 Θ 记号的条件较强，故在某些情况下，可能无法找到这个紧贴的 $h(n)$，因此多数情况下我们都直接使用 $\mathcal{O}$ 来描述一个算法的时间复杂度。



## 常见时间复杂度

### 常数 $\mathcal{O}(1)$

一般地，仅含一次或常数次基本操作的算法均属此类。此类算法通常不含循环、分支、子程序调用等，但也不能仅凭语法结构的表面形式一概而论。

### 线性 $\mathcal{O}(n)$

如：

```c++
long long power(int a, int n) {
  long long ret = 1; // O(1)
  for (int i = 0; i < n; i++) { // O(n)
    ret *= a; // O(1)
  }
  return ret; // O(1)
}
```

的时间复杂度为： $\mathcal{O}(1)+\mathcal{O}(1)\cdot \mathcal{O}(n)+\mathcal{O}(1)=\mathcal{O}(n+2)=\mathcal{O}(n)$。

对于输入的每一单元，此类算法平均消耗常数时间。就大多数问题而言，在对输入的每一单元均至少访问一次之前，不可能得出解答。以数组求和为例，在尚未得知每一元素的具体数值之前，绝不可能确定其总和。故就此意义而言，此类算法的效率亦足以令人满意。

### 对数 $\mathcal{O}(\log n)$

如：

```c++
long long power(int a, int n) {
  long long tmp = 1;
  while (n > 0) { // O(log n)
    if (n & 1) { // O(1)
      tmp *= a; // O(1)
    }
    a = a * a; // O(1)
    n >>= 1; // O(1)
  }
  return tmp; // O(1)
}
```

的复杂度为 $\mathcal{O}(\log n)$。

注意：根据换底公式和大 O 记号的性质，有 $\mathcal{O}(\log_a n)=\mathcal{O}(\frac{1}{\log a}\cdot \log n)=\mathcal{O}(\log n)$，故一般省略对数底数简写成 $\mathcal{O}(\log n)$。

### 多项式 $\mathcal{O}(n^k)$

若运行时间可以表示和度量为 $T(n) = \mathcal{O}(f(n))$ 的形式，而且 $f(x)$ 为多项式，则对应的算法称作“多项式时间复杂度算法”（polynomial-time algorithm）。当然，线性时间复杂度算法，也属于多项式时间复杂度算法的特例，其中线性多项式 $f(n) = n$ 的次数为1。

在算法复杂度理论中，多项式时间复杂度被视作一个具有特殊意义的复杂度级别。多项式级的运行时间成本，在实际应用中一般被认为是可接受的或可忍受的。某问题若存在一个复杂度在此范围以内的算法，则称该问题是可有效求解的或易解的（tractable）。 

请注意，这里仅要求多项式的次数为一个正的常数，而并未对其最大取值范围设置任何具体上限，故实际上该复杂度级别涵盖了很大的一类算法。比如，从理论上讲，复杂度分别为 $\mathcal{O}(n^2)$ 和 $\mathcal{O}(n^{1000})$ 算法都同属此类，尽管二者实际的计算效率有天壤之别。之所以如此，是因为相对于以下的指数级复杂度，二者之间不超过多项式规模的差异只是小巫见大巫。

### 指数 $\mathcal{O}(2^n)$

如：

```c++
long long fib(int n) {
  return n <= 1 ? 1 : fib(n - 1) + fib(n - 2);
}
```

$T(n) = \mathcal{O}(\Phi^{n+1}) = \mathcal{O}(2^n)$

由于指数增量过大，很难在实际中有效解决问题，因此指数级的“算法”也被称作难解（intractable）问题。

![常见函数增长模型](/assets/blog_res/2022-08-31-time-complexity.assets/image-20220901160341309.png)



## 时间复杂度分析

### 常见的求和公式

- 自然等幂级数

$$
\sum^{n}_{i=1} i = \frac{n(n+1)}{2}=\mathcal{O}(n^{2})
$$

$$
\sum _{i=1}^{n}i^{{2}}={\frac  {1}{3}}n^{3}+{\frac  {1}{2}}n^{2}+{\frac  {1}{6}}n=\mathcal{O}(n^{3})
$$

$$
\sum _{i=1}^{n}i^{{3}}={\frac  {1}{4}}n^{4}+{\frac  {1}{2}}n^{3}+{\frac  {1}{4}}n^{2}=\mathcal{O}(n^{4})
$$

$$
\sum _{i=1}^{n}i^{{4}}={\frac  {1}{5}}n^{5}+{\frac  {1}{2}}n^{4}+{\frac  {1}{3}}n^{3}-{\frac  {1}{30}}n=\mathcal{O}(n^{5})
$$

$$
\sum _{i=0}^{n}i^{{k}}\approx \int_{0}^{n} x^k\, dx=\mathcal{O}(n^{k+1})
$$

- 几何级数

$$
\sum _{i=0}^{n}a^{{k}}=\frac{a^{n+1}-1}{a-1} =\mathcal{O}(a^{n})\;(a>1)
$$

- 收敛级数

$$
\sum _{i=2}^{n}\frac{1}{i\cdot(i-1)}=1-\frac{1}{n} =\mathcal{O}(1)
$$

$$
\sum _{i=1}^{n}\frac{1}{i^2}=\frac{\pi^2}{6} =\mathcal{O}(1)
$$

$$
\sum _{i\textup{ is a perfect power}}\frac{1}{i-1}=\frac{1}{3}+\frac{1}{7}+\frac{1}{8}\dots=1=\mathcal{O}(1)
$$

$$
\textup{几何分布: }(1-\lambda)\cdot(1+2\lambda+3\lambda^2+4\lambda^3+\dots)=\frac{1}{1-\lambda}=\mathcal{O}(1)\;(0<\lambda<1)
$$



- 调和级数

$$
\sum_{k=1}^n\,\frac{1}{k} \;=\; \ln n + \gamma + \mathcal{O}(\frac{1}{2n})=\Theta(\log n)
$$

- 对数级数

$$
\sum_{k=1}^n\,\ln k \;=\Theta(n\log n)
$$

- 线性对数
  $$
  \sum_{k=1}^n\,k\cdot \log k \;\approx\int_1^nx\ln x\,dx=\mathcal{O}(n^2\log n)
  $$

- 线性指数

$$
\sum_{k=1}^n\,k\cdot 2^ k =\mathcal{O}(n\cdot2^n)
$$



### 常用分析方法

#### 非递归算法

基本步骤：

1) 决定用哪个（或哪些）参数作为算法问题规模的度量 
2) 找出算法中的基本语句（作为一个规律，它总是位于算法最内层循环中）
3) 检查基本语句的执行次数是否只依赖于问题规模
4) 建立基本语句执行次数的求和表达式 
5) 用渐进符号表示这个求和表达式

（1）for / while 循环 循环体内计算时间*循环次数； 

（2）嵌套循环 循环体内计算时间*所有循环次数； 

（3）顺序语句 各语句计算时间相加； 

（4）if-else语句 if语句计算时间和else语句计算时间的较大者。

#### 递归算法

根据递归方式，列出关于时间 $T(n)$ 的递推方程，并解之。

解时间递推方程的方法有：1. 展开法（归纳法）；2. 递归树法（recursion trace）；3. Master 定理

##### 展开法

例如，递推式
$$
T(n)=\left\{\begin{matrix} 1 & n=1\\ 2\,T(n/2)+4n^2 & n>1 \end{matrix}\right.
$$
的时间复杂度为


$$
\begin{aligned}
T(n) & = 2\,T(n/2)+5n^2 \\
& =2\,(2\,T(n/4)+5(n/2)^2)+5n^2 \\
& =2\,(2\,(2\,T(n/8)+5(n/4)^2)+5(n/2)^2)+5n^2 \\
& \dots \\
& = 2^k\cdot T(1)+2^{k-1}\cdot 5 \cdot(\frac{n}{2^{k-1}})^2+\dots+5n^2 \\
& = \mathcal{O}(n^2)
\end{aligned}
$$


##### 递归树法

对于程序：

```c++
int sum(int a[], int n) {
  return n == 0 ? 0 : sum(a, n - 1) + a[n - 1];
}
```

可以画出递归图：

![递归图1](/assets/blog_res/2022-08-31-time-complexity.assets/image-20220904095915919.png)

每一个递归实例的时间复杂度为 $\mathcal{O}(1)$，故算法的整体时间复杂度为 $\mathcal{O}(1)\times\mathcal{O}(n+1)=\mathcal{O}(n)$

又如：

```c++
int sum(int A[], int lo, int hi) { 
  if (lo == hi) return A[lo];
  int mi = (lo + hi) >> 1;
  return sum(A, lo, mi) + sum(A, mi+1, hi);
} // 入口形式为sum(A, 0, n-1)
```

画出递归图：



![递归图2](/assets/blog_res/2022-08-31-time-complexity.assets/image-20220904100450877.png)

每一个递归实例的复杂度均为 $\mathcal{O}(1)$，只需统计出递归实例的个数即可，可以看到第一层有 $2^0$ 个，第二层有 $2^1$个，...，一共有 $\lceil \log n \rceil$ 层，即最后一层的个数为 $2^{\log n}$ 个，故最后总数为 $2^0+2^1+\dots+2^{\log n}$，也就是说：


$$
\begin{align} 
T(n) & = \mathcal{O}(1) \times \mathcal{O}(2^0+2^1+\dots+2^{\log n})\\
& = \mathcal{O}(2^{\log n}) \\
& = \mathcal{O}(n)
\end{align}
$$

##### Master Theorem

对于形如：
$$
T(n)=a\cdot T(n/b)+\mathcal{O}(f(n))
$$
其中 $a\cdot T(n/b)$ 对应着 $\mathcal{O}(n^{\log_ba})$，

- 若 $f(n)=\mathcal{O}(n^{\log_b a\textcolor[rgb]{1,0,0}{-\epsilon}})$，即 $\mathcal{O}(f(n))<\mathcal{O}(n^{\log_b a-\epsilon})$，则 $T(n)=\Theta(n^{\log_b a})$
- 若 $f(n)=\Omega (n^{\log_b a\textcolor[rgb]{1,0,0}{+\epsilon}})$，即 $\mathcal{O}(f(n))>\mathcal{O}(n^{\log_b a-\epsilon})$，则 $T(n)=\Theta(f(n))$
- 若 $f(n)=\Theta (n^{\log_b a}\cdot\log^k n)$，即 $\mathcal{O}(f(n))\sim \mathcal{O}(n^{\log_b a})$，则 $T(n)=\Theta(n^{\log_b a}\cdot\log^{k+1} n)$



如，求 $T(n)=\left\{\begin{matrix} 1 & n=1\\ 2\,T(n/2)+4n & n>1 \end{matrix}\right.$ 的时间复杂度。

可知 $a=b=2$，

即 $\log_ba=\log_22=1,\;n^{\log_ba}=n,\;f(n)=4n$，

故 $\mathcal{O}(f(n))\sim \mathcal{O}(n^{\log_b a})$，其中 $k=0$，

故 $T(n)=\Theta(n\log n)$。



又如，求 $T(n)=\left\{\begin{matrix} 1 & n=1\\ 2\,T(n/2)+n\log n & n>1 \end{matrix}\right.$ 的时间复杂度。

可知 $a=b=2$，

即 $\log_ba=\log_22=1,\;n^{\log_ba}=n,\;f(n)=n\log n$，

也就是说 $f(n)=\Theta(n\cdot \log n)$，即 $k=1$，

故 $\mathcal{O}(f(n))\sim \mathcal{O}(n^{\log_b a})$，其中 $k=1$，

故 $T(n)=\Theta(n\log^2 n)$。

### 实例

#### 实例一

```c++
for (int i = 0; i < n; i++)
  for (int j = 0; j < i * i; j++)
    for (int k = 0; k < j; k++) 
      sum++;

```

可以看到是很平凡的循环，基本操作为 `sum++;`，显然有：


$$
\begin{aligned}
T(n) & = \sum_{i\,=\,0}^{n\,-\,1}\,\sum_{j\,=\,0}^{i^2}\,\sum_{k\,=\,0}^{j\,-\,1}\,1 \\\\
& = \sum_{i\,=\,0}^{n\,-\,1}\,\sum_{j\,=\,0}^{i^2}\,j\\\\
& = \sum_{i\,=\,0}^{n\,-\,1}\,(0+1+2+\dots+i^2)\\\\
& = \sum_{i\,=\,0}^{n\,-\,1}\,\frac{i^2\cdot(i^2+1)}{2}\\\\
& = \mathcal{O}(n^5)
\end{aligned}
$$

#### 实例二

```c++
for (int i = 0; i < n; i++) 
  for (int j = 0; j < i * i; j++) 
    if (j % i == 0)
      for (int k = 0; k < j; k++) 
        sum++;
```

只有在 `j` 能够被 `i` 整除的时候才会进行基本操作 `sum++`，上述循环等价于：

```c++
for (int i = 0; i < n; i++) 
  for (int r = 0; r < i; j++) 
    for (int k = 0; k < r * i; k++) 
      sum++;
```




$$
\begin{aligned}
T(n) & = \sum_{i\,=\,0}^{n\,-\,1}\,\sum_{r\,=\,0}^{i\,-\,1}\,\sum_{k\,=\,0}^{r\cdot i\,-\,1}\,1 \\\\
& = \sum_{i\,=\,0}^{n\,-\,1}\,\sum_{r\,=\,0}^{i\,-\,1}\,r\cdot i   \\\\
& = \sum_{i\,=\,0}^{n\,-\,1}\, i\cdot \frac{i\cdot(i-1)}{2}   \\\\
& = \mathcal{O}(n^4)
\end{aligned}
$$

