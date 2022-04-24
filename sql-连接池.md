[toc]

# 连接池

## 01_什么是连接池

> *连接池是创建和管理一个连接的缓冲池的技术，这些连接准备好被任何需要它们的线程使用。*

*连接池是装有连接的容器,使用连接的话,可以从连接池中获取,使用完后要将连接归还给连接池*

## 02_为什么要学习连接池

*因为连接的创建和销毁都是需要消耗时间的,在服务器初始化时候就初始化一些连接。把这些连接放入连接池中,使用的时候可以直接从内存中获取,使用完成后放回至连接池中。从内存中获取和归还效率远远高于创建和销毁。*

*图示:<img src="https://gitee.com/thisisafigurebed/figure-bed/raw/master/servletNode/1.png" style="zoom:30%">*

## 03_自定义连接池

### 3.1编写一个类实现DataSource接口

***`Interface DataSource`***

*这DataSource对象代表的物理数据源的连接工厂*

```java
Connection getConnection()          //试图建立这种 DataSource对象代表数据源连接。  
```

### 3.2 重写getConnection()方法

```java
//从连接池中获得连接的方法
public Connection getConnection() throws SQLException {
    Connection conn = connList.remove(0);
    return conn;
}
```

### 3.3 初始化多个连接在内存中

```java
//创建一个集合存储一些连接
private List<Connection> connList = new ArrayList<Connection>();
//初始化  初始化时候提供一些连接
public MyData(){
    //初始化3个
    for (int i = 1; i <= 3; i++) {
        //向集合中存储连接
        connList.add(jdbcUtils.getConnection());
    }
}
```

### 3.3编写归还连接的方法

```java
    //编写归还的代码
    public void addBack(Connection conn){
        connList.add(conn);
    }
```

### 3.4 合集

1. *创建一个类实现DataSource接口 (接口有很多子接口不用搭理)*

2. *初始化  定义一个构造方法进行初始化*

3. *仅仅实现getConnection()方法即可*

4. *编写一个归还的类*

5. *连接池完整示例如下*
   
   ```java
   import javax.sql.DataSource;
   import java.io.PrintWriter;
   import java.sql.Connection;
   import java.sql.SQLException;
   import java.sql.SQLFeatureNotSupportedException;
   import java.util.ArrayList;
   import java.util.List;
   import java.util.logging.Logger;
   
   public class MyData implements DataSource {
       //创建一个集合存储一些连接
       private List<Connection> connList = new ArrayList<Connection>();
       //初始化  初始化时候提供一些连接
       public MyData(){
           //初始化3个
           for (int i = 1; i <= 3; i++) {
               //向集合中存储连接
               connList.add(jdbcUtils.getConnection());
           }
       }
       @Override
       //从连接池中获得连接的方法
       public Connection getConnection() throws SQLException {
           Connection conn = connList.remove(0);
           return conn;
       }
       //编写归还的代码
       public void addBack(Connection conn){
           connList.add(conn);
       }
   
   
   
       @Override
       public Connection getConnection(String username, String password) throws SQLException {
           return null;
       }
   
       @Override
       public PrintWriter getLogWriter() throws SQLException {
           return null;
       }
   
       @Override
       public void setLogWriter(PrintWriter out) throws SQLException {
   
       }
   
       @Override
       public void setLoginTimeout(int seconds) throws SQLException {
   
       }
   
       @Override
       public int getLoginTimeout() throws SQLException {
           return 0;
       }
   
       @Override
       public Logger getParentLogger() throws SQLFeatureNotSupportedException {
           return null;
       }
   
       @Override
       public <T> T unwrap(Class<T> iface) throws SQLException {
           return null;
       }
   
       @Override
       public boolean isWrapperFor(Class<?> iface) throws SQLException {
           return false;
       }
   }
   ```
   
   
   
6. *测试连接池代码示例*

   ```java
   public void testDataSource() {
       Connection conn = null;
       PreparedStatement pstm = null;
       ResultSet rs = null;
       MyData myData = null;
       try {
           //获得连接
           //conn = jdbcUtils.getConnection();
   
           //从连接池获得连接
           myData = new MyData();
           conn = myData.getConnection();
   
           //编写SQL
           String sql = "select * from account";
   
           //预编译SQL
           pstm = conn.prepareStatement(sql);
           //设置参数
           //执行SQL
           rs = pstm.executeQuery();
           while (rs.next()) {
               System.out.print(rs.getInt("id")+" ");
               System.out.print(rs.getString("name")+" ");
               System.out.print(rs.getDouble("money")+" ");
               System.out.println();
           }
       } catch (SQLException e) {
           e.printStackTrace();
       }
       //归还
       myData.addBack(conn);
   }
   ```

### 3.5自定义连接池可能出现的问题

#### 3.5.1使用接口的实现类完成的构造

```java
DataSource ds = new DataSource();
```

*这种写法不方便程序扩展*

*应该这么写*

```java
DataDouce ds = null;
ds = new DataSource();
```

#### 3.5.2 额外的提供了方法来归还连接

*addBack()方法是自己写的*

*如果有人需要使用,并不知道,归还方法名是什么*

```java
    //归还
    myData.addBack(conn);
```

### 3.6 自定义连接池问题的解决思路

*如果不提供自定义的方法就可以解决,但是,如何解决归还问题呢?*

*原来Connection从有一个colse()方法完成了连接的销毁,将原有的close()方法增强改写为归还*

+ *增强一个类的方法*

  + *继承   使用继承的局限性:原类要能控制构造(比如原类是一个接口找不到他的实现类就无法继承 )*

    ```java
    
    class Man{
        public void run(){
            sout("跑....");
        } 
    }
    class SuperMan extends Man{
        public void run(){
            sout("飞.....");
        }
    } 
    ```

  + *包装即装饰者模式*

    *使用条件 1. 增强的类和被增强的类必须实现相同的接口    2. 在增强类中获得被增强类的引用*

    ```java
    insterface Waiter(){
        public void server();
    }
    class Waiteress implements Waiter{
        publid void server(){
            sout("服务中....");
        }
    }
    class WaiteressWrapper implements Waiter{
        private Waiter waiter;
        public WaiteressWrapper(Waiter waiter){
            this.waiter = waiter;
        }
            publid void server(){
            sout("增强了....");
            waiter.server();
        }
    }
    ```

  + *动态代理方法*
### 3.7 自定义连接池问题的解决代码实现

*实现思路:   需求是重写增强接口里的一个方法*

1. *寻找原接口实现类*
2. *找是肯定找不到的,随便写一个接口的实现类2,里面构造方法实现一个原接口实现类1(因为类2也是接口的实现类,实现接口类就要实现里面所有方法.所以在类2里构造一个类1。对外看起来像是类2实现了接口其实类2里的方法都是类1的)*
3. *现在就有了三个类了取名:1.接口A   2. 原来实现类B(上面的类1)    3. 虚假的新实现类C(上面的类1)*
4. *新来一个类D  继承类C  (因为类C实现了接口   所以看起来是类D继承了C实际上则是类D实现了接口A)*
5. *在类D中就可以重写接口里的方法了.*

*具体实现代码:*

1. *A接口(系统有不用写)*

2. *类C*

   ```java
   /*
   * 模板类
   *
   *
   *
   * */
   public class ConnectionWrapper implements Connection {
   
    private Connection conn;
   
    public ConnectionWrapper(Connection conn) {
   //       super();
        this.conn = conn;
    }
   
    @Override
    public boolean isWrapperFor(Class<?> iface) throws SQLException {
        return conn.isWrapperFor(iface);
    }
   
    @Override
    public <T> T unwrap(Class<T> iface) throws SQLException {
        return conn.unwrap(iface);
    }
   
    @Override
    public void abort(Executor executor) throws SQLException {
        conn.abort(executor);
    }
   
    @Override
    public void clearWarnings() throws SQLException {
        conn.clearWarnings();
    }
   
    @Override
    public void close() throws SQLException {
        conn.close();
    }
   
    @Override
    public void commit() throws SQLException {
        conn.commit();
    }
   
    @Override
    public Array createArrayOf(String typeName, Object[] elements) throws SQLException {
        return conn.createArrayOf(typeName, elements);
    }
   
    @Override
    public Blob createBlob() throws SQLException {
        return conn.createBlob();
    }
   
    @Override
    public Clob createClob() throws SQLException {
        return conn.createClob();
    }
   
    @Override
    public NClob createNClob() throws SQLException {
        return conn.createNClob();
    }
   
    @Override
    public SQLXML createSQLXML() throws SQLException {
        return conn.createSQLXML();
    }
   
    @Override
    public Statement createStatement() throws SQLException {
        return conn.createStatement();
    }
   
    @Override
    public Statement createStatement(int resultSetType, int resultSetConcurrency) throws SQLException {
        return conn.createStatement(resultSetType, resultSetConcurrency);
    }
   
    @Override
    public Statement createStatement(int resultSetType, int resultSetConcurrency, int resultSetHoldability)
            throws SQLException {
        return conn.createStatement(resultSetType, resultSetConcurrency, resultSetHoldability);
    }
   
    @Override
    public Struct createStruct(String typeName, Object[] attributes) throws SQLException {
        return conn.createStruct(typeName, attributes);
    }
   
    @Override
    public boolean getAutoCommit() throws SQLException {
        return conn.getAutoCommit();
    }
   
    @Override
    public String getCatalog() throws SQLException {
        return conn.getCatalog();
    }
   
    @Override
    public Properties getClientInfo() throws SQLException {
        return conn.getClientInfo();
    }
   
    @Override
    public String getClientInfo(String name) throws SQLException {
        return conn.getClientInfo(name);
    }
   
    @Override
    public int getHoldability() throws SQLException {
        return conn.getHoldability();
    }
   
    @Override
    public DatabaseMetaData getMetaData() throws SQLException {
        return conn.getMetaData();
    }
   
    @Override
    public int getNetworkTimeout() throws SQLException {
        return conn.getNetworkTimeout();
    }
   
    @Override
    public String getSchema() throws SQLException {
        return conn.getSchema();
    }
   
    @Override
    public int getTransactionIsolation() throws SQLException {
        return conn.getTransactionIsolation();
    }
   
    @Override
    public Map<String, Class<?>> getTypeMap() throws SQLException {
        return conn.getTypeMap();
    }
   
    @Override
    public SQLWarning getWarnings() throws SQLException {
        return conn.getWarnings();
    }
   
    @Override
    public boolean isClosed() throws SQLException {
        return conn.isClosed();
    }
   
    @Override
    public boolean isReadOnly() throws SQLException {
        return conn.isReadOnly();
    }
   
    @Override
    public boolean isValid(int timeout) throws SQLException {
        return conn.isValid(timeout);
    }
   
    @Override
    public String nativeSQL(String sql) throws SQLException {
        return conn.nativeSQL(sql);
    }
   
    @Override
    public CallableStatement prepareCall(String sql) throws SQLException {
        return conn.prepareCall(sql);
    }
   
    @Override
    public CallableStatement prepareCall(String sql, int resultSetType, int resultSetConcurrency) throws SQLException {
        return conn.prepareCall(sql, resultSetType, resultSetConcurrency);
    }
   
    @Override
    public CallableStatement prepareCall(String sql, int resultSetType, int resultSetConcurrency,
            int resultSetHoldability) throws SQLException {
        return conn.prepareCall(sql, resultSetType, resultSetConcurrency, resultSetHoldability);
    }
   
    @Override
    public PreparedStatement prepareStatement(String sql) throws SQLException {
        return conn.prepareStatement(sql);
    }
   
    @Override
    public PreparedStatement prepareStatement(String sql, int autoGeneratedKeys) throws SQLException {
        return conn.prepareStatement(sql, autoGeneratedKeys);
    }
   
    @Override
    public PreparedStatement prepareStatement(String sql, int[] columnIndexes) throws SQLException {
        return conn.prepareStatement(sql, columnIndexes);
    }
   
    @Override
    public PreparedStatement prepareStatement(String sql, String[] columnNames) throws SQLException {
        return conn.prepareStatement(sql, columnNames);
    }
   
    @Override
    public PreparedStatement prepareStatement(String sql, int resultSetType, int resultSetConcurrency)
            throws SQLException {
        return conn.prepareStatement(sql, resultSetType, resultSetConcurrency);
    }
   
    @Override
    public PreparedStatement prepareStatement(String sql, int resultSetType, int resultSetConcurrency,
            int resultSetHoldability) throws SQLException {
        return conn.prepareStatement(sql, resultSetType, resultSetConcurrency, resultSetHoldability);
    }
   
    @Override
    public void releaseSavepoint(Savepoint savepoint) throws SQLException {
        conn.releaseSavepoint(savepoint);
    }
   
    @Override
    public void rollback() throws SQLException {
        conn.rollback();
    }
   
    @Override
    public void rollback(Savepoint savepoint) throws SQLException {
        conn.rollback(savepoint);
    }
   
    @Override
    public void setAutoCommit(boolean autoCommit) throws SQLException {
        conn.setAutoCommit(autoCommit);
    }
   
    @Override
    public void setCatalog(String catalog) throws SQLException {
        conn.setCatalog(catalog);
    }
   
    @Override
    public void setClientInfo(Properties properties) throws SQLClientInfoException {
        conn.setClientInfo(properties);
    }
   
    @Override
    public void setClientInfo(String name, String value) throws SQLClientInfoException {
        conn.setClientInfo(name, value);
    }
   
    @Override
    public void setHoldability(int holdability) throws SQLException {
        conn.setHoldability(holdability);
    }
   
    @Override
    public void setNetworkTimeout(Executor executor, int milliseconds) throws SQLException {
        conn.setNetworkTimeout(executor, milliseconds);
    }
   
    @Override
    public void setReadOnly(boolean readOnly) throws SQLException {
        conn.setReadOnly(readOnly);
    }
   
    @Override
    public Savepoint setSavepoint() throws SQLException {
        return conn.setSavepoint();
    }
   
    @Override
    public Savepoint setSavepoint(String name) throws SQLException {
        return conn.setSavepoint(name);
    }
   
    @Override
    public void setSchema(String schema) throws SQLException {
        conn.setSchema(schema);
    }
   
    @Override
    public void setTransactionIsolation(int level) throws SQLException {
        conn.setTransactionIsolation(level);
    }
   
    @Override
    public void setTypeMap(Map<String, Class<?>> map) throws SQLException {
        conn.setTypeMap(map);
    }
   
   }
   
   ```
   
   3. *类D*   
   
   ```java
   import java.sql.Connection;
   import java.sql.SQLException;
   import java.util.List;
   /*
   * 使用装饰者模式增强connection中close方法
   *
   *
   * */
   public class MyConnectionWrapper extends ConnectionWrapper{
   
       private final List<Connection> connList;
       Connection conn = null;
   
       public MyConnectionWrapper(Connection conn, List<Connection> connList) {
        super(conn);
           this.conn = conn;
        this.connList = connList;
       }
   
       //增强某个方法
       @Override
       public void close() throws SQLException {
   //        super.close();
           //归还连接
           connList.add(conn);
       }
   }
   ```

### 3.8代码过多总结一下

+ *类结构*
  *<img src="https://gitee.com/thisisafigurebed/figure-bed/raw/master/servletNode/2.png" style="zoom:70%">*

  ** 
  
+ *ConnectionWrapper*
```java
  import java.sql.Array;
  import java.sql.Blob;
  import java.sql.CallableStatement;
  import java.sql.Clob;
  import java.sql.Connection;
  import java.sql.DatabaseMetaData;
  import java.sql.NClob;
  import java.sql.PreparedStatement;
  import java.sql.SQLClientInfoException;
  import java.sql.SQLException;
  import java.sql.SQLWarning;
  import java.sql.SQLXML;
  import java.sql.Savepoint;
  import java.sql.Statement;
  import java.sql.Struct;
  import java.util.Map;
  import java.util.Properties;
  import java.util.concurrent.Executor;
  /*
  * 模板类
  *
  *
  *
  * */
  public class ConnectionWrapper implements Connection {
  
    private Connection conn;
  
    public ConnectionWrapper(Connection conn) {
  //        super();
        this.conn = conn;
    }
  
    @Override
    public boolean isWrapperFor(Class<?> iface) throws SQLException {
        return conn.isWrapperFor(iface);
    }
  
    @Override
    public <T> T unwrap(Class<T> iface) throws SQLException {
        return conn.unwrap(iface);
    }
  
    @Override
    public void abort(Executor executor) throws SQLException {
        conn.abort(executor);
    }
  
    @Override
    public void clearWarnings() throws SQLException {
        conn.clearWarnings();
    }
  
    @Override
    public void close() throws SQLException {
        conn.close();
    }
  
    @Override
    public void commit() throws SQLException {
        conn.commit();
    }
  
    @Override
    public Array createArrayOf(String typeName, Object[] elements) throws SQLException {
        return conn.createArrayOf(typeName, elements);
    }
  
    @Override
    public Blob createBlob() throws SQLException {
        return conn.createBlob();
    }
  
    @Override
    public Clob createClob() throws SQLException {
        return conn.createClob();
    }
  
    @Override
    public NClob createNClob() throws SQLException {
        return conn.createNClob();
    }
  
    @Override
    public SQLXML createSQLXML() throws SQLException {
        return conn.createSQLXML();
    }
  
    @Override
    public Statement createStatement() throws SQLException {
        return conn.createStatement();
    }
  
    @Override
    public Statement createStatement(int resultSetType, int resultSetConcurrency) throws SQLException {
        return conn.createStatement(resultSetType, resultSetConcurrency);
    }
  
    @Override
    public Statement createStatement(int resultSetType, int resultSetConcurrency, int resultSetHoldability)
            throws SQLException {
        return conn.createStatement(resultSetType, resultSetConcurrency, resultSetHoldability);
    }
  
    @Override
    public Struct createStruct(String typeName, Object[] attributes) throws SQLException {
        return conn.createStruct(typeName, attributes);
    }
  
    @Override
    public boolean getAutoCommit() throws SQLException {
        return conn.getAutoCommit();
    }
  
    @Override
    public String getCatalog() throws SQLException {
        return conn.getCatalog();
    }
  
    @Override
    public Properties getClientInfo() throws SQLException {
        return conn.getClientInfo();
    }
  
    @Override
    public String getClientInfo(String name) throws SQLException {
        return conn.getClientInfo(name);
    }
  
    @Override
    public int getHoldability() throws SQLException {
        return conn.getHoldability();
    }
  
    @Override
    public DatabaseMetaData getMetaData() throws SQLException {
        return conn.getMetaData();
    }
  
    @Override
    public int getNetworkTimeout() throws SQLException {
        return conn.getNetworkTimeout();
    }
  
    @Override
    public String getSchema() throws SQLException {
        return conn.getSchema();
    }
  
    @Override
    public int getTransactionIsolation() throws SQLException {
        return conn.getTransactionIsolation();
    }
  
    @Override
    public Map<String, Class<?>> getTypeMap() throws SQLException {
        return conn.getTypeMap();
    }
  
    @Override
    public SQLWarning getWarnings() throws SQLException {
        return conn.getWarnings();
    }
  
    @Override
    public boolean isClosed() throws SQLException {
        return conn.isClosed();
    }
  
    @Override
    public boolean isReadOnly() throws SQLException {
        return conn.isReadOnly();
    }
  
    @Override
    public boolean isValid(int timeout) throws SQLException {
        return conn.isValid(timeout);
    }
  
    @Override
    public String nativeSQL(String sql) throws SQLException {
        return conn.nativeSQL(sql);
    }
  
    @Override
    public CallableStatement prepareCall(String sql) throws SQLException {
        return conn.prepareCall(sql);
    }
  
    @Override
    public CallableStatement prepareCall(String sql, int resultSetType, int resultSetConcurrency) throws SQLException {
        return conn.prepareCall(sql, resultSetType, resultSetConcurrency);
    }
  
    @Override
    public CallableStatement prepareCall(String sql, int resultSetType, int resultSetConcurrency,
            int resultSetHoldability) throws SQLException {
        return conn.prepareCall(sql, resultSetType, resultSetConcurrency, resultSetHoldability);
    }
  
    @Override
    public PreparedStatement prepareStatement(String sql) throws SQLException {
        return conn.prepareStatement(sql);
    }
  
    @Override
    public PreparedStatement prepareStatement(String sql, int autoGeneratedKeys) throws SQLException {
        return conn.prepareStatement(sql, autoGeneratedKeys);
    }
  
    @Override
    public PreparedStatement prepareStatement(String sql, int[] columnIndexes) throws SQLException {
        return conn.prepareStatement(sql, columnIndexes);
    }
  
    @Override
    public PreparedStatement prepareStatement(String sql, String[] columnNames) throws SQLException {
        return conn.prepareStatement(sql, columnNames);
    }
  
    @Override
    public PreparedStatement prepareStatement(String sql, int resultSetType, int resultSetConcurrency)
            throws SQLException {
        return conn.prepareStatement(sql, resultSetType, resultSetConcurrency);
    }
  
    @Override
    public PreparedStatement prepareStatement(String sql, int resultSetType, int resultSetConcurrency,
            int resultSetHoldability) throws SQLException {
        return conn.prepareStatement(sql, resultSetType, resultSetConcurrency, resultSetHoldability);
    }
  
    @Override
    public void releaseSavepoint(Savepoint savepoint) throws SQLException {
        conn.releaseSavepoint(savepoint);
    }
  
    @Override
    public void rollback() throws SQLException {
        conn.rollback();
    }
  
    @Override
    public void rollback(Savepoint savepoint) throws SQLException {
        conn.rollback(savepoint);
    }
  
    @Override
    public void setAutoCommit(boolean autoCommit) throws SQLException {
        conn.setAutoCommit(autoCommit);
    }
  
    @Override
    public void setCatalog(String catalog) throws SQLException {
        conn.setCatalog(catalog);
    }
  
    @Override
    public void setClientInfo(Properties properties) throws SQLClientInfoException {
        conn.setClientInfo(properties);
    }
  
    @Override
    public void setClientInfo(String name, String value) throws SQLClientInfoException {
        conn.setClientInfo(name, value);
    }
  
    @Override
    public void setHoldability(int holdability) throws SQLException {
        conn.setHoldability(holdability);
    }
  
    @Override
    public void setNetworkTimeout(Executor executor, int milliseconds) throws SQLException {
        conn.setNetworkTimeout(executor, milliseconds);
    }
  
    @Override
    public void setReadOnly(boolean readOnly) throws SQLException {
        conn.setReadOnly(readOnly);
    }
  
    @Override
    public Savepoint setSavepoint() throws SQLException {
        return conn.setSavepoint();
    }
  
    @Override
    public Savepoint setSavepoint(String name) throws SQLException {
        return conn.setSavepoint(name);
    }
  
    @Override
    public void setSchema(String schema) throws SQLException {
        conn.setSchema(schema);
    }
  
    @Override
    public void setTransactionIsolation(int level) throws SQLException {
        conn.setTransactionIsolation(level);
    }
  
    @Override
    public void setTypeMap(Map<String, Class<?>> map) throws SQLException {
        conn.setTypeMap(map);
    }
  
  }
  
```

+ *DataSourceDemo1*

  ```java
  import javax.sql.DataSource;
  import java.sql.Connection;
  import java.sql.PreparedStatement;
  import java.sql.ResultSet;
  import java.sql.SQLException;
  
  public class DataSourceDemo1 {
  
      public static void main(String[] args) {
          DataSourceDemo1 demo1 = new DataSourceDemo1();
          demo1.testDataSource();
      }
  
      public void testDataSource() {
          Connection conn = null;
          PreparedStatement pstm = null;
          ResultSet rs = null;
          DataSource dataSource = null;
          try {
              //获得连接
              //conn = jdbcUtils.getConnection();
              //从连接池获得连接
              dataSource = new MyDataSource();
              conn = dataSource.getConnection();
  
              //编写SQL
              String sql = "select * from account";
  
              //预编译SQL
              pstm = conn.prepareStatement(sql);
              //设置参数
              //执行SQL
              rs = pstm.executeQuery();
              while (rs.next()) {
                  System.out.print(rs.getInt("id")+" ");
                  System.out.print(rs.getString("name")+" ");
                  System.out.print(rs.getDouble("money")+" ");
                  System.out.println();
              }
          } catch (SQLException e) {
              e.printStackTrace();
          }
          //归还
          jdbcUtils.release(pstm, conn, rs);
      }
  }
  
  ```

+ *db.properties  (文件)*

  ```txt
  classForName = com.mysql.cj.jdbc.Driver
  url = jdbc:mysql://127.0.0.1:3306/test1?rewriteBatchedStatements=true
  username = root
  password = 321321
  ```

+ *jdbcUtils*

  ```java
  import java.io.FileInputStream;
  import java.io.IOException;
  import java.sql.*;
  import java.util.Properties;
  
  public class jdbcUtils {
      private static final String classForName;
      private static final String url;
      private static final String username;
      private static final String password;
  
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
  ```

+ *MyConnectionWrapper*

  ```java
  import java.sql.Connection;
  import java.sql.SQLException;
  import java.util.List;
  /*
  * 使用装饰者模式增强connection中close方法
  *
  *
  * */
  public class MyConnectionWrapper extends ConnectionWrapper{
  
      private final List<Connection> connList;
      Connection conn = null;
  
      public MyConnectionWrapper(Connection conn, List<Connection> connList) {
          super(conn);
          this.conn = conn;
          this.connList = connList;
      }
  
      //增强某个方法
      @Override
      public void close() throws SQLException {
  //        super.close();
          //归还连接
          connList.add(conn);
      }
  }
  ```

+ *MyDataSource*

  ```java
  import javax.sql.DataSource;
  import java.io.PrintWriter;
  import java.sql.Connection;
  import java.sql.SQLException;
  import java.sql.SQLFeatureNotSupportedException;
  import java.util.ArrayList;
  import java.util.List;
  import java.util.logging.Logger;
  /*
  * 自定义连接池
  *
  *
  * */
  public class MyDataSource implements DataSource {
      //创建一个集合存储一些连接
      private List<Connection> connList = new ArrayList<Connection>();
  
      //初始化  初始化时候提供一些连接
      public MyDataSource() {
          //初始化3个
          for (int i = 1; i <= 3; i++) {
              //向集合中存储连接
              connList.add(jdbcUtils.getConnection());
          }
      }
  
      @Override
      //从连接池中获得连接的方法
      public Connection getConnection() throws SQLException {
          Connection conn = connList.remove(0);
          MyConnectionWrapper connWrapper = new MyConnectionWrapper(conn, connList);
          return connWrapper;
      }
  
      @Override
      public Connection getConnection(String username, String password) throws SQLException {
          return null;
      }
  
      @Override
      public PrintWriter getLogWriter() throws SQLException {
          return null;
      }
  
      @Override
      public void setLogWriter(PrintWriter out) throws SQLException {
  
      }
  
      @Override
      public void setLoginTimeout(int seconds) throws SQLException {
  
      }
  
      @Override
      public int getLoginTimeout() throws SQLException {
          return 0;
      }
  
      @Override
      public Logger getParentLogger() throws SQLFeatureNotSupportedException {
          return null;
      }
  
      @Override
      public <T> T unwrap(Class<T> iface) throws SQLException {
          return null;
      }
  
      @Override
      public boolean isWrapperFor(Class<?> iface) throws SQLException {
          return false;
      }
  }
  ```
  
  

## 04_开源连接池Druid

*阿里旗下开源连接池的产品使用非常简单,可以与Spring框架进行很快整合*

***Druid是一个JDBC组件，它包括三部分：*** 

- *DruidDriver 代理Driver，能够提供基于Filter－Chain模式的插件体系。* 
- *DruidDataSource 高效可管理的数据库连接池。* 
- *SQLParser* 

### 4.1Druid主要类

*`DruidDataSource`* 

#### 4.1.1手动设置方法:

```java
import com.alibaba.druid.pool.DruidDataSource;
import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.ResultSet;

public class DruidDemo {

    public static void main(String[] args) {

        Connection conn = null;
        PreparedStatement pstm = null;
        ResultSet rs = null;
        DruidDataSource dds = null;
        try {
            dds = new DruidDataSource();

            dds.setDriverClassName("com.mysql.cj.jdbc.Driver");
            dds.setUrl("jdbc:mysql://127.0.0.1:3306/test1");
            dds.setUsername("root");
            dds.setPassword("321321");

            conn = dds.getConnection();
            String sql = "select * from account";
            pstm = conn.prepareStatement(sql);
            rs = pstm.executeQuery();
            while (rs.next()) {
                System.out.print(rs.getInt("id")+" ");
                System.out.print(rs.getString("name")+" ");
                System.out.print(rs.getDouble("money")+" ");
                System.out.println();
            }
        } catch (Exception e) {
            e.printStackTrace();
        }finally {
             //Druid连接池底层对close方法进行了增强  所以这里不是关闭而是归还
            jdbcUtils.release(conn,pstm,rs);
        }
    }

}
```

#### 4.1.2使用配置设置

*配置文件写法:(gruid配置文件名没有规定可以任意写 但是里面的语法必须严格遵守)*

```properties
#驱动加载
driverClassName=com.mysql.cj.jdbc.Driver
#注册驱动
url=jdbc:mysql://127.0.0.1:3306/test1?rewriteBatchedStatements=true
#连接数据库的用户名
username=root
#连接数据库的密码
password=321321
#属性类型的字符串，通过别名的方式配置扩展插件， 监控统计用的stat 日志用log4j 防御sql注入:wall
#filters=stat
#初始化时池中建立的物理连接个数。
#initialSize=2
#最大的可活跃的连接池数量
#maxActive=300
#获取连接时最大等待时间，单位毫秒，超过连接就会失效。配置了maxWait之后，缺省启用公平锁，并发效率会有所下降， 如果需要可以通过配置useUnfairLock属性为true使用非公平锁。
#maxWait=60000
#连接回收器的运行周期时间，时间到了清理池中空闲的连接，testWhileIdle根据这个判断
#timeBetweenEvictionRunsMillis=60000
#minEvictableIdleTimeMillis=300000
#用来检测连接是否有效的sql，要求是一个查询语句。
#validationQuery=SELECT 1
#建议配置为true，不影响性能，并且保证安全性。 申请连接的时候检测，如果空闲时间大于timeBetweenEvictionRunsMillis， 执行validationQuery检测连接是否有效。
#testWhileIdle=true
#申请连接时执行validationQuery检测连接是否有效，做了这个配置会降低性能。设置为false
#testOnBorrow=false
#归还连接时执行validationQuery检测连接是否有效，做了这个配置会降低性能,设置为flase
#testOnReturn=false
#是否缓存preparedStatement，也就是PSCache。
#poolPreparedStatements=false
#池中能够缓冲的preparedStatements语句数量
#maxPoolPreparedStatementPerConnectionSize=200

```





```java
import com.alibaba.druid.pool.DruidDataSource;
import com.alibaba.druid.pool.DruidDataSourceFactory;

import javax.sql.DataSource;
import java.io.FileInputStream;
import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.util.Properties;

public class DruidDemo {
    public static void main(String[] args) {
        Connection conn = null;
        PreparedStatement pstm = null;
        ResultSet rs = null;
        DataSource dataSource;
        try {
            //获取配置文件
            Properties properties = new Properties();
            properties.load(new FileInputStream("src/gruid.properties"));
            dataSource = DruidDataSourceFactory.createDataSource(properties);
            //获取连接
            conn = dataSource.getConnection();
            String sql = "select * from account";
            pstm = conn.prepareStatement(sql);
            rs = pstm.executeQuery();
            while (rs.next()) {
                System.out.print(rs.getInt("id")+" ");
                System.out.print(rs.getString("name")+" ");
                System.out.print(rs.getDouble("money")+" ");
                System.out.println();
            }
        } catch (Exception e) {
            e.printStackTrace();
        }finally {
             //Druid连接池底层对close方法进行了增强  所以这里不是关闭而是归还
            jdbcUtils.release(conn,pstm,rs);
        }
    }
}

```

## 05_C3P0开源连接池

*C3P0是一个开源的JDBC连接池，它实现了数据源和JNDI绑定，支持JDBC3规范和JDBC2的标准扩展。目前使用它的开源项目有Hibernate、Spring等。*

#### 5.1 手动配置c3p0

```java

import com.mchange.v2.c3p0.ComboPooledDataSource;
import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.ResultSet;

public class C3P0Demo {
    public static void main(String[] args) {
        Connection conn = null;
        PreparedStatement pstm = null;
        ResultSet rs = null;
        
        try {
            //获取配置文件
            //创建连接池和手动参数
            ComboPooledDataSource dataSource = new ComboPooledDataSource();
            dataSource.setDriverClass("com.mysql.cj.jdbc.Driver");
            dataSource.setJdbcUrl("jdbc:mysql://127.0.0.1:3306/test1?serverTimezone=GMT%2B8");
            dataSource.setUser("root");
            dataSource.setPassword("321321");
            //获取连接
            conn = dataSource.getConnection();
            String sql = "select * from account";
            pstm = conn.prepareStatement(sql);
            rs = pstm.executeQuery();
            while (rs.next()) {
                System.out.print(rs.getInt("id")+" ");
                System.out.print(rs.getString("name")+" ");
                System.out.print(rs.getDouble("money")+" ");
                System.out.println();
            }
        } catch (Exception e) {
            e.printStackTrace();
        }finally {
            //Druid连接池底层对close方法进行了增强  所以这里不是关闭而是归还
            jdbcUtils.release(conn,pstm,rs);
        }
    }
}
```

#### 5.1配置文件`c3p0-config.xml`配置

*必须在类的创建目录下即`src/c3p0-config.xml`*

```xml
<c3p0-config>
    <!-- 使用默认的配置读取连接池对象 -->
    <default-config>
        <!--  连接参数 -->
        <property name="driverClass">com.mysql.cj.jdbc.Driver</property>
        <property name="jdbcUrl">jdbc:mysql://127.0.0.1:3306/test1?rewriteBatchedStatements=true</property>
        <property name="user">root</property>
        <property name="password">321321</property>
    </default-config>
```

*只需创建ComboPooledDataSource对象c3p0会自动寻找xml的配置*

```java

import com.mchange.v2.c3p0.ComboPooledDataSource;
import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.ResultSet;

public class C3P0Demo {
    public static void main(String[] args) {
        Connection conn = null;
        PreparedStatement pstm = null;
        ResultSet rs = null;
        try {
            //获取配置文件
            //创建连接池    只需要创建就会默认去类路径下查找c3p0-config.xml文件
            ComboPooledDataSource dataSource = new ComboPooledDataSource();
            
            //获取连接
            conn = dataSource.getConnection();
            String sql = "select * from account";
            pstm = conn.prepareStatement(sql);
            rs = pstm.executeQuery();
            while (rs.next()) {
                System.out.print(rs.getInt("id")+" ");
                System.out.print(rs.getString("name")+" ");
                System.out.print(rs.getDouble("money")+" ");
                System.out.println();
            }
        } catch (Exception e) {
            e.printStackTrace();
        }finally {
            //Druid连接池底层对close方法进行了增强  所以这里不是关闭而是归还
            jdbcUtils.release(conn,pstm,rs);
        }
    }
}
```

## 06_改写工具类

>  *每次使用都创建连接池对内存消耗太大了所以改写工具类*
> *连接池的对象应该一个应用只创建一次*

*工具类:*

```java
import com.mchange.v2.c3p0.ComboPooledDataSource;

import javax.sql.DataSource;
import java.sql.Connection;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.sql.Statement;

public class jdbcUtils {
    //创建连接池  ,  只创建一次
    private static final ComboPooledDataSource dataSource = new ComboPooledDataSource();
    //获得连接
    public static Connection getConnection() throws SQLException {
        return dataSource.getConnection();
    }
    //获得连接池方法
    public static DataSource getDataSource(){
        return dataSource;
    }
    //释放资源
    public static void release(Connection conn, Statement stm) {
        if (conn != null) {
            try {
                conn.close();
            } catch (Exception e) {
                e.printStackTrace();
            }
        }
        conn = null;
        if (stm != null) {

            try {
                stm.close();
            } catch (Exception e) {
                e.printStackTrace();
            }
        }
        stm = null;
    }
    public static void release(Connection conn, Statement stm, ResultSet rs) {
        if (conn != null) {
            try {
                conn.close();
            } catch (Exception e) {
                e.printStackTrace();
            }
        }
        conn = null;
        if (stm != null) {

            try {
                stm.close();
            } catch (Exception e) {
                e.printStackTrace();
            }
        }
        stm = null;
        if (rs != null) {

            try {
                rs.close();
            } catch (Exception e) {
                e.printStackTrace();
            }
        }
        rs = null;
    }
}
```

*测试类:*

```java
import com.mchange.v2.c3p0.ComboPooledDataSource;
import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
public class C3P0Demo {
    public static void main(String[] args) {
        Connection conn = null;
        PreparedStatement pstm = null;
        ResultSet rs = null;
        try {
            conn = jdbcUtils.getConnection();
            String sql = "select * from account";
            pstm = conn.prepareStatement(sql);
            rs = pstm.executeQuery();
            while (rs.next()) {
                System.out.print(rs.getInt("id")+" ");
                System.out.print(rs.getString("name")+" ");
                System.out.print(rs.getDouble("money")+" ");
                System.out.println();
            }
        } catch (Exception e) {
            e.printStackTrace();
        }finally {
            jdbcUtils.release(conn,pstm,rs);
        }
    }
}
```

## 07_DBUtils

> *Commons DbUtils是Apache组织提供的一个对JDBC进行简单封装的开源工具类库，使用它能够简化JDBC应用程序的开发，同时也不会影响程序的性能。*

*对JDBC的简单封装而且没有影响.*

### 7.1为什么学习DBUtils

*因为JDBC手写比较麻烦,而且重复代码过多。比如获得连接,预编译SQL,释放资源等等。将这些操作抽取出来放入工具类中.进化JDBC编程。*

### 7.2DBUtils的API

#### 7.2.1QueryRunner对象:核心运行类

1. 构造方法

   + QueryRunner()      
   + QueryRunner(DataSource ds)

2. CRUD方法:

   + int update(String sql,Object....params)     //执行给定的增删改sql语句声明
   + int update(Connection conn,String sql,Object....params)     
   + <T> T  query(String sql, ResultSetHandler<T> rsh, Object... params)   //执行查询
   + <T> T  query(Connection conn, String sql, ResultSetHandler<T> rsh, Object... params)

   一般情况下CRUD(无事务)使用:
   + QueryRunner(DataSource ds)
   + int update(String sql,Object....params)
   + <T> T  query(String sql, ResultSetHandler<T> rsh, Object... params)

   有事务管理使用
   + QueryRunner() 
   + int update(Connection conn,String sql,Object....params) 
   + <T> T  query(Connection conn, String sql, ResultSetHandler<T> rsh, Object... params)

3. 批处理
   +  int[] batch(Connection conn, String sql, Object[][] params)
   + int[]  batch(String sql, Object[][] params)

#### 7.2.2Dbutils对象

1. 方法(安静的意思是异常顺便给处理了)
   + static void commitAndCloseQuietly(Connection conn)    //提交并安静的归还
   + static void rollbackAndCloseQuietly(Connection conn)  //回滚并安静归还

### 7.3DButils的使用

#### 7.3.1增删改操作(工具类使用06_改写工具类)

```java
import org.apache.commons.dbutils.QueryRunner;
import java.sql.SQLException;
public class DButilsDemo {
    /*
     * DBUtils的增删改操作
     * */
    //创建核心类
    QueryRunner queryRunner = new QueryRunner(jdbcUtils.getDataSource());

    public static void main(String[] args) throws SQLException {
        DButilsDemo t = new DButilsDemo();
        t.deleteDate(8);
    }
    
    //增
    public void insertDate(String name,double money) throws SQLException {
        queryRunner.update("insert into account values (null,?,?)", name, money);
    }
    //改
    public void upDate(int id ,String name,double money) throws SQLException {
        queryRunner.update("update account set name = ? , money = ? where id = ?", id,name ,money);
    }
    //删
    public void deleteDate(int id) throws SQLException {
        queryRunner.update("delete from account where id = ?",  id);
    }
}
```

#### 7.3.2查询操作

新建一个类,存放列名 和get set方法

+ 查询一条记录

  ```java
  //查询一条记录
  public void queryValue(int id) throws SQLException {
      Account account = queryRunner.query("select * from account where id = ?", new ResultSetHandler<Account>() {
          public Account handle(ResultSet rs) throws SQLException {
              Account account = new Account();
              while (rs.next()) {
                  account.setId(rs.getInt("id"));
                  account.setName(rs.getString("name"));
                  account.setMoney(rs.getDouble("money"));
              }
              return account;
          }
      }, id);
      System.out.println(account);
  }
  ```

+ 查询多条记录

  ```java
  //查询多条记录
  public void queryValues() throws SQLException {
      List<Account> list = queryRunner.query("select * from account", new ResultSetHandler<List<Account>>() {
          public List<Account> handle(ResultSet rs) throws SQLException {
              List<Account> list = new ArrayList<Account>();
              while (rs.next()) {
                  Account account = new Account();
                  account.setId(rs.getInt("id"));
                  account.setName(rs.getString("name"));
                  account.setMoney(rs.getDouble("money"));
                  list.add(account);
              }
              return list;
          }
      });
      for (Account a : list) {
          System.out.println(a);
      }
  }
  ```

  #### 7.3.3`Interface ResultSetHandler<T>`接口实现类

  1. ArrayHandler和ArrayListHandler

     + ArrayHanlder:将一条数据封装到一个数组中,这个数组是Object[]类型数组

     + ArrayListHandler:将多条记录封装到一个装有Object[]的List集合中。

     + 实现过程

       ```java
       public void testArrayHandler() throws SQLException {
           //ArrayHanlder将一条记录封装在一个Object[]数组中
           Object[] ob = queryRunner.query("select * from account where id = 3", new ArrayHandler());
           System.out.println(Arrays.toString(ob));
       }
       public void testArrayListHandler() throws SQLException{
           //ArrayListHandler将多条记录封装在一个object[]类型的List集合中
           List<Object[]> list = queryRunner.query("select * from account", new ArrayListHandler());
           for (Object[] a : list) {
               System.out.println(Arrays.toString(a));
           }
       }
       ```

  2. BeanHandler和BeanListHandler

     + BeanHandler:将一条数据封装到javaBean组中

     + BeanListHandler:将多条记录封装到一个装有javaBean的List集合中。

  3. MapHandler和MapListHandler
     + MapHandler  将一条数据封装到Map集合中  key是列名 value是记录的值
     + MapListHandler  多条记录封装到一个装有Map集合的List集合中
  4. ColumnListHandler和ScalarHandler和KeyedHandler
     + ColumnListHandler将数据中的某一列封装到list集合中
     + ScalarHandler 封装单个值  比如统计表记录数   或者某个记录出现次数
     + KeyedHandler  将多条记录封装到一个装有Map集合的Map集合中,值得一提的是外面Map的key可以指定

```java
public void testArrayHandler() throws SQLException {
    //ArrayHanlder将一条记录封装在一个Object[]数组中
    Object[] ob = queryRunner.query("select * from account where id = 3", new ArrayHandler());
    System.out.println(Arrays.toString(ob));
}
public void testArrayListHandler() throws SQLException{
    //ArrayListHandler将多条记录封装在一个object[]类型的List集合中
    List<Object[]> list = queryRunner.query("select * from account", new ArrayListHandler());
    for (Object[] a : list) {
        System.out.println(Arrays.toString(a));
    }
}
public void testBeanHandler() throws SQLException {
    //BeanHandler测试
    Account account = queryRunner.query("select * from account where id = 3",new BeanHandler<Account>(Account.class));
    System.out.println(account);
}
public void testBeanListHandler() throws SQLException {
    //BeanHandler测试
    List<Account> account = queryRunner.query("select * from account ", new BeanListHandler<Account>(Account.class));
    for (Account a :account) {
        System.out.println(a);
    }
}
public void testMapHabdler() throws SQLException {
    //MapHandler测试
    Map<String, Object> map = queryRunner.query("select * from account where id = 3", new MapHandler());
    System.out.println(map);
}
public void testMapListHabdler() throws SQLException {
    //MapHandler测试
    List<Map<String, Object>> list = queryRunner.query("select * from account", new MapListHandler());
    for (Map<String, Object> map :list) {
        System.out.println(map);
    }
}
public void testColumnListHandler() throws SQLException{
    //ColumnListHandler测试  查询表中某一列
    List<Object> list = queryRunner.query("select* from account", new ColumnListHandler("name"));
    System.out.println(list);
}

public void testScalarHandler() throws SQLException{
    //ColumnListHandler测试  查询表中某一列
    Object o = queryRunner.query("select count(*) from account ", new ScalarHandler());
    System.out.println(o);
}
public void KeyedHandler() throws SQLException{
    Map<Object, Map<String, Object>> map = queryRunner.query("select* from account", new KeyedHandler("name"));
    for (Object key : map.keySet()) {
        System.out.println(key+" "+map.get(key));
    }
}
```

   