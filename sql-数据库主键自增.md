#  给postgreSQL设置主键自增

 

> 问题描述: 在mysql中可以很容易的设置主键自增; 但是pg数据库中默认不支持主键自增我们可以通过以下方式来添加主键自增



## 1. mysql

### 1.1 命令行下

1. 建表时可以直接设置主键自增

   ``` sql
   字段名 数据类型 AUTO_INCREMENT
   
   -- 当然也可以设置自增初始值
   mysql> CREATE TABLE tb_student2 (
       -> id INT NOT NULL AUTO_INCREMENT,
       -> name VARCHAR(20) NOT NULL,
       -> PRIMARY KEY(ID)
       -> )AUTO_INCREMENT=100;
   ```

2. 建表完成了,可以追加一个主键自增

   ``` sql
   alter table 表名 add 字段名 数据类型 primary key AUTO_INCREMENT; # 添加自增主键
   ```

### 1.2 navicat下

1. 建表时可以直接设置主键自增

   ![image-20220506095754802](https://tva1.sinaimg.cn/large/e6c9d24ely1h1ygaxddiij21do0oajtk.jpg)

2. 建表完成后 追加主键自增

   点击设计表 接下来和上面一样



## 2. postgreSQL



### 2.1 命令行下

1. 建表时候

   ``` sql
   create table 表名(
   	id serial primary key,
   )
   ```

   

2. 建表完成后追加

``` sql
-- 请注意下面 下面的命令没有包含数据库名 表名  模式名   
-- 标准的pg语句是 数据库名.模式名.表名.字段名

-- 新增一个名为mip_empower_app_manage_app_id_seq的序列  下面是序列的配置 
CREATE SEQUENCE mip_empower_app_manage_app_id_seq
-- 一次加多少
INCREMENT 1
-- 初始值
START 1
-- 最小值
MINVALUE 1
-- 最大值
MAXVALUE 99999999
CACHE 1;

-- 查询序列 查看是否创建成功
SELECT nextval('序列名');

-- 给字段设置序列   
-- mip_empower_app_manage表名
alter table mip_empower_app_manage
-- app_id字段名   后面的是序列名
alter column app_id set default nextval('mip_empower_app_manage_app_id_seq');
```



### 2.2 navicat下

1. 建表时没有这样设置选项

2. 建表后

   1. 执行查询命令

      ``` sql
      -- 新增一个名为mip_empower_app_manage_app_id_seq的序列  下面是序列的配置 
      CREATE SEQUENCE mip_empower_app_manage_app_id_seq
      -- 一次加多少
      INCREMENT 1
      -- 初始值
      START 1
      -- 最小值
      MINVALUE 1
      -- 最大值
      MAXVALUE 99999999
      CACHE 1;
      ```

   2. 添加进字段, 点击设计表

      默认值处: `nextval('上面添加的序列名'::regclass)`

      ![image-20220506101014483](https://tva1.sinaimg.cn/large/e6c9d24ely1h1ygngpu68j21iu0leq68.jpg)

### 2.3 补充序列的相关方法

相关的方法如下（regclass 表示序列的名称）：

| 函 数                              | 返 回 类 型 | 描 述                                                        |
| ---------------------------------- | ----------- | ------------------------------------------------------------ |
| currval( regclass )                | bigint      | 获取指定序列最近一次使用netxval后的数值，如果没有使用nextval而直接使用currval会出错。 |
| lastval()                          | bigint      | 返回最近一次用 nextval 获取的任意序列的数值                  |
| nextval( regclass )                | bigint      | 递增序列并返回新值                                           |
| setval( regclass,bigint )          | bigint      | 设置序列的当前数值                                           |
| setval( regclass,bigint ,boolean ) | bigint      | 设置序列的当前数值以及 is_called 标志，如果为true则立即生效，如果为false，则调用一次nextval后才会生效 |



**举例**

1. 获取当前序列

   ``` sql
   select currval('序列名')
   ```

   

# Mybatis-Plus中配置

主键字段注解添加: type = IdType.AUTO

![image-20220506101946230](https://tva1.sinaimg.cn/large/e6c9d24ely1h1ygxduxgkj20ui0aodgi.jpg)
