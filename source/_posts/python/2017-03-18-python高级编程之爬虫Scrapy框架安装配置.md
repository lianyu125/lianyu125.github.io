---
title: Python高级编程之爬虫Scrapy框架安装配置
date: 2017-03-18 8:10:09
category: python
tags: python
keywords: python
description: 初级的爬虫我们利用urllib和urllib2库以及正则表达式就可以完成了，不过还有更加强大的工具，爬虫框架Scrapy
---
## Mac下安装Scrapy框架
刚刚试了下可以，可以简化为下面三个步骤
一、删除Mac自带的Python
sudo rm -rf /usr/bin/python
二、homebrew安装最新的Python
brew install python
创建一个软链接
sudo ln -s /usr/local/bin/python /usr/bin/python
三、使用pip安装scrapy
使用homebrew安装完python后会自动安装好包管理工具pip，所以执行下面安装命令
pip install scrapy
安装完成后如下所示
```
machaismile$ scrapy
Scrapy 0.22.2 - no active project

Usage:
  scrapy <command> [options] [args]

Available commands:
  bench         Run quick benchmark test
  fetch         Fetch a URL using the Scrapy downloader
  runspider     Run a self-contained spider (without creating a project)
  settings      Get settings values
  shell         Interactive scraping console
  startproject  Create new project
  version       Print Scrapy version
  view          Open URL in browser, as seen by Scrapy

  [ more ]      More commands available when run from project directory

Use "scrapy <command> -h" to see more info about a command
```
出现 zsh: command not found: scrapy 执行

 ln -s  /Library/Frameworks/Python.framework/Versions/2.7/bin/scrapy /usr/local/bin/scrapy

 

