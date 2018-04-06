---
title: Objective-C运行时编程指南之类型编码
category: iOS
date: 2016-11-15 8:10:09
tags: iOS
keywords: iOS
---
为了和运行时系统协作，编译器将方法的返回类型和参数类型都编码成一个字符串，并且和方法选标关联在一起。这些编码在别的上下文环境中同样有用，所以您可以直接使用@encode()编译指令来得到具体的编码。给定一个类型， @encode()将返回该类型的编码字符串。类型可以是基本类型例如整形，指针，结构体或者联合体，也可以是一个类，就和C语言中的sizeof()操作符的参数一样，可以是任何类型。
```objc
char *buf1 = @encode(int **);
char *buf2 = @encode(struct key);
char *buf3 = @encode(Rectangle);
```
下表列出了这些类型编码。注意，它们可能很多和您使用的对象编码有一些重合。然而，这儿列出来的有些编码是您写编码器时候不会使用的，也有一些不是@encode()产生的，但是在您写编码器的时候是会使用的。（关于对象编码的更多信息，请参考Foundation框架参考库中的NSCoder类文档。） 
![](http://okjl482qy.bkt.clouddn.com/type_encode_01.png)
![](http://okjl482qy.bkt.clouddn.com/type_encode_02.png)
**重要： Objective-C不支持long double类型。 @encode(long double)和double一样，返回的字符串都是d**
数组的类型编码以方括号来表示，紧接着左方括号的是数组元素的数量，然后是数据元素的类型。例如，一个12个浮点数（floats）指针的数组可以表示如下： 
```objc
 [12^f] 
```
结构体和联合体分别用大括号和小括号表示。括号中首先是结构体标签，然后是一个=符号，接着是结构体中各个成员的编码。例如，结构体 
```objc
typedef struct example {
     id   anObject;
     char *aString;
     int  anInt;
} Example;  
```
的编码如下：
```objc
{example=@*i} 
```
定义的类型名（Example）和结构体标签（example）有同样的编码结果。指向结构体类型的指针的编码同样也包含了结构体内部数据成员的编码信息，如下所示： 
```objc
^{example=@*i} 
```
然而，更高层次的间接关联就没有了内部数据成员的编码信息： 
```objc
 ^^{example} 
```
对象的编码类似结构体。 例如， @encode()对NSObject编码如下： 
```objc
 {NSObject=#} 
```
NSObject类仅声明了一个Class类型的实例变量，isa。 注意，尽管有一些编码无法从 @encode() 的结果中直接得到，但是运行时系统会使用它们来表示协议类中方法的修饰符，这些编码如表6-2所示。 
![](http://okjl482qy.bkt.clouddn.com/type_encode_03.png)


