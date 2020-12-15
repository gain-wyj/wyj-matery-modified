# wyj-matery-modified

# Demo
https://gain-wyj.cn/

# 安装 Hexo
可以使用 npm 开始安装 Hexo 。也可查看 Hexo 的详细文档。
在命令行输入执行以下命令：

```bash
npm install -g hexo-cli
```

# 初始化项目
安装 Hexo 完成后，再执行下列命令，Hexo 将会在指定文件夹中新建所需要的文件。

```bash
hexo init my-blog
cd my-blog
npm install
```
新建完成后，指定文件夹的目录如下：

```
.
├── _config.yml # 网站的配置信息，您可以在此配置大部分的参数。 
├── package.json
├── scaffolds # 模版文件夹
├── source  # 资源文件夹，除 _posts 文件，其他以下划线_开头的文件或者文件夹不会被编译打包到public文件夹
|   ├── _drafts # 草稿文件
|   └── _posts # 文章Markdowm文件 
└── themes  # 主题文件夹
```
好了，如果上面的命令都没报错的话，就恭喜了，运行 `hexo s` 命令，其中 s 是 server 的缩写，在浏览器中输入 http://localhost:4000 回车就可以预览效果了。

```bash
hexo s
```


# 写文章、发布文章
首先在博客根目录下右键打开git bash，安装一个扩展`npm i hexo-deployer-git`。

然后输入`hexo new post "article title" `，新建一篇文章。

然后打开`D:\study\program\blog\source\_posts`的目录，可以发现下面多了一个文件夹和一个.md文件，一个用来存放你的图片等数据，另一个就是你的文章文件啦。

编写完markdown文件后，根目录下输入`hexo g`生成静态网页，然后输入`hexo s`可以本地预览效果，最后输入`hexo d`上传到github上。这时打开你的github.io主页就能看到发布的文章啦。

当然也可以直接输入 `hexo d -g` 进行生成与上传

# 常用 hexo 命令

```bash
hexo new "postName" #新建文章
hexo new page "pageName" #新建页面
hexo generate #生成静态页面至public目录
hexo server #开启预览访问端口（默认端口4000，'ctrl + c'关闭server）
hexo deploy #部署到GitHub
hexo help  # 查看帮助
hexo version  #查看Hexo的版本
```

# 缩写的 hexo 命令

```bash
hexo n ==> hexo new
hexo g ==> hexo generate
hexo s ==> hexo server
hexo d ==> hexo deploy
```

# 组合命令

```bash
hexo s -g # 生成并本地预览
hexo d -g # 生成并上传
```

# 备份博客源文件
有时候我们想换一台电脑继续写博客，这时候就可以将博客目录下的所有源文件都上传到github上面。

首先在github博客仓库下新建一个分支`hexo`，然后`git clone`到本地，把`.git`文件夹拿出来，放在博客根目录下。

然后`git checkout hexo`切换到`hexo`分支，然后`git add .`，然后`git commit -m "xxx"`，最后`git push origin hexo`提交就行了。

# 个性化设置（matery主题）
下面的个性化设置主要针对的是matery主题，如果你想用我现在博客这个主题，可以直接看这个章节。

## 更换主题
这两天花时间将我的博客换了一个主题，现在这个主题看着更加的炫（zhuang）酷（bi），并且响应式更友好，点起来就很舒服，功能也多很多。

主题的原地址在这里：[hexo-theme-matery](https://github.com/blinkfox/hexo-theme-matery) ，它的文档写得也非常的详细，还有中英文两个版本，作者回复也很及时。效果图如下，可以看出非常合我的口味：

![gain-wyj.png](https://i.loli.net/2020/12/15/WHZ6rJXkpwAdcBe.png)

但是我自己使用起来还是遇到了好几个问题，经过两天的不懈摸鱼，终于基本解决了，这里分享一下。

首先先按照文档教程安装一遍主题，然后是可以正常打开的，如果你是一般使用的话，基本没啥问题了。但是我是重度强迫症，一点小毛病就看着难受，下面列举一下我遇到的问题以及解决方法。

## 文章头设置
首先为了新建文章方便，建议将`/scaffolds/post.md`修改为如下代码：

```json
---
title: {{ title }}
date: {{ date }}
top: false
cover: false
password:
toc: true
mathjax: true
summary:
tags:
categories:
---
```
这样新建文章后不用你自己补充了，修改信息就行。

## 添加404页面
原来的主题没有404页面，加一个也不是什么难事。首先在`/source/`目录下新建一个`404.md`，内容如下：
```json
---
title: 404
date: 2019-07-19 16:41:10
type: "404"
layout: "404"
description: "你来到了没有知识的荒原 :("
---
```
然后在`/themes/matery/layout/`目录下新建一个`404.ejs`文件，内容如下：
```html
<style type="text/css">
    /* don't remove. */
    .about-cover {
        height: 75vh;
    }
</style>

<div class="bg-cover pd-header about-cover">
    <div class="container">
        <div class="row">
            <div class="col s10 offset-s1 m8 offset-m2 l8 offset-l2">
                <div class="brand">
                    <div class="title center-align">
                        404
                    </div>
                    <div class="description center-align">
                        <%= page.description %>
                    </div>
                </div>
            </div>
        </div>
    </div>
</div>

<script>
    // 每天切换 banner 图.  Switch banner image every day.
    $('.bg-cover').css('background-image', 'url(/medias/banner/' + new Date().getDay() + '.jpg)');
</script>
```

## “关于”页面增加简历（可选）
修改`/themes/matery/layout/about.ejs`，找到`<div class="card">`标签，然后找到它对应的`</div>`标签，接在后面新增一个`card`，语句如下：
```html
<div class="card">
    <div class="card-content">
        <div class="card-content article-card-content">
                <div class="title center-align" data-aos="zoom-in-up">
                    <i class="fa fa-address-book"></i>&nbsp;&nbsp;<%- __('myCV') %>
                </div>
                <div id="articleContent" data-aos="fade-up">
                    <%- page.content %>
                </div>
        </div>
    </div>
</div>
```
这样就会多出一张card，然后可以在`/source/about/index.md`下面写上你的简历了，当然这里的位置随你自己设置，你也可以把简历作为第一个card。

## 解决mathjax与代码高亮的冲突
如果你按照教程安装了代码高亮插件hexo-prism-plugin，单独使用是没有问题的，但如果你又使用了mathjax，并且按照网上教程，安装kramed插件并修改了js文件里的正则表达式（为了解决markdown和mathjax的语法冲突），好了，那你的代码就无法高亮了。解决方法很简单，别用kramed插件了，还用原来自带的marked插件，直接改它的正则表达式就行了，改法如下：

打开`D:\study\program\blog\node_modules\marked\lib\marked.js`
`escape:`处替换成：
```json
escape: /^\\([`*\[\]()#$+\-.!_>])/,
```
`em:`处替换成：
```json
em: /^\*((?:\*\*|[\s\S])+?)\*(?!\*)/,
```
这时在文章里写数学公式基本没有问题了，但是要注意：
数学公式中如果出现了连续两个{，中间一定要加空格！

## 增加建站时间
修改`/themes/matery/layout/_partial/footer.ejs`文件，在最后加上
```javascript
<script language=javascript>
    function siteTime() {
        window.setTimeout("siteTime()", 1000);
        var seconds = 1000;
        var minutes = seconds * 60;
        var hours = minutes * 60;
        var days = hours * 24;
        var years = days * 365;
        var today = new Date();
        var todayYear = today.getFullYear();
        var todayMonth = today.getMonth() + 1;
        var todayDate = today.getDate();
        var todayHour = today.getHours();
        var todayMinute = today.getMinutes();
        var todaySecond = today.getSeconds();
        /* Date.UTC() -- 返回date对象距世界标准时间(UTC)1970年1月1日午夜之间的毫秒数(时间戳)
        year - 作为date对象的年份，为4位年份值
        month - 0-11之间的整数，做为date对象的月份
        day - 1-31之间的整数，做为date对象的天数
        hours - 0(午夜24点)-23之间的整数，做为date对象的小时数
        minutes - 0-59之间的整数，做为date对象的分钟数
        seconds - 0-59之间的整数，做为date对象的秒数
        microseconds - 0-999之间的整数，做为date对象的毫秒数 */
        var t1 = Date.UTC(2017, 09, 11, 00, 00, 00); //北京时间2018-2-13 00:00:00
        var t2 = Date.UTC(todayYear, todayMonth, todayDate, todayHour, todayMinute, todaySecond);
        var diff = t2 - t1;
        var diffYears = Math.floor(diff / years);
        var diffDays = Math.floor((diff / days) - diffYears * 365);
        var diffHours = Math.floor((diff - (diffYears * 365 + diffDays) * days) / hours);
        var diffMinutes = Math.floor((diff - (diffYears * 365 + diffDays) * days - diffHours * hours) / minutes);
        var diffSeconds = Math.floor((diff - (diffYears * 365 + diffDays) * days - diffHours * hours - diffMinutes * minutes) / seconds);
        document.getElementById("sitetime").innerHTML = "本站已运行 " +diffYears+" 年 "+diffDays + " 天 " + diffHours + " 小时 " + diffMinutes + " 分钟 " + diffSeconds + " 秒";
    }/*因为建站时间还没有一年，就将之注释掉了。需要的可以取消*/
    siteTime();
</script>
```
然后在合适的地方（比如copyright声明后面）加上下面的代码就行了：
```html
<span id="sitetime"></span>
```
## 修改不蒜子初始化计数
因为不蒜子至今未开放注册，所以没办法在官网修改初始化，只能自己动手了。和上一条一样，在`/themes/matery/layout/_partial/footer.ejs`文件最后加上：
```javascript
<script>
    $(document).ready(function () {

        var int = setInterval(fixCount, 50);  // 50ms周期检测函数
        var pvcountOffset = 80000;  // 初始化首次数据
        var uvcountOffset = 20000;

        function fixCount() {
            if (document.getElementById("busuanzi_container_site_pv").style.display != "none") {
                $("#busuanzi_value_site_pv").html(parseInt($("#busuanzi_value_site_pv").html()) + pvcountOffset);
                clearInterval(int);
            }
            if ($("#busuanzi_container_site_pv").css("display") != "none") {
                $("#busuanzi_value_site_uv").html(parseInt($("#busuanzi_value_site_uv").html()) + uvcountOffset); // 加上初始数据 
                clearInterval(int); // 停止检测
            }
        }
    });
</script>
```
然后把上面几行有段代码：
```html
<% if (theme.busuanziStatistics && theme.busuanziStatistics.totalTraffic) { %>
    <span id="busuanzi_container_site_pv">
        <i class="fa fa-heart-o"></i>
        本站总访问量 <span id="busuanzi_value_site_pv" class="white-color"></span>
    </span>
<% } %>
<% if (theme.busuanziStatistics && theme.busuanziStatistics.totalNumberOfvisitors) { %>
    <span id="busuanzi_container_site_uv">
        人次,&nbsp;访客数 <span id="busuanzi_value_site_uv" class="white-color"></span> 人.
    </span>
<% } %>
```
修改为：
```html
<% if (theme.busuanziStatistics && theme.busuanziStatistics.totalTraffic) { %>
    <span id="busuanzi_container_site_pv" style='display:none'>
        <i class="fa fa-heart-o"></i>
        本站总访问量 <span id="busuanzi_value_site_pv" class="white-color"></span>
    </span>
<% } %>
<% if (theme.busuanziStatistics && theme.busuanziStatistics.totalNumberOfvisitors) { %>
    <span id="busuanzi_container_site_uv" style='display:none'>
        人次,&nbsp;访客数 <span id="busuanzi_value_site_uv" class="white-color"></span> 人.
    </span>
<% } %>
```
其实就是增加了两个`style='display:none'`而已。

## 添加动漫人物
其实三步就行了，不用像网上有些教程那么复杂。

第一步：
```bash
npm install --save hexo-helper-live2d
```
第二步：
```bash
npm install live2d-widget-model-shizuku
```
第三步：
在根目录配置文件中添加如下代码：
```json
live2d:
  enable: true
  scriptFrom: local
  pluginRootPath: live2dw/
  pluginJsPath: lib/
  pluginModelPath: assets/
  tagMode: false
  log: false
  model:
    use: live2d-widget-model-shizuku
  display:
    position: right
    width: 150
    height: 300
  mobile:
    show: true
  react:
    opacity: 0.7
```
然后`hexo g`再`hexo s`就能预览出效果了，但是有个注意的地方，我发现这个动漫人物最好不要和不蒜子同时使用，不然不蒜子会显示不出来。

## 图片添加水印
为了防止别人抄袭你文章，可以把所有的图片都加上水印，方法很简单。

首先在博客根目录下新建一个`watermark.py`，代码如下：
```python
# -*- coding: utf-8 -*-
import sys
import glob
from PIL import Image
from PIL import ImageDraw
from PIL import ImageFont


def watermark(post_name):
    if post_name == 'all':
        post_name = '*'
    dir_name = 'source/_posts/' + post_name + '/*'
    for files in glob.glob(dir_name):
        im = Image.open(files)
        if len(im.getbands()) < 3:
            im = im.convert('RGB')
            print(files)
        font = ImageFont.truetype('STSONG.TTF', max(30, int(im.size[1] / 20)))
        draw = ImageDraw.Draw(im)
        draw.text((im.size[0] / 2, im.size[1] / 2),
                  u'@yourname', fill=(0, 0, 0), font=font)
        im.save(files)


if __name__ == '__main__':
    if len(sys.argv) == 2:
        watermark(sys.argv[1])
    else:
        print('[usage] <input>')
```
字体也放根目录下，自己找字体。然后每次写完一篇文章可以运行`python3 watermark.py postname`添加水印，如果第一次运行要给所有文章添加水印，可以运行`python3 watermark.py all`。

## 添加评论插件
主题已经自带了gitalk插件了，所以你只需要去github官网配置好就行了。

首先打开[github](https://github.com/settings/applications/new)申请一个应用，要填四个东西：
```txt
Application name //应用名称，随便填
Homepage URL //填自己的博客地址
Application description //应用描述，随便填
Authorization callback URL //填自己的博客地址
```
然后点击注册，会出现两个字符串`Client ID`和`Client Secret`，这个要复制出来。

然后去主题的配置文件`_config.yml`下修改`gitalk`那里：
```txt
gitalk:
  enable: true
  owner: 你的github用户名
  repo: 你的github用户名.github.io
  oauth:
    clientId: 粘贴刚刚注册完显示的字符串
    clientSecret: 粘贴刚刚注册完显示的字符串
  admin: 你的github用户名
```
以后写文章的时候，只要在文章页面登陆过github，就会自动创建评论框，记得每次写完文章后打开博客文章页面一下。

## 添加百度统计和谷歌统计代码
首先打开[百度站长平台](https://ziyuan.baidu.com/site/index)，然后点击添加网站，输入网址并选择领域。

接下来要验证网站所有权，有三种方式，推荐采用HTML标签验证，最简单，将代码复制出来。

打开`themes/matery/layout/_partial/head.ejs`，修改下面两行：
```html
<meta name="baidu-site-verification" content="xxx" />
<meta name="google-site-verification" content="xxx" />
```
其中`content`内容改成你自己刚刚复制出来的就行了。

## 修复代码块行号不显示bug
修改`themes/matery/source/css/matery.css`第95行左右的`pre`和`code`两段改为如下代码：
```css
pre {
    /* padding: 1.5rem !important; */
    padding: 1.5rem 1.5rem 1.5rem 3.3rem !important;
    margin: 1rem 0 !important;
    background: #272822;
    overflow: auto;
    border-radius: 0.35rem;
    tab-size: 4;
}

code {
    padding: 1px 5px;
    font-family: Inconsolata, Monaco, Consolas, 'Courier New', Courier, monospace;
    /* font-size: 0.91rem; */
    color: #e96900;
    background-color: #f8f8f8;
    border-radius: 2px;
}
```
然后在根目录的`_config.yml`中设置`prism_plugin.line_number`为`true`。

## 添加雪花特效
首先在`themes/matery/source/libs/others`下新建文件`snow.js`，并插入如下代码：
```javascript
/*样式一*/
(function($){
    $.fn.snow = function(options){
    var $flake = $('<div id="snowbox" />').css({'position': 'absolute','z-index':'9999', 'top': '-50px'}).html('&#10052;'),
    documentHeight  = $(document).height(),
    documentWidth   = $(document).width(),
    defaults = {
        minSize     : 10,
        maxSize     : 20,
        newOn       : 1000,
        flakeColor  : "#AFDAEF" /* 此处可以定义雪花颜色，若要白色可以改为#FFFFFF */
    },
    options = $.extend({}, defaults, options);
    var interval= setInterval( function(){
    var startPositionLeft = Math.random() * documentWidth - 100,
    startOpacity = 0.5 + Math.random(),
    sizeFlake = options.minSize + Math.random() * options.maxSize,
    endPositionTop = documentHeight - 200,
    endPositionLeft = startPositionLeft - 500 + Math.random() * 500,
    durationFall = documentHeight * 10 + Math.random() * 5000;
    $flake.clone().appendTo('body').css({
        left: startPositionLeft,
        opacity: startOpacity,
        'font-size': sizeFlake,
        color: options.flakeColor
    }).animate({
        top: endPositionTop,
        left: endPositionLeft,
        opacity: 0.2
    },durationFall,'linear',function(){
        $(this).remove()
    });
    }, options.newOn);
    };
})(jQuery);
$(function(){
    $.fn.snow({ 
        minSize: 5, /* 定义雪花最小尺寸 */
        maxSize: 50,/* 定义雪花最大尺寸 */
        newOn: 300  /* 定义密集程度，数字越小越密集 */
    });
});
/*样式二*/
/* 控制下雪 */
function snowFall(snow) {
    /* 可配置属性 */
    snow = snow || {};
    this.maxFlake = snow.maxFlake || 200;   /* 最多片数 */
    this.flakeSize = snow.flakeSize || 10;  /* 雪花形状 */
    this.fallSpeed = snow.fallSpeed || 1;   /* 坠落速度 */
}
/* 兼容写法 */
requestAnimationFrame = window.requestAnimationFrame ||
    window.mozRequestAnimationFrame ||
    window.webkitRequestAnimationFrame ||
    window.msRequestAnimationFrame ||
    window.oRequestAnimationFrame ||
    function(callback) { setTimeout(callback, 1000 / 60); };

cancelAnimationFrame = window.cancelAnimationFrame ||
    window.mozCancelAnimationFrame ||
    window.webkitCancelAnimationFrame ||
    window.msCancelAnimationFrame ||
    window.oCancelAnimationFrame;
/* 开始下雪 */
snowFall.prototype.start = function(){
    /* 创建画布 */
    snowCanvas.apply(this);
    /* 创建雪花形状 */
    createFlakes.apply(this);
    /* 画雪 */
    drawSnow.apply(this)
}
/* 创建画布 */
function snowCanvas() {
    /* 添加Dom结点 */
    var snowcanvas = document.createElement("canvas");
    snowcanvas.id = "snowfall";
    snowcanvas.width = window.innerWidth;
    snowcanvas.height = document.body.clientHeight;
    snowcanvas.setAttribute("style", "position:absolute; top: 0; left: 0; z-index: 1; pointer-events: none;");
    document.getElementsByTagName("body")[0].appendChild(snowcanvas);
    this.canvas = snowcanvas;
    this.ctx = snowcanvas.getContext("2d");
    /* 窗口大小改变的处理 */
    window.onresize = function() {
        snowcanvas.width = window.innerWidth;
        /* snowcanvas.height = window.innerHeight */
    }
}
/* 雪运动对象 */
function flakeMove(canvasWidth, canvasHeight, flakeSize, fallSpeed) {
    this.x = Math.floor(Math.random() * canvasWidth);   /* x坐标 */
    this.y = Math.floor(Math.random() * canvasHeight);  /* y坐标 */
    this.size = Math.random() * flakeSize + 2;          /* 形状 */
    this.maxSize = flakeSize;                           /* 最大形状 */
    this.speed = Math.random() * 1 + fallSpeed;         /* 坠落速度 */
    this.fallSpeed = fallSpeed;                         /* 坠落速度 */
    this.velY = this.speed;                             /* Y方向速度 */
    this.velX = 0;                                      /* X方向速度 */
    this.stepSize = Math.random() / 30;                 /* 步长 */
    this.step = 0                                       /* 步数 */
}
flakeMove.prototype.update = function() {
    var x = this.x,
        y = this.y;
    /* 左右摆动(余弦) */
    this.velX *= 0.98;
    if (this.velY <= this.speed) {
        this.velY = this.speed
    }
    this.velX += Math.cos(this.step += .05) * this.stepSize;

    this.y += this.velY;
    this.x += this.velX;
    /* 飞出边界的处理 */
    if (this.x >= canvas.width || this.x <= 0 || this.y >= canvas.height || this.y <= 0) {
        this.reset(canvas.width, canvas.height)
    }
};
/* 飞出边界-放置最顶端继续坠落 */
flakeMove.prototype.reset = function(width, height) {
    this.x = Math.floor(Math.random() * width);
    this.y = 0;
    this.size = Math.random() * this.maxSize + 2;
    this.speed = Math.random() * 1 + this.fallSpeed;
    this.velY = this.speed;
    this.velX = 0;
};
// 渲染雪花-随机形状（此处可修改雪花颜色！！！）
flakeMove.prototype.render = function(ctx) {
    var snowFlake = ctx.createRadialGradient(this.x, this.y, 0, this.x, this.y, this.size);
    snowFlake.addColorStop(0, "rgba(255, 255, 255, 0.9)");  /* 此处是雪花颜色，默认是白色 */
    snowFlake.addColorStop(.5, "rgba(255, 255, 255, 0.5)"); /* 若要改为其他颜色，请自行查 */
    snowFlake.addColorStop(1, "rgba(255, 255, 255, 0)");    /* 找16进制的RGB 颜色代码。 */
    ctx.save();
    ctx.fillStyle = snowFlake;
    ctx.beginPath();
    ctx.arc(this.x, this.y, this.size, 0, Math.PI * 2);
    ctx.fill();
    ctx.restore();
};
/* 创建雪花-定义形状 */
function createFlakes() {
    var maxFlake = this.maxFlake,
        flakes = this.flakes = [],
        canvas = this.canvas;
    for (var i = 0; i < maxFlake; i++) {
        flakes.push(new flakeMove(canvas.width, canvas.height, this.flakeSize, this.fallSpeed))
    }
}
/* 画雪 */
function drawSnow() {
    var maxFlake = this.maxFlake,
        flakes = this.flakes;
    ctx = this.ctx, canvas = this.canvas, that = this;
    /* 清空雪花 */
    ctx.clearRect(0, 0, canvas.width, canvas.height);
    for (var e = 0; e < maxFlake; e++) {
        flakes[e].update();
        flakes[e].render(ctx);
    }
    /*  一帧一帧的画 */
    this.loop = requestAnimationFrame(function() {
        drawSnow.apply(that);
    });
}
/* 调用及控制方法 */
var snow = new snowFall({maxFlake:60});
snow.start();
```
然后在主题配置文件里`libs.js`里添加一行`snow: /libs/others/snow.js`。

在`themes/matery/layout/layout.ejs`里添加如下代码：
```html
<!-- 雪花特效 -->
<% if (theme.snow.enable) { %>
<script type="text/javascript" src="<%- theme.libs.js.snow %>"></script>
<% } %>
```
最后在主题配置文件最后添加：
```txt
# 雪花特效
snow:
  enable: true
```

> 这个特效和动漫一样，有点花里胡哨，并且可能会造成卡顿，所以我并没有开启，还是给大家一个最干净的阅读体验吧。

## 动态标签栏
在`theme/matery/layout/layout.ejs`下添加如下代码：
```html
    <script type="text/javascript"> var OriginTitile = document.title, st; document.addEventListener("visibilitychange", function () { document.hidden ? (document.title = "Σ(っ °Д °;)っ喔哟，崩溃啦！", clearTimeout(st)) : (document.title = "φ(゜▽゜*)♪咦，又好了！", st = setTimeout(function () { document.title = OriginTitile }, 3e3)) })
    </script>
```
## 常见问题及解答（FAQ）
这个章节主要更新许多同学在搭建博客的过程中咨询我的一些问题。

### 为什么本地生成完ssh key之后没有.ssh文件夹？
这是我没有想到会遇到的问题，但是很多人还是遇到了，主要问题就是在输入完`ssh-keygen -t rsa -C "xxxxxx@qq.com"`之后，很多同学没有按照提示继续输入，而是就这么结束了，然后报错了也没有发现。正确做法是按照提示，一路按回车就行了。

### 为什么代码块显示有问题？
代码要显示正确，要注意以下几个点：

- 根目录`_config.yml`文件下的`highlight`中的`line_number`要设置为`false`，因为行号有bug，当然如果你按照上面教程修复了bug，就可以改成`true`。
- 不要按照网上教程安装kramed插件，已经装了的卸载掉。
- 修改`node_modules/marked/lib/marked.js`文件中的`escape`和`em`两行（在538行左右），改成下面：
```json
escape: /^\\([`*\[\]()#$+\-.!_>])/,
em: /^\*((?:\*\*|[\s\S])+?)\*(?!\*)/,
```
### 为什么博客本地预览没问题，push到github上就显示不正常？
这个问题可能原因有很多，我暂时列举遇到的情况：

github博客的仓库名称一定要和你的github名字完全一样，比如你github名字叫abc，那么仓库名字一定要是abc.github.io。这是大多数人会犯的错误，会导致显示不正常。
更换主题之后，配置文件是修改根目录下的还是主题目录下的？
这个其实都要修改，一般也不会出现重复的属性。具体使用哪个，要看主题的源代码，如果是config.xxx那就是用的根目录配置文件，如果是theme.xxx那就用的是主题目录的配置文件。

### 在哪建立github分支？
点击仓库的`settings-branches`，右边点击`add new branch`即可。





