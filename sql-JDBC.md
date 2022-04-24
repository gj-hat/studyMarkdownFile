@[toc]
# JDBC相关

## 1. 下载和导入jdbc

1. https://dev.mysql.com/downloads/connector/j/

2. 选择:Platform Independent

3. 下载zip包

4. 创建项目,给项目添加lib目录

5. 将zip包中的jar包复制到lib目录

6. IDEA相关操作

   6.1 先点击项目,再点击File->Projecrt Structure

   6.2 点击左侧moudle

   <img src="https://gitee.com/thisisafigurebed/figure-bed/raw/master/servletNode/6.png" style="zoom:70%">

## 2. JDBC代码实现

### 2.1 JDBC开发步骤

+ 加载驱动
+ 获取连接
+ 基本操作
+ 释放资源

### 2.2 代码实现

```js
import java.sql.*;

/*
 * JDBC入门项目
 *
 * */
public class jdbcDemo1 {
    public static void main(String[] args) throws SQLException, ClassNotFoundException {
        jdbcDemo1 j = new jdbcDemo1();
        j.demo1();

    }
    public void demo1() throws ClassNotFoundException, SQLException {
        //加载驱动
        Class.forName("com.mysql.cj.jdbc.Driver");
        //获得连接
        Connection conn = DriverManager.getConnection("jdbc:mysql://localhost:3306/web_test3", "root", "321321");
        Statement statement = conn.createStatement();
        //基本操作
        String sql = "select * from user";
        ResultSet rs = statement.executeQuery(sql);
        while (rs.next()) {
            System.out.print(rs.getInt("id") + " ");
            System.out.print(rs.getString("username") + " ");
            System.out.print(rs.getString("password") + " ");
            System.out.print(rs.getString("nickname") + " ");
            System.out.print(rs.getInt("age") + " ");
            System.out.println();
        }
        //释放资源
        rs.close();
        statement.close();
        conn.close();
    }
}

```
  <img src="https://gitee.com/thisisafigurebed/figure-bed/raw/master/servletNode/1.png" style="zoom:70%">

## 3.JDBC的API之DriverManage

> - 管理一套JDBC drivers.
>   **注：**的DataSource接口的基本服务，新的JDBC  2 API，提供了另一种方式来连接数据源。一个`DataSource`使用对象是连接到数据源的首选手段。

### 3.1 作用一:注册驱动

```java
static void registerDriver(Driver driver)     //注册了 DriverManager驱动
```

可以完成驱动的注册,但是实际开发中不使用这个方法完成驱动的注册

例如:  若使用则语句   `DriverManage.registerDriver(new Driver());

查看jdbc的Driver代码可见, 有一个静态代码块以及完成注册
  <img src="https://gitee.com/thisisafigurebed/figure-bed/raw/master/servletNode/2.png" style="zoom:70%">

### 3.2 作用二:获得连接
```java
static Connection getConnection(String url, String user,  String password) 
```

## 4.JDBC的API之Connection:与数据库连接对象
> 一个特定数据库的连接（会话）。SQL语句的执行结果是在一个连接上下文返回结果。

### 4.1 作用一:创建执行sql语句的对象

```java
Statement createStatement()    //创建用于向数据库发送SQL语句的Statement对象。
CallableStatement prepareCall(String sql)    //创建一个 CallableStatement对象调用数据库存储过程。
PreparedStatement prepareStatement(String sql)      //创建参数化的SQL语句发送到数据库的 PreparedStatement对象。 
```
**常用执行SQL语句的对象 **

+ Statement						:执行SQL语句
+ CallableStatement          :执行数据库中存储过程
+ PreparedStatement        :执行SQL对SQL进行预处理,可以解决SQL注入漏洞

### 4.2 管理事务

```java
void setAutoCommit(boolean autoCommit)      //将此连接的自动提交模式设置为给定的状态。(开启事务)
void commit()      					//使自上次提交/回滚永久释放任何数据库锁的 Connection对象目前持有的所有更改。
void rollback()          //撤消所有更改在当前事务并释放任何数据库锁的 Connection对象目前持有。
```

## 5. JDBC的API之Statement

> 该对象用于执行静态SQL语句并返回它产生的结果。
>
> 两个子接口:CallableStatement ,PreparedStatement 

### 5.1 执行SQL语句

```java
boolean execute(String sql)       //执行给定的SQL语句，可以返回多个结果。   
ResultSet executeQuery(String sql)        //执行给定的SQL语句，返回一个 ResultSet对象。  
int executeUpdate(String sql)      //执行给定的SQL语句，这可能是一个 INSERT， UPDATE，或 DELETE语句或SQL语句不返回值，例如SQL DDL语句。 

```

+ boolean execute(String sql)
  + 可以执行增删改查操作返回布尔类型
+ ResultSet executeQuery(String sql)      **常用**
  + 专门用来执行查询(select).返回的是一个ResultSet类型
+ int executeUpdate(String sql)            **常用**
  + 专门用来执行增加,删除,修改操作.	返回影响的行数



### 5.2 执行批处理

```java
void addBatch(String sql)      //将给定的SQL命令的命令的当前列表这 Statement对象。  
void clearBatch()              //把这 Statement SQL命令对象的当前列表。 
int[] executeBatch()           //  向数据库提交一个命令到执行，如果所有的命令都成功执行，则返回一个更新计数的数组。 
```



## 6. JDBC的API之ResultSet

> 表示数据库结果集的数据表，它通常是通过执行查询数据库的语句生成的。(返回的是结果集)
>
> 通过select语句的查询结果(只要select查询才有结果集)

### 6.1 结果集的遍历

```java
boolean next()         //从当前位置移动光标向前一行。  
```



```java
 while (rs.next()) {
            System.out.print(rs.getInt("id") + " ");
            System.out.print(rs.getString("username") + " ");
            System.out.print(rs.getString("password") + " ");
            System.out.print(rs.getString("nickname") + " ");
            System.out.print(rs.getInt("age") + " ");
            System.out.println();
        }
```





### 6.2 结果集的获取

> 各种get方法

```java
int getInt(String columnLabel) 
long getLong(String columnLabel)  
String getString(String columnLabel) 
Object getObject(String columnLabel)      //以上方法通吃
```

**getXXX()  一般有两个重载方法   各种巴拉巴拉的方法**

+ getXXX(int columnIndex)     //传入列号
+ getXXX(String columnName)     //传入列名
+ Object getObject(String columnLabel)      //以上方法通吃

## 7. JDBC资源释放

> JDBC程序结束后一定要将与数据库连接的对象释放 Connection,Statement,ResultSet其中Connection一定要做到晚创建早释放
>
> 将释放代码写道finally的代码块内(finally中的内容一定会被程序执行)

**标准资源释放代码**

```java
import java.sql.*;

/*
 * JDBC入门项目
 *
 * */
public class jdbcDemo1 {
    public static void main(String[] args) {
        jdbcDemo1 j = new jdbcDemo1();
        j.demo1();

    }
    public void demo1() {
        Connection conn = null;
        Statement statement = null;
        ResultSet rs = null;
        try {
            //加载驱动
            Class.forName("com.mysql.cj.jdbc.Driver");
            //获得连接
            conn = DriverManager.getConnection("jdbc:mysql://localhost:3306/web_test3", "root", "321321");
            statement = conn.createStatement();
            //基本操作
            String sql = "select * from user";
            rs = statement.executeQuery(sql);
            while (rs.next()) {
                System.out.print(rs.getInt("id") + " ");
                System.out.print(rs.getString("username") + " ");
                System.out.print(rs.getString("password") + " ");
                System.out.print(rs.getString("nickname") + " ");
                System.out.print(rs.getInt("age") + " ");
                System.out.println();
            }
        } catch (Exception e) {
            e.printStackTrace();
        } finally {             //标准资源释放代码
            if (rs != null) {
                try {
                    rs.close();
                } catch (SQLException e) {
                    e.printStackTrace();
                }
                rs = null;   //垃圾回收机制更早更快的回收资源
            }
            if (statement != null) {
                try {
                    statement.close();
                } catch (SQLException e) {
                    e.printStackTrace();
                }
                statement = null;   //垃圾回收机制更早更快的回收资源
            }
            if (conn != null) {
                try {
                    conn.close();
                } catch (SQLException e) {
                    e.printStackTrace();
                }
               conn = null;   //垃圾回收机制更早更快的回收资源
            }
        }
    }
}

```

## 8. JDBC增删改查CRUD操作

### 8.1 添加操作

```java
import java.sql.*;
/*
 * JDBC添加操作
 *
 * */
public class jdbcDemo1 {
    public static void main(String[] args) {
        Connection conn = null;
        Statement statement = null;
        try {
            //注册驱动
            Class.forName("com.mysql.cj.jdbc.Driver");
            //获得连接
            conn = DriverManager.getConnection("jdbc:mysql://127.0.0.1:3306/web_test3", "root", "321321");
            statement = conn.createStatement();

            //基本操作

            String sql = "insert into user value(null,'eee','123','阿黄',21)";

            int i = statement.executeUpdate(sql);

            if (i > 0) {
                System.out.println("添加成功");
            } else {
                System.out.println("添加失败");
            }

        } catch (Exception e) {
            e.printStackTrace();
        } finally {

            if (conn != null) {
                try {
                    conn.close();
                } catch (Exception e) {
                    e.printStackTrace();
                } finally {
                    conn = null;
                }
            }
            if (statement != null) {
                try {
                    statement.close();
                } catch (Exception e) {
                    e.printStackTrace();
                } finally {
                    statement = null;
                }
            }
        }
    }
}
```

### 8.2 修改操作

```java
import java.sql.*;
/*
 * JDBC添加操作
 *
 * */
public class jdbcDemo1 {
    public static void main(String[] args) {
        Connection conn = null;
        Statement statement = null;
        try {
            //注册驱动
            Class.forName("com.mysql.cj.jdbc.Driver");
            //获得连接
            conn = DriverManager.getConnection("jdbc:mysql://127.0.0.1:3306/web_test3", "root", "321321");
            statement = conn.createStatement();

            //基本操作

            String sql = "update user set nickname = '阿黄改过' where id = 4";

            int i = statement.executeUpdate(sql);

            if (i > 0) {
                System.out.println("修改成功");
            } else {
                System.out.println("修改失败");
            }

        } catch (Exception e) {
            e.printStackTrace();
        } finally {

            if (conn != null) {
                try {
                    conn.close();
                } catch (Exception e) {
                    e.printStackTrace();
                } finally {
                    conn = null;
                }
            }
            if (statement != null) {
                try {
                    statement.close();
                } catch (Exception e) {
                    e.printStackTrace();
                } finally {
                    statement = null;
                }
            }
        }
    }
}
```



###  8.3 删除操作

```java
import java.sql.*;
/*
 * JDBC添加操作
 *
 * */
public class jdbcDemo1 {
    public static void main(String[] args) {
        Connection conn = null;
        Statement statement = null;
        try {
            //注册驱动
            Class.forName("com.mysql.cj.jdbc.Driver");
            //获得连接
            conn = DriverManager.getConnection("jdbc:mysql://127.0.0.1:3306/web_test3", "root", "321321");
            statement = conn.createStatement();

            //基本操作

            String sql = "delete from user where id = 4";

            int i = statement.executeUpdate(sql);

            if (i > 0) {
                System.out.println("删除成功");
            } else {
                System.out.println("删除失败");
            }

        } catch (Exception e) {
            e.printStackTrace();
        } finally {

            if (conn != null) {
                try {
                    conn.close();
                } catch (Exception e) {
                    e.printStackTrace();
                } finally {
                    conn = null;
                }
            }
            if (statement != null) {
                try {
                    statement.close();
                } catch (Exception e) {
                    e.printStackTrace();
                } finally {
                    statement = null;
                }
            }
        }
    }
}
```

### 8.4查询操作

上面有演示

## 9. JDBC工具类的抽取

> 因为传统的jdbc开发注册驱动,获得连接,释放资源的操作都是重复的,所以将重复的代码提取到一个类中

```java
import java.sql.*;

public class jdbcUtils {
    private static final String classForName;
    private static final String url;
    private static final String username;
    private static final String password;

    static {
        classForName = "com.mysql.cj.jdbc.Driver";
        url = "jdbc:mysql://127.0.0.1:3306/web_test3";
        username = "root";
        password = "321321";
    }

    /*
     * 驱动注册
     */
    public static void loadDriver() {
        try {
            Class.forName(classForName);
        } catch (Exception e) {
            e.printStackTrace();
        }
    }

    /*
     * 获得连接
     */
    public static Connection getConnection() {
        Connection connection = null;
        try {
            loadDriver();
            connection = DriverManager.getConnection(url, username, password);
        } catch (Exception e) {
            e.printStackTrace();
        }
        return connection;

    }

    /*
     * 释放资源,方法重载   增删改一个查一个
     */

    public static void release(Statement statement, Connection connection) {

        if (connection != null) {
            try {
                connection.close();
            } catch (Exception e) {
                e.printStackTrace();
            } finally {
                connection = null;
            }
        }
        if (statement != null) {
            try {
                statement.close();
            } catch (Exception e) {
                e.printStackTrace();
            } finally {
                statement = null;
            }
        }

    }
    public static void release(Statement statement, Connection connection, ResultSet resultSet) {
        if (resultSet != null) {
            try {
                resultSet.close();
            } catch (SQLException e) {
                e.printStackTrace();
            }
            resultSet = null;   //垃圾回收机制更早更快的回收资源
        }
        if (statement != null) {
            try {
                statement.close();
            } catch (SQLException e) {
                e.printStackTrace();
            }
            statement = null;
        }
        if (connection != null) {
            try {
                connection.close();
            } catch (SQLException e) {
                e.printStackTrace();
            }
            connection = null;
        }
    }
}



import java.sql.Connection;
import java.sql.ResultSet;
import java.sql.Statement;

public class jdbcDemo1 {
    public static void main(String[] args) {
        Statement statement = null;
        Connection connection = null;
        ResultSet resultSet = null;
        try {
            connection = jdbcUtils.getConnection();
            statement = connection.createStatement();
            String sql = "select * from user";
            resultSet = statement.executeQuery(sql);

            while (resultSet.next()) {
                System.out.print(resultSet.getInt("id") + " ");
                System.out.print(resultSet.getString("username") + " ");
                System.out.print(resultSet.getString("password") + " ");
                System.out.print(resultSet.getString("nickname") + " ");
                System.out.print(resultSet.getInt("age") + " ");
                System.out.println();
            }
        } catch (Exception e) {
            e.printStackTrace();
        } finally {
            jdbcUtils.release(statement, connection, resultSet);
        }
    }
}
```

## 10.JDBC配置信息提取到配置文件

### 10.1 配置文件

+ 属性文件
  + 格式: 拓展名是`.properties`
  + 内容: key = value

### 10.2 提取信息到配置文件

+ 定义一个配置文件

+ 在src下新建一个xx.properties文件<img src="3.png" style="zoom:40%">

+ 在工具类中解析属性文件

+ 获取到具体内容为场常量赋值

+ 改写上述工具类中static内容<img src="4.png" style="zoom:30%">

  ```java
      static {
          Properties properties = new Properties();
          try {
              properties.load(new FileInputStream("src/db.properties"));
          } catch (IOException e) {
              e.printStackTrace();
          }
          classForName = properties.getProperty("classForName");;
          url = properties.getProperty("url");;
          username = properties.getProperty("username");;
          password = properties.getProperty("password");;
      }
  ```

## 11.SQL注入漏洞`preparedStatement`对象

### 11.1什么是SQL注入漏洞

  > 知道用户名不知道密码也可以登录

  ### 11.2 比如

  用户名是 aaa '  or ' 1=1

  用户名是aaa -- 

  <img src="5.png" style="zoom:70%">

### 11.3解决方法

需要采用`preparedStatement`对象解决SQL注入漏洞.

这个对象会将sql语句预先进行编译。使用?作为占位符。

?代表的内容是SQL所固定。再次传入变量(包含SQL关键字)。这时候也不会识别这些关键字。

```java
//解决SQL注入漏洞   
public boolean login(String user, String psd) {
        Connection connection = null;
        PreparedStatement preparedStatement = null;
        ResultSet resultSet = null;
        //定义一个变量
        boolean flag = false;
        try {
            //获得连接
            connection = jdbcUtils.getConnection();
            //编写SQL语句
            String sql = "select * from user where username = ? and password = ?";
            //预编译sql
            preparedStatement = connection.prepareStatement(sql);
            //设置参数   1,2,3分别代表第几个问号
            preparedStatement.setString(1, user);
            preparedStatement.setString(2, psd);
            resultSet = preparedStatement.executeQuery();
            if (resultSet.next()) {
                flag = true;
                System.out.printf("登录成功");
            } else {
                flag = false;
                System.out.println("登录失败");
            }
        } catch (Exception e) {
            e.printStackTrace();
        } finally {
            jdbcUtils.release(preparedStatement,connection,resultSet);
        }
        return flag;
    }
```

### 11.4使用`preparedStatement`对象保存

```java
    public void insertDate() {
        Connection connection = null;
        PreparedStatement preparedStatement = null;
        //定义一个变量
        boolean flag = false;
        try {
            //获得连接
            connection = jdbcUtils.getConnection();
            //编写SQL语句
            String sql = "insert into user values (null,?,?,?,?)";
            //预编译sql
            preparedStatement = connection.prepareStatement(sql);
            //设置参数   1,2,3分别代表第几个问号
            preparedStatement.setString(1, "fff");
            preparedStatement.setString(2, "123");
            preparedStatement.setString(3, "小狗");
            preparedStatement.setInt(4, 21);
            int i = preparedStatement.executeUpdate();
            if (i > 0) {
                System.out.println("写入成功");
            } else {
                System.out.println("写入失败");
            }
        } catch (Exception e) {
            e.printStackTrace();
        } finally {
            jdbcUtils.release(preparedStatement,connection);
        }
    } 
```

## 12. JDBC的批处理操作

> 批处理就是一组语句一起处理

### 12.1 批处理的使用

#### 12.1.1使用Statsment对象

```java
void addBatch(String sql) 		//将给定的SQL命令的命令的当前列表这 Statement对象。  
void clearBatch() 			//把这 Statement SQL命令对象的当前列表。 
int[] executeBatch() 		//向数据库提交一个命令到执行，如果所有的命令都成功执行，则返回一个更新计数的数组。  
```

```java
 public void insertDate() {
    Connection connection = null;
    PreparedStatement preparedStatement = null;
        Statement statement = null;
    try {
        connection  = jdbcUtils.getConnection();
        statement = connection.createStatement();
        String sql1 = "create database test1";
        String sql2 = "use test1";
        String sql3 = "create table user(id int primary key auto_increment,name varchar(20))";
        String sql4 = "insert into user values(null,'张三')";
        String sql5 = "insert into user values(null,'李四')";
        String sql6 = "insert into user values(null,'王五')";
        String sql7 = "update user set name = '啦啦啦' where id = 2";
        String sql8 = "delete from user where id = 3";
        statement.addBatch(sql1);
        statement.addBatch(sql2);
        statement.addBatch(sql3);
        statement.addBatch(sql4);
        statement.addBatch(sql5);
        statement.addBatch(sql6);
        statement.addBatch(sql7);
        statement.addBatch(sql8);
        int[] sum = statement.executeBatch();
        System.out.println("--------");
        for (int a : sum) {
            System.out.println(a);
        }
    } catch (Exception e) {
            e.printStackTrace();
        } finally {
            jdbcUtils.release(preparedStatement,connection);
        }
    }
```

#### 12.1.2使用PreparedStatement对象

```java
void addBatch() 		//增加了一个参数集的命令这 PreparedStatement对象的批。  
void clearBatch() 			//把这 Statement SQL命令对象的当前列表。 
int[] executeBatch() 		//向数据库提交一个命令到执行，如果所有的命令都成功执行，则返回一个更新计数的数组。 
```

`url = jdbc:mysql://127.0.0.1:3306/test1?rewriteBatchedStatements=true`

<font color=red size=5>以上操作开启mysql批处理功能.</font>

MySQL JDBC驱动在默认情况下会无视executeBatch()语句，把我们期望批量执行的一组sql语句拆散，一条一条地发给MySQL数据库，批量插入实际上是单条插入，直接造成较低的性能。
只有把rewriteBatchedStatements参数置为true, 驱动才会帮你批量执行SQL

```
public class jdbcDemo1 {
    public static void main(String[] args) {
        long l1 = System.currentTimeMillis();
        jdbcDemo1 t = new jdbcDemo1();
       t.insertDate();
        long l2 = System.currentTimeMillis();
        System.out.println("花费时间:"+(l2-l1));
    }
    public void insertDate() {
    Connection connection = null;
    PreparedStatement preparedStatement = null;
    try {
        connection  = jdbcUtils.getConnection();
        String sql  = "insert into user values (null,?)";
        preparedStatement = connection.prepareStatement(sql);

        for (int i = 1; i <= 10000; i++) {
            preparedStatement.setString(1,"name"+i);
            preparedStatement.addBatch();
            //数据过大内存容易溢出 所以每1000条提交并清理一次
            if (i % 1000 == 0) {
                preparedStatement.executeBatch();
                preparedStatement.clearBatch();
            }
        }
        System.out.println("添加成功");
    } catch (Exception e) {
            e.printStackTrace();
        } finally {
            jdbcUtils.release(preparedStatement,connection);
        }
    }
}
```
 批处理错误示范<font zize = 4 color = red>没弄明白</font>

```java
public void insertDate() {
    Connection connection = null;
    PreparedStatement preparedStatement = null;
    try {
        connection  = jdbcUtils.getConnection();
        String sql1  = "create table account (id int primary key auto_increment,name varchar(20),money double)";
        String sql2  = "insert into account values (null,'aaa',10000)";
        String sql3  = "insert into account values (null,'bbb',10000)";
        String sql4  = "insert into account values (null,'ccc',10000)";
        String sql5  = "insert into account values (null,'ddd',10000)";
        String sql6  = "insert into account values (null,'eee',10000)";
        String sql7  = "insert into account values (null,'fff',10000)";
        String sql8  = "insert into account values (null,'ggg',10000)";
        preparedStatement = connection.prepareStatement(sql1);
        preparedStatement.addBatch();
        preparedStatement = connection.prepareStatement(sql2);
        preparedStatement.addBatch();
        preparedStatement = connection.prepareStatement(sql3);
        preparedStatement.addBatch();
        preparedStatement = connection.prepareStatement(sql4);
        preparedStatement.addBatch();
        preparedStatement = connection.prepareStatement(sql5);
        preparedStatement.addBatch();
        preparedStatement = connection.prepareStatement(sql6);
        preparedStatement.addBatch();
        preparedStatement = connection.prepareStatement(sql7);
        preparedStatement.addBatch();
        preparedStatement = connection.prepareStatement(sql8);
        preparedStatement.addBatch();
        preparedStatement.executeBatch();
        preparedStatement.clearBatch();

        System.out.println("添加成功");
    } catch (Exception e) {
            e.printStackTrace();
        } finally {
            jdbcUtils.release(preparedStatement,connection);
        }
    }
```



## 13. JDBC事务管理

### 13.1 无事务操作

没有添加事务管理执行完1出现异常操作2没有执行.

```java
public void insertDate() {
    Connection connection = null;
    PreparedStatement preparedStatement = null;
    try {
        /*
             * account转账操作
             */
        connection = jdbcUtils.getConnection();

        String sql1 = "update account set money = money + ? where name = ? ";
        //预编译
        //aaa减1000
        preparedStatement = connection.prepareStatement(sql1);
        preparedStatement.setDouble(1, -1000);
        preparedStatement.setString(2, "aaa");
        preparedStatement.executeUpdate();

        //手动添加一个异常
        int i = 1/0;

        //bbb加1000
        preparedStatement.setDouble(1, 1000);
        preparedStatement.setString(2, "bbb");
        preparedStatement.executeUpdate();

        System.out.println("转账完成");
    } catch (Exception e) {
        e.printStackTrace();
    } finally {
        jdbcUtils.release(preparedStatement, connection);
    }
}
```

### 13.2 添加事务

#### 13.2.1事务管理API

`Connection`对象提供了一些事务操作

```java
void setAutoCommit(boolean autoCommit)      //将此连接的自动提交模式设置为给定的状态。(开启事务)
void commit()      			//使自上次提交/回滚永久释放任何数据库锁的 Connection对象目前持有的所有更改。
void rollback()          //撤消所有更改在当前事务并释放任何数据库锁的 Connection对象目前持有。
```

#### 13.2.2示例事务代码

```java
public void insertDate() {
    Connection connection = null;
    PreparedStatement preparedStatement = null;
    try {
        /*
             * account转账操作
             */
        connection = jdbcUtils.getConnection();
        //开启事务
        connection.setAutoCommit(false);
        String sql1 = "update account set money = money + ? where name = ? ";
        //预编译
        //aaa减1000
        preparedStatement = connection.prepareStatement(sql1);
        preparedStatement.setDouble(1, -1000);
        preparedStatement.setString(2, "aaa");
        preparedStatement.executeUpdate();

        //手动添加一个异常
        //int i = 1/0;

        //bbb加1000
        preparedStatement.setDouble(1, 1000);
        preparedStatement.setString(2, "bbb");
        preparedStatement.executeUpdate();

        //提交事务
        connection.commit();

        System.out.println("转账完成");
    } catch (Exception e) {
        try {
            //出现异常事务回滚
            connection.rollback();
        } catch (SQLException throwables) {
            throwables.printStackTrace();
        }
        e.printStackTrace();
    } finally {
        jdbcUtils.release(preparedStatement, connection);
    }
}
```

