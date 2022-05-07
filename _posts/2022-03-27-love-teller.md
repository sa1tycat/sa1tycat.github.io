---
title: 【题解】情话鉴定器
author: saltycat
date: 2022-03-27 22:46:02 +0800
categories: [题解,PAT]
tags: [字符串]
math: true
comments: true
mermaid: true
typora-root-url: ../../sa1tycat.github.io

---

# [H]情话鉴定器

## Description

Mr. High准备来到明理农场给Mr. Ox打工，赚点外快。

明理农场的猪之间会相互通讯，而Mr. Ox想知道他们之间说话的内容，于是他让Mr. High开发一个情话鉴定器。

情话鉴定器的功能很简单，如果通讯的内容中有 `'I'、'love'、'you'`（区分大小写）三个子串，并且它们按顺序出现，则这一段内容为情话，否则不是。

Mr. Ox现在截取了若干段通讯记录，他希望你分辨出每一段记录是否为情话。

子串：串中任意个连续的字符组成的子序列称为该串的子串。

数据范围：假设字符串总长度为 $len$，则 $1 \leq len \leq 10^6$，字符串由大写字母、小写字母、数字、空格组成


## Input

若干行每行代表一段通讯记录，当检测到输入单独一行为 `Shu wan le` 的时候，代表输入结束，之后的内容作废。

输入保证至少存在一行 `Shu wan le`。


## Output

在非作废内容中，对于 `Shu wan le` 之前的每一行，若这一行的通讯内容为情话，则输出 `Yes`，否则输出 `No`。


## Sample Input 1 

```
Iaaaaaloveaaaaayou
iaaaaaloveaaaaayou
I love you
I1love2you23333333
love I you
Iloveyou
Shu wan le
aaaaaaaa
I love you
```

## Sample Output 1

```
Yes
No
Yes
Yes
No
Yes
```

# 分析

题目要求很简单，就是要在读取终止前，判断每一行字符串，是否以此包含字串 `I`、`love`、`you`。

借助 cstring 库函数 `strstr()`，判断非常容易，该函数的用法如下：

```c++
const char *strstr(const char *str, const char *substr);
```

该函数用返回 substr，在 str 中第一次出现的地址，如果没有找到，则返回 NULL。

借助与这个函数我们即可轻松地找到字串了。

如果找到 `I`，将它的地址记作 `indexI`，那么只需查找 `strstr(indexI, love)` 即可。

# 代码

```c++
#include<cstdio>
#include<cstring>
#define MAXN 1000001
const char END[]="Shu wan le";
const char I[]="I";
const char love[]="love";
const char you[]="you";
char str[MAXN];

int main(){
	gets(str);
	int start=1;
	while(strcmp(str,END)!=0){
		if(!start) printf("\n");
		else start=0;
		char* indexI=strstr(str,I);
		if(indexI==NULL){
			printf("No");
			gets(str);
			continue;
		}
		char* indexLove=strstr(indexI,love);
		if(indexLove==NULL){
			printf("No");
			gets(str);
			continue;
		}
		char* indexYou=strstr(indexLove,you);
		if(indexYou==NULL){
			printf("No");
			gets(str);
			continue;
		}
		printf("Yes");
		gets(str);
	}
	return 0;
}
```