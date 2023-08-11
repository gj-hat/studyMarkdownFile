[TOC]

## Tomcat安装

> 内容很简单，没什么需要细细琢磨的这是懒得记路径，所以也记录一下

## Tomcat下载

[TomCat下载地址](https://tomcat.apache.org/)

[Tomcat安装版](http://mirrors.tuna.tsinghua.edu.cn/apache/tomcat/tomcat-8/v8.5.37/bin/apache-tomcat-8.5.37-windows-x64.zip)

不建议使用安装版，虽然安装版不需要配置环境变量等，但是TomCat会作为进程一直在后台运行，且JavaEE项目每次运行时都得强行关闭一下进程。本文主要记录免安装版的配置，一下是免安装版的下载地址（下载前记得MD5校验）：

[TomCat免安装版](http://mirrors.tuna.tsinghua.edu.cn/apache/tomcat/tomcat-8/v8.5.37/bin/apache-tomcat-8.5.37.zip)

## 环境变量配置

1. 配置Tomcat基础目录路径
   [![img](https://s2.ax1x.com/2019/01/14/FxBdW6.png)](https://s2.ax1x.com/2019/01/14/FxBdW6.png)

```
变量名： CATALINA_BASE
变量值： D:\apache-tomcat-8.5.37
```

1. 配置TomCat家目录路径

[![img](https://s2.ax1x.com/2019/01/14/FxBhSf.png)](https://s2.ax1x.com/2019/01/14/FxBhSf.png)

```
变量名：CATALINA_HOME
变量值： D:\apache-tomcat-8.5.37
```

1. 配置系统变量PATH

[![img](https://s2.ax1x.com/2019/01/14/FxB7wj.png)](https://s2.ax1x.com/2019/01/14/FxB7wj.png)

```
%CATALINA_HOME%\lib
%CATALINA_HOME%\bin
```

以上就是Tomcat的下载和配置全过程。
安装完成后cmd下输入startup：

[![img](https://s2.ax1x.com/2019/01/14/FxBOf0.png)](https://s2.ax1x.com/2019/01/14/FxBOf0.png)

出现这样的内容表示安装成功

测试地址：[Tomcat测试连接](http://http//localhost:8080/)

附加：

如果出现这个内容表示JAVA坏境变量配置有问题。

[![img](https://s2.ax1x.com/2019/01/14/FxDikR.md.png)](https://s2.ax1x.com/2019/01/14/FxDikR.md.png)

解决办法：配置jdk坏境变量；

添加两个系统变量一个PATH变量

[![img](https://s2.ax1x.com/2019/01/14/FxDFt1.png)](https://s2.ax1x.com/2019/01/14/FxDFt1.png)

```
JAVA_HOME
C:\Program Files\Java\jdk1.8.0_111
```

和

[![img](https://s2.ax1x.com/2019/01/14/FxDkfx.png)](https://s2.ax1x.com/2019/01/14/FxDkfx.png)

```
CLASSPATH
.;%Java_Home%\bin;%Java_Home%\lib\dt.jar;%Java_Home%\lib\tools.jar
```



和

[![img](https://s2.ax1x.com/2019/01/14/FxDZ6O.png)](https://s2.ax1x.com/2019/01/14/FxDZ6O.png)

```
%Java_Home%\bin;%Java_Home%\jre\bin;
```