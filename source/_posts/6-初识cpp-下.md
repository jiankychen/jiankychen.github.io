---
title: 初识C++(下)
categories:
  - C++
cover: false
abbrlink: 7ca2f1e1
date: 2021-10-27 19:40:56
tags:
---

# 类
类(class)：一个类定义了一个类型(class type)，其类型名就是类名，以及与其关联的一组操作。

如：`sales_item`类定义了一个名为`sales_item`的类型

与内置类型如`int`一样，我们可以定义类类型的变量，如：```sales_item item```表示`item`是一个`sales_item`类型的对象，通常简单表述为“一个`sale-item`对象”，或者，“一个`sales_item`”

# 头文件
头文件：通常根据其中定义的类的名字来命名，通常使用`.h`作为头文件的后缀，但也有些习惯用`.H`、`.hpp`或`.hxx`。标准库头文件通常不带后缀

头文件的作用：通过使用头文件来使用标准库设施，或者是访问为自己的应用程序所定义的类

通过`#include`指令使用头文件时，若是标准库的头文件，则用尖括号`<>`包围头文件名，如`#include <iostream>`，若是不属于标准库的头文件，则用双引号`""`包围，如`#include "sales_item"`