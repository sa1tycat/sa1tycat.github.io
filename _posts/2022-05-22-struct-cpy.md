---
title: 结构体的拷贝
author: saltycat
date: 2022-05-22 14:28:09 +0800
categories: []
tags: []
math: true
comments: true
mermaid: true
typora-root-url: ../../sa1tycat.github.io

---



## 引言

在转专业考试的时候遇到一道，需要用到结构体排序，也就是说需要交换两个结构体。

最开始的时候我想到的方法是利用指针：

```c++
struct node{
    int id,in;
    char name[20];
    double ave;
}stu[55];

void swap(node* a,node* b){
   	node t;
    t=*a;
    *a=*b;
    *b=t;
}

int main(){
    node a,b;
    swap(&a,&b);
    return 0;
}

```

但是我还发现了一种其他写法：

```c++
node t;
t=a;
a=b;
b=t;
```

上网查阅之后才知道原来结构体可以直接赋值。

## 结构体之间赋值

结构体之间是可以直接赋值的

但要注意的是，如果含有指针，则要小心内存溢出[^1]。

[ ^1 ]:  C语言中结构体直接赋值？_茫茫大士的博客-CSDN博客_结构体赋值(https://blog.csdn.net/chenzhen1080/article/details/83239743)

