---
title: 存储类型
author: saltycat
date: 2022-05-22 14:58:57 +0800
categories: []
tags: []
math: true
comments: true
mermaid: true
typora-root-url: ../../sa1tycat.github.io

---



# 存储类型



## auto 型

局部变量可以声明为 auto 型（实际上可以省略）

对于 auto 型变量，在调用函数时，会为他们分配空间，函数调用结束时，空间被释放。

``` c++
int fun(){
    auto int a; // auto 可省略
    ...
    return a;
}
```

例如这道题，函数内部 m,n 均为 auto 型，每次调用都是重新开辟空间，上一次的修改并不会保存，因此选 B。

![ppt1](/assets/blog_res/2022-05-22-storage-class.assets/cover346_20220520100326.jpg)

## static 型

static 型局部变量存储在静态存储区，整个程序运行期间均占有内存，只有在程序结束时才会释放内存。

也就是说，第一次调用函数对他做出的改变，在第二次调用时并不会改变。

例如：

![ppt2](/assets/blog_res/2022-05-22-storage-class.assets/cover345_20220520100326.jpg)

主函数中一共调用 3 次 Function() ，由于 Function() 内部 n 为 static 型，因此前一次调用的改变是保留的，也就是说：

i=1 时，调用 Function() ，在 Function() 内 n 变为 2；

i=2 时，调用 Function() ，在 Function() 内 n 变为 2+1=3；

i=3 时，调用 Function() ，在 Function() 内 n 变为 3+1=4；

故这道题选 A。

又例如这道题

![img](/assets/blog_res/2022-05-22-storage-class.assets/slide_9_3_20220520114146.png)

我们不妨利用这个程序来帮助理解：

```c++
#include<cstdio>
using namespace std;
int f(int a,int b){
    static int m=1;
    printf("进入f后 内部最开始m=%d\n",m);
    m=m+2*a+b;
    printf("进入f后 m的值变为m=%d\n",m);
    return m;
}

int main(){
    int k=2,m=3,p;
    printf("在主函数里 m=%d\n第一次",m);
    p=f(k,m);
    printf("第一次调用 f 后 p=%d\n",p);
    printf("第一次调用 f 后 主函数里 m=%d\n第二次",m);
    p=f(k,m);
    printf("第二次调用 f 后 p=%d\n",p);
    return 0;
}
```

输出结果：

在主函数里 m=3
第一次进入f后 内部最开始m=1
进入f后 m的值变为m=8
第一次调用 f 后 p=8
第一次调用 f 后 主函数里 m=3
第二次进入f后 内部最开始m=8
进入f后 m的值变为m=15
第二次调用 f 后 p=15

首先需要明确的是，不管是在 main() 还是在 f() 中，m 都只是局部变量，作用域仅限于对应的函数中。

在 main() 中，m 为 auto 型，函数调用结束后 m 的内存释放。而在 f() 中，m 是 static 型，也就是说，只有在程序结束后，其内存才会被释放，f 函数调用结束并不会释放他的内存。

从输出结果也可以看到，第一次结束调用 f() 时，f() 中的 m 从1变成了 8，而在第二次调用 f() 时，并没有为 f() 中的 m 重新开辟空间，而是继续读取之前 m 所储存的空间，故最开始 m 的值仍为 8.

## register 型

register 型变量储存在寄存器中，而非内存之中。因为寄存器读取速度比内存快一些，因此 register 类型读取更快。通常编译器会自主选择是否将某个变量变成 register 型。

1.如果对寄存器变量使用＆运算符，则编译器可能会给出错误或警告（取决于您使用的编译器），因为当我们说变量是寄存器时，它可能存储在寄存器中而不是内存中，并且寄存器的访问地址无效。

2.register关键字可以与指针变量一起使用。显然，寄存器可以具有存储位置的地址。以下程序不会有任何问题。

3.寄存器是一个存储类，并且C不允许变量使用多个存储类说明符。因此，register不能与static一起使用。

4.寄存器只能在一个块内使用（局部），而不能在全局范围内（在主外部）使用。

5.C程序中对寄存器变量的数量没有限制，但重点是编译器可能会将某些变量放入寄存器中，而有些则不会。

## extern 型

若要在全局变量声明之前使用它的值，则需要用到 extern 类型。

如：

``` c++
#include<cstdio>
using namespace std;
extern int a,b; // 全局变量声明在最后面，可以提前声明来引用

void f(){
    printf("a=%d b=%d",a,b);
}

int main(){
    f();
    return 0;
}

int a=3,b=2;
```

最后输出结果：a=3 b=2

实际上 extern 类型更常用于工程文件中，需要在一个 cpp 文件中调用另一个 cpp 文件的全局变量时，则需要用 extern 来声明。

## 函数储存类型

函数储存类型：

- 内部函数：只能被当前文件的函数调用的函数，储存类型：static
- 外部函数：可供其他文件的函数调用的函数，储存类型：extern

默认省略储存类型的函数为 extern 外部函数。

如果要调用外部文件必须：

1. 被调函数为 extern
2. 主调函数内必须用 extern 声明被调函数

如：

主调函数：

{: file='main.cpp'}

``` c++
#include<cstdio>
using namespace std;
extern int f(int );
int main(){
    f(1);
    return 0;
}
```

{: file='main.cpp'}

被调函数：

{: file='extern.cpp'}

``` c++
#include<cstdio>
using namespace std;
int f(int n){
    return n==1?f(n-1)*n;
}
```



