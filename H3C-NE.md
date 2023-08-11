



# 01_小东西

## 1.1 网络分类

1. LAN Local Area Network   局域网

   由用户使用私有地址自行建设的内部网络

2. MAN Metropolitan Area Network  城域网

   由运营商或大规模企业建设,连接城市范围内的网络

3. WAN Wide Area Network   广域网

   运营商建设,连接全国各个城域网的网络  又称骨干网,核心网,传输网

   <img src="https://raw.githubusercontent.com/gj-hat/drawing-bed/master/blog/Snipaste_2023-07-12_14-41-30.png" alt="Snipaste_2023-07-12_14-41-30" style="zoom:50%;" />

 

## 1.2 网络的拓扑

1. 总线型: 几乎不用了  总线断了 网络就故障  
2. 环型：几乎不用  一个线路断了 网络就故障
3. 树型、星型：  父(中央)节点故障网络则故障， 树型可以理解为多层的星型结构  一般不区分
4. 全网状:  成本高
5. 部分网状:  成本低

<img src="https://raw.githubusercontent.com/gj-hat/drawing-bed/master/blog/Snipaste_2023-07-12_15-48-50.png" alt="Snipaste_2023-07-12_15-48-50" style="zoom:50%;" />

## 1.3 电路(专线)交换和分组交换

1. 电路交换(专线): 基于电话网的电路交换
   + 优点: 延迟小, 透明传输
   + 缺点: 宽带固定  网络资源利用率低  初始建立连接慢
2. 分组交换: 以分组为单位存储转发 需要使用交换机
   + 优点: 多路复用  网络资源利用率高
   + 缺点: 延迟大 实时性差  设备功能复杂 



## 1.4 交换机和路由器的使用特点

1. 多个PC之间使用交换机连接组成一个网络范围  即网段

   注意 二层交换机 是不可以连接不同网段设备的

2. 多个网络范围(网段)使用路由器连接, 组成一个更大的网络



## 1.5 冲突域广播域

**同一个冲突域之间的设备可以互相访问,** 但是需要借助一些协议 比如CSMA/CD等等

**同一个广播域之间的设备可以互相访问,**  使用交换机的Vlan或者路由器就可以做到

**交换机和路由器都不属于一个冲突域:**

即 对报文的发送有路由和交换  而不是简单的复制

**交换机各个端口之间是一个广播域:** 

 即一个端口发送消息 所有端口都可以收到. 但是也有例外 比如配置了vlan 

**路由器各个端口之间不是一个广播域:** 

一般情况下路由器的各个端口连接的是不同网段的设备,所以无法广播.  但是如果连接的是同一网段就可以广播





# 02_两种模型和数据传输

## 2.1 OSI模型 (有点过时了)

> IP用来寻找大致位置   MAC寻址用于定位具体的设备  567三层是程序员干的事情 不是网络工程师干的

1. 物理层: 电压 接口 线缆 传输距离  传输介质等物理参数
2. 数据链路层: MAC地址寻址
3. 网络层: IP地址寻址,路由
4. 传输层: 数据分段(拆分数据段) 建立端到端的连接,维护传输可靠性.  端口用于区分不同的应用程序.  TCP/UDP
5. 会话层: 建立维护拆除应用程序间的会话 ,  区分应用程序的不用访问者
6. 表示层: 定义数据格式,结构. 加密,压缩
7. 应用层: 为应用程序进程 提供网络服务

**遇到的问题**

1. 划分的层次太多。 会话层表示层意义不大
2. 网络层几乎被IP协议完全取代



## 2.2 TCP/IP

### 2.2.1 四次划分方法（课本上常见）

1. 网络接口层： 包含 物理层和数据链路层
2. 网络层
3. 传输层
4. 应用层： 包含 会话层 表示层 应用层



### 2.2.2 五层划分方法（厂商遵守的划分）

1. 物理层
2. 数据链路层
3. 网络层
4. 传输层
5. 应用层： 包含  会话层 表示层 应用层





## 2.3 数据的封装和解封装

### 2.3.1 定义

1. 封装： 在原始数据上加上一些额外信息形成的新格式
2. 解封装： 拆除封装的额外信息，还原原始数据

### 2.3.2 TCP/IP分层封装

1. 物理层: 比特流
2. 数据链路层: 数据帧
3. 网络层: 数据包
4. 传输层: 数据段
5. 应用层：数据



### 2.3.3 数据封装和解封装的过程

<img src="https://raw.githubusercontent.com/gj-hat/drawing-bed/master/blog/Snipaste_2023-07-12_16-55-17.png" alt="Snipaste_2023-07-12_16-55-17" style="zoom:75%;" />



# 03_局域网基本



## 3.1 使用的协议和线缆

1. 物理层: 双绞线  同轴线缆  光纤 无线电
2. 数据链路层: 以太网  令牌环(淘汰 )     FDDI(淘汰)

3. 网络层:  IP    IPX (淘汰) Apple talk(淘汰)



### 3.1.1 双绞线

**线序:** 凹槽朝上(朝自己)  从左到右  真正使用到的只有四跟  1,3和2,6  一般分别两两一对,一组发送一组接收

1. T568A: 白绿、绿、白橙、蓝、白蓝、橙、白棕、棕 
2. T568B: 白橙、橙、白绿、蓝、白蓝、绿、白棕、棕



**线型:**  华三设备已经可以支持自适应线序 不用管了

1. 直连线:  异类直连  两端线序一致
2. 交叉线: 同类交叉  两端线序不一致
3.  口诀同列交叉,异类直连   比如交换机连交换机属于同类  交换机连路由器属于异类



### 3.1.2 光纤

1. 多模光纤: 纤芯粗 传递多种光源 距离短 成本低
2. 单模光纤:   纤芯细  一种光源  距离长  成本高



### 3.3 局域网设备

> 名词解释
>
> 冲突域: 设备发送数据会产生冲突的网络范围
>
> CSMA/CD: 带冲突检测的载波侦听的多路访问

1. 集线器:(淘汰) 内部为总线型拓扑  同一时间只能有一个主机占用总线  所有设备处于一个冲突域  工作在物理层没有寻址能力,所以数据泛洪式转发
2. 交换机:  内部每两个接口都有一个独立的线路,每个接口都是独立的冲突域。 工作在2层，基于mac寻址，数据可单点转发



# 04_广域网

 

## 4.1 使用的协议和线缆

1. 物理层: 
   + 串行线缆:   电话线 很少用了
   + 光纤:  EPON  已经是主流了
2. 数据链路层:
   + HDLC  淘汰
   + 帧中继 淘汰
   + PPP :  点到点协议
   + 以太网:  
3. 网络层
   + IP  只能选IP协议



## 4.2 连接方式

1. 电路交换:  PSTN    ISDN(淘汰)
2. 分组交换:   几乎已经被淘汰了
3. ADSL:   淘汰
4. EPON : 目前最主流     



## 4.3 广域网接入分类

1. 接入到互联网  这个比较简单 接一个运营商就行
2. 远程连接到分支机构
   + 高速专线
   + VPN



# 05_IP协议



## 5.1 作用

1. 标识节点和链路
2. 寻址和转发
3. 适应各种数据链路:  异种组网可能会不同  比如PPP 以太网等等

 

## 5.2 IP头

>如果两个网络采用的协议不一样而导致的MTU不一致  比如一侧是以太网(1500字节) 另一侧 PPPoE(1492字节)
>
>则会对包进行分片发送

 <img src="https://raw.githubusercontent.com/gj-hat/drawing-bed/master/blog/Snipaste_2023-07-14_22-22-17.png" alt="Snipaste_2023-07-14_22-22-17" style="zoom:75%;" />

1. Version 版本
2. IHL  头部长度 描述了数据包头的内容长度
3. TOS  服务类型 用于标识DCSP或IP的优先级 用于QOS识别 数字越大 越优先
4. ToTal length 数据包总长度
5. Identification  标识符 标识某个分片来自于那个数据包

 <img src="https://raw.githubusercontent.com/gj-hat/drawing-bed/master/blog/Snipaste_2023-07-14_22-28-37.png" alt="Snipaste_2023-07-14_22-28-37" style="zoom:75%;" />





## 5.3 IP地址划分

<img src="https://raw.githubusercontent.com/gj-hat/drawing-bed/master/blog/Snipaste_2023-07-14_23-21-12.png" alt="Snipaste_2023-07-14_23-21-12" style="zoom:75%;" />

<img src="https://raw.githubusercontent.com/gj-hat/drawing-bed/master/blog/Snipaste_2023-07-14_23-21-45.png" alt="Snipaste_2023-07-14_23-21-45" style="zoom:75%;" />



## 5.4 几个IP的延申协议



### 5.4.1 ARP协议

**定义:**  

地址解析协议:  用于IP地址和MAC地址的转化

**工作原理:**

![Snipaste_2023-07-15_00-26-31](https://raw.githubusercontent.com/gj-hat/drawing-bed/master/blog/Snipaste_2023-07-15_00-26-31.png)



相关命令:

``` shell
// 查询arp缓存
arp -a
// 清空arp缓存
arp -d
```



### 5.4.2 RARP逆向ARP

**定义:**

逆向地址解析协议    用于根据本机自己的MAC地址,查询本机自己的IP地址

网络工作站使用的比较多





### 5.4.3 ICMP协议

网络控制消息协议

**常见应用:**

Ping:  不解释了

Tracert:   路由跟踪  可以跟踪路由经过了那些路径

**命令:**

H3C设备开启路由跟踪功能需要两条前置命令:

``` 
ip ttl-expires enable
ip unreachables enable
```



### 5.4.4 IP数据转发原理

> 如果目的IP和本机IP是一个网段 则直接查询目的的IP地址 并进行封装  (这一步包括封装IP和MAC)
>
> 如果不是同一个网段   那么 IP封装的还是目的IP  但是 MAC封装的是网关的MAC 然后由网关进行转发,并重新封装新的MAC地址

<img src="https://raw.githubusercontent.com/gj-hat/drawing-bed/master/blog/Snipaste_2023-07-15_01-28-11.png" alt="Snipaste_2023-07-15_01-28-11" style="zoom:75%;" />

1. `1.1.1.1`要发送到`2.2.2.2` 三层封装的目的ip是`2.2.2.2` 但是由于路由器有广播隔离即查询不到`2.2.2.2`的MAC地址.  那么二层封装的其实是网关`1.1.1.2`的地址
2. 数据进入到路由器中  由路由器拆包(拆二层的头) 发现目的是`2.2.2.x`网段  则内部转发到2号口
3. 然后再通过ARP协议 查询到`2.2.2.2`的MAC地址  由交换机发送到目的PC

**注意:** 路由器只做大致的网段转发, 具体的转发是由交换机根据MAC进行转发的



# 06_路由器和交换机入门

 

## 6.1 路由器

**作用:**

1. 连接具有不同介质的链路  比如使用串口连接则是PPP  双绞线光纤就是以太网  可以无视这些介质的不同
2. 连接网络或者子网  天生隔离广播域
3. 对数据报文执行寻路和转发
4. 交换和维护路由信息

华三的路由器设备:

1. CR系列  核心路由器
2. SR系列  高端路由器
3. MSR系列  一般是中型企业
4. ER系列 家用或者企业用



## 6.2 交换机

**作用:**

1. 连接多个以太网物理段, 隔离冲突域
2. 对以太网帧进行告诉而透明的交换转发
3. 自行学习和维护MAC地址信息

 特点:

1. 工作在一二两层
2. 只有以太网组网时候才由交换机 即 提供以太网间的透明桥接和交换
3. 一句链路层的MAC地址,将以太网数据帧在端口间进行转发



普及度: 企业内网几乎只用二层和三层交换机

+ 路由器贵 接口少
+ 一般路由器用作广域网连接  例如和远程分支机构对接VPN, 或对接其他广域网
+ 实际上 接入层使用二层交换机  核心汇聚三层交换机 出网时候使用防火墙





# 07_命令行入门

> 可以通过 Console AUX Telnet和 SSH 等 连接设备
>
> 使用命令行的方式进行配置权限的分级控制

``` shell


/*****************************路由器****************************************/
display ip interface brief // 查看接口的状态和ip等信息
ip address xxx.xxx.xxx.xxx xx // 进入接口后 给接口配置ip



/*****************************二层交换机***************************************/



/*****************************通用****************************************/
interface g0/0  // 进入接口
display current-configuration(dis cu) //查看当前的所有配置 
display this // 查看当前视图下 是如何配置的
sysname R1 // 修改名称
reboot // 重启  用户视图下
save  // 保存  即重启后还在
reset saved-configuration // 清空所有配置  不会影响设备当前的状态 重启后生效  用户视图
undo    // 在命令前undo  撤销此条配置
shutdowm   // 关闭接口  
undo shutdown  // 开启接口


```



## 7.1 文件系统配置加载

<img src="https://raw.githubusercontent.com/gj-hat/drawing-bed/master/blog/Snipaste_2023-07-15_15-12-12.png" alt="Snipaste_2023-07-15_15-12-12" style="zoom:60%;" />

配置文件选择顺序：

1. 如果用户指定了启动配置文件，且配置文件存在则以用户为准
2. 如果不存在 则以空配置启动

``` shell
save  // 保存配置
reset saved-configuration // 清空配置
backup startup-configuration to ftp-server[dest-filename]  // 备份下次启动的配置
restore startup-configuration to ftp-server[dest-filename]  // 恢复下次启动的配置
startup saved-configuration [configuration file]  // 指定下次启动时候加载的配置
display saved-configuration // 查看当前保存的配置
display startUP  // 查看系统的启动配置文件
display current-configuration   // 查看当前生效的配置  还没有保存的 也能看到
```



## 7.2 telnet开启

``` shell
// 开启telnet
[h3c] telnet server enable // 1. 开启telnet
[h3c] local-user guojia class manage // 2. 创建一个xxx的用户 类型是管理员
[h3c-luser-manage-guojia]password simple 123455 // 3. 配置一个密码 123456
[h3c-luser-manage-guojia]service-type telnet // 4. 设置这个用户的服务类型为telnet
[h3c-luser-manage-guojia]authorization-attribute user-role level-15 // 5. 配置用户权限为最高级别 15级
[h3c-luser-manage-guojia]quit // 6. 退出用户视图
[h3c]user-interface vty 0 4 // 7. 创建进入用户虚拟视图  并允许这个用户只能创建5个虚拟视图(5个tlenet连接)
authentication-mode xxx // 8. 设置进入用户虚拟视图的方式 有三种 无密码、密码、用户名+密码
[R1-line-vty0-4]user-role level-15
```



## 7.3 开启ftp

``` shell
// 开启ftp
[h3c] ftp server enable // 1. 开启ftp
[h3c] local-user xxx class manage // 2. 创建一个xxx的用户 类型是管理员
[h3c] password simple 123455 // 3. 配置一个密码 123456
[h3c] service-type ftp // 4. 设置这个用户的服务类型为ftp
[h3c] authorization-attribute user-role level-15 // 5. 配置用户权限为最高级别 15级
quit // 6. 退出用户视图
/*********************ftp命令**********************/
[h3c] ftp xxx.xxx.xxx.xxx  //连接ftp服务器  并输入用户名密码
[h3c] get [filename]  // 下载
[h3c] put [filename]  // 上传
```



# 08_基本调试

## 8.1 网络设备的引导流程

<img src="https://raw.githubusercontent.com/gj-hat/drawing-bed/master/blog/Snipaste_2023-07-15_15-40-38.png" alt="Snipaste_2023-07-15_15-40-38" style="zoom:60%;" />

## 8.2 系统调试的基本操作

``` shell
/*********** 尽量不要用 网络开销太大容易崩   打开调试 在用户视图下  *****************/
<h3c> terminal monitor // 1. 开启控制台对系统信息的监视功能
<h3c> terminal debugging //2. 打开调试信息的屏幕输出开关
<h3c> debugging 调试的服务  // 3. 打开服务模块的调试开关
<h3c> display debugging  // 4. 显示调试开关
<h3c> undo debugging all // 结束所有调试  ctrl+o  同理
```



# 09_交换机相关



## 9.1 以太网

**定义**

传输标准Ethernet II 类型帧的网络

**特征**

多路访问, 广播式的网络  (例如PPP点对点就不是多路访问)

**MAC地址**

1. 每台网络设备生产时就写入的一个全球唯一的物理地址
2. 48为长度, 16进制格式地址
3. 前24位为厂商标识OUI  后24是设备标识 

**以太网帧格式**

目的MAC 源MAC 服务和类型 DATA 帧校验序列和

**共享式以太网**

+ 所有终端主机都域中,局域网中所有接入终端共享总线的带宽 <img src="https://raw.githubusercontent.com/gj-hat/drawing-bed/master/blog/Snipaste_2023-07-15_16-09-13.png" alt="Snipaste_2023-07-15_16-09-13" style="zoom:40%;" />在一个冲突

**交换式以太网**

+ 使用交换机网络网桥进行组网每个端口都是独立的冲突域 终端主机独占端口的带宽 <img src="https://raw.githubusercontent.com/gj-hat/drawing-bed/master/blog/Snipaste_2023-07-15_16-10-38.png" alt="Snipaste_2023-07-15_16-10-38" style="zoom:40%;" />, 交换机的

## 9.2 交换机的数据转发原理

<img src="https://raw.githubusercontent.com/gj-hat/drawing-bed/master/blog/Snipaste_2023-07-15_16-17-23.png" alt="Snipaste_2023-07-15_16-17-23" style="zoom:60%;" />

<img src="https://raw.githubusercontent.com/gj-hat/drawing-bed/master/blog/Snipaste_2023-07-15_16-18-12.png" alt="Snipaste_2023-07-15_16-18-12" style="zoom:60%;" />



## 9.3 VLAN

### 9.3.1 定义

> 主要目的是隔离广播域 至于是否隔离通讯是次要功能  一般情况下 是在三层做的

虚拟局域网, 用来在二层网络中**隔离广播域**

不同的VLAN的设备在二层网络中无法互相通讯  (二层虽然不行,但是三层可能是可以的)



### 9.3.2 VLAN转发过程 

> 原始帧格式是Ethernet II    打上VLAN标签后就是802.1Q帧  但是只有交换机可以识别802.1Q帧 PC则无法识别

<img src="https://raw.githubusercontent.com/gj-hat/drawing-bed/master/blog/Snipaste_2023-07-15_16-37-43.png" alt="Snipaste_2023-07-15_16-37-43" style="zoom:60%;" />

### 9.3.3 VLAN类型

<img src="https://raw.githubusercontent.com/gj-hat/drawing-bed/master/blog/Snipaste_2023-07-15_16-39-56.png" alt="Snipaste_2023-07-15_16-39-56" style="zoom:60%;" />

### 9.3.4 交换机的端口类型

1. Access端口:
   + 必须且只能加入一个缺省Vlan .  从access端口收到的帧会打上该端口的vlan标签;从access端口发出的帧会剥离vlan标签
   + 一般用来连接PC或路由器  因为从access端口出来的帧都没vlan标签即为Ethernet II数据帧. 802.1Q只能交换机才能识别
   + 华三设备默认所有端口都是access类型且属于vlan1  华为是hybrid
2. Trunk端口
   + 可以允许多个vlan的数据通过(即可以配置多个vlan)  从trunk端口发出的数据会保留vlan标签. 如果是缺省vlan标签则也会剥离vlan标签  ,  如果帧没有vlan标签则会打上缺省默认的vlan标签
   + 一般用来连接交换机
3. Hybrid端口
   + 可以允许多个vlan的数据通过; 可以手动配置从hybrid端口发出的数据帧,哪个保留vlan标签那个剥离vlan标签;  hybtid收到未打vlan标签的e帧 会重新打上缺省的vlan标签
   + 既可以连接PC 路由器 也可以连接交换机

### 9.3.5 PVID

**定义:**

标识某个端口的缺省vlan

**特征:**

1. access所属的端口vlan就是pvid  不配置的话默认是vlan1
2. trunk端口需要手动配置pvid  默认是vlan1
3. hybrid端口需要手动配置pvid  默认是vlan1



### 9.3.6 vlan相关命令

``` shell
[h3c] vlan 数字   // 创建并进入vlan视图
[h3c-vlan-xx] name 'text'  // 重命名
[h3c-vlan-xx] port '接口'  // 把端口以access类型加入到vlan中  直接在vlan中加入接口
[h3c-g0/0/1] port 'access/trunk/bybrid' vlanID  // 和上面的二选一就行 
[h3c-g0/0/1] port link-type 'access/trunk/bybrid'  // 配置端口类型
[h3c-g0/0/1]port trunk permit vlan 'vlan-id-list/all'   // 配置trunk允许的vlan集合
[h3c] display vlan 'vlan id'   // 查看vlan的详细信息
[h3c] display vlan brief // 查看vlan的摘要信息
[h3c] display port trunk // 查看trunk端口信息
[h3c-g0/0/1] undo port link-type   // 还原交换机的默认端口类型
```



## 9.4 STP生成树协议(过时)

### 9.4.1 前置知识

**目的:**

由于二层环路会引起广播风暴MAC地址表震荡, 所以STP生成树协议主要是用来解决二层环路的问题

**简要概括如何实现:**

+ 通过阻断冗余链路来消除桥接网络中可能存在的路径回环
+ 当前路径发生故障的时候, 激活冗余备份链路, 恢复网络连通性

<img src="https://raw.githubusercontent.com/gj-hat/drawing-bed/master/blog/Snipaste_2023-07-15_23-49-31.png" alt="Snipaste_2023-07-15_23-49-31" style="zoom:40%;" />

 **STP相关概念:**

<img src="https://raw.githubusercontent.com/gj-hat/drawing-bed/master/blog/Snipaste_2023-07-16_00-03-20.png" alt="Snipaste_2023-07-16_00-03-20" style="zoom:60%;" />



### 9.4.2 STP的选举机制

<img src="https://raw.githubusercontent.com/gj-hat/drawing-bed/master/blog/Snipaste_2023-07-16_00-04-19.png" alt="Snipaste_2023-07-16_00-04-19" style="zoom:60%;" />

**举例如下:**

1. 蓝框选出了根交换机
2. 绿点选出了非根交换机的根端口
3. 橙点选出了每个物理段的指定端口
4. 最后没有选中的端口被阻塞即红点

<img src="https://raw.githubusercontent.com/gj-hat/drawing-bed/master/blog/Snipaste_2023-07-16_00-07-18.png" alt="Snipaste_2023-07-16_00-07-18" style="zoom:33%;" />

### 9.4.3 STP的初始化流程

根据交换机的端口状态来分析流程

1. **disable: 禁用状态**   刚开机 端口还没有被启用
2. **blocking: 阻塞状态**   接收BPDU 但不发送BPDU 不学习MAC地址 不转发数据
   + 交换机刚开机 所有端口一起进入到blocking状态  一瞬间后所有端口进入下面的listening状态
3. **listening: 监听状态**  接收发送BPDU 不学习MAC地址  不转发数据  固定15秒
   + 上面的选举机制就在这一步 并固定持续15秒   
   + 未选中的端口进入上面的blocking状态
4. **learning: 学习状态:** 接收发送BPDU 学习MAC地址  不转发数据  固定15秒
   + 在这一步骤专门用来学习MAC地址
5. **forwarding:  转发状态**   接收发送BPDU 学习MAC地址  转发数据



### 9.4.4 STP的不足

**STP三种计时器:**

<img src="https://raw.githubusercontent.com/gj-hat/drawing-bed/master/blog/Snipaste_2023-07-16_00-22-18.png" alt="Snipaste_2023-07-16_00-22-18" style="zoom:60%;" />

**STP的拓扑变更机制:**

>  根据选举和STP计时器可以计算出STP协议的收敛速度大致为30-50秒左右, 但是交换机的MAC表老化时间是300. 所以最终完成一次拓扑变更的时间将会在五分钟左右  所以引入了下面的拓扑变更机制

<img src="/Users/jia-guo/Desktop/网络/Snipaste_2023-07-16_00-30-00.png" alt="Snipaste_2023-07-16_00-30-00" style="zoom:60%;" />

**STP的不足:**

1. 收敛速度慢  故障切换时间长
2. 网络中大量主机频繁上下线 会导致TCN BPDU以及TC置位BPDU大量发送



### 9.4.5 STP的改进协议

> 这里不做具体描述  都是基于STP协议的 重点应放在上面的STP选举, 计时器和和拓扑变更等

<img src="https://raw.githubusercontent.com/gj-hat/drawing-bed/master/blog/Snipaste_2023-07-16_00-38-17.png" alt="Snipaste_2023-07-16_00-38-17" style="zoom:60%;" />



### 9.4.6 STP的配置

> STP协议在交换机上是自动运行的一般情况下 不需要动  但是也可以做一些调整 比如优先级 边缘端口等

``` shell
[h3c] stop global enable // 开启设备的stp特性  一般是自动开启的
[h3c-eth1/0/1] undop stp enable  // 关闭端口的STP特性
[h3c] stp mode {stp|rstp|pvst|mstp}  // 配置stp的工作模式
[h3c] stp [instance 'instance-id'] priority 'proity'  // 配置当前设备的优先级
[h3c-eth1/0/1] stp edged-port   // 配置端口为边缘端口
[h3c] display stp
[h3c] display stp root
[h3c] display stp brief
```



## 9.5 交换机的端口安全

### 9.5.1 802.1X

**解决的问题是:**  接入安全认证

<img src="https://raw.githubusercontent.com/gj-hat/drawing-bed/master/blog/Snipaste_2023-07-16_01-03-58.png" alt="Snipaste_2023-07-16_01-03-58" style="zoom:33%;" />

**有两种认证方式:**

1. **本地认证:**  在交换机内 直接对客户端进行认证
2. **远程集中认证:** 由远程认证服务器对客户端进行认证

**端口接入有两种可以控制的地方:**

1. 基于端口的认证  即这个端口下的所有设备都可以访问网络
2. 基于MAC的认证  即只要是这个MAC地址  管你那个端口都可以

**基本配置:** 这里配置的太简单了 SE还会详细讲

``` shell
[h3c] dot1x  // 默认802.1x是关闭的 使用这个命令开启全局802.1x
[h3c ge1/1/1] dot1x  // 开启端口的802.1x  建立在上面全局的基础上 
/*************添加本地接入用户并配置一些参数*********************/
[h3c] local-user 'username' class network   // 新建并进入用户中  用户属于的组是网络
[h3c xxx这个网络用户的视图] service-type lan-access  // 设置服务类型为 局域网访问
[h3c xxx这个网络用户的视图] password {cipher | simple} 'password'  // 密码
```





### 9.5.2 端口隔离

解决的问题是:  同一个交换机的不同端口之间不能互通  (当然是在同一个vlan的前提下进一步做隔离)

<img src="https://raw.githubusercontent.com/gj-hat/drawing-bed/master/blog/Snipaste_2023-07-16_01-14-35.png" alt="Snipaste_2023-07-16_01-14-35" style="zoom:40%;" />

**基本配置:**   

>  注意加入到隔离组内的端口之间是不能访问的 
>
>  即 相同隔离组不能访问  不同隔离组 可以访问

``` shell
[h3c] port-isolate group 1 //创建一个隔离组  
[h3c ge 1/1/1/1] port-isolate enable group 1   // 将端口加入到隔离组
```



### 9.6 链路聚合

> ne阶段主要是静态聚合

<img src="https://raw.githubusercontent.com/gj-hat/drawing-bed/master/blog/Snipaste_2023-07-16_02-13-00.png" alt="Snipaste_2023-07-16_02-13-00" style="zoom:60%;" />

**配置:**

<img src="https://raw.githubusercontent.com/gj-hat/drawing-bed/master/blog/Snipaste_2023-07-16_02-13-56.png" alt="Snipaste_2023-07-16_02-13-56" style="zoom:60%;" />



``` shell
[h3c] interface bridge-aggregation 'number'   // 创建一个聚合组 
[h3c g1/1/1/] port link-aggregation 'number'    // 将这个接口加入到聚合组中
[h3c] display link-aggregation summart // 查看聚合状态  这里主要看选中的接口和未选中的接口 后者一定要是0  前者就是聚合的端口数量

/***** 如果配置了聚合 那么对聚合端口的所有操作都要以聚合组为单位进行 即聚合组视图**********/

/********例如配置trunk****/
[h3c] interface bridge 1  // 进入聚合组1
[h3c bridge-xxx] port link-type 'access/trunk/bybrid'  // 配置端口类型
```



### 10_IP相关

### 10.1 划分子网

> 注意192.168.0.1/26  本质是C类  所以在C类的基础上划分子网  只有第三个字节的前两位可以划分 2*2=4个子网    A类B类同理

<img src="https://raw.githubusercontent.com/gj-hat/drawing-bed/master/blog/Snipaste_2023-07-16_02-47-12.png" alt="Snipaste_2023-07-16_02-47-12" style="zoom:60%;" />



# 11_DHCP协议

 **特点:**

+ 即插即用:  客户端不需要配置  
+ 统一管理: IP地址和相关参数  都由DHCP服务器进行管理
+  效率高:  通过IP地址租期管理  提高地址利用率
+ 可跨网段实现: 通过DHCP中继 使不同子网的客户端和DHCP服务器实现协议报文交互



## 11.1 原理

<img src="https://raw.githubusercontent.com/gj-hat/drawing-bed/master/blog/Snipaste_2023-07-17_23-53-23.png" alt="Snipaste_2023-07-17_23-53-23" style="zoom:60%;" />



## 11.2基本实验

<img src="https://raw.githubusercontent.com/gj-hat/drawing-bed/master/blog/Snipaste_2023-07-18_00-38-30.png" alt="Snipaste_2023-07-18_00-38-30" style="zoom:60%;" />

``` shell
/*****************设备1 **********************/
[h3c]dhcp enable
[R1]dhcp server ip-pool 1
[R1-dhcp-pool-1]network 192.168.1.0 mask 255.255.255.0
[R1-dhcp-pool-1]gateway-list 192.168.1.254
[R1-dhcp-pool-1]dns-list 202.103.24.68 202.103.0.117
[R1-dhcp-pool-1]forbidden-ip 192.168.1.10 192.168.1.20 //只在地址池1中排除1和20

[R1]dhcp server forbidden-ip 192.168.1.10 192.168.1.20  // 排除10个网段

/*****************如果对端连接的是路由器 还需要设置路由器的dhcp **********************/
[R2]dhcp  enable
[R2]interface g0/0
[R2-GigabitEthernet0/0]ip address  dhcp-alloc      #获取地址方式为dhcp
```



## 11.3 中继

>因为dhcp是广播获取地址 所以一般情况下是不能跨网段的   使用中继路由器就可以实现跨网段

<img src="https://raw.githubusercontent.com/gj-hat/drawing-bed/master/blog/Snipaste_2023-07-18_00-56-27.png" alt="Snipaste_2023-07-18_00-56-27" style="zoom:60%;" />



``` shell
/*****************设备1 **********************/
//步骤1：在R1上开启DHCP功能，并创建1号DHCP地址池，宣告网段192.168.2.0/24，网关为192.168.2.254，DNS为202.103.24.68和202.103.0.117
[R1]dhcp enable
[R1]dhcp server ip-pool 1
[R1-dhcp-pool-1]network 192.168.2.0 mask 255.255.255.0
[R1-dhcp-pool-1]gateway-list 192.168.2.254
[R1-dhcp-pool-1]dns-list 202.103.24.68 202.103.0.117
// 步骤3：在R1上配置默认路由，使R1的DHCP协议报文能够到达PC3
[R1]ip route-static 0.0.0.0 0 192.168.1.2


/*****************设备2 **********************/
//步骤2:在R2上开启DHCP功能，并在连接客户端的接口（g0/1）上开启DHCP中继功能，并指定DHCP服务器的IP地址
[R2]dhcp enable
[R2]interface g0/1
[R2-GigabitEthernet0/1]dhcp select relay
[R2-GigabitEthernet0/1]dhcp relay server-address 192.168.1.1
```

## 11.4 常用命令

``` shell
[h3c]dhcp enable // 开启dhcp
[h3c]dhcp server ip-pool 1    // 创建dhcp地址池
[R1-dhcp-pool-1]network 192.168.2.0 mask 255.255.255.0   // 用于可分配的地址范围
[R1-dhcp-pool-1]gateway-list 192.168.2.254 114.114.114.114  // 同上 可分配的网关
[R1-dhcp-pool-1]dns-list 192.168.2.254 114.114.114.114  // 同上 可分配的dns
[R1-dhcp-pool-1]forbidden-ip 192.168.1.10 192.168.1.20 //只在地址池1中排除1和20
[h3c]expired ...   //配置dhcp的租约
[R1]dhcp server forbidden-ip 192.168.1.10 192.168.1.20  // 不参与分配的ip辞职  排除10个网段
[R2-GigabitEthernet0/1]dhcp select relay           // 在接口上开启dhcp中继
[R2-GigabitEthernet0/1]dhcp relay server-address 192.168.1.1  // 指定dhcp的服务器
[h3c]dis dhcp server statistics   // 查看dhcp服务器的统计信息
/*****************一些dis命令 **********************/
[H3C]dis ip int g0/0
[H3C]dis ip interface brief
[H3C]dis ip routing-table
[H3C]dis dhcp server ?
```



# 12_IPv6

<img src="https://raw.githubusercontent.com/gj-hat/drawing-bed/master/blog/Snipaste_2023-07-18_01-12-49.png" alt="Snipaste_2023-07-18_01-12-49" style="zoom:60%;" />

<img src="https://raw.githubusercontent.com/gj-hat/drawing-bed/master/blog/Snipaste_2023-07-18_01-22-13.png" alt="Snipaste_2023-07-18_01-22-13" style="zoom:60%;" />

<img src="https://raw.githubusercontent.com/gj-hat/drawing-bed/master/blog/Snipaste_2023-07-18_01-24-44.png" alt="Snipaste_2023-07-18_01-24-44" style="zoom:60%;" />

<img src="https://raw.githubusercontent.com/gj-hat/drawing-bed/master/blog/Snipaste_2023-07-18_01-35-46.png" alt="Snipaste_2023-07-18_01-35-46" style="zoom:60%;" />



# 13_IP路由原理

<img src="https://raw.githubusercontent.com/gj-hat/drawing-bed/master/blog/Snipaste_2023-07-18_01-53-16.png" alt="Snipaste_2023-07-18_01-53-16" style="zoom:60%;" />

<img src="https://raw.githubusercontent.com/gj-hat/drawing-bed/master/blog/Snipaste_2023-07-18_01-53-39.png" alt="Snipaste_2023-07-18_01-53-39" style="zoom:60%;" />

![image-20230719001159953](https://raw.githubusercontent.com/gj-hat/drawing-bed/master/blog/image-20230719001159953.png)

<img src="https://raw.githubusercontent.com/gj-hat/drawing-bed/master/blog/Snipaste_2023-07-18_02-00-46.png" alt="Snipaste_2023-07-18_02-00-46" style="zoom:60%;" />

<img src="https://raw.githubusercontent.com/gj-hat/drawing-bed/master/blog/Snipaste_2023-07-18_02-01-00.png" alt="Snipaste_2023-07-18_02-01-00" style="zoom:60%;" />

<img src="https://raw.githubusercontent.com/gj-hat/drawing-bed/master/blog/Snipaste_2023-07-18_02-04-38.png" alt="Snipaste_2023-07-18_02-04-38" style="zoom:60%;" />



# 14_直连路由和静态路由



## 14.1 Vlan间通信

<img src="https://raw.githubusercontent.com/gj-hat/drawing-bed/master/blog/Snipaste_2023-07-18_02-44-08.png" alt="Snipaste_2023-07-18_02-44-08" style="zoom:60%;" />



### 14.1.1 用802.1Q和子接口实现VLAN间通信  (单边路由)

+ 额外需要一个路由器, 配置多个子接口  

<img src="https://raw.githubusercontent.com/gj-hat/drawing-bed/master/blog/Snipaste_2023-07-18_02-29-08.png" alt="Snipaste_2023-07-18_02-29-08" style="zoom:40%;" />



<img src="https://raw.githubusercontent.com/gj-hat/drawing-bed/master/blog/Snipaste_2023-07-18_02-37-36.png" alt="Snipaste_2023-07-18_02-37-36" style="zoom:80%;" />

### 实验需求

1. 按照图示为 PC3 和 PC4 配置 IP 地址和网关
2. PC3 属于 Vlan10，PC4 属于 Vlan20，配置单臂路由实现 Vlan10 和 Vlan20 三层互通
3. PC3 和 PC4 可以互通

### 实验解法

1. **PC 配置 IP 地址部分略**
2. **PC3 属于 Vlan10，PC4 属于 Vlan20，配置单臂路由实现 Vlan10 和 Vlan20 三层互通**
   + 分析：用单臂路由实现 Vlan 间三层互通，需要把 SW2 连接 R1 的接口配置为 Trunk，并允许 Vlan10 和 Vlan20 . 通过R1 连接 SW2 的接口上要开启子接口，分别作为 Vlan10 和 Vlan20 的网关。这里规划 g0/0.1 子接口作为 Vlan10 的网关，IP 地址就是`192.168.1.254/24`，g0/0.2 子接口作为 Vlan20 的网关，IP 地址就是`192.168.2.254/24`. R1 的子接口上为了能够识别 SW2 的 Trunk 端口发送的 802.1Q 帧，还需要开启 dot1q 识别并绑定相应 Vlan。根据上述分析，g0/0.1 子接口绑定 Vlan10，g0/0.2 子接口绑定 Vlan20

```shell
// 步骤 1：在 SW2 上创建 Vlan10 和 Vlan20，并把 g1/0/1 接口加入 Vlan10，把 g1/0/2 接口加入 Vlan20
[SW2]vlan 10
[SW2-vlan10]port g1/0/1
[SW2-vlan10]vlan 20
[SW2-vlan20]port g1/0/2
// 步骤 2：把 SW2 的 g1/0/3 接口配置为 Trunk，并允许 Vlan10 和 Vlan20 通过
[SW2]interface g1/0/3
[SW2-GigabitEthernet1/0/3]port link-type trunk
[SW2-GigabitEthernet1/0/3]port trunk permit vlan 10 20
// 步骤 3：在 R1 上创建子接口 g0/0.1，开启 dot1q 识别，绑定到 Vlan10，并配置 IP 地址 192.168.1.254/24
[R1]interface g0/0.1
[R1-GigabitEthernet0/0.1]vlan-type dot1q vid 10
[R1-GigabitEthernet0/0.1]ip address 192.168.1.254 24
// 步骤 4：在 R1 上创建子接口 0/0.2，开启 dot1q 识别，绑定到 Vlan20，并配置 IP 地址192.168.2.254/24
[R1]interface g0/0.2
[R1-GigabitEthernet0/0.2]vlan-type dot1q vid 20
[R1-GigabitEthernet0/0.2]ip address 192.168.2.254 24
// 步骤 5  自己看路由表的Interface去
```





### 14.1.2 用三层交换机实现VLAN间路由 ( 三层交换技术  主要看这个)

+ 三层交换以内置的三层路由转发引擎执行VLAN间路由功能

<img src="https://raw.githubusercontent.com/gj-hat/drawing-bed/master/blog/Snipaste_2023-07-18_02-30-20.png" alt="Snipaste_2023-07-18_02-30-20" style="zoom:40%;" />

<img src="https://raw.githubusercontent.com/gj-hat/drawing-bed/master/blog/Snipaste_2023-07-18_02-45-26.png" alt="Snipaste_2023-07-18_02-45-26" style="zoom:60%;" />

### 实验需求

1. 按照图示为 PC2 和 PC3 配置 IP 地址和网关
2. PC2 属于 Vlan10，PC3 属于 Vlan20，在三层交换机上配置 Vlanif 三层接口实现 Vlan10 和 Vlan20 三层互通
3. PC2 和 PC3 可以互通

### 实验解法

1. **PC 配置 IP 地址部分略**
2. **PC2 属于 Vlan10，PC3 属于 Vlan20，在三层交换机上配置 Vlanif 三层接口实现 Vlan10 和 Vlan20 三层互通**
   + 分析：在三层交换机上实现 Vlan 间的路由互通，只需要对每个 Vlan 开启 Vlanif 三层接口，并配置对应网段的网关地址就可以了

``` shell
// 步骤 1：在 SW1 上创建 Vlan10 和 Vlan20，并把 g1/0/1 接口加入 Vlan10，把 g1/0/2 接口加入 Vlan20
[SW1]vlan 10
[SW1-vlan10]port g1/0/1
[SW1-vlan10]vlan 20
[SW1-vlan20]port g1/0/2
// 步骤 2：在 SW1 上对 Vlan10 和 Vlan20 开启 Vlanif 接口，并配置 Vlanif10 接口 IP 地址为 Vlan10 的网关地址 192.168.1.254/24，配置 Vlanif20 接口 IP 地址为 Vlan20 的网关地址 192.168.2.254/24
[SW1]interface Vlan-interface 10
[SW1-Vlan-interface10]ip address 192.168.1.254 24
[SW1]interface Vlan-interface 20
[SW1-Vlan-interface20]ip address 192.168.2.254 24
// 步骤 3  自己看路由表的Interface去
```



## 14.2 静态路由

<img src="https://raw.githubusercontent.com/gj-hat/drawing-bed/master/blog/Snipaste_2023-07-18_02-58-00.png" alt="Snipaste_2023-07-18_02-58-00" style="zoom:60%;" />



# 15_路由协议



**路由协议和可路由协议:**

1. 路由协议:

   + 路由器用来计算维护网络路由信息的协议, 通常由一定的算法 工作在传输层或者应用层
   + 常见的路由协议  RIP  OSPF  BGP

2. 可路由协议

   + 可被路由器转发的协议, 工作在网络层
   + 常见的可路由协议有IP IPX等等

   

**路由协议的功能:**

1. 邻居发现
2. 路由交换
3. 路由计算
4. 路由维护



**路由协议的分类:**

1. 按照使用的位置分类:
   1. IGP  内部网关协议:  运行在自治系统内部的路由协议 RIP OSPF IS-IS
   2. EGP 外部网关协议: 运行在自治系统之间的路由协议  BGP

2. 按照协议算法分类:
   1. 距离矢量协议: 度量值是跳数  RIP
   2. 链路状态协议: 度量值是开销(带宽越大 开销越小)  OSPF IS-IS
   3. 路径矢量协议: 有多种度量值  BGP



*自治系统(AS): 一组被统一管理, 运行同一个IGP的路由器组成的网络范围  一般不同城域网和不同的运行商都是不同的AS*





# 16_RIP(几乎不用)

<img src="https://raw.githubusercontent.com/gj-hat/drawing-bed/master/blog/Snipaste_2023-07-18_13-01-02.png" alt="Snipaste_2023-07-18_13-01-02" style="zoom:60%;" />

<img src="https://raw.githubusercontent.com/gj-hat/drawing-bed/master/blog/Snipaste_2023-07-18_13-01-52.png" alt="Snipaste_2023-07-18_13-01-52" style="zoom:60%;" />

<img src="https://raw.githubusercontent.com/gj-hat/drawing-bed/master/blog/Snipaste_2023-07-18_13-05-13.png" alt="Snipaste_2023-07-18_13-05-13" style="zoom:60%;" />





# 17_OSPF

**定义:** 

开放式最短路径优先, 基于链路状态特征

工作在IP层 协议号89

**专业名词:**

DR: 选举主路由器

BDR: 备份DR路由器

LSA: 链路状态通告

LSDB: 链路状态数据库

**五种报文:**

Hello: 发现和建立邻居关系报文  每隔10s发送一次Hellp报文   超过四次没有回复则认为对方下线则删除路由信息 

DD: 数据库描述报文  描述本地LSDB中所有LSA的摘要

LSR: 链路状态请求报文

LSU: 链路状态更新报文

LSAck: 链路状态确认报文



## 17.1 OSPF概述

<img src="https://raw.githubusercontent.com/gj-hat/drawing-bed/master/blog/Snipaste_2023-07-18_21-50-21.png" alt="Snipaste_2023-07-18_21-50-21" style="zoom:60%;" />



## 17.2 OSPF初始化流程

1. 建立邻居和邻接关系

   <img src="https://raw.githubusercontent.com/gj-hat/drawing-bed/master/blog/Snipaste_2023-07-18_22-05-52.png" alt="Snipaste_2023-07-18_22-05-52" style="zoom:60%;" />

2. 邻接路由器之间交换链路状态信息, 实现区域内链路状态数据库同步

   <img src="https://raw.githubusercontent.com/gj-hat/drawing-bed/master/blog/Snipaste_2023-07-18_22-07-04.png" alt="Snipaste_2023-07-18_22-07-04" style="zoom:60%;" />

3. 每台路由器根据本机链路状态数据库. 计算到达每个目的网段的最优路由, 写入路由表



## 17.3 OSPF区域管理

<img src="https://raw.githubusercontent.com/gj-hat/drawing-bed/master/blog/Snipaste_2023-07-18_22-23-18.png" alt="Snipaste_2023-07-18_22-23-18" style="zoom:60%;" />

<img src="https://raw.githubusercontent.com/gj-hat/drawing-bed/master/blog/Snipaste_2023-07-18_22-24-18.png" alt="Snipaste_2023-07-18_22-24-18" style="zoom:60%;" />



## 17.4 配置

<img src="https://raw.githubusercontent.com/gj-hat/drawing-bed/master/blog/Snipaste_2023-07-18_22-46-15.png" alt="Snipaste_2023-07-18_22-46-15" style="zoom:60%;" />

<img src="https://raw.githubusercontent.com/gj-hat/drawing-bed/master/blog/image-20230719001549664.png" alt="image-20230719001549664" style="zoom:67%;" />

### 实验需求

1. 按照图示配置 IP 地址
2. 按照图示分区域配置 OSPF ，实现全网互通
3. 为了路由结构稳定，要求路由器使用环回口作为 Router-id，ABR 的环回口宣告进骨干区域

### 实验解法

1. **配置 IP 地址部分略**

2. **按照图示分区域配置 OSPF ，实现全网互通**

   　　*分析：实现全网互通，意味着每台路由器都要宣告本地的所有直连网段，包括环回口所在的网段。要求 ABR 的环回口宣告进骨干区域，即区域 0，
      　　同时，每台路由器手动配置各自环回口的 IP 地址作为 Router-id*

   *步骤 1：在路由器上分别配置 OSPF，按区域宣告所有直连网段和环回口*

   ```shell
   [R1]ospf 1 router-id 1.1.1.1
   [R1-ospf-1]area 0
   [R1-ospf-1-area-0.0.0.0]network 1.1.1.1 0.0.0.0
   [R1-ospf-1-area-0.0.0.0]network 100.1.1.0 0.0.0.255
   [R1-ospf-1-area-0.0.0.0]area 1
   [R1-ospf-1-area-0.0.0.1]network 100.3.3.0 0.0.0.255
   
   [R2]ospf 1 router-id 2.2.2.2
   [R2-ospf-1]area 0
   [R2-ospf-1-area-0.0.0.0]network 2.2.2.2 0.0.0.0
   [R2-ospf-1-area-0.0.0.0]network 100.1.1.0 0.0.0.255
   [R2-ospf-1-area-0.0.0.0]network 100.2.2.0 0.0.0.255
   
   [R3]ospf 1 router-id 3.3.3.3
   [R3-ospf-1]area 0
   [R3-ospf-1-area-0.0.0.0]network 3.3.3.3 0.0.0.0
   [R3-ospf-1-area-0.0.0.0]network 100.2.2.0 0.0.0.255
   [R3-ospf-1-area-0.0.0.0]area 2
   [R3-ospf-1-area-0.0.0.2]network 100.4.4.0 0.0.0.255
   
   [R4]ospf 1 router-id 4.4.4.4
   [R4-ospf-1]area 1
   [R4-ospf-1-area-0.0.0.1]network 4.4.4.4 0.0.0.0
   [R4-ospf-1-area-0.0.0.1]network 100.3.3.0 0.0.0.255
   
   [R5]ospf 1 router-id 5.5.5.5
   [R5-ospf-1]area 2
   [R5-ospf-1-area-0.0.0.2]network 5.5.5.5 0.0.0.0
   [R5-ospf-1-area-0.0.0.2]network 100.4.4.0 0.0.0.255
   ```

3. **检查是否全网互通**

　　*分析：检查 OSPF 是否全网互通，一个是检查邻居关系表，看邻居关系是否正常；另一个是检查路由表，看是否学习到全网路由
　　这里只展示 R1 的检查结果*

*步骤 1：检查 R1 的邻居关系表*

```
[R1]display ospf peer 

     OSPF Process 1 with Router ID 1.1.1.1
           Neighbor Brief Information

 Area: 0.0.0.0        
 Router ID       Address         Pri Dead-Time  State             Interface
 2.2.2.2         100.1.1.2       1   36         Full/BDR          GE0/0

 Area: 0.0.0.1        
 Router ID       Address         Pri Dead-Time  State             Interface
 4.4.4.4         100.3.3.4       1   36         Full/DR           GE0/1
```

　　*说明：可以看到，R1 分别和 R2 和 R4 建立了邻接关系，状态为 FULL，邻居关系正常*
　　
*步骤 2：检查 R1 的路由表*

```
 [R1]display ip routing-table 

Destination/Mask   Proto   Pre Cost        NextHop         Interface
1.1.1.1/32         Direct  0   0           127.0.0.1       InLoop0
2.2.2.2/32         O_INTRA 10  1           100.1.1.2       GE0/0
3.3.3.3/32         O_INTRA 10  2           100.1.1.2       GE0/0
4.4.4.4/32         O_INTRA 10  1           100.3.3.4       GE0/1
5.5.5.5/32         O_INTER 10  3           100.1.1.2       GE0/0
100.1.1.0/24       Direct  0   0           100.1.1.1       GE0/0
100.1.1.0/32       Direct  0   0           100.1.1.1       GE0/0
100.1.1.1/32       Direct  0   0           127.0.0.1       InLoop0
100.1.1.255/32     Direct  0   0           100.1.1.1       GE0/0
100.2.2.0/24       O_INTRA 10  2           100.1.1.2       GE0/0
100.3.3.0/24       Direct  0   0           100.3.3.1       GE0/1
100.3.3.0/32       Direct  0   0           100.3.3.1       GE0/1
100.3.3.1/32       Direct  0   0           127.0.0.1       InLoop0
100.3.3.255/32     Direct  0   0           100.3.3.1       GE0/1
100.4.4.0/24       O_INTER 10  3           100.1.1.2       GE0/0
```

　　*说明：可以看到，R1 已经学习到了全网所有网段的路由信息*



# 18_ACL包过滤



## 18.1 ACL

> 只做匹配不做过滤

定义: 

1. 访问控制权限
2. 用于数据流的匹配和筛选

常见功能:

1. 访问控制:   ACL+Packet-filter
2. 路由控制:  ACL+Route-policy
3. 流量控制:  ACL+QOS



## 18.2 基于ACL的包过滤

**概述:**

<img src="https://raw.githubusercontent.com/gj-hat/drawing-bed/master/blog/image-20230719002342675.png" alt="image-20230719002342675" style="zoom:67%;" />

+ 对进出口的数据包逐个过滤, 丢弃或者允许通行
+ ACL应用于接口上, 每个接口的出入双向分别进行过滤
+ 仅当数据包经过一个接口时, 才能被此接口此方向的ACL进行匹配然后过滤

<img src="https://raw.githubusercontent.com/gj-hat/drawing-bed/master/blog/image-20230719002729428.png" alt="image-20230719002729428" style="zoom:67%;" />

 <img src="https://raw.githubusercontent.com/gj-hat/drawing-bed/master/blog/image-20230719003737264.png" alt="image-20230719003737264" style="zoom:50%;" />

<img src="https://raw.githubusercontent.com/gj-hat/drawing-bed/master/blog/image-20230719003805365.png" alt="image-20230719003805365" style="zoom:60%;" />



## 18.3 配置

<img src="https://raw.githubusercontent.com/gj-hat/drawing-bed/master/blog/image-20230719020404370.png" alt="image-20230719020404370" style="zoom:67%;" />

<img src="https://raw.githubusercontent.com/gj-hat/drawing-bed/master/blog/image-20230719004315030.png" alt="image-20230719004315030" style="zoom:67%;" />

### 实验需求

1. 按照图示配置 IP 地址
2. 全网路由互通
3. 在 SERVER1 上配置开启 TELNET 和 FTP 服务
4. 配置 ACL 实现如下效果
   1. `192.168.1.0/24` 网段不允许访问 `192.168.2.0/24` 网段，要求使用基本 ACL 实现
   2. PC1 可以访问 SERVER1 的 TELNET 服务，但不能访问 FTP 服务
   3. PC2 可以访问 SERVER1 的 FTP 服务，但不能访问 TELNET 服务
   4. `192.168.2.0/24` 网段不允许访问 SERVER1，要求通过高级 ACL 实现



### 实验解法

1. **配置 IP 地址部分**
2. **R1，R2，R3 上配置 OSPF使全网路由互通**
3. **在 SERVER1 上配置开启 TELNET 和 FTP 服务部分略**
4. **配置 ACL 部分**

```shell
/************配IP**********/
[PC1]int g 0/0
[PC1-GigabitEthernet0/0]ip add
[PC1-GigabitEthernet0/0]ip address 192.168.1.1 24

/*********给PC设置网关  (PC1/2是用路由器模拟的)********/
[PC1]ip route-static 0.0.0.0  0  192.168.1.1

/************配ospf*****************/
[R1]ospf 1 router-id 1.1.1.1
[R1-ospf-1]area 0
[R1-ospf-1-area-0.0.0.0]network 1.1.1.1 0.0.0.0
[R1-ospf-1-area-0.0.0.0]network 100.1.1.0 0.0.0.255
[R1-ospf-1-area-0.0.0.1]network 192.168.1.0 0.0.0.255


/***************开启 Telnet Ftp***************/
// 开启telnet
[h3c] telnet server enable // 1. 开启telnet
[h3c] local-user guojia class manage // 2. 创建一个xxx的用户 类型是管理员
[h3c-luser-manage-guojia]password simple 123455 // 3. 配置一个密码 123456
[h3c-luser-manage-guojia]service-type telnet // 4. 设置这个用户的服务类型为telnet
[h3c-luser-manage-guojia]authorization-attribute user-role level-15 // 5. 配置用户权限为最高级别 15级
[h3c-luser-manage-guojia]quit // 6. 退出用户视图
[h3c]user-interface vty 0 4 // 7. 创建进入用户虚拟视图  并允许这个用户只能创建5个虚拟视图(5个tlenet连接)
authentication-mode xxx // 8. 设置进入用户虚拟视图的方式 有三种 无密码、密码、用户名+密码
[R1-line-vty0-4]user-role level-15

// 开启ftp
[h3c] ftp server enable // 1. 开启ftp
[h3c] local-user xxx class manage // 2. 创建一个xxx的用户 类型是管理员
[h3c] password simple 123455 // 3. 配置一个密码 123456
[h3c] service-type ftp // 4. 设置这个用户的服务类型为ftp
[h3c] authorization-attribute user-role level-15 // 5. 配置用户权限为最高级别 15级

/*******ACL**********/

```

*步骤 1：创建基本 ACL，使 `192.168.1.0/24` 网段不能访问 `192.168.2.0/24` 网段，并在 R2 的 g0/2 接口的出方向配置包过滤*

```
[R2]acl basic 2000
[R2-acl-ipv4-basic-2000]rule deny source 192.168.1.0 0.0.0.255
[R2]interface g0/2
[R2-GigabitEthernet0/2]packet-filter 2000 outbound 
```

*步骤 2：创建高级 ACL，使 PC1 可以访问 SERVER1 的 TELNET 服务，但不能访问 FTP 服务；PC2 可以访问 SERVER1 的 FTP 服务，但不能访问 TELNET 服务，并在 R1 的 g0/1 接口的入方向配置包过滤*

```
[R1]acl advanced 3000
[R1-acl-ipv4-adv-3000]rule deny tcp source 192.168.1.1 0 destination 192.168.3.1 0 destination-port range 20 21
[R1-acl-ipv4-adv-3000]rule deny tcp source 192.168.1.2 0 destination 192.168.3.1 0 destination-port eq 23
[R1]interface g0/1
[R1-GigabitEthernet0/1]packet-filter 3000 inbound
```

*步骤 3：创建高级 ACL，使PC3 不能访问 SERVER1，并在 R2 的 g0/2 接口的入方向配置包过滤*

```
[R2]acl advanced 3000
[R2-acl-ipv4-adv-3000]rule deny ip source 192.168.2.0 0.0.0.255 destination 192.168.3.1 0
[R2]interface g0/2
[R2-GigabitEthernet0/2]packet-filter 3000 inbound 
```



# 19_PPP协议

> PPP（Point-to-Point Protocol）是一种常用的数据链路层协议，用于在计算机网络中传输数据。它的设计目标是使计算机通过串行线路（例如电话线、光纤线等）进行通信，以连接到因特网或其他网络服务提供者。
>
> PPP协议通常用于建立和维护两个设备之间的点对点连接。它支持多种传输介质，如串行线路、同轴电缆、光纤等，使得在不同物理网络上的两个设备能够进行数据通信。
>
> PPP协议的一些重要特点包括：
>
> 1. **认证与加密：** PPP支持认证机制，可用于验证连接设备的身份。常见的认证方式包括PAP（Password Authentication Protocol）和CHAP（Challenge Handshake Authentication Protocol）。PPP也支持可选的数据加密，增强数据传输的安全性。
> 2. **链路质量检测：** PPP可以在连接建立过程中检测链路质量，并根据需要进行错误检测和纠正。
> 3. **IP地址分配：** PPP协议支持IP地址的动态分配，通常使用PPP协议的一种变种PPP/IP来进行网络层的配置。
> 4. **多种网络层协议：** PPP本身是一种数据链路层协议，它可以在上层支持多种网络层协议，如IPv4、IPv6、IPX等。
> 5. **多种工作模式：** PPP支持多种工作模式，包括点对点模式、点对多点模式等。
>
> PPP协议通常用于建立拨号连接（Dial-up Connection），这种连接方式在早期因特网时代非常流行。用户通过拨号调制解调器连接到因特网服务提供商的服务器，PPP协议用于在用户计算机和服务提供商的服务器之间建立和管理数据通信连接。
>
> 虽然在高速宽带因特网普及后，拨号连接的使用已大大减少，但PPP协议仍然是一种重要的协议，特别是在某些特定的应用场景和网络环境中，如点对点专线、串口通信等。同时，PPP协议也为后来的协议提供了一些设计和实现上的启示。

<img src="https://raw.githubusercontent.com/gj-hat/drawing-bed/master/blog/image-20230719223745283.png" alt="image-20230719223745283" style="zoom:50%;" />

## 19.1 PPP会话流传

**注意: 如果要验证  那么 配置完后一定要重启接口  才能正式的开启PPP验证**

<img src="https://raw.githubusercontent.com/gj-hat/drawing-bed/master/blog/image-20230719225040782.png" alt="image-20230719225040782" style="zoom:50%;" />

## 19.2 PAP验证

**两次握手  且明文密码 不安全**

![image-20230719225235874](https://raw.githubusercontent.com/gj-hat/drawing-bed/master/blog/image-20230719225235874.png)

## 19.3 CHAP验证

**三次握手 不直接发送密码  安全**

<img src="https://raw.githubusercontent.com/gj-hat/drawing-bed/master/blog/image-20230719225345695.png" alt="image-20230719225345695" style="zoom:50%;" />





## 19.1 配置

### 实验拓扑

<img src="https://raw.githubusercontent.com/gj-hat/drawing-bed/master/blog/image-20230719224247636.png" alt="image-20230719224247636" style="zoom:50%;" />

### 实验需求

1. R1 和 R2 使用 PPP 链路直连，R2 和 R3 把 2 条 PPP 链路捆绑为 PPP MP 直连
   + 即R2为主验证方, 主验证方存储正确的用户和密码    
2. 按照图示配置 IP 地址
3. R2 对 R1 的 PPP 进行单向 chap 验证
4. R2 和 R3 的 PPP 进行双向 chap 验证



### 实验解法

1. **R2 和 R3 上配置 PPP MP**

   *步骤 1：在 R2 上创建 MP-GROUP 口*

   ```
   [R2]int MP-group 1
   ```

   *步骤 2：把 S1/0 和 S2/0 加入到上一步创建的 MP-GROUP口*

   ```
   [R2]interface s1/0
   [R2-Serial1/0]ppp mp MP-group 1
   ```

   ```
   [R2]interface s2/0
   [R2-Serial2/0]ppp mp MP-group 1
   ```

   *步骤 3：R3 上配置命令与 R2 一致，此处省略*

2. **按照图示配置 IP 地址**

   *分析：如果 PPP 链路上配置了 PPP-MP，那么链路的 IP 地址就必须配置在 MP-GROUP 口上，而不是物理口上*
   　　
   *步骤 1：R1 和 R2 的直连链路 IP 地址配置略*

   *步骤 2：在 R2 和 R3 的 MP-GROUP 口上配置 IP 地址*

   ```
   [R2]interface MP-group 1
   [R2-MP-group1]ip address 192.168.2.2 24
   ```

   ```shell
   [R3]interface MP-group 1
   [R3-MP-group1]ip address 192.168.2.3 24
   ```

3. **R2 对 R1 的 PPP 进行单向 chap 验证**

   *分析：R2 对 R1 进行单向验证，表示 R2 是主验证方，R1 是被验证方。所以需要在 R2 上创建用于验证的用户*

   *步骤 1：在 R2 上创建用于验证 R1 的用户*

   ```
   [R2]local-user user1 class network 
   [R2-luser-network-user1]password simple 123
   [R2-luser-network-user1]service-type ppp
   ```

   *步骤 2：在 R2 连接 R1 的接口上配置需要进行 PPP 验证，验证方式为 chap*

   ```
   [R2-Serial3/0]ppp authentication-mode chap  
   ```

   *步骤 3：在 R1 连接 R2 的接口上配置用于验证的用户名和密码*

   ```
   [R1-Serial1/0]ppp chap user user1
   [R1-Serial1/0]ppp chap password simple 123
   ```

   *步骤 4：关闭在开启 R1 和 R2 的 PPP 链路，检查验证是否能够通过（重启端口 ping）*

4. **R2 和 R3 的 PPP 进行双向 chap 验证**

   *分析：双向验证意味着 R2 和 R3 双方都需要创建用于验证的用户，且需要在各自接口上配置对端的用户名.
   另外，虽然R2 和 R3 之间的 PPP 链路配置了 PPP-MP，但是身份验证仍然需要配置在物理接口上，所以 R2 和 R3 相连的所有 PPP 接口上都需要配置验证*

   *步骤 1：在 R2 和 R3上创建用户验证 R3 的用户*

   ```shell
   [R2]local-user user2 class network
   [R2-luser-network-user2]password simple 123
   [R2-luser-network-user2]service-type ppp
   [R3]local-user user3 class network 
   [R3-luser-network-user3]password simple 123
   [R3-luser-network-user3]service-type ppp
   ```

   *步骤 2：在 R2 和 R3 相连的接口上配置需要进行 PPP 验证，验证方式为 chap，并配置对端验证本端的用户名*

   ```shell
   [R2-Serial1/0]ppp authentication-mode chap
   [R2-Serial1/0]ppp chap user user3   //密码一样 仅仅配置用户名就好
   [R2-Serial2/0]ppp authentication-mode chap
   [R2-Serial2/0]ppp chap user user3   //密码一样 仅仅配置用户名就好
   [R3-Serial1/0]ppp authentication-mode chap
   [R3-Serial1/0]ppp chap user user2    //密码一样 仅仅配置用户名就好
   [R3-Serial2/0]ppp authentication-mode chap
   [R3-Serial2/0]ppp chap user user2    //密码一样 仅仅配置用户名就好
   ```



## 20_WLAN

概述:

分为两个频率:

+ 2.4G  
+ 5G

802.11b/g工作频段

一共有十四个信道,  同时有三个无冲突信道

<img src="https://raw.githubusercontent.com/gj-hat/drawing-bed/master/blog/image-20230719232218273.png" alt="image-20230719232218273" style="zoom:50%;" />



设备:

1. AP 无线接入点
   1. FAT-AP: 无线网络配置在AP本机完成
   2. FIT-AP: 配置在AC上完成   AP会自动调用AC的配置
2. AC:  无线控制器
3. STA: 无线介入终端:   就是终端设备  电脑 手机
4. SSID:  无线介入标识
5. BSS:  基本服务集
6. DS域:  多个BSS通过无线桥接形成一个分布式系统





