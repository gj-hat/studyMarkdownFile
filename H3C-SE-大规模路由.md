# 01_企业网模型

## 1.1 基于SOA的网络架构

<img src="https://raw.githubusercontent.com/gj-hat/drawing-bed/master/blog/image-20230807094829214.png" alt="image-20230807094829214" style="zoom:50%;" />

解释:

1. 基础设施层: 通过网络IP将所有的基础资源整合在一起形成一个资源池(服务层)
2. 服务层:  整个了所有的资源形成一个一个的虚拟机 供应用层去调用
3. 应用层: 最终呈现出来的东西.  



## 1.2 层级化网络模型

> 优点: 网络结构清晰 便于规划和维护 增强网络稳定性  增强网络可拓展性

<img src="https://raw.githubusercontent.com/gj-hat/drawing-bed/master/blog/image-20230807103000741.png" alt="image-20230807103000741" style="zoom:67%;" />

### 1.2.1 接入层

> 小于园区网大部分指的是二层(广域网需要跨地域可能就是三层)   这一部分一般是直接连接用户设备的交换机  影响范围比较小 可能就是一个办公室等等

1. 为用户提供网络的访问接口
2. 丰富的大量接口
3. 接入速率控制, 基于策略的分类, 数据包标记等
4. 较少考虑冗余层   (接入层直接面向用户设备,如果考虑冗余代价太大)

### 1.2.2 汇聚层

> 这一部分一般是在机房中  影响范围比较大

1. 将接入层的数据汇聚起来, 依据策略对数据,信息等实施控制.
2. 必要的冗余设计
3. 复杂的策略配置(NE阶段只学习简单的ACL策略)
   + 包括路由策略, 安全策略, QoS策略(流控)等

### 1.2.3 核心层

> 一般是园区 企业最核心的机房  影响范围最广 设备一般比较贵 尽可能不考虑成本

1. 对来汇聚层的数据进行尽可能快速的交换
2. 强大的数据交换能力
3. 稳定, 可靠的高冗余设计
4. 不配置复杂策略



## 1.3 典型企业网结构

> 默认来说: 
>
> 1. 园区网大于等于两栋楼
> 2. 大型分支机构: 一栋楼
> 3. 中型: 一层或者好几层楼

<img src="https://raw.githubusercontent.com/gj-hat/drawing-bed/master/blog/image-20230807104047536.png" alt="image-20230807104047536" style="zoom:67%;" />

## 1.4 模块化企业网模型

<img src="https://raw.githubusercontent.com/gj-hat/drawing-bed/master/blog/image-20230807104226848.png" alt="image-20230807104226848" style="zoom:67%;" />

例如:

如果网络比较小: 可以只选择园区网主干+广域网汇聚+Internet介入  同理需要数据中心再加一个模块.

**模块化网络架构的优点:**

1. 确定网络 边界清晰 流量类型清楚
2. 便于规划 增加伸缩性
3. 模块方便增删 降低复杂性
4. 设计的完整性



# 02_大规模路由概述

## 2.1 三层网络模型与路由技术

![image-20230807105246586](https://raw.githubusercontent.com/gj-hat/drawing-bed/master/blog/image-20230807105246586.png)

补充:

1. 核心层如果涉及多园区 可能就会使用BGP 
2. 互联网接入(WAN连接): 一般使用静态路由  多园区可能BGP多个自治系统AS

## 2.2 大规模网络的路由可靠性需求

<img src="https://raw.githubusercontent.com/gj-hat/drawing-bed/master/blog/image-20230807105639625.png" alt="image-20230807105639625" />

补充:

1. 浮动静态路由: 园区入口可能会有不止一条的线路做冗余  如果有一条挂了 另一个补上



## 2.3 大规模网络的路由可拓展性需求

![image-20230807105952426](https://raw.githubusercontent.com/gj-hat/drawing-bed/master/blog/image-20230807105952426.png)

## 2.4 大规模网络的路由可管理性需求

<img src="https://raw.githubusercontent.com/gj-hat/drawing-bed/master/blog/image-20230807113647280.png" alt="image-20230807113647280" style="zoom:80%;" />

补充:

1. 调整路由开销...:  人为控制路由路线
2. 用路由过滤...:  控制路由的传播范围
3. PBR: 策略路由 (不是路由策略)  控制数据报文做路由转发  比如控制数据包的转发不看目的地址而是原地址. 

## 2.5 网络路由的快速恢复需求

<img src="https://raw.githubusercontent.com/gj-hat/drawing-bed/master/blog/image-20230807113959987.png" alt="image-20230807113959987" style="zoom:80%;" />

# 03_路由控制与转发

## 3.1 路由器转发原理

<img src="https://raw.githubusercontent.com/gj-hat/drawing-bed/master/blog/image-20230807114314706.png" alt="image-20230807114314706" style="zoom:80%;" />

## 3.2 FIB表的生成

<img src="https://raw.githubusercontent.com/gj-hat/drawing-bed/master/blog/image-20230807114812690.png" alt="image-20230807114812690" style="zoom:80%;" />

补充:

1. `display ip routing-table`看到的不是完整路由信息
2. 查看路由详细信息是另一个命令  `display ip routing-table verbose`
3. 路由表中的Active路由自动转入FIB表中

## 3.3 快速转发表

<img src="https://raw.githubusercontent.com/gj-hat/drawing-bed/master/blog/image-20230807143414252.png" alt="image-20230807143414252" style="zoom:80%;" />

补充:

1. 命令: `dis ip fast-forwarding cache`
2. 清除: `reset ip fast-forwardind cache`
3. 这是一个缓冲表: 查表速度快 但是很快就会清空
4. 专门有存储芯片

快速转发过程图:

![image-20230807143654218](https://raw.githubusercontent.com/gj-hat/drawing-bed/master/blog/image-20230807143654218.png)

# 04_路由协议基础

## 4.1 路由分类

1. 直连路由: 无需配置维护 有链路层协议自动发现
2. 静态路由: 人工配置和维护 不能自适应网络拓扑变化    无协议开销
3. 动态路由: 协议自动学习,计算. 无需人工配置及维护 自适应拓扑变化  路由协议开销大

分类:

<img src="https://raw.githubusercontent.com/gj-hat/drawing-bed/master/blog/image-20230807144543974.png" alt="image-20230807144543974" style="zoom:80%;" />

1. 按照使用的位置分类:
   1. IGP  内部网关协议:  运行在自治系统内部的路由协议 RIP OSPF IS-IS
   2. EGP 外部网关协议: 运行在自治系统之间的路由协议  BGP

2. 按照协议算法分类:
   1. 距离矢量协议: 度量值是跳数  RIP
   2. 链路状态协议: 度量值是开销(带宽越大 开销越小)  OSPF IS-IS
   3. 路径矢量协议: 有多种度量值  BGP

![image-20230807145302248](https://raw.githubusercontent.com/gj-hat/drawing-bed/master/blog/image-20230807145302248.png)

![image-20230807145317359](https://raw.githubusercontent.com/gj-hat/drawing-bed/master/blog/image-20230807145317359.png)

<img src="https://raw.githubusercontent.com/gj-hat/drawing-bed/master/blog/image-20230807145525745.png" alt="image-20230807145525745" />

![image-20230807145936573](https://raw.githubusercontent.com/gj-hat/drawing-bed/master/blog/image-20230807145936573.png)

**补充: BGP是基于TCP协议  即路由协议建立之前 就知道其他区域的IP** 

![image-20230807150021757](/Users/jia-guo/Library/Application Support/typora-user-images/image-20230807150021757.png)

## 4.2 路由选择原则

![image-20230807145744296](https://raw.githubusercontent.com/gj-hat/drawing-bed/master/blog/image-20230807145744296.png)

![image-20230807145805053](https://raw.githubusercontent.com/gj-hat/drawing-bed/master/blog/image-20230807145805053.png)

补充: IS-IS是优先级15



## 4.3 路由备份

两根线带宽差不多时候使用等价路由  差的多使用路由备份

### 4.3.1 ECMP等价路由

> 目的地址一样 开销一样

![image-20230807150530851](https://raw.githubusercontent.com/gj-hat/drawing-bed/master/blog/image-20230807150530851.png)

+ 通过ECMP(多路径等值路由  cost一样 目的地址一样) 系统可以自动实现路由负载分担
+ 分担方式有两种:
  + 基于流:  不同五元组的流量分担  例如访问qq youtube
  + 基于包 :  接收方需要做包重组  比较麻烦用的少

### 4.3.2 路由备份

> 目的地址一样 开销不一样

![image-20230807151120288](https://raw.githubusercontent.com/gj-hat/drawing-bed/master/blog/image-20230807151120288.png)



### 4.3.3 路由备份实验

实验图:

![image-20230807152411515](https://raw.githubusercontent.com/gj-hat/drawing-bed/master/blog/image-20230807152411515.png)

说明:

1. 红线是低速线路
2. 默认应该是走直连 直连的开销小  但是红线是低速度 所以手动需要配置一下优先级大于OSPF 150的开销

配置:

``` shell
// ---------R1--------------------//
[R1]int LoopBack 0
[R1-LoopBack0]ip address 10.1.1.1 24
[R1-LoopBack0]in g 0/0
[R1-GigabitEthernet0/0]ip address 192.168.1.1 24
[R1-Serial1/0]ip address 192.168.3.1 24
[R1]ospf 1 router-id 10.1.1.1
[R1-ospf-1]area 0
[R1-ospf-1-area-0.0.0.0]network 10.1.1.1 0.0.0.255
[R1-ospf-1-area-0.0.0.0]network 192.168.1.0 0.0.0.255
[R1]ip route-static 20.1.1.1 24 192.168.3.3
// ---------R2--------------------//
[R2]int g 0/0
[R2-GigabitEthernet0/0]ip address 192.168.1.2 24
[R2-GigabitEthernet0/0]in g 0/1
[R2-GigabitEthernet0/1]ip address 192.168.2.2 24
[R2]ospf 1 router-id 30.1.1.1
[R2-ospf-1]area 0
[R2-ospf-1-area-0.0.0.0]network 192.168.1.0 0.0.0.255
[R2-ospf-1-area-0.0.0.0]network 192.168.2.0 0.0.0.255

// ---------R3--------------------//
[R3]int LoopBack 0
[R3-LoopBack0]ip address 20.1.1.1 24
[R3-LoopBack0]in g 0/0
[R3-GigabitEthernet0/0]ip address 192.168.2.3 24
[R3-Serial1/0]ip address 192.168.3.3 24
[R3]ospf 1 router-id 20.1.1.1
[R3-ospf-1]area 0 
[R3-ospf-1-area-0.0.0.0]net
[R3-ospf-1-area-0.0.0.0]network 20.1.1.1 0.0.0.255
[R3-ospf-1-area-0.0.0.0]network 192.168.2.0 0.0.0.255
[R3]ip route-static 10.1.1.1 24 192.168.3.1
```

解释:  此时通过路由表可以看到  优先走的是ospf路由协议而不是静态路由

配置静态路由优先级:

``` shell
[R1]ip route-static 10.1.1.1 24 192.168.3.3 preference 151
[R3]ip route-static 20.1.1.1 24 192.168.3.1 preference 151
```



## 4.4 路由聚合与CIDR

### 4.4.1 概述

![image-20230807181219974](https://raw.githubusercontent.com/gj-hat/drawing-bed/master/blog/image-20230807181219974.png)

![image-20230807174324874](https://raw.githubusercontent.com/gj-hat/drawing-bed/master/blog/image-20230807174324874.png)

 补充:

1. 聚合后可能会因为聚合设备的掉线或者IP分配不合理导致环路.此时就需要配置一个黑洞路由, 即匹配到黑洞路由的数据包会被丢弃.

   ``` shell
   ip route-static 10.0.0.0 255.255.255.252.0 Null0
   ```

### 4.4.2 路由备份/聚合实验

![image-20230807200034014](https://raw.githubusercontent.com/gj-hat/drawing-bed/master/blog/image-20230807200034014.png)

配置:  断开R2 则走静态路由

```
[R1]rip
[R1-rip-1]version 2
[R1-rip-1]undo summary
[R1-rip-1]network 200.1.1.0

[R2]rip
[R2-rip-1]version 2
[R2-rip-1]undo summary
[R2-rip-1]network 200.1.1.0
[R2-rip-1]network 200.2.2.0

[R3]rip
[R3-rip-1]version 2
[R3-rip-1]undo summary
[R3-rip-1]network 200.2.2.0
[R3-rip-1]network 192.168.0.0
[R3-rip-1]network 192.168.1.0
[R3-rip-1]network 192.168.2.0
[R3-rip-1]network 192.168.3.0

[R1]ip route-static 192.168.0.0 22 200.3.3.3 preference 110
[R3-GigabitEthernet0/1]rip summary-address 192.168.0.0 22
```



# 05_OSPF协议

> Open Shortest Path First，  开放最短路径优先   协议号89





## 5.1 前情概述

**五种报文类型:**

+ Hello：用于邻居、邻接 发现、建立、保活 hello time 默认10s或30s
+ DD：数据库描述包  用DD报文描述自己的完整LSDB每一条LSA的头部信息
+ LSR：链路状态请求    如果发现自身和对端的LSDB不一致则发送LSR报文请求缺少的LSA信息(这次是LSA的摘要信息  称之为LSR)
+ LSU：链路状态更新    接上一步  用来发送对方缺少LSA的完整信息  
+ LSack：链路状态确认         用来对上一步的操作进行确认

**邻居状态机**

- Down：一旦本地发出hello包进入下一个状态
- Init：初始化 收到的hello包若存在本地的RID进入下一个状态
- 2way：双向通讯 邻居关系建立的标志

**初始化流程**:

1. 建立邻居和邻接关系  (邻居表) 

    `dis ospf peer`

2. 邻接路由器之间交换链路状态信息, 实现区域内链路状态数据库同步  (LSDB表/拓扑表) 

   `dis ospf lsdb`

3. 每台路由器根据本机链路状态数据库. 计算到达每个目的网段的最优路由, 写入路由表  (路由表)

   `dis ospf routing`

**ospf的路由器类型**

1. 区域内路由器(IR): 所有的接口都在一个区域内

2. 骨干路由器(BR): 有接口在骨干区域

3. 区域边界路由器(ABR): 连接骨干和一个非骨干区域

   ospf的所有非骨干区域都要连接骨干区域(只有一个). 如果连接了两个非骨干区域则不会成为ABR

4. 自治系统边界路由器(ASBR) : 连接外部自治系统,并引入外部路由

OSPF的网络类型:

> 对于不同的二层网络类型 OSPF会生成不同的网络类型, 不同的网络其DR/BDR LSA细节等也会不同

1. Boradcast(广播网络 以太网的默认类型)
   1. 组播发送协议报文:`224.0.0.5/6`前者所有运行OSPF的接口监听 后者DR/BDR接口监听.
   2. 由于广播网络会有很多设备  则需要选举DR/BDR
   3. hello-time=10s   dead-time=40s
2. NBMA(非广播多路访问):  很少用了   
3. P2MP(点到多点网络, 来源:手动从其他网络类型修改过来)
   1. 模拟组播发送协议报文,可以自动发现邻居
   2. 不选举DR/BDR
   3. hello-time=30s   dead-time=120
4. P2P(点到点网络, PPP默认的类型):  有时候只有两个设备可以手动配置P2P则可以加快ospf协议建立
   1. 组播发送报文
   2. 不选举DR/BDR  PPP是点到点的 只有两个设备
   3. hello-time=10s  dead-time=40s

​                        

**邻接建立流程**:  `dis ospf peer`

1. Down:  关闭状态(稳定)
2. Init(初始化): 收到了对方的hello报文  耽美收到对方的hello确认报文
3. Attempt: 这种只出现在NBMA网络中, 发出了hello包 但没收到对方的hello包  (不重要)
4. 2Way 处于邻居状态(稳定):
   1. 邻居双方互相发现并确认了DR/BDR角色: 选举完后哪怕出现优先级更高也不会重新选举. 除非原来的DR/BDR失效或者重置ospf进程
   2. 2Way前提: Router-id无冲突   掩码长度/区域ID/验证密码/hello和dead-time/特殊区域类型一致 
5. Exstart(交换开始状态): 
   1. 发送第一个DD报文: 不传送LSDB摘要 仅用于确定LSDB协商的主从
6. Exchange(交换状态): 发送都需的DD报文 用于通过LSDB摘要
7. Loading(读取状态): 进行LSA的请求,加入,确认
8. Full(邻接状态 稳定): 两段的LSDB同步
   1. Full前提: 两端MTU一致
   2. 能够计算路由的前提:  两端网络类型一致 

**更新流程:**

1. 收到LSA更新, 在本地LSDB中查询此LSA 未查询到此LSA则更新
2. 查到LSA  则和本地LSA的序号对比  序号大的留下

开销:

1. 参考带宽
   1. 计算开销的基准带宽值
   2. 默认参考带宽是100M  建议把网络中最高链路的带宽设置为参考带宽
   3. 参考带宽仅仅对本地有效
2. 计算方法
   1. 链路带宽大于等于参考带宽  cost=1
   2. 链路带宽小于参考带宽 cost = 参考带宽/链路带宽

## 5.2 命令

``` shell
[h3c]route id "id"    // 配置全局ID这个配置对所有的协议都生效 
[h3c]ospf process-id(进程 数字可变) route-id "id"  // 协议内配置id  优先级更高
[h3c]dis ospf peer   // ospf邻居
[h3c]display ospf interface   //查看OSPF接口信息  网络类型通过此命令查看
[h3c]display ospf routing    //查看OSPF路由信息 可以查看路由所属区域和发布者
[h3c]display ospf statistics error  // 查看ospf的报错信息
[h3c]display ospf lsdb   //查看本地LSDB汇总信息
[h3c-G0/0]ospf network- type 'broadcast/nmba/p2mp/p2p'  //配置接口网络类型
[h3c-G0/0]ospf cost value  //改开销
[h3c-G0/0]ospf timer hello/dead ' seconds'   //配置接口HELLO时间
[h3c-ospf-1]Bandwidth-reference 'value'  //配置参考带宽
[h3c-ospf-1]default-route-advertise  将已经存在的默认路由引入到OSPF
[h3c-ospf-1]default-route-advertise always  //自动产生一条默认路由LSA下发到OSPF
[h3c]
[h3c]
[h3c]
[h3c]
[h3c]
[h3c]
[h3c]
[h3c]
[h3c]
[h3c]
[h3c]
[h3c]
[h3c]
[h3c]
[h3c]
[h3c]
```



## 5.3 实验1

<img src="https://raw.githubusercontent.com/gj-hat/drawing-bed/master/blog/image-20230809181024111.png" alt="image-20230809181024111" style="zoom:80%;" />

**要求:**

1. 按照图示配置 IP 地址
2. R1，R2，R3 运行 OSPF 使内网互通，所有接口（公网接口除外）全部宣告进 Area 0；要求使用环回口作为 Router-id 
3. 业务网段不允许出现协议报文
4. R5 模拟互联网，内网通过 R1 连接互联网，在 R1 上配置默认路由并引入到 OSPF
5. R1 上配置 EASY IP，只允许业务网段访问互联网
6. 要求业务网段访问互联网流量经过 R3，R2，R1

**步骤:**

``` shell
[R1]ospf router-id 1.1.1.1
[R1-ospf-1]area 0
[R1-ospf-1-area-0.0.0.0]network 10.1.1.0 0.0.0.255
[R1-ospf-1-area-0.0.0.0]network 10.2.2.0 0.0.0.255
[R2]ospf router-id 2.2.2.2
[R2-ospf-1]area 0
[R2-ospf-1-area-0.0.0.0]network 10.1.1.0 0.0.0.255
[R2-ospf-1-area-0.0.0.0]network 10.3.3.0 0.0.0.255
[R3]ospf router-id 3.3.3.3
[R3-ospf-1]area 0
[R3-ospf-1-area-0.0.0.0]network 10.2.2.0 0.0.0.255
[R3-ospf-1-area-0.0.0.0]network 10.3.3.0 0.0.0.255
[R3-ospf-1-area-0.0.0.0]network 192.168.1.0 0.0.0.255
//---------------------------业务网不允许出现协议报文---------------------------------//
[R3-ospf-1]silent-interface g0/2   // R3 连接业务网段的 g0/2 口配置为静默接口，防止协议报文冲击业务网段
//----------------------在 R1 上配置默认路由并引入到 OSPF-----------------------------//
[R1]ip route-static 0.0.0.0 0 202.1.1.5
[R1-ospf-1]default-route-advertise
//----------------------R1 上配置 EASY IP，只允许业务网段访问互联网------------------//
[R1]acl basic 2000
[R1-acl-ipv4-basic-2000]rule permit source 192.168.1.0 0.0.0.255
[R1-GigabitEthernet0/2]nat outbound 2000
//----------------------要求业务网段访问互联网流量经过 R3，R2，R1------------------//
[R1-GigabitEthernet0/0]ospf cost 1000
[R3-GigabitEthernet0/0]ospf cost 1000
```



## 5.4 OSPF高级



### 5.4.1 v2五类LSA

1. Router LSA(路由器LSA): 描述区域内部与路由器直连的链路信息,仅仅在区域内部传输















