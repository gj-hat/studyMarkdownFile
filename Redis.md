# Redis



## 01_简介

### 1.1 NoSQL

**NoSQL:**即Not-Only-SQL(泛指非关系型数据库),作为关系型数据库的补充

作用: 应对基于海量数据前提下的数据处理问题

解决的问题: 关系型数据库的多表关系过于复杂,高频查询时容易出现问题. 专注于数据的内容不在乎数据关系,且存储在内存中,大大减少了IO次数

**特征:**

+ 可扩容,可伸缩
+ 大数据量下高性能
+ 灵活的数据模型
+ 高可用

**常见的NoSQL:**

+ Redis
+ memcatche
+ HBase
+ MongoDB

**解决方案:(电商场景为例)**

1. 商品的基本信息  

   + 名称
   + 价格
   + 厂商
   + 参数

   这类信息一般来讲,对于一个页面来讲只查询一次,所以存储在Mysql之类的数据库

2. 商品的附加信息

   + 评论
   + 价格
   + 详情

   这类文档类信息在一个页面中一般对多次频繁查询,所以存放在缓存数据库中MongoDB

3. 图片信息

   这种文件类型的数据,一般存放在分布式文件系统中

4. 搜索关键字

   这种数据要求超高速存储 一般使用ES, Lucense, solr

5. 热点信息(包含以上的信息)

   + 高频
   + 波段性

   这种信息一般来讲访问量很大, 且持续时间不会太久.这种数信息数据存放在Redis,memcache,tair等

**基本信息存储方式:** 

<img src="https://tva1.sinaimg.cn/large/008i3skNly1gxd5g2a7pej311y0q6q59.jpg" alt="image-20211214102410952" style="zoom:50%;" />



### 1.2 Redis简介

**概念:**开源的高性能键值对数据库key-value数据库

**特征:**

	1. 数据间没有必然的关联关系
	1. 内部采用单线程机制进行工作
	1. 高性能
	4. 多重数据类型支持:
	 + 字符串类型    string
	 + 列表类型:   list
	 + 散列类型:    hash
	 + 集合类型:    set
	 + 有序集合类型    sorted_set

5. 持久化支持, 可以进行数据灾难恢复

**应用:**

+ 为热点数据加速查询(主要场景) 如热点商品, 热点新闻, 推广类等高访问信息等
+ 任务队列, 如秒杀, 抢购, 购票排队等
+ 即时信息查询, 如排行榜, 各类网站的数据访问统计, 公交到站信息, 在线人数信息(聊天室, 网站), 设备信号等
+ 时效性信息控制, 如验证码, 投票控制等等
+ 分布式数据共享, 如分布式集群架构中的session分离
+ 消息队列
+ 分布式锁



## 02_Redis下载与安装





## 03_Redis的基本操作



### 3.1 命令行模式

1. 功能性命令
   1. 信息添加:  `set key value`
   2. 信息获取:  `get key`  如果key不存在返回空(nil)
2. 清除屏幕信息 `clear/cls`
3. 帮助信息查询  
   1.  直接`help` 可以查看help的命令规范
   2. `help 命令名` 查看该命令详情: 例如`help get`
   3. `help @组名`查看组
4. 退出指令



## 04_Redis数据类型



### 4.1 数据类型介绍



#### 4.1.1业务数据的特殊性

##### 4.1.1.1作为缓存使用

1. 原始业务就需要缓存的设计
   + 秒杀
   + 618
   + 双十一
   + 排队
   + 购票
2. 运营平台监控到突发的高频次访问
   + 八卦新闻
   + 时政要闻
3. 高频, 复杂的统计数据
   + 在线人数
   + 投票..

##### 4.1.1.2附加功能

系统功能优化或升级:

+ 单服务器升级为多集群
+ Sesion管理
+ Token管理

#### 4.1.2Redis数据类型(5中常用的)

Redis是键值对形式: 

key: 永远是String

value: 有以下五种常用的类型

类比java数据类型

+ string      String
+ hash       HashMap
+ list          LinkedList
+ set         HashSet
+ sorted_set       TreeSet



### 4.2 string

#### 4.2.1简单操作

+ 添加/修改 `set key value`

+ 获取 `get key`

+ 删除数据 `del key`

+ 添加/修改多个数据 `mset key1 value1 key2 value2 key3....`

+ 获取多个数据 `mget key1 key2 key3....`

+ 获取数据字符串个数 `strlen key`

  ``` redis
  set a 100 b hahahaha
  strlen a   ---- 3
  strlen b   ---- 8
  ```

  

+ 追加信息到原始信息的尾部(存在则追加 不存在则新建) `append key value`



#### 4.2.2扩展操作

##### 4.2.2.1业务场景:

传统数据库中,大型的企业级应用中数据过多时候会进行分表操作.使用多张表存储同类型的数据,但是对应的主键id必须保证统一性,不能重复. Oracle数据库具有sequence设定,可以解决如上的问题。但是mysql数据库中没有这样的机制怎么解决呢?

解决方案(将主键的决定权交给redis去选择):

+ 设置数值数据增加指定范围的值

  ``` redis
  incr key            --------   给key的值加1
  incrby key increment          -------- key值增加的步长
  incrbyfloat key increment      -------- 可以用小数作为步长
  ```

  

+ 设置数值数据减少指定范围的值

  ``` redis
  decr key
  dectby key increment
  ```

总结:

+ 由于redis的单线程的  所以这个操作是线程安全的
+ redis用作控制数据库表的id, 为数据库主键提供生成策略,保证了主键唯一性
+ 使用所有的数据库,  且支持数据库集群

##### 4.2.2.2业务场景:

数据留存时间控制,  比如投票每四小时只能投一次, 时效新闻的时间控制

解决方案:

+ 设置数据指定的声明周期:

  ``` redis
  setex key seconds value          ------ 秒   setex key 10 value
  psetex key mulliseconds value       ------ 毫秒   psetex key 10 value
  ```



#### 4.2.3 注意事项

1. 数据操作不成功的反馈和正常操作之间的差异

   Redis的成功和失败是用`0/1`进行标识的

   当正常操作也可能反馈`0/1`

2. 数据未找到

   (nill) 等同于  null

3. 数据最大存储量

   512MB

4. 数值计算最大值范围(java中long的最大值)

#### 4.2.4命名习惯

1. 用户主键和属性值     更新修改操作

`key:表名:主键id:主键值:属性名 value:值   `

例如:

`set user:id:2:sex boy `

2. 值可以使用json格式存储      插入新

`key:表名:主键id:主键值 value:{json}   `

例如:

`set user:id:2 {name:zhangsan;age:18;sex:boy}`

### 4.3 hash

存储遇到的问题:

对象类数据(想上面的user就是一个对象类型的)的存储如果具有比较频繁的更新需求操作就会很麻烦

hash类型:

+ 新的存储需求: 对一系列存储的数据进行编组,方便管理, 典型应用于存储对象信息
+ 需要存储结构: 一个存储空间保存多个键值对数据
+ 存储结构:  `key:string   value:Map<field:value>`
  + 如果Map的值比较少  会优化成一个类数组
  + 如若Map的值比较多    存储结构使用HashMap结构



#### 4.3.1 基本操作

+ 添加/修改数据

  `hset key field value`

  例如:

  ``` redis
  hset user name zhangsan
  hset user age 18
  hset user sex boy
  ```

  

+ 获取数据

  `hget key field`

  `hgetall key`

  例如:

  ``` redis
  hsert user name
  hsetall user
  ```

  

+ 删除数据

  `hdel key filed1 [field2,filed3....]`



+ 添加/修改多个数据

  ``` redis
  hmset key field1 value1 filed2 value2 filed3 value3 ....
  ```



+ 获取多个数据

  ``` redis
  hmget key filed1 filed2 ....
  ```



+ 获取哈希表中字段的数量

  ``` redis
  hlen key
  ```

  

+ 获取哈希表中是否存在指定字段

  ``` redis
  hexists key field
  ```

  

#### 4.3.2 扩展操作

+ 获取哈希表中的所有字段名或字段值

  ``` redis
  hkets key       ----- 获取Map中的(也就是key-value的value)的key的列表
  kvals key       ----- 获取Map中的(也就是key-value的value)的value的列表
  ```

  

+ 设置指定字段的数值数据增加指定范围的值

  ``` redis
  hincrby key field increment   -------  可以给user里的age增加    user参考上面
  hincrbyfloat key filed increment
  ```



+ filed中有值不操作, 没值则添加

  ``` redis
  hsetnx key filed value
  ```

  





#### 4.3.3 注意事项:

+ hash的value只能是字符串 不可以嵌套
+ 键值对是有上限的
+  hash的类型十分接近对象的存储,并且可以灵活的添加个修改删除属性, 但是hash的初衷不是为了存储大量的对象而设计的, 不可以滥用, 更不可以将hash作为对象列表使用
+ hgetall可以获取全部的属性, 但是内部的filed过多的时候,效率非常的低 丧失了redis的意义



#### 4.3.4 应用场景

##### 业务场景

电商网站购物车的设计(抢购 限购 限量…)

 Key: 用户id

Value: 商品名 id 数量 值

例如:

``` redis
hmset user:id:001 g01 1 g02 3
hmset user:id:001 0号商品:1个  1号商品: 3个     ------伪代码
```

增强版本

``` redis
hmset user:id:001 0号商品:数量 5个 0号商品:信息 {json}
```





### 4.4 list

**主要关注点:**  数据存放的顺序

+ 数据存储需求: 存储多个数据, 并对数据进入存储空间的顺序进行区分
+ 需要的存储结构: 一个存储空间保存多个数据, 且通过数据可以体现进入顺序
+ list类型: 保存多个数据, 底层使用双向链表存储结构实现



#### 4.4.1 基本操作

+ 添加/修改数据

  ``` redis
  lpush key value1 [value2 value3 ...]       ------- 从左进入
  rpush key value1 [value2 value2 ...]       ------- 从右进入
  ```

+ 获取数据

  ``` redis
  lrange key start stop    -----start开始的下标  stop结束的下标  (如果不知道长度可以倒数第一个即-1)
  lindex key index
  llen key
  ```

+ 获取并移除数据

  ``` redis
  lpop key
  rpop key
  ```

  

#### 4.4.2 扩展操作

+ 规定时间内获取并移除数据

  ``` redis
  blpop key [key2 key3 ...] timeout   -----  如果有则取出 没有则等待超时时间 可以从多个列表中取出     测试的话需要开启两个客户端
  brpop key [key2 key3 ...] timeout
  ```

+ 移除指定数据

  ``` redis
  lrem key count value    --------  count是移除一个元素
  ```

  

#### 4.4.3 注意事项:

+ list中保存的数据是string类型   有上限
+ list有索引的概念, 但是操作数据时通常以队列的形式进行出入队, 或以栈的形式进行出入栈
+ list的stop可以是负数  代表倒数第几个
+ list可以对数据进行分页操作,  通常第一页的信息来自于list,  第二列及第二页之后的信息通过数据库的形式进行加载



#### 4.4.4 应用场景:

业务场景:  需要顺序  

朋友圈点赞,  日志   新闻的发布时间  

点赞则添加列表

取消点赞则移除



### 4.5 set

重点看重: 不重复


+ 新的存储需求: 存储大量的数据, 还需要更高的查询效率
+ 新的存储结构:  保存大量的数据, 高效的内部存储机制,  便于查询
+ 和java一样  不允许重复   底层实现和hash一样 只不过没有value




#### 4.5.1 基本操作

+ 添加/更新数据

  ``` redis
  sadd key memner1 [member2 member3 ...]
  ```

  

+ 获取全部数据

  ``` redis
  smembers key
  ```

  

+ 删除数据

  ``` redis
  srem key member1 [member2 member3 ...]
  ```

  

#### 4.5.2 扩展操作

##### 4.5.2.1 业务场景:

每位用户在注册时候 可以选择三项爱好内容, 但是后期为了增加么用户的活跃度,兴趣点 必须让用户对其他信息也要产生兴趣.  如何实现? 

**解决方案:**

+ 随机获取几个中指定数量的数据

  ``` redis
  srandmember key [count]
  ```

  

+ 随机获取几个中的某个数据并将其移除集合

  ``` redis
  spop key
  ```

  

##### 4.5.2.2 业务场景:

社交软件中经常会推送两个人的共同好友或共同爱好  如何实现?

**解决方案:**

+ 两个集合交并差

  ``` redis
  sinter key1 [key2]        -------   交
  sunion key1 [key2]        -------   并
  sdiff key1 [key2]         -------   差
  ```

  

+ 两个集合交并差并存到新的集合中

  ``` redis
  sinterstore destination key1 [key2]        -------- 交
  sunionstore destination key1 [key2]        -------- 并
  sdiffstore destination key1 [key2]         -------- 差
  ```

  

+ 将指定数据中原始数据中移动等到目标集合中

  ``` redis
  smovbe source derstination member          ------- source原始集合  derstination目标集合 member指定的数据
  ```

  

#### 4.5.3 注意事项:

+ set类型不允许重复, 如果已经存在只保留一份
+ set虽然于hash的结构相同  但是无法启用hash的存储值的空间



#### 4.5.4 应用场景:

业务场景:  

企业中有12000员工, 内部的OA系统中有700多角色, 3000多个操作员, 23000多种数据, 每位员工有一种或多种角色  如何快速的进行校验?

 

### 4.6 sorted_set

+ 新的存储需求: 数据排序有利于数据的有效展示, 需要提供一种可以根据自身特征进行排序的方式
+ 新的存储结构: 可以保存可排序的数据
+ sorted_set类型: 在set集合的存储结构基础上添加可排序字段   暂称其为score字段



#### 4.6.1 基本操作

+ 添加数据

  ``` redis
  zadd key score1 member1 [score2 member2 ....]
  ```

  

+ 获取全部数据

  ``` redis
  zrange kry start stop [WITHSCORES]
  zrevrange key start stop [WITHSCORES]
  ```

  

+ 删除数据

  ``` redis
  zrem key member [member ... ]
  ```

  

+ 按条件获取数据

  ``` redis
  zrangebyscore key min max [WITHSCORES] [LIMLT]
  zreangebyscore key max min [WITHSCORES]
  ```

​		示例:

​		`zrangebyscore userAge 5 18 withscores limit 0 3 --------- 查询5到18岁的数据 取前四个`




+ 条件删除数据

  ``` redis
  zremrangebyrank key start stop
  zremrangebyscore key min max
  ```




+ 获取集合数据总量

  ``` redis
  zcard key
  zcount key min max
  ```

  

+ 集合交并操作

  ``` redis
  zinterstore destination numkeys key [key ...]        --------- 配置aggregate可以实现交并时候对score列的求和求差运算
  zunionstore destination numkeys key [key ...]
  ```

  

  



#### 4.6.2 扩展操作

##### 4.6.2.1 各大平台榜单排序?

解决:

+ 获取数据对应的索引

  ``` redis
  zrank key member           -------- zrank user 77  查询到77这个数 在集合中排名顺序的下标
  zrevrank key member        --------   从大到小排序
  ```

+ 

+ score值得获取与修改

  ``` redis
  zscore key member                  -------  zscore user 77  查到77这个数的score值
  zincrby key increment member       -------    increment 可以给上述的score值进行增加和减小
  ```

  



#### 4.6.3注意事项:

+ min和max用于限定搜索查询条件

+ start和stop用于限定查询范围,  作用于索引  表示开始和结束的索引

+ offset于count用于限定查询范围, 作用于查询结果, 表示开始位置和数据总量

+ score保存的数空间是64位    可以是整数和双精度double值    慎用 可能会丢失精度

  

#### 4.6.4 应用场景:

**业务场景:**

视频网站VIP试用 的时间管理   时效性的信息

  

## 05_数据类型实例

### 5.1 业务场景:

手机短线,验证码,密码输入次数…..之类的规定时间内限定每个用户使用次数  如何实现?

#### 5.1.1 解决方案:

+ 设计计数器, 记录调用次数, 用于控制业务执行次数. 以用户id作为key, 使用次数作为value
+ 在调用前获取次数, 判断是否超过限定次数
  + 不超过次数的情况下:   次数+1
  + 失败调用的情况下:  次数-1
+ 为计算数据设置生命周期为指定周期, 例如1s,1min 自动清空周期内使用次数

实现思路:

1. 用户访问时,创建一个用户id作为key
   1. 如果key不存在则+1,  同时设置生命周期60s
   2. 达到阈值次数  则停止调用

实现过程:

``` redis
get uid:001             ----------- nil 先看看有没有重名
setex uid:001 60 1        -----------  超时时间60秒 值为1
get uid:001          ---------------   1
incr uid:001          ---------------  2
incr uid:001          ---------------  3
-------------60s后----------------------
get uid:001            -------------     nil
```

#### 5.1.2 遇到的问题:

每次都要通过业务层进行判断,  重复过多(阈值设置为10次)

解决(操作数有最大值):

+ 取消最大值的判断, 利用incr的操作超过最大值抛出异常的形式替代每次判断是否大于最大值
+ 判断是否为nil
  + 如果是  设置Max-次数   (9223372036854775807)
  + 如果不是   计数+1
  + 业务调用失败   计数-1

实现过程:

``` redis
get uid:002
sextex uid:002 60 9223372036854775797
incr uid:002
.......   执行10次 后抛出异常
```



### 5.2 业务场景

微信qq等的聊天软件, 接收消息时候 会将默认收到的消息指定, 当多个好友同时发送消息时候,该排序会不停的交替.  同时还可以将重要的会话置顶. 用户离线后  消息该按照什么样的顺序显示?

 描述:

用户uid:001作为接收信息的用户

用户uid:002 — uid:005 消息的发送方

实现思路:

1. 设置一直set集合 存放置顶的好友   例如002  003

2. 两个list集合存放(普通好友和置顶好友)信息的顺序

3. 如果有消息进入 则 先判断是否在置顶set中

   	1. 如果在  则存放在置顶list中
   	1. 不在    则放在普通好友list中

4. list的顺序   存放list的时候先判断之前是不是已经存在(同一好友第二次发送消息)

   1. 如果是  先删除原来的记录   再右添加
   2. 如果不是  则直接右添加

   

### 5.3 常见解决场景

![image-20211215093717439](https://tva1.sinaimg.cn/large/008i3skNly1gxe9phdhgoj31kt0u0tfc.jpg)





## 06_通用命令



### 6.1 key通用指令

**特征:**

+ key是一个字符串  通过key获取redis中保存的数据

**key操作:**

+ 有关key自身状态的操作   删除 判定存在  获取类型
+ key有效性    有效期设定   判定是否有效  有效状态切换
+ key快速查询      按指定策略查询key



#### 6.1.1 key基本操作

+ 删除指定key

  `del key`

+ 获取key是否存在

  `exists key`

+ 获取key的类型

  `type key`



#### 6.1.2 key扩展操作

+ 为指定key设置有效期

  ``` redis
  expire key seconds
  pexpire key milliseconds
  expireat key timeout
  pexpireat key millisecomds-timestamp
  ```

  

+ 获取key的有效时间

  ``` redis
  ttl key
  pttl key
  ```

  

+ 切换key从时效性转化为永久性

  ``` redis
  persist key
  ```

  

+ 查询key

  ``` redis
  keys pattern
  --------------------值得一提-----------------
  pattern只支持少量的正则
  1. * 任意长度 任意符号
  2. ? 任意符号  长度为1
  3. [] 匹配其中的一个指定符号
  ```

  

+ 为key改名

  ``` redis
  rename key newKey         ------覆盖改名
  renamex key newKey        ------存在则不改名
  ```

  

+ 对key排序

  ``` redis
   sort  [还有一些参数控制]        ------- 默认只排 不改原来
  ```

  

+ key的通用操作

  ``` redis
  help @generic
  ```

  

### 6.2 数据库通用指令

+ key重复的问题

解决:

+ redis为每个服务提供了16个数据库, 编号从0到16
+ 每个数据库相互独立
+  数据库相互独立  但是这16个公用的是同一片内存



#### 6.2.1 db基本操作

+ 切换数据库

  ``` redis
  select index       -------    选择0 --- 15号
  ```

  

+ 其他操作

  ``` redis
  quit
  ping
  echo message          ------类似输出
  ```

  

#### 6.2.2 db基本操作

+ 数据移动

  ``` redis
  move key db    ------- 将key移动到 db号库
  ```

  

+ 数据清除

  ``` redis
  dbsize           --------- 查看库中有多少个key
  flushdb          --------- 清空当前库
  flushall         --------- 清空所有库
  ```

  



## 07_Jedis

Jedis是一个工具 连接编程语言和redis

**编程语言与redis:**

+ java连接到redis使用到的工具
  + Jedis
  + SpringData Redis
  + Lettuce
+ 其他语言也有起自身操作redis的工具

### 7.1 快速入门:

``` java
public void test1(){
  // 1. 连接redis
  var jedis = new Jedis("127.0.0.1", 6379);
  // 2. 操作redis
  var l = jedis.lpush("list1", "aaa", "sss");
  System.out.println("st = " + l);
  // 3. 关闭连接
  jedis.close();
}
```



### 7.2 业务场景

服务调用次数控制(上文又提到   规定时间内用户至多操作几次)

案例设定:

1. 设定ABC三个用户
2. A用户限制10次/分钟,  B用户30次/分钟,  C用户不限制

设计思路:

1. 设定一个服务方法,用于模拟实际业务调用的服务, 内部采用打印模拟调用
2. 在业务调用前服务于调用控制单元, 内部使用redis进行控制, 参照之前的方案
3. 对调用超限使用异常进行控制, 异常处理设定为打印提示信息
4. 主程序启动三个线程, 分别表示三种不同用户的调用

代码实现:

``` java
public class Service {
    private String id;
    private int count;
    public Service(String id, int count) {
        this.id = id;
        this.count = count;
    }
    //控制单元
    public void service() {
        var jedis = new Jedis();
        String s = jedis.get("compid:" + id);
        try {
            //判断值是否存在;
            if (s == null) {
                if (count == 0) {
                    jedis.setex("compid:" + id, 20, count + "");
                } else {
                    jedis.setex("compid:" + id, 20, Long.MAX_VALUE - count + "");
                }
            } else {
                var count1 = jedis.incr("compid:" + id);
                if (count == 0) {

                    this.business1(id, count1);
                } else {
                    this.business(id, count1);

                }
            }
        } catch (JedisDataException jedisDataException) {
            System.out.println("使用已经上限了 请稍后再试");
            return;
        } finally {
            jedis.close();
        }
    }
    public void business(String id, Long count) {
        System.out.println("用户:" + id + " 业务操作执行第" + (count - (Long.MAX_VALUE - this.count)) + "次");
    }
    public void business1(String id, Long count) {
        System.out.println("用户:" + id + " 业务操作执行第" + count + "次");
    }
}
class MyThread extends Thread {
    Service se;
    public MyThread(String id, int count) {
        se = new Service(id, count);
    }
    @Override
    public void run() {
        while (true) {
            se.service();
            try {
                Thread.sleep(500);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
    
}
class Main {
    public static void main(String[] args) {
        MyThread myThread = new MyThread("A", 10);   myThread.start();
        MyThread myThread1 = new MyThread("B", 20);  myThread1.start();
        MyThread myThread2 = new MyThread("C", 0);   myThread2.start();
    }
}
```



### 7.3 工具类



#### 7.3.1 基于连接池获取连接

+ JedisPool: Jedis提供的连接池技术

  + poolConfig: 连接池配置对象
  + host: redis地址
  + port: 端口号

  ``` java
  public JedisPool(GenericObjectPoolConfig poolConfig, String host, int port) {
    this(poolConfig, host, port, 2000, (String)null, 0, (String)null);
  }
  ```



**代码实现:**

``` java
public class JedisUtils {
    private static JedisPool jedis = null;
    static {
        JedisPoolConfig jedisPoolConfig = new JedisPoolConfig();
        jedisPoolConfig.setMaxIdle(10);
        jedisPoolConfig.setMaxTotal(30);
        jedis = new JedisPool(jedisPoolConfig, "127.0.0.1", 6379);
    }
    // 上下留一个
    static {
        ResourceBundle resourceBundle = ResourceBundle.getBundle("redisUtils");
        JedisPoolConfig jedisPoolConfig = new JedisPoolConfig();        jedisPoolConfig.setMaxIdle(Integer.parseInt(resourceBundle.getString("redis.maxIdle")));      jedisPoolConfig.setMaxTotal(Integer.parseInt(resourceBundle.getString("redis.maxTotal")));
        jedis = new JedisPool(jedisPoolConfig, resourceBundle.getString("redis.host"), Integer.parseInt(resourceBundle.getString("redis.port")));
    }
    public static Jedis JedisPool(String host, int port) {
        JedisPoolConfig jedisPoolConfig = new JedisPoolConfig();
        jedisPoolConfig.setMaxIdle(10);
        jedisPoolConfig.setMaxTotal(30);
        var jedis = new JedisPool(jedisPoolConfig, host, port);
        return jedis.getResource();
    }
    public static Jedis JedisPool() {
        return jedis.getResource();
    }
}
```



 ### 7.3 Linux/Unix

1. 安装  
2. 使用
   1. 启动server   `redis-server`
   2. 启动client    `redis-client`
3. 使用配置文件启动
   1. `redis-server config.conf`
   2. 可选命令`redis-client -p port`

4. 出现的问题

   1. 启动失败, 终结之前的进程

      `ps -ef | grep -i redis`

      `kill -9 pid`





## 08_持久化

+ 将当前数据状态进行保存, 快照形式, 存储数据结构, 存储格式简单, 关注点在数据
+ 将数据的操作过程进行保存, 日志形式, 存储操作过程, 存储格式复杂, 关注点在数据的操作过程

按照类型分类:

1. RDB:  快照
2. AOF:   过程(日志)



### 8.1 RDB

命令:

``` redis
save       ---------  保存当前的数据
```



53https://www.bilibili.com/video/BV1CJ411m7Gc?p=53



















