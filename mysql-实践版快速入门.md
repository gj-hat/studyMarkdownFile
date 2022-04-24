[TOC]

# mysql数据库

> 数据库database，简而言之可视为电子化的文件柜——存储电子文件的处所，用户可以对文件中的数据进行新增、截取、更新、删除等操作。
> 所谓“数据库”是以一定方式储存在一起、能与多个用户共享、具有尽可能小的冗余度、与应用程序彼此独立的数据集合。
> 表是组成数据库的基本单位，简单来说甚至一张excel也是一个数据库

## mysql和相关软件下载

1. windows端下载
   [http://https://dev.mysql.com/downloads/mysql/](http://https//dev.mysql.com/downloads/mysql/)
2. linux端下载
   debian系：sudo apt-get install mysql
   redhat系：yum install mysql

本文以windows端为例
主要在命令行方式下进行操作，图形界面为辅。
需要用到的软件：navicat for mysql | workbench
前者收费软件可用替代软件：heidisql
后者官方软件下载地址：https://www.mysql.com/cn/products/workbench/

## mysql安装

通过前文所说的下载为官方免安装版，下载后直接解压到任意目录即可
修改如下配置：

```
在文件主目录创建mysql.ini文件
内容如下：
[mysql]
# 设置mysql客户端默认字符集
default-character-set=utf8
 
 [mysqld]
 # 设置3306端口
 port = 3306
 # 设置mysql的安装目录
 # datadir=C:\\web\\sqldata
 # 允许最大连接数
 max_connections=20
 # 服务端使用的字符集默认为8比特编码的latin1字符集
 character-set-server=utf8
 # 创建新表时将使用的默认存储引擎
 default-storage-engine=INNODB
```



验证是否安装成功
在cmd下进入mysql文件夹/bin目录，输入如下初始化命令

```
mysqld --initialize --console
```



收到如下配置则初始化成功

```
...
2018-04-20T02:35:05.464644Z 5 [Note] [MY-010454] [Server] A temporary password is generated for root@localhost: APWCY5ws&hjQ
...
```



其中APWCY5ws&hjQ是数据库登录的密码,后续可以进行更改

安装和启动命令如下：

```
mysqld install
net start mysql
```



## 登录mysql

还是在mysql/bin目录下：

```
mysql -u root -p
```



回车后提示输入密码，输入刚刚的密码即可登录

修改密码

```
mysqladmin -uroot -p123； password 456
```



解释：123为原密码，456为更改后的密码

## mysql基本命令

在数据库的操作界面内：

```
show databases;
```



显示数据库的名称（因为数据库有多个，所以需要加复数形式）

```
CREATE DATABASES test
```

新建test数据库

```
use test
```

使用use数据库，即进入test数据库中

**正如前文所说，数据库是由一张张表组成**
接下来先来新建表：

```
CREATE TABLE test1（
Tno INT,
Tname CHAR(10),
Tsex CHAR(1)
）；
```



新建test1表:
表包括Tno，Tname，Tsex 三列
（mysql数据库不分大小写，命名基本采用驼峰法命名）

```
show COLUMNS FROM test1;
```



显示test表的列属性。

```
show TABLE STATUS FROM test；
```

显示test数据库的性能和统计信息

```
ALTER test1
```

修改test1表
修改操作包括：

1. 增加主键

2. 删除主键

3. 增加和删除外键

4. 增加和删除其他索引

   ## 软件介绍

   ### 前文提到的几个软件做简单的介绍

5. navicat for mysql和 heidisql
   是比较人性化的图形化界面的软件，在其中可以以鼠标的形式进行增删改等的操作（初学比较建议从命令行入手，从命令行更能深刻的了解数据库的基本结构原理）

6. workbench是官方的UI软件
   增删改的操作基本包含上述两个软件但是对于新手不够友好
   比上面的两个软件多的功能是可以绘制E-R图，（E-R图是数据库的骨架）
   可以通过绘制E-R图自动生成数据库也可以根据已有的数据库生成E-R图

   ## 放一个我的在线考试系统的数据库结构吧

   ### E-R图如下

   [![img](https://s2.ax1x.com/2019/03/22/A8StI0.png)](https://s2.ax1x.com/2019/03/22/A8StI0.png)

   ### 数据库构成如下

   ```sql
   CREATE TABLE user(          //系统用户
   user_id int primary key,
   user_password CHAR(16) DEFAULT 123456,
   user_identity CHAR(3)
   );
   
   
   create TABLE student(      //学生
   student_no int primary key,
   student_name CHAR(5),
   student_sex CHAR(1),
   );
   ALTER user
   ADD CONSTRAINT `fk_student_no` FOREIGN KEY (`student_no`) REFERENCES `user` (`user_no`) ON DELETE CASCADE ON UPDATE CASCADE;
   
   
   
   CREATE TABLE teacher(      //教师
   teacher_no INT PRIMARY KEY,
   teacher_name CHAR(5),
   teacher_sex CHAR(1),
   teacher_subject CHAR(10)
   );
   ALTER teacher
   ADD CONSTRAINT `fk_teacher_no ` FOREIGN KEY (`teacher_no`) REFERENCES `user` (`user_no`) ON DELETE CASCADE ON UPDATE CASCADE;
   ADD CONSTRAINT `fk_teacher_subject ` FOREIGN KEY (`teacher_subject`) REFERENCES `subject` (`subject_name`) ON DELETE CASCADE ON UPDATE CASCADE
   
   CREATE TABLE subject(      //科目
   subject_no INT primary KEY,
   subject_name CHAR(10)
   );
   
   CREATE TABLE chapter (     //章节
   chapter_subject_name CHAR(10),
   chapter_name CHAR(10),
   PRIMARY KEY(chapter_subject_no,chapter_name)
   FOREIGN KEY  fk_chapter_subject_name(chapter_subject_name) REFERENCES `subject` (`subject_name`)
   );
   
   
   
   
   CREATE TABLE port(     //知识点
   port_subject_name CHAR(10) ,
   port_name CHAR(100) ,
   PRIMARY KEY(port_subject_no,port_name) 
   );
   ALTER port
   ADD CONSTRAINT `fk_port_subject_name ` FOREIGN KEY (`port_subject_name`) REFERENCES `user` (`subject_name`) ON DELETE CASCADE ON UPDATE CASCADE;
   
   
   
   CREATE TABLE question (        //题目
   question_no INT,
   question_subject_name CHAR(10),
   question_type CHAR (3),
   question_content CHAR (100),
   question_answer CHAR (100),
   question_difficult CHAR (2),
   question_port CHAR (10) ,
   question_chapter CHAR (10) ,
   PRIMARY KEY (question_no,question_subject_name),
   
   );
   
   ALTER TABLE `question` 
   ADD CONSTRAINT `fk_question_subject_no` FOREIGN KEY (`question_subject_name`) REFERENCES `subject` (`subject_name`) ON DELETE CASCADE ON UPDATE CASCADE;
   
   ALTER TABLE `exam`.`chapter` ADD INDEX (`chapter_name`);
   ALTER TABLE `question` 
   ADD CONSTRAINT `fk_question_chapter` FOREIGN KEY (`chapter_port`) REFERENCES `chapter` (`chapter_name`) ON DELETE CASCADE ON UPDATE CASCADE;
   
   ALTER TABLE `exam`.`port` ADD INDEX (`port_name`);
   ALTER TABLE `question` 
   ADD CONSTRAINT `fk_question_port` FOREIGN KEY (`question_port`) REFERENCES `port` (`port_name`) ON DELETE CASCADE ON UPDATE CASCADE;
   
   CREATE TABLE optn(     //选项表
   optn_question_no INT  ,
   optn_content CHAR(100) UNIQUE
   );
   ALTER TABLE `exam`.`question` ADD INDEX (`question_no`);
   ALTER TABLE `optn` 
   ADD CONSTRAINT `fk_optn_question_no` FOREIGN KEY (`optn_question_no`) REFERENCES `question` (`question_no`) ON DELETE CASCADE ON UPDATE CASCADE;
   
   create TABLE test_question(        //题目—试题关系表
   test_no INT ,
   test_question_no INT  ,
   test_subject_no INT,
   FOREIGN KEY fk_test_question_no(test_question_no) REFERENCES `question`(`question_no`),
   FOREIGN KEY fk_test_subject_no(test_subject_no)  REFERENCES `subject`(`subject_no`) 
   );
   
   create TABLE test(         //试题表
   test_no INT PRIMARY KEY,
   test_subject_name CHAR(5),
   test_select INT,
   test_judge INT,
   test_ompletion INT,
   test_jianda INT
   
   ); 
   ALTER TABLE `exam`.`test_question` ADD INDEX (`test_no`);
   ALTER TABLE `test` 
   ADD CONSTRAINT `fk_test_no` FOREIGN KEY (`test_no`) REFERENCES `test_question` (`test_no`);
   ALTER TABLE `test` 
   ADD CONSTRAINT `kf_test_subject_name` FOREIGN KEY (`test_subject_name`) REFERENCES `subject` (`subject_name`) ON DEL
   ```