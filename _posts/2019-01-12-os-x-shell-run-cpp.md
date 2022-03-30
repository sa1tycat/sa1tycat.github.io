---
title: OS X下利用shell脚本在Terminal中直接编译运行cpp文件
author: saltycat
date: 2019-01-12 19:20:24 +0800
categories: [实用]
tags: []
math: true
comments: true
mermaid: true
typora-root-url: ../../sa1tycat.github.io

---

每次用Terminal运行cpp文件是总是麻烦地重复输入 `g++ -o name name.cpp` 以至于引起不适（比如昨天调试 `gcd()` 时），于是学了一下shell脚本，发现是真的方便。

# 操作方法

1.打开Terminal

2.输入 `vi name.sh` ，来创建一个脚本

3.输入以下代码：



```shell
#!/bin/bash
read name
cd 文件目录
g++ -o ${name} ${name}.cpp
./${name}
```


### 代码解释



 - 第二行读入文件名字
 - 请将默认保存cpp文件的地址替换掉“文件目录”
 - 下面几行自动输入指令

4.输入 `:wq` 以保存并退出

5.执行时，输入 `. .\name.sh` （注意.之间有空格）

6.输入cpp文件名（不用输入.cpp），回车

7.Enjoy it！