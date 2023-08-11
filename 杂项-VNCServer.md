[TOC]

# VNCserver

**VNCserver?大家可能都不知道吧。他的功能是可以远程连接linux时候可以以
图形界面的形式操作。**

WHAT?什么？远程连接linux还要图形界面？

对没错我的心情就这这样的，工作中总能遇到”特别“的客户。

作为一个几乎连图形界面的linux都没怎么用过的人，去使用图形界面而且还远程。

两个感受：

1. 太尼玛慢了，效率怎么提高
2. 这客户很有勇气啊！！！！！！！！
3. 脸上笑嘻嘻，心里..

作为一直用惯debian系的，第一次使用redhat系，表时各种不方便。所以整整折腾了一早上。

为了以后能更快的处理这个问题，也记录一下过程吧；

## 前期准备

关闭防火墙，centos的防火墙是firewalld，关闭防火墙的命令
(虽然极其不建议关闭firewall但是能提出这样要求的客户也不在乎这个了，还能避免遇到很多问题)

```
systemctl stop firewalld.service
```



关闭selinux

```
setenforce 0
```



centos本身需要安装UI，如果没装参考下面的命令，如果已经安装跳过

```
yum groupinstall "GNOME Desktop"
```

## 安装tigervncserver

```
yum install tigervnc-server tigervnc-server-module
```

## 拷贝配置文件

> 为了以后可更改,备份一下吧；

```
cp /lib/systemd/system/vncserver@.service /etc/systemd/system/vncserver@:1.service
```

## 进入到配置文件目录

```
cd /etc/systemd/system
```

## 修改配置文件

> 这是以root用户登录的设置。不推荐但也推荐，理由同前期准备

```
$ vim vncserver@:1.service
[Unit]
Description=Remote desktop service (VNC)
After=syslog.target network.target

[Service]
Type=forking
User=root
ExecStart=/usr/bin/vncserver :1 -geometry 1280x1024 -depth 16 -securitytypes=none -fp /usr/share/X11/fonts/misc
ExecStop=/usr/bin/vncserver -kill :1

[Install]
WantedBy=multi-user.target
```

## 启用配置文件

```
systemctl enable vncserver@:1.service
```

## 设置登陆密码

```
vncpasswd
```

## 启动vncserver

```
systemctl start vncserver@:1.service
```

## 查看端口状态

```
netstat -lnt | grep 590*
```

## 查看报错信息

```
grep vnc /var/log/messages
```

## 最后

诚恳的恳求甲方，不要为难自己，不熟悉linux就尽量少用。毕竟windows 2008Server也还不错是吧。

> 感谢latest