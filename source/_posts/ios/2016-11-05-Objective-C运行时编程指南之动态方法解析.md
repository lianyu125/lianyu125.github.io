
---
title: Objective-C运行时编程指南之动态方法解析
category: iOS
date: 2016-11-05 8:10:09
tags: iOS
keywords: iOS
---
  本章将描述怎样动态地提供一个方法的实现。 
  <!--more-->
## 动态方法解析
 有时候，您需要动态地提供一个方法的实现。例如，Objective-C中属性（Property）（参考Objective-C 2.0 程序设计语言中属性小节）前的修饰符@dynamic 
```objc 
@dynamic propertyName;
```
表示编译器须动态地生成该属性对应地方法。
您可以通过实现resolveInstanceMethod:和resolveClassMethod:来动态地实现给定选标的对象方法或者类方法。 
Objective-C方法可以认为是至少有两个参数——self和_cmd—— 的C函数。您可以通过class_addMethod方法将一个函数加入到类的方法中。例如，有如下的函数： 
```objc
void dynamicMethodIMP(id self, SEL _cmd) {     
     // implementation ....
 } 
``` 
 您可以通过resolveInstanceMethod:将它作为类方法resolveThisMethodDynamically的实现： 
```objc
 @implementation MyClass
  + (BOOL)resolveInstanceMethod:(SEL)aSEL {
       if (aSEL == @selector(resolveThisMethodDynamically)) {           
           class_addMethod([self class], aSEL, (IMP) dynamicMethodIMP, "v@:");
             return YES;
       }
       return [super resolveInstanceMethod:aSEL]; 
 } 
@end 
```
通常消息转发（见 “消息转发”）和动态方法解析是互不相干的。在进入消息转发机制之前，respondsToSelector:和instancesRespondToSelector: 会被首先调用。您可以在这两个方法中为传进来的选标提供一个IMP。如果您实现了resolveInstanceMethod:方法但是仍然希望正常的消息转发机制进行，您只需要返回NO就可以了。 
## 动态加载 
Objective-C程序可以在运行时链接和载入新的类和范畴类。新载入的类和在程序启动时载入的类并没有区别。 动态加载可以用在很多地方。例如，系统配置中的模块就是被动态加载的。 在Cocoa环境中，动态加载一般被用来对应用程序进行定制。您的程序可以在运行时加载其他程序员编写的模块——和Interface Build载入定制的调色板以及系统配置程序载入定制的模块的类似。 这些模块通过您许可的方式扩展了您的程序，而您无需自己来定义或者实现。您提供了框架，而其它的程序员提供了实现。 尽管已经有一个运行时系统的函数来动态加载Mach-O文件中的Objective-C模块（objc_loadModules，在objc/objc-load.h中定义），Cocoa的NSBundle类为动态加载提供了一个更方便的接口——一个面向对象的，已和相关服务集成的接口。关于NSBundle类的更多相关
信息请参考Foundation框架中关于NSBundle类的文档。关于Mach-O文件的有关信息请参考Mac OS X ABI Mach-O 文件格式参考库


