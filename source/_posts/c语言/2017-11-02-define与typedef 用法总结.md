---
title: define与typedef用法总结
date: 2017-11-02 8:10:09
category: C语言
tags: C语言
keywords: C语言
---
## define的用法
 #define为一宏定义语句，通常用它来定义常量(包括无参量与带参量)，以及用来实现那些“表面似和善、背后一长串”的宏，它本身并不在编 译过程中进行，而是在这之前(预处理过程)就已经完成了，但也因此难以发现潜在的错误及其它代码维护问题，它的实例像：
<!--more--> 
```c/c++
#define INT int
#define TRUE 1 #define Add(a,b) ((a)+(b)); 
#define Loop_10 for (int i=0; i<10; i++)
```
## typedef的用法
 在C/C++语言中，typedef常用来定义一个标识符及关键字的别名，它是语言编译过程的一部分，但它并不实际分配内存空间，实例像： typedef unsigned char UCHAR; typedef可以增强程序的可读性，以及标识符的灵活性，但它也有“非直观性”等缺点。
## define 与typedef的区别
* typedef给出的符号名称仅限于对类型，而不是对值
* typedef的解释由编译器，而不是预处理器执行
* 虽然typedef的范围有限，但在其受限范围内,typedef比#define更灵活

