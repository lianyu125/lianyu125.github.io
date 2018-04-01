---
title: Cocoapods 之pod search无法搜索到类库的解决办法
category: iOS
tags: cocoapods
keywords: iOS
description: 当自己向cocoapods提交了自己的代码库时，用pod search找不到
---
## 问题描述
pod search ViewLayoutExtention(自己向cocoapods提交的代码库)
提示如下错误
```bash
[!] Unable to find a pod with name, author, summary, or descriptionmatching '······'
```
## 解决办法
```bash
rm ~/Library/Caches/CocoaPods/search_index.json
pod search ViewLayoutExtention
```

## 我当时的操作过程
在终端输入pod setup,会出现Setting up CocoaPods master repo，等几分钟，会输入Setup completed，说明pod setup执行成功。
结果pod search还是失败
在终端输入pod search ViewLayoutExtention
依然还是提示Unable to find a pod with name, author, summary, or description matching 'ZLYNetWorking'。
但是我输入pod search AFNetworking，却有相应的结果。
删除~/Library/Caches/CocoaPods目录下的search_index.json文件

pod setup成功后会生成~/Library/Caches/CocoaPods/search_index.json文件。
终端输入rm ~/Library/Caches/CocoaPods/search_index.json
删除成功后再执行pod search
终端输入：pod search ViewLayoutExtention(不区分大小写)
输出：Creating search index for spec repo 'master'.. Done!，稍等片刻就会出现所有带ViewLayoutExtention字段的类库出现。



