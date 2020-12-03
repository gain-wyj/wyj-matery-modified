---
title: 基于混沌Logistic加密算法的图片加密与还原
date: 2020-12-01 08:12:04
tags: 
- Logistic加密
- matlab
- 数字图像处理
categories: 
- 数字图像处理
---

# 摘要

> 一种基于混沌Logistic加密算法的图片加密与还原的方法，并利用Lena图和Baboon图来验证这种加密算法的加密效果。为了能够体现该算法在图片信息加密的效果，本文还采用了普通行列置乱加密算法和像素点的RGB的值的缩放算法这两种算法对相同的图片的图片进行处理，利用matlab通过显示加密过后的图片以及直方图分析可以很直观的发现混沌Logistic加密算法对图片信息加密的效果更好，并且很好地隐藏了原始图像的统计特性，能够有效地抵御基于图像像素值的统计攻击，达到了图像加密的效果。

# 混沌Logistic映射的理论

## 混沌的基本概念

1975年，美国数学家约克和美籍华人李天岩发表了《周期3意味着混沌》的文章，首次提出了“混沌”—词，阐述了混沌的数学定义，对混沌学的发展具有重大意义。自此以后，混沌研究开始蓬勃发展。
混沌是指在确定性动力学系统中，由于对初值敏感而表现出的类似随机的、不可预测的运动。混沌是确定的非线性系统中出现的内在随机性现象，其变化并非随机确貌似随机。

## Logistic映射方程

Logistic映射是一个典型的非线性的迭代方程，如式所示：
                               ![在这里插入图片描述](https://img-blog.csdnimg.cn/20200322115202600.png)
称为Logistic映射的控制参数，对任意的k有![在这里插入图片描述](https://img-blog.csdnimg.cn/20200322115314240.png)，其中k为迭代时间步。==Logistic映射的动态行为与控制参数u密切相关==，对于不同的u值系统将呈现不同的特性（即当k趋于无穷大，xk的变化情况）。其中==Logistic映射有两个主要的参数，一个是初值x0，一个是系统参数μ==，研究表明，==当 0<μ<=3.5699456时，Logistic呈现出周期性；而当映射方程满足0<x0<1和3.5699456<μ<=4这两个条件时，Logistic映射处于混沌状态==，即一种无序的、不可预测的、混乱的、摸不到头、摸不到尾的状态。对给定的初始值x0，生成的序列是非周期性、非收敛以及对初始条件敏感的。

> 有界性
混沌是有界的，它的运动轨线始终局限于一个确定的区域，这个区域称为混沌吸引域。由图 所示，无论控制参数μ怎么变，迭代值xn始终在(0,1)之间。

![**不同控制参数μ下的Logistic分岔图**](https://img-blog.csdnimg.cn/20200322120312766.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3l3c3lkd3Nibg==,size_16,color_FFFFFF,t_70)

# 混沌Logistic映射与其他加密算法介绍

## 普通行列置乱加密算法

### 普通置乱加密算法的流程

将读入的水印图片，先获取图片的大小，得到原始图片矩阵，首先随机打乱各行，输出打乱后的矩阵，再将这个矩阵随机打乱各列，最后图像成功加密，显示加密图像。算法流程框图如图：

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200322120508960.png)

### 算法分析

```
s = size(handles.img);
r = randsample(s(1), s(1)); 
RGBS = handles.img(r, :, :);
c = randsample(s(2), s(2)); 
RGBSS = RGBS(:, c, :);
axes(handles.axes2);        
imshow(RGBSS); 
title('普通置乱加密图像');
figure(2);
hist_im=histogram(RGBSS); %加密后直方图
title('普通置乱加密直方图');
```

## 像素点的RGB值缩放加密

### 像素点的RGB值缩放加密算法的流程

首先读入原始图片，通过size获取水印图片的大小矩阵，接着获取图片各R、G、B的值，然后将获取到的RGB值分别扩大20倍并将值赋给r，最后再将r与将水印图片转换成double类型的矩阵进行点乘运算实现图像的成功加密。算法流程框图如图：

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200322120755105.png)

### 算法分析

```
s = size(handles.img);
r = rand(s(1), s(2), s(3)) * 20;
RGBD = im2double(handles.img);
RGB_jiami = RGBD .* r;
axes(handles.axes2);      
imshow(RGB_jiami); 
title('像素点的RGB值缩放加密图像');
figure(3);
hist_im=histogram(RGB_jiami); 
title('像素点的RGB值缩放加密直方图');
```

## 混沌Logistic映射加密算法

### 混沌Logistic映射加密算法模型

读入待处理的原始图片，通过加密密钥进入混沌序列，通过混沌系统设计加密算法，实现加密目的；再输入解密密钥，把加密过程逆向运算即可得到解密图像。系统参数u和初值x0设置成密钥。混沌Logistic映射加密算法模型如图：

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200322121024286.png)

当迭代n次后，我们就得到了x1、x2、…，xn这n个值，这就是一个混沌序列，是一维的，称作序列A，也就是我们想要得到的序列，在MATLAB中，可以看出xi（i=1,2,…,n）的取值是在(0,1)之间的，就像图像灰度值是在(0,255)之间一样。那么我们把这个一维序列归一化到(0,255)之间得到序列B。异或过程如图：

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200322121130568.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3l3c3lkd3Nibg==,size_16,color_FFFFFF,t_70)

### 算法分析
```  
u=4;   
for i=1:500 
    x=u*x*(1-x); 
end 
fprintf('x(k+1)=%d\n',x); 
A=zeros(1,M*N); 
A(1)=x;   
for i=1:M*N-1 
    A(i+1)=u*A(i)*(1-A(i)); 
end
B=uint8(255*A); %
Imgn=reshape(B,M,N);   
C=zeros(M,N); 
for x=1:M 
    for y=1:N 
        C(x,y)=handles.img(x,y); 
    end
end
C; 
D=uint8(C); 
Rod=bitxor(D,Imgn); 
Rod; 
rod=reshape(Rod,M,N/3,3); 
```

# 验证与性能分析
## Matlab GUI操作界面
使用的是**MATLAB GUI可视化仿真平台**。它是采用图形方式显示的计算机操作用户界面，是MATLAB用户可视化交互式的工具，运用GUI生成的操作界面用户可以不用浏览繁冗的代码而进行操作。如图是设计的**GUI操作界面**
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200322121557542.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3l3c3lkd3Nibg==,size_16,color_FFFFFF,t_70)
***看到这个界面是不是很不错，对的。。。你没看错MATLAB GUI确实是这么厉害。。。。。。。***
## 普通行列置乱加密实现
Lena原图像、加密图像、解密图像
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200322121749624.png)
Baboon原图像、加密图像、解密图像
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200322121818930.png)
## 像素点的RGB值的缩放加密实现
Lena原图像、加密图像、解密图像
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200322121940411.png)
Baboon原图像、加密图像、解密图像
![在这里插入图片描述](https://img-blog.csdnimg.cn/2020032212200941.png)
## 混沌Logistic映射加密实现
Lena原图像、加密图像、解密图像
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200322122101398.png)
Baboon原图像、加密图像、解密图像
![在这里插入图片描述](https://img-blog.csdnimg.cn/2020032212213175.png)
## 直方图性能分析

> 这里就只对lena图进行直方图分析，Baboon图大致和lena图一样。


Lena图的普通置乱与混沌Logistic加密的直方图
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200322122315134.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3l3c3lkd3Nibg==,size_16,color_FFFFFF,t_70)
普通行列置乱图像的直方图与原始图像的直方图相同，且像素点的分布都不均匀，而混沌Logistic加密图像的直方图的像素点分布相对均匀，很好地隐藏了原始图像的统计特性，达到了图像加密的效果。

Lena图的像素点的RGB缩放与混沌Logistic加密的直方图
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200322125400671.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3l3c3lkd3Nibg==,size_16,color_FFFFFF,t_70)
由直方图可知：像素点的RGB缩放图像的直方图与原始图像的直方图不相同且像素点的分布都不均匀，而混沌Logistic加密图像的直方图的像素点分布相对均匀，很好地隐藏了原始图像的统计特性。


完整代码以上传至我的github：[完整代码](https://github.com/gain-wyj/-Logistic-)

> ***你的三连击是我的荣幸！！！！！***

