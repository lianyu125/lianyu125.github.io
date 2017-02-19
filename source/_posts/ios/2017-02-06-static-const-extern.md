---
title: static、const、extern简介
category: iOS
tags: 
keywords: textfield
description: 
---
## const
1.const作用:仅仅是用来修饰右边的变量(只能修饰变量:基本变量、指针变量、对象变量)
2.const修饰的变量表示只读
## const与宏的区别
1.编译时刻不同,宏:预编译const:编译时刻
  2.宏不会做编译检查错误,const会做编译检查错误
  3.宏可以定义代码
  4.过多的使用宏会导致编译时间过长
const:当有字符串常量的时候,苹果推荐我们使用const



