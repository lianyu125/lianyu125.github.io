
---
title: Objective-C运行时编程指南之消息转发
category: iOS
tags: iOS
keywords: iOS
---
 通常，给一个对象发送它不能处理的消息会得到出错提示，然而，Objective-C运行时系统在抛出错误之前，会给消息接收对象发送一条特别的消息来通知该对象。 
 <!--more-->
## 消息转发 
如果一个对象收到一条无法处理的消息，运行时系统会在抛出错误前，给该对象发送一条forwardInvocation:消息，该消息的唯一参数是个NSInvocation类型的对象——该对象封装了原始的消息和消息的参数。 您可以实现forwardInvocation:方法来对不能处理的消息做一些默认的处理，也可以以其它的某种方式来避免错误被抛出。如forwardInvocation:的名字所示，它通常用来将消息转发给其它的对象。 关于消息转发的作用，您可以考虑如下情景：假设，您需要设计一个能够响应negotiate消息的对象，并且能够包括其它类型的对象对消息的响应。 通过在negotiate方法的实现中将negotiate消息转发给其它的对象来很容易的达到这一目的。 更进一步，假设您希望您的对象和另外一个类的对象对negotiate的消息的响应完全一致。一种可能的方式就是让您的类继承其它类的方法实现。 然后，有时候这种方式不可行，因为您的类和其它类可能需要在不同的继承体系中响应negotiate消息。 虽然您的类无法继承其它类的negotiate方法，您仍然可以提供一个方法实现，这个方法实现只是简单的将negotiate消息转发给其他类的对象，就好像从其它类那儿“借”来的现一样。如下所示： 
```objc
- negotiate {
     if ( [someOtherObject respondsTo:@selector(negotiate)] )
        return [someOtherObject negotiate];
     return self;
} 
```
这种方式显得有欠灵活，特别是有很多消息您都希望传递给其它对象时，您必须为每一种消息提供方法实现。此外，这种方式不能处理未知的消息。当您写下代码时，所有您需要转发的消息的集合也必须确定。然而，实际上，这个集合会随着运行时事件的发生，新方法或者新类的定义而变化。
forwardInvocation:消息给这个问题提供了一个更特别的，动态的解决方案：当一个对象由于没有相应的方法实现而无法响应某消息时，运行时系统将通过forwardInvocation:消息通知该对象。每个对象都从NSObject类中继承了forwardInvocation:方法。然而，NSObject中的方法实现只是简单地调用了doesNotRecognizeSelector:。通过实现您自己的forwardInvocation:方法，您可以在该方法实现中将消息转发给其它对象。 
 要转发消息给其它对象，forwardInvocation:方法所必须做的有：
*   决定将消息转发给谁
*   并且将消息和原来的参数一块转发出去 
  消息可以通过invokeWithTarget:方法来转发:
```objc
- (void)forwardInvocation:(NSInvocation *)anInvocation {
     if ([someOtherObject respondsToSelector:[anInvocation selector]]) 
           [anInvocation invokeWithTarget:someOtherObject];
     else
           [super forwardInvocation:anInvocation];
} 
```
转发消息后的返回值将返回给原来的消息发送者。您可以将返回任何类型的返回值，包括: id，结构体，浮点数等。
forwardInvocation:方法就像一个不能识别的消息的分发中心，将这些消息转发给不同接收对象。或者它也可以象一个运输站将所有的消息都发送给同一个接收对象。它可以将一个消息翻译成另外一个消息，或者简单的"吃掉“某些消息，因此没有响应也没有错误。forwardInvocation:方法也可以对不同的消息提供同样的响应，这一切都取决于方法的具体实现。该方法所提供是将不同的对象链接到消息链的能力。

**注意： forwardInvocation:方法只有在消息接收对象中无法正常响应消息时才会被调用。 所以，如果您希望您的对象将negotiate消息转发给其它对象，您的对象不能有negotiate方法。否则，forwardInvocation:将不可能会被调用。 更多消息转发的信息，参考Foundation框架参考库中NSInvocation类的文档。** 
## 消息转发和多重继承 
消息转发很象继承，并且可以用来在Objective-C程序中模拟多重继承。如图 5-1所示， 一个对象通过转发来响应消息，看起来就象该对象从别的类那借来了或者”继承“了方法实现一样。  
![](http://okjl482qy.bkt.clouddn.com/message_inherit.png)
 在上图中，Warrior类的一个对象实例将negotiate消息转发给Diplomat类的一个实例。看起来，Warrior类似乎和Diplomat类一样， 响应negotiate消息，并且行为和Diplomat一样（尽管实际上是Diplomat类响应了该消息）。 
  转发消息的对象看起来有两个继承体系分支——自己的和响应消息的对象的。在上面的例子中，Warrior看起来同时继承自Diplomat和自己的父类。
  消息转发提供了多重继承的很多特性。然而，两者有很大的不同：多重继承是将不同的行为封装到单个的对象中，有可能导致庞大的，复杂的对象。而消息转发是将问题分解到更小的对象中，但是又以一种对消息发送对象来说完全透明的方式将这些对象联系起来。
## 消息代理对象
消息转发不仅和继承很象，它也使得以一个轻量级的对象（消息代理对象）代表更多的对象进行消息处理成为可能。 Objective-C 2.0程序设计语言中“远程消息”一节中的代理类就是这样一个代理对象。代理类负责将消息转发给远程消息接收对象的管理细节，保证消息参数的传输等等。但是消息类没有进一步的复制远程对象的功能，它只是将远程对象映射到一个本地地址上，从而能够接收其它应用程序的消息。 同时也存在着其它类型的消息代理对象。例如，假设您有个对象需要操作大量的数据——它可能需要创建一个复杂的图片或者需要从磁盘上读一个文件的内容。创建一个这样的对象是很费时的，您可能希望能推迟它的创建时间——直到它真正需要时，或者系统资源空闲时。同时，您又希望至少有一个预留的对象和程序中其它对象交互。 在这种情况下，你可以为该对象创建一个轻量的代理对象。该代理对象可以有一些自己的功能，例如响应数据查询消息，但是它主要的功能是代表某个对象，当时间到来时，将消息转发给被代表的对象。当代理对象的forwardInvocation:方法收到需要转发给被代表的对象的消息时，代理对象会保证所代表的对象已经存在，否则就创建它。所有发到被代表的对象的消息都要经过代理对象，对程序来说，代理对象和被代表的对象是一样的。
## 消息转发和类继承 
尽管消息转发很“象”继承，但它不是继承。例如在NSObject类中，方法respondsToSelector:和isKindOfClass:只会出现在继承链中，而不是消息转发链中。例如，如果向一个Warrior类的对象询问它能否响应negotiate消息， 
```objc
 if ( [aWarrior respondsToSelector:@selector(negotiate)] )     ...
``` 
返回值是NO，尽管该对象能够接收和响应negotiate。（见图 5-1。） 大部分情况下，NO是正确的响应。但不是所有时候都是的。例如，如果您使用消息转发来创建一个代理对象以扩展某个类的能力，这儿的消息转发必须和继承一样，尽可能的对用户透明。如果您希望您的代理对象看起来就象是继承自它代表的对象一样，您需要重新实现respondsToSelector:和isKindOfClass:方法： 
```objc
 - (BOOL)respondsToSelector:(SEL)aSelector {
      if ( [super respondsToSelector:aSelector] )
            return YES;
      else { 
        /* Here, test whether the aSelector message can     *          * be forwarded to another object and whether that  *          * object can respond to it. Return YES if it can.  */ 
      }
      return NO;
 } 
```
除了respondsToSelector:和isKindOfClass:外，instancesRespondToSelector:方法也必须重新实现。如果您使用的是协议类，需要重新实现的还有conformsToProtocol:方法。类似地，如果对象需要转发远程消息，则methodSignatureForSelector:方法必须能够返回实际响应消息的方法的描述。例如，如果对象需要将消息转发给它所代表的对象，您可能需要如下的methodSignatureForSelector:实现： 
```objc
- (NSMethodSignature*)methodSignatureForSelector:(SEL)selector {     
     NSMethodSignature* signature = [super methodSignatureForSelector:selector];
     if (!signature) {
       signature = [surrogate methodSignatureForSelector:selector];
     }
     return signature;
} 
```
 您也可以将消息转发的部分放在一段私有的代码里，然后从forwardInvocation:调用它。 
  **注意：  消息转发是一个比较高级的技术，仅适用于没有其它更好的解决办法的情况。它并不是用来代替继承的。如果您必须使用该技术，请确定您已经完全理解了转发消息的类和接收转发消息的类的行为。 本节中涉及的方法在Foundation框架参考库中的NSObject类的文档中都有描述。关于invokeWithTarget:的具体信息，请参考Foundation框架参考库中NSInvocation类的文档。 **


