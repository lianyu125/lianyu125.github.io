---
title: React-Native环境配置之安装Node.js
date: 2017-5-16 8:10:09
tags: React-Naticve
categories: React-Native 
---
## 一、安装NVM

nvm的作用：nvm是Node的管理器,用来安装Node.js.

 安装步骤：
### 1.使用命令行brew install curl或者brew install wget来确保已经安装curl或者wget。
### 2.安装方式有两种
 使用curl方式安装
`curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.31.0/install.sh | bash`
使用wget安装
`wget -qO- https://raw.githubusercontent.com/creationix/nvm/v0.31.0/install.sh | bash`
我是采用第一种安装方式安装的,截图如下,请注意红框中的路径一会儿要使用哦
![](http://img.blog.csdn.net/20160915123223820?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQv/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)

### 3.在.bash_profile配置一下环境变量使用命令sudo vi ~/.bash_profile 打开该文件，
![](http://img.blog.csdn.net/20160915123854203?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQv/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)
加入如下信息
`export NVM_DIR="上面图片中红框的路径"[ -s "$NVM_DIR/nvm.sh" ] && . "$NVM_DIR/nvm.sh"  #This loads nvm`
然后重新打开命令行输入 nvm --version 检验是否安装成功
![](http://img.blog.csdn.net/20160915124321403?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQv/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)
我的已经安装成功,版本为0.31.0
## 二、使用nvm安装Node.js
在终端使用如下命令
`nvm install node && nvm alias default node`
如图node.js 安装成功
![](http://img.blog.csdn.net/20160915124709686?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQv/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)


