---
title: 数学评价模型（二）：模糊综合评判
top: false
cover: false
toc: true
mathjax: true
date: 2020-12-15 15:32:47
password:
summary: 综合评判就是对受多个因素制约的事物或对象作出一个总的评价，这是在日常生活和科研工作中经常遇到的问题，例如对学生的评价应该考虑到各门功课的成绩，但绝不能忽略品行和健康。在歌手大赛中，也不能只考虑艺术水平而忽略了歌手的文化素质。由于从多方面对事物进行评价难免带有模糊性和主观性，采用模糊数学的方法进行综合评判将使结果尽量客观，从而取得更好的实际效果。
tags: 
- matlab
- 数学评价模型
categories: 
- 数学建模
---

**综合评判就是对受多个因素制约的事物或对象作出一个总的评价**，这是在日常生活和科研工作中经常遇到的问题，例如对学生的评价应该考虑到各门功课的成绩，但绝不能忽略品行和健康。在歌手大赛中，也不能只考虑艺术水平而忽略了歌手的文化素质。由于从多方面对事物进行评价难免带有模糊性和主观性，采用模糊数学的方法进行综合评判将使结果尽量客观，从而取得更好的实际效果。

**模糊综合评判的数学模型可分为一级模型和多级模型。**下面就常用基本的模型进行介绍。

# 1. 一级模糊综合评判

综合评判有三要素:
(ⅰ) 因素集U={u1,u2,u3,...,un}——被评判对象的各因素组成的集合；
(ⅱ) 评判集V={v1,v2,v3,...,vn}——评语组成的集合；
(ⅲ) 建立单因素判断，即对单个因素ui(i=1,2,...,n)的评判，得到V上的模糊集(ri1,ri2,ri3,...,rim)，所以它是从U到V的一个模糊映射：

![](https://i.loli.net/2020/12/15/VfNUsl3QATeip5B.png)

由模糊映射f可以确定一个模糊关系

![](https://i.loli.net/2020/12/15/9kOoZisU7gPuzvW.png)

称为评判矩阵，它是由所有对单因素评判的模糊集组成的。

由于各因素地位未必相等，所以需对各因素加权，用U上的模糊集A=(a1,a2,a3,...,an)表示各因素的权重分配，将它与评判矩阵R进行合成，即得到模糊综合评判集，采用最大隶属度原则对各因素进行综合评判。下面四种综合评判模型则是根据合成运算的不同而得出的。


## 模型Ⅰ:  M(∨,∧)

![](https://img-blog.csdnimg.cn/20200410123138352.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3l3c3lkd3Nibg==,size_16,color_FFFFFF,t_70)

这里bj是(ri1,ri2,ri3,...,rim)的函数，也就是评判函数。这种方法通过取小与取大两种运算，因此，称该模型为 M(∨,∧)模型。此模型的特点是一种“**主因素突出法**”的综合评判。**由于权重系数的作用体现较弱，有时会导致评价结果的不理想，这时需要将进行修正并进行归一化处理。**


## 模型Ⅱ: M(·,∨)

该模型采用两种运算：

- 一种是普通乘法运算，用·表示；
- 另一种是取大运算，用∨表示。

利用此模型计算为　

![](https://img-blog.csdnimg.cn/20200410123639237.png)


其中乘法运算不会丢失有用的信息，而取大运算会丢失有用信息。**该模型优点是较好地反映了单因素评价结果的重要程度。**

## 模型Ⅲ: M(+,∧)
该模型采用两种运算：

- 一种是普通加法运算，用+表示；
- 另一种是取小运算，用∧表示。

利用此模型计算为　

![](https://img-blog.csdnimg.cn/20200410123834743.png)

此模型的特点是对每个评语vj都同时考虑各种因素的综合评判。

## 模型Ⅳ: M(+,·)
该模型采用两种运算：

- 一种是普通加法运算，用“+”表示；
- 另一种是乘法运算，用“·”表示。

利用此模型计算为

![](https://img-blog.csdnimg.cn/20200410133216825.png)

此模型的特点是：在确定评语vj对模糊综合评判集的隶属度bj时，考虑了所有因素ui(i=1,2,...,n)的影响；由于同时考虑到所有因素的影响，所以各ai的大小具有刻画各因素重要程度的权系数意义。

在上述各种评价模型中，因为运算的定义不同，所以对同一评价对象求出的评价结果也会不一样。

##【典例】

> 服装评判问题

设U={花色式样，耐穿程度，价格费用}，V={很欢迎，比较欢迎，不太欢迎，不欢迎}，对某一种服装，请若干专门人员进行单因素评价。


单考虑花色式样，若有70%的人很欢迎，有20%的人比较欢迎，10%的人不太欢迎便可得出

花色式样→（0.7，0.2，0.1，0）
类似地，可以得到耐穿程度和价格费用的数值，如下：
耐穿程度→（0.2，0.3，0.4，0.1）
价格费用→（0.3，0.4，0.2，0.1）
将上述的所有单因素评判组成评判矩阵，可以得到模糊综合评判矩阵，如下：

![](https://img-blog.csdnimg.cn/20200410150531628.png)

不同的顾客，由于职业、性别、年龄、爱好、经济状况等等的不同，对服装的三要素所给予的权数也不同。假若设某类顾客所给的权重为：A=（0.5,0.3,0.2），采用模型Ⅳ可得此类顾客对这种服装的综合评判为：

![](https://img-blog.csdnimg.cn/20200410150615883.png)

它表示的评价是：“很欢迎”的程度为47%；“比较欢迎” 的程度为27%；“不太欢迎” 的程度为21%；“不欢迎” 的程度为5%。按最大隶属原则，结论是“很欢迎”。
这个结果是归一化的。如果采用模型“”，“”Ⅰ的,运算，得出的综合评判结果不一定是归一化的，因此需将结果归一化处理。

Matlab代码实现
```bash
%% 模糊评判矩阵
R = [0.7 0.2 0.1 0 
     0.2 0.3 0.4 0.1
     0.3 0.4 0.2 0.1]
%% 各因素的权重
A = [0.5 0.3 0.2]
%% 隶属度计算
B = A*R
```

# 2. 多级模糊综合评判

如果评判对象的有关因素很多，很难合理地定出权数分配，即难以真实地反应各因素在整体的地位，这是需采取多级评判。


**多因素多层次系统的综合评判方法**是：**首先按最低层次的各因素进行综合评价，然后再按上一层次的各因素进行综合评判；依次向更高一层评判，一直评定到最高层次得出总的评价结果。**


下面给出多级模糊综合评判的数学模型的一般描述。
假设因素集为U={U1,U2,U3,...,Un}，对其中的Ui(i=1,2,...,n)细划分为Ui={Ui1,Ui2,...,Uim}

对其中的Uij(i=1,2,...,n;j=1,2,...,m)再进一步细划分为Uij={Uij1,Uij2,...,Uijp}

如此划分下去，就反映了影响因素的层次性。而评判时，应从最后一次划分最底层的因素开始，一级一级往上评判，直到评定到最高层。
下图给出三级模糊综合评判模型示意，更多级的评判过程依次类推。

![](https://img-blog.csdnimg.cn/20200410154901525.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3l3c3lkd3Nibg==,size_16,color_FFFFFF,t_70)

##【典例】

> 产品质量的综合评判


对同一个产品质量问题先使用单层模糊综合评判，再使用双层模糊综合评判。
### （1）单层模糊综合评判
因素集：U={u1,u2,u3,u4,u5,u6,u7}
评判集：V={v1,v2,v3,v4} 式中v1表一级, v2二级, v3等外, v4表废品。
权重向量：A=(0.1,0.12,0.07,0.07,0.16,0.1,0.1,0.1,0.18)
评判矩阵：由专家、客户、质量检查员组成的评判小组，先打分并做简单处理得到如下的综合评判矩阵：

![](https://img-blog.csdnimg.cn/2020041015541676.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3l3c3lkd3Nibg==,size_16,color_FFFFFF,t_70)

利用模型Ⅰ计算综合评判

![](https://img-blog.csdnimg.cn/20200410155431407.png)

结果显示，无论是一级、二级、等外、废品的隶属度都是0.18，对这个具体问题而言模糊变换无法给出答案。可以采用其他方法，例如加权平均，也可以采用双层模糊综合评判。


### （2）双层模糊综合评判
因素集：

![](https://img-blog.csdnimg.cn/20200410155502472.png)

评判集：V={v1,v2,v3,v4}式中v1表一级,  v2二级, v3等外, v4表废品。

第一层权重向量：

![](https://img-blog.csdnimg.cn/20200410155536383.png)

第一层评价矩阵：

![](https://img-blog.csdnimg.cn/20200410155548784.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3l3c3lkd3Nibg==,size_16,color_FFFFFF,t_70)

第一层评判，使用模型Ⅰ，有

![](https://img-blog.csdnimg.cn/20200410155559355.png)

第二层评判矩阵：将一层评判结果组合起来形成二级评判矩阵。

![](https://img-blog.csdnimg.cn/20200410155610939.png)

第二层权重分配：因素集U={U1,U2,U3}的权重分配为A=(0.2,0.35,0.45)。
第二层评判，仍然使用模型Ⅰ，有

![](https://img-blog.csdnimg.cn/2020041015572129.png)

按最大隶属度原则，此产品属一级品。

Matlab代码实现
模糊综合评价的函数：fuzzy_zhpp.m
```bash
function[B]=fuzzy_zhpp(model,A,R) %模糊综合评判
B=[];
[m,s1]=size(A);
[s2,n]=size(R);
if(s1~=s2)
     disp('A的列不等于R的行');
else
    if(model==1)                 %主因素决定型
        for(i=1:m)
           for(j=1:n)
               B(i,j)=0;
               for(k=1:s1)
                   x=0;
                   if(A(i,k)<R(k,j))
                      x=A(i,k);
                   else
                      x=R(k,j);
                   end
                  if(B(i,j)<x)
                     B(i,j)=x;
                  end
               end
           end
       end
   elseif(model==2)               %主因素突出型
       for(i=1:m)
          for(j=1:n)
              B(i,j)=0;
              for(k=1:s1)
                  x=A(i,k)*R(k,j);
                  if(B(i,j)<x)
                     B(i,j)=x;
                  end
              end
          end
       end
   elseif(model==3)              %加权平均型
          for(i=1:m)
             for(j=1:n)
                B(i,j)=0;
                for(k=1:s1)
                    B(i,j)=B(i,j)+A(i,k)*R(k,j);
                end
              end
           end
    elseif(model==4)             %取小上界和型
           for(i=1:m)
               for(j=1:n)
                   B(i,j)=0;
                   for(k=1:s1)
                       x=0;
                       x=min(A(i,k),R(k,j));
                       B(i,j)=B(i,j)+x;
                   end
                       B(i,j)=min(B(i,j),1);
               end
            end
      elseif(model==5)            %均衡平均型
            C=[];
            C=sum(R);
            for(j=1:n)
               for(i=1:s2)
                   R(i,j)=R(i,j)/C(j);
               end
            end
            for(i=1:m)
                for(j=1:n)
                    B(i,j)=0;
                   for(k=1:s1)
                       x=0;
                       x=min(A(i,k),R(k,j));
                       B(i,j)=B(i,j)+x;
                   end
                end
            end
        else
            disp('模型赋值不当');
        end
end
end
```

主函数：main.m

```bash
function main
A1=[0.30 0.42 0.28];
A2=[0.20 0.50 0.30];
A3=[0.30 0.30 0.40];

R=[0.30 0.32 0.26 0.27;
    0.26 0.36 0.20 0.20;
    0.40 0.28 0.30 0.20];
fuzzy_zhpp(1,A1,R)
fuzzy_zhpp(1,A2,R)
fuzzy_zhpp(1,A3,R)
end
```

有帮助的话各位小伙伴们来个三连击，鼓励鼓励一下我这只小白！！！

# 资源传送门

- 关注【**做一个柔情的程序猿**】公众号
- 在【**做一个柔情的程序猿**】公众号后台回复 【**python资料**】【**2020秋招**】 即可获取相应的惊喜哦！
# 「❤️ 感谢大家」

- 点赞支持下吧，让更多的人也能看到这篇内容（收藏不点赞，都是耍流氓 -_-）
- 欢迎在留言区与我分享你的想法，也欢迎你在留言区记录你的思考过程