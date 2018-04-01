---
title: xcodebuild指令详解
category: iOS
tags: iOS
keywords: xcodebuild
description: xcodebuild是苹果发布自动构建的工具。它在一个Xcode项目下能构建一个或者多个targets ，也能在一个workspace或者Xcode项目上构建scheme。
---

## xcodebuild 简介
xcodebuild 是苹果提供的打包项目或者工程的命令，了解该命令最好的方式就是使用 man xcodebuild 查看其 man page. 尽管是英文，一定要老老实实的读一遍就好了。
[官方文档](https://developer.apple.com/legacy/library/documentation/Darwin/Reference/ManPages/man1/xcodebuild.1.html)
## 使用说明
1.需要在包含 name.xcodeproj 的目录下执行 xcodebuild 命令，且如果该目录下有多个 projects，那么需要使用 -project 指定需要 build 的项目。
2.在不指定 build 的 target 的时候，默认情况下会 build project 下的第一个 target
3.当 build workspace 时，需要同时指定 -workspace 和 -scheme 参数，scheme 参数控制了哪些 targets 会被 build 以及以怎样的方式 build。
4.有一些诸如 -list, -showBuildSettings, -showsdks 的参数可以查看项目或者工程的信息，不会对 build action 造成任何影响，放心使用
## 解析官方文档
```
NAME

xcodebuild – build Xcode projects and workspaces

SYNOPSIS
1. xcodebuild [-project name.xcodeproj] [[-target targetname] … | -alltargets] [-configuration configurationname] [-sdk [sdkfullpath | sdkname]] [action …] [buildsetting=value …] [-userdefault=value …]

2. xcodebuild [-project name.xcodeproj] -scheme schemename [[-destination destinationspecifier] …] [-destination-timeout value] [-configuration configurationname] [-sdk [sdkfullpath | sdkname]] [action …] [buildsetting=value …] [-userdefault=value …]

3. xcodebuild -workspace name.xcworkspace -scheme schemename [[-destination destinationspecifier] …] [-destination-timeout value] [-configuration configurationname] [-sdk [sdkfullpath | sdkname]] [action …] [buildsetting=value …] [-userdefault=value …]

4. xcodebuild -version [-sdk [sdkfullpath | sdkname]] [infoitem]

5. xcodebuild -showsdks

6. xcodebuild -showBuildSettings [-project name.xcodeproj | [-workspace name.xcworkspace -scheme schemename]]

7. xcodebuild -list [-project name.xcodeproj | -workspace name.xcworkspace]

8. xcodebuild -exportArchive -archivePath xcarchivepath -exportPath destinationpath -exportOptionsPlist path

9. xcodebuild -exportLocalizations -project name.xcodeproj -localizationPath path [[-exportLanguage language] …]

1. xcodebuild -importLocalizations -project name.xcodeproj -localizationPath path

```
挑几个我常用的形式介绍一下，较长的使用方式以序列号代替:
* xcodebuild [-project name.xcodeproj] [[-target targetname] ... | -alltargets] build: 上述序号1的使用方式，会 build 指定 project，其中 -target 和 -configuration 参数可以使用 xcodebuild -list 获得，-sdk 参数可由 xcodebuild -showsdks 获得，[buildsetting=value ...] 用来覆盖工程中已有的配置。 action... 的可用选项如下, 打包的话当然用 build，这也是默认选项。

* xcodebuild -showsdks: 列出 Xcode 所有可用的 SDKs

* xcodebuild -showBuildSettings: 上述序号6的使用方式，查看当前工程 build setting 的配置参数，Xcode 详细的 build setting 参数参考官方文档 Xcode Build Setting Reference， 已有的配置参数可以在终端中以 buildsetting=value 的形式进行覆盖重新设置.

* xcodebuild -list: 上述序号7的使用方式，查看 project 中的 targets 和 configurations，或者 workspace 中 schemes, 输出如下:
```
Information about project "lezu":
    Targets:
        lezu
        lezuTests
        lezuUITests

    Build Configurations:
        Debug
        Release

    If no build configuration is specified and -scheme is not passed then "Release" is used.

    Schemes:
        lezu
```
* xcodebuild -workspace name.xcworkspace -scheme schemename build: 上述序号3的使用方式，build 指定 workspace，当我们使用 CocoaPods 来管理第三方库时，会生成 xcworkspace 文件，这样就会用到这种打包方式.
## 实例
以一个实际工程举例，该工程的名字叫 lezu。Scheme 名字也是 lezu。 那么 archive 的命令如下
`xcodebuild -scheme lezu  -archivePath build/lezu.xcarchive archive`
导出 ipa 包的命令如下
`xcodebuild  -exportArchive -exportFormat IPA -archivePath build/lezu.xcarchive -exportPath build/lezu.ipa`
`xcodebuild -exportArchive -archivePath lezu -exportPath lezu -exportOptionsPlist path`
依次执行完这两个命令后，工程根路径下的 build 文件夹内容如下图。


导出 ipa 包后，就可以利用 iTools 之类的软件直接安装到对应的 iPhone ，或者利用 items-service 协议来远程安装。
使用xcodebuild和xcrun打包签名





