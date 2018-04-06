---
title: iOS Button在两层View以上的不可点击
category: iOS
date: 2016-07-21 
tags: 
keywords: iOS,UIButton
description: 我们经常会遇到一个按钮创建后，明明添加了点击事件，但是点击事件就是不响应,这时候是是不是很恼火。
---

## 问题描述
我在ZLYView上创建了一个普通的Button,点击事件已经添加过,但是点击事件就是不响应。
## 问题产生的原因
1.按钮的父视图的`userInteractionEnabled` 交互属性设为NO了 【注】imageView的这个属性默认是为NO的,如果在ImageView上添加按钮,那么应当设置`imageView.userInteractionEnabled = YES;`
2.按钮的位置未在父视图内。【注】如果一直找不到原因,请仔细分析是否是该原因。

## 解决方案
根据问题对症下药

