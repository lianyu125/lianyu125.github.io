---
title: 让你的库文件支持cocoapods
category: iOS
tags: cocoapods
keywords: [iOS,cocoapods]
description: iOS模块化编程最重要的工具莫过于cocoapods,下面介绍一下让你的库文件支持cocoapods
---
## 整体步骤
1.在本地创建静态库工程
2.创建podspec文件,并验证是否通过
3.上传到github,并创建release版本
4.注册cocoapods账号
5.上传代码到cocoapods
6.检查上传是否成功
## 首先我们打开github.com，然后创建自己的项目工程：
![](http://okjl482qy.bkt.clouddn.com/cocoapods_170317_01.png)
【注】因为我的github中已经创建了上面那个仓库，所以才会有上面那个提示
这里注意那个MIT License，在后面添加Cocoapods支持的时候会用到（稍后介绍）。然后点击创建即可。
然后用SouceTree将代码down到本地,你会发现文件夹中包含一个LICENSE文件，这里的LICENSE就是刚才说的MIT License添加的文件。然后我们在ZLYNetWorking文件夹中创建一个静态库工程![](http://okjl482qy.bkt.clouddn.com/cocoapods_01.png)

接下来就开始编写自己的代码了，然后提交到Github就可以了。

 

## 创建podspec文件

我们使用终端到仓库的根目录下ZLYNetWorking.
然后执行下面的命令：
`pod spec create ZLYNetWorking`
这里的ZLYNetWorking就是pod添加市的名字（例如MBProgressHUD）。执行完后的结果： 
![](http://okjl482qy.bkt.clouddn.com/cocoapods_170317_02.png)
此时在工程文件夹下也会多一个ZLYNetWorking.podspec文件。这里我用xcode打开并做了如下编辑：

```objc
#
#  Be sure to run `pod spec lint ZLYNetWorking.podspec' to ensure this is a
#  valid spec and to remove all comments including this before submitting the spec.
#
#  To learn more about Podspec attributes see http://docs.cocoapods.org/specification.html
#  To see working Podspecs in the CocoaPods repo see https://github.com/CocoaPods/Specs/
#

Pod::Spec.new do |s|

# ―――  Spec Metadata  ―――――――――――――――――――――――――――――――――――――――――――――――――――――――――― #
  #
  #  These will help people to find your library, and whilst it
  #  can feel like a chore to fill in it's definitely to your advantage. The
  #  summary should be tweet-length, and the description more in depth.
  #

  s.name         = "ZLYNetWorking"
  s.version      = "0.0.1"
  s.summary      = "A network Framework based on AFNetworking"

  # This description is used to generate tags and improve search results.
  #   * Think: What does it do? Why did you write it? What is the focus?
  #   * Try to keep it short, snappy and to the point.
  #   * Write the description between the DESC delimiters below.
  #   * Finally, don't worry about the indent, CocoaPods strips it!
  s.description  = <<-DESC
                    网络请求模块
                    网络请求模块
                    网络请求模块
                    网络请求模块
                    网络请求模块
                    网络请求模块
                   DESC

  s.homepage     = "https://github.com/lianyu125/ZLYNetWorking"
  # s.screenshots  = "www.example.com/screenshots_1.gif", "www.example.com/screenshots_2.gif"


  # ―――  Spec License  ――――――――――――――――――――――――――――――――――――――――――――――――――――――――――― #
  #
  #  Licensing your code is important. See http://choosealicense.com for more info.
  #  CocoaPods will detect a license file if there is a named LICENSE*
  #  Popular ones are 'MIT', 'BSD' and 'Apache License, Version 2.0'.
  #

  # s.license      = "MIT"
    s.license      = { :type => "MIT", :file => "LICENSE" }


  # ――― Author Metadata  ―――――――――――――――――――――――――――――――――――――――――――――――――――――――――   #  
  #
  #  Specify the authors of the library, with email addresses. Email addresses
  #  of the authors are extracted from the SCM log. E.g. $ git log. CocoaPods also
  #  accepts just a name if you'd rather not provide an email address.
  #
  #  Specify a social_media_url where others can refer to, for example a twitter
  #  profile URL.
  #

  s.author             = { "lianyu.zou" => "lianyu125@gmail.com" }
  s.platform     = :ios, "7.0"


  s.source       = { :git => "https://github.com/lianyu125/ZLYNetWorking.git", :branch => "master" }
  s.source_files  = "ZLYNetWorking", "ZLYNetWorking/**/*.{h,m}"
  s.exclude_files = "ZLYNetWorking/Exclude"
end
```

name:类库的名称这里字段介绍如下：
version:库的版本
summary就是介绍语,就是：
homtepage:Github上项目地址
license:许可证
author:作者
source:项目的https链接地址
source_files:要共享的代码，这里是ZLYNetWorking下面的所有代码。
接下来执行下面的命令进行验证：

`pod lib lint ZLYNetWorking.podspec`
结果多种多样，如果有错，则按照提示进行改错即可。在这里，我执行的结果如下图： 
![](http://okjl482qy.bkt.clouddn.com/cocoapods_170317_03.png)
发现了多个警告，只要不是错误就行，警告可以直接忽略（红色也提示如何忽略）：
pod lib lint ZLYNetWorking.podspec —allow-warnings
结果如下：
![](http://okjl482qy.bkt.clouddn.com/cocoapods_170317_04.png)
当看到ZLYNetWorking passed validation之后，就说明验证通过了。
## 在Github上创建release版本
打开项目的目录，然后创建release版本的类库：
![](http://okjl482qy.bkt.clouddn.com/cocoapods_170317_05.png)
点击 箭头指向开始创建release版本，（点击 Create a new release）：
![](http://okjl482qy.bkt.clouddn.com/cocoapods_170317_06.png)
点击Publish release即可。创建完成后如图所示：
![](http://okjl482qy.bkt.clouddn.com/cocoapods_170317_07.png)
这样第三步就完成了
![](http://okjl482qy.bkt.clouddn.com/cocoapods_170317_08.png)
## 注册CocoaPods账号

执行命令行：

`pod trunk register 邮箱地址 ‘用户名’ —description='描述信息'`

黄色提示已经发送了一个验证码到邮箱，你可以打开你的邮箱验证即可。
这样就成功注册了Cocoapods账号。

可以用

`pod trunk me`
检查是否创建成功。成功的结果如下：
![](http://okjl482qy.bkt.clouddn.com/cocoapods_170317_09.png)
## 上传代码到CocoaPods

首先检测文件格式的有效性：

pod spec lint
结果如下： 
![](http://okjl482qy.bkt.clouddn.com/cocoapods_170317_10.png)
没有错误，但是有警告。可以使用 —allow-warnings忽略：
出现passed validation就说明通过验证了。然后执行：

`pod trunk push ZLYNetWorking.podspec —allow-warnings`
执行结果如下：（速度应该有的慢）
![](http://okjl482qy.bkt.clouddn.com/cocoapods_170317_10.png)
说明了已经上传成功。

 

## 检查上传是否成功

使用
`pod search ZLYNetWorking`

若果提示如下错误
```bash
[!] Unable to find a pod with name, author, summary, or descriptionmatching '······'
```
则请看[我的另一篇博客](https://www.devzou.com/2017/03/15/ios/2017-03-15-pod%20search/)

ok，已经成功了。这样就可以让其他人进行搜索使用了。


