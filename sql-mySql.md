[TOC]
# MySql相关

## 01_安装

windows安装器安装：https://www.runoob.com/w3cnote/windows10-mysql-installer.html

cmd： mysql  -u root -p

## 02_语言分类

### 2.1DDL数据定义语言
+ Create   创建库，表
+ Drop      删除库，表
+ Alter      修改库，表

### 2.2DCL数据控制语言
+ grant		权限设置
+ if		编程

### 2.3DML数据操纵语言
+ insert	增
+ update	改
+ delete	删

### 2.4DQL数据查询语言
+ select	查询

## 03_使用SQL（小写部分非必要）

### 3.1 操作数据库
+ 增	
  CREATE DATABASE 数据库名  character set 字符集 collate 字符集校对规则 

+ 删  

  DROP DATABASE 数据库名

+ 改  

  ALTER DATABASE 数据库名 character set 字符集 collate 字符集校对规则 

+ 查  

  + 查看所有数据库SHOW DATABASES； 
  + 查看指定数据库定义信息 SHOW DATABASES 数据库名；

+ 切换使用中的数据库 

  USE DATABASES 数据库名

+ 显示当前正在使用的数据库

  SELECT DATABASE<>;

### 3.2操作数据表

+ 增	
  CREATE  TABLE 数据表（字段名 字段类型（长度）约束，字段名 字段类型（长度）约束，字段名 字段类型（长度）约束） 

  + 字段类型
  <img src="https://gitee.com/thisisafigurebed/figure-bed/raw/master/servletNode/1.png" style="zoom:70%">
  <img src="https://gitee.com/thisisafigurebed/figure-bed/raw/master/servletNode/2.png" style="zoom:70%">
  <img src="https://gitee.com/thisisafigurebed/figure-bed/raw/master/servletNode/3.png" style="zoom:70%">
  + 约束
    + 主键约束 primary key （默认唯一非空  可以双主键）
  + 唯一约束 unique
    + 非空约束 not null
  
+ 删  

  DROP TABLE 数据表名；

+ 改  

  + 添加列

    ALTER TABLE 表名 ADD 列名 类型(长度) 约束

  + 修改列类型，长度，约束

    ALTER TABLE 表名 MODIFY 列名 类型(长度) 约束

  + 删除列

    ALTER TABLE 表名 drop 列名

  + 改列名

    ALTER TABLE 表名 CHANGE 旧列名 新列名

  + 改表名

    RENAME 旧表名 TO 新表名

  + 改表字符集

    ALTER TABLE 表名 CHARACTER SET 字符集；

  + ......

+ 查  

  + 查看所有数据表SHOW TABLES； 
  + 查看指定数据表结果 desc 表名；


### 3.3操作数据表记录

+ 增

  INSERT INRTO 表名(列名1，列名2，列名3......) VLAUES (值1，值2，值3......)

  INSERT INRTO 表名 VLAUES (值1，值2，值3......)

  <font size="4" color="red">中文乱码问题：mysql服务器客户端编码改成gbk</font>

+ 删

  + 修改表中所有记录

    DELETE FROM 表名；

  + 删除表中某一行记录

    DELETE FROM 表名 where 条件；

  + 删除表中所有记录的两种办法

    + delete from 表名

      属于DML语句    一条一条删除   事务可以作用在DML上

    + truncate table 表名

      属于DDL语句  删除表然后创建一个结果 一样的表    事务不能控制DDL

    + 事务回滚

      rolback；

+ 改

  + 修改一列中所有值

    UPDATE 表名 set 列名=值，列名=值；

  + 修改某一条记录某个值

    UPDATE 表名 set 列名=值，列名=值 where 条件；

+ 查

  + 基本查询   []为可选属性    |或者的意思   distinct不显示重复值

    select [distinct] *|列名 from 表名 

    + 带运算查询

      <font size="4" color="red">可以执行运算： select name,chinese+math+english from stuExam</font>

      <font size="4" color="red">可以给列取别名运算： select name,chinese+math+english as sum from stuExam</font>

  + 条件查询 Where

    select [distinct] *|列名 from 表名 [where 条件]

    + <,>,<=,>=,<>,=

    + like:模糊查询（比如姓张）

    + in:范围查询

    + and，not，or：条件关联

    + 实践

      + 名字李四英语成绩大于90分

        select * from stuExam where name='李四' and english > 90；

      + 姓张   可以使用_或者%做为占位符     前者占一个字符  后者不限

        select * from stuExam  where name like '张_|%'      

        select * from stuExam  where name like `'_|%四_|%'`

      + in范围检索  英语分数为65，80，90的记录

        select * from stuExam where english in <65,80,90>;

  + 排序查询 order by关键字    asc升序    desc降序

    + ORDER BY 字段 asc

      select * from stuExxam order by english asc|desc;

    + 实践  

      + 按照学生语文成绩降序排列，如果语文成绩相同则按照英语成绩升序排列

        select * from stuExxam order by chinese desc，english asc;

      + 查询姓李同学再按照英语成绩降序排列查询

        select * from stuExam where like name '李%' order by english desc

  + 分组统计查询

    + 聚合函数使用

      + sum();    求和

      + count();    求总数个数

      + max();      求最大值

      + min();       求最小值

      + avg();        求平均值

      + 实践

        获取所有学生英语成绩数学总和 select sum(english),sum(math) from stuExam;

        获取李姓学生英语成绩综合select sum(english) from stuExam where like name '李%';

        获取各科成绩总和   <font size="4" color="red">函数ifnull<english,0>  如果英语记录有null替换成0</font>

        + select sum(ifnumm(english,0)+math+chinese) from stuExam;
        + select sum(english)+sum(math)+sum(chinese) from stuExxam;

  + 分组查询  group by 字段名称

    + 按照商品名分类统计卖出去的个数
      
      select product,count(*) from ordrtitm group by product;
          <img src="https://gitee.com/thisisafigurebed/figure-bed/raw/master/servletNode/4.png" style="zoom:70%"><img src="https://gitee.com/thisisafigurebed/figure-bed/raw/master/servletNode/5.png" style="zoom:70%">
      
      + 按照商品名统计合计金额
    
        select product,sum(price) from orderitm group by product;
        <img src="https://gitee.com/thisisafigurebed/figure-bed/raw/master/servletNode/6.png" style="zoom:70%">
    
  + 按照商品名统计，统计每类商品合计金额在5000以上的商品
  
     <font size="4" color="red">where后不能跟聚合函数。如果需要使用带聚合函数的条件过滤需要使用关键字having</font>
  
    select product,sum(price) from orderitm group by product having sum(price) > 5000;
  
  + 上面条件再按照总价降序
  
    select product,sum(price) from orderitm group by product having sum(price) > 5000 order by sum(price) desc;

### 3.4查询记录总结

s(select.....) .....F(from....)....W(where....)....G(group by....)....H(having.....).....O(order by.....asc|desc);

## 04_数据库的备份和还原

### 4.1 备份

`mysqldump -u root -p 需要备份的数据库名称 > 需要备份的路径`

### 4.2 还原

+ 备份还原

  1. 进入数据库中创建一个数据库
  2. 进入创建的数据库
  3. `source 备份的数据库路径 `
+ 进入内部还原

  1. 进入数据库中创建一个数据库
  2. 重新打开一个命令框
  3. `mysql -u root -p 刚刚创建数据库名 < 备份的数据库路径 `

## 05_多表设计之外键约束

### 5.1 单表

+ 主键约束
+ 唯一约束
+ 非空约束

### 5.2多表约束

+ 外键约束  



## 06_多表设计原则

### 6.1 一对一

+ 唯一外键对应  在一方新建一列外键用来指向另一方主键
+ 主键对应：两张表主键建立对应

### 6.2 一对多  

多的一方创建外键指向1的一方主键

### 6.3 多对多

创建一个中间表  则两个多和中间表关系就成为了一对多
<img src="https://gitee.com/thisisafigurebed/figure-bed/raw/master/servletNode/7.png" style="zoom:40%">

## 07_多表查询

### 7.1 连接查询

+ 交叉连接（不经常使用）：查询的是笛卡尔积

  select * from 表1 cross join 表2；

  select * from 表1，表2；

+ 内连接 inner join   inner可以省略

  + 显式内连接(使用inner join)    

    select * from 表1 inner join 表2 on 关联条件；

  + 隐士内连接(不使用inner join)

    select * from 表1,表2 where 关联条件；

+ 外连接outer join     outer可以省略

  + 左外连接

    select * from 表1 left outer join 表2 on 关联条件；

  + 右外连接

    select * from 表1 left outer join 表2 on 关联条件；

+ 内连接和外连接的区别
  + 内连接查询两个表之间公共部分
  + 外连接查询的是两个表之间公共部分和某一张表的特有部分
    <img src="https://gitee.com/thisisafigurebed/figure-bed/raw/master/servletNode/8.png" style="zoom:70%">

  

### 7.2 子查询

> 一个查询语句条件依赖与另一个查询语句的结果

+ 带in的子查询

  + 查询学生生日在91年之后学生的班级信息

    1. select cno from student where birthday > '1991-01-01'
    2. select * from classes where cid in(上面);
    
    合并在一起`select * from classes where cid in(seclect cno from student where birthday > '1991-01-01')`
  
+ 带exists的子查询

  + 查询学生生日在91年之后的学生信息,   如果查询结果存在则执行前面语句

    select * from classes where exists(seclect cno from student where birthday > '1991-01-01')

+ 带any的子查询     返回值中任意一个即可

  + 返回大于子语句查询出来的任意一个值

    select * from classes where cid > any(seclect cno from student )
	<img src="https://gitee.com/thisisafigurebed/figure-bed/raw/master/servletNode/9.png" style="zoom:70%">
  
+ 带all的子查询        返回值中所有值满足才行

  + 返回大于子语句查询出来的任意一个值 

     select * from class where cid > all(select cno from student);
	<img src="https://gitee.com/thisisafigurebed/figure-bed/raw/master/servletNode/10.png" style="zoom:70%">
  
  
## 08_事务

### 8.1 事务的概念

事务:指的是逻辑上的一组操作，组成这组操作的各个逻辑单元，要么全部成功要么全部失败。

### 8.2 MySql的事务管理

准备工作<img src="https://gitee.com/thisisafigurebed/figure-bed/raw/master/servletNode/11.png" style="zoom:70%">

+ 转账案例 小张给小风转账1000元

  1. 给小张减1000元   update account set money = money - 1000 where name = "小张"；

  2. 给小风加1000元   update account set money = money + 1000 where name = "小风"；

  3. 上述操作必须1和2同时完成才视为完成  ，否则则失败

  4. 上述操作就是事务

###    8.3 事务开启提交和回滚 

+ 开启事务

  start transaction;

+ 提交事务

  commit;

+ 回滚事务

  rollback;

+ 上述案例sql语句操作
  + 开启和提交
    <img src="https://gitee.com/thisisafigurebed/figure-bed/raw/master/servletNode/12.png" style="zoom:70%">
  + 开启和回滚
    <img src="https://gitee.com/thisisafigurebed/figure-bed/raw/master/servletNode/14.png" style="zoom:50%"><img src="https://gitee.com/thisisafigurebed/figure-bed/raw/master/servletNode/13.png" style="zoom:50%">

  ```sql
  start transaction;
  update account set money = money - 1000 where name = "小张"；
  update account set money = money + 1000 where name = "小风"；
  //成功则事务提交  commit;
  //如果失败可以执行rollback回滚数据
  //提交后则不能回滚
  ```

### 8.4 事务的特性

+ 原子性

  强调的是事务的不可分割，组成事务的各个逻辑单元不可分割

+ 一致性

  事务执行的前后，数据完整性保持一致

+ 隔离性

  事务执行不应该受到其他事务的干扰

+ 持久性

  事务一旦提交（或者回滚或者结束），数据就持久化到数据库中 

### 8.5 事务的隔离级别

> 隔离性：一个事务的执行不应该受到其他事务的干扰
>
> 如果不考虑隔离性会引发一些安全问题，注意体现在读取上



#### 8.5.1主要问题有

+ 脏读：一个事务读到了另一个事务还未提交的数据，而导致查询结果不一致
+ 不可重复读：一个事务读到了另一个事务update的数据，导致多次读取不一致
+ 虚读/幻读：一个事务读到了另一个事务insert的数据，导致多次读取不一致

#### 8.5.2 如何解决这些问题

设置事务的隔离级别（效率依次下降 ，安全性依次上升）   开发中一般设置中间两个

+ read uncommitted    未提交读					:脏读，不可重复读，虚度都有可能发生
+ read committed      已提交读(orcle默认)    :避免脏读   其他三个有可能发生 
+ repeatable read      (MySql默认)                  :避免脏读不可重复读，其他一个可能发生
+ seriallizable      可串行的                               :三种都可以避免

**设置事务隔离级别 SET SESSION TRANSACTION ISOLATION LEVEL  隔离级别**

**查看当前事务隔离级别  SELECT @@tx_isolation; **

## 09_事务隔离级别演示

### 9.1 演示脏读
1. 开启a,b两个窗口（多线程）
2. 设置a窗口的隔离级别为read uncommitted
3. 在a,b中分别开启事务
4. 在b窗口完成转账操作（只执行并为提交）
5. 在a窗口中查询钱到转账成功（已经发生脏读：一个事务已经读到了另一个事务未提交的数据）
### 9.2 避免脏读，演示重复读
1. 开启a,b两个窗口（多线程）
2. 设置a窗口的隔离级别为read committed
3. 在a,b中分别开启事务
4. 在b窗口完成转账操作（只执行并为提交）
5. 在a窗口中查询钱未到账，未转账成功（以及避免脏读）
6. 在b窗口中提交事务commit；
7. 在a窗口查询转账成功（一个事务中多次查询结果不一致，即发生不可重复读）

### 9.3 避免不可重复度，演示虚读（虚读不一定会出现，MySql中以概率形式发生）

1. 开启a,b两个窗口（多线程）
2. 设置a窗口的隔离级别为repeatable read
3. 在a,b中分别开启事务
4. 在b窗口完成转账操作（只执行并为提交）
5. 在a窗口中查询钱未到账，未转账成功（以及避免脏读）
6. 在b窗口中提交事务commit；
7. 在a窗口中查询钱未到账，未转账成功（一个事务中多次查询结果一致）
8. 虚读不一定会出现（MySql中以概率形式发生）

### 9.4 隔离最高级别

演示类似上面：b发生改变，a查询时候会卡住，直到b提交数据。