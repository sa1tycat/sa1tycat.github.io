---
title: 文档处理
author: saltycat
date: 2022-03-23 17:03:18 +0800
categories: [题解,PAT]
tags: [字符串]
math: true
comments: true
mermaid: true
typora-root-url: ../../sa1tycat.github.io

---

# [题面](https://code.sipcoj.com/problem/130)

## Description

明理农场收到了一份文档，这份文档很长，Mr. Ox 看不懂，他需要你来处理一下这份文档。

这是一篇长度为 $n$，由大写字母`'A'-'Z'`、小写字母`'a'-'z'`和空格组成的文档，他需要你统计出每一段连续字母的个数，并将所有空格符`' '`替换为`'-'`。比如`Good luck`，处理之后为`1G2o1d-1l1u1c1k`。

在处理完文档之后，你需要将处理的结果进行输出。

数据范围：$1 \leq n \leq 10^5$
 

## Input

一行一个字符串表示文档的内容。


## Output

一行一个字符串表示处理后的内容。


## Sample Input 1 

```
Wooooo AAAAAKKKKK leEe
```

## Sample Output 1

```
1W5o-5A5K-1l1e1E1e
```

# 分析

朴素的做法：模拟数数过程，从 `str[0]` 开始计数，初始化 `count = 1`。如果遇到空格，直接输出 `-` 然后跳过。如果遇到当前字符与下一个字符不相同，说明我们需要输出当前计数的字符了并且重置计数器 `count = 1`，否则 `count ++`。

检查的时候可以测试一下特殊数据：

| Input     | Output    |
|:---       |:---        |
| ` `（空格）|  `-`      |
| ` a`      |  `-1a`    |
| ` aa`     |  `-2a`    |
| ` aab `   |  `-2a1b-` |
| `aa bb`   |  `2a-2b`  |

# 代码

```c++
#include<cstdio>
#include<cstdlib>
#include<cstring>
#define MAXN 100005
using namespace std;
char str[MAXN];
 
int main() {
	gets(str);
	int count = 1;
	for(int i = 0; i < strlen(str); i++) {
		if(str[i] == ' ') {
			printf("-");
			continue;
		}
		if(str[i] != str[i + 1]) {
			printf("%d%c", count, str[i]);
			count = 1;
		}
		else {
			count++;
		}
	}
	return 0;
}
```
