---
title: iOS textfield光标位置
category: iOS
date: 2015-03-11 8:10:09
tags: 
keywords: textfield
description: 
---

```objc
 UITextPosition* beginning = self.beginningOfDocument;   
 UITextRange* selectedRange = self.selectedTextRange;  
 UITextPosition* selectionStart = selectedRange.start;  
 UITextPosition* selectionEnd = selectedRange.end;  
 const NSInteger location = [self offsetFromPosition:beginning  toPosition:selectionStart]; //光标所在的位置 
const NSInteger length = [self offsetFromPosition:selectionStart toPosition:selectionEnd]; //选中文字的长度 
```



