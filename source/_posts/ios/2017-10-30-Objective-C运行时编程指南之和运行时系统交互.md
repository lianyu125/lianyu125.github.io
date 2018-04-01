---
title: Objective-C运行时编程指南之和运行时系统交互
category: iOS
tags: iOS
keywords: iOS
---
Objective-C程序有三种途径和运行时系统交互：通过Objective-C源代码；通过Foundation框架中类NSObject的方法；通过直接调用运行时系统的函数。 
<!--more-->
## 通过Objective-C源代码 
大部分情况下，运行时系统在后台自动运行，您只需编写和编译Objective-C源代码。
当您编译Objective-C类和方法时，编译器为实现语言动态特性将自动创建一些数据结构和函数。这些数据结构包含类定义和协议类定义中的信息，如在Objective-C 2.0 程序设计语言中定义类和协议类一节所讨论的类的对象和协议类的对象，方法选标，实例变量模板，以及其它来自于源代码的信息。运行时系统的主要功能就是根据源代码中的表达式发送消息，如"消息”一节所述。 
## 通过类NSObject的方法
  程序中绝大部分类都是NSObject类的子类，所以大部分都继承了NSObject类的方法，因而继承了NSObject的行为。（NSProxy类是个例外；更多细节参考““消息转发”一节)然而，某些情况下，NSObject类仅仅定义了完成某件事情的模板，而没有提供所有需要的代码。
  例如，NSObject类定义了description方法，返回该类内容的字符串表示。这主要是用来调试程序——GDB中的print-object方法就是直接打印出该方法返回的字符串。NSObject类中该方法的实现并不知道子类中的内容，所以它只是返回类的名字和对象的地址。NSObject的子类可以重新实现该方法以提供更多的信息。例如，NSArray类改写了该方法来返回NSArray类包含的每个对象的内容。
  某些NSObject的方法只是简单地从运行时系统中获得信息，从而允许对象进行一定程度的自我检查。例如，class返回对象的类；isKindOfClass:和isMemberOfClass:则检查对象是否在指定的类继承体系中；respondsToSelector:检查对象能否响应指定的消息；conformsToProtocol:检查对象是否实现了指定协议类的方法；methodForSelector:则返回指定方法实现的地址。
## 通过运行时系统的函数
运行时系统是一个有公开接口的动态库，由一些数据结构和函数的集合组成，这些数据结构和函数的声明头文件在/usr/include/objc中。这些函数支持用纯C的函数来实现和Objective-C同样的功能。还有一些函数构成了NSObject类方法的基础。这些函数使得访问运行时系统接口和提供开发工具成为可能。尽管大部分情况下它们在Objective-C程序不是必须的，但是有时候对于Objecitve-C程序来说某些函数是非常有用的。 这些函数的文档参见Objective-C 2.0 运行时系统参考库。

