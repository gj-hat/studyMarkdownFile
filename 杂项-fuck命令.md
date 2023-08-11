[TOC]

## what the fuck

今天介绍给大家一款解压神器： The FUCK

linux命令敲错？

**fuck**

重写依然错误？

**fuck**

权限不够？

**fuck**

参数错误？

**fuck**

…….

总之简单来说这款”the fuck“神器是一个linux的纠错小软件。

总之出现问题了，报错了加不妨fuck以下；

## 软件

以下是作者原图：

[![img](https://raw.githubusercontent.com/nvbn/thefuck/master/example.gif)](https://raw.githubusercontent.com/nvbn/thefuck/master/example.gif)

```
➜ apt-get install vim
E: Could not open lock file /var/lib/dpkg/lock - open (13: Permission denied)
E: Unable to lock the administration directory (/var/lib/dpkg/), are you root?

➜ fuck
sudo apt-get install vim [enter/↑/↓/ctrl+c]
[sudo] password for nvbn:
Reading package lists... Done

➜ git push
fatal: The current branch master has no upstream branch.
To push the current branch and set the remote as upstream, use

    git push --set-upstream origin master


➜ fuck
git push --set-upstream origin master [enter/↑/↓/ctrl+c]
Counting objects: 9, done.
...
➜ puthon
No command 'puthon' found, did you mean:
 Command 'python' from package 'python-minimal' (main)
 Command 'python' from package 'python3' (main)
zsh: command not found: puthon

➜ fuck
python [enter/↑/↓/ctrl+c]
Python 3.4.2 (default, Oct  8 2014, 13:08:17)
...

...
```

## 安装

git安装

```
git clone https://github.com/nvbn/thefuck.git
```



ubuntu:

```
sudo apt update
sudo apt install python3-dev python3-pip python3-setuptools
sudo pip3 install thefuck
```

## 配置

```
#编辑bashrc配置文件
vim ~/.bashrc
#在文件尾加入一行给thefuck取别名fuck
eval "$(thefuck --alias fuck)"
#使生效
source ~/.bashrc
```