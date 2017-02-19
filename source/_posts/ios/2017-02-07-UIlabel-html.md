---
title: iOS UILabel显示html标签
category: iOS
tags: 
keywords: textfield
description: 
---
iOS7以后系统提供了显示html标签的方法
```
UIKIT_EXTERN NSString *const NSHTMLTextDocumentType NS_AVAILABLE_IOS(7_0);
```

```
NSString *str = @"<font color=\"#6c6c6c\">满20减5 满40减15，还剩<font color=\"#ff9147\">113天";
UILabel *label = [[UILabel alloc] initWithFrame:CGRectMake(30, 50, 300, 50)];
NSAttributedString *attrStr = [[NSAttributedString alloc] initWithData:[str dataUsingEncoding:NSUnicodeStringEncoding] options:@{NSDocumentTypeDocumentAttribute:NSHTMLTextDocumentType} documentAttributes:nil error:nil];
label.attributedText = attrStr;
//如果想要改变文字的字体,请在设置attributedText之后设置
label.font = [UIFont systemFontOfSize:20];
[self.view addSubview:label];
```

