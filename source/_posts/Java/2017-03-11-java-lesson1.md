---
title: Java入门教程之第一个程序
category: Java
tags: Java
keywords: Java
description: 学习编程语言的第一个程序一般是从Hello world开始的,Java也不例外
---
## 前言
Java 是由Sun Microsystems公司于1995年5月推出的高级程序设计语言。
Java可运行于多个平台，如Windows, Mac OS，及其他多种UNIX版本的系统。
本教程通过简单的实例将让大家更好的了解JAVA编程语言。
## 编译器安装 
1.下载eclipse,下载地址 [https://www.eclipse.org/downloads/download.php?file=/oomph/epp/neon/R2a/eclipse-inst-mac64.tar.gz&mirror_id=1222](https://www.eclipse.org/downloads/download.php?file=/oomph/epp/neon/R2a/eclipse-inst-mac64.tar.gz&mirror_id=1222)
2.下载下来的文件为tar.gz格式的。【注】
3.在终端上执行命令`tar zxvf /Users/one/Desktop/eclipse-java-neon-2-macosx-cocoa-x86_64.tar.gz` 即可完成安装

## 第一个Java程序
打开编译器,如图选择工作空间,
![](http://okjl482qy.bkt.clouddn.com/java_01.png)
然后点击ok进入下一个页面
![](http://okjl482qy.bkt.clouddn.com/java_02.png)
选择creat a new Java Project 
为你的工程起一个名字,比如HelloJava
![](http://okjl482qy.bkt.clouddn.com/java_03.png)
在src文件下创建HelloWord类
![](http://okjl482qy.bkt.clouddn.com/java_04.png)
然后点击运行如图
![](http://okjl482qy.bkt.clouddn.com/java_05.png)
第一个程序运行成功Hello World
```java
public class HelloWorld {
    public static void main(String []args) {
        System.out.println("Hello World");
    }
}
```




