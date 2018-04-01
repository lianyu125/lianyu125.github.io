---
title: cocoapods私有库的创建
category: iOS
tags: iOS
keywords: cocoapods
---
## 创建版本库
创建一个像 master 一样的存放版本描述文件的git仓库，因为是私人git仓库，我选择 oschina 创建远程私有仓库。
![](http://okjl482qy.bkt.clouddn.com/cocoapods_repo_01.png)
在终端执行如下命令，将这个远程的私有版本仓库添加到本地，repo 就是 repository 储存库的缩写
```bash
pod repo add  lianyuRepo https://gitee.com/xiaoyuu/lianyuRepo.git
```
查看在 Finder 目录 ~/.cocoapods/repos， 可以发现增加了一个 lianyuRepo 的储存库
![](http://okjl482qy.bkt.clouddn.com/cocoapods_repo_02.png)
## 创建代码库
创建时添加 MIT License 和 README
![](http://okjl482qy.bkt.clouddn.com/cocoapods_repo_03.png)
将仓库克隆到本地，添加你的代码文件、仓库名.podspec 描述文件,如下所示:


.podspec 文件是你这个代码库的pod描述文件,可以通过pod指令创建空白模板：
`pod spec create UnifiedJump`
将.podspec文件修改如下

```

Pod::Spec.new do |s|

  s.name         = "UnifiedJump"
  s.version      = "0.0.1"
  s.summary      = "Unified transfer agreement between modules"

  # This description is used to generate tags and improve search results.
  #   * Think: What does it do? Why did you write it? What is the focus?
  #   * Try to keep it short, snappy and to the point.
  #   * Write the description between the DESC delimiters below.
  #   * Finally, don't worry about the indent, CocoaPods strips it!
  s.description  = <<-DESC
                   模块间统一跳转协议
                   模块间统一跳转协议
                   模块间统一跳转协议
                   模块间统一跳转协议
                   模块间统一跳转协议
                   模块间统一跳转协议
                   模块间统一跳转协议
                   DESC

  s.homepage     = "https://devzou.com"
  s.license      = "MIT"
  # s.license      = { :type => "MIT", :file => "LICENSE" }


  # ――― Author Metadata  ―――――――――――――――――――――――――――――――――――――――――――――――――――――――――      s.author             = { "lianyu.zou" => "lianyu125@gmail.com" }
  # Or just: s.author    = "lianyu.zou"
  # s.authors            = { "lianyu.zou" => "lianyu.zou@qunar.com" }
  # s.social_media_url   = "http://twitter.com/lianyu.zou"


  # s.platform     = :ios
    s.platform     = :ios, "8.0"

  s.source       = { :git => "https://github.com/lianyu125/UnifiedJump.git", :tag => "#{s.version}" }

end
```
然后开始验证我们的仓库配置是否正确，并按照要求进行修改
```
pod lib lint UnifiedJump.podspec —allow-warnings
```
验证成功后如下
```
 -> UnifiedJump (0.0.1)

UnifiedJump passed validation.
```
## 将描述文件推送到版本库
将项目打上标签推到远程仓库，标签号 和 版本号对应 都是0.0.1
最后将我们的代码仓库的描述信息，push 到我们的版本仓库中

```
pod repo push lianyuRepo UnifiedJump.podspec
```
## 私有库的使用
使用私有pod库的需要在Podflie中添加这句话，指明你的版本库地址。
```
https://gitee.com/xiaoyuu/lianyuRepo.git
```
注意是版本库的地址，而不是代码库的地址
若有还使用了公有的pod库，需要把公有库地址也带上

