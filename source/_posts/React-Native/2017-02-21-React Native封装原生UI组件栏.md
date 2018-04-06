---
title: React Native封装原生UI组件栏
date: 2017-02-21 18:40:12
tags: React-Naticve
categories: React-Native 
descripton: 有时候我们的应用需要进行访问原生平台系统的API接口,但是React Native可能还没有封装相应功能的组件还有可能我们需要去复用一些原生Objective-C,Swift或者C++代码而不是让JavaScript重新去实现一遍。或者我们可能需要些一些更加高级的功能代码，所线程相关的。例如:图片处理,数据库以及一些高级功能扩展之类的。 
---
React Native平台的开发其实本身也是可以让你写纯原生代码并且还可以让你访问原生平台的功能。这是一个比较高级的功能不过官方还是不推荐你在平时开发中使用这样的开发形式。但是如果你具备这样的开发能力，也是还是不错的。特别当在React Native暂时未提供部分原生功能或者模块，那么你可以使用这样的方法进行封装扩展。今天我们就来看一下原生组件的封装扩展方法。
     问题来源:项目中要在注册页面显示一张验证码图片,当用户点击验证码图片时,验证码改变。
     问题分析:由于RN显示图片主要有两种[请查看React-Native正确加载图片的姿势](http://www.cnblogs.com/AliliWl/p/5849524.html),而且加载的都是固定资源的图片,并不能实现点击图片换一次验证码的需求
     
## 一、对原生视图进一步封装
举个栗子,我们要在React-Native上调用ZLYCustomView显示,那我们就得对ZLYCustomView进行一次封装才能被RN调用。原生视图如下
```objc
@interface ZLYCustomView:UIView
@property (nonatomic, assign) id<VerifyPicCodeViewDelegate>delegate;
@property (nonatomic, copy) RCTDirectEventBlock onGetCookie;
@property (nonatomic, strong) UIImageView * imageView;
@property (nonatomic, strong) NSString * source;
@end

@implementation ZLYCustomView

@end
```


## 二、创建RCTViewManager的子类来管理原生视图
原生视图都需要被一个RCTViewManager的子类来创建和管理。
这些管理器在功能上有些类似“视图控制器”，但它们本质上都是单例 - React Native只会为每个管理器创建一个实例。
它们创建原生的视图并提供给RCTUIManager，RCTUIManager则会反过来委托它们在需要的时候去设置和更新视图的属性。RCTViewManager还会代理视图的所有委托，并给JavaScript发回对应的事件。

提供原生视图步骤如下：

首先创建一个子类 —— 命名规范为“视图名称+Manager”. 视图名称可以加上自己的前缀，这里最好避免使用RCT前缀，除非你想给官方pull request
添加RCT_EXPORT_MODULE()标记宏 —— 让模块接口暴露给JavaScript
实现-(UIView *)view方法 —— 创建并返回组件视图
封装属性及传递事件
下面先贴出完整的代码，然后会对属性和事件进行进一步的解说。
### ZLYCustomViewManager.h

```objc 
 #import "ZLYCustomViewManager.h
 #import "RCTViewManager.h"

@interface ZLYCustomViewManager : RCTViewManager
@end
```

    
### ZLYCustomViewManager.m

```objc
 #import "ZLYCustomViewManager.h
 #import "RCTBridgeModule.h"
 #import "RCTBridge.h"
 #import "RCTEventDispatcher.h"
@interface ZLYCustomViewManager : RCTViewManager
//  标记宏（必要）
RCT_EXPORT_MODULE()
//  事件的导出，onClickBanner对应view中扩展的属性
RCT_EXPORT_VIEW_PROPERTY(onClickBanner, RCTDirectEventBlock)
//  通过宏RCT_EXPORT_VIEW_PROPERTY完成属性的映射和导出
RCT_EXPORT_VIEW_PROPERTY(autoScrollTimeInterval, CGFloat);
RCT_EXPORT_VIEW_PROPERTY(imageURLStringsGroup, NSArray);
RCT_EXPORT_VIEW_PROPERTY(autoScroll, BOOL);

- (UIView *)view
{
    //  实际组件的具体大小位置由js控制
    ZLYCustomView *customView = [ZLYCustomView alloc]init];
    return customView;
}
@end
``` 


 
 


    




