---
title: 手把手教你进行pip换源
top: false
cover: false
toc: true
mathjax: true
date: 2020-12-03 20:27:39
password:
summary: 学习python，最重要的是我们需要的各样第三方资源包，没有他们学习python是寸步难行，正常情况下大家都是通过在终端输入命令行pip install xx进行安装，但是一般下载都是非常缓慢的，因此就需要自己去换源，加快资源包下载速度。
tags: 
- python
- pip
categories: 
- python
---

## 为什么要换源
学习python，最重要的是我们需要的各样第三方资源包，比如爬虫，有`requests，xpath`，爬虫界的扛把子`Scrapy；Web有django，flask，restframework`；可视化pyQT有`PyQt5，PyQt5.QtWidgets，skimage，cv2`数据可视化届的扛把子`dlib，basemap，pyproj`，其他的比如`sys，os，datatime`等等，没有他们学习python是寸步难行，正常情况下大家都是通过在终端输入命令行`pip install xx`进行安装，但是我相信，以下这种情况大家肯定遇到过：

![](https://img-blog.csdnimg.cn/20200316194419813.png)

可以看到，安装资源包的过程非常的慢，可能都是几KB/s，有时甚至是一百多B/s，但是正常的网速最起码有3-5M/s，这就比较不开心了，最恶心的是，安装的慢就算了，可能安装安装着，直接error了，嗯...哭吧

![](https://img-blog.csdnimg.cn/20200316194520677.png)

还有一种情况是什么呢，就是这种，直接飘黄，警告，恶心吧。。。

![](https://img-blog.csdnimg.cn/2020031619452719.png)

***为什么会造成这种情况呢？***

因为需要的获取的资源包，默认是直接从pypi官网获取的，而pypi是国外的网速就慢，再加上我们国家会限制一些国外不正常的网站，可能会存在误杀，所以，直接从pypi官网获取资源包的时候，难免会各种限速，尤其是安装大一点的资源包的时候，更凉。。。。。

## 国内映像
总有一些大佬，牛逼的人为我们指路，让我们少踩点坑。虽然官网的pypi下载速度慢，但是大佬们为了照顾我们的情绪，专门开发了国内站点，内容和官网的pypi一模一样，但是他的服务器在国内，而且速度非常快，只要我们将pip默认的下载源换成国内源，我们在pip安装时，就是从国内获取了，速度绝对杠杠的，而且包质量没问题，国内站点会隔一段时间同步一次，基本不用担心获取的包有问题，美滋滋，在这里先感谢各位大佬、大神。
  这里呢，我们把现有的国内源贴出来，如下图所示：
  

> 这里是引用

![](https://img-blog.csdnimg.cn/2020031619472466.png)
 ## 换源步骤
（1）首先，打开c盘，找到用户这个文件夹，找到对应你自己主机的文件夹，我的是如下图所示。
![](https://img-blog.csdnimg.cn/20200316194831891.png)
（2）接着在文件夹下创建一个名为pip的文件夹，就像下图这样
![](https://img-blog.csdnimg.cn/20200316194838851.png)
（3）然后进入这个文件夹，创建一个pip.ini文件
![](https://img-blog.csdnimg.cn/20200316194845398.png)
（4）编辑文件，添加如下内容
![](https://img-blog.csdnimg.cn/20200316194852623.png)
选择哪个源链接看自己的兴趣。。。。。我这选择豆瓣
（5）最后保存，重新打开cmd，再安装时，速度杠杠的。。。。。
![](https://img-blog.csdnimg.cn/20200316194858861.png)
我还有一篇博文讲的是另外一种方法也可以达到这样的效果，见：

[python快速安装资源包](https://blog.csdn.net/ywsydwsbn/article/details/104896612)


**~~希望可以帮助到大家！！！！！！！~~** 