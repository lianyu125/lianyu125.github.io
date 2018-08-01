---
title: React-Native For IOS 开发之环境配置
date: 2017-5-16 8:10:09
tags: React-Naticve
categories: React-Native 
---
## React-Native环境安装
### 1.安装Homebrew。
   Homebrew简称brew，是Mac OSX上的软件包管理工具，能在Mac中方便的安装软件或者卸载软件。Homebrew的安装十分简单，只需打开终端复制、粘贴以下命令，回车，搞定。附[Homebrew官网](http://brew.sh/)
`/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"`
   通过命令行执行brew -v进行检查brew 是否已经安装成功。安装成功之后的提示如下
![](http://img.blog.csdn.net/20160915082408843?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQv/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)
### 2.安装Node.js 
安装Node.js有两种方法

1.使用Homebrew来安装,在终端执行brew install node ,并按回车，执行完毕之后使用 node -v检查是否安装成功
![](http://img.blog.csdn.net/20160915083741376?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQv/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)
2.使用NVM安装，详细步骤请看这里

## 3.安装React-Native的命令行工具。
React-Native的命令行工具用于执行创建、初始化更新项目、运行打包服务等任务。安装命令为
`npm install -g react-native-cli`
安装这一步需要较长时间,请耐心等待
安装成功后如下
![](http://img.blog.csdn.net/20160915084527186?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQv/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)

## 4.Xcode  
React-Native 目前需要 Xcode 7.0或更高版本，你可以通过App Store或是到Apple开发者官网上下载。这一步骤会同时安装Xcode IDE和Xcode的命令行工具。

完成以上安装就可以初始化您的第一个React-Native项目,但是很遗憾在我创建第一个项目的时候提示如下错误
![](http://img.blog.csdn.net/20160915093120211?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQv/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)
这是由于网络原因导致react-native命令行从npm官方源拖代码时出问题,解决方法请到[这里](http://blog.csdn.net/pengyuan_d/article/details/50622315)。
## 5.推荐安装工具

    Watchman  。Watchman是由Facebook提供的监视文件系统变更的工具。安装此工具可以提高开发时的性能（packager可以快速捕捉文件的变化从而实现实时刷新）。

  安装指令为
`brew install watchman`
安装成功如下
![](http://img.blog.csdn.net/20160915130237736?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQv/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)

### 5.2 Flow
  Flow是一个静态的JS类型检查工具。译注：你在很多示例中看到的奇奇怪怪的冒号问号，以及方法参数中像类型一样的写法，都是属于这个flow工具的语法。这一语法并不属于ES标准，只是Facebook自家的代码规范。所以新手可以直接跳过（即不需要安装这一工具，也不建议去费力学习flow相关语法）。
安装方法
`brew install flow`
![](http://img.blog.csdn.net/20160915091627762?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQv/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)
查看安装是否成功使用 flow  --version 来查看
![](http://img.blog.csdn.net/20160915091744030?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQv/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)
在桌面上创建工程如下
![](http://img.blog.csdn.net/20160915130039942?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQv/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)
然后就可以运行你的第一个React-Native项目啦

