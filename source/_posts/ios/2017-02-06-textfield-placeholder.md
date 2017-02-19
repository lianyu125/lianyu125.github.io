---
title: iOS设置textfield.placeholder字体及颜色
category: iOS
tags: 
keywords: textfield
description: 
---

```
textField.placeholder = @"请输入手机号码";
[textField setValue:[UIColor blue] forKeyPath:@"_placeholderLabel.textColor"];
[textField setValue:[UIFont systemFontOfSize:14] forKeyPath:@"_placeholderLabel.font"];
```


 

