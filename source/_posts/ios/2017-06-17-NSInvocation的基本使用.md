---
title:  NSInvocation的基本使用
date: 2017-06-17 8:10:09
tags: iOS
categories: iOS 
---
## 前言
NSInvocation是一个消息调用类，它包含了所有OC消息的成分：target、selector、参数以及返回值。NSInvocation可以将消息转换成一个对象，消息的每一个参数能够直接设定，而且当一个NSInvocation对象调度时返回值是可以自己设定的。一个NSInvocation对象能够重复的调度不同的目标(target)，而且它的selector也能够设置为另外一个方法签名。NSInvocation遵守NSCoding协议，但是仅支持NSPortCoder编码，不支持归档型操作。
<!--more-->
## NSInvocation的接口
```objc
@interface NSInvocation : NSObject {
@private
    __strong void *_frame;
    __strong void *_retdata;
    id _signature;
    id _container;
    uint8_t _retainedArgs;
    uint8_t _reserved[15];
}

// 通过NSMethodSignature对象创建NSInvocation对象，NSMethodSignature为方法签名类
+ (NSInvocation *)invocationWithMethodSignature:(NSMethodSignature *)sig;

// 获取NSMethodSignature对象
@property (readonly, retain) NSMethodSignature *methodSignature;

// 保留参数，它会将传入的所有参数以及target都retain一遍
- (void)retainArguments;

// 判断参数是否还存在
// 调用retainArguments之前，值为NO，调用之后值为YES
@property (readonly) BOOL argumentsRetained;

// 设置消息调用者，注意：target最好不要是局部变量
@property (nullable, assign) id target;

// 设置要调用的消息
@property SEL selector;

// 获取消息返回值
- (void)getReturnValue:(void *)retLoc;

// 设置消息返回值
- (void)setReturnValue:(void *)retLoc;

// 获取消息参数
- (void)getArgument:(void *)argumentLocation atIndex:(NSInteger)idx;

// 设置消息参数
- (void)setArgument:(void *)argumentLocation atIndex:(NSInteger)idx;

// 发送消息，即执行方法
- (void)invoke;

// target发送消息，即target执行方法
- (void)invokeWithTarget:(id)target;

@end
```
## NSMethodSignature的接口

```objc
@interface NSMethodSignature : NSObject {
@private
    void *_private;
    void *_reserved[6];
}

// 通过Objective-C类型编码(Objective-C Type Encodings)创建一个NSMethodSignature对象
+ (nullable NSMethodSignature *)signatureWithObjCTypes:(const char *)types;

// 方法参数个数
@property (readonly) NSUInteger numberOfArguments;

// 获取参数类型
- (const char *)getArgumentTypeAtIndex:(NSUInteger)idx NS_RETURNS_INNER_POINTER;

// 获取方法的长度
@property (readonly) NSUInteger frameLength;

// 是否是单向
- (BOOL)isOneway;

// 获取方法返回值类型
@property (readonly) const char *methodReturnType NS_RETURNS_INNER_POINTER;

// 获取方法返回值的长度
@property (readonly) NSUInteger methodReturnLength;

@end
```
##  NSInvocation的使用方法
 **使用步骤**

* 1. 根据方法创建签名对象(NSMethodSignature对象)
* 2. 根据签名对象创建调用对象(NSInvocation对象)
* 3. 设置调用对象(NSInvocation对象)的相关信息
* 4. 调用方法
* 5. 获取方法返回值

 **调用无参无返回值的方法**
```objc
- (void)invocation1 {
    // 1. 根据方法创建签名对象sig
    NSMethodSignature *sig = [[self class] instanceMethodSignatureForSelector:@selector(method)];
    // 2. 根据签名对象创建调用对象invocation
    NSInvocation *invocation = [NSInvocation invocationWithMethodSignature:sig];
    // 3. 设置调用对象的相关信息
    // 注意：target不要设置成局部变量
    invocation.target = self;
    invocation.selector = @selector(method);
    //4. 调用方法
    [invocation invoke];
}

- (void)method {
    NSLog(@"无参无返回值");
}
```
**调用有参无返回值的方法**
```objc
- (void)invocation2 {
    // 1. 根据方法创建签名对象sig
    NSMethodSignature *sig = [[self class] instanceMethodSignatureForSelector:@selector(methodWithArg1:arg2:)];
    // 2. 根据签名对象创建调用对象invocation
    NSInvocation *invocation =[NSInvocation invocationWithMethodSignature:sig];
    // 3. 设置调用对象的相关信息
    invocation.target = self;
    invocation.selector = @selector(methodWithArg1:arg2:);
    NSString *name = @"SJM";
    int age = 18;
    // 参数必须从第2个索引开始，因为前两个已经被target和selector使用
    [invocation setArgument:&name atIndex:2];
    [invocation setArgument:&age atIndex:3];
    // 4. 调用方法
    [invocation invoke];
}

- (void)methodWithArg1:(NSString *)arg1 arg2:(int)arg2 {
    NSLog(@"我叫%@，今年%d岁。", arg1, arg2);
}
```
**调用有参有返回值的方法** 
```objc
- (void)invocation3 {
    // 1. 根据方法创建签名对象sig
    NSMethodSignature *sig = [[self class] instanceMethodSignatureForSelector:@selector(methodWithArg1:arg2:arg3:)];
    // 2. 根据签名对象创建调用对象invocation
    NSInvocation *invocation = [NSInvocation invocationWithMethodSignature:sig];
    // 3. 设置调用对象的相关信息
    invocation.target = self;
    invocation.selector = @selector(methodWithArg1:arg2:arg3:);
    int value1 = 111;
    int value2 = 999;
    int value3 = 666;
    [invocation setArgument:&value1 atIndex:2];
    [invocation setArgument:&value2 atIndex:3];
    [invocation setArgument:&value3 atIndex:4];
    // 4. 调用方法
    [invocation invoke];
    // 5. 获取方法返回值
    NSNumber *num = nil;
    [invocation getReturnValue:&num];
    NSLog(@"最大数为：%@",num);
}

- (NSNumber *)methodWithArg1:(int)arg1 arg2:(int)arg2 arg3:(int)arg3 {
    int max1 = MAX(arg1, arg2);
    int max2 = MAX(arg2, arg3);
    // 返回最大数
    return @(MAX(max1, max2));
}
```
## NSInvocation的其他属性与方法
```objc
- (void)invocation4 {


    // 1. 根据方法创建一个签名对象sig
    NSMethodSignature *sig = [[self class] instanceMethodSignatureForSelector:@selector(methodWithArg1:arg2:arg3:)];

    // 2. 根据签名对象创建调用对象invocation
    NSInvocation *invocation = [NSInvocation invocationWithMethodSignature:sig];

    // 3. 设置调用对象的相关信息
    invocation.target = self;
    invocation.selector = @selector(methodWithArg1:arg2:arg3:);
    int value1 = 111;
    int value2 = 999;
    int value3 = 666;
    [invocation setArgument:&value1 atIndex:2];
    [invocation setArgument:&value2 atIndex:3];
    [invocation setArgument:&value3 atIndex:4];

    // 4. 调用方法，也可以使用invokeWithTarget:方法指定调用者
    [invocation invoke];


    // 5. 获取方法返回值
    NSNumber *num = nil;
    [invocation getReturnValue:&num];
    NSLog(@"最大数为：%@",num);

/* -------------------------------分隔线---------------------------------- */

    // NSInvocation的其他属性

    // argumentsRetained属性是判断参数是否还存在
    // 调用retainArguments之前，值为NO，调用之后值为YES
    NSLog(@"%@参数保留", (invocation.argumentsRetained ? @"有" : @"没有"));
    [invocation retainArguments];
    NSLog(@"%@参数保留", (invocation.argumentsRetained ? @"有" : @"没有"));

    // 获取参数值
    if (invocation.argumentsRetained) {
        int arg;
        [invocation getArgument:&arg atIndex:4];
        NSLog(@"Argument ---- %d",arg);
    }

    // 设置方法返回值
    NSNumber *value = @987;
    [invocation setReturnValue:&value];
    [invocation getReturnValue:&value];
    NSLog(@"ReturnValue ---- %@",value);


    // 关于NSMethodSignature对象
    NSLog(@"-------------关于NSMethodSignature对象---------------");
    [self methodSignature:invocation];

}

- (NSNumber *)methodWithArg1:(int)arg1 arg2:(int)arg2 arg3:(int)arg3 {

    int max1 = MAX(arg1, arg2);
    int max2 = MAX(arg2, arg3);

    // 返回最大数
    return @(MAX(max1, max2));
}

// NSMethodSignature的使用
- (void)methodSignature:(NSInvocation *)invocation {

    // 获取方法签名对象
    NSMethodSignature *signature = invocation.methodSignature;
    // 获取方法所占字节数
    NSUInteger frameLength = signature.frameLength;
    NSLog(@"frameLength ---- %ld",frameLength);
    // 获取方法返回值所占字节数
    // 这里只对数值型的类型有效，OC类型打印都是8字节
    NSUInteger returnLength = signature.methodReturnLength;
    NSLog(@"returnLength ---- %ld",returnLength);

    // 判断方法是否是单向
    NSString *oneWay =  [signature isOneway] ? @"是" : @"不是";
    NSLog(@"方法%@单向", oneWay);

    // 获取参数个数
    NSInteger count = signature.numberOfArguments;
    // 打印所有参数类型，
    // 这里打印的结果是  @ : i i i  它们是Objective-C类型编码
    // @ 表示 NSObject* 或 id 类型
    // : 表示 SEL 类型
    // i 表示 int 类型
    for (int i = 0; i < (int)count; i++) {
        const char *argTybe = [signature getArgumentTypeAtIndex:i];
        NSLog(@"参数类型 %s",argTybe);
    }
    // 获取返回值的类型
    const char *returnType = [signature methodReturnType];
    NSLog(@"返回值的类型 %s",returnType);
}
```



