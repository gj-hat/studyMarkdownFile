# 小车正式版



### 设计:

#### 多线程:

1. 每个拖车一个作为线程
2. 可以使用用户名作为区分

#### redis设计

1. key:   每一个小车的小车名
2. value:  `[拖车ID, 时间][….] `







### 实现:

假定排除条件:

1. 出发点和结束点认为没有数据或者不发送数据

基于以下事实:

1. 小车同一时间段物理上只能被一辆拖车操作
2. 一个小车可能同时会被多个拖车逻辑上拖动,且也符合连接时间+超时时间。正确的拖车对小车的操作时间甚至不一定最长，但是一定是最早的。

算法目的:

1. 计算出小车每一次操作的时间和操作设备(拖车ID)
2. 排除多辆拖车同行时的脏数据
2. 即时清理数据



#### 计算:

> 通过lua编写

关于连接:   每10秒执行一次定时任务

以下只是一个小车的value的示例:

1. 处于确认连接时间内

   1. 所有`[拖车ID,时间]` 都被记录下来,但是最先出现拖车ID(称为A)优先级最高, (前提是系统中没有优先级高的数据)

2. A处于确认连接时间外 且还在连接:

   1. A数组的`[1]` 时间之前的所有其他拖车数据全部删除
   2. 确认连接时间–断开连接的时间内,redis不再记录新`[拖车ID,时间]` 数据

3. 拖车断连, :

   1. 没有经过了确认连接且超时时间内:

      1. 执行`1.1` (保留优先级)

   2. 没有经过了确认连接且超时时间外

      1. 移除该拖车数据 (优先级自动交出)

   3. 经过了确认连接且超时时间内

      1. 执行`1.1` 等到连接上了再执行`2.*`  (保留优先级) 

   4. 经过了确认连接且超时时间外

      1.  将该数据进入持久层  且redis中删除该数据

      

**难点**:

优先级如何实现?



























