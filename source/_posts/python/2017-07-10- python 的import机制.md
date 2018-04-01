---
title: python的import机制
date: 2017-07-04 8:10:09
tags: python
categories: python 
description: python的import机制
---
python中，每个py文件被称之为模块，每个具有__init__.py文件的目录被称为包。只要模块或者包所在的目录在sys.path中，就可以使用import 模块或import 包来使用。 
  
如果想使用非当前模块中的代码，需要使用Import，这个大家都知道。 
如果你要使用的模块（py文件）和当前模块在同一目录，只要import相应的文件名就好，比如在a.py中使用b.py： 
import b 
但是如果要import一个不同目录的文件(例如b.py)该怎么做呢？ 
首先需要使用sys.path.append方法将b.py所在目录加入到搜素目录中。然后进行import即可，例如 
import sys 
sys.path.append('c:\xxxx\b.py') # 这个例子针对 windows 用户来说的 
大多数情况，上面的代码工作的很好。但是如果你没有发现上面代码有什么问题的话，可要注意了，上面的代码有时会找不到模块或者包（ImportError: No module named xxxxxx），这是因为： 
sys模块是使用c语言编写的，因此字符串支持 '\n', '\r', '\t'等来表示特殊字符。所以上面代码最好写成： 
sys.path.append('c:\\xxx\\b.py') 
或者sys.path.append('c:/xxxx/b.py') 
这样可以避免因为错误的组成转义字符，而造成无效的搜索目录（sys.path）设置。 




