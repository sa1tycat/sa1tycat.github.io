---
title: C++之vector
author: saltycat
date: 2022-07-12 19:20:37 +0800
categories: [C++]
tags: []
math: true
comments: true
mermaid: true
typora-root-url: ../../sa1tycat.github.io
---

## vector 定义

vector 是一个长度不定的数组，要使用它，首先要 `include <vector>`，然后按照 `std::vector<elementType> name;` 这样的形式进行定义，如：`std::vector<std::string> students;`。

值得一提的是，可以通过 `{}` 来初始化 vector 的值，如：`std::vector<std::string> students = {"Charles", "Ann"};`

## Index 下标

和数组一样，下标从 0 开始，可以通过 `[index]` 来访问下标为 `index` 的元素。

## `.size()`函数

和 `string` 的 `.length()` 一样，可以使用 vector 的 `.size()` 来返回元素的数量。

## `.push_back() & .pop_back()` 函数

通过 `.push_back(element)` 来添加元素到末尾；通过 `.pop_back()` 来删除末尾元素。



