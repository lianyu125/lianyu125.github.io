---
title: 使用hexo+gitpages创建自己的博客
date: 2017-01-26 18:40:12
tags: hexo
categories: hexo
descriptions: 工作一段时间后,我一直想拥有一个属于自己的博客网站,之前尝试过使用WordPress、jekyll+gitpages等方式创建自己的博客网站,然而这两个用着都不太顺手。有一天无意间发现微信阅读团队的博客主题比较绚丽,让我有一种眼前一亮的感觉。然后我就开始查看他们是使用什么方式构建的博客网站,最后发现他们使用hexo+gitpages方式来构建团队的博客。我在网上翻阅了相关资料后发现，用hexo+gitpages方式构建博客网站好处多多,不仅仅是速度快，还有就是可以使用leanCloud来存储阅读次数。
---
## 准备工作
1.安装git
2.安装node.js
3.申请github账号
## 安装hexo
1.安装hexo
```
npm install -g hexo-cli
```
![](/Users/one/Desktop/hexo-cli.png)

2.安装hexo
```
npm install hexo --save
```
![](/Users/one/Desktop/hexo_save.png)
【注】warning可以忽略
3.查看hexo安装是否成功
```
 hexo -v 
```
如下图所示,则表示安装成功
![](/Users/one/Desktop/hexo_v.png)
## 本地运行hexo
1.初始化hexo
```
hexo init
```
如下图则表示初始化成功
![](../img/hexo_init.png)
2.安装生成器
```
npm install 
```
3.本地运行hexo
```
hexo s -g 
```
![](../img/hexo_run.png)
 如图则表示运行成功
 ![](../img/hexo_demo.png)
## 一键部署到github
1.打开博客目录中的_config.yml文件
修改文件中的deploy下内容

```
# Deployment
## Docs: https://hexo.io/docs/deployment.html
deploy:
  type: git
  #elfwalk改为你的github用户名
  repository: https://github.com/lianyu125/lianyu125.github.io.git
  branch: master
```
2.安装hexo git 插件
```
npm install hexo-deployer-git --save
```
3.生成静态页面
```
hexo generate
```
4.发布到github
```
hexo deploy
```
这样我们的博客就搭建起来了 [](https://lianyu125.github.io)

 













