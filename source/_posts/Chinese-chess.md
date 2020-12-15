---
title: 国粹——中国象棋震撼来袭，python的赶紧上车！
top: false
cover: false
toc: true
mathjax: true
date: 2020-12-13 15:22:31
password:
summary: 利用 python 将中国国粹——中国象棋展示出来。
tags: 
- python
- turtle
categories: 
- python
---

> **阅读博客前，先点赞，养成习惯，顺便来个关注，这对我是最大的鼓励！！！**

中国象棋相必大家都玩过，突发奇想，想着怎么用python把中国国粹的中国象棋做出来呢？？？？？？

首先老样子看看用python做出来的效果：
![](https://img-blog.csdnimg.cn/20200712205642693.gif#pic_center)

# 第一步：导入资源包
这次利用到的还是`海龟turtle`

```python
import turtle
```
# 第二步：初始化
初始化过程中首先获得海龟的钢笔，接着设置窗口的大小、标题和背景。

```python
# 初始化
pen = turtle.Pen()# 获取海龟的画笔
turtle.setup(714,800)# 设置窗口的大小
turtle.title("中国象棋")# 设置窗口的标题
turtle.bgcolor("#F4C79E")# 设置窗口的背景
pen.hideturtle()
turtle.tracer(False)
```
# 第三步：定义棋子名称与坐标

中国象棋中由車、馬、相（象）、士（仕）、炮、卒、将、帥（帅）组成，各个棋子有相应的坐标。这个可供参考，如果有问题可以自行设置相应的坐标。

```python
array = [
    # A方棋子
    {
        "text": "車",
        "role": "A",
        "pix": (-330, 369)

    },
    {
        "text": "馬",
        "role": "A",
        "pix": (-247.0, 369.0)
    },
    {
        "text": "象",
        "role": "A",
        "pix": (-166.0, 369.0)
    },
    {
        "text": "士",
        "role": "A",
        "pix": (-86.0, 368.0)
    },
    {
        "text": "将",
        "role": "A",
        "pix": (-5.0, 369.0)
    },
    {
        "text": "士",
        "role": "A",
        "pix": (79.0, 368.0)
    },
    {
        "text": "象",
        "role": "A",
        "pix": (159.0, 368.0)
    },
    {
        "text": "馬",
        "role": "A",
        "pix": (239.0, 367.0)
    },
    {
        "text": "車",
        "role": "A",
        "pix": (318.0, 369.0)
    },
    {
        "text": "卒",
        "role": "A",
        "pix": (-329.0, 126.0)
    },
    {
        "text": "卒",
        "role": "A",
        "pix": (-167.0, 126.0)
    },
    {
        "text": "卒",
        "role": "A",
        "pix": (-6.0, 126.0)
    },
    {
        "text": "卒",
        "role": "A",
        "pix": (156.0, 126.0)
    },
    {
        "text": "卒",
        "role": "A",
        "pix": (319.0, 126.0)
    },
    {
        "text": "炮",
        "role": "A",
        "pix": (-248.0, 209.0)
    },
    {
        "text": "炮",
        "role": "A",
        "pix": (239.0, 208.0)
    },

    # B方棋子
    {
        "text": "車",
        "role": "B",
        "pix": (-330.0, -359.0)
    },
    {
        "text": "馬",
        "role": "B",
        "pix": (-247.0, -359.0)
    },
    {
        "text": "相",
        "role": "B",
        "pix": (-166.0, -359.0)
    },
    {
        "text": "仕",
        "role": "B",
        "pix": (-86.0, -359.0)
    },
    {
        "text": "帥",
        "role": "B",
        "pix": (-5.0, -359.0)
    },
    {
        "text": "仕",
        "role": "B",
        "pix": (79.0, -359.0)
    },
    {
        "text": "相",
        "role": "B",
        "pix": (159.0, -359.0)
    },
    {
        "text": "馬",
        "role": "B",
        "pix": (239.0, -359.0)
    },
    {
        "text": "車",
        "role": "B",
        "pix": (318.0, -359.0)
    },
    {
        "text": "卒",
        "role": "B",
        "pix": (-329.0, -126.0)
    },
    {
        "text": "卒",
        "role": "B",
        "pix": (-167.0, -126.0)
    },
    {
        "text": "卒",
        "role": "B",
        "pix": (-6.0, -126.0)
    },
    {
        "text": "卒",
        "role": "B",
        "pix": (156.0, -117.0)
    },
    {
        "text": "卒",
        "role": "B",
        "pix": (319.0, -117.0)
    },
    {
        "text": "炮",
        "role": "B",
        "pix": (-248.0, -199.0)
    },
    {
        "text": "炮",
        "role": "B",
        "pix": (239.0, -199.0)
    },
]
```
# 第四步：绘制棋盘

如下图，棋盘是由网格组成，中间有一个“楚河——漢界”。

```python
# 绘制棋盘函数
def draw():
    # 绘制网格边框
    pen.penup()
    pen.setposition(-360, 402)
    pen.pendown()
    pen.color("#6E3F25")
    pen.width(30)
    for x in range(1,5):
        if x % 2 != 0:
            pen.forward(710)
        else:
            pen.forward(795)
        pen.right(90)

    # 绘制网格
    pen.penup()
    pen.setposition(-330, 370)
    pen.width(2)
    pen.pendown()
    for x in range(9):
        pen.forward(650)
        pen.backward(650)
        pen.right(90)
        pen.forward(81)
        pen.left(90)
    pen.forward(650)

    pen.left(90)
    for x in range(8):
        pen.forward(730)
        pen.backward(730)
        pen.left(90)
        pen.forward(81)
        pen.right(90)

    pen.penup()
    pen.setposition(-280, 6)
    pen.pendown()
    pen.pencolor("#F4C79E")
    pen.right(90)
    pen.width(79)
    pen.forward(550)
    pen.width(1)

    pen.penup()
    pen.setposition(-230, -25)
    pen.color("#6E3F25")
    pen.write("楚河", align="center", font=("Baoli TC", 50, "bold"))

    pen.penup()
    pen.forward(450)
    pen.write("漢界", align="center", font=("Baoli TC", 50, "bold"))

    pen.penup()
    for x in [[-3.0, 290.0], [-4.0, -278.0]]:
        pen.up()
        pen.setposition(x)
        pen.down()
        pen.setheading(45)
        pen.pendown()
        pen.width(2)
        pen.color("#5E3F25")
        for x in range(4):
            pen.forward(114)
            pen.backward(114)
            pen.left(90)

    for x in array:
        if x["role"] == "A":
            chess(x["text"], x["pix"], "#A46A0C", "#2F1500")
        else:
            chess(x["text"], x["pix"], "#E69772", "#AB2A0E")

    turtle.update()# 刷新图像
```

![](https://img-blog.csdnimg.cn/20200712205826616.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3l3c3lkd3Nibg==,size_16,color_FFFFFF,t_70)

# 第五步：定义落子函数
如下图，我们每一次点击棋子并落下过程中，后台会显示我们点击棋子的状态和是否落子。

```python
# 落子函数

def chess(text, pix, bgcolor, textcolor):

    """
    text:     落子显示文本
    pix:      落子坐标
    bgcolor:  背景颜色
    textcolor:落子颜色
    """

    pen.penup()
    pen.setposition(pix)
    pen.pendown()
    pen.color("#6E3F25")
    pen.dot(70)
    pen.color(bgcolor)
    pen.dot(55)
    pen.color("white")
    pen.penup()
    pen.setheading(270)
    pen.forward(25)
    pen.color(textcolor)
    pen.write(text, align="center", font=("Baoli TC", 40, "bold"))
```

落子情况：

![](https://img-blog.csdnimg.cn/20200712205943984.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3l3c3lkd3Nibg==,size_16,color_FFFFFF,t_70)

# 第六步：鼠标点击事件
当我们点击棋子时会触发相应的时间，如上图所示，比如当我们点击“卒”时，它会显示我们点击的是哪一方的棋子，棋子的坐标是什么。

```python
def click(x, y):

    global priChess

    if priChess == {}:
        for z in array:
            if abs(z["pix"][0] - x) <= 35 and abs(z["pix"][1] - y) <= 35:
                print("发现目标：", z)
                priChess = z
                pen.penup()
                pen.setposition(z["pix"])
                pen.color("white")
                pen.penup()
                pen.setheading(270)
                pen.forward(25)
                pen.write(z["text"], align="center", font=("Baoli TC", 40, "bold"))
                break
    else:
        print("落子")
        priChess["pix"] = (x, y)
        array.append(priChess)
        priChess = {}
        pen.reset()
        draw()
```
