---
title: 数据分析模型（二）：模糊聚类分析方法及实例（附完整代码）
top: false
cover: false
toc: true
mathjax: true
date: 2020-12-05 19:30:02
password:
summary: 聚类分析方法是数理统计中研究“物以类聚”的一种多元分析方法，及用数学定量地确定样品的亲疏关系，从而客观地分型化类。由于事物本身在很多情况下都带有模糊性，因此把模糊数学方法引入聚类分析，能使分类更切合实际。我们所应用的模糊聚类方法是基于模糊相似关系上的模糊聚类法，又称系统聚类法。
tags: 
- 数学分析模型
- matlab
categories: 
- 数学建模
---

聚类分析是数据挖掘技术中的一种重要的方法，可以作为一个独立的工具来获得数据分布情况，它广泛地应用于**模式识别、数据分析、图像处理、生物学、经济学**等许多领域。

聚类分析方法是数理统计中研究“==物以类聚==”的一种多元分析方法，及用数学定量地确定样品的亲疏关系，从而客观地分型化类。由于事物本身在很多情况下都带有模糊性，因此把模糊数学方法引入聚类分析，能使分类更切合实际。我们所应用的模糊聚类方法是基于模糊相似关系上的模糊聚类法，又称==系统聚类法==。

# 模糊聚类分析基本知识
## 1. 普通等价关系

**（1）自反性**
对任意![](https://img-blog.csdnimg.cn/20200503094146607.png)，都有![](https://img-blog.csdnimg.cn/20200503094200203.png)（即任何元素的自身和自身有这种关系），则称R是自反关系。相应的矩阵称自反矩阵。
**（2）对称性**
如果从![](https://img-blog.csdnimg.cn/20200503094225686.png)可以推出![](https://img-blog.csdnimg.cn/20200503094237848.png)（即若![](https://img-blog.csdnimg.cn/20200503094253823.png)有这种关系，则![](https://img-blog.csdnimg.cn/20200503094308104.png)也有这种关系），则称R是对称关系。相应的矩阵称为对称矩阵。
例如，朋友是对称关系。父子则不是对称关系。
**（３）传递性**
如果![](https://img-blog.csdnimg.cn/20200503094338355.png)可以推出![](https://img-blog.csdnimg.cn/20200503094350947.png)（即若u和v有这种关系，v和w有这种关系，u和w则必有这种关系），则称R是传递关系。相应的矩阵称传递矩阵。

**相似关系：具有自反性和对称性的关系称为相似关系。
等价关系：具有自反性、对称性和传递性的关系称为等价性。**
## 2. 模糊等价关系
设论域U为有限集合，U上的一个模糊关系R，与其对应的模糊矩阵![](https://img-blog.csdnimg.cn/20200503094519267.png)若满足：
自反性：![](https://img-blog.csdnimg.cn/20200503094531312.png)

对称性：![](https://img-blog.csdnimg.cn/2020050309453962.png)

传递性：![](https://img-blog.csdnimg.cn/20200503094544877.png)

则称n是一个模糊等价矩阵，其关系是模糊等价关系。
若![](https://img-blog.csdnimg.cn/2020050309460354.png)只满足自反性和对称性，则称为相似关系。

# 相关定理
**定理1**  设R是![](https://img-blog.csdnimg.cn/20200503094655826.png)上的一个自反、对称关系，即R是n阶模糊相似矩阵，则存在一个最小的自然数![](https://img-blog.csdnimg.cn/20200503094716636.png)，使得![](https://img-blog.csdnimg.cn/20200503094727845.png)为模糊等价矩阵，且对于一切大于k的自然数w，恒有![](https://img-blog.csdnimg.cn/20200503094750558.png)。![](https://img-blog.csdnimg.cn/20200503094802206.png)称为R的传递闭包矩阵，记为![](https://img-blog.csdnimg.cn/20200503094814304.png)。

**定理2**  如果模糊关系矩阵R是模糊等价关系，则对于任意![](https://img-blog.csdnimg.cn/20200503094849389.png)，所得的λ截矩阵![](https://img-blog.csdnimg.cn/20200503095004541.png)也是等价关系。
根据这个定理，在模糊关系R确定之后，对给定的数![](https://img-blog.csdnimg.cn/20200503095033698.png)，便可得到一个相应的普通等价关系![](https://img-blog.csdnimg.cn/20200503095043887.png)，可以决定一个λ水平分类。

**定理3**  如果![](https://img-blog.csdnimg.cn/20200503095104718.png)，则![](https://img-blog.csdnimg.cn/20200503095118112.png)所分出的每一类必是![](https://img-blog.csdnimg.cn/20200503095126706.png)的某一类的子类。称![](https://img-blog.csdnimg.cn/20200503095137659.png)分类法是![](https://img-blog.csdnimg.cn/20200503095147371.png)分类法的细化。

根据上述3个定理，可以进行聚类分析操作，例如，当所给矩阵关系是相似关系，由定理1可知，自乘若干次后，就可以获得等价关系矩阵，然后再由定理2和定理3进行聚类。
# 模糊聚类分析步骤
模糊聚类分析步骤可以概括为：**==数据标准化，建立模糊相似矩阵，聚类==**。

# 模糊等价矩阵聚类
## （1）传递闭包法
根据标定所建立的模糊矩阵R，一般说来似具有自反性和对称性，不满足传递性，只是模糊相似矩阵，只有当R是模糊等价矩阵时才能聚类，故需要将R改造成模糊等价矩阵。可以通过求传递包将n阶模糊相似矩阵R改造成n阶模糊等价矩阵t（R）。从模糊矩阵R出发，依次求平方：![](https://img-blog.csdnimg.cn/20200503095814480.png)，当第一次出现![](https://img-blog.csdnimg.cn/20200503095828465.png)时，表明![](https://img-blog.csdnimg.cn/20200503095836432.png)已经具有传递性，![](https://img-blog.csdnimg.cn/20200503095908503.png)就是所求的传递闭包t（R）。

在R改造成模糊等价矩阵![](https://img-blog.csdnimg.cn/20200503100025132.png)之后进行截运算，即![](https://img-blog.csdnimg.cn/2020050310004179.png)，可以获得所需分类。
从上述分析可知，λ从大到小分类从细到粗，是一个动态过程。
当n较大时，求传递闭包法的运算量比较大，不适于手工分类，便于计算机程序设计。
## （2）最大树法
在被分类的元素比较多时，要把所建立的模糊相似关系“改造”成模糊等值关系是相当麻烦的，比较简便的方法是——最大树法。
最大树法进行模糊聚类分析的步骤如下：在模糊相似关系矩阵![](https://img-blog.csdnimg.cn/20200503100131515.png)中，按![](https://img-blog.csdnimg.cn/2020050310014411.png)的大小顺序依次用直线将元素连接起来，并标上权重（![](https://img-blog.csdnimg.cn/20200503100156524.png)的数值）。如果在某一步使图中出现了回路，就不画这一步，依次走下一步，直到所有元素连通为止。这样就得到了一棵所谓的最大树（最大树不是唯一的，但不影响分类的结果）。然后，取定λ值，去掉权重低于λ的连线，即可将元素分类，互相连通的元素归为一类。
## （3）编网法
先取定λ，作截矩阵![](https://img-blog.csdnimg.cn/20200503100340180.png)，并将![](https://img-blog.csdnimg.cn/20200503100347908.png)的主对角线上填入元素的符号。在主对角线下方，以节点号代替![](https://img-blog.csdnimg.cn/20200503100511230.png)中的“1”，而“0”则略去不写。由节点“*”向主对角线上引经线（竖线）和纬线（横线）。所谓编网，就是在结点处将经过的经纬线捆绑起来，这样来实现分类。通过打结而能互相联结的点属于同一类。

# 系统聚类法
在平面上有 7 个点w1,w2,w3,...,w7（如图 1（a）），可以用聚类图（如图 1（b））来表示聚类结果。
![](https://img-blog.csdnimg.cn/20200503102459346.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3l3c3lkd3Nibg==,size_16,color_FFFFFF,t_70)

![](https://img-blog.csdnimg.cn/20200503102436316.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3l3c3lkd3Nibg==,size_16,color_FFFFFF,t_70)
怎样才能生成这样的聚类图呢？步骤如下：设Ω ={w1,w2,...,w7}，
1）计算 n 个样本点两两之间的距离{dij} ，记为矩阵
![](https://img-blog.csdnimg.cn/20200503102716543.png)

2）首先构造 n 个类，每一个类中只包含一个样本点，每一类的平台高度均为零；
3）合并距离最近的两类为新类，并且以这两类间的距离值作为聚类图中的平台高
度；
4）计算新类与当前各类的距离，若类的个数已经等于 1，转入步骤 5），否则，回到步骤 3）；
5）画聚类图；
6）决定类的个数和类。
显而易见，这种系统归类过程与计算类和类之间的距离有关，采用不同的距离定义，有可能得出不同的聚类结果。

# 示例
设有5个销售员w1,w2,w3,w4,w5 ，他们的销售业绩由二维变量 （v1,v2）描述，见表 1。
![](https://img-blog.csdnimg.cn/20200503103028700.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3l3c3lkd3Nibg==,size_16,color_FFFFFF,t_70)
记销售员 wi(i=1,2,3,4,5)的销售业绩为(vi1,vi2) 。如果使用绝对值距离来测量点与点之间的距离，使用最短距离法来测量类与类之间的距离，即
![](https://img-blog.csdnimg.cn/20200503103152701.png)
由距离公式d(.,.)，可以算出距离矩阵。
![](https://img-blog.csdnimg.cn/20200503103247213.png)
![](https://img-blog.csdnimg.cn/20200503103310906.png)
![](https://img-blog.csdnimg.cn/20200503103328450.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3l3c3lkd3Nibg==,size_16,color_FFFFFF,t_70)
这样，h9已把所有的样本点聚为一类，因此，可以转到画聚类图步骤。画出聚类图（如图 2（a））。这是一颗二叉树，如图 2（b）。
有了聚类图，就可以按要求进行分类。可以看出，在这五个推销员中w5的工作成绩最佳，w3,w4 的工作成绩较好，而w1,w2 的工作成绩较差。
完全类似于以上步骤，但以最长距离法来计算类间距离，就称为系统聚类法中的最长距离法。

# matlab代码详解

```bash
clc,clear
a=[1,0;1,1;3,2;4,3;2,5];
[m,n]=size(a);
d=zeros(m);
for i=1:m
for j=i+1:m
d(i,j)=mandist(a(i,:),a(j,:)');
%求第一个矩阵的行向量与第二个矩阵的列向量之间对应的绝对值距离
end
end
d
nd=nonzeros(d); %去掉d中的零元素，非零元素按列排列
nd=union(nd,nd) %去掉重复的非零元素
for i=1:m-1
nd_min=min(nd);
[row,col]=find(d==nd_min);tm=union(row,col); %row和col归为一类
tm=reshape(tm,1,length(tm)); %把数组tm变成行向量
fprintf('第%d次合成，平台高度为%d时的分类结果为：%s\n',...
i,nd_min,int2str(tm));
nd(find(nd==nd_min))=[]; %删除已经归类的元素
if length(nd)==0
break
end
end
```
运行结果如下：
![](https://img-blog.csdnimg.cn/20200503104056646.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3l3c3lkd3Nibg==,size_16,color_FFFFFF,t_70)
或者使用MATLAB统计工具箱的相关命令，编写如下程序：

```bash
clc,clear
a=[1,0;1,1;3,2;4,3;2,5];
y=pdist(a,'cityblock'); %求a的两两行向量间的绝对值距离
yc=squareform(y) %变换成距离方阵
z=linkage(y) %产生等级聚类树
[h,t]=dendrogram(z) %画聚类图
T=cluster(z,'maxclust',3) %把对象划分成3类
for i=1:3
tm=find(T==i); %求第i类的对象
tm=reshape(tm,1,length(tm)); %变成行向量
fprintf('第%d类的有%s\n',i,int2str(tm)); %显示分类结果
end
```
运行结果：
![](https://img-blog.csdnimg.cn/20200503104238600.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3l3c3lkd3Nibg==,size_16,color_FFFFFF,t_70)
![](https://img-blog.csdnimg.cn/20200503104251606.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3l3c3lkd3Nibg==,size_16,color_FFFFFF,t_70)
聚类图：
![](https://img-blog.csdnimg.cn/20200503104310537.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3l3c3lkd3Nibg==,size_16,color_FFFFFF,t_70)

> **接下的博文将对代码里面相关的命令进行详解，请大家持续关注！！！**
![](https://img-blog.csdnimg.cn/20200503104718800.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3l3c3lkd3Nibg==,size_16,color_FFFFFF,t_70#pic_center)

**都看到这里了各位老铁就点个赞再走呗，来个关注也行，能够让更多的人看到这篇文章，有问题欢迎各位指导批评！！！！**

**都看到这里了各位老铁就点个赞再走呗，来个关注也行，能够让更多的人看到这篇文章，有问题欢迎各位指导批评！！！！**

**都看到这里了各位老铁就点个赞再走呗，来个关注也行，能够让更多的人看到这篇文章，有问题欢迎各位指导批评！！！！**

> 未经本人允许不得转载

