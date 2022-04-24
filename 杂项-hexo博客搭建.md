[TOC]

## 看着五分钟搭建hexo的教程自己折腾了近一个礼拜

**由于这个周被hexo折腾的会说脏话了，所以我决定第一篇就从搭建hexo开始说起**
**本文使用linux中基于depain分支下的ubuntu系统（理论上来说基于depain分支都可以使用）**

**本文使用github作为服务器托管我们的hexo博客**

> ### 注册github

#### [注册github](http://github.com/)

注册成功后，注册github提供给我们的域名
其实就是按照人家的要求创建一个库(项目);
要求如下：新建的库名必须是自己的github用户名.github.io

#### 创建新库

github的主页面找到这个按钮new repository（因为我已经建立了所以这个图标的位置可能和你们不太一样）

[![img](https://s1.ax1x.com/2018/11/27/FEA03T.png)](https://s1.ax1x.com/2018/11/27/FEA03T.png)

点击后，安装提示输入自己库的名称， username.github.io

这么一来自己的域名创建完毕

> ### 第二步开始安装各种巴拉巴拉的环境

#### 首先使用更新一下软件

sudo apt-get update（以后安装前为了不出现问题大家尽可能的先使用此命令）

```
sudo apt-get update
```



#### 安装git：

记得sudo一下

```
sudo apt-get install git
```



使用git –version查看自己git的版本，也可查看自己是否已经安装

```
git --version
```



安装完成后记得设置自己用户名和邮箱，用户名和邮箱和github一致即可；
设置命令如下:

```
$ git config --global user.name "John Doe"
$ git config --global user.email johndoe@example.com
```



PS:插一句哦，如果是小白不知道什么是git，简单来说git是全世界全宇宙最牛的版本控制器。
至于什么是git什么是版本控制器大家仔细baidu，google。

算了知道你们懒给你们地址吧：
[戳这里，这是git](https://git-scm.com/book/zh/)

#### 安装npm（node package manager）

直译过来是node的包管理工具

```
sudo apt-get install npm
```



**老规矩安装完成后先查看版本；**
*大家可能会说都安装了还有看什么版本有意义吗？
不不不，这还是很有意义的，按照笔者的万年遇bug的体质还是很有意义的。
我的英文水准很差，提示语句大多看不懂；所以执行完安装命令后到底有没有安装成功还是得用下面的命令来查看。万一安装失败了，那我给你讲哦接下来的大多步骤你都不知道错在哪里了；
按照流程先心态炸裂，然后离原地爆炸。然后各种卸载命令，最后一句骂娘关电脑去他爸爸的吧。*

```
sudo apt-get install npm
```

执行完成后大家就可是使用npm的包管理工具了；

#### 安装NODE.JS

知其然还要知其所以然，为什么要安装note.js呢；
根据大神的解释是这样的
hexo发布后是一个静态网页，而hexo本身并不是静态网页可以运行诸如js等页面。
这里的note.js就是将我们写的网页“翻译”成静态页面；

如果有人想细细研究的话，那就给你看看官方文档吧（总觉得这么用标签不太好）

```
Node.js是一个开放源代码、跨平台的、可用于服务器端和网络应用的运行环境。Node.js应用JavaScript语言写成，在Node.js运行时运行。它支持OS X、Microsoft Windows、Linux、FreeBSD、NonStop、IBM AIX、IBM System z和IBM i。Node.js由Node.js基金会拥有和维护，该基金会与Linux基金会有合作关系。

 Node.js提供事件驱动和非阻塞I/O API，可优化应用程序的吞吐量和规模。这些技术通常被用于实时应用程序。

 Node.js采用Google的V8引擎来执行代码。Node.js的大部分基本模块都是用JavaScript写成的。Node.js含有一系列内置模块，使得程序可以作为独立服务器运行，从而脱离Apache HTTP Server或IIS运行。

 Node.js正在向服务器端平台发展，并已被IBM、Microsoft、Yahoo!、Walmart、Groupon、SAP、LinkedIn、Rakuten、PayPal、Voxer和GoDaddy等采用

 Node.js允许通过JavaScript和一系列模块来编写服务器端应用和网络相关的应用。核心模块包括文件系统I/O、网络（HTTP、TCP、UDP、DNS、TLS/SSL等）、二进制数据流、加密算法、数据流等等。Node模块的API形式简单，降低了编程的复杂度。

 使用框架可以加速开发。常用的框架有Express.js、Socket.IO和Connect等。Node.js的程序可以在Microsoft Windows、Linux、Unix、Mac OS X等服务器上运行。Node.js也可以使用CoffeeScript（一种旨在简化JavaScript的替代语言，其代码可按照一定规则转化为合法的JavaScript代码）、TypeScript（微软开发的强化了数据类型的JavaScript变体）、Dart语言，以及其他能够编译成JavaScript的语言编程。

 Node.js主要用于编写像Web服务器一样的网络应用，这和PHP和Python是类似的。但是Node.js与其他语言最大的不同之处在于，PHP等语言是阻塞的（只有前一条命令执行完毕才会执行后面的命令），而Node.js是非阻塞的（多条命令可以同时被运行，通过回调函数得知命令已结束运行）。

 Node.js是事件驱动的。开发者可以在不使用线程的情况下开发出一个能够承载高并发的服务器。其他服务器端语言难以开发高并发应用，而且即使开发出来，性能也不尽人意。Node.js正是在这个前提下被创造出来。Node.js把JavaScript的易学易用和Unix网络编程的强大结合到了一起。

 Node.js使用Google V8 JavaScript 引擎，因为：

 V8是基于BSD许可证的开源软件
 V8速度非常快
 V8专注于网络功能，在HTTP、DNS、TCP等方面更加成熟
 Node.js已经有数十万模块，它们可以通过一个名为**npm**的管理器免费下载。Node.js开发社 区主要有两个邮件列表、一个在freenode的名为#node.js的IRC频道。社区集中在NodeConf
```



**安装命令如下**

```
npm install note.js
```



（插曲一下，本人当时安装的时候一直卡在这里不动，这个时候别傻傻的一直等，人是活的总要变通一下吧）

[![img](https://s1.ax1x.com/2018/11/27/FEZeHJ.png)](https://s1.ax1x.com/2018/11/27/FEZeHJ.png)

这是网络问题不是所有人都有，但如果有了就很mmp，解决的办法就是”换源“使用国内的taobao源

```
npm --registry=https://registry.npm.taobao.org install -g cnpm
```



接下来用cnpm替换上面的npm，再执行一遍

[![img](https://s1.ax1x.com/2018/11/27/FEZGuD.png)](https://s1.ax1x.com/2018/11/27/FEZGuD.png)

（它动了，真的动了，真酷）接下来执行下面的命令安装

```
npm install note.js
```



#### 安装hexo

### 啊终于到了安装 hexo的地步了为了庆祝之前大家受的苦我决定用标题标签来见证这一刻

安装命令来了

```
npm install hexo
```



老规矩如果超过三秒它还不动，那只能宣告它死亡了；

既然费了老大功夫换了taobao源总得用一下吧

```
cnpm install hexo
```



**不信你看，等了半小时就等来一个空行，换了cnpm秒开**

[![img](https://s1.ax1x.com/2018/11/27/FEZj8x.png)](https://s1.ax1x.com/2018/11/27/FEZj8x.png)

#### 初始化hexo

软件装好了，博客总得有地方放吧。

接下来使用各种cd mkdir 巴拉巴拉的命令定位到自己想安装的目录下，执行下面的初始化命令

```
hexo init
```

[![img](https://s1.ax1x.com/2018/11/27/FEensS.png)](https://s1.ax1x.com/2018/11/27/FEensS.png)

接下来如果不出意外的话大家可以看到该目录下有各种各样的文件了。（下篇文章介绍这些目录的作用）如果出了意外的话，比如等可久，这个时候就不能换源了，这个时候就需要走代理了
代理软件prochains大家自行baidu，google。奥对了，之后还会有教程，教大家搭建ssvpn是不是很激动呀。就是传说中的小飞机。

哦对了，如果中途等不及了又或者不小心按了ctrl+c的话，重新执行hexo init会报错。
解决办法就是删除文件夹内所有文件记得是所有哦，包括隐藏文件（PS:使用ll命令可以查看，rm可以删除）

[![img](https://s1.ax1x.com/2018/11/27/FEeOeg.png)](https://s1.ax1x.com/2018/11/27/FEeOeg.png)

等待总是漫长的，不如喝杯java？

*像这样？*
[![img](https://s1.ax1x.com/2018/11/27/FEm9S0.jpg)](https://s1.ax1x.com/2018/11/27/FEm9S0.jpg)
*像这样？？*
[![img](https://s1.ax1x.com/2018/11/27/FEmiOU.jpg)](https://s1.ax1x.com/2018/11/27/FEmiOU.jpg)
*又或者这样？*
[![img](https://s1.ax1x.com/2018/11/27/FEmAw4.png)](https://s1.ax1x.com/2018/11/27/FEmAw4.png)

### 初始化完成，纪念一下漫长的等待老规矩上标题标签

[![img](https://s1.ax1x.com/2018/11/27/FEm8TH.png)](https://s1.ax1x.com/2018/11/27/FEm8TH.png)

初始化完成后看到的就是这样的东西

接下来终于可以看到界面化的东西了

开始一套操作

```
hexo clean
hexo g
hexo s
```

终于要结束了。
[![img](https://s1.ax1x.com/2018/11/27/FEma1P.png)](https://s1.ax1x.com/2018/11/27/FEma1P.png)

哎哟卧槽，纳尼，报错了

别急别急 ，好像少装了server插件

命令来啦

npm hexo -s

好嘞接下来一套骚操作:

```
hexo clean
hexo g
hexo s
```



[![img](https://s1.ax1x.com/2018/11/27/FEnkgP.png)](https://s1.ax1x.com/2018/11/27/FEnkgP.png)

安装提示浏览器输入：[http://127.0.0.1:4000](http://127.0.0.1:4000/)
[![img](https://s1.ax1x.com/2018/11/27/FEnYbF.png)](https://s1.ax1x.com/2018/11/27/FEnYbF.png)

**别惊讶为什么我的和你们的不一样，我花了三天时间换主题容易嘛我，还不让我装个逼了（装逼得加粗）**

> ### hexo安装没毛病了,开始得先办法怎么让别人也能看到了

#### github

大家还记得一开始的github吗？再不介绍都忘了。

什么是github？简单说就是传说中的全球最大的同性交友网站。
为什么这么说呢，因为这是一个代码交流的地方因为男性居多所以这么叫。

大家的交流方式就是代码库，奥对了linux的源码也在里面发布
[![img](https://s1.ax1x.com/2018/11/27/FEQWef.jpg)](https://s1.ax1x.com/2018/11/27/FEQWef.jpg)

#### 创建ssh密钥（为了安全，安全 ，安安全）

命令来了：

```
ssh-keygen
```



根据提示找到id_dsa.pub这个文件

使用cat命令查看并ctrl+c复制：

```
cat id_dsa.pub
```



粘贴到
[![img](https://s1.ax1x.com/2018/11/27/FElupd.png)](https://s1.ax1x.com/2018/11/27/FElupd.png)
[![img](https://s1.ax1x.com/2018/11/27/FElM6I.png)](https://s1.ax1x.com/2018/11/27/FElM6I.png)
[![img](https://s1.ax1x.com/2018/11/27/FEl878.png)](https://s1.ax1x.com/2018/11/27/FEl878.png)

#### 接下来修改根目录下配置文件_config.yml信息

```
deploy:
type: git
repo: <repository url>
branch: [branch]
message: [message]
```

| 参数    | 描述                                                         |
| :------ | :----------------------------------------------------------- |
| repo    | 库（Repository）地址                                         |
| branch  | 分支名称。如果您使用的是 GitHub 或 GitCafe 的话，程序会尝试自动检测。 |
| message | 自定义提交信息                                               |

我的是这么写的
[![img](https://s1.ax1x.com/2018/11/27/FE1wPe.png)](https://s1.ax1x.com/2018/11/27/FE1wPe.png)

接下来使用一系列命令

```
git status -s
git add .
git commit -m "src commit master"
hexo d
```



**上传至github库中，接下来就可以使用user.github.io访问了**