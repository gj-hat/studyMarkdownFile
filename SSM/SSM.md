[TOC]
# SSM框架



# 01_Spring框架



## 1.1 Spring简介

### 1.1.1 Spring体系结构

Spring 有可能成为所有企业应用程序的一站式服务点，然而，Spring 是模块化的，允许你挑选和选择适用于你的模块，不必要把剩余部分也引入。下面的部分对在 Spring 框架中所有可用的模块给出了详细的介绍。

Spring 框架提供约 20 个模块，可以根据应用程序的要求来使用。

 <img src="https://atts.w3cschool.cn/attachments/image/wk/wkspring/arch1.png">



### 1.1.2 核心容器

核心容器由 **spring-core，spring-beans，spring-context，spring-context-support和spring-expression**（SpEL，Spring 表达式语言，Spring Expression Language）等模块组成 ，它们的细节如下：

- **spring-core** 模块提供了框架的基本组成部分，包括 IoC 和依赖注入功能。
- **spring-beans** 模块提供 BeanFactory，工厂模式的微妙实现，它移除了编码式单例的需要，并且可以把配置和依赖从实际编码逻辑中解耦。
- **context** 模块建立在由 **core**和 **beans** 模块的基础上建立起来的，它以一种类似于 JNDI 注册的方式访问对象。Context 模块继承自 Bean 模块，并且添加了国际化（比如，使用资源束）、事件传播、资源加载和透明地创建上下文（比如，通过 Servelet 容器）等功能。Context 模块也支持 Java EE 的功能，比如 EJB、JMX 和远程调用等。**ApplicationContext** 接口是 Context 模块的焦点。**spring-context-support** 提供了对第三方集成到 Spring 上下文的支持，比如缓存（EhCache, Guava, JCache）、邮件（JavaMail）、调度（CommonJ, Quartz）、模板引擎（FreeMarker, JasperReports, Velocity）等。
- **spring-expression** 模块提供了强大的表达式语言，用于在运行时查询和操作对象图。它是 JSP2.1 规范中定义的统一表达式语言的扩展，支持 set 和 get 属性值、属性赋值、方法调用、访问数组集合及索引的内容、逻辑算术运算、命名变量、通过名字从 Spring IoC 容器检索对象，还支持列表的投影、选择以及聚合等。

### 1.1.3 数据访问

数据访问/集成层包括 JDBC，ORM，OXM，JMS 和事务处理模块，它们的细节如下：

（注：JDBC=Java Data Base Connectivity，ORM=Object Relational Mapping，OXM=Object XML Mapping，JMS=Java Message Service）

- **JDBC** 模块提供了 JDBC 抽象层，它消除了冗长的 JDBC 编码和对数据库供应商特定错误代码的解析。
- **ORM** 模块提供了对流行的对象关系映射 API 的集成，包括 JPA、JDO 和 Hibernate 等。通过此模块可以让这些 ORM 框架和 spring的其它功能整合，比如前面提及的事务管理。
- **OXM** 模块提供了对 OXM 实现的支持，比如 JAXB、Castor、XML Beans、JiBX、XStream 等。
- **JMS** 模块包含生产（produce）和消费（consume）消息的功能。从 Spring 4.1 开始，集成了 spring-messaging 模块。
- **事务**模块为实现特殊接口类及所有的 POJO 支持编程式和声明式事务管理。（注：编程式事务需要自己写 beginTransaction()、commit()、rollback() 等事务管理方法，声明式事务是通过注解或配置由 spring 自动处理，编程式事务粒度更细）

### 1.1.4 Web

Web 层由 Web，Web-MVC，Web-Socket 和 Web-Portlet 组成，它们的细节如下：

- **Web** 模块提供面向 web 的基本功能和面向 web 的应用上下文，比如多部分（multipart）文件上传功能、使用 Servlet 监听器初始化 IoC 容器等。它还包括 HTTP 客户端以及 Spring 远程调用中与 web 相关的部分。
- **Web-MVC** 模块为 web 应用提供了模型视图控制（MVC）和 REST Web服务的实现。Spring 的 MVC 框架可以使领域模型代码和 web 表单完全地分离，且可以与 Spring 框架的其它所有功能进行集成。
- **Web-Socket** 模块为 WebSocket-based 提供了支持，而且在 web 应用程序中提供了客户端和服务器端之间通信的两种方式。
- **Web-Portlet** 模块提供了用于 Portlet 环境的 MVC 实现，并反映了 spring-webmvc 模块的功能。

### 1.1.5 其他

其他一些重要的模块，像 AOP，Aspects，Instrumentation，Web 和测试模块，它们的细节如下：

- **AOP** 模块提供了面向方面（切面）的编程实现，允许你定义方法拦截器和切入点对代码进行干净地解耦，从而使实现功能的代码彻底的解耦出来。使用源码级的元数据，可以用类似于.Net属性的方式合并行为信息到代码中。
- **Aspects** 模块提供了与 **AspectJ** 的集成，这是一个功能强大且成熟的面向切面编程（AOP）框架。
- **Instrumentation** 模块在一定的应用服务器中提供了类 instrumentation 的支持和类加载器的实现。
- **Messaging** 模块为 STOMP 提供了支持作为在应用程序中 WebSocket 子协议的使用。它也支持一个注解编程模型，它是为了选路和处理来自 WebSocket 客户端的 STOMP 信息。
- **测试**模块支持对具有 JUnit 或 TestNG 框架的 Spring 组件的测试。

## 1.2 Spring快速入门

### 1.2.1 Spring程序开发步骤

1. 导入Srping基本包maven坐标
2. 编写Dao接口和实现类(创建bean)
3. 创建和配置Spring核心配置文件(指定Bean)
4. 测试(可选)

### 1.2.2流程

1. 导入Srping基本包maven坐标

```xml
<!--    依赖管理-->
<dependencies>
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-context</artifactId>
        <version>5.0.5.RELEASE</version>
    </dependency>
```

2. 编写Dao接口和实现类(创建bean)

``` java
// 接口类
package com.springStudy.dao;
public interface UserDao {
    public void save();
}

// 实现类
package com.springStudy.dao.impl;
import com.springStudy.dao.UserDao;
public class UserDaoImpl implements UserDao {
    @Override
    public void save() {
        System.out.println("save running....");
    }
}
```

3. 创建Spring核心配置文件(指定Bean)

> 在resources目录下创建xml文件(命名随意)

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
	
    // 1. 指定id(随意)   2. 指定路径
    <bean id="userDao" class="com.springStudy.dao.impl.UserDaoImpl"></bean>
    
</beans>
```

4. 测试

> 新建main方法

```java
package com.springStudy.demo;
import com.springStudy.dao.UserDao;
import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;
public class UserDaoDemo {
    public static void main(String[] args) {
		// 调用spring客户端  参数是第三步创建的xml文件的文件名
        ApplicationContext app = new ClassPathXmlApplicationContext("applicationContext.xml");
        // 通过上一步获取到xml文件,进而获取Bean
        UserDao userDao = (UserDao) app.getBean("userDao");
        // 调用
        userDao.save();
    }
}
```

5. 运行

## 1.3 配置文件

> 快速入门的第三步配置文件
>
> 调用id时其实调用的就是class的无参构造

### 1.3.1 Bean标签

**基本配置**

1. id: Bean实例在Spring容器中的唯一标识
2. class: Bean的全限定名称

**范围配置**

scope: 值有以下五种(每次调用都会创建一个实例)

+ singleton	默认值:单例(调用多次只创建一次)
+ prototype	多例的(调用多次创建多次)
+ request	WEB 项目中，Spring 创建一个 Bean 的对象，将对象存入到 request 域中
+ session	WEB 项目中，Spring 创建一个 Bean 的对象，将对象存入到 session 域中
+ global session	WEB 项目中，应用在 Portlet 环境，如果没有 Portlet 环境那么globalSession 相当于 session

**声明周期配置**

+ init-method: 指定类中初始化方法名称
+ destroy-method: 指定类中销毁方法名称
+ 值得一提:加载顺序   无参构造先实例化->加载初始化方法-> 加载销毁方法

**实例化的三种方式**

+ 无参构造方法实例化
+ 工厂静态方法实例化
+ 工厂实例方法实例化

范围配置实例:

singleton属性(xml, java, 结果):

``` xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
    <bean id="userDao" class="com.springStudy.dao.impl.UserDaoImpl" scope="singleton"/>
</beans>

public class SpringTest {
    @Test
    public void test1() {
        ApplicationContext app = new ClassPathXmlApplicationContext("applicationContext.xml");
        UserDao userDao1 = (UserDao) app.getBean("userDao");
        UserDao userDao2 = (UserDao) app.getBean("userDao");
        System.out.println("userDao1 = " + userDao1);
        System.out.println("userDao2 = " + userDao2);
    }
}

Result(相同):
userDao1 = com.springStudy.dao.impl.UserDaoImpl@1a451d4d
userDao2 = com.springStudy.dao.impl.UserDaoImpl@1a451d4d
```

prototype属性(xml, java, 结果):

``` xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
    <bean id="userDao" class="com.springStudy.dao.impl.UserDaoImpl" scope="prototype"/>
</beans>

public class SpringTest {
    @Test
    public void test1() {
        ApplicationContext app = new ClassPathXmlApplicationContext("applicationContext.xml");
        UserDao userDao1 = (UserDao) app.getBean("userDao");
        UserDao userDao2 = (UserDao) app.getBean("userDao");
        System.out.println("userDao1 = " + userDao1);
        System.out.println("userDao2 = " + userDao2);
    }
}

Result(不同):
userDao1 = com.springStudy.dao.impl.UserDaoImpl@441772e
userDao2 = com.springStudy.dao.impl.UserDaoImpl@7334aada
```

### 1.3.2 依赖注入

> 原先controller里new service   
>
> service里new Dao
>
> 现在不需要new了 new的工作交给spring

依赖注入(Dependency Injection)和控制反转(Inversion of Control)是同一个概念。

具体含义是:当某个角色(可能是一个Java实例，调用者)需要另一个角色(另一个Java实例，被调用者)的协助时，在 传统的程序设计过程中，通常由调用者来创建被调用者的实例。但在Spring里，创建被调用者的工作不再由调用者来完成，因此称为控制反转;创建被调用者 实例的工作通常由Spring容器来完成，然后注入调用者，因此也称为依赖注入。

#### 引用注入:set注入

> 如果不用xml通知spring的话set的Dao会提示空报错

1. xml文件中属性的形式  注意: ref关键字是引用数据类型

   ``` java
   //Service
   public class UserServiceImpl implements UserService {
    private UserDao userDao;
    public void setUserDao(UserDao userDao) {
        this.userDao = userDao;
    }
    @Override
    public void save() {
        userDao.save();
    }
   }
   //xml  方式一
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
    <bean id="userDao" class="com.springStudy.dao.impl.UserDaoImpl"/>
    <bean id="userService" class="com.springStudy.service.impl.UserServiceImpl">
        <property name="userDao" ref="userDao"/>
    </bean>
   </beans>
   ```

2. xml中p命名空间的形式(不常用)  注意: ref关键字是引用数据类型

   ``` java
   //Service
   public class UserServiceImpl implements UserService {
    private UserDao userDao;
    public void setUserDao(UserDao userDao) {
        this.userDao = userDao;
    }
    @Override
    public void save() {
        userDao.save();
    }
   }
   //xml  方式二 
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
           //有改动
       xmlns:p="http://www.springframework.org/schema/p"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
    <bean id="userDao" class="com.springStudy.dao.impl.UserDaoImpl"/>
        //有改动
    <bean id="userService" class="com.springStudy.service.impl.UserServiceImpl" p:userDao-ref="userDao"/>
   </beans>       
   ```

3. 完整版代码

   ``` java
   //原始代码 完整版
   //appplicationContext.cml
   ......
       <bean id="userDao" class="com.springStudy.dao.impl.UserDaoImpl"/>
       <bean id="userService" class="com.springStudy.service.impl.UserServiceImpl"/>
   ......
   //Dao层
    public interface UserDao {
       public void save();
   }
   public class UserDaoImpl implements UserDao {
       @Override
       public void save() {
           System.out.println("save running....");
       }
   }
   // Service层
   public interface UserService {
       public void save();
   }
   public class UserServiceImpl implements UserService {
       @Override
       public void save() {
           ApplicationContext app = new ClassPathXmlApplicationContext("applicationContext.xml");
           UserDao userDao = (UserDao) app.getBean("userDao");
           userDao.save();
       }
   }
   //Controller层(假的)
   public class UserController {
       public static void main(String[] args) {
           ApplicationContext app = new ClassPathXmlApplicationContext("applicationContext.xml");
           UserService userService = (UserService) app.getBean("userService");
           userService.save();
       }
   }
   //运行结果  save running....
   //运行顺序  控制层需要持久层的save方法  控制层调用Service的save方法需要new一个app  Service调用Dao的save也需要new一个app
   //依赖注入代码实现   把new交给Spring去做
   //appplicationContext.cml
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns="http://www.springframework.org/schema/beans"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xmlns:p="http://www.springframework.org/schema/p"
          xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
       <bean id="userDao" class="com.springStudy.dao.impl.UserDaoImpl"/>
       <bean id="userService" class="com.springStudy.service.impl.UserServiceImpl" p:userDao-ref="userDao"/>
   </beans>
   //Dao层
    public interface UserDao {
       public void save();
   }
   public class UserDaoImpl implements UserDao {
       @Override
       public void save() {
           System.out.println("save running....");
       }
   }
   // Service层
   public interface UserService {
       public void save();
   }
   public class UserServiceImpl implements UserService {
       private UserDao userDao;
       public void setUserDao(UserDao userDao) {this.userDao = userDao;}
       @Override
       public void save() {
           userDao.save();
       }
   }
   //Controller层(假的)
   public class UserController {
       public static void main(String[] args) {
           ApplicationContext app = new ClassPathXmlApplicationContext("applicationContext.xml");
           UserService userService = (UserService) app.getBean("userService");
           userService.save();
       }
   }
   ```
   
   

#### 引用注入:构造注入

> 如果不用xml通知spring的话set的Dao会提示空报错

1. xml中属性方式 注意: ref关键字是引用数据类型

   ``` xml
   ......
       <bean id="userDao" class="com.springStudy.dao.impl.UserDaoImpl"/>
       <bean id="userService" class="com.springStudy.service.impl.UserServiceImpl">
           // name是构造函数的参数名  ref是引用注入的id名
           <constructor-arg name="userDao" ref="userDao"/>
       </bean>
   ......
   ```

2. UserServiceImpl方法

   ``` java
   public class UserServiceImpl implements UserService {
       private UserDao userDao;
       public UserServiceImpl() {}
       public UserServiceImpl(UserDao userDao) {this.userDao = userDao;}
       @Override
       public void save() {
           userDao.save();
       }
   }
   ```



#### 普通数据类型注入

1. xml的属性注入普通数据类型

``` xml
......
    <bean id="userDao" class="com.springStudy.dao.impl.UserDaoImpl">
        // name 实体类的属性名 
        // value 属性的值
        <property name="username" value="zhangsan"/>
        <property name="age" value="18"/>
    </bean>
    <bean id="userService" class="com.springStudy.service.impl.UserServiceImpl">
        <constructor-arg name="userDao" ref="userDao"/>
    </bean>
......
```

2. UserDao方法

   ``` java
   public class UserDaoImpl implements UserDao {
   
       private String username;
       private int age;
       public void setUsername(String username) {this.username = username;}
       public void setAge(int age) {this.age = age;}
       @Override
       public void save() {
           System.out.println("username = " + username);
           System.out.println("age = " + age);
           System.out.println("save running....");
       }
   }
   ```

#### 集合数据类型注入

> List  Map  Properties

<font color="red" size="6">注Bean标签属性中的name是set方法的方法名set后字符串首字母小写的字符串</font>

1. xml的属性注入集合型

   ``` xml
   ......
       <bean id="userDao" class="com.springStudy.dao.impl.UserDaoImpl">
   <!--        List注入-->
           <property name="stringList">
               <list>
                   <value>aaa</value>
                   <value>bbb</value>
                   <value>ccc</value>
               </list>
           </property>
   <!--        Map注入-->
           <property name="userMap">
               <map>
                   <entry key="user1" value-ref="user1"/>
                   <entry key="user2" value-ref="user2"/>
               </map>
           </property>
   <!--        properties注入-->
           <property name="properties">
                   <props>
                       <prop key="key1">ppp1</prop>
                       <prop key="key2">ppp2</prop>
                       <prop key="key3">ppp3</prop>
                   </props>
           </property>
       </bean>
   ......
   ```

2. UserDao方法

   ``` java
   public class UserDaoImpl implements UserDao {
       private List<String> stringList;
       private Map<String, User> userMap;
       private Properties properties;
       public void setStringList(List<String> stringList) {this.stringList = stringList;}
       public void setUserMap(Map<String, User> userMap) {this.userMap = userMap;}
       public void setProperties(Properties properties) {this.properties = properties;}
       @Override
       public void save() {
           System.out.println("UserDaoImpl{" +
                   "stringList=" + stringList +
                   ", userMap=" + userMap +
                   ", properties=" + properties +
                   '}');
           System.out.println("save running....");
       }
   }
   ```

3. User方法

   ```java
   package com.springStudy.domain;
   public class User {
       private String name;
       private String addr;
       private int age;
       public User(String name, String addr, int age) {....}
       public String getName() {return name;}
       public void setName(String name) {this.name = name;}
       public String getAddr() {return addr;}
       public void setAddr(String addr) {this.addr = addr;}
       public int getAge() {return age;}
       public void setAge(int age) {this.age = age;}
       @Override
       public String toString() {....}
   }
   ```



### 1.3.3 导入配置文件

 配置xml文件: `<import resource="path/file.xml">`

 

## 1.4 相关API



#### 1.4.1 ApplicationContext相关API

1. ClassPathXmlApplicationContext 从类的根路径加载配置文件
2. FileSystemXmlApplicationContext  从磁盘路径加载配置文件
3. AnnotationConfigApplicationContext 当使用注解配置容器对象时,需要使用此类创建spring容器,用来读取注解

### 1.4.2 getBean()方法使用

1. public Object getBean(String name) throws BeanException{}

   按照名称查找Bean

2. public <T> T getBean(Class<T> requiredType) throws BeanException{} 

   按照类型名查找Bean  (不需要强制类型转化)

3. 实例

   ``` java
   public class UserController {
       public static void main(String[] args) {
           ApplicationContext app = new ClassPathXmlApplicationContext("applicationContext.xml");
   //        UserService userService = (UserService) app.getBean("userService");
           UserService userService = app.getBean(UserService.class);
           userService.save();
       }
   }
   ```



## 1.5 Spring配置数据源

> 数据源是指数据的来源的概括，包含了数据库位置 和 数据库类型等信息，实际上是一种数据连接的抽象。
>
> 连接池包括很多个这个的数据连接，要是时候可以从这个“池子”里取，不要可以放回去，”池子“直接放在机器内存中，我们访问数据库的时候就不需要找数据源要连接，直接在本地内存中取得连接，这样可以提高程序的性能。
> 形象点：
> 连接池好比水站，数据连接要像连接水站的分水管，当然水站的源头就是数据库(数据源)了。

常见的连接池: DBCP,C3P0,BoneCP,Druid等

### 1.5.1 动手配数据源

+ 导入数据源和驱动坐标
+ 创建数据源对象
+ 设置数据源的基本数据链接
+ 使用数据源获取连接资源和归还资源

#### c3p0

1. 手动创建c3p0数据源

   > c3p0 0.9.1.X时候一个jar包就可以   之后需要commons-java辅助包

   ```java
       @Test
   //    手动创建c3p0数据源
       public void test1() throws Exception {
           ComboPooledDataSource dataSource = new ComboPooledDataSource();
           dataSource.setDriverClass("com.mysql.cj.jdbc.Driver");
           dataSource.setJdbcUrl("jdbc:mysql://127.0.0.1:3306/web04_student?serverTimezone=GMT%2B8");
           dataSource.setUser("root");
           dataSource.setPassword("321321");
           Connection connection = dataSource.getConnection();
           System.out.println(connection);
           connection.close();
       }
   ```

2. 加载配置文件

   ``` java
   // jdbc.properties
   jdbc.Driver = com.mysql.cj.jdbc.Driver
   jdbc.Url = jdbc:mysql://127.0.0.1:3306/web04_student?serverTimezone=GMT%2B8
   jdbc.Username = root
   jdbc.Password = 321321    
   
   @Test
       //  手动创建c3p0数据源(加载配置文件)
       public void test3() throws Exception{
           ResourceBundle rb = ResourceBundle.getBundle("jdbc");
           String driver = rb.getString("jdbc.Driver");
           String url = rb.getString("jdbc.Url");
           String username = rb.getString("jdbc.Username");
           String password = rb.getString("jdbc.Password");
           ComboPooledDataSource dataSource = new ComboPooledDataSource();
           dataSource.setDriverClass(driver);
           dataSource.setJdbcUrl(url);
           dataSource.setUser(username);
           dataSource.setPassword(password);
           Connection connection = dataSource.getConnection();
           System.out.println(connection);
           connection.close();
       }
   ```

#### druid

1. 手动创建druid数据源

   ``` java
    @Test
      // 手动创建druid数据源
    public void test2() throws Exception{
        DruidDataSource dataSource = new DruidDataSource();
        dataSource.setDriverClassName("com.mysql.cj.jdbc.Driver");
        dataSource.setUrl("jdbc:mysql://127.0.0.1:3306/web04_student?serverTimezone=GMT%2B8");
        dataSource.setUsername("root");
        dataSource.setPassword("321321");
        DruidPooledConnection connection = dataSource.getConnection();
        System.out.println(connection);
        dataSource.close();
   
    }
   ```



### 1.5.2 Spring配数据源

> 因为c3p0和druid都有无参构造 都有set方法
>
> 所以可以把DataSource的创建权交给Spring容器

#### c3p0

1. Spring内直接配置

```java
...
    <bean id="c3p0DataSource" class="com.mchange.v2.c3p0.ComboPooledDataSource">
        <property name="driverClass" value="com.mysql.cj.jdbc.Driver"/>
        <property name="jdbcUrl" value="jdbc:mysql://127.0.0.1:3306/web04_student?serverTimezone=GMT%2B8"/>
        <property name="user" value="root"/>
        <property name="password" value="321321"/>
    </bean>    
...
@Test
    // Spring容器创建c3p0配置文件
    public void test4() throws Exception{
        ApplicationContext applicationContext = new ClassPathXmlApplicationContext("applicationContext.xml");
        DataSource bean = applicationContext.getBean(ComboPooledDataSource.class);
        Connection connection = bean.getConnection();
        System.out.println(connection);
        connection.close();
    }
```

2. Spring抽取并加载

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns="http://www.springframework.org/schema/beans"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
              <!-- 把所有的beans改为context -->
          xmlns:context="http://www.springframework.org/schema/context"
          xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
              <!-- 把所有的beans改为context -->
           http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd
           ">
   <!--   加载外部properties -->
       <context:property-placeholder location="classpath:jdbc.properties"/>
   
       <bean id="c3p0DataSource" class="com.mchange.v2.c3p0.ComboPooledDataSource">
           <property name="driverClass" value="${jdbc.Driver}"/>
           <property name="jdbcUrl" value="${jdbc.Url}"/>
           <property name="user" value="${jdbc.Username}"/>
           <property name="password" value="${jdbc.Password}"/>
       </bean>
   </beans>
   ```

   

#### druid

1. Spring内直接配置

```java
...
 <bean id="druidDataSource" class="com.alibaba.druid.pool.DruidDataSource">
        <property name="driverClassName" value="com.mysql.cj.jdbc.Driver"/>
        <property name="url" value="jdbc:mysql://127.0.0.1:3306/web04_student?serverTimezone=GMT%2B8"/>
        <property name="username" value="root"/>
        <property name="password" value="321321"/>
    </bean>
...
@Test
// Spring容器创建druid配置文件
public void test5() throws Exception{
    ApplicationContext applicationContext = new ClassPathXmlApplicationContext("applicationContext.xml");
    DataSource bean = applicationContext.getBean(DruidDataSource.class);
    Connection connection = bean.getConnection();
    System.out.println(connection);
    connection.close();
}
```



## 1.6 注解开发



### 1.6.1 原始注解

> Spring原始注解主要替代的是<Bean>当中的配置

| 注解           | 说明                                                         |
| -------------- | ------------------------------------------------------------ |
| @Component     | 使用在类上用于实例化Bean  等同于`<bean class=".....">`       |
| @Controller    | 使用在web层类上用于实例化Bean                                |
| @Service       | 使用在service层类上用于实例化Bean                            |
| @Repository    | 使用在dao层类上用于实例化Bean                                |
| @Autowired     | 使用在字段上用于根据类型依赖注入    按照类型进行注入         |
| @Qualifier     | 结合@Autowired一起使用用于根据名称进行依赖注入 按照id注入    |
| @Resource      | 相当于@Autowired+@Qualifier，按照名称进行注入 等价于上面两个结合体 |
| @Value         | 注入普通属性                                                 |
| @Scope         | 标注Bean的作用范围                                           |
| @PostConstruct | 使用在方法上标注该方法是Bean的初始化方法                     |
| @PreDestroy    | 使用在方法上标注该方法是Bean的销毁方法                       |

**入门示例**

1. 把bean标签换成@Component

2. bean的注入换成@Autowried和@Qualifier

3. 修改xml文件指定扫描的包

4. controll, service, dao

   ``` java
   //Dao
   // <bean id="userDao" class="com.gjstudy.test.dao.Impl.UserDaoImpl" />
   @Component("userDao")
   public class UserDaoImpl implements UserDao {
       @Override
       public void save() {
           System.out.println("save running......");
       }
   }
   
   // Service
   @Component("userService")
   public class UserServiceImpl implements UserService {
       //<property name="userDao" ref="userDao"/>
       @Autowired
       @Qualifier("userDao")   //括号内字符串就是需要注入的内容 本例为ref的值
       private UserDao userDao;
       public void setUserDao(UserDao userDao) {
           this.userDao = userDao;
       }
       @Override
       public void save() {
           userDao.save();
       }
   }
   
   // Controller
   public class UserController {
       public static void main(String[] args) {
           ApplicationContext app = new ClassPathXmlApplicationContext("applicationContext.xml");
           UserService userService = app.getBean(UserService.class);
           userService.save();
       }
   }
   ```

5. xml

   ``` xml
   <!--&lt;!&ndash;声明userDao&ndash;&gt;-->
   <!--    <bean id="userDao" class="com.gjstudy.test.dao.Impl.UserDaoImpl" />-->
   <!--&lt;!&ndash;  声明userService  &ndash;&gt;-->
   <!--    <bean id="userService" class="com.gjstudy.test.service.Impl.UserServiceImpl">-->
   <!--&lt;!&ndash;   依赖注入  name的名字是userService方法中的set方法发的去set方法名     &ndash;&gt;-->
   <!--        <property name="userDao" ref="userDao"/>-->
   <!--    </bean>-->
   <context:component-scan base-package="com.gjstudy" />
   ```

   

**逐个讲解**


1. Component

   等同于`<bean id="userDao" class="com.gjstudy.test.dao.Impl.UserDaoImpl" />` 括号内的字符串就是bean下的id

   ``` java
   // <bean id="userDao" class="com.gjstudy.test.dao.Impl.UserDaoImpl" />
   @Component("userDao")
   public class UserDaoImpl implements UserDao {
       @Override
       public void save() {
           System.out.println("save running......");
       }
   }
   ```

2. Controller, Service, Repository

   这三个作用等同于Component只是为了区分DAO,Service,Web层
   
   ``` java
   //Dao
   // <bean id="userDao" class="com.gjstudy.test.dao.Impl.UserDaoImpl" />
   @Repository("userDao")
   public class UserDaoImpl implements UserDao {
       @Override
       public void save() {
           System.out.println("save running......");
       }
   }
   ```
   
3. Autowired

   按照类型进行注入(自动读取变量类型的bean注入到当前)

   ``` java
   @Service("userService")
   public class UserServiceImpl implements UserService {
       //<property name="userDao" ref="userDao"/>
       @Autowired
       private UserDao userDao;
       public void setUserDao(UserDao userDao) {
           this.userDao = userDao;
       }
       @Override
       public void save() {
           userDao.save();
       }
   }
   ```

4. Qualifier

   按照id进行注入(但必须结合Autowired使用 可以理解为Autowired先进行扫描)

   值得一提:使用xml方式需要写set方法,使用注解方式可以不用写set

   ``` java
   @Service("userService")
   public class UserServiceImpl implements UserService {
       //<property name="userDao" ref="userDao"/>
       @Autowired
       @Qualifier("userDao")   //括号内字符串就是需要注入的内容的id
       private UserDao userDao;
       @Override
       public void save() {
           userDao.save();
       }
   }
   ```

5. Resource

   简化了Autowired+Qualifier的写法  括号内的name写需要注入bean的id

   ``` java
   @Service("userService")
   public class UserServiceImpl implements UserService {
       @Resource(name = "userDao")   //name是需要注入bean的id
       private UserDao userDao;
       @Override
       public void save() {
           userDao.save();
       }
   }
   ```

    

6. Value

   普通数据类型赋值给变量

   ${jdbc.Driver}会在Spring容器中找到加载外部配置文件的代码 然后根据${}去查找

   ``` java
   @Repository("userDao")
   public class UserDaoImpl implements UserDao {
       @Value("${jdbc.Driver}")
       private String driver;
       @Override
       public void save() {
           System.out.println(driver);
           System.out.println("save running......");
       }
   }
   ```

7. Scpoe

   产生bean的数量: 多个或者单个 prototype/singleton

8. PostConstruct

   init初始化方法标记

9. ProDestroy

   destroy销毁方法标记





### 1.6.2 Spring新注解

以上原始注解能解决一部分问题比如加载Bean,依赖注入等,但是也有一些不能解决比如:

+ 非自定义的Bean:`<bean>`
+ 加载properties文件: `<context:property-placeholder>`
+ 组件扫描:`<context:component-scan>`
+ 引入其他配置文件:`<import>`



|注解|说明|
|---|----|
|@Configuration|用于指定当前类是一个Spring配置类，当创建容器时会从该类上加载注解   简单讲:用类的方式代替xml|
|@ComponentScan|用于指定Spring在初始化时要扫描的包。作用和Spring的xml配置文件中的<context:component-scan beas-package=“com.szl”/>一样|
|@Bean|使用在方法上，标注将该方法的返回值存储到Spring容器中|
|@PropertySource|用于加载.properties文件中中的配置|
|@Import|用于导入其他配置类|



# 02_Spring集成web环境



## 2.1 ApplicationContext对象的获取方式

**示例**

``` java
public class UserServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        ApplicationContext app = new ClassPathXmlApplicationContext("applicationContext.xml");
        UserService userService = app.getBean(UserService.class);
        userService.save();
        resp.getWriter().println("save running ....");
    }
    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        super.doGet(req, resp);
    }
}
```

### 出现的问题

可以看到每次使用Servlet时都需要创建ApplicationContext对象。

### 解决思路

在web项目中可以使用ServletContextListener监听器监听Web应用的启动。这样就可以在Web应用启动时候加载Spring的配置文件,创建ApplicationContext对象。

如此一来就可以在任何地方从域中获取应用上下文对象ApplicationContext。

### 实现过程

1. 创建监听器 -> 在监听器中创建ApplicationContext对象

   ``` java
   public class ContextLoaderListener implements ServletContextListener {
       // 上下文初始化方法
       public void contextInitialized(ServletContextEvent servletContextEvent) {
           ApplicationContext app = new ClassPathXmlApplicationContext("applicationContext.xml");
           // 将Spring应用的上下文对象存储到最大的域(ServletContext)中
           ServletContext servletContext = servletContextEvent.getServletContext();
           servletContext.setAttribute("app", app);
       }
       // 上下文销毁方法
       public void contextDestroyed(ServletContextEvent servletContextEvent) {
       }
   }
   ```

2. web.xml中配置监听器

   ``` xml
   ......
   <!-- 配置监听器  -->
       <listener>
           <listener-class>com.gjstudy.listener.ContextLoaderListener</listener-class>
       </listener>
   ......
   ```

3. 重写servlet方法

   ``` java
   public class UserServlet extends HttpServlet {
       protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
           ApplicationContext app = (ApplicationContext) req.getServletContext().getAttribute("app");
           UserService userService = app.getBean(UserService.class);
           userService.save();
       }
   ```

   

### 优化实现过程

> applicationContext.xml写死了,后期更改麻烦
>
> 将applicationContext.xml配置为web.xml的初始化参数

1. web.xml

   ``` xml
   ......
   <!--  初始化参数 -->
       <context-param>
           <param-name>applicationConfig</param-name>
           <param-value>applicationContext.xml</param-value>
       </context-param>
   .....
   ```

   

2. 监听器.java

   ``` java
   ......
   public class ContextLoaderListener implements ServletContextListener {
       // 上下文初始化方法
       public void contextInitialized(ServletContextEvent servletContextEvent) {
           ServletContext servletContext =servletContextEvent.getServletContext();
           // 从servletContext域中获取初始化参数
           String applicationConfig = servletContext.getInitParameter("applicationConfig");
           ApplicationContext app = new ClassPathXmlApplicationContext(applicationConfig);
           // 将Spring应用的上下文对象存储到最大的域(ServletContext)中
           servletContext.setAttribute("app", app);
       }
   ......
   ```



## 2.2 ApplicationContext对象的获取工具

> 上文手动创建了ContextLoaderListener监听器.
>
> Spring容器提供了ContextLoaderListener就是上述功能的封装,
>
> 该监听器内部加载Spring配置文件 -> 创建应用上下文对象 -> 并存储到ServletContext域中 -> 提供了WebApplicationContextUtils工具类供使用者获取应用上下文对象

### 实现思路

1. 在web.xml中配置ContextLoaderListener监听器(需要导入spring-web坐标)
2. 使用WebApplicationContextUtils获得应用上下文对象ApplicationContext

### 实现

``` xml
......
    <!--  初始化参数(名字必须是这个) -->
    <context-param>
        <param-name>contextConfigLocation</param-name>
        <param-value>classpath:applicationContext.xml</param-value>
    </context-param>
<!-- 配置监听器  -->
    <listener>
        <listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
    </listener>
......
```

``` java
public class UserServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        ServletContext servletContext = this.getServletContext();
        ApplicationContext app = WebApplicationContextUtils.getWebApplicationContext(servletContext);
        UserService userService = app.getBean(UserService.class);
        userService.save();
    }
```



# 03_SpringMVC

> Spring MVC 是 Spring 提供的一个基于 MVC 设计模式的轻量级 Web 开发框架，本质上相当于 Servlet。
>
> Spring MVC 是结构最清晰的 Servlet+JSP+JavaBean 的实现，是一个典型的教科书式的 MVC 构架，不像 Struts 等其它框架都是变种或者不是完全基于 MVC 系统的框架。



## 3.1 快速入门

### 3.1.1 实现步骤

1. 导入SpringMVC坐标
2. 配置Servlet(核心控制器DispathcerServlet)
3. 编写POJO(习惯的把POJO成为控制器Controller)(创建controller类和视图页面jsp)
4. 将Controller使用注解配置到Spring容器中(@Component   @Controller)
5. 配置spring-mvc.xml(或者配置组件扫描)
6. 测试

### 3.1.2 实现

1. 导入SpringMVC坐标

   ``` xml
   ......
           <dependency>
               <groupId>org.springframework</groupId>
               <artifactId>spring-webmvc</artifactId>
               <version>5.0.5.RELEASE</version>
           </dependency>
   ......
   ```

2. 配置Servlet(核心控制器DispathcerServlet)

   ``` xml
   <!-- 配置SpringMVC的前端控制器  -->
       <servlet>
           <servlet-name>DispatcherServlet</servlet-name>
           <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
           <!-- 配置什么时候创建对象  -->
           <load-on-startup>1</load-on-startup>
       </servlet>
       <servlet-mapping>
           <servlet-name>DispatcherServlet</servlet-name>
           <url-pattern>/</url-pattern>
       </servlet-mapping>
   ```

3. 编写POJO(习惯的把POJO成为控制器Controller)(创建controller类和视图页面jsp)

   ``` java
   public class UserController {
       public String save(){
           System.out.println("Controller save ....");
           return "success.jsp";
       }
   }
   ```

4. 将Controller使用注解配置到Spring容器中(@Component   @Controller)

   ```java
   @Controller
   public class UserController {
       // 配置映射 访问localhost:8080/success时候执行这个
       @RequestMapping(value = "/success", product = "text/html;charset=UTF-8")
       public String save(){
           System.out.println("Controller save ....");
           // return后跟的是需要跳转的视图jsp
           return "success.jsp";
       }
   }
   ```

5. 配置spring-mvc.xml(或者配置组件扫描)

   + spring-mvc.xml

     ``` xml
     <!-- Controller的组件扫描  -->
         <context:component-scan base-package="com.gjstudy.controller" />
     ```

   + 使用初始化参数加载springmvc配置文件

     ``` xml
     <!-- 配置SpringMVC的前端控制器  -->
         <servlet>
             <servlet-name>DispatcherServlet</servlet-name>
             <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
             <!-- 使用初始化参数加载配置文件   -->
             <init-param>
                 <param-name>contextConfigLocation</param-name>
                 <param-value>classpath:spring-mvc.xml</param-value>
             </init-param>
             <!-- 配置什么时候创建对象  -->
             <load-on-startup>1</load-on-startup>
         </servlet>
         <servlet-mapping>
             <servlet-name>DispatcherServlet</servlet-name>
             <url-pattern>/</url-pattern>
         </servlet-mapping>
     ```

6. 测试



## 3.2 SpringMVC工作流程解析



### 3.2.1 简易执行流程

![image-20210727231754064](E:\study\MarkDown\SSM\Spring\springmvc.png)

1. 客户端发出请求
2. tomcat引擎接收
3. tomcat封装request,response对象给service(值得一提SpringMVC只有一个Servlet)
4. 由service方法发送给controller
5. controller方法的return一个页面给客户端

### 3.2.2 SpringMVC执行流程

![image-20210728201709251](E:\study\MarkDown\SSM\Spring\springmvc2.png)

1. 客户端发出请求 -> 前端控制器DispathcherServlet组件(分析请求,通过处理器映射器找资源)
2. DispathcherServlet组件请求查询Handler -> 处理器映射器HandlerMapping组件(负责找资源,返回的是一串资源地址即处理器执行链)
3. HanlerMapping组件返回处理器执行链HandlerExecutionChain -> DispathcherServlet组件(得到了资源地址,通过处理器适配器去调度具体的资源)
4. DispathcherServlet组件请求执行Handler -> 处理器适配器HandlerAdapter组件(负责真正的调用资源也就是自己写的Controller)
5. Controller得到HandlerAdapter组件的请求然后相应回去
6. HandlerAdapter组件处理请求的结果也就是返回ModelAndView对象 -> DispathcherServlet组件(得到了响应处理的结果例如某个jsp页面的名字,通过请求试图解析器解析出来视图对象)
7. DispathcherServlet组件请求试图解析器 -> 试图解析器ViewResolver组件(负责解析出视图对象)
8. ViewResolver组件返回View对象 -> DispathcherServlet组件(得到了视图对象)
9. DispathcherServlet组件渲染试图对象(视图Jsp) -> 客户端



## 3.3 SpringMVC注解解析

1. @RequestMapping

   将请求的虚拟地址映射成具体的方法类

   应用位置:

   + 类上: 请求URL的一级访问目录. 此处不写的话,相当于应用的根目录。
   + 方法上：请求的URL的第二级访问目录，与类上的@RequestMapping标注的一级目录一起组成访问路径

   属性:

   + value: 用于指定请求的URL,和path属性作用是一样的
   + method: 用于指定请求方式
   + params: 用于指定限制请求参数的条件.他支持简单的表达式.请求参数的key和value必须配置的一样.

   示例：

   ``` java
   @Controller
   @RequestMapping("/user")
   // 相当于访问localhost:8080/user 可以用于区分controller
   public class UserController {
       // 配置映射 访问localhost:8080/user/success时候执行这个
       @RequestMapping(value = "/success", product = "text/html;charset=UTF-8")
       public String save(){
           ......
           // 如此写的话返回的jsp相对user目录,或者写绝对路径
           return "/success.jsp";
       }
   }
   ```



## 3.4 SpringMVC的XML配置解析

**给返回页面的字符串加前后缀**

不具体讲了,太tmm麻烦了感觉啥用没有。知道有这么一回事,需要的时候去谷歌。

**配置重定向/转发:**

重定向: return "redirect:xxxx.jsp"

转发: return "forward:xxxx.jsp"



## 3.5 SpringMVC数据响应

**SpringMVC数据相应方式:**

1. 页面跳转: 
   + 直接返回字符串
   + 通过ModelAndView对象返回
2. 回写数据:
   + 直接返回字符串
   + 返回对象或集合



### 3.5.1 页面跳转

1. 返回字符串形式:

   直接返回字符串: 此种方式会将返回的字符串与视图解析器的前后缀拼接后进行跳转。

2. 返回ModelAndView对象

   ``` java
       //访问localhost:8080/user/quick2时候重定向success.jsp
       @RequestMapping("/quick2")
       public ModelAndView save2(){
           /*
           * Model: 模型   作用封装数据
           * View:   视图   作用展示数据
           * */
           ModelAndView modelAndView = new ModelAndView();
           // 设置视图名称
           modelAndView.setViewName("/success.jsp");
           // 设置模数据
           modelAndView.addObject("name", "zhangsan");
           return modelAndView;
       }
   }
   // 前端使用el表达式${name} 可以获取值
   
   // 第二种方式
       // SpringMVC会帮你注入modelAndView对象
       @RequestMapping("/quick3")
       public ModelAndView save3(ModelAndView modelAndView){
           modelAndView.setViewName("/success.jsp");
           modelAndView.addObject("name", "zhangsan");
           return modelAndView;
       }
   ```



### 3.5.2 回写数据



#### 3.5.2.1直接返回字符串

Web基础时候,客户端访问服务端,如果想直接回写字符串作为响应体的话,只需要request.getWriter().print("xxxxx")即可。

+ 方式一: 使用SpringMVC框架注入request对象,调用他的request.getWriter().print("xxxxx")即可。回写不需要视图跳转业务方法返回void 

+ 方式二:使用@ResponseBody注解告知SpringMVC框架,方法返回字符串不是跳转而是字符串

+ ```java
  // SpringMVC会帮你注入modelAndView对象
  @RequestMapping(value = "/quick4", product="text/html;charset=UTF-8")
  @ResponseBody
  public String save4(){
      return "Return String";
  }
  ```

#### 3.5.2.2返回json

+ 方式一:

  ``` java
  @RequestMapping("/quick4")
  @ResponseBody
  public String save4(){
      return "{\"name\":\"zhangsan\",\"age\":18}";
  }
  ```

+ 方式二: 使用json转换工具

  1. 导包

     ```xml
     ......
     <dependency>
          <groupId>com.fasterxml.jackson.core</groupId>
          <artifactId>jackson-core</artifactId>
          <version>2.9.0</version>
     </dependency>
     
     <dependency>
         <groupId>com.fasterxml.jackson.core</groupId>
         <artifactId>jackson-databind</artifactId>
         <version>2.9.0</version>
     </dependency>
     
     <dependency>
         <groupId>com.fasterxml.jackson.core</groupId>
         <artifactId>jackson-annotations</artifactId>
         <version>2.9.0</version>
     </dependency>
     ......
     ```

  2. controller

     ```java
         @RequestMapping(value = "/quick4", product="text/html;charset=UTF-8")
         @ResponseBody
         public String save4() throws JsonProcessingException {
             User user = new User();
             user.setUsername("zhangsan");
             user.setAge(18);
             ObjectMapper objectMapper = new ObjectMapper();
             String s = objectMapper.writeValueAsString(user);
             return s;
         }
     }
     ```



#### 3.5.2.3 返回对象或集合

1. 方式一:

    通过SpringMVC帮助我们对对象或集合进行json字符串的转化并回写,为处理器适配器配置消息转化参数,指定使用jackson进行对象或集合的转换,因此需要在spring-mvc.xml中进行配置。

    ``` xml
    ......
    <bean class="org.springframework.web.servlet.mvc.method.annotation.RequestMappingHandlerAdapter">
        <property name="messageConverters">
            <list>
                <bean class="org.springframework.http.converter.json.MappingJackson2HttpMessageConverter"/>
            </list>
        </property>
    </bean>
        ......
    ```

    ```java
    @RequestMapping("/quick4")
    @ResponseBody
    public User save4() {
        User user = new User();
        user.setUsername("zhangsan");
        user.setAge(18);
        return user;
    }
    ```

2. 方式二:

   上面配置xml比较麻烦,可以使用mvc的注解驱动代替上述的操作。(在spring-mvc.xml中)

   ``` xml
   ......
   <!-- mvc的注解驱动  -->
   <mvc:annotation-driven />
   ......
   ```

   解释:`<mvc:annotation-driven />`

   主要就是为了Spring MVC来用的，提供Controller请求转发，json自动转换等功能




## 3.6 SpringMVC获得请求数据



### 3.6.1 获得请求参数

客户端请求参数的格式:name=value&name=value....

服务端收到请求参数有时候需要进行数据的封装,SpringMVc可以接收几乎所有常见类型的参数:

+ 基本数据类型
+ POJO类型
+ 数组类型
+ 集合类型



### 3.6.2 获得基本类型的参数

Controller中业务方法的参数名要与请求参数的name一致,参数值会自动匹配映射

```java
//访问 http://localhost:8080/user/quick5?username=lisi&age=89

@RequestMapping("/quick5")
@ResponseBody
public void save5(String username, int age) {
    System.out.println(username);
    System.out.println(age);
}
```



### 3.6.3 获得POJO类型参数

Controller中业务方法的POJO 参数名要与请求参数的name一致,参数值会自动匹配映射

@

```java
//访问 http://localhost:8080/user/quick6?username=lisi&age=89

@RequestMapping("/quick6")
@ResponseBody
public void save6(User user) {
    System.out.println(user.getUsername());
    System.out.println(user.getAge());
    System.out.println(user);
}
```



### 3.6.4 获得数组类型参数

Controller中业务方法的数组名要与请求参数的name一致,参数值会自动匹配映射



```java
//访问 http://localhost:8080/user/quick6?username=lisi&username=89

@RequestMapping("/quick6")
@ResponseBody
public void save6(Sring[] username) {
    System.out.println(Arrays.asList(username));
}
```



### 3.6.5 获得集合类型参数

#### 3.6.5.1 不能直接写方法形参情况

获得集合参数时,需要将集合参数包装到一个POJO中才可以

1. 新建一个POJO

   ```java
   public class VO {
       private List<User> users;
       public List<User> getUsers() {return users;}
       public void setUsers(List<User> users) {this.users = users;}
           public String toString() {......}
   }
   ```

2. Controller

   ```java
   @RequestMapping("/quick7")
   @ResponseBody
   public void save7(VO vo) {
       System.out.println(vo);
   }
   ```

3. 前端jsp页面

   ``` jsp
   <%--注意name里的写法--%>
   <form action="${pageContext.request.contextPath}/user/quick7" method="post">
       <input type="text" name="users[0].username"/><br />
       <input type="text" name="users[0].age"/><br />
       <input type="text" name="users[1].username"/><br />
       <input type="text" name="users[1].age"/><br />
       <input type="submit" name="提 交">
   </form>
   ```

   

#### 3.6.5.2 可以直接写方法形参情况

当使用ajax提交时,可以指定contextType为json形式,那么在方法的参数位置使用@RequestBody可以直接接收集合数据

1. Controller

   ``` java
   @RequestMapping("/quick8")
   @ResponseBody
   public void save8(@RequestBody List<User> userList) {
       System.out.println(userList);
   }
   ```

2. jsp

   ``` jsp
   <%@ page contentType="text/html;charset=UTF-8" language="java" %>
   <html>
   <head>
       <title>Title</title>
       <script src="${pageContext.request.contextPath}/js/jquery-1.12.4.js"></script>
       <script>
           let userList = [];
           userList.push({username: 'zhangsan', age: 18})
           userList.push({username: 'lisi', age: 28})
   
           $.ajax(
               {
                   type: "POST",
                   url: "${pageContext.request.contextPath}/user/quick8",
                   data: JSON.stringify(userList),
                   contentType: "application/json; charset=utf-8"
               }
           )
   
       </script>
   </head><body></body></html>
   
   ```

3. 配置静态资源访问

   ``` xml
   <mvc:resources mapping="/js/**" location="/js/" />
   ```




### 3.6.6 静态资源访问



SpringMVC中前端页面的js和img等资源也需要进行HTTP请求(例如:localhost/js/jqery.js).

对于服务器而言/js/..也是一次请求,但是由于js没有配置相应的servlet接收则前端会报找不到资源404错.

此时就需要开启某些路径(一般是img,js等)的访问权限称之为静态资源访问

1. 方式一:

   ```xml
   <!-- 开启镜头资源访问 -->
   <!-- mapping是url的映射 localtion是具体的本机资源地址  -->
   <!-- 即就是访问xxxx/js/...时候不进入servlet而直接返回静态资源 -->
   <mvc:resources mapping="/js/**" location="/js/" />
   ```

2. 方式二:

   ``` xml
   <!-- 代表找不到的servlet交给上一级容器即tomcat去找 -->
   <mvc:default-servlet-handler />
   ```



### 3.6.7 请求数据乱码问题

当使用POST请求时候数据会出现乱码,需要设置一个过滤器进行编码的过滤

配置全局filter:

```xml
<!-- web.xml -->
<filter>
    <filter-name>CharacterEncodingFilter</filter-name>
    <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
    <init-param>
        <param-name>encoding</param-name>
        <param-value>UTF-8</param-value>
    </init-param>
</filter>
<filter-mapping>
    <filter-name>CharacterEncodingFilter</filter-name>
    <url-pattern>/*</url-pattern>
</filter-mapping>
```



### 3.6.8 请求参数注解绑定

前端的name和后端接收的name可以不相同但是需要使用@RequestParam注解进行绑定

**@RequestParam参数:**

+ value: 请求的参数名(url问号后的东西)
+ required: 默认是turn即在请求时候必须带有参数否则报错   false则不带参数请求时候默认后台接收到null
+ defaultValue: 提供一个默认的参数,请求的时候没有提供参数则使用这个默认的参数

```java
// 访问 localhost:8080/user/quick9?name=zhangsan

@RequestMapping("/quick9")
@ResponseBody
// @RequestParam参数: value=""访问的url的name required=ture(默认) 即访问 localhost:8080/user/quick9不带参数时会报错
public void save9(@RequestParam(value = "name") String username) {
    System.out.println(username);
}
```



### 3.6.9 获取Restful风格的参数

>  restful是一种软件架构风格、设计风格，而不是标准，只是提供了一组设计原则和约束条件。它主要用于客户端和服务器交互类的软件。基于这个风格设计的软件可以更简洁，更有层次，更易于实现缓存等机制。

Restful风格的请求是使用"url+请求方式"表示一次请求的目的,HTTP协议里面四个表示操作方式的动词如下:

+ GET:           用于获取资源
+ POST:        用于新建资源
+ PUT:          用于更新资源
+ DELETE:    用于删除资源

例如:

+ /user/1    GET:                得到id=1的user数据
+ /user/1    DELETE:          删除id=1的user数据
+ /user/1    PUT:                更新id=1的user数据
+ /user        POST:             新增user数据

简单说人话来讲,就是原来使用url?...后的东西可以使用url的资源定位符号/进行传参

```java
// http://localhost:8080/user/quick10/zhangsan 后台可以得到quick10后的内容

@RequestMapping("/quick10/{username}")
@ResponseBody
public void save10(@PathVariable(value = "username") String username) {
    System.out.println(username);
}
```



### 3.6.10 自定义类型转化器

> 客户端请求的任何类型都是string类型的字符串

SpringMVC已经内置了一部分的类型转化器,例如String -> int

但是内部的转换器不够用时就需要自己定义类型转化器(比如日期)



**自定义类型转化器步骤:**

1. 定义转化器类实现Converter接口
2. 在配置文件中声明转化器
3. 在`<annontation-driver>`中引用转化器



**实现:**

1. 新建一个类实现Converter接口

   ``` java
   // 参数前者转后者  即 String转Date
   public class DateConverter implements Converter<String, Date> {
       @Override
       public Date convert(String s) {
           // 将字符串转成Date
           SimpleDateFormat simpleDateFormat = new SimpleDateFormat("yyyy-MM-dd");
           Date date = null;
           try {
               date = simpleDateFormat.parse(s);
           } catch (ParseException e) {
               e.printStackTrace();
           }
           return date;
       }
   }
   ```

2. 在配置文件中声明转化器

   ``` xml
   <!--  自定义类型转化器  -->
       <bean id="conversionService" class="org.springframework.context.support.ConversionServiceFactoryBean">
           <property name="converters">
               <list>
                   <bean class="com.gjstudy.converter.DateConverter"/>
               </list>
           </property>
       </bean>
   ```

3. 在`<annontation-driver>`中引用转化器

   ``` xml
    <mvc:annotation-driven conversion-service="conversionService"/>
   ```



### 3.6.11 获得Servlet相关API

SpringMVC支持原生ServletAPI对象作为控制器注入例如:HttpRequest,HttpResponse,HttpSession

```java
// 方法参数直接写就好 SpringMVC调用的时候会自动注入
@RequestMapping("/quick12")
@ResponseBody
public void save12(HttpServletRequest request, HttpServletResponse response) throws IOException {
    response.getWriter().println("hahahah");
}
```



#### 3.6.12 获得请求头

#### 3.6.12.1 @RequestHeader

使用@RequestHeader可以获得请求头信息,相当于web阶段的request.getHeader(name)

**@RequestHeader参数如下:**

+ value:  请求头的名称
+ required: 是否必须携带此请求头

```java
@RequestMapping("/quick13")
@ResponseBody
public void save13(@RequestHeader("User-Agent") String s) {
    System.out.println(s);
}
```

#### 3.6.12.2 @CookieValue

获得cookie

**@CookieValue参数:**

+ value: 指定cookie名称
+ required: 是否必须携带此请求头

```java
//  cookie是键值对的形式
@RequestMapping("/quick13")
@ResponseBody
public void save13(@RequestHeader("Cookie") String s) {
    System.out.println(s);
}
// results  JSESSIONID=F6AF171A65967442DD9658E61250785F
@RequestMapping("/quick14")
@ResponseBody
public void save14(@CookieValue("JSESSIONID") String s) {
    System.out.println(s);
}
// results F6AF171A65967442DD9658E61250785F
```



### 3.6.12 文件上传



文件上传三要素: 

+ 表单项 type = "file"
+ 表单的提交方式 post
+ 表单的enctype属性是多部份表单形式, 即enctype = "multipart/form-data"

文件上传原理:

+ 当form表单设置为多部份表单时,request.getParameter()将失效

+ enctype = "application/x-www-form-urlencoded"时,form表单提交的正文格式是:key=value&key=value....

+ 当form的enctype="Multiplart/form-data"时,表单提交的正文就是:

  ```html
  --分隔符(boundary)[换行]
  Content-Disposition: form-data; name="参数名"[换行] 
  [换行]
  参数值[换行]
  --分隔符(boundary)[换行]
  Content-Disposition: form-data; name="图片名"; filename="图片文件名"[换行]
  Content-Type: 类型[换行]
  [换行]
  图片文件的二进制内容[换行]
  --分隔符(boundary)--
  ```

#### 3.6.12.1 单文件上传步骤

1. 导入fileupload坐标和io坐标
2. 配置文件上传解析器
3. 编写文件夹上传代码



**示例:**

1. 导入fileupload坐标和io坐标

   ```xml
   ......
   <dependency>
       <groupId>commons-fileupload</groupId>
       <artifactId>commons-fileupload</artifactId>
       <version>1.3.1</version>
   </dependency>
   <dependency>
       <groupId>commons-io</groupId>
       <artifactId>commons-io</artifactId>
       <version>2.3</version>
   </dependency>
   ......
   ```

2. 配置文件上传解析器

   ```xml
   ......
   <!-- 配置文件上传解析器   -->
   <bean id="multipartResolver" class="org.springframework.web.multipart.commons.CommonsMultipartResolver">
       <!--   文件上传总大小     -->
       <property name="maxInMemorySize" value="5242800"/>
       <!--   上传单个文件大小     -->
       <property name="maxUploadSizePerFile" value="5242800"/>
       <!--  上传文件的编码类型      -->
       <property name="defaultEncoding" value="utf-8"/>
   </bean>
   ......
   ```

3. 编写文件夹上传代码

   + ```java
         @RequestMapping("/quick15")
         @ResponseBody
         public void save15(String fileName, MultipartFile uploadFile) throws IOException {
             //获得文件名
             String filename = uploadFile.getOriginalFilename();
             //保存文件
             uploadFile.transferTo(new File("E:\\upload\\"+filename));
         }
     ```

   + form.jsp

        ``` jsp
        <form action="${pageContext.request.contextPath}/user/quick15" method="post" enctype="multipart/form-data">
            名称:<input type="text" name="fileName" />
            文件:<input type="file" name="uploadFile" />
            <input type="submit" name="提 交" />
        </form>
        ```




#### 3.6.12.2 多文件上传

和单文件上传类型,只需要在接收方法的参数多加入一个参数或者使用MultipartFile[]数组也可以

**使用多参数:**

```jsp
<form action="${pageContext.request.contextPath}/user/quick16" method="post" enctype="multipart/form-data">
    文件1:<input type="file" name="uploadFile" /><br>
    文件2:<input type="file" name="uploadFile2" /><br>
    <input type="submit" name="提 交" />
</form>
```

```java
@RequestMapping("/quick16")
@ResponseBody
public void save16(MultipartFile uploadFile,MultipartFile uploadFile2) throws IOException {
    //获得文件名
    String filename = uploadFile.getOriginalFilename();
    String filename2 = uploadFile2.getOriginalFilename();
    //保存文件
    uploadFile.transferTo(new File("E:\\upload\\"+filename));
    uploadFile2.transferTo(new File("E:\\upload\\"+filename2));
}
```

**使用MultipartFile[]数组:**

```jsp
<form action="${pageContext.request.contextPath}/user/quick16" method="post" enctype="multipart/form-data">
    文件1:<input type="file" name="uploadFile" /><br>
    文件2:<input type="file" name="uploadFile" /><br>
    <input type="submit" name="提 交" />
</form>
```

```java
@RequestMapping("/quick17")
@ResponseBody
public void save17(MultipartFile[] uploadFile) throws IOException {
    for (MultipartFile multipartFile : uploadFile){
        String filename = multipartFile.getOriginalFilename();
        multipartFile.transferTo(new File("E:\\upload\\"+filename));
    }
}
```



# 04_SpringJdbcTemplate

Spring对数据库的操作在jdbc上面做了深层次的封装，使用spring的注入功能，可以把DataSource注册到JdbcTemplate之中。



## 4.1 JdbcTemplate开发步骤

1. 导入spring-jdbc和spring-tx坐标
2. 创建数据库表和实体
3. 创建JdbcTemplate对象执行数据库操作

## 4.2 快速入门

1. 导入spring-jdbc和spring-tx坐标

   ```xml
   <dependency>
       <groupId>org.springframework</groupId>
       <artifactId>spring-jdbc</artifactId>
       <version>5.0.5.RELEASE</version>
   </dependency>
   <dependency>
       <groupId>org.springframework</groupId>
       <artifactId>spring-tx</artifactId>
       <version>5.0.5.RELEASE</version>
   </dependency>
   ```

2. 创建数据库表和实体

   mysql> show columns from account;

   | Field | Type         | Null | Key | Default | Extra |
   |-------|--------------|------|-----|---------|-------|
   | name  | varchar(255) | YES  |     | NULL    |       |
   | money | float        | YES  |     | NULL    |       |

### 4.2.1 基础款

创建JdbcTemplate对象执行数据库操作

``` java
// 测试jdbc模板开发步骤
public void test1() throws PropertyVetoException {
// 创建数据源对象
ComboPooledDataSource dataSource = new ComboPooledDataSource();
dataSource.setDriverClass("com.mysql.cj.jdbc.Driver");
dataSource.setJdbcUrl("jdbc:mysql://localhost:3306/web01");
dataSource.setUser("root");
dataSource.setPassword("321321");

JdbcTemplate jdbcTemplate = new JdbcTemplate();
// 设置数据源对象 Connection对象(知道数据源在哪里)
jdbcTemplate.setDataSource(dataSource);
// 执行操作
jdbcTemplate.update("insert into account values(?,?)", "李四", 13);
}
```



### 4.2.2 高级一点点

Spring生成JdbcTemplate对象

1. applicationContext.xml

   ```xml
   <!-- 数据源对象   -->
   <bean id="dataSource" class="com.mchange.v2.c3p0.ComboPooledDataSource">
       <property name="driverClass" value="com.mysql.cj.jdbc.Driver"/>
       <property name="jdbcUrl" value="jdbc:mysql://localhost:3306/web01"/>
       <property name="user" value="root"/>
       <property name="password" value="321321"/>
   </bean>
   <!-- jdbc模板对象   -->
   <bean id="jdbcTemplate" class="org.springframework.jdbc.core.JdbcTemplate">
       <property name="dataSource" ref="dataSource"/>
   </bean>
   ```

2. test.java

   ```java
   @Test
   // Spring产生的数据源
   public void test2(){
       ClassPathXmlApplicationContext app = new ClassPathXmlApplicationContext("applicationContext.xml");
       JdbcTemplate bean = app.getBean(JdbcTemplate.class);
       bean.update("insert into account values(?,?)", "王五", 130);
   }
   ```



### 4.2.3 再高级一点点

c3p0参数分离出去,解耦合

1. jdbc.properties

   ```properties
   jdbc.Driver = com.mysql.cj.jdbc.Driver
   jdbc.Url = jdbc:mysql://127.0.0.1:3306/web01
   jdbc.Username = root
   jdbc.Password = 321321
   ```

2. applicationContext.xml

   ```xml
   <!-- 数据源对象   -->
   <bean id="dataSource" class="com.mchange.v2.c3p0.ComboPooledDataSource">
               <property name="driverClass" value="${jdbc.Driver}"/>
               <property name="jdbcUrl" value="${jdbc.Url}"/>
               <property name="user" value="${jdbc.Username}"/>
               <property name="password" value="${jdbc.Password}"/>
   </bean>
   <!-- jdbc模板对象   -->
   <bean id="jdbcTemplate" class="org.springframework.jdbc.core.JdbcTemplate">
       <property name="dataSource" ref="dataSource"/>
   </bean>
   <context:property-placeholder location="classpath:jdbc.properties" />
   ```

3. test.java

   同上不变



### 4.2.3 很高级

`@RunWith(SpringJUnit4ClassRunner.class)`
 使用了Spring的SpringJUnit4ClassRunner
 以便在测试开始的时候自动创建Spring的应用上下文
 注解了@RunWith就可以直接使用spring容器，直接使用@Test注解，不用启动spring容器

1. jdbc.properties和applicationContext.xml同上

2. test.java

   ```java
   @RunWith(SpringJUnit4ClassRunner.class)
   @ContextConfiguration("classpath:applicationContext.xml")
   public class JdbcTemplateCRUDTest {
       @Autowired
       private JdbcTemplate jdbcTemplate;
       @Test
       public void test1() {jdbcTemplate.update("update account set money = ? where name = ?", 100000, "张三");}
       @Test
       public void test2(){jdbcTemplate.update("delete from account where name = ;", "李四");}
       @Test
       public void test3(){jdbcTemplate.update("insert into account  values (name=? , money=?);", "李四",222);}
       @Test
       // 查询多个
       public void test4(){
           List<Account> query = jdbcTemplate.query("select * from account", new BeanPropertyRowMapper<Account>(Account.class));
           for (Account account : query) {
               System.out.println(account);
           }
       }
           @Test
       // 查询单个
       public void test5(){
                   Account account = jdbcTemplate.queryForObject("select * from account where name =?", new BeanPropertyRowMapper<Account>(Account.class),"李四");
           System.out.println(account);
       }
           @Test
       //查询条数
       public void test6(){
           Integer integer = jdbcTemplate.queryForObject("select count(*) from account", Integer.class);
           System.out.println(integer);
       }
   }
   
   ```

   

# 05_SpringMVC拦截器

SpringMVC拦截器(intercepter)相当于web中的过滤器(filter)用于对处理器进行预处理或后处理

将拦截器按照一定顺序联结成一条链成为拦截器链。在访问被拦截的资源或者方法时,按照设定的顺序通过拦截器链。拦截器也是AOP思想的体现



## 5.1 拦截器快速入门

### 5.1.1自定义拦截器步骤:

1. 创建拦截器实现HandleIntercepter接口
2. 配置拦截器(Spring-mvc.xml)
3. 测试拦截器

### 5.1.2 实现自定义拦截器

+ 创建拦截器类(ctrl o实现方法)

  ``` java
  public class MyInterceptor1 implements HandlerInterceptor {
      // 在目标方法执行前 执行
      @Override
      public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
          System.out.println("目标方法执行前");
          // 放行true 不放行false
          return true;
      }
  
      // 目标方法执行之后 视图返回之前
      @Override
      public void postHandle(HttpServletRequest request, HttpServletResponse response, Object handler, ModelAndView modelAndView) throws Exception {
          System.out.println("目标方法执行后 视图前");
      }
  
      // 整个流程执行完毕之后 执行
      @Override
      public void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler, Exception ex) throws Exception {
          System.out.println("目标方法执行后  视图后");
      }
  }
  ```

+ 配置拦截器(spring-mvc.xml)

  ``` xml
      <!-- 5. 配置连接器  -->
  <mvc:interceptors>
      <mvc:interceptor>
          <!--   对那些资源执行拦截操作     -->
          <mvc:mapping path="/**"/>
          <bean class="com.interceptor.MyInterceptor1" />
      </mvc:interceptor>
  </mvc:interceptors>
  
  ```



## 5.2 拦截器详解

> 其他部分不变 ,主要拦截器内部

1. 拦截器内部编写
2. 多个拦截器只需要在xml中添加即可
   + 顺序需要细细想想即可
   + preHandle: 先配置先执行
   + postHandle: 后配置先执行
   + afterCompletion: 后配置先执行

``` java
public class MyInterceptor1 implements HandlerInterceptor {
    // 在目标方法执行前 执行
    @Override
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
        System.out.println("目标方法执行前");
        String parameter = request.getParameter("check");
        if ("yes".equals(parameter)) {
            return true;
        } else {
            request.getRequestDispatcher("/error.jsp").forward(request,response);
            return false;
        }
    }

    // 目标方法执行之后 视图返回之前
    @Override
    public void postHandle(HttpServletRequest request, HttpServletResponse response, Object handler, ModelAndView modelAndView) throws Exception {
        System.out.println("目标方法执行后 视图前");
        modelAndView.addObject("name", "I've changed the parameters");
    }

    // 整个流程执行完毕之后 执行
    @Override
    public void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler, Exception ex) throws Exception {
        System.out.println("目标方法执行后  视图后");
    }
}
```



# 06_SpringMVC异常处理

## 6.1 异常处理思路

系统中异常分为两大类预期异常和运行时异常RuntimeException,前者捕获异常信息从而获得异常信息，后者主要通过规范代码开发，测试等手段减少运行时异常的发生。

+ 预期异常： 需要try/catche否则编译不通过
+ 运行时异常:  不try/catche可以但是运行的过程中可能会出错



在Spring中Dao,Service,Controller层中依次将异常向上抛出。最后由SpringMVC前端控制器异常处理器进行异常处理

<img src="https://gitee.com/thisisafigurebed/figure-bed/raw/master/servletNode/exception1.jpg">



## 6.2 异常处理的两种方式



1. 使用SpringMVC提供的简单映射异常处理器SimpleMappingExceptionResolver

   + 可以处理一些简单的异常,比如脚标,出现异常跳转哪里等等 
   + 需要配置

2. 实现Spring的异常处理接口HandlerExceptionResolver自定义自己的异常处理器

   

## 6.3 简单映射异常处理器

### 6.3.1 spring-mvc.xml配置

```xml
......
    <!-- 6. 配置简单异常映射器  -->
    <bean class="org.springframework.web.servlet.handler.SimpleMappingExceptionResolver">
        <!-- 配置了视图解析器,自动加前后缀       -->
        <property name="defaultErrorView" value="error"/>
        <!--        <property name="defaultErrorView" value="/error.jsp"/>-->
        <property name="exceptionMappings">
            <map>
                <entry key="java.lang.ClassCastException" value="error1"/>
                <entry key="java.lang.ArithmeticException" value="error2"/>
                <entry key="java.io.FileNotFoundException" value="error3"/>
                <entry key="java.lang.NullPointerException" value="error4"/>
                <entry key="com.exception.MyException" value="error5"/>
            </map>
        </property>
    </bean>
......
```

### 6.3.2 讲解

+ value的值就是跳转到对应的视图
+ 配置视图解析器时就可以不用写路径和后缀
+ 优先找map里的异常
+ 找到了就跳转到map对应异常的视图
+ 没找到就去默认异常对应的视图

### 6.3.3 测试

``` java
// 1. controller
@Controller
public class DemoController {
    @Resource(name = "service")
    private DemoService demoService;
    @RequestMapping("/show1")
    public void show1(){demoService.show1();}
    @RequestMapping("/show2")
    public void show2(){demoService.show2();}
    @RequestMapping("/show3")
    public void show3() throws FileNotFoundException {demoService.show3();}
    @RequestMapping("/show4")
    public void show4(){demoService.show4();}
    @RequestMapping("/show5")
    public void show5() throws MyException {demoService.show5();}
}

// 2. service
@Service(value = "service")
public class DemoService {
    public void show1() {
        System.out.println("Throws a type cast exception");
        Object s = "zhangsan";
        Integer i = (Integer) s;
    }
    public void show2() {
        System.out.println("Throw 0 exception");
        int i = 1 / 0;
    }
    public void show3() throws FileNotFoundException {
        System.out.println("file not found exception");
        FileInputStream fileInputStream = new FileInputStream("C:/dd/dd/d/d.txt");
    }
    public void show4() {
        System.out.println("null pointer exception");
        String str = null;
        str.length();
    }
    public void show5() throws MyException {
        System.out.println("custom exception");
        throw new MyException();
    }
}

```



## 6.4 自定义异常



### 6.4.1 spring-mvc.xml配置

```xml
......
    <!-- 7. 配置自定义异常处理器  -->
    <bean class="com.resolver.MyResolver" />
......
```

### 6.4.2 测试

```java
// 1. controller
@Controller
public class DemoController {
    @Resource(name = "service")
    private DemoService demoService;
    @RequestMapping("/show1")
    public void show1(){demoService.show1();}
    @RequestMapping("/show2")
    public void show2(){demoService.show2();}
    @RequestMapping("/show3")
    public void show3() throws FileNotFoundException {demoService.show3();}
    @RequestMapping("/show4")
    public void show4(){demoService.show4();}
    @RequestMapping("/show5")
    public void show5() throws MyException {demoService.show5();}
}

// 2. service
@Service(value = "service")
public class DemoService {
    public void show1() {
        System.out.println("Throws a type cast exception");
        Object s = "zhangsan";
        Integer i = (Integer) s;
    }
    public void show2() {
        System.out.println("Throw 0 exception");
        int i = 1 / 0;
    }
    public void show3() throws FileNotFoundException {
        System.out.println("file not found exception");
        FileInputStream fileInputStream = new FileInputStream("C:/dd/dd/d/d.txt");
    }
    public void show4() {
        System.out.println("null pointer exception");
        String str = null;
        str.length();
    }
    public void show5() throws MyException {
        System.out.println("custom exception");
        throw new MyException();
    }
}

// 3. 自定义异常处理器
public class MyResolver implements HandlerExceptionResolver {
    @Override
//    Exception参数: 页面报的异常对象
//    返回值: 返回的视图
    public ModelAndView resolveException(HttpServletRequest httpServletRequest, HttpServletResponse httpServletResponse, Object o, Exception e) {
        ModelAndView modelAndView = new ModelAndView();
        if (e instanceof ClassCastException) {
            modelAndView.addObject("info", "Throws a type cast exception");
            modelAndView.setViewName("error");
            return modelAndView;
        } else if (e instanceof ArithmeticException) {
            modelAndView.addObject("info", "Throw 0 exception");
            modelAndView.setViewName("error");
            return modelAndView;
        } else if (e instanceof FileNotFoundException) {
            modelAndView.addObject("info", "file not found exception");
            modelAndView.setViewName("error");
            return modelAndView;
        } else if (e instanceof NullPointerException) {
            modelAndView.addObject("info", "null pointer exception");
            modelAndView.setViewName("error");
            return modelAndView;
        } else if (e instanceof MyException) {
            modelAndView.addObject("info", "custom exception");
            modelAndView.setViewName("error");
            return modelAndView;
        }
        return null;
    }
}
```

### 6.4.3 讲解

好像没什么要讲的,大概看看应该很轻松就能看懂





# 07_Spring AOP

## 7.1 Spring的AOP简介

### 7.1.1 什么是AOP

​		在软件业，AOP为Aspect Oriented Programming的缩写，意为：面向切面编程，通过预编译方式和运行期间动态代理实现程序功能的统一维护的一种技术。

​		AOP是OOP的延续，是软件开发中的一个热点，也是Spring框架中的一个重要内容，是函数式编程的一种衍生范型。利用AOP可以对业务逻辑的各个部分进行隔离，从而使得业务逻辑各部分之间的耦合度降低，提高程序的可重用性，同时提高了开发的效率。



### 7.1.2 AOP的作用 及优势

+ aop的作用：在不修改代码的情况下可以达到进行增强

+ 优势：减少重复代码、提高开发效率、维护方便



### 7.1.3 AOP动态代理技术

常用的动态代理技术

1. JDK代理： 基于接口的动态代理技术

   ``` java
   // 目标接口
   public interface TargetInterface {public void save();}
   
   // 目标方法 原始的方法
   public class Target implements TargetInterface {public void save() {System.out.println("save running.....");}}
   
   // 增强的方法
   public class Advice {
       // 前置增强
       public void before(){System.out.println("before ....");}
       // 后者增强
       public void afterReturn(){System.out.println("after ....");}
   }
   
   // 测试
   public class ProxtTest {
       public static void main(String[] args) {
           // 目标对象
           final Target target = new Target();
           // 增强对象
           final Advice advice = new Advice();
           // 返回值就是动态生成的代理对象
           TargetInterface proxy = (TargetInterface) Proxy.newProxyInstance(
                   target.getClass().getClassLoader(), //参数1 目标对象类加载器
                   target.getClass().getInterfaces(),// 参数2 目标对象相同的接口的字节码对象数组
                   //new InvocationHandler()
                   new InvocationHandler() {
                       @Override
                       // 调用代理对象的方法  实质执行的都是invoke方法
                       public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
                           advice.before();  //前置增强
                           Object invoke = method.invoke(target, args);   // 执行目标方法
                           advice.afterReturn();  //后置增强
                           return invoke;
                       }
                   }
           );
           // 调用代理对象的方法
           proxy.save();
       }
   }
   ```

   

2. cglib代理： 基于父类的动态代理技术

   > Java9开始的JDK支持模块化，如果模块A引用模块B，模块A的module-info.java:
   > moudle A {requires B;}
   > 但模块B并未加入module-info.java达成模块化，或者未开放外部所需部分，那么就会产生“Unable to make {member} accessible: module {模块A或模块A中的一部分} does not 'opens {部分包}' to {模块B}”
   > 例如:报错module java.base does not "opens java.lang" to unnamed module
   >
   > ​		添加VM option: --add-opens java.base/java.lang=ALL-UNNAMED

   ``` java
   
   // 目标方法 原始的方法
   public class Target{public void save() {System.out.println("save running.....");}}
   
   // 增强的方法
   public class Advice {
       // 前置增强
       public void before(){System.out.println("before ....");}
       // 后者增强
       public void afterReturn(){System.out.println("after ....");}
   }
   
   // 测试
   public class ProxtTest {
       public static void main(final String[] args) {
           // 目标对象
           final Target target = new Target();
           // 增强对象
            final Advice advice = new Advice();
           // 返回值就是生成的动态代理对象 基于cglib
           // 1. 创建增强器
           Enhancer enhancer = new Enhancer();
           // 2. 设置父类 (目标)
           enhancer.setSuperclass(Target.class);
           // 3. 设置回调
           enhancer.setCallback(new MethodInterceptor() {
               @Override
               public Object intercept(Object o, Method method, Object[] objects, MethodProxy methodProxy) throws Throwable {
                   advice.before();  //前置增强
                   Object invoke = method.invoke(target, objects);   // 执行目标方法
                   advice.afterReturn();  //后置增强
                   return invoke;
               }
           });
           // 4. 创建代理对象
           Target proxy = (Target) enhancer.create();
           proxy.save();
       }
   }
   ```



### 7.1.4 AOP中的相关概念

+ 目标对象(Target): 代理的目标对象
+ 代理(Proxy): 一个类被AOP组织增强后,就产生一个结果代理类

+ 连接点（Join point）：

  所谓连接点就是指那些被拦截到的点。在Spring AOP 目前只支持方法的调用。也就是说Spring AOP中的连接点就是所有的方法。（可以被增强的(目标)方法就是连接点Spring中）

+ 切入点（pointcut）：

  拦截的方法，连接点拦截后变成切入点。连接点是AOP能够拦截的方法，切入点是指实际拦截的方法。（被增强的(目标)方法就是切入点）

+ 通知/增强（advice）：

  拦截到被增强的(目标)方法然后进行增强的方法

+ 切面（aspect）：

  目标方法+增强就是切面。是切入点和通知的结合

+ 织入（Weaving）：上面的都是名词，织入是动词

  简单讲就是切点和增强结合的过程就是织入。

## 7.2 AOP开发明确事项



### 7.2.1 需要编写的内容

1. 编写核心业务代码(目标类的目标方法)
2. 编写切面类,切面类中有通知(增强功能方法)
3. 在配置文件中,配置织入关系。即那些切点与哪些增强结合

### 7.2.2 AOP技术实现的内容

Spring框架监控切入点方法的执行。一旦监控到切入点方法被运行，使用 代理机制，动态创建目标对象的代理对象，根据通知（增强）类别，在代理对象的对应位置将通知的对应的功能织入，完成完整的代码逻辑运行。



### 7.2.3 AOP底层使用哪种代理方式

+ jdk
+ cglib

Spring框架会判断目标类是否有接口来动态决定使用那种动态代理方式



## 7.3 基于XML的AOP开发

### 7.3.1 开发步骤

1. 导入AOP相关坐标
2. 创建目标接口和目标类(内部有切口)
3. 创建切面类(内部有增强方法)
4. 将目标类和切面类的对象创建权交给Spring
5. 在applicationContext.xml中配置织入关系
6. 测试代码

### 7.3.2 实现

1. 导入坐标

   ``` xml
   ......
   <dependency>
       <groupId>org.springframework</groupId>
       <artifactId>spring-tx</artifactId>
       <version>5.0.5.RELEASE</version>
   </dependency>
   <dependency>
       <groupId>org.aspectj</groupId>
       <artifactId>aspectjweaver</artifactId>
       <version>1.8.4</version>
   </dependency>
   ......
   ```

2. 创建目标接口和目标类(内部有切口)

   ```java
   // 目标方法 原始的方法  接口自己写
   public class Target implements TargetInterface {
       public void save() {
           System.out.println("save running.....");
       }
   }
   ```

3. 创建切面类(内部有增强方法)

   ```java
   public class MyAspect {
       // 前置增强
       public void before() {
           System.out.println("before ....");
       }
       // 后者增强
       public void afterReturn() {
           System.out.println("after ....");
       }
   }
   ```

4. 将目标类和切面类的对象创建权交给Spring和在applicationContext.xml中配置织入关系

   ```xml
   <!-- 5. XML方式实现AOP    -->
       <!--  5.1 目标对象(自己建立的)  -->
       <bean id="target" class="com.aop.Target"></bean>
       <!--  5.2 切面对象(自己建立的)   -->
       <bean id="myAspect" class="com.aop.MyAspect"></bean>
       <!--  5.3 配置织入: 告诉spring框架,那些方法需要进行哪些增强(前置增强,后置增强等等)   -->
       <aop:config>
           <!--  5.3.1声明切面      -->
           <aop:aspect ref="myAspect">
               <!-- 5.3.2切面: 切点+通知           -->
               <!-- pointcut后的是切点表达式           -->
               <aop:before method="before" pointcut="execution(public void com.aop.Target.save())"/>
               <aop:after-returning method="afterReturn" pointcut="execution(public void com.aop.Target.save())" />
           </aop:aspect>
       </aop:config>
   ```

   

### 7.3.3 xml讲解

1. 切点表达式写法: `execution([修饰符] 返回值类型 包名.类名.方法名(参数)) `

   + 修饰符可以省略

   + 返回值类型、包名、类名、方法名可以使用*号代替

   + 包名与类名之间一个点代表包下的类, 两个点代表当前包及其子包下的类

   + 参数列表可以使用两个点..表示任意个数任意类型的参数列表

     ```xml
     <aop:before method="before" pointcut="execution(public void com.aop.Target.save())"/>
     <aop:before method="before" pointcut="execution(public void com.aop.Target.*(..))"/>
     <aop:before method="before" pointcut="execution(*  com.aop.*.*(..))"/>
     <aop:before method="before" pointcut="execution(*  com.aop..*.*(..))"/>
     <aop:before method="before" pointcut="execution(* *..*.*(..))"/>
     ```

2. 通知类型: `<aop:通知类型 method="切面类中方法名" pointcut="切点表达式" />`

   | 名称 | 标签 | 说明 |
   | ---- | ---- | ------ |
   | 前置通知 | <aop:before method="before" pointcut-ref="pc01" /> | 在目标方法执行之前执行执行的通知 |
   | 后置通知 | <aop:after-returning method="afterreturning" pointcut-ref="pc01" returning="msg"/> | 在目标方法执行之后执行的通知 |
   | 环绕通知 | <aop:around method="around" pointcut-ref="pc02"/> | 在目标方法执行之前和之后都可以执行额外代码的通知 |
   | 异常抛出通知 | <aop:after-throwing method="afterthrowing" pointcut-ref="pc01" throwing="e"/> | 在目标方法抛出异常时执行的通知 |
   | 最终通知 | <aop:after method="after" pointcut-ref="pc01" /> | 是在目标方法执行之后执行的通知 |

   3. 测试切面类
   
      ``` java
      public class MyAspect {
          // 前置增强
          public void before() {
              System.out.println("前置增强 ......");
          }
          // 后置增强
          public void afterReturn() {
              System.out.println("后置增强 ......");
          }
          //    环绕增强
          public Object around(ProceedingJoinPoint pjp) throws Throwable {
              System.out.println("环绕1 .....");
              Object proceed = pjp.proceed();
              System.out.println("环绕2 ......");
              return proceed;
          }
          // 抛出异常增强  例如给目标方法写一个i/0 抛出异常增强后其他就不执行了包括环绕2
          public void afterThrowing() {
              System.out.println("抛出异常增强 ......");
          }
          // 最终增强 无论抛不抛异常都执行
          public void after() {
              System.out.println("最终增强 ......");
          }
      }
      ```
   
### 7.3.4 切点表达式的抽取

   在配置织入时切点表达式都是一样的代码的导致重复,可以使用如下:

   ``` xml
   <aop:config>
       <aop:aspect ref="myAspect">
           <aop:pointcut id="myPointcut" expression="execution(public void com.aop.Target.save())"/>       
           <aop:before method="before" pointcut-ref="myPointcut"/>
           <aop:after-returning method="afterReturn" pointcut-ref="myPointcut"/>
           <aop:around method="around" pointcut-ref="myPointcut"/>
           <aop:after-throwing method="afterThrowing" pointcut-ref="myPointcut"/>
           <aop:after method="after" pointcut-ref="myPointcut"/>
       </aop:aspect>
   </aop:config>
   ```

   

## 7.4 基于注解的AOP开发

### 7.4.1 开发步骤

1. 创建目标接口和目标类(内部有切口)
2. 创建切面类(内部有增强方法)
3. 将目标类和切面类的对象创建权交给Spring
4. 在切面类中使用注解配置织入关系
5. 在配置文件中开启组件扫面和AOP的自动代理
6. 测试代码



### 7.4.2 实现

1. 创建目标接口和目标类(内部有切口)

   ```java
   @Component("target")
   // 目标方法 原始的方法 接口自己创建
   public class Target implements TargetInterface {
       public void save() {System.out.println("save running.....");}
   }
   ```

2. 创建切面类(内部有增强方法)

   ```java
   @Component("myAspect")
   public class MyAspect {
       @Before("execution(public void com.anno.Target.save())")
       // 前置增强
       public void before() {
           System.out.println("前置增强 ......");
       }
       @AfterReturning("execution(public void com.anno.Target.save())")
       // 后置增强
       public void afterReturn() {
           System.out.println("后置增强 ......");
       }
       @Around("execution(public void com.anno.Target.save())")
       //    环绕增强
       public Object around(ProceedingJoinPoint pjp) throws Throwable {
           System.out.println("环绕1 .....");
           Object proceed = pjp.proceed();
           System.out.println("环绕2 ......");
           return proceed;
       }
       @AfterThrowing("execution(public void com.anno.Target.save())")
       // 抛出异常增强  例如给目标方法写一个i/0 抛出异常增强后其他就不执行了包括环绕2
       public void afterThrowing() {
           System.out.println("抛出异常增强 ......");
       }
       @After("execution(public void com.anno.Target.save())")
       // 最终增强 无论抛不抛异常都执行
       public void after() {
           System.out.println("最终增强 ......");
       }
   }
   ```

3. 将目标类和切面类的对象创建权交给Spring,在切面类中使用注解配置织入关系,在配置文件中开启组件扫面和AOP的自动代理

   ```xml
   <!--  开启组件扫描  -->
       <context:component-scan base-package="com" />
   <!--  AOP自动代理   -->
       <aop:aspectj-autoproxy />
   ```

4. 测试

   ```java
   @RunWith(SpringJUnit4ClassRunner.class)
   @ContextConfiguration("classpath:applicationContext-anno.xml")
   public class AnnoTest {
       @Autowired
       private TargetInterface target;
       @Test
       public void test1() {
           target.save();
       }
   }
   ```

### 7.4.3 抽取切点表达式

> 其他同理

```java
@After("pointcut")
public void after() {System.out.println("最终增强 ......");}
@AfterThrowing("MyAspect.pointcut")
public void afterThrowing() {System.out.println("抛出异常增强 ......");}
// 定义切点表达式
@Pointcut("execution(public void com.anno.Target.save())")
public void pointcut(){}
```



# 08_事务控制

## 8.1 编程式事务控制相关对象

> 事务三大接口:(前两者必须配置 最后一个选配)
>
> 1. PlatformTransactionManager 平台事务管理器
> 2. TransactionDefinition 事务定义对象: 事务的一些基础信息，如超时时间、隔离级别、传播属性等
> 3. TransactionStatus 事务的一些状态信息，如是否一个新的事务、是否已被标记为回滚

### 8.1.1 PlatformTransactionManager

PlatformTransactionManager 接口是 spring 的事务管理器，它里面提供了我们常用的操作事务的方法如下:

| **方法**                                                     | **说明**           |
| ------------------------------------------------------------ | ------------------ |
| TransactionStatus  getTransaction(TransactionDefination  defination) | 获取事务的状态信息 |
| void  commit(TransactionStatus status)                       | 提交事务           |
| void  rollback(TransactionStatus status)                     | 回滚事务           |

<font color="red" size="5">注意：</font>

PlatformTransactionManager 是接口类型，不同的 Dao 层技术则有不同的实现类，例如：Dao 层技术是jdbc 或 mybatis 时：org.springframework.jdbc.datasource.DataSourceTransactionManager

Dao 层技术是hibernate时：org.springframework.orm.hibernate5.HibernateTransactionManager

### 8.1.2TransactionDefinition

TransactionDefinition 是事务的定义信息对象，里面有如下方法:

| **方法**                      | **说明**           |
| ----------------------------- | ------------------ |
| int  getIsolationLevel()      | 获得事务的隔离级别 |
| int  getPropogationBehavior() | 获得事务的传播行为 |
| int  getTimeout()             | 获得超时时间       |
| boolean  isReadOnly()         | 是否只读           |

#### 8.1.2.1 事务隔离级别

设置隔离级别，可以解决事务并发产生的问题，如脏读、不可重复读和虚读。

+ ISOLATION_DEFAULT
+ ISOLATION_READ_UNCOMMITTED
+ ISOLATION_READ_COMMITTED
+ ISOLATION_REPEATABLE_READ
+ ISOLATION_SERIALIZABLE

#### 8.1.2.2 事务的传播行为

> A调用B时候  主要使用前两个

+ REQUIRED：如果当前没有事务，就新建一个事务，如果已经存在一个事务中，加入到这个事务中。一般的选择（默认值）
+ SUPPORTS：支持当前事务，如果当前没有事务，就以非事务方式执行（没有事务）
+ MANDATORY：使用当前的事务，如果当前没有事务，就抛出异常
+ REQUERS_NEW：新建事务，如果当前在事务中，把当前事务挂起。
+ NOT_SUPPORTED：以非事务方式执行操作，如果当前存在事务，就把当前事务挂起
+ NEVER：以非事务方式运行，如果当前存在事务，抛出异常
+ NESTED：如果当前存在事务，则在嵌套事务内执行。如果当前没有事务，则执行 REQUIRED 类似的操作
+ 超时时间：默认值是-1，没有超时限制。如果有，以秒为单位进行设置
+ 是否只读：建议查询时设置为只读



### 8.1.3TransactionStatus

TransactionStatus 接口提供的是事务具体的运行状态，方法介绍如下:

| **方法**                    | **说明**       |
| --------------------------- | -------------- |
| boolean  hasSavepoint()     | 是否存储回滚点 |
| boolean  isCompleted()      | 事务是否完成   |
| boolean  isNewTransaction() | 是否是新事务   |
| boolean  isRollbackOnly()   | 事务是否回滚   |



## 8.2 基于XML的声明式事务控制

### 8.2.1 声明式业务控制及作用

Spring 的声明式事务顾名思义就是采用声明的方式来处理事务。这里所说的声明，就是指在配置文件中声明，用在 Spring 配置文件中声明式的处理事务来代替代码式的处理事务。

作用:

+ 事务管理不侵入开发的组件。具体来说，业务逻辑对象就不会意识到正在事务管理之中，事实上也应该如此，因为事务管理是属于系统层面的服务，而不是业务逻辑的一部分，如果想要改变事务管理策划的话，也只需要在定义文件中重新配置即可
+ 在不需要事务管理的时候，只要在设定文件上修改一下，即可移去事务管理服务，无需改变代码重新编译，这样维护起来极其方便

<font color="red" size="5">值得一提:</font>Spring 声明式事务控制底层就是AOP。

### 8.2.2 实现原理

```java
public void transfer(String outMoney, String inMoney, double money) {
    accountDao.out(outMoney, money);
    int i = i/0
    accountDao.in(inMoney, money);
}
```

以上service进行转账业务时,执行到i/0时会报错,导致转账出现致命错误 

**大致解决原理**

使用Spring AOP对transfer方法进行增强

其前->开启事务

其后->没有异常提交事务有异常回滚事务

**实现具体原理**

Spring AOP对事务控制有专门的解决 不需要自己写增强方法



### 8.2.3 实现代码

1. dao

   ```java
   public class AccountDaoImpl implements AccountDao {
       private JdbcTemplate jdbcTemplate;
       public void setJdbcTemplate(JdbcTemplate jdbcTemplate) {
           this.jdbcTemplate = jdbcTemplate;
       }
       public void out(String outMan, double money) {
           jdbcTemplate.update("update account set money=money-? where name=?", money, outMan);
       }
       public void in(String inMan, double money) {
           jdbcTemplate.update("update account set money=money+? where name=?", money, inMan);
       }
   }
   ```

2. service

   ```java
   public class AccountServiceImpl implements AccountService {
       private AccountDao accountDao;
       public void setAccountDao(AccountDao accountDao) {this.accountDao = accountDao;}
       public void transfer(String outMoney, String inMoney, double money) {
           accountDao.out(outMoney, money);
           // int i = 1/0;
           accountDao.in(inMoney, money);
       }
   }
   ```

3. controller

   ```java
   public class AccountController {
       public static void main(String[] args) {
           ApplicationContext app = new ClassPathXmlApplicationContext("applicationContext-tx.xml");
           AccountService accountService = app.getBean(AccountService.class);
           accountService.transfer("Tom","Lucy",500);
       }
   }
   ```

4. applicationContext-tx.xml

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns="http://www.springframework.org/schema/beans"
          xmlns:context="http://www.springframework.org/schema/context"
          xmlns:aop="http://www.springframework.org/schema/aop"
          xmlns:tx="http://www.springframework.org/schema/tx"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xsi:schemaLocation="
          http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
          http://www.springframework.org/schema/aop http://www.springframework.org/schema/aop/spring-aop.xsd
          http://www.springframework.org/schema/tx http://www.springframework.org/schema/tx/spring-tx.xsd
          http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd">
       <context:property-placeholder location="classpath:jdbc.properties"/>
       <bean id="dataSource" class="com.mchange.v2.c3p0.ComboPooledDataSource">
           <property name="driverClass" value="${jdbc.Driver}"/>
           <property name="jdbcUrl" value="${jdbc.Url}"/>
           <property name="user" value="${jdbc.Username}"/>
           <property name="password" value="${jdbc.Password}"/>
       </bean>
       <bean id="jdbcTemplate" class="org.springframework.jdbc.core.JdbcTemplate">
           <property name="dataSource" ref="dataSource"/>
       </bean>
       <bean id="accountDao" class="com.tx.dao.impl.AccountDaoImpl">
           <property name="jdbcTemplate" ref="jdbcTemplate"/>
       </bean>
       <!-- 1. 目标对象  内部的方法就是切点   -->
       <bean id="accountService" class="com.tx.service.impl.AccountServiceImpl">
           <property name="accountDao" ref="accountDao"/>
       </bean>
       <!-- 2. 配置平台事务管理器transaction-manager    -->
       <bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
           <property name="dataSource" ref="dataSource"/>
       </bean>
       <!-- 3. 通知  事务的增强  -->
       <tx:advice id="txAdvice" transaction-manager="transactionManager">
           <tx:attributes>
               <tx:method name="*"/>
           </tx:attributes>
       </tx:advice>
       <!-- 4. 配置aop的织入  -->
       <aop:config>
           <aop:advisor advice-ref="txAdvice" pointcut="execution(* com.tx.service.impl.*.*(..))"/>
       </aop:config>
   </beans>
   ```

### 8.2.4 代码讲解

1. 配置平台事务管理器

   没什么说的,照做就好

2. 通知事务的增强

   transaction-manager配置平台事务管理器: 上面配置好的配置平台事务管理器

   tx:attributes设置事务的属性信息

   tx:method name="*":设置需要增强的方法就是目标方法,其后还有很多参数详情见上 8.1.2 不写其他参数就是默认

   ``` xml
   <tx:advice id="txAdvice" transaction-manager="transactionManager">
       <tx:attributes>
           <tx:method name="*"/>
           <tx:method name="transfer" isolation="REPEATABLE_READ" propagation="REQUIRED" read-only="false" timeout="-1"/>
       </tx:attributes>
   </tx:advice>
   ```

3. 配置事务的织入

   pointcut切点:一般就是业务层业务方法

   advice-ref通知引用: 引用上面写好的通知的id

   ``` xml
   <aop:config>
       <aop:advisor advice-ref="txAdvice" pointcut="execution(* com.tx.service.impl.*.*(..))"/>
   </aop:config>
   ```

   

## 8.3 基于注解的声明式事务控制

### 8.3.1 实现代码

1. dao

   ```java
   @Repository("accountDao")
   public class AccountDaoImpl implements AccountDao {
       @Autowired
       private JdbcTemplate jdbcTemplate;
       @Override
       public void out(String outMan, double money) {
           jdbcTemplate.update("update account set money=money-? where name=?", money, outMan);
       }
       @Override
       public void in(String inMan, double money) {
           jdbcTemplate.update("update account set money=money+? where name=?", money, inMan);
       }
   }
   ```

2. service

   ```java
   @Service("accountService")
   //@Transactional()    //isolation = .....
   public class AccountServiceImpl implements AccountService {
       @Autowired
       private AccountDao accountDao;
       @Transactional()    //isolation = .....
       public void transfer(String outMoney, String inMoney, double money) {
           accountDao.out(outMoney, money);
           //int i = 1/0;
           accountDao.in(inMoney, money);
       }
   }
   ```

3. controller

   ```java
   public class AccountController {
       public static void main(String[] args) {
           ApplicationContext app = new ClassPathXmlApplicationContext("applicationContext-txAnno.xml");
           AccountService accountService = app.getBean(AccountService.class);
           accountService.transfer("Tom","Lucy",500);
       }
   }
   ```

4. applicationContext-tx.xml

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns="http://www.springframework.org/schema/beans"
          xmlns:context="http://www.springframework.org/schema/context"
          xmlns:tx="http://www.springframework.org/schema/tx"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xsi:schemaLocation="
          http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
          http://www.springframework.org/schema/tx http://www.springframework.org/schema/tx/spring-tx.xsd
          http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd">
       <context:property-placeholder location="classpath:jdbc.properties"/>
       <bean id="dataSource" class="com.mchange.v2.c3p0.ComboPooledDataSource">
           <property name="driverClass" value="${jdbc.Driver}"/>
           <property name="jdbcUrl" value="${jdbc.Url}"/>
           <property name="user" value="${jdbc.Username}"/>
           <property name="password" value="${jdbc.Password}"/>
       </bean>
       <bean id="jdbcTemplate" class="org.springframework.jdbc.core.JdbcTemplate">
           <property name="dataSource" ref="dataSource"/>
       </bean>
       <context:component-scan base-package="com" />
       <!-- 配置平台事务管理器transaction-manager    -->
       <bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
           <property name="dataSource" ref="dataSource"/>
       </bean>
       <!-- 事务的注解驱动   -->
       <tx:annotation-driven transaction-manager="transactionManager" />
   </beans>
   ```

### 8.3.1 代码讲解

+ 事务通知注解@Transactional():
  + 加在目标类或者方法上(加载类上代表类的所有方法都增强)
  + 可以配置具体的参数
+ xml讲解:
  + 省去dao和service的bean声明和注入
  + 加上注解扫描驱动
  + 配置平台事务管理器transaction-manager还是需要
  + 添加事务的注解驱动



# 09_MyBatis

MyBatis 是一款优秀的持久层框架，它支持自定义 SQL、存储过程以及高级映射。MyBatis 免除了几乎所有的 JDBC 代码以及设置参数和获取结果集的工作。MyBatis 可以通过简单的 XML 或注解来配置和映射原始类型、接口和 Java POJO（Plain Old Java Objects，普通老式 Java 对象）为数据库中的记录。

## 9.1 Mybatis快速入门

### 9.1.1开发步骤

1. 导入坐标
2. 创建User表(数据库表)
3. 创建User实体
4. 编写映射文件UserMapper.xml
5. 编写核心文件SqlMapConfig.xml
6. 编写测试类

### 9.1.2 实现

1. User.java

   ``` java
   public class User {
       private int id;
       private String userName;
       private String password;
       //get set toString
   }
   ```

2. UserMapper.xml

   ```xml
   <?xml version="1.0" encoding="UTF-8" ?>
   <!DOCTYPE mapper  PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
   <mapper namespace="userMapper">
       <select id="findAll" resultType="com.domain.User">
           select * from mybatis1.user
       </select>
   </mapper>
   ```

3. sqlMapperConfig.xml

   ```xml
   <?xml version="1.0" encoding="utf-8"?>
   <!DOCTYPE configuration PUBLIC "-//mybatis.org//DTD Config 3.0//EN" "http://mybatis.org/dtd/mybatis-3-config.dtd">
   <configuration>
   <!-- 数据源环境   -->
       <environments default="development">
           <environment id="development">
               <transactionManager type="JDBC"></transactionManager>
               <dataSource type="POOLED">
                   <property name="driver" value="com.mysql.cj.jdbc.Driver"/>
                   <property name="url" value="jdbc:mysql://127.0.0.1:3306/mybatis1"/>
                   <property name="username" value="root"/>
                   <property name="password" value="321321"/>
               </dataSource>
           </environment>
       </environments>
   <!-- 加载映射文件  -->
       <mappers>
           <mapper resource="com/mapper/UserMapper.xml"/>
       </mappers>
   </configuration>
   ```

4. MybatisTest.java

   ```java
   public class MybatisTest {
       @Test
       public void test1() throws IOException {
       // 获得核心配置文件
       InputStream resourceAsStream = Resources.getResourceAsStream("sqlMapperConfig.xml");
       // 获得SqlSession工厂对象
       SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(resourceAsStream);
       // 获得session会话对象
       SqlSession sqlSession = sqlSessionFactory.openSession();
       // 执行操作  参数:namespace+id
       List<User> userList = sqlSession.selectList("userMapper.findAll");
       System.out.println(userList);
       sqlSession.close();
       }
   }
   ```



### 9.1.3 Mybatis映射文件概述

``` xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper  PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="userMapper">
    <select id="findAll" resultType="com.domain.User">
        select * from mybatis1.user
    </select>
</mapper>
```

### 9.1.4 Mybatis的增删改查

1. 配置文件同上

2. 映射文件

   ```xml
   <?xml version="1.0" encoding="UTF-8" ?>
   <!DOCTYPE mapper  PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
   <mapper namespace="userMapper">
   	<!-- 查询操作   -->
       <select id="findAll" resultType="com.domain.User">
           select * from mybatis1.user
       </select>
       <!-- 增加操作   -->
       <insert id="save" parameterType="com.domain.User">
           insert into mybatis1.user values(#{id},#{username},#{password})
       </insert>
       <!-- 修改操作   -->
       <update id="update" parameterType="com.domain.User">
           update mybatis1.user set username=#{username}, password=#{password} where id=#{id}
   	</update>
       <!-- 删除操作   -->
       <delete id="delete" parameterType="java.lang.Integer">
           delete from mybatis1.user where id=#{id}
       </delete>
   </mapper>
   ```

3. 测试类

   ```java
   public class MybatisTest {
       @Test
       // 查找操作
       public void test1() throws IOException {
           // 获得核心配置文件
           InputStream resourceAsStream = Resources.getResourceAsStream("sqlMapperConfig.xml");
           // 获得SqlSession工厂对象
           SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(resourceAsStream);
           // 获得session会话对象
           SqlSession sqlSession = sqlSessionFactory.openSession();
           // 执行操作  参数:namespace+id
           List<User> userList = sqlSession.selectList("userMapper.findAll");
           System.out.println(userList);
           sqlSession.close();
       }
       @Test
       // 插入操作
       public void test2() throws IOException {
           // 模拟user对象
           User user = new User();
           user.setId(4);
           user.setUsername("赵六");
           user.setPassword("321321");
           // 获得核心配置文件
           InputStream resourceAsStream = Resources.getResourceAsStream("sqlMapperConfig.xml");
           // 获得SqlSession工厂对象
           SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(resourceAsStream);
           // 获得session会话对象
           SqlSession sqlSession = sqlSessionFactory.openSession();
           // 执行操作  参数:namespace+id
           sqlSession.insert("userMapper.save", user);
           // Mybatis通过会话提交事务
           sqlSession.commit();
           sqlSession.close();
       }
       @Test
       // 修改操作
       public void test3() throws IOException {
           // 模拟user对象
           User user = new User();
           user.setId(3);
           user.setUsername("王五");
           user.setPassword("111111");
           // 获得核心配置文件
           InputStream resourceAsStream = Resources.getResourceAsStream("sqlMapperConfig.xml");
           // 获得SqlSession工厂对象
           SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(resourceAsStream);
           // 获得session会话对象
           SqlSession sqlSession = sqlSessionFactory.openSession();
           // 执行操作  参数:namespace+id
           sqlSession.update("userMapper.update", user);
           // Mybatis通过会话提交事务
           sqlSession.commit();
           sqlSession.close();
       }
       @Test
       //删除操作
       public void test4() throws IOException {
           // 获得核心配置文件
           InputStream resourceAsStream = Resources.getResourceAsStream("sqlMapperConfig.xml");
           // 获得SqlSession工厂对象
           SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(resourceAsStream);
           // 获得session会话对象
           SqlSession sqlSession = sqlSessionFactory.openSession();
           // 执行操作  参数:namespace+id
           sqlSession.delete("userMapper.delete", 2);
           // Mybatis通过会话提交事务
           sqlSession.commit();
           sqlSession.close();
       }
   }
   ```



### 9.1.5 Mybatis的核心配置文件概述

#### 9.1.5.1 mybatis核心配置层级关系

> 书写时需要按照如下的顺序

+ configuration配置
+ properties 属性。
+ settings 设置
+ typeAliases类型别名。
+ typeHandlers类型处理器。
+ objectFactory对象工厂。
+ plugins插件
+ environments环境
  + environment 环境变量
    + transactionManager事务管理器
    + datasource数据源
+ databaseldProvider数据库厂商标识
+ mappers映射器

#### 9.1.5.2 常用配置

1. environments标签

   >  配置数据源信息

<img src="https://gitee.com/thisisafigurebed/figure-bed/raw/master/servletNode/mybatis1.jpg">

+ 事务管理器（transactionManager）类型有两种：
  + JDBC：这个配置就是直接使用了JDBC 的提交和回滚设置，它依赖于从数据源得到的连接来管理事务作用域。
  + MANAGED：这个配置几乎没做什么。它从来不提交或回滚一个连接，而是让容器来管理事务的整个生命周期（比如 JEE 应用服务器的上下文）。 默认情况下它会关闭连接，然而一些容器并不希望这样，因此需要将 closeConnection 属性设置为 false 来阻止它默认的关闭行为。

+ 数据源（dataSource）类型有三种：
  + UNPOOLED：这个数据源的实现只是每次被请求时打开和关闭连接。

  + POOLED：这种数据源的实现利用“池”的概念将 JDBC 连接对象组织起来。

  + JNDI：这个数据源的实现是为了能在如 EJB 或应用服务器这类容器中使用，容器可以集中或在外部配置数据源，然后放置一个 JNDI 上下文的引用。

    

2. mapper标签

   >  该标签的作用是加载映射的

   加载方式有如下几种：

+ 使用相对于类路径的资源引用，例如：<mapper resource="org/mybatis/builder/AuthorMapper.xml"/>

+ 使用完全限定资源定位符（URL），例如：<mapper url="file:///var/mappers/AuthorMapper.xml"/>

+ 使用映射器接口实现类的完全限定类名，例如：<mapper class="org.mybatis.builder.AuthorMapper"/>

+ 将包内的映射器接口实现全部注册为映射器，例如：<package name="org.mybatis.builder"/>

  

3. properties标签

   `<properties resource="jdbc.properties" />`

   用于加载外部配置文件



4. typeAliases标签

   >  用于给类型名称设置别称

   类似于linux的alias。

   1. 配置文件

    ```xml
    <!-- 自定义别名    -->
    <typeAliases>
        <typeAlias type="com.domain.User" alias="user" />
    </typeAliases>
    ```

	2. 映射文件

    ```xml
    <select id="findAll" resultType="user">
        select * from mybatis1.user
    </select>
    ```

### 9.1.6 Mybatis的相应API

> 初学必学,老手不用系列



#### 9.1.6.1 SqlSessionFactoryBuilder

**SqlSession工程构建器**

常用API：SqlSessionFactory  build(InputStream inputStream)

通过加载mybatis的核心文件的输入流的形式构建一个SqlSessionFactory对象

``` java
String resource = "org/mybatis/builder/mybatis-config.xml"; 
InputStream inputStream = Resources.getResourceAsStream(resource); 
SqlSessionFactoryBuilder builder = new SqlSessionFactoryBuilder(); 
SqlSessionFactory factory = builder.build(inputStream);
```

其中， Resources 工具类，这个类在 org.apache.ibatis.io 包中。Resources 类帮助你从类路径下、文件系统或一个 web URL 中加载资源文件。

#### 9.1.6.2 SqlSessionFactory

**SqlSession工程对象**

SqlSessionFactory 有多个个方法创建 SqlSession 实例。常用的有如下两个

| **方法**                         | **解释**                                                     |
| -------------------------------- | ------------------------------------------------------------ |
| openSession()                    | 会默认开启一个事务，但事务不会自动提交，也就意味着需要手动提交该事务，更新操作数据才会持久化到数据库中 |
| openSession(boolean  autoCommit) | 参数为是否自动提交，如果设置为true，那么不需要手动提交事务   |

#### 9.1.6.3 SqlSession会话对象

SqlSession 实例在 MyBatis 中是非常强大的一个类。在这里你会看到所有执行语句、提交或回滚事务和获取映射器实例的方法。

执行语句的方法主要有：

```java
<T> T selectOne(String statement, Object parameter) 
<E> List<E> selectList(String statement, Object parameter) 
int insert(String statement, Object parameter) 
int update(String statement, Object parameter) 
int delete(String statement, Object parameter)
```

操作事务的只要方法有:

``` java
void commit()
void rolback()
```



## 9.2 Dao层实现方法

### 9.2.1 代理方式要求

采用 Mybatis 的代理开发方式实现 DAO 层的开发，这种方式是我们后面进入企业的主流。

Mapper 接口开发方法只需要程序员编写Mapper 接口（相当于Dao 接口），由Mybatis 框架根据接口定义创建接口的动态代理对象，代理对象的方法体同上边Dao接口实现类方法。

Mapper 接口开发需要遵循以下规范：

1. Mapper.xml文件中的namespace与mapper接口的全限定名相同
2.  Mapper接口方法名和Mapper.xml中定义的每个statement的id相同
3. Mapper接口方法的输入参数类型和mapper.xml中定义的每个sql的parameterType的类型相同
4. Mapper接口方法的输出参数类型和mapper.xml中定义的每个sql的resultType的类型相同

###  9.2.2 代理方式实现

1. dao层的接口

   ```java
   public interface UserMapper {
       public List<User> findAll();
       public void add(User user);
       public void update(User user);
       public void delete(int id);
   }
   ```

2. 映射文件

   ```xml
   <?xml version="1.0" encoding="UTF-8" ?>
   <!DOCTYPE mapper  PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
   <!-- 命名空间必须和接口的全包名相同   -->
   <mapper namespace="dao.UserMapper">
       <!--  id必须和接口声明的方法相同  返回值类型必须和方法的返回值相同 此处user是别名,原名是User的全包名   -->
       <select id="findAll" resultType="user">
           select * from mybatis1.user
       </select>
       <!--  id必须和接口声明的方法相同  参数类型必须和方法的返回值相同 此处user是别名,原名是User的全包名   -->
       <insert id="add" parameterType="user">
       insert into mybatis1.user values(#{id},#{username},#{password})
       </insert>
       <!--  id必须和接口声明的方法相同  参数类型必须和方法的返回值相同 此处user是别名,原名是User的全包名   -->
       <update id="update" parameterType="user">
           update mybatis1.user set username=#{username},password=#{password} where id=#{id}
       </update>
       <!--  id必须和接口声明的方法相同  参数类型必须和方法的返回值相同 此处int是别名,原名是Integer的全包名   -->
       <delete id="delete" parameterType="int">
           delete from mybatis1.user where id=#{id}
       </delete>
   </mapper>
   ```

3. 配置文件

   ```xml
   <?xml version="1.0" encoding="utf-8"?>
   <!DOCTYPE configuration PUBLIC "-//mybatis.org//DTD Config 3.0//EN" "http://mybatis.org/dtd/mybatis-3-config.dtd">
   <configuration>
       <properties resource="jdbc.properties" />
       <typeAliases>
           <typeAlias type="domain.User" alias="user" />
       </typeAliases>
       <environments default="development">
           <environment id="development">
               <transactionManager type="JDBC" />
               <dataSource type="POOLED">
                   <property name="driver" value="${jdbc.Driver}" />
                   <property name="url" value="${jdbc.Url}"/>
                   <property name="username" value="${jdbc.Username}"/>
                   <property name="password" value="${jdbc.Password}"/>
               </dataSource>
           </environment>
       </environments>
       <!-- 加载映射文件  -->
       <mappers>
           <mapper resource="mapper/UserMapper.xml"/>
       </mappers>
   </configuration>
   ```

4. service的测试

   ```java
   public class UserServiceDemo {
       public static void main(String[] args) throws IOException {
           User user = new User(1,"张三","321321");
           InputStream resourceAsStream = Resources.getResourceAsStream("sqlMapperConfig.xml");
           SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(resourceAsStream);
           SqlSession sqlSession = sqlSessionFactory.openSession(true);
           UserMapper mapper = sqlSession.getMapper(UserMapper.class);
           //查
   //        List<User> userList = mapper.findAll();
   //        System.out.println(userList);
           //增
           mapper.add(user);
       }
   }
   ```



## 9.3 映射文件深入

Mybatis 的映射文件中，前面我们的 SQL 都是比较简单的，有些时候业务逻辑复杂时，我们的 SQL是动态变化的，此时在前面的学习中我们的 SQL 就不能满足要求了。

### 9.3.1 动态sql之if

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper  PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd">

<!-- 命名空间必须和接口的全包名相同   -->
<mapper namespace="com.mapperdeepin.dao.UserMapper">
    ......
    <select id="findByCondition" resultType="user">
        select * from mybatis1.user
        <where>
            <if test="id!=null">
                and id = #{id}
            </if>
            <if test="username!=null">
                and username = #{username}
            </if>
            <if test="password!=null">
                and password = #{password}
            </if>
        </where>
    </select>
    .......
</mapper>
```

这很简单的,相信你一眼就能看明白这是什么意思

### 9.3.2 动态sql之foreach

循环执行sql的拼接操作，例如：SELECT * FROM USER WHERE id IN (1,2,5)。

``` xml
<!--    <select id="findByIds" parameterType="int[]" resultType="user">-->
<select id="findByIds" parameterType="list" resultType="user">
    select * from User
    <where>
        <foreach collection="array" open="id in(" close=")" item="id" separator=",">
            #{id}
        </foreach>
    </where>
</select>
```

这也很简单的,相信你一眼就能看明白这是什么意思



### 9.3.3 sql抽取

对没错,又要抽取解耦合了

Sql 中可将重复的 sql 提取出来，使用时用 include 引用即可，最终达到 sql 重用的目的

``` xml
<!--抽取sql片段简化编写-->
<sql id="selectUser" select * from User</sql>
<select id="findById" parameterType="int" resultType="user">
    <!--代替的是select * from User-->
    <include refid="selectUser"></include> where id=#{id}
</select>
<select id="findByIds" parameterType="list" resultType="user">
    <include refid="selectUser"></include>
    <where>
        <foreach collection="array" open="id in(" close=")" item="id" separator=",">
            #{id}
        </foreach>
    </where>
</select>
```

## 9.4 配置文件深入



### 9.4.1 typeHandlers标签

> 说人话就是sql的数据类型和java的不一定通用,需要进行转化。系统会提供一些基础的如果不够用自己再重写

无论是 MyBatis 在预处理语句（PreparedStatement）中设置一个参数时，还是从结果集中取出一个值时， 都会用类型处理器将获取的值以合适的方式转换成 Java 类型。下表描述了一些默认的类型处理器（截取部分）。
<img src="https://www.yiibai.com/uploads/allimg/140806/1-140P62021045Q.png" >

1. 自定义类转化器步骤:

   1. 定义转换类继承类BaseTypeHandler<T>
   2. 覆盖4个未实现的方法，其中setNonNullParameter为java程序设置数据到数据库的回调方法，getNullableResult为查询时 mysql的字串类型转换成 java的Type类型的方法
   3. 在MyBatis核心配置文件中进行注册
   4. 测试转换是否正确

2. 实现:

   + DateTypeHandler.java

     ```java
     public class DateTypeHandler extends BaseTypeHandler<Date> {
         // java类型  转化成 数据库类型
         public void setNonNullParameter(PreparedStatement preparedStatement, int i, Date date, JdbcType jdbcType) throws SQLException {
             // 生日时间转成毫秒值存到数据库
             long time = date.getTime();
             preparedStatement.setLong(i,time);
         }
         // 数据库类型 转化成 java数据类型
         // String是需要转化列的字段名    ResultSet:查询的结果集
         public Date getNullableResult(ResultSet resultSet, String s) throws SQLException {
             // 将数据库中的long型转化成date型
             long aLong = resultSet.getLong(s);
             Date date = new Date(aLong);
             return date;
         }
         // 数据库类型 转化成 java数据类型
         public Date getNullableResult(ResultSet resultSet, int i) throws SQLException {
             long aLong = resultSet.getLong(i);
             Date date = new Date(aLong);
             return date;
         }
         // 数据库类型 转化成 java数据类型
         public Date getNullableResult(CallableStatement callableStatement, int i) throws SQLException {
             long aLong = callableStatement.getLong(i);
             Date date = new Date(aLong);
             return date;
         }
     }
     ```

   + 配置文件.xml

     ``` xml
     ......
     <typeHandlers>
         <typeHandler handler="com.mapperdeepin.handler.DateTypeHandler" />
     </typeHandlers>
     ......
     ```

   + 测试

     ```java
     public class UserServiceDemo {
     
         public static void main(String[] args) throws IOException {
             InputStream resourceAsStream = Resources.getResourceAsStream("sqlMapperConfig2.xml");
             SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(resourceAsStream);
             SqlSession sqlSession = sqlSessionFactory.openSession(true);
             UserMapper mapper = sqlSession.getMapper(UserMapper.class);
             // 将当前时间转化成毫秒值存入数据库
             User user = new User(1,"张三","321321",new Date());
             mapper.add(user);
             // 将数据库的毫秒值转化成时间输出
             List<User> userList = mapper.findAll(1);
             System.out.println(userList);
         }
     ```

   

### 9.4.2 plugins标签

MyBatis可以使用第三方的插件来对功能进行扩展，分页助手PageHelper是将分页的复杂操作进行封装，使用简单的方式即可获得分页的相关数据:

1. 开发步骤：
   1. 导入通用PageHelper的坐标
   2. 在mybatis核心配置文件中配置PageHelper插件
   3. 测试分页数据获取

2. 实现

    + 导包

    ``` xml
    <!--  mybatis数据库分页助手插件      -->
    <dependency>
    <groupId>com.github.pagehelper</groupId>
    <artifactId>pagehelper</artifactId>
    <version>3.7.5</version>
    </dependency>
    <dependency>
    <groupId>com.github.jsqlparser</groupId>
    <artifactId>jsqlparser</artifactId>
    <version>0.9.1</version>
    </dependency>
    ```

    + 配置文件

    ``` xml
    ......
    <plugins>
    <plugin interceptor="com.github.pagehelper.PageHelper">
    <!--  指定数据库方言      -->
    <property name="dialect" value="mysql"/>
    </plugin>
    </plugins>
    ......
    ```

    + 测试代码

    ```java
    public class UserServiceDemo {
    public static void main(String[] args) throws IOException {
            InputStream resourceAsStream = Resources.getResourceAsStream("sqlMapperConfig2.xml");
            SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(resourceAsStream);
            SqlSession sqlSession = sqlSessionFactory.openSession(true);
            UserMapper mapper = sqlSession.getMapper(UserMapper.class);
            // 设置分页的相关参数
            // 当前页+每页显示的条数
            PageHelper.startPage(2,10);
            List<User> userList = mapper.findAll();
            for (User user1 : userList) {
                System.out.println(user1);
            }
            // 获得分页相关参数
            PageInfo<User> pageInfo = new PageInfo<>(userList);
            System.out.println("当前页:" + pageInfo.getPageNum());
            System.out.println("每页显示条数:" + pageInfo.getPageSize());
            System.out.println("总条数:" + pageInfo.getTotal());
            System.out.println("总页数:" + pageInfo.getPages());
            System.out.println("上一页:" + pageInfo.getLastPage());
            System.out.println("下一页:" + pageInfo.getNextPage());
            System.out.println("是否是第一页:"+pageInfo.isIsFirstPage());
            System.out.println("是否是最后一页:"+pageInfo.isIsLastPage());
            sqlSession.close();
        }
    }
    ```



## 9.5 Mybatis多表操作

### 9.5.1 一对一

因为有两张表参与查询 所以实体类将主表封装在外键表中,重点是配置

1. 映射文件.xml

   ```xml
   <?xml version="1.0" encoding="UTF-8" ?>
   <!DOCTYPE mapper  PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
   <mapper namespace="com.multi.dao.OrderMapper">
   <!--  设置resultMap   -->
       <resultMap id="orderMap" type="order">
   <!--  手动指定实体与属性的映射
           column: 数据表的字段名
           property: 实体的属性名
        -->
           <id  column="oid" property="id" />
           <result column="ordertime" property="ordertime" />
           <result column="total" property="total" />
           <result column="uid" property="user.id" />
           <result column="username" property="user.username" />
           <result column="password" property="user.password" />
           <result column="birthday" property="user.birthday" />
       </resultMap>
   <!--  order实体类中有user类 而字段中没有user字段  所以使用resultMap键值对   -->
       <select id="findAll" resultMap="orderMap">
           select *,o.id oid from mybatis1.orders o,mybatis1.user u where o.id=u.id
       </select>
   </mapper>
   ```

2. 配置文件.xml

   ```xml
   <!-- 加载映射文件  -->
   <mappers>
       <mapper resource="mapper/OrderMulti.xml"/>
   </mappers>
   ```

3. Order.java

   ```java
   public class Order {
       private int id;
       private String ordertime;
       private double total;
       // 表示当前订单属于哪一个用户
       private User user;
   
       // get set toString
   }
   ```

4. 测试

   ```java
   public class UserServiceDemo {
       public static void main(String[] args) throws IOException {
           InputStream resourceAsStream = Resources.getResourceAsStream("sqlMultiConfig.xml");
           SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(resourceAsStream);
           SqlSession sqlSession = sqlSessionFactory.openSession(true);
           OrderMapper orderMapper = sqlSession.getMapper(OrderMapper.class);
           List<Order> orderMapperAll = orderMapper.findAll();
           for (Order order : orderMapperAll) {
               System.out.println(order);
           }
           sqlSession.close();
       }
   }
   ```



### 9.5.2 一对多

将多的一方封装在一的一方,其他类似与一对一

1. User.java

    ``` java
    public class User {
        private int id;
        private String username;
        private String password;
        private Date birthday;
        //描述的是当前用户存在哪些订单
        private List<Order> orderList;

    }
    ```

2. 映射配置

   ``` xml
   <?xml version="1.0" encoding="UTF-8" ?>
   <!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
   <mapper namespace="com.mapper.UserMapper">
       <resultMap id="userMap" type="user">
           <id column="uid" property="id"></id>
           <result column="username" property="username"></result>
           <result column="password" property="password"></result>
           <result column="birthday" property="birthday"></result>
           <!--配置集合信息
               property:集合名称
               ofType：当前集合中的数据类型
           -->
           <collection property="orderList" ofType="order">
               <!--封装order的数据-->
               <id column="oid" property="id"></id>
               <result column="ordertime" property="ordertime"></result>
               <result column="total" property="total"></result>
           </collection>
       </resultMap>
       <select id="findAll" resultMap="userMap">
           SELECT *,o.id oid FROM USER u,orders o WHERE u.id=o.uid
       </select>
   </mapper>
   ```

3. 测试

   ``` java
   public void test2() throws IOException {
       InputStream resourceAsStream = Resources.getResourceAsStream("sqlMultiConfig.xml");
       SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(resourceAsStream);
       SqlSession sqlSession = sqlSessionFactory.openSession();
       UserMapper mapper = sqlSession.getMapper(UserMapper.class);
       List<User> userList = mapper.findAll();
       for (User user : userList) {
           System.out.println(user);
       }
       sqlSession.close();
   }
   ```

   

### 9.5.3 多对多

多对多需要建立一个中间表,用于维护两个表的主键

1. Role.java

   ``` java
   public class Role {
       private int id;
       private String roleName;
       private String roleDesc;
   }
   ```

2. 映射配备

   ``` xml
   <?xml version="1.0" encoding="UTF-8" ?>
   <!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
   <mapper namespace="com.mapper.UserMapper">
       <resultMap id="userRoleMap" type="user">
           <!--user的信息-->
           <id column="userId" property="id"></id>
           <result column="username" property="username"></result>
           <result column="password" property="password"></result>
           <result column="birthday" property="birthday"></result>
           <!--user内部的roleList信息-->
           <collection property="roleList" ofType="role">
               <id column="roleId" property="id"></id>
               <result column="roleName" property="roleName"></result>
               <result column="roleDesc" property="roleDesc"></result>
           </collection>
       </resultMap>
   
       <select id="findUserAndRoleAll" resultMap="userRoleMap">
           SELECT * FROM USER u,sys_user_role ur,sys_role r WHERE u.id=ur.userId AND ur.roleId=r.id
       </select>
   </mapper>
   ```

3. 测试

   ``` java
   public void test3() throws IOException {
       InputStream resourceAsStream = Resources.getResourceAsStream("sqlMultiConfig.xml");
       SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(resourceAsStream);
       SqlSession sqlSession = sqlSessionFactory.openSession();
       UserMapper mapper = sqlSession.getMapper(UserMapper.class);
       List<User> userAndRoleAll = mapper.findUserAndRoleAll();
       for (User user : userAndRoleAll) {
           System.out.println(user);
       }
       sqlSession.close();
   }
   ```

   

## 9.6 注解开发

> 使用注解可以代替映射文件.xml

### 9.6.1 常用CRUD注解

+ @Insert：实现新增
+ @Update：实现更新
+ @Delete：实现删除
+ @Select：实现查询
+ @Result：实现结果集封装
+ @Results：可以与@Result 一起使用，封装多个结果集
+ @One：实现一对一结果集封装
+ @Many：实现一对多结果集封装



### 9.6.2 简单CRUD操作

1. 配置文件.xml

   ```xml
   <?xml version="1.0" encoding="utf-8"?>
   <!DOCTYPE configuration PUBLIC "-//mybatis.org//DTD Config 3.0//EN" "http://mybatis.org/dtd/mybatis-3-config.dtd">
   <configuration>
   <!--  1. 加载属性文件   -->
   <properties resource="jdbc.properties" />
   <!-- 4. 数据库分页    -->
       <plugins>
           <plugin interceptor="com.github.pagehelper.PageHelper">
               <!--  指定数据库方言      -->
               <property name="dialect" value="mysql"/>
           </plugin>
       </plugins>
   <!-- 5. 数据库环境     -->
       <environments default="development">
           <environment id="development">
               <transactionManager type="JDBC" />
               <dataSource type="POOLED">
                   <property name="driver" value="${jdbc.Driver}" />
                   <property name="url" value="${jdbc.Url}"/>
                   <property name="username" value="${jdbc.Username}"/>
                   <property name="password" value="${jdbc.Password}"/>
               </dataSource>
           </environment>
       </environments>
   <!-- 6. 映射关系    -->
       <mappers>
           <package name="com.anno1.dao.UserMapper"/>
       </mappers>
   </configuration>
   ```

2. 接口UserMapper.java

   ```java
   public interface UserMapper {
       @Select("select * from mybatis1.user")
       public List<User> findAll();
       @Insert("insert into mybatis1.user values (#{id}, #{username}, #{password})")
       public void add(User user);
       @Delete("delete from mybatis1.user where id = #{id}")
       public void delete(int id);
       @Update("update mybatis1.user set username=#{username}, password=#{password} where id = #{id}")
       public void update(User user);
       @Select("select * from mybatis1.user where id = #{id}")
       public User findById(int id);
   }
   ```

3. 测试

   ```java
   public class UserMapperTest {
       private UserMapper mapper;
       //每个test运行前执行的代码
       @Before
       public void before() throws IOException {
           InputStream resourceAsStream = Resources.getResourceAsStream("sqlMapConfig.xml");
           SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(resourceAsStream);
           SqlSession sqlSession = sqlSessionFactory.openSession(true);
           mapper = sqlSession.getMapper(UserMapper.class);
       }
       @Test
       public void test1()  {
           List<User> userList = mapper.findAll();
           System.out.println(userList);
       }
       @Test
       public void test2()  {
          mapper.add(new User(6,"张三6","321321"));
       }
       @Test
       public void test3()  {
          mapper.update(new User(6,"张三6","111111"));
       }
       @Test
       public void test4()  {
          mapper.delete(6);
       }
       @Test
       public void test5()  {
           User userList = mapper.findById(3);
           System.out.println(userList);
       }
   }
   ```

   

### 9.6.3 注解实现复杂查询

实现复杂关系映射之前我们可以在映射文件中通过配置`<resultMap>`来实现，使用注解开发后，我们可以使用@Results注解，@Result注解，@One注解，@Many注解组合完成复杂关系的配置

| **注解**          | **说明**                                                     |
| ----------------- | ------------------------------------------------------------ |
| @Results          | 代替的是标签`<resultMap>`该注解中可以使用单个@Result注解，也可以使用@Result集合。使用格式：@Results（{@Result（），@Result（）}）或@Results（@Result（）） |
| @Resut            | 代替了`<id>`标签和`<result>`标签  @Result中属性介绍：  column：数据库的列名  property：需要装配的属性名  one：需要使用的@One  注解（@Result（one=@One）（）））  many：需要使用的@Many  注解（@Result（many=@many）（））） |
| @One  （一对一）  | 代替了`<assocation>`  标签，是多表查询的关键，在注解中用来指定子查询返回单一对象。  @One注解属性介绍：  select:  指定用来多表查询的  sqlmapper  使用格式：@Result(column="  ",property="",one=@One(select="")) |
| @Many  （多对一） | 代替了`<collection>`标签,  是是多表查询的关键，在注解中用来指定子查询返回对象集合。  使用格式：@Result(property="",column="",many=@Many(select="")) |



### 9.6.4 一对一



1. 一条语句联合右链接查询

   ``` java
   public interface OrderMapper {
       // 一条语句联合右链接查询
       @Select("select *,o.id oid from orders o, user u where o.uid = u.id")
       @Results({
           @Result(column = "oid" , property = "id"),
           @Result(column = "ordertime" , property = "ordertime"),
           @Result(column = "total" , property = "total"),
           @Result(column = "uid" , property = "user.id"),
           @Result(column = "username" , property = "user.username"),
           @Result(column = "password" , property = "user.password")
       })
       public List<Order> findAll();
   }
   ```

   

2. 两条语句,通过第一条的结果查询第二条

   ``` java
   public interface OrderMapper {
       //两条语句,通过第一条的结果查询第二条
       @Select("select * from orders")
       @Results({
           @Result(column = "oid" , property = "id"),
           @Result(column = "ordertime" , property = "ordertime"),
           @Result(column = "total" , property = "total"),
           @Result(
               property = "user",  //要封装的属性名
               column = "uid",  // 根据上一条语句查询的结果的uid字段去查询user表
               javaType = User.class,   // 要封装的实体类
               // select属性   近似代表第二个语句
               one = @One(select = "com.anno1.dao.UserMapper.findById")
           )
       })
       public List<Order> findAll1();
   }
   ```

   

### 9.6.5 一对多

1. OrderMapper.java

   ```java
   ......
   @Select("select * from mybatis1.orders where uid = #{uid}")
   public List<Order> findByUid(int uid);
   ......
   ```

1. UserMapper.java

   ```java
   ......
   @Select("select * from user")
   @Results({
           @Result(column = "id",property = "id"),
           @Result(column = "username",property = "username"),
           @Result(column = "password",property = "password"),
           @Result(
                   property = "orderList",  //要封装的属性名
                   column = "id",  // 根据上一条语句查询的结果的id字段去查询order表
                   javaType = List.class,   // 要封装的实体类
                   // select属性   近似代表第二个语句
                   many = @Many(select = "com.anno1.dao.OrderMapper.findByUid")
           )
   })
   public List<User> findUserAndOrderAll();
   ......
   ```

3. 测试

   ```java
   @Test
   public void test6()  {
       List<User> userAndOrderAll = mapper.findUserAndOrderAll();
       for (User user : userAndOrderAll) {
           System.out.println(user);
       }
   ```



### 9.6.6 多对多

1. User.java

   ``` java
   public class User {
       private int id;
       private String username;
       private String password;
       private Date birthday;
       //代表当前用户具备哪些角色
       private List<Role> roleList;
   }
   ```

2. Role.java

   ``` java
   public class Role {
       private int id; private String rolename;
   }
   public class Role {
       private int id; private String rolename;
   }
   ```

3. RoleMapper.java

   ``` java
   public interface RoleMapper {
       @Select("SELECT * FROM sys_user_role ur,sys_role r WHERE ur.roleId=r.id AND ur.userId=#{uid}")
       public List<Role> findByUid(int uid);
   
   }
   ```

4. UserMapper.java

   ``` java
   @Select("SELECT * FROM USER")
   @Results({
       @Result(column = "id",property = "id"),
       @Result(column = "username",property = "username"),
       @Result(column = "password",property = "password"),
       @Result(
           property = "roleList",
           column = "id",
           javaType = List.class,
           many = @Many(select = "com.anno1.dao.RoleMapper.findByUid")
       )
   })
   public List<User> findUserAndRoleAll();
   ```

5. 测试

   ``` java
   @Test
   public void testSave(){
       List<User> userAndRoleAll = mapper.findUserAndRoleAll();
       for (User user : userAndRoleAll) {
           System.out.println(user);
       }
   } 
   ```

   



# 10_SSM整合

>  基础款完整配置

**简单的用户余额查询和添加**

## pom.xml

```xml
<dependencies>
    <!--spring相关-->
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-context</artifactId>
        <version>5.0.5.RELEASE</version>
    </dependency>
    <dependency>
        <groupId>org.aspectj</groupId>
        <artifactId>aspectjweaver</artifactId>
        <version>1.8.7</version>
    </dependency>
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-jdbc</artifactId>
        <version>5.0.5.RELEASE</version>
    </dependency>
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-tx</artifactId>
        <version>5.0.5.RELEASE</version>
    </dependency>
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-test</artifactId>
        <version>5.0.5.RELEASE</version>
    </dependency>
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-webmvc</artifactId>
        <version>5.0.5.RELEASE</version>
    </dependency>
    <!--servlet和jsp-->
    <dependency>
        <groupId>javax.servlet</groupId>
        <artifactId>servlet-api</artifactId>
        <version>2.5</version>
    </dependency>
    <dependency>
        <groupId>javax.servlet.jsp</groupId>
        <artifactId>jsp-api</artifactId>
        <version>2.0</version>
    </dependency>
    <dependency>
        <groupId>jstl</groupId>
        <artifactId>jstl</artifactId>
        <version>1.2</version>
    </dependency>
    <!--mybatis相关-->
    <dependency>
        <groupId>org.mybatis</groupId>
        <artifactId>mybatis</artifactId>
        <version>3.4.5</version>
    </dependency>
    <dependency>
        <groupId>org.mybatis</groupId>
        <artifactId>mybatis-spring</artifactId>
        <version>1.3.1</version>
    </dependency>
    <dependency>
        <!--JDBC      -->
        <groupId>mysql</groupId>
        <artifactId>mysql-connector-java</artifactId>
        <version>8.0.18</version>
    </dependency>
    <dependency>
        <!--C3P0      -->
        <groupId>com.mchange</groupId>
        <artifactId>c3p0</artifactId>
        <version>0.9.5.5</version>
    </dependency>
    <dependency>
        <groupId>c3p0</groupId>
        <artifactId>c3p0</artifactId>
        <version>0.9.1.2</version>
    </dependency>
    <!--  mybatis数据库分页助手插件      -->
    <dependency>
        <groupId>com.github.pagehelper</groupId>
        <artifactId>pagehelper</artifactId>
        <version>3.7.5</version>
    </dependency>
    <dependency>
        <groupId>com.github.jsqlparser</groupId>
        <artifactId>jsqlparser</artifactId>
        <version>0.9.1</version>
    </dependency>
    <!--测试-->
    <dependency>
        <groupId>junit</groupId>
        <artifactId>junit</artifactId>
        <version>4.12</version>
    </dependency>
</dependencies>
```



## web.xml

``` xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xmlns="http://java.sun.com/xml/ns/javaee"
         xsi:schemaLocation="http://java.sun.com/xml/ns/javaee http://java.sun.com/xml/ns/javaee/web-app_2_5.xsd" id="WebApp_ID" version="2.5">
    <!--spring 监听器-->
    <context-param>
        <param-name>contextConfigLocation</param-name>
        <param-value>classpath:applicationContext.xml</param-value>
    </context-param>
    <listener>
        <listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
    </listener>
    <!--springmvc的前端控制器-->
    <servlet>
        <servlet-name>DispatcherServlet</servlet-name>
        <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
        <init-param>
            <param-name>contextConfigLocation</param-name>
            <param-value>classpath:spring-mvc.xml</param-value>
        </init-param>
        <load-on-startup>1</load-on-startup>
    </servlet>
    <servlet-mapping>
        <servlet-name>DispatcherServlet</servlet-name>
        <url-pattern>/</url-pattern>
    </servlet-mapping>
    <!--乱码过滤器-->
    <filter>
        <filter-name>CharacterEncodingFilter</filter-name>
        <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
        <init-param>
            <param-name>encoding</param-name>
            <param-value>UTF-8</param-value>
        </init-param>
    </filter>
    <filter-mapping>
        <filter-name>CharacterEncodingFilter</filter-name>
        <url-pattern>/*</url-pattern>
    </filter-mapping>
</web-app>
```



## jdbc.properties

``` properties
jdbc.driver=com.mysql.jdbc.Driver
jdbc.url=jdbc:mysql://localhost:3306/ssm
jdbc.username=root
jdbc.password=root
```



## log4j.properties

``` properties
#
# Hibernate, Relational Persistence for Idiomatic Java
#
# License: GNU Lesser General Public License (LGPL), version 2.1 or later.
# See the lgpl.txt file in the root directory or <http://www.gnu.org/licenses/lgpl-2.1.html>.
#
### direct log messages to stdout ###
log4j.appender.stdout=org.apache.log4j.ConsoleAppender
log4j.appender.stdout.Target=System.err
log4j.appender.stdout.layout=org.apache.log4j.PatternLayout
log4j.appender.stdout.layout.ConversionPattern=%d{ABSOLUTE} %5p %c{1}:%L - %m%n
### direct messages to file hibernate.log ###
#log4j.appender.file=org.apache.log4j.FileAppender
#log4j.appender.file.File=hibernate.log
#log4j.appender.file.layout=org.apache.log4j.PatternLayout
#log4j.appender.file.layout.ConversionPattern=%d{ABSOLUTE} %5p %c{1}:%L - %m%n
### set log levels - for more verbose logging change 'info' to 'debug' ###
log4j.rootLogger=all, stdout
```



## spring-mvc.xml

``` xml
<?xml version="1.0" encoding="UTF-8" ?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:mvc="http://www.springframework.org/schema/mvc"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
                           http://www.springframework.org/schema/beans/spring-beans.xsd
                           http://www.springframework.org/schema/mvc
                           http://www.springframework.org/schema/mvc/spring-mvc.xsd
                           http://www.springframework.org/schema/context
                           http://www.springframework.org/schema/context/spring-context.xsd">
    <!--组件扫描  主要扫描controller-->
    <context:component-scan base-package="com.controller" />
    <!--配置mvc注解驱动-->
    <mvc:annotation-driven />
    <!--内部资源视图解析器-->
    <bean id="resourceViewResolver" class="org.springframework.web.servlet.view.InternalResourceViewResolver">
        <property name="prefix" value="/WEB-INF/pages/" />
        <property name="suffix" value=".jsp" />
    </bean>
    <!--开发静态资源访问权限-->
    <mvc:default-servlet-handler />
    
 <!----------------------------------------- 不必要-------------------------------------->
     <!-- 5. 配置权限连接器(拦截器)   -->
    <mvc:interceptors>
        <mvc:interceptor>
            <!--   对哪些资源拦截     -->
            <mvc:mapping path="/**"/>
            <!--  对哪些资源放行      -->
            <mvc:exclude-mapping path="/user/login"/>
            <bean class="com.interceptor.PrivilegeInterceptor" />
        </mvc:interceptor>
    </mvc:interceptors>
    <!-- 6. 配置简单异常映射器  -->
    <bean class="org.springframework.web.servlet.handler.SimpleMappingExceptionResolver">
        <!-- 配置了视图解析器,自动加前后缀       -->
        <property name="defaultErrorView" value="error"/>
        <!--        <property name="defaultErrorView" value="/error.jsp"/>-->
        <property name="exceptionMappings">
            <map>
                <entry key="java.lang.ClassCastException" value="error1"/>
                <entry key="java.lang.ArithmeticException" value="error2"/>
                <entry key="java.io.FileNotFoundException" value="error3"/>
                <entry key="java.lang.NullPointerException" value="error4"/>
                <entry key="com.exception.MyException" value="error5"/>
            </map>
        </property>
    </bean>
    <!-- 7. 配置自定义异常处理器  -->
    <bean class="com.resolver.MyResolver" />
</beans>
```



## applicationContext.xml

``` xml
<?xml version="1.0" encoding="UTF-8" ?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:aop="http://www.springframework.org/schema/aop"
       xmlns:tx="http://www.springframework.org/schema/tx"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
                           http://www.springframework.org/schema/beans/spring-beans.xsd
                           http://www.springframework.org/schema/tx
                           http://www.springframework.org/schema/tx/spring-tx.xsd
                           http://www.springframework.org/schema/aop
                           http://www.springframework.org/schema/aop/spring-aop.xsd
                           http://www.springframework.org/schema/context
                           http://www.springframework.org/schema/context/spring-context.xsd">

    <!--组件扫描 扫描service和mapper-->
    <context:component-scan base-package="com">
        <!--排除controller的扫描-->
        <context:exclude-filter type="annotation" expression="org.springframework.stereotype.Controller" />
    </context:component-scan>
    <!--加载propeties文件-->
    <context:property-placeholder location="classpath:jdbc.properties"/>
    <!--配置数据源信息-->
    <bean id="dataSource" class="com.mchange.v2.c3p0.ComboPooledDataSource">
        <property name="driverClass" value="${jdbc.driver}" />
        <property name="jdbcUrl" value="${jdbc.url}"/>
        <property name="user" value="${jdbc.username}"/>
        <property name="password" value="${jdbc.password}"/>
    </bean>
    <!--配置sessionFactory-->
    <bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
        <property name="dataSource" ref="dataSource" />
        <!--加载mybatis核心文件-->
        <property name="configLocation" value="classpath:sqlMapConfig-spring.xml" />
    </bean>
    <!--扫描mapper所在的包 为mapper创建实现类-->
    <bean class="org.mybatis.spring.mapper.MapperScannerConfigurer">
        <property name="basePackage" value="com.mapper" />
    </bean>

    <!--声明式事务控制-->
    <!--平台事务管理器-->
    <bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
        <property name="dataSource" ref="dataSource" />
    </bean>
    <!--配置事务增强-->
    <tx:advice id="txAdvice">
        <tx:attributes>
            <tx:method name="*"/>
        </tx:attributes>
    </tx:advice>
    <!--事务的aop织入-->
    <aop:config>
        <aop:advisor advice-ref="txAdvice" pointcut="execution(* com.service.impl.*.*(..))" />
    </aop:config>
</beans>
```



## sqlMapConfig-spring.xml

``` xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
    <!--定义别名-->
    <typeAliases>
        <!--<typeAlias type="com.domain.Account" alias="account" />-->
        <package name="com.domain"/>
    </typeAliases>
    <!-- 3. 注册类型转化器    -->
    <typeHandlers>
        <typeHandler handler="" />
    </typeHandlers>
    <!-- 4. 数据库分页    -->
    <plugins>
        <plugin interceptor="com.github.pagehelper.PageHelper">
            <!--  指定数据库方言      -->
            <property name="dialect" value="mysql"/>
        </plugin>
    </plugins>
    <!--加载映射-->
    <mappers>
        <!--<mapper resource="com/mapper/AccountMapper.xml" />-->
        <package name="com.mapper"/>
    </mappers>
</configuration>
```



## sqlMapConfig.xml

``` xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
    <!--加载properties文件-->
    <properties resource="jdbc.properties"></properties>
    <!--定义别名-->
    <typeAliases>
        <!--<typeAlias type="com.domain.Account" alias="account"></typeAlias>-->
        <package name="com.domain"></package>
    </typeAliases>
    <!--环境-->
    <environments default="developement">
        <environment id="developement">
            <transactionManager type="JDBC"></transactionManager>
            <dataSource type="POOLED">
                <property name="driver" value="${jdbc.driver}"></property>
                <property name="url" value="${jdbc.url}"></property>
                <property name="username" value="${jdbc.username}"></property>
                <property name="password" value="${jdbc.password}"></property>
            </dataSource>
        </environment>
    </environments>
    <!--加载映射-->
    <mappers>
        <!--<mapper resource="com/mapper/AccountMapper.xml" />-->
        <package name="com.mapper"></package>
    </mappers>
</configuration>
```



## 映射文件AccountMapper.xml

``` xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.mapper.AccountMapper">
    <insert id="save" parameterType="account">
        insert into account values(#{id},#{name},#{money})
    </insert>
    <select id="findAll" resultType="account">
        select * from account
    </select>
</mapper>
```



## domain.Account.java

``` java
public class Account {
    private Integer id;
    private String name;
    private Double money;
}
```



## Service.AccountServiceImpl.java

```java
package com.service.impl;

import com.domain.Account;
import com.mapper.AccountMapper;
import com.service.AccountService;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

import java.util.List;
@Service("accountService")
public class AccountServiceImpl implements AccountService {

    @Autowired
    private AccountMapper accountMapper;

    @Override
    public void save(Account account) {
        accountMapper.save(account);
    }

    @Override
    public List<Account> findAll() {
        return accountMapper.findAll();
    }

// 未整合的 使用sqlMapConfig.xml
//    public void save(Account account) {
//        SqlSession sqlSession = MyBatisUtils.openSession();
//        AccountMapper accountMapper = sqlSession.getMapper(AccountMapper.class);
//        accountMapper.save(account);
//        sqlSession.commit();
//        sqlSession.close();
//    }
//    public List<Account> findAll() {
//        SqlSession sqlSession = MyBatisUtils.openSession();
//        AccountMapper accountMapper = sqlSession.getMapper(AccountMapper.class);
//        return accountMapper.findAll();
//    }
}
```



## Mapper.AccountMapper.java

``` java
public interface AccountMapper {
    @Insert(" insert into account values(#{id},#{name},#{money})")
    void save(Account account);
    @Select({"select * from account"})
    List<Account> findAll();
}
```



## 前端



### /save.jsp

```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>Title</title>
</head>
<body>
    <h1>添加账户信息表单</h1>
    <form name="accountForm" action="${pageContext.request.contextPath}/account/save" method="post">
        账户名称:<input type="text" name="name"><br>
        账户金额:<input type="text" name="money"><br>
        <input type="submit" value="保存"><br>
    </form>
</body>
</html>
```



### page.accountList.jsp

```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
<html>
<head>
    <title>Title</title>
</head>
<body>
    <h1>展示账户数据列表</h1>
    <table border="1">
        <tr>
            <th>账户id</th>
            <th>账户名称</th>
            <th>账户金额</th>
        </tr>
        <c:forEach items="${accountList}" var="account">
            <tr>
                <td>${account.id}</td>
                <td>${account.name}</td>
                <td>${account.money}</td>
            </tr>
        </c:forEach>
    </table>
</body>
</html>
```



# Mybits查漏补缺用

## 配置文件

```xml
<?xml version="1.0" encoding="utf-8"?>
<!DOCTYPE configuration PUBLIC "-//mybatis.org//DTD Config 3.0//EN" "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
<!--  1. 加载属性文件   -->
<properties resource="jdbc.properties" />
<!-- 2. 常用包取别名    -->
    <typeAliases>
        <typeAlias type="com.anno1.domain.User" alias="user" />
        <typeAlias type="" alias="" />
    </typeAliases>
<!-- 3. 注册类型转化器    -->
    <typeHandlers>
        <typeHandler handler="" />
    </typeHandlers>
<!-- 4. 数据库分页    -->
    <plugins>
        <plugin interceptor="com.github.pagehelper.PageHelper">
            <!--  指定数据库方言      -->
            <property name="dialect" value="mysql"/>
        </plugin>
    </plugins>
<!-- 5. 数据库环境     -->
    <environments default="development">
        <environment id="development">
            <transactionManager type="JDBC" />
            <dataSource type="POOLED">
                <property name="driver" value="${jdbc.Driver}" />
                <property name="url" value="${jdbc.Url}"/>
                <property name="username" value="${jdbc.Username}"/>
                <property name="password" value="${jdbc.Password}"/>
            </dataSource>
        </environment>
    </environments>
<!-- 6.1 加载映射文件    -->
<mappers>
    <mapper resource="mapper/UserMapper.xml" />
</mappers>
<!-- 6.2 映射关系    -->
<mappers>
    <package name="com.anno1.dao.UserMapper"/>
</mappers>
</configuration>
```



## 映射文件

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper  PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd">

<!-- 命名空间必须和接口的全包名相同   -->
<mapper namespace="com.mapperdeepin.dao.UserMapper">
    
<!--******单表增删改查*****************************************************************-->
    
    <!--  id必须和接口声明的方法相同  返回值类型必须和方法的返回值相同 此处user是别名,原名是User的全包名   -->
    <select id="findAll" resultType="user">
        select * from mybatis1.user
    </select>

    <select id="findByCondition" resultType="user">
        select * from mybatis1.user
        <where>
            <if test="id!=null">
                and id = #{id}
            </if>
            <if test="username!=null">
                and username = #{username}
            </if>
            <if test="password!=null">
                and password = #{password}
            </if>
        </where>
    </select>
    
    <!--  id必须和接口声明的方法相同  参数类型必须和方法的返回值相同 此处user是别名,原名是User的全包名   -->
    <insert id="add" parameterType="user">
        insert into mybatis1.user values(#{id},#{username},#{password})
    </insert>
    
    <!--  id必须和接口声明的方法相同  参数类型必须和方法的返回值相同 此处user是别名,原名是User的全包名   -->
    <update id="update" parameterType="user">
        update mybatis1.user set username=#{username},password=#{password} where id=#{id}
    </update>
    
    <!--  id必须和接口声明的方法相同  参数类型必须和方法的返回值相同 此处int是别名,原名是Integer的全包名   -->
    <delete id="delete" parameterType="int">
        delete from mybatis1.user where id=#{id}
    </delete>

    <select id="findByIds" parameterType="int[]" resultType="user">
        select * from mybatis1.user
        <where>
            <foreach collection="array" open="id in(" close=")" item="id" separator=",">
                #{id}
            </foreach>
        </where>
    </select>
    
    
<!--******多表 实体类中含有其他类 比如Order类中有User类型的变量*************************************************-->
     <resultMap id="orderMap" type="order">
        <!--手动指定字段与实体属性的映射关系
            column: 数据表的字段名称
            property：实体的属性名称
        -->
        <id column="oid" property="id"></id>
        <result column="ordertime" property="ordertime"></result>
        <result column="total" property="total"></result>
        <!--<result column="uid" property="user.id"></result>
        <result column="username" property="user.username"></result>
        <result column="password" property="user.password"></result>
        <result column="birthday" property="user.birthday"></result>-->

        <!--
            property: 当前实体(order)中的属性名称(private User user)
            javaType: 当前实体(order)中的属性的类型(User)
        -->
        <association property="user" javaType="user">
            <id column="uid" property="id"></id>
            <result column="username" property="username"></result>
            <result column="password" property="password"></result>
            <result column="birthday" property="birthday"></result>
        </association>

    </resultMap>

    <select id="findAll" resultMap="orderMap">
         SELECT *,o.id oid FROM orders o,USER u WHERE o.uid=u.id
    </select>
    
<!--******多表 实体类中含有其他类的泛型 比如User类中有role类型的变量*************************************************-->
    <resultMap id="userRoleMap" type="user">
        <!--user的信息-->
        <id column="userId" property="id"></id>
        <result column="username" property="username"></result>
        <result column="password" property="password"></result>
        <result column="birthday" property="birthday"></result>
        <!--user内部的roleList信息-->
        <collection property="roleList" ofType="role">
            <id column="roleId" property="id"></id>
            <result column="roleName" property="roleName"></result>
            <result column="roleDesc" property="roleDesc"></result>
        </collection>
    </resultMap>

    <select id="findUserAndRoleAll" resultMap="userRoleMap">
        SELECT * FROM USER u,sys_user_role ur,sys_role r WHERE u.id=ur.userId AND ur.roleId=r.id
    </select>
    
</mapper>
```



