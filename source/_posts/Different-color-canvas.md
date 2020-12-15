---
title: 教你在python中用不同的方式画不同颜色的画布
top: true
cover: false
toc: true
mathjax: true
date: 2020-12-05 15:57:15
password:
summary: 给大家讲解如何分别用`numpy`的方法，与`numpy与cv2结合`的方法创建空白画布，创建白色画布，与创建彩色画布。在讲解过程中还会介绍`cv2`进行通道分割`cv2.split`与通道合并`cv2.merge`的两个函数的具体使用以及深究numpy的ndarray数据结构的索引与赋值。
tags: 
- python
- numpy
- OpenCV
categories: 
- python
---

# 摘要
在这篇文章中将给大家讲解如何分别用`numpy`的方法，与`numpy与cv2结合`的方法创建空白画布，创建白色画布，与创建彩色画布。在讲解过程中还会介绍`cv2`进行通道分割`cv2.split`与通道合并`cv2.merge`的两个函数的具体使用以及深究numpy的ndarray数据结构的索引与赋值。

# numpy的ndarray数据结构的索引与赋值
在使用画图工具的时候, 第一件事情就是创建一个新的空白画布，我们可以指定画布的大小和颜色。

> **那我们如何使用opencv来创建一个空白的画布(值相同的图片) ?**

**其实image的数据结构上的图片，本质上就是`numpy`里面的`ndarray的对象`，创建一个画布本质上就是创建一个同等规格的`ndarray`。**

创建一个新的特定尺寸的`ndarray`我们可以使用`np.zeors` 函数，我们将图像的高度(height)，图像的宽度(width)，以及图像的通道数`channel` 以`tuple 类型`传入`np.zeros`。**再次声明是tuple类型**。

另外由于不是所有的`numpy`类型的数值，都可以放到opencv中进行图像处理.

数值取值范围在`0-255`， 我们需要指定数据类型为`uint8` unsigned integer 8-bit
```python
np.zeros((height, width, channels), dtype="uint8")
```
举个例子：想创建一个800 x 600 x 3 的图片，一个BGR格式的图像，我们就得这么写：

```python
# 初始化一个空画布 300×300 三通道 背景色为黑色 
canvas_black = np.zeros((600, 800, 3), dtype="uint8")
```
得到的效果如下：

![在这里插入图片描述](https://img-blog.csdnimg.cn/20201006200546404.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3l3c3lkd3Nibg==,size_16,color_FFFFFF,t_70#pic_center)

> 注意： height写在前面
>  **为什么Height写在前面？** 
>  就得知道opencv图像的数据结构是numpy，Image的属性,其实就是numpy的ndarray数据格式的属性。
>  
我们可以直接获取img对象的诸多属性，例如我们打印lena图的属性，具体如下：

![在这里插入图片描述](https://img-blog.csdnimg.cn/20201006195435156.jpg#pic_center)

```python
 # -*- coding: utf-8 -*- 
import numpy as np
import cv2

# 导入一张图像 模式为彩色图片
img = cv2.imread('lena.jpg', cv2.IMREAD_COLOR)

print("================打印图像的属性================")
print("图像对象的类型 {}".format(type(img)))
print(img.shape)
print("图像宽度: {} pixels".format(img.shape[1]))
print("图像高度: {} pixels".format(img.shape[0]))
print("通道: {}".format(img.shape[2]))
print("图像分辨率: {}".format(img.size))
print("数据类型: {}".format(img.dtype))
```

**输出结果**：

```python
================打印图像的属性================
图像对象的类型 <class 'numpy.ndarray'>
(256, 256, 3)
图像宽度: 256 pixels
图像高度: 256 pixels
通道: 3
图像分辨率: 196608
数据类型: uint8
```

有时候我们也可以偷懒，如果我们想创建与另外一个图像尺寸相同的画布的时候，我们可以使用`np.zeros_like`

```python
canvas_black = np.zeros_like(img)
```

# 创建空白画布
**创建空白画布的函数**如下：

```python
def InitCanvas(width, height, color=(255, 255, 255)):
    canvas = np.ones((height, width, 3), dtype="uint8")
    canvas[:] = color
    return canvas
```
调用的时候传入图像的宽度、高度和画布的颜色。例如创建一个800*600 颜色为纯黑色的画布：

```python
canvas = InitCanvas(800, 600, color=(255,255,255))
```
**创建空白画布的完整代码**如下：

```python
'''
初始化画布
'''
import cv2
import numpy as np

def init_canvas(width, height, color=(255, 255, 255)):
    canvas = np.ones((height, width, 3), dtype="uint8")
    canvas[:] = color
    return canvas

canvas = init_canvas(200, 200, color=(125, 40, 255))

cv2.imshow('canvas', canvas)
cv2.waitKey(0)

cv2.destroyAllWindows()
```
**效果展示：**

![在这里插入图片描述](https://img-blog.csdnimg.cn/20201006193553314.png#pic_center)

# 初始化白色的画布
白色的画布， 因为比较简单，而且三个通道的值都相同。

> ps: 其实灰色的图片(GRAY2BGR)，三个通道的值都相同。

那么我们创建一个全都是1的矩阵，然后乘上某个数值，问题是不是就解决了。

我们需要用到`np.ones` 函数

```python
# 初始化一个空画布 300×300 三通道 背景色为白色 
canvas_white = np.ones((300, 300, 3), dtype="uint8")
```

接下来, 需要乘上一个整数，255 (你可以填入0-255的任意值)

```python
canvas_white *= 255
```
这种运算称之为 `全局乘法` 。

**具体代码如下：**

```python
import cv2
import numpy as np
canvas_white = np.ones((300, 300, 3), dtype="uint8")
canvas_white *= 255
cv2.imshow('canvas', canvas_white)
cv2.waitKey(0)
cv2.destroyAllWindows()
```

**创建的白色画布如下**：

![在这里插入图片描述](https://img-blog.csdnimg.cn/20201006200648233.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3l3c3lkd3Nibg==,size_16,color_FFFFFF,t_70#pic_center)

# 初始化彩色的画布
## 利用cv2的内置方法merge与split
我们初始化`BGR`的图片`canvas_white` 之后将原来的图片进行通道分离，之后分别乘上`BGR`三个通道的整数值，然后将三个通道合并在一起，就得到我们想要的彩图纯色背景。

那通道的分离我们需要用到的函数是`cv2.split(img)`.

```python
# 将原来的三个通道抽离出来， 分别乘上各个通道的值
(channel_b, channel_g, channel_r) = cv2.split(canvas)
```

**channel_b 蓝色通道**，**channel_g 绿色通道**，**channel_r 红色通道**，都是二维的ndarray对象。

我们指定一种颜色，例如 `color = (100, 20, 50))`

> 注意：我们这里的**颜色指的BGR格式**
> 
> 也就是
> 
> B -> 100 
> G -> 20 
> R -> 50

接下来我们分别将其乘上对应的值.

```python
# 颜色的值与个通道的全1矩阵相乘
channel_b *= color[0]
channel_g *= color[1]
channel_r *= color[2]
```

接下来我们将三个通道重新合并，需要用到的函数是`cv2.merge`

```python
cv2.merge([channel_b, channel_g, channel_r])
```

> 注意：三个通道的矩阵以`list []` 的方式传入`merge`函数.

综合起来，就是我们的第一个**初始化彩色背景的函数**：

```python
# 初始化一个彩色的画布 - cv2版本
def init_canvas(width, height, color=(255, 255, 255)):
    canvas = np.ones((height, width, 3), dtype="uint8")

    # 将原来的三个通道抽离出来， 分别乘上各个通道的值
    (channel_b, channel_g, channel_r) = cv2.split(canvas)
    # 颜色的值与个通道的全1矩阵相乘
    channel_b *= color[0]
    channel_g *= color[1]
    channel_r *= color[2]

    # cv.merge 合并三个通道的值
    return cv2.merge([channel_b, channel_g, channel_r])
```
**具体实现代码如下：**

```python
'''
初始化画布
'''
import cv2
import numpy as np

# 初始化一个彩色的画布 - cv2版本
def init_canvas(width, height, color=(255, 255, 255)):
    canvas = np.ones((height, width, 3), dtype="uint8")

    # 将原来的三个通道抽离出来， 分别乘上各个通道的值
    (channel_b, channel_g, channel_r) = cv2.split(canvas)
    # 颜色的值与个通道的全1矩阵相乘
    channel_b *= color[0]
    channel_g *= color[1]
    channel_r *= color[2]

    # cv.merge 合并三个通道的值
    return cv2.merge([channel_b, channel_g, channel_r])

canvas = init_canvas(200, 200, color=(125, 100, 255))

cv2.imshow('canvas', canvas)
cv2.waitKey(0)

cv2.destroyAllWindows()
```
**运行效果：**

![在这里插入图片描述](https://img-blog.csdnimg.cn/20201006204105226.png#pic_center)

> 注意：此函数使用 `cv2.split` 非常耗时 所以只有在需要的时候才能做到。 否则用Numpy索引。

## 利用numpy内置的索引
使用numpy原生的方法， 性能会比opencv中的要好。我们直接使用`numpy的ndarray的索引`的方法。

例如 `canvas[:,:,0]` 选中的是所有行，所有列，像素元素的第一个值，也就是，所有B通道的值。

然后对其进行赋值：

```python
canvas[:,:,0] = color[0]
```

完整版本的函数如下，B/G/R通道分别进行赋值：

```python
def init_canvas(width, height, color=(255, 255, 255)):
    canvas = np.ones((height, width, 3), dtype="uint8")
    # Blue 
    canvas[:,:,0] = color[0]
    # Green
    canvas[:,:,1] = color[1]
    # Red
    canvas[:,:,2] = color[2]

    return canvas
```
**具体实现代码如下：**

```python
'''
初始化画布
'''
import cv2
import numpy as np

def init_canvas(width, height, color=(255, 255, 255)):
    canvas = np.ones((height, width, 3), dtype="uint8")
    # Blue 
    canvas[:,:,0] = color[0]
    # Green
    canvas[:,:,1] = color[1]
    # Red
    canvas[:,:,2] = color[2]

    return canvas

canvas = init_canvas(200, 200, color=(125, 100, 255))

cv2.imshow('canvas', canvas)
cv2.waitKey(0)

cv2.destroyAllWindows()
```
**运行实现的效果和第一种方法一样：**

![在这里插入图片描述](https://img-blog.csdnimg.cn/20201006204652418.png#pic_center)

实际上我们还有更快的方法, 可以实现这个功能, 这就需要你熟练掌握Numpy的使用技巧.

我们可以直接赋值color

```python
canvas[:] = color
```
**完整实现过程如下：**

```python
'''
初始化画布
'''
import cv2
import numpy as np

def init_canvas(width, height, color=(255, 255, 255)):
    canvas = np.ones((height, width, 3), dtype="uint8")
    canvas[:] = color
    return canvas

canvas = init_canvas(200, 200, color=(125, 40, 255))

cv2.imshow('canvas', canvas)
cv2.waitKey(0)

cv2.destroyAllWindows()
```

**运行的效果：**

![在这里插入图片描述](https://img-blog.csdnimg.cn/20201006204920615.png#pic_center)

# 综合实验-初始化背景

在这个综合实验里会分别用上述的方法，创建黑色背景，白色背景，彩色背景。

**具体代码如下：**

```python
'''
初始化一个空白的画布
并指定画布的颜色
'''

import cv2
import numpy as np

# 初始化一个空画布 300×300 三通道 背景色为黑色 
canvas_black = np.zeros((300, 300, 3), dtype="uint8")
cv2.imshow("canvas_black", canvas_black)

# 初始化一个空画布 300×300 三通道 背景色为白色 
canvas_white = np.ones((300, 300, 3), dtype="uint8")
canvas_white *= 255

cv2.imshow("canvas_white", canvas_white)

'''
初始化一个彩色的画布 - cv2版本
此函数使用 cv2.split 非常耗时 所以只有在需要的时候才能做到。 否则用Numpy索引。

'''
def InitCanvasV1(width, height, color=(255, 255, 255)):
    canvas = np.ones((height, width, 3), dtype="uint8")

    # 将原来的三个通道抽离出来， 分别乘上各个通道的值
    (channel_b, channel_g, channel_r) = cv2.split(canvas)
    # 颜色的值与个通道的全1矩阵相乘
    channel_b *= color[0]
    channel_g *= color[1]
    channel_r *= color[2]

    # cv.merge 合并三个通道的值
    return cv2.merge([channel_b, channel_g, channel_r])

'''
初始化一个彩色的画布 - numpy版本
使用numpy的索引　赋值
'''
def InitCanvasV2(width, height, color=(255, 255, 255)):
    canvas = np.ones((height, width, 3), dtype="uint8")
    # Blue 
    canvas[:,:,0] = color[0]
    # Green
    canvas[:,:,1] = color[1]
    # Red
    canvas[:,:,2] = color[2]

    return canvas

'''
初始化终极版本

熟练掌握 numpy 才可以提高工作效率哦
'''
def InitCanvasV3(width, height, color=(255, 255, 255)):
    canvas = np.ones((height, width, 3), dtype="uint8")
    canvas[:] = color
    return canvas

# 初始化一个彩色的画布
canvas_color = InitCanvasV2(300, 300, color=(100, 20, 50))
cv2.imshow("canvas_color", canvas_color)

# 等待e键按下 关闭所有窗口
while cv2.waitKey(0) != ord('e'):
    continue
cv2.destroyAllWindows()

```

**运行结果：**

![在这里插入图片描述](https://img-blog.csdnimg.cn/20201006205102788.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3l3c3lkd3Nibg==,size_16,color_FFFFFF,t_70#pic_center)

# 资源传送门

 - 关注【**做一个柔情的程序猿**】公众号
 - 在【**做一个柔情的程序猿**】公众号后台回复 【**python资料**】【**2020秋招**】 即可获取相应的惊喜哦！

# 「❤️ 感谢大家」

 1. **点赞支持下吧，让更多的人也能看到这篇内容（收藏不点赞，都是耍流氓 -_-）**
 2. **欢迎在留言区与我分享你的想法，也欢迎你在留言区记录你的思考过程。**