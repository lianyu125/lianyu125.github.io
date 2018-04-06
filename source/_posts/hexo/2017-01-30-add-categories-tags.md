---
title: hexo博客在导航条中添加分类和标签栏
date: 2017-01-26 18:40:12
tags: hexo
categories: hexo 
descripton: 在导航条中添加分类和标签栏
---
## 在导航栏上添加分类,标签等类目
添加导航栏方式：直接打开hexo安装根目录下的themes文件夹中，我用的是next主题（根据你主题而定），然后打开next文件夹，更改_config.yml文件，用文本方式打开，找到如下位置，默认情况下有些事注释掉的，你可以试着打开，我选择全部不注释，导航栏就是上图所示。
![](http://okjl482qy.bkt.clouddn.com/add-categories_01.png)
## 添加导航栏相应页面
导航栏我们刚刚添加成功了，这个时候如果部署 提交，你点击的话，可能是给你报错了，404 提示你找不到页面，这是因为我们只是创建了这个链接，而页面并没有创建，默认情况下home archives 页面都是不用自己创建的，所以分类、标签、关于、404都是要我们自己创建的。
在hexo安装根目录下执行如下命令,
### 创建分类页面
```
hexo new page "categories"
```
执行完，在hexo根目录下的source文件夹中会多出一个categories文件夹，里面默认有一个index.md的文件，这就是我们创建的导航栏的分类页面 。
### 创建标签页面
```
hexo new page "tags"
```
执行完，在hexo根目录下的source文件夹中会多出一个tags文件夹，里面默认有一个index.md的文件，这就是我们创建的导航栏的标签页面 。
### 创建关于页面
```
hexo new page "about"
```
执行完，在hexo根目录下的source文件夹中会多出一个about文件夹，里面默认有一个index.md的文件，这就是我们创建的导航栏的标签页面 。





