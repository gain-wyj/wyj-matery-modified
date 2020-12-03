---
title: Python实现树莓派摄像头持续录像并传送到主机
top: true
cover: false
toc: true
mathjax: true
date: 2020-12-03 15:40:22
password:
summary: 关于树莓派，想必从事嵌入式开发的开发者都有听过，树莓派原名为Raspberry Pi，此博文教你如何利用Python实现树莓派摄像头持续录像并传送到主机。
tags: 
- python
- Raspberry Pi
- 网络
categories: 
- python
---

关于树莓派，想必从事嵌入式开发的开发者都有听过，树莓派原名为`Raspberry Pi`，也就是它的英文读法，树莓派诞生于英国，由“Raspberry Pi 基金会”这个慈善组织注册开发。埃•厄普顿就是该项目的头目。在2012年的3月，英国剑桥大学埃本•阿普顿（Eben Epton）正式发售世界上最小的台式机，又称卡片式电脑，外形只有信用卡大小，却具有电脑的所有基本功能，这就是Raspberry Pi电脑板，中文译名”树莓派”！

树莓派作为一个轻便迷你的小终端很受大众的喜爱！！！
![树莓派](https://img-blog.csdnimg.cn/20200330190903552.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3l3c3lkd3Nibg==,size_16,color_FFFFFF,t_70)

# 树莓派的特点

与常见的51单片机和STM32等这类的嵌入式微控制器相比，不仅可以完成相同的IO引脚控制之外，还能运行有相应的操作系统，可以完成更复杂的任务管理与调度，能够支持更上层应用的开发，为了开发者提供了更广阔的应用空间。比如开发语言的选择不仅仅只限于C语言，连接底层硬件与上层应用，可以实现物联网的云控制和云管理，也可以忽略树莓派的IO控制，使用树莓派搭建小型的网络服务器，做一些小型的测试开发和服务。

与一般的PC计算机平台相比，树莓派可以提供的IO引脚，能够直接控制其他底层硬件的功能，这是一般PC计算机做不到的，当然，树莓派体积小，成本低，照常可以完成一些PC任务与应用。

树莓派自带的摄像头拍摄夜空是有先例的，起码可以做到延时摄影。对于实时拍摄没有研究，但是仍然有必要测试。
树莓派自带的摄像头是500万像素，价格在26-29欧元（人民币200+左右）

# 实时还是事后采集记录结果？
树莓派上的摄像机，是使用一个`raspivid`命令操作的。 抛开这个命令的其他参数，其**输出数据有2种方式**：

 - 将数据保存成文件，储存在SD卡上，以便事后读取;
 - 将数据按照字节流的形式，直接输出到STDOUT标准输出中，可以实时获取。

选择哪种方式，首先要考虑我们能否具有足够的采集数据的能力。

`raspivid`命令可以调节相机模块的输出比特率。输出是以`H264`编码输出的，比特率一般默认是17Mbps，但是这个数字可以调小。 如果按照17Mbps算，就是一秒钟2.12兆字节。 我们记录数据或者获取数据的速度不能低于这个值，否则长时间录像可能造成树莓派的缓存充满，导致树莓派崩溃。

树莓派的网卡是使用了其USB总线，传送速度是100Mb/s或者12.5MB/s。 实际上后文的实验表明，目前能达到的传送速度只有**3MB/s（TCP）** 或者 **6MB/s（UDP）**。

如果使用SD卡存储，这个记录速度也是可以达到的，但是，SD卡有写入寿命，这是要考虑的。 例如，对于32GB的卡，即使我们能利用全部存储空间，以2MB/s的速度录像，也只能记录4.55小时。

# 如何通过网络实时传送数据？
`raspivid`命令的`-o`选项，就是用来指定输出文件的。 在`Linux`系统中，输出到文件并不等于写入到磁盘（这里是SD卡）。 我们仍然可能使用`RAMDisk`这种技术，让输出只是暂时存储在内存中，并稍后读取，然后删除之。 但是，树莓派的可用内存可能只有280MB，这最多只能记录差不多2分钟的视频。

如果我们有文件形式的摄像记录，那么就似乎可以使用文件传输的协议，例如`sftp, scp`等等登录到树莓派下载文件了。 然而这是不对的。这些协议在传输中使用了加密。

树莓派在向我们的电脑进行数据传送的时候，如果用这些协议，就必须先对发送的数据进行加密。 在互联网上，加密是很好的设计。但是在树莓派和电脑之间只用一根网线连接的时候，就不是了。 树莓派的运算能力是很有限的，使用加密只会让传送速度变慢，所以，不要使用加密！

我们使用最原始而简单的方法：**使用netcat命令**，在笔记本电脑这一端监听数据输入。 在树莓派这一端，我们让raspivid获取一定周期（比如10分钟，也许可以更长）的录像， 将结果设定为直接输出，然后利用Linux的管道机制，直接送进netcat发送。

# 配置由树莓派和笔记本构成的网络
树莓派和笔记本电脑之间的连接，使用普通网线即可， 因为笔记本和树莓派上的网卡都能自动适应网线，设定正确的模式（正常来说要使用交叉网线）。

重要的一步是，笔记本电脑和树莓派连接后构成的网络中，需要手动为两个设备设定IP地址。 对于笔记本电脑的设定，就比较简单了。 我们将笔记本电脑和树莓派相连的网卡上，将电脑的**IP地址设定为`xxx.xxx.x.xxx`，子网掩码为`255.255.255.0`，网关不要填**。

配置树莓派的方法是，先将树莓派断电，然后取出所用的SD卡，用读卡器插回电脑。 在SD卡的`boot分区`中，有个`cmdline.txt`，这是树莓派开机时所用到的一些参数。

打开这个文件，会发现里面只有一行。这一行中用空格分开了很多设定参数。 我们在这一行的结尾，不添加空行，直接加上空格，然后写上：`ip=xxx.xxx.x.xxx`

当然如果这一行里面已经有了ip=的参数，应该直接修改它。

这样的结果就是，树莓派开机之后，会自己选择这个IP地址作为自己的地址。 

# 实现在笔记本上监听输入
`netcat命令`，在每收到一个文件的`EOF`（End Of File，表明文件已经到结尾），就会退出。 我们为了让接收能够连续进行，需要用脚本连续运行这个命令。 这样就会为每个新接收到的视频，在笔记本电脑这一端建立一个文件用来存储。

```python
# -*- coding: utf-8 -*-

import os
import signal
import subprocess
import sys
import uuid

# 获取程序运行的本地目录，和用来存储接收结果的recv文件夹目录

BASEPATH = os.path.realpath(os.path.dirname(sys.argv[0]))
RECV = os.path.join(BASEPATH, 'recv')

# 如果接收目录不存在，就自动新建

print " *** Received files are put into: %s" % RECV
if not os.path.isdir(RECV):
    os.system('mkdir -p %s' % RECV)

# 下面的部分用来记录正在等待接收的文件。这个文件会以一个UUID.tmp的格式命名。
# 在接收成功后，就会被重命名为UUID。（UUID是一个特定格式的唯一字符串，不会重复）。
# 如果在接收过程中按下Ctrl+C，就会发送一个终止命令给程序，这样程序会退出，
# 并删除没有接收完整的那个文件。

working = False
fullname = False

def sigint_handler(signum, frame):
    global fullname, working
    print "\n"
    print " *** SIGINT detected. End the program."
    if working and fullname != False:
        print " *** Unfinished recording deleted."
        os.system('rm -f %s.tmp' % fullname)
    exit()
signal.signal(signal.SIGINT, sigint_handler)

# 使用一个死循环来不断运行netcat(nc)命令。

n = 1
while True:
    recname = str(uuid.uuid1())
    print " [%8d] Listening for file [%s]. Use Ctrl+C to stop this script." % (n, recname)
    fullname = os.path.join(RECV, recname)

    working = True # 标记接收开始
    # 使用 nc -lp 10401 命令接收数据，表明端口为10401。
    subprocess.call('nc -lp 10401 > %s.tmp' % fullname, shell=True)
    os.system('mv %s.tmp %s' % (fullname, fullname))
    working = False # 标记接收完毕

    n += 1
```
上文所述的脚本，在笔记本上运行之后，就会在本地开启10401端口，等待树莓派上传送的文件。 传送的会直接写入一个由UUID（全局唯一ID）标识的文件中，可以供以后处理。

# 在树莓派上摄像并发送摄像结果
在树莓派上命令拍摄的方法是：

```powershell
$ raspivid -o - -b 16000000 -t 100000 | nc xxx.xxx.x.xxx 10401
```

**这条指令的意义如下**：

 - -o -，使用-o设定输出，-表示直接输出到标准输出中，不写入文件。
 - -b 16000000，设定输出比特率为16000000 bit/s。这大约是2兆字节每秒。
 - -t 100000，设定录像时间为100000毫秒，亦即100秒。
 - | nc xxx.xxx.x.xxx 10401，使用管道|将结果导入到nc中，nc是发送模式，目标是xxx.xxx.x.xxx计算机上的10401端口。
