---
title: 删除字符串的子串
author: saltycat
date: 2022-05-31 15:02:27 +0800
categories: []
tags: [字符串]
math: true
comments: true
mermaid: true
typora-root-url: ../../sa1tycat.github.io

---



## Description

输入2个字符串S1和S2，要求删除字符串S1中出现的所有子串S2，即结果字符串中不能包含S2。

## Input

输入在2行中分别给出不超过80个字符长度的、以回车结束的2个非空字符串，对应S1和S2。

## Output

在一行中输出删除字符串S1中出现的所有子串S2后的结果字符串。

### 输入样例：


## Sample Input 

```
Tomcat is a male ccatat
cat
```

## Sample Output 

```
Tom is a male
```

## 分析

只需要一直搜索是否存在子串，直到不存在为止，存在的情况下，将s1中子串后的字符串拷贝到s1中子串出现的位置即可。

注意使用strcat()进行拼接的时候要避免内存空间出现重叠以免出错。

因此可以把后半段字符串拷贝到一个tmp数组中，再把tmp拼接在s1子串出现位置上。

## Code

```c
#include<stdio.h>
#include<string.h>
char s1[1001];
char s2[1001];


int main(){
    gets(s1);
    gets(s2);A
    char *p; // 用于接收strstr()传递的地址
    while((p=strstr(s1,s2))!=NULL){ // 即存在子串
        /*
            由于strcat()不能将内存区域有重叠的字符串拼接
            故需要将s1末尾的字符串拷贝到一个tmp数组中
        */
       char tmp[1001];
       strcpy(tmp,p+strlen(s2));
       // 在拼接s1和tmp前，需要改变s1字符串结束位置，即
       *p='\0';
       strcat(s1,tmp);
    }
    puts(s1);
    return 0;
}
```

