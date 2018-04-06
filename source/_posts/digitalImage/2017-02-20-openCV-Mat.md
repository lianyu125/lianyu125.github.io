---
title: openCV使用之Mat类
date: 2017-02-20 8:10:09
category: openCV
tags: 数字图像处理
keywords: 数字图像处理,openCV,Mat
description: 早期的 OpenCV 中,使用 IplImage 和 CvMat 数据结构来表示图像。IplImage 和 CvMat 都是 C 语言的结构。使用这两个结构的问题是内存需要手动管理,开 发者必须清楚的知道何时需要申请内存,何时需要释放内存。这个开发者带来了 一定的负担,开发者应该将更多精力用于算法设计,因此在新版本的 OpenCV 中 引入了 Mat 类。
---
新加入的 Mat 类能够自动管理内存。使用 Mat 类,你不再需要花费大量精 力在内存管理上。而且你的代码会变得很简洁,代码行数会变少。但 C++接口唯 一的不足是当前一些嵌入式开发系统可能只支持 C 语言,如果你的开发平台支持 C++,完全没有必要再用 IplImage 和 CvMat。在新版本的 OpenCV 中,开发者依 然可以使用 IplImage 和 CvMat,但是一些新增加的函数只提供了 Mat 接口。本书 中的例程也都将采用新的 Mat 类,不再介绍 IplImage 和 CvMat。Mat 类的定义如下所示,关键的属性如下方代码所示:

```c/c++
class CV_EXPORTS Mat   {   public:    //一系列函数 ...    /* flag参数中包含许多关于矩阵的信息,如: 
           -Mat 的标识           -数据是否连续 
           -深度 
           -通道数目   */    int flags; 
    int dims; //矩阵的维数,取值应该大于或等于 2
int rows, cols;//矩阵的行数和列数,如果矩阵超过 2 维,这两个变量的值都为-1 
uchar* data;//指向数据的指针      
int* refcount;//指向引用计数的指针,如果数据是由用户分配的,则为 NULL 
     //其他成员变量和成员函数      ... 
};
```
## 创建Mat类
Mat 是一个非常优秀的图像类,它同时也是一个通用的矩阵类,可以用来创建和操作多维矩阵。有多种方法创建一个 Mat 对象。
### 构造函数法
Mat 类提供了一系列构造函数,可以方便的根据需要创建 Mat 对象。下面是 一个使用构造函数创建对象的例子。

```c/c++
Mat M(3,2, CV_8UC3, Scalar(0,0,255));cout << "M = " << endl << " " << M << endl;
```
第一行代码创建一个行数(高度)为 3,列数(宽度)为 2 的图像,图像元 素是 8 位无符号整数类型,且有三个通道。图像的所有像素值被初始化为(0, 0, 255)。由于 OpenCV 中默认的颜色顺序为 BGR,因此这是一个全红色的图像。
第二行代码是输出 Mat 类的实例 M 的所有像素值。Mat 重定义了<<操作符, 使用这个操作符,可以方便地输出所有像素值,而不需要使用 for 循环逐个像素输出。
该段代码输出的内容为
```
M= [0,0,255,0,0,255;
    0,0,255,0,0,255;
    0,0,255,0,0,255]
```
常用的构造函数有
```c/c++
Mat::Mat()  //无参数构造方法
Mat::Mat(int rows, int cols, int type) //创建行数为 rows,列数为 col,类型为 type 的图像
Mat::Mat(Size size, int type)  //创建大小为 size,类型为 type 的图像
Mat::Mat(int rows, int cols, int type, const Scalar& s)//创建行数为 rows,列数为 col,类型为 type 的图像,并将所有元素初始 化为值 s
Mat::Mat(Size size, int type, const Scalar& s)//创建大小为 size,类型为 type 的图像,并将所有元素初始化为值 s
Mat::Mat(const Mat&m)//将 m 赋值给新创建的对象,此处不会对图像数据进行复制,m 和新对象 共用图像数据
Mat::Mat(int rows, int cols, int type, void* data, size_t step=AUTO_STEP)//创建行数为 rows,列数为 col,类型为 type 的图像,此构造函数不创建 图像数据所需内存,而是直接使用 data 所指内存,图像的行步长由 step 指定
Mat::Mat(Size size, int type, void* data, size_t step=AUTO_STEP)//创建大小为 size,类型为 type 的图像,此构造函数不创建图像数据所需 内存,而是直接使用 data 所指内存,图像的行步长由 step 指定。
Mat::Mat(const Mat& m, const Range& rowRange, const Range& colRange) //创建的新图像为 m 的一部分,具体的范围由 rowRange 和 colRange 指 定,此构造函数也不进行图像数据的复制操作,新图像与 m 共用图像数据
Mat::Mat(const Mat& m, const Rect& roi)//创建的新图像为 m 的一部分,具体的范围 roi 指定,此构造函数也不进 行图像数据的复制操作,新图像与 m 共用图像数据。
```
这些构造函数中,很多都涉及到类型 type。type 可以是 CV_8UC1,CV_16SC1,..., CV_64FC4 等。里面的 8U 表示 8 位无符号整数,16S 表示 16 位有符号整数,64F 表示 64 位浮点数(即 double 类型);C 后面的数表示通道数,例如 C1 表示一个 通道的图像,C4 表示 4 个通道的图像,以此类推。
如果你需要更多的通道数,需要用宏 CV_8UC(n),例如:
```c/c++
Mat M(3,2, CV_8UC(5));//创建行数为3,列数为2,通道数为5的图像
```
### create()函数创建对象方法
除了在构造函数中可以创建图像,也可以使用 Mat类的 create()函数创建图 像。如果 create()函数指定的参数与图像之前的参数相同,则不进行实质的内存 申请操作;如果参数不同,则减少原始数据内存的索引,并重新申请内存。使用方法如下面例程所示:
```c/c++
Mat M(2,2, CV_8UC3);//构造函数创建图像 
M.create(3,2, CV_8UC2);//释放内存重新创建图像
```
【注】 使用 create()函数无法设置图像像素的初始值。
### Matlab风格的创建对象方法
OpenCV 2 中提供了 Matlab 风格的函数,如 zeros(),ones()和 eyes()。这种方 法使得代码非常简洁,使用起来也非常方便。使用这些函数需要指定图像的大小 和类型,使用方法如下:
```c/c++
Mat Z = Mat::zeros(2,3, CV_8UC1);cout << "Z = " << endl << " " << Z << endl;Mat O = Mat::ones(2, 3, CV_32F);cout << "O = " << endl << " " << O << endl;Mat E = Mat::eye(2, 3, CV_64F);cout << "E = " << endl << " " << E << endl;
```
该代码中,有些 type 参数如 CV_32F 未注明通道数目,这种情况下它表示单 通道。上面代码的输出结果如图 所示。
```
Z=[0,0,0;
   0,0,0]
O=[1,1,1;
   1,1,1]
E=[1,0,0;
   0,1,0]
```


 



