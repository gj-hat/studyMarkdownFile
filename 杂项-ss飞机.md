[toc]

## 购买VPS

[搬瓦工购买地址](http://www.bwh8.net/)

安装ubuntu 16.04 x64系统

## 安装pip

```
sudo apt-get install pip
```

## 安装shadowsocks

```
pip install shadowsocks
```

## 配置shadowsocks

在/etc目录下

```
vi /etc/shadowsocks.json
```

输入i开启输入模式
以下是简易配置

```
{
    "server":"$Your_IP",     //每个人的都不一样
    "local_address": "127.0.0.1",    //固定
    "local_port": 1080,    //固定
    "port_password":{      以下为多用户模式
      "8381":"密码",        
      "8382":"密码"
      },
    "timeout":600,
    "method":"aes-256-cfb"  
    "fast_open":false
}
```

## 启动

```
ssserver -c /etc/shadowsocks.json -d start
```

## 总结

各种加密方式有各种加密方式的优缺点，我倾向于chacha20；
不过采用chacha20的还需要一些配置，时间有点久了，忘的差不多了，用的时候再google吧；

手机pc端需要下载相应软件
https://shadowsocks.org/en/download/clients.html