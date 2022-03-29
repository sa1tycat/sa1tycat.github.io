---
title: 质数
author: saltycat
date: 2022-03-24 23:04:46 +0800
categories: [数学]
tags: [质数]
math: true
comments: true
mermaid: true
typora-root-url: ../../sa1tycat.github.io

---

# 质数判断

试除法：用所有可能因数去除该数，若余数为 $0$，则为合数，否则为质数。

最朴素的做法就是穷举 $[2, n)$ 之间的所有数来除该数。

实际上，只需穷举至 $ \sqrt{n} $ 即可。假设合数 $k$ 有且仅有因数 $f \in ( \sqrt{n},n)$，显然 $k$ 还有一个因数 $g = \frac{k}{f}$，由于 $f > \sqrt{n}$，则 $g < \sqrt{n}$，故假设不成立。因此合数必然有小于等于 $\sqrt{n}$ 的因数。

算法时间复杂度为：$\mathcal{O}(\sqrt{n})$。

代码如下：

```c++
bool isPrime(int x){ // 试除法
	if(x<2) return 0; // 0,1均不是质数也不是合数 
	for(int i=2;i<=sqrt(x);i++){
		if(x%i==0) return 0;
	}
	return 1;
}
```

# 质数筛选

## Eratosthenes 筛选

质数的倍数一定是合数，因此可以通过穷举质数的倍数，从而达到筛选出合数，得到质数的效果。

最经典朴素的筛法就是 Eratosthenes 筛法（埃氏筛法）

注意，对于一个质数 $i$，由于对于倍数 $n<i$，数 $ni$ 已经在得到质数 $i$ 之前被筛选过了，因此只需要从倍数 $n=i$ 开始枚举即可。

```c++
bool prime[MAXN];
void primes(int n){ // Eratosthenes 筛法 
	// 记得初始化 prime[] 为 1 
    memset(prime,1,sizeof(prime))
	for(int i=2;i<=n;++i){ // 从 2 开始筛选到 n 
		if(!prime[i]) // 数跳过 
			continue;
		for(int j=i;j<=n/i;j++) // 从 x^2 开始筛，x*i（在 i 小于 x 时）已经筛选过 
			prime[i*j]=0; 
	}
	for(int i=2;i<=n;i++){
		if(prime[i])
			printf("in primes(): %d\n",i);
	}
}
```


## 线性筛选

Eratosthenes 筛选即使是从 $i^2$ 开始筛选，仍然会重复筛选，例如 $12 = 2 \times 6 = 3 \times 4$，实际上是有重复的。

对于任意合数 $c$，其都可以表示为 $c=p_1^{k_1} p_2^{k_2} ...p_n^{k_n}$（$p_1,p_2,...,p_n \in \mathbb{P}$，$k_1,k_2,...,k_n \in \mathbb{N^+}$），故只需要将质数累乘起来，即可筛选出合数。例如 $12=2^2 \times 3$。

对于 $i \in [2,n]$，用一个数组 `prime_factor[i]` 来储存 $i$ 的最小质因数。初始化 `prime_factor[i]=0`，若穷举到 $i$ 的对应 `prime_factor[i]=0`，则说明他是质数，令 `prime_factor[i]=i`，并记录下该质数。为了维护 `prime_factor[]`，每一轮循环都要穷举已知的质数 `prime[j]`，用 `prime[j]` 来替换 `prime_factor[i*prime[j]]`，因为 `prime[j] < i` 因此维护了 `prime_factor[]` 的统一，因此需要保证 `prime[j]` 是最小的，故 `prime[j] > prime_factor[i]` 时需要退出循环

```c++
void linearPrimes(int n){ // 线性筛法 
	int primenum=0; 
	// prime_factor 用于储存 i 的最小质因子
	// 储存了最小质因子，就是合数，为 0 说明没有被筛掉，是质数 
	memset(prime_factor,0,sizeof(prime_factor));
	for(int i=2;i<=n;i++){
		if(prime_factor[i]==0){ // i 没有被筛掉，所以是合数 
			prime_factor[i]=i;
			nprime[++primenum]=i;
		}
		for(int j=1;j<=primenum;j++){
			if(nprime[j]*i>n||nprime[j]>prime_factor[i]){
				// 所筛的数字大于最大数字或者数字 i 的最小质因子更小
				break; 
			}
			prime_factor[i*nprime[j]]=nprime[j];
		}
	}
	for(int i=1;i<=primenum;i++){
		printf("in linearPrimes(): %d\n",nprime[i]);
	}
}  
```