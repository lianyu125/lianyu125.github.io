---
title: 使用自定义URL Scheme与您的APP进行交互
date: 2015-04-16 7:10:09
category: iOS
tags: iOS
keywords: URL Scheme 
---
## 前言
本文参考文献源于[苹果官方文档](https://developer.apple.com/library/content/featuredarticles/iPhoneURLScheme_Reference/Introduction/Introduction.html#//apple_ref/doc/uid/TP40007899)
本文档描述了iOS，macOS和watchOS 2及更高版本上的系统应用程序支持的几种URL方案。 在任何平台上的Safari中运行的本机iOS应用程序和Web应用程序都可以使用这些方案与系统应用程序集成，并为用户提供更加无缝的体验。 例如，如果您的iOS应用显示电话号码，则只要有人点击其中一个号码，就可以使用适当的URL来启动电话应用。 同样，单击iTunes链接将启动iTunes应用程序并播放链接中指定的歌曲。 当用户点击链接时，会发生什么情况取决于平台和已安装的系统应用程序。

本文档描述了那些需要特殊属性或特殊格式的方案才能被关联的系统应用程序理解。 因此，本文档并未描述不同Apple平台上支持的所有URL方案。
## 概述
如果你要从iOS或MacOS应用程序启动系统应用程序，或者从运行在Safari中的Web应用程序启动系统应用程序，则应阅读本文档。 本文档包含Cocoa Touch示例代码 - 使用openURL：options：completionHandler：共享UIApplication对象的方法来打开URL和HTML样本。

### 使用邮件编写项目
使用mailto方案打开邮件应用程序并填入一封包含信息的新邮件。
Relevant Chapter: [Mail Links](https://developer.apple.com/library/content/featuredarticles/iPhoneURLScheme_Reference/MailLinks/MailLinks.html#//apple_ref/doc/uid/TP40007899-CH4-SW1)
### 开始电话或FaceTime对话
使用tel和facetime scheme启动电话或视频对话。
Relevant Chapter: [Phone Links](https://developer.apple.com/library/content/featuredarticles/iPhoneURLScheme_Reference/PhoneLinks/PhoneLinks.html#//apple_ref/doc/uid/TP40007899-CH6-SW1), [FaceTime Links](https://developer.apple.com/library/content/featuredarticles/iPhoneURLScheme_Reference/FacetimeLinks/FacetimeLinks.html#//apple_ref/doc/uid/TP40007899-CH2-SW1)
### 指定文本消息
使用短信 scheme撰写短信并指定收件人。
Relevant Chapter: [SMS Links](https://developer.apple.com/library/content/featuredarticles/iPhoneURLScheme_Reference/SMSLinks/SMSLinks.html#//apple_ref/doc/uid/TP40007899-CH7-SW1)
### 在地图中打开位置
使用特殊格式的网址来打开地图应用并显示路线或位置。
相关章节：[地图链接](https://developer.apple.com/library/content/featuredarticles/iPhoneURLScheme_Reference/MapLinks/MapLinks.html#//apple_ref/doc/uid/TP40007899-CH5-SW1)

### 在iTunes中打开项目
使用特殊格式的URL打开iTunes并在iTunes音乐商店中显示项目。
相关章节：[iTunes链接](https://developer.apple.com/library/content/featuredarticles/iPhoneURLScheme_Reference/iTunesLinks/iTunesLinks.html#//apple_ref/doc/uid/TP40007899-CH3-SW1)

### 打开YouTube视频
使用特殊格式的网址在Safari中打开YouTube视频。
相关章节：[YouTube链接](https://developer.apple.com/library/content/featuredarticles/iPhoneURLScheme_Reference/YouTubeLinks/YouTubeLinks.html#//apple_ref/doc/uid/TP40007899-CH8-SW1)


