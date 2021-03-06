---
layout: post
title:  "MATLAB 实现伪彩色增强"
---

* content
{:toc}

## 伪彩色增强

最近在上数值图像处理的课，做了个作业，用 MATLAB 实现伪彩色增强。虽然用 MATLAB 实现起来很简单，但这东西还是挺有意思的。

> 伪彩色增强是把黑白图像的各个不同灰度级按照线性或非线性的映射函数变换成不同的彩色，得到一幅彩色图像的技术。

对图像进行伪彩色增强的主要原因是人对图像灰度的分辨能力比较低，只能分辨出几十级，而对色彩的辨别能力却非常强，可以分辨出 上千种颜色，为了更有效地提取图形信息，使原图像细节更易辨认，目标更容易识别。

<!-- more --> <!-- 摘要预览与正文的分隔符 -->

伪彩色增强的方法主要有密度分割法、灰度级——彩色变换和频率域伪彩色增强三种。

（1）密度分割法

密度分割法是把灰度图像的灰度级从 0（黑）到 M0（白）分成 N 个区间 Li(i=1,2,…,N)，给每个区间 Li 指定一种彩色 Ci，这样便可以把一幅灰度图像变成一幅伪彩色图像。该方法比较简单、直观。缺点是变换出的彩色数目有限。

（2）空间域灰度级——彩色变换

根据色度学原理，将原图像 f(x,y) 的灰度范围分段，经过红、绿、蓝三种不同变换 TR、TG 和 TB，变成三基色分量 IR(x,y)、IG(x,y)、IB(x,y)，然后用它们分别去控制彩色显示器的红、绿、蓝电子枪，便可以在彩色显示器的屏幕上合成一幅彩色图像。

（3）频率域伪彩色增强

把灰度图像经傅立叶变换到频率域，在频率域内用三个不同传递特性的滤波器分离成三个独立分量；然后对它们进行逆傅立叶变换，便得到三幅代表不同频率分量的单色图像，接着对这三幅图像作进一步的处理（如直方图均衡化）。最后将它们作为三基色分量分别加到彩色显示器的红、绿、蓝显示通道，得到一幅彩色图像。

---

## MATLAB 实现

我所采用的是最简单粗暴的密度分割法（*看代码就知道很简单粗暴*）。

密度分割法的主要原理，可见下图：

![](https://raw.githubusercontent.com/LiangSongpeng/liangsongpeng.github.io/master/_images/2019-12-21-DensitySegmentation-2019-12-21-MATLABPseudo-colorEnhancement.png)

代码实现如下：

```matlab
clc; clear; close all;

tic;

[f, p] = uigetfile('*.*', '选择图片');
I = imread(strcat(p, f));

figure;
imshow(I);
title('原图');

G = rgb2gray(I);
X = grayslice(G, 5000);
figure;
imshow(X, jet(5000));
title('伪彩色增强效果图');

toc;
```

输入的原图像：

![](https://raw.githubusercontent.com/LiangSongpeng/liangsongpeng.github.io/master/_images/2019-12-21-Input-2019-12-21-MATLABPseudo-colorEnhancement.png)

输出的结果图像：

![](https://raw.githubusercontent.com/LiangSongpeng/liangsongpeng.github.io/master/_images/2019-12-21-Output1-2019-12-21-MATLABPseudo-colorEnhancement.png)

![](https://raw.githubusercontent.com/LiangSongpeng/liangsongpeng.github.io/master/_images/2019-12-21-Output2-2019-12-21-MATLABPseudo-colorEnhancement.png)

实际上就是，先选择图像，显示图像，伪彩色增强一下，再显示一下处理后的图像。

而 MATLAB 中有很多自带的函数，直接调用就行，编程上能省很多麻烦。

MATLAB 中有个叫颜色映像的数据结构来代表颜色，颜色映像定义了一个有三行和若干列的矩阵，利用0到1之间的数，矩阵的每一行都代表了一种色彩，每一行的数字都指定了一个 RGB 值，即红、绿、蓝三种颜色的强度，形成一种特定的颜色。

而 MATLAB 中还有 10 个函数产生预定义的颜色映像，具体如下：

| 预定义的颜色映像名 | 预定义的颜色映像含义                            |
| ------------------ | ----------------------------------------------- |
| hsv                | 色彩饱和值（以红色开始和结束）                  |
| hot                | 从黑到橘红和黄到白                              |
| cool               | 青蓝和洋红的色度                                |
| pink               | 粉红的彩色度                                    |
| gray               | 线性灰度                                        |
| bone               | 带一点蓝色的灰度                                |
| jet                | hsv 的一种变形（以蓝色开始和结束）              |
| copper             | 线性铜色度                                      |
| prim               | 三棱镜。交替为红色， 橘黄色，黄色，蓝色和天蓝色 |
| flag               | 交替为红色，白色，蓝色和黑色                    |

上述程序中所用便是 jet，即 jet(5000)。

若使用 hot(m)，即表示产生一个 m x 3 的矩阵（即有 m 中 RGB 组合值，也就是有 m 种颜色），它包含的 RGB 颜色的值从黑经过红、橘红和黄到白。

而程序中所涉及到的 grayslice() 函数功能则是通过设定阈值将灰度图像转换成索引色图像。

还有一些图像之间转换的常用函数，具体如下：

| 函数        | 功能                                                 |
| ----------- | ---------------------------------------------------- |
| gray2ind()  | 将灰度图像转换成索引图像                             |
| grayslice() | 通过设定阈值将灰度图像转换成索引色图像               |
| im2bw()     | 通过设定亮度阈值将真彩色、索引色、灰度图转换成二值图 |
| ind2gray()  | 将索引色图像转换成灰度图像                           |
| ind2rgb()   | 将索引色图像转换成真彩色图像                         |
| mat2gray()  | 将一个数据矩阵转换成一副灰度图                       |
| rgb2gray()  | 将一副真彩色图像转换成灰度图像                       |
| rgb2ind()   | 将真彩色图像转换成索引色图像                         |

（*程序中所用到 tic 和 toc 其实和这段程序要实现的功能没什么关系，主要是用来记录程序运行的时间，毕竟程序运行的效率也很重要啊*）
