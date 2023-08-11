[TOC]

# 利用FRP实现内网穿透（动态域名解析）

> 本文使用的是极简配置配置，如有特殊需求请参考https://github.com/fatedier/frp文献

**环境准备：外网服务器（ubuntu）+本地机器（ubuntu子系统）**

> 本地服务器windows也可以配置类似

## 什么是frp

rp 是一个高性能的反向代理应用，可以帮助您轻松地进行内网穿透，对外网提供服务，支持 tcp, http, https 等协议类型，并且 web 服务支持根据域名进行路由转发。

## 准备工作

1. 需要一台外网服务器windows或Linux（本文采用Ubuntu操作系统）
2. 需要一台或多台内网PC，indows或Linux（本文采用Ubuntu子系统）

## 软件安装

### 外网服务器端

下载ftp：
[![img](https://s2.ax1x.com/2019/02/21/kR1BV0.png)](https://s2.ax1x.com/2019/02/21/kR1BV0.png)

```
wget https://github.com/fatedier/frp/releases/download/v0.20.0/frp_0.20.0_linux_amd64.tar.gz
```



解压frp:

```
mkdir /usr/local/frp    创建frp目录
tar -zxvf frp_0.20.0_linux_amd64.tar.gz    解压frp
mv /frp_0.20.0_linux_amd64.tar.gz/* /usr/local/frp   将解压文件放入frp文件夹中
```



看一下frp的文件结构：

[![img](https://s2.ax1x.com/2019/02/21/kR1DaV.png)](https://s2.ax1x.com/2019/02/21/kR1DaV.png)
其中frps.ini和frpc.ini一个是服务器端的配置文件一个是本地端的配置文件

配置frps.ini(因为是在服务器端所以配置frps.ini当然大家也可以直接删除frpc.ini)

```
vi /usr/local/frp/frps.ini

# 星号部分为打码大家需要自行定义
[common]
bind_port = **211
log_file = /var/log/frp.log
allow_port = **212-**220

# 以下为解释
第一行[common] 默认生成可以不用管
第二行bind_port = **211 为服务器和本地PC通信用的端口
第三行log_file = /var/log/frp.log 为log文件地址
第四行allow_port = **212-**220 为端口转发地址（可以一个可以多个）
例如：外网访问gjweb.top:**212则将212端口的信息转发到于212端口连接的内网PC
```

运行：

```
./frps -c frps.ini
```



## 内网PC

### 外网服务器端

下载ftp：

```
wget https://github.com/fatedier/frp/releases/download/v0.20.0/frp_0.20.0_linux_amd64.tar.gz
```



解压frp:

```
mkdir /usr/local/frp    创建frp目录
tar -zxvf frp_0.20.0_linux_amd64.tar.gz    解压frp
mv /frp_0.20.0_linux_amd64.tar.gz/* /usr/local/frp   将解压文件放入frp文件夹中
```



配置frpc.ini(因为是在PC端所以配置frpc.ini当然大家也可以直接删除frps.ini)
[![img](https://s2.ax1x.com/2019/02/21/kR14Vx.png)](https://s2.ax1x.com/2019/02/21/kR14Vx.png)

```
vi /usr.local/frp/frpc.ini

# 星号为打码部分
[common]
server_addr = 97.64.39.132
log_file = /var/log/frpc.log
server_port = **211



[Web_80]
type = tcp
local_ip = 127.0.0.1
local_port = 80
remote_port = **212

# 解释
第一行[common]  默认
第二行server_addr = 97.64.39.132 地址为你外网服务器的IP地址
第三行log_file = /var/log/frpc.log 为日志文件
第四行与服务器进行通信的端口需要与服务器端bind_port配置一致
第五行[Web_80]中括号内可以自定义
第六行type = tcp为连接类型具体需求请参考frp文献
第七行local_ip = 127.0.0.1 本地地址
第八行local_port = 80服务器转发回本地的端口
第九行remote_port = **212使用212连接服务器与都服务器开发的端口一致allow_port的内容一致
```

运行：

```
./frpc -c frpc.ini
```



## 至此frp安装完成

例如你的本地服务器当作web服务器的话
访问服务器域名：你设置的端口 就可以正常访问了
[![img](https://s2.ax1x.com/2019/02/21/kR3pi8.png)](https://s2.ax1x.com/2019/02/21/kR3pi8.png)

## 常间问题

1. 如果遇到无法访问的情况请查看端口设置是否正确
2. 防火墙问题 使用ufw status 查看相关端口是否被禁止

## 设置后台运行

在如下位置新建frps.ini文件
[![img](https://s2.ax1x.com/2019/02/21/kR3FMj.png)](https://s2.ax1x.com/2019/02/21/kR3FMj.png)

内容如下：
[![img](https://s2.ax1x.com/2019/02/21/kR3Zd0.png)](https://s2.ax1x.com/2019/02/21/kR3Zd0.png)
其中ExecStart内容为需要执行的命令

接下来执行：

```
sudo systemctl daemon-reload
sudo systemctl status frps
```