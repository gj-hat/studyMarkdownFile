# SpringBoot基础



## 01_SpringBoot概述

### 1.1 SpringBoot是什么

> 简化了Spring开发

众所周知 Spring 应用需要进行大量的配置，各种 XML 配置和注解配置让人眼花缭乱，且极容易出错，因此 Spring 一度被称为“配置地狱”。

为了简化 Spring 应用的搭建和开发过程，Pivotal 团队在 Spring 基础上提供了一套全新的开源的框架，它就是 Spring Boot。

Spring Boot 具有 Spring 一切优秀特性，Spring 能做的事，Spring Boot 都可以做，而且使用更加简单，功能更加丰富，性能更加稳定而健壮。随着近些年来微服务技术的流行，Spring Boot 也成为了时下炙手可热的技术。

Spring Boot 提供了大量开箱即用（out-of-the-box）的依赖模块，例如 spring-boot-starter-redis、spring-boot-starter-data-mongodb 和 spring-boot-starter-data-elasticsearch 等。这些依赖模块为 Spring Boot 应用提供了大量的自动配置，使得 Spring Boot 应用只需要非常少量的配置甚至零配置，便可以运行起来，让开发人员从 Spring 的“配置地狱”中解放出来，有更多的精力专注于业务逻辑的开发。

### 1.2 SpringBoot特点

Spring Boot 具有以下特点：

1. 独立运行的 Spring 项目

   Spring Boot 可以以 jar 包的形式独立运行，Spring Boot 项目只需通过命令“ java–jar xx.jar” 即可运行。

2. 内嵌 Servlet 容器

   Spring Boot 使用嵌入式的 Servlet 容器（例如 Tomcat、Jetty 或者 Undertow 等），应用无需打成 WAR 包 。

3. 提供 starter 简化 Maven 配置

   Spring Boot 提供了一系列的“starter”项目对象模型（POMS）来简化 Maven 配置。

4. 提供了大量的自动配置

   Spring Boot 提供了大量的默认自动配置，来简化项目的开发，开发人员也通过配置文件修改默认配置。

5. 自带应用监控

   Spring Boot 可以对正在运行的项目提供监控。

6. 无代码生成和 xml 配置

   Spring Boot 不需要任何 xml 配置即可实现 Spring 的所有配置。



### 1.3 SpringBoot 功能

1. 自动配置

   Spring Boot的自动配置是一个运行时(更准确地说，是应用程序启动时)的过程，考虑了众多因素，才决定Spring配置应该用哪个，不该用哪个。该过程是SpringBoot自动完成的。

2. 起步依赖

   起步依赖本质上是一个Maven项目对象模型(Project Object Model，POM)，定义了对其他库的传递依赖，这些东西加在一起即支持某项功能。简单的说，起步依赖就是将具备某种功能的坐标打包到一起，并提供一些默认的功能。

3.  辅助功能

   供了一些大型项目中常见的非功能性特性，如嵌入式服务器、安全、指标，健康检测、外部配置等。Spring Boot 并不是对 Spring 功能上的增强，而是提供了一种快速使用 Spring 的方式



## 02_SpringBoot快速入门

### 2.1 要求

1. 搭建SpringBoot项目, 定义一个HelloController.hello()方法, 返回HelloSpringBoot

### 2.2 实现步骤

1. 创建Maven工程
2. 导入SpringBoot 起步依赖
3. 定义Controller
4. 编写引导类
5. 启动测试

### 2.3 代码

1. pom.xml

   ``` xml
   <?xml version="1.0" encoding="UTF-8"?>
   <project xmlns="http://maven.apache.org/POM/4.0.0"
            xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
            xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
       <modelVersion>4.0.0</modelVersion>
       <groupId>org.example</groupId>
       <artifactId>SpringBootStudy</artifactId>
       <version>1.0-SNAPSHOT</version>
       <!--父工程,有这个项目即为springboot项目-->
       <parent>
           <groupId>org.springframework.boot</groupId>
           <artifactId>spring-boot-starter-parent</artifactId>
           <version>2.5.2</version>
           <relativePath/> <!-- lookup parent from repository -->
       </parent>
       <dependencies>
       <!--我们刚才选择的Spring Web依赖-->
       <dependency>
           <groupId>org.springframework.boot</groupId>
           <artifactId>spring-boot-starter-web</artifactId>
       </dependency>
       </dependencies>
   </project>
   ```

2. 编写Controller

   ``` java
   package com.controller;
   import org.springframework.web.bind.annotation.RequestMapping;
   import org.springframework.web.bind.annotation.RestController;
   @RestController
   public class HelloController {
       @RequestMapping("/hello")
       public String hello() {
           return "HelloSpringBoot";
       }
   }
   ```

3. 编写入口方法

   ``` java
   package com;
   import org.springframework.boot.SpringApplication;
   import org.springframework.boot.autoconfigure.SpringBootApplication;
   /*
   * 引导类 SpringBoot项目入口
   *
   * */
   @SpringBootApplication
   public class HelloApplication {
       public static void main(String[] args) {
           SpringApplication.run(HelloApplication.class);
       }
   }
   ```

4. 访问127.0.0.1:8080/hello

### 2.4 小结

1. `spring-boot-starter-parent`定义了版本信息, 默认组合了一套最优的版本搭配

2. 各种`starter`中定义了完成该功能的坐标合集, 其中大部分版本信息来自于父类

3. 继承`parent`引入`starter`后,通过依赖传递,可以获得需要的jar包,且不会产生冲突

4. `spring-boot-starter-web`

   

## 03_配置文件

### 3.1 配置文件分类

> springBoot的配置文件位置在`resource`下, 名必须是`application`文件格式可以是`properties`或`yml`

#### 3.1.1 配置文件概述

SpringBoot是基于约定的. 所以大部分配置都是默认的,  如果要修改可以使用`application.properties`或者`application.yml`进行配置

1. `.properties`

   键值对形式

   ``` properties
   server.port=80
   ```

2. `yml`

   ``` yaml
   server:
     port: 8080
   ```

   

#### 3.1.2 配置文件执行顺序

> 三个配置文件同时配置相同内容时 优先级从高到低

+ properties

+ yml

+ yaml




### 3.2 Yaml

#### 3.2.1 数据格式

+ 对象(map): 键值对数据格式

  ``` yaml
  # 缩进写法
  person:
    name: zhangsan
  # 行内写法
  person: {name: zhangsan}
  ```

+ 数据: 

  ``` yaml
  person:
    - zhangsan
    - lisi
    - wangwu
    
  person: [zhangsan,lisi]
  ```

+ 纯量: 单个的不可分割的值

  ``` yaml
  masg1: 'nihaohah \n  ahahaah'   # 单引号忽略转义字符
  masg2: "woshihahaha \n ahaha"   # 双引号识别转义字符
  ```

  

### 3.3 获取配置文件内容

#### 3.3.1 三种方式

+ @Value		值较少时
+ Environment     值较少时
+ @ConfigurationProperties

#### 3.3.3.2 实现

**yaml文件**

```yaml
person:
  name: zhangsan
  name2: ${name}
  age: 22

name: lisi

msg1: 18

address:
  - beijing
  - shanghai
```



1. @Value

   ``` java
   // Controller下  其实都可以任何地方
   @Value("${msg1}")
   private int msg;
   
   @Value("${person.name}")
   private String personName;
   
   @Value("${person.age}")
   private String age;
   
   @Value("${address[1]}")
   private String address;
   ```

2. Environment

   ``` java
   // Controller下  其实都可以任何地方
   @Autowired
   private Environment env;
   System.out.println(this.env.getProperty("person.age"));
   ```

3. @ConfigurationProperties

   ``` java
   // Domain
   package com.domain;
   import org.springframework.boot.context.properties.ConfigurationProperties;
   import org.springframework.boot.context.properties.EnableConfigurationProperties;
   import org.springframework.stereotype.Component;
   @Component
   @ConfigurationProperties(prefix = "person")
   public class Person {
   
       private String name;
       private String name2
       private int age;
   	// set get
       @Override
       public String toString() {...}
   }
   
   // Controller
       @Autowired
       private Person person;
   ```



### 3.4 profile

在实际的项目开发中，一个项目通常会存在多个环境，例如，开发环境、测试环境和生产环境等。不同环境的配置也不尽相同，例如开发环境使用的是开发数据库，测试环境使用的是测试数据库，而生产环境使用的是线上的正式数据库。

Profile 为在不同环境下使用不同的配置提供了支持，我们可以通过激活、指定参数等方式快速切换环境。

#### 3.4.1 properties配置

在 helloworld 的 src/main/resources 下添加 4 个配置文件：

- application.properties：主配置文件
- application-dev.properties：开发环境配置文件
- application-test.properties：测试环境配置文件
- application.prod-properties：生产环境配置文件



在 applcation.properties 文件中，指定默认服务器端口号为 8080，并通过以下配置激活生产环境（prod）的 profile。

```
#默认端口号
server.port=8080
#激活指定的
profilespring.profiles.active=prod
```


在 application-dev.properties 中，指定开发环境端口号为 8081，配置如下

```
# 开发环境
server.port=8081
```

在 application-test.properties 中，指定测试环境端口号为 8082，配置如下。

```
# 测试环境
server.port=8082
```

在 application-prod.properties 中，指定生产环境端口号为 8083，配置如下。

```
# 生产环境
server.port=8083
```


重启 Spring Boot 主启动程序，控制台输出为 8083



#### 3.4.2 yml配置

与 properties 文件类似，我们也可以添加 4 个配置文件：

- applcation.yml：默认配置
- application-dev.yml：开发环境配置
- application-test.yml：测试环境配置
- application-prod.yml：生产环境配置



在 applcation.yml 文件中指定默认服务端口号为 8080，并通过以下配置来激活开发环境的 profile。

```
#默认配置
server:
  port: 8080
#切换配置
spring:
  profiles:
  active: dev #激活开发环境配置
```

在 application-dev.yml 中指定开发环境端口号为 8081，配置如下。

```
#开发环境server:  port: 8081
```


在 application-test.yml 中指定测试环境端口号为 8082，配置如下。

```
#测试环境server:  port: 8082
```

在 application-prod.yml 中指定生产环境端口号为 8083，配置如下。

```
#生产环境server:  port: 8083
```


重启 Spring Boot 主程序，查看控制台输出 8081



#### 3.4.3 多 Profile 文档块模式

在 YAML 配置文件中，可以使用“---”把配置文件分割成了多个文档块，因此我们可以在不同的文档块中针对不同的环境进行不同的配置，并在第一个文档块内对配置进行切换。

以 helloworld 项目为例，修改 application.yml ，配置多个文档块，并在第一文档快内激活测试环境的 Profile，代码如下。

```yaml
#默认配置
server:
  port: 8080
  
#切换配置
spring:
  profiles:
  active: test
  
---
#开发环境
server:
  port: 8081
spring:
  config:
    activate:
      on-profile: dev
        
---
#测试环境
server:
  port: 8082
spring:
    config:
      activate:
        on-profile: test
        
---
#生产环境
server:
  port: 8083
spring:
  config:
    activate:
      on-profile: prod
```


重启 Spring Boot 主启动程序，查看控制台输出，8082



#### 3.4.4 激活 Profile

除了可以在配置文件中激活指定 Profile，Spring Boot 还为我们提供了另外 2 种激活 Profile 的方式：

- 命令行激活
- 虚拟机参数激活

**命令行激活**

我们可以将 Spring Boot 项目打包成 JAR 文件，并在通过命令行运行时，配置命令行参数，激活指定的 Profile。

我们还以 helloworld 为例，执行以下 mvn 命令将项目打包。

```
mvn clean package
```


项目打包完成，结果如下图。

![Spring Boot 打包](http://c.biancheng.net/uploads/allimg/210726/1329222929-3.png)

打开命令行窗口，跳转到 JAR 文件所在目录，执行以下命令，启动该项目，并激活开发环境（dev）的 Profile。

```
java -jar helloworld-0.0.1-SNAPSHOT.jar  --spring.profiles.active=dev
```


以上命令中，--spring.profiles.active=dev 为激活开发环境（dev）Profile 的命令行参数。

执行结果如下图。



![Spring Boot 命令行激活 Profile](http://c.biancheng.net/uploads/allimg/210726/132922O02-4.png)


**虚拟机参数激活**

我们还可以在 Spring Boot 项目运行时，指定虚拟机参数来激活指定的 Profile。

以 helloworld 为例，将该项目打包成 JAR 文件后，打开命令行窗口跳转到 JAR 所在目录，执行以下命令，激活生产环境（prod）Profile。

```
java -Dspring.profiles.active=prod -jar helloworld-0.0.1-SNAPSHOT.jar
```


以上命令中，-Dspring.profiles.active=prod 为激活生产环境（prod）Profile 的虚拟机参数。

执行结果如下图。

![img](http://c.biancheng.net/uploads/allimg/210726/1329225541-5.png)



### 3.5 内部配置加载顺序

> 注：file: 指当前项目根目录；classpath: 指当前项目的类路径，即 resources 目录。

Spring Boot 项目中可以存在多个 application.properties 或 apllication.yml。

Spring Boot 启动时会扫描以下 5 个位置的 application.properties 或 apllication.yml 文件，并将它们作为 Spring boot 的默认配置文件。

1. file:./config/
2. file:./config/*/
3. file:./
4. classpath:/config/
5. classpath:/

以上所有位置的配置文件都会被加载，且它们优先级依次降低，序号越小优先级越高。其次，位于相同位置的 application.properties 的优先级高于 application.yml。

所有位置的文件都会被加载，高优先级配置会覆盖低优先级配置，形成互补配置，即：

- 存在相同的配置内容时，高优先级的内容会覆盖低优先级的内容；
- 存在不同的配置内容时，高优先级和低优先级的配置内容取并集。



### 3.6 外部配置加载顺序

1. 命令行参数

   + 所有的配置都可以在命令行上进行指定，多个配置用空格分开； --配置项=值

    ```
    java -jar spring-boot-0.0.1-SNAPSHOT.jar --server.port=8081 --server.context-path=/abc
    ```
   
   + 也可以使用   命令`--spring.config.location=path`来指定外部的配置文件
   
2. 来自java:comp/env的JNDI属性

3. Java系统属性（System.getProperties()）

4. 操作系统环境变量

5. RandomValuePropertySource配置的random.*属性值

6. jar包外部的application-{profile}.properties或application.yml(带spring.profile)配置文件

7. jar包内部的application-{profile}.properties或application.yml(带spring.profile)配置文件



## 04_SpringBoot整合其他框架



### 4.1 整合Junit

#### 4.1.1 实现步骤

1. 搭建springBoot工程
2. 引入starter-test起步依赖
3. 编写测试类
4. 添加测试相关注解
   + @RunWith(SpringRunner.class)
   + @SpringBootTest(classes = 启动类.class)
5. 编写测试方法



#### 4.1.2 实现代码

1. 加入test的依赖

   ``` xml
   <dependency>
       <groupId>org.springframework.boot</groupId>
       <artifactId>spring-boot-starter-test</artifactId>
   </dependency>
   ```

2. 编写被测试类(一般测试的都是service    main目录下  不是test目录下)

   ``` java
   // Person类之前已经写过  也加过ConfigurationProperties注解了
   package com.service;
   @Service
   public class UserService {
       @Autowired
       private Person person;
       public void userService() {
           System.out.println("person = " + person);
       }
   }
   ```

3. 在test目录下编写测试类

   ``` java
   @RunWith(SpringRunner.class)
   @SpringBootTest(classes = HelloApplication.class)
   public class UserServiceTest {
       @Autowired
       private UserService userService;
       @Test
       public void userServiceTest(){
           userService.userService();
       }
   }
   ```

   

### 4.2 整合redis

#### 4.2.1 实现步骤

1. 搭建springBoot工程
2. 引入redis起步依赖
3. 配置redis相关属性
4. 注入RedisTemplate模板
5. 编写测试方法
   

#### 4.2.2 实现代码

1. 加入起步依赖

   ``` xml
   <dependency>
       <groupId>org.springframework.boot</groupId>
       <artifactId>spring-boot-starter-data-redis</artifactId>
   </dependency>
   ```

2. 配置属性  本机的话 大概率不用管

3. 测试方法+RedisTemplate注入

   ``` java
   @RunWith(SpringRunner.class)
   @SpringBootTest(classes = HelloApplication.class)
   public class UserServiceTest {
       @Autowired
       private RedisTemplate redisTemplate;
       @Test
       public void testSet(){
           // 存入数据
           redisTemplate.boundValueOps("name").set("zhangsan");
   
       }
       @Test
       public void testGet(){
           // 获取数据
           Object name = redisTemplate.boundValueOps("name").get();
           System.out.println("name = " + name);
       }
   }
   ```

4. 配置Redis  备用  在resource/application.yml

   ``` yaml
   ......
   spring:
       redis:
         host: 127.0.0.1   # 主机地址
         port:  6379
   ......
   ```

   

### 4.3 整合myBatis

#### 4.3.1 实现步骤

1. 搭建springBoot工程
2. 引入Mybatis起步依赖, 添加Mysql驱动
3. 编写DataSource和MyBatis相关配置
4. 定义表和实体类
5. 编写dao和mapper文件/纯注解开发
6. 测试



#### 4.3.2 实现代码

1. 引入Mybatis起步依赖, 添加Mysql驱动

   ``` xml
   <dependency>
       <groupId>org.mybatis.spring.boot</groupId>
       <artifactId>mybatis-spring-boot-starter</artifactId>
       <version>2.2.0</version>
   </dependency>
   
   <dependency>
       <groupId>mysql</groupId>
       <artifactId>mysql-connector-java</artifactId>
       <scope>runtime</scope>
   </dependency>
   ```

2. 创建数据库

   ``` sql
   create table user
   (
       id       int       null,
       username char(255) null,
       password char(255) null
   );
   ```

3. 配置DataSource

   ``` yaml
   # dataSource
   spring:
     datasource:
       url: jdbc:mysql:///springbootstudy
       username: root
       password: 321321
       driver-class-name: com.mysql.cj.jdbc.Driver
   ```

   

4. 创建实体类

    ``` java
   package com.domain;
   public class User {
       private int id;
       private String username;
       private String password;
   
   	// set get
       @Override
       public String toString() {..}
   }
   ```

5. Mapper类 

   1. 纯注解开发

       ``` java
       package com.mapper;
       @Mapper
       @Repository
       public interface UserMapper {
           @Select("select * from user")
           public List<User> findAll();
       }
       ```
       
   2. xml配置开发

      1. 编写Mapper类

         ``` java
         package com.mapper;
         @Mapper
         @Repository
         public interface UserMapper {
             public List<User> findAll();
         }
         ```

      2. 新建mapper源文件  resource/mapper/UserMapper.xml

         ``` xml
         <?xml version="1.0" encoding="UTF-8"?>
         <!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
                 "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
         <mapper namespace="com.mapper.UserMapper">
             <select id="findAll" resultType="user">
                 select * from springbootstudy.user
             </select>
         </mapper>
         ```

      3. application.yml配置MyBatis

         ``` yaml
         # dataSource
         spring:
           datasource:
             url: jdbc:mysql:///springbootstudy
             username: root
             password: 321321
             driver-class-name: com.mysql.cj.jdbc.Driver
         
         mybatis:
           mapper-locations: classpath:mapper/*Mapper.xml    # Mapper的映射文件路径
           type-aliases-package: com.domain     #  别名
         #  config-location:     指定MyBatis的核心配置文件
         ```

         

6. 编写测试类

   ``` java
   @RunWith(SpringRunner.class)
   @SpringBootTest(classes = HelloApplication.class)
   public class UserServiceTest {
       @Autowired
       private UserMapper userMapper;
       @Test
       public void testFindAll(){
           List<User> users = userMapper.findAll();
           for (User u : users) {
               System.out.println(u);
           }
       }
   }
   ```

   



# SpringBoot高级



## 01_SpringBoot原理分析

### 1.1 自动配置

#### 1.1.1 @Conditional  条件判断注解

> 根据条件创建Bean   难点: Spring是怎么帮我们创建好Bean的   问题: 我们如何控制spring在我们想的时候帮我们创建Bean

1. 原理讲解

   1. 查看@Conditional注解源码

      ``` java
      public @interface Conditional {
          Class<? extends Condition>[] value();
      }
      ```

      得出1.该注解是一个接口2.需要一个Condition类型的对象

   2. 进一步查看Condition的源码

      ``` java
      public interface Condition {
          boolean matches(ConditionContext var1, AnnotatedTypeMetadata var2);
      }
      ```

   3. 得出需要实现`Condition`接口 并返回一个布尔类型用来判断`1`中的`@Bean`是否执行

2. 实现   需求:  当Jedis坐标导入时候   加载该Bean   否则不加载

   1. 编写Bean

      ``` java
      public class User{}
      ```

   2. 编写spring的配置类(使用@Configure注解的类),并使用@Bean将方法标记为Bean

      ```
      @Configuration
      public class UserConfig {
          @Bean
          @Conditional(ClassCondition.class)
          public User user(){
              return new User();
          }
      }
      ```

   3. 编写类实现Condition接口 (测试用例是: 判断Jedis坐标是否被导入)   并将类文件写入`2`中注解中

      ``` java
      package com.condition;
      public class ClassCondition implements Condition {
          @Override
          public boolean matches(ConditionContext conditionContext, AnnotatedTypeMetadata annotatedTypeMetadata) {
              return true/false;
          }
      }
      ```

   4. 在启动类中进行测试   显示成功或者失败(取决于`3`)

      ```java
      @SpringBootApplication
      public class HelloApplication {
          public static void main(String[] args) {
              ConfigurableApplicationContext context = SpringApplication.run(HelloApplication.class);
      
              Object user = context.getBean("user");
              System.out.println(user);
          }
      }
      ```

3. 实现2  需求: 通过注解属性值value 指定坐标后创建Bean

   1. 自定义注解

      ``` java
      package com.condition;
      
      import org.springframework.context.annotation.Conditional;
      import java.lang.annotation.*;
      
      @Target({ElementType.TYPE, ElementType.METHOD})
      @Retention(RetentionPolicy.RUNTIME)
      @Documented
      @Conditional(ClassCondition.class)
      public @interface ConditionOnClass {
          String[] value();
      }
      
      ```

   2. 实现注解

      ``` java
      package com.condition;
      
      import org.springframework.context.annotation.Condition;
      import org.springframework.context.annotation.ConditionContext;
      import org.springframework.core.type.AnnotatedTypeMetadata;
      import java.util.Map;
      
      public class ClassCondition implements Condition {
          /**
           * @param conditionContext      上下文对象, 用于获取环境  IOC容器  ClassLoader对象
           * @param annotatedTypeMetadata 注解的元对象, 可以获取注解定义的属性值
           * @return
           */
          @Override
          public boolean matches(ConditionContext conditionContext, AnnotatedTypeMetadata annotatedTypeMetadata) {
      //        1.需求2 通过注解属性值value 指定坐标后创建Bean
              // 获取注解属性值  value
              Map<String, Object> map = annotatedTypeMetadata.getAnnotationAttributes(ConditionOnClass.class.getName());
              String[] value = (String[]) map.get("value");
              boolean check = true;
              try {
                  for (String className : value) {
                      Class<?> aClass = Class.forName(className);
                  }
              } catch (ClassNotFoundException e) {
                  check = false;
              }
              return check;
          }
      }
      ```

   3. 测试注解

      ``` java
      package com.config;
      import com.condition.ClassCondition;
      import com.condition.ConditionOnClass;
      import com.domain.User;
      import org.springframework.context.annotation.Bean;
      import org.springframework.context.annotation.Conditional;
      import org.springframework.context.annotation.Configuration;
      @Configuration
      public class UserConfig {
          @Bean
          // 此处填入指定的坐标  就可以通过ConditionOnClass判断 是否被加载
          @ConditionOnClass("redis.clients.jedis.Jedis")
          public User user(){
              return new User();
          }
      }
      ```

   4. **小结**

      + 自定义条件： 
        + 定义条件类：自定义类实现Condition接口，重写 matches 方法，在 matches 方法中进行逻辑判断，返回 boolean值 。 matches 方法两个参数： 
          + context：上下文对象，可以获取属性值，获取类加载器，获取BeanFactory等。 
          +  metadata：元数据对象，用于获取注解属性。 
        + 判断条件： 在初始化Bean时，使用 @Conditional(条件类.class)注解

      + SpringBoot 提供的常用条件注解： 
        + ConditionalOnProperty：判断配置文件中是否有对应属性和值才初始化Bean 
        + ConditionalOnClass：判断环境中是否有对应字节码文件才初始化Bean 
        + ConditionalOnMissingBean：判断环境中没有对应Bean才初始化Bean

####  1.1.2 切换内置Web服务器

SpringBoot的web环境中默认使用tomcat作为内置服务器，其实SpringBoot提供了4中内置服务器供我们选择，我们可 以很方便的进行切换。

内置了四个web服务器:

+ TomCat (默认)\
+ Jetty
+ Netty
+ Undertow

如何切换(步骤 pom.xml):

1. 排除tomcat依赖

   ``` xml
   <dependency>
       <groupId>org.springframework.boot</groupId>
       <artifactId>spring-boot-starter-web</artifactId>
       <exclusions>
           <exclusion>
               <artifactId>spring-boot-starter-tomcat</artifactId>
               <groupId>org.springframework.boot</groupId>
           </exclusion>
       </exclusions>
   </dependency>
   ```

2. 加入新依赖

   ``` xml
   <dependency>
       <groupId>org.springframework.boot</groupId>
       <artifactId>spring-boot-starter-jetty</artifactId>
   </dependency>
   ```

#### 1.1.3 @Enable系列注解

SpringBoot中提供了很多Enable开头的注解，这些注解都是用于动态启用某些功能的。而其底层原理是使用@Import注 解导入一些配置类，实现Bean的动态加载。

#### 1.1.4 @Import注解

Enable*底层依赖于@Import注解导入一些类，使用@Import导入的类会被Spring加载到IOC容器中。而@Import提供4中用 法：

1. 导入Bean
2. 导入配置类     值得一提:  如果使用这种方式 那么配置类中@Configuration注解可以省略
3. 导入 ImportSelector 实现类。一般用于加载配置文件中的类
4. 导入 ImportBeanDefinitionRegistrar 实现类。

示例:

1. 导入Bean

   ``` java
   @SpringBootApplication
   @Import(User.class)
   public class HelloApplication {
       public static void main(String[] args) {
           ConfigurableApplicationContext context = SpringApplication.run(HelloApplication.class);
           Object user1 = context.getBean("user");
           System.out.println(user1);
       }
   }
   ```

2. 导入配置类     值得一提:  如果使用这种方式 那么配置类中@Configuration注解可以省略

   ``` java
   @SpringBootApplication
   @Import(UserConfig.class)
   public class HelloApplication {
       public static void main(String[] args) {
           ConfigurableApplicationContext context = SpringApplication.run(HelloApplication.class);
           Map<String, User> userMap = context.getBeansOfType(User.class);
           System.out.println("userMap = " + userMap);
           // 假设配置类标记了两个Bean  userMap就会有两个Bean
       }
   }
   ```

3. 导入 ImportSelector 实现类。一般用于加载配置文件中的类

   ``` java
   // 1. 实现ImportSelector接口
   package com.config;
   import org.springframework.context.annotation.ImportSelector;
   import org.springframework.core.type.AnnotationMetadata;
   public class MyImportSelector implements ImportSelector {
       @Override
       public String[] selectImports(AnnotationMetadata annotationMetadata) {
           return new String[]{"com.domain.Person","com.domain.User"};
       }
   }
   
   // 2. 导入实现类
   @SpringBootApplication
   @Import(MyImportSelector.class)
   public class HelloApplication {
       public static void main(String[] args) {
           ConfigurableApplicationContext context = SpringApplication.run(HelloApplication.class);
           System.out.println("------------------");
           Object user1 = context.getBean("user");
           System.out.println(user1);
           Object person = context.getBean("person");
           System.out.println(person);
       }
   }
   ```

4. 导入 ImportBeanDefinitionRegistrar 实现类。

   ``` java
   // 1. ImportBeanDefinitionRegistrar 
   public class MyImportBeanDefinitionRegistrar implements ImportBeanDefinitionRegistrar {
       @Override
       public void registerBeanDefinitions(AnnotationMetadata importingClassMetadata, BeanDefinitionRegistry registry) {
           AbstractBeanDefinition beanDefinition = BeanDefinitionBuilder.rootBeanDefinition(User.class).getBeanDefinition();
           registry.registerBeanDefinition("user",beanDefinition);
           AbstractBeanDefinition beanDefinition1 = BeanDefinitionBuilder.rootBeanDefinition(Person.class).getBeanDefinition();
           registry.registerBeanDefinition("person",beanDefinition1);
       }
   }
   
   // 2. 同上
   @SpringBootApplication
   @Import({MyImportBeanDefinitionRegistrar.class})
   public class HelloApplication {
       public static void main(String[] args) {
           ConfigurableApplicationContext context = SpringApplication.run(HelloApplication.class);
           Object user1 = context.getBean("user");
           System.out.println(user1);
   
           Object person = context.getBean("person");
           System.out.println(person);
       }
   }
   ```

#### 1.1.5 @EnableAutoConfiguration 注解

+ @EnableAutoConfiguration 注解内部使用 @Import(AutoConfigurationImportSelector.class)来加载配置类.
+ 配置文件位置：META-INF/spring.factories，该配置文件中定义了大量的配置类，当 SpringBoot 应用启动时，会自动加载 这些配置类，初始化Bean
+ 并不是所有的Bean都会被初始化，在配置类中使用Condition来加载满足条件的Bean



#### 1.1.6 实践

**需求**: 自定义redis-starter, 要求当导入redis坐标时候,SpringBoot自动创建Jedis的Bean

**实现步骤**:

1. 创建`redis-spring-boot-autoconfigure`模块
2. 创建`redis-spring-boot-starter`模块依赖`redis-spring-boot-autoconfigure`

3. 在`redis-spring-boot-autoconfigure`模块初始化`Jedis`的`Bean`,并定义`META-INF/spring.factories`文件

4. 在测试模块中引入自定义的`redis-starter`依赖测试获取`Jedis`的`Bean`,操作`redis`

**实现:**

1. 创建上述两个模块

2. 修改上述两个模块的`pom.xml`

   ``` xml
   <!--redis-spring-boot-starter    依赖里只留下下面两个  bulding也不需要-->
   <dependencies>
       <dependency>
           <groupId>org.springframework.boot</groupId>
           <artifactId>spring-boot-starter</artifactId>
       </dependency>
       <!--  引入autoconfigure      -->
       <dependency>
           <groupId>com.gjstudy</groupId>
           <artifactId>demo1-redis-spring-boot-autoconfigure</artifactId>
           <version>0.0.1-SNAPSHOT</version>
       </dependency>
   </dependencies>
   
   <!--redis-spring-boot-autoconfigure    依赖里只留下下面两个  bulding也不需要-->
   <dependencies>
       <dependency>
           <groupId>org.springframework.boot</groupId>
           <artifactId>spring-boot-starter</artifactId>
       </dependency>
       <!--引入Jedis依赖-->
       <dependency>
           <groupId>redis.clients</groupId>
           <artifactId>jedis</artifactId>
       </dependency>
   </dependencies>
   ```

3. 添加相关Bean

   ``` java
   // redis-spring-boot-autoconfigure   两个类 一个配置类 一个实体类用来指定host和port
   package com.gjstudy.demo1redisspringbootautoconfigure;
   @Configuration
   @EnableConfigurationProperties(RedisProperties.class)
   public class RedisAutoConfiguration {
       /**
        * 提供Jedis的Bean即可
        */
       @Bean
       public Jedis jedis(RedisProperties redisProperties) {
           return new Jedis(redisProperties.getHost(), redisProperties.getPort());
       }
   }
   
   package com.gjstudy.demo1redisspringbootautoconfigure;
   @ConfigurationProperties(prefix = "redis")
   public class RedisProperties {
   
       private String host = "localhost";
       private int port = 6379;
   	// setter  getter
   }
   ```

4. 配置Bean

   ``` properties
   # 新建resource/META-INF/spring.factories
   org.springframework.boot.autoconfigure.EnableAutoConfiguration =\
     com.gjstudy.demo1redisspringbootautoconfigure.RedisAutoConfiguration
   ```

   

5. 开始测试  在测试工程pom中导入starter

   ``` xml
   <dependency>
       <groupId>com.gjstudy</groupId>
       <artifactId>demo1-redis-spring-boot-starter</artifactId>
       <version>0.0.1-SNAPSHOT</version>
   </dependency>
   ```

6. 测试工程中 查看能不能获取到`Jedis`

   ``` java
   package com;
   @SpringBootApplication
   public class HelloApplication {
       public static void main(String[] args) {
           ConfigurableApplicationContext context = SpringApplication.run(HelloApplication.class);
           Jedis jedis = context.getBean(Jedis.class);
           System.out.println("jedis = " + jedis);
           //1. 先启动Redis
           jedis.set("name", "zhangsan");
           String name = jedis.get("name");
           System.out.println(name);
       }
   }
   ```

7. 配置一下redis(在autoConfig模块中)   系统中没有Jedis时才加载自定义的

   ``` java
   package com.gjstudy.demo1redisspringbootautoconfigure;
   @Configuration
   // 保证Jedis在的时候加载
   @ConditionalOnClass(Jedis.class)
   @EnableConfigurationProperties(RedisProperties.class)
   public class RedisAutoConfiguration {
       @Bean
       // 如果系统中没有Jedis 那么使用下面的自定义的
       @ConditionalOnMissingBean(name = "jedis")
       public Jedis jedis(RedisProperties redisProperties) {
           System.out.println("自定义的Jedis");
           return new Jedis(redisProperties.getHost(), redisProperties.getPort());
       }
   }
   
   // 测试一下 主模块中新建一个Jedis的Bean
   package com;
   @SpringBootApplication
   @Import({MyImportBeanDefinitionRegistrar.class})
   public class HelloApplication {
       public static void main(String[] args) {
           ConfigurableApplicationContext context = SpringApplication.run(HelloApplication.class);
           Jedis jedis = context.getBean(Jedis.class);
           System.out.println("jedis = " + jedis);
       }
       @Bean
       public Jedis jedis(){
           System.out.println("11111");
           return new Jedis("localhots",6379);
       }
   }
   ```

   

### 1.2 监听机制

SpringBoot 在项目启动时，会对几个监听器进行回调，我们可以实现这些监听器接口，在项目启动时完成 一些操作。 

+ ApplicationContextInitializer   
  + 在项目启动之前就加载(IOC容器之前就加载)
  + 需要配置才能启动   新建`META-INF/spring.factories`在里面配置一句话  具体自己百度
+ SpringApplicationRunListener
  + 项目生命周期相关
+ CommandLineRunner:   项目启动时候加载 有参(启动参数)              可以在项目启动时候执行一些预操作(比如Redis)
+ ApplicationRunner: 项目启动时候加载(作用同上 只是参数不同)

**实践:**

1. 自己编写一个类 实现以上意思相应的接口
2. 上面3 4使用@Component标注即可
3. 上面1 2 由于是在IOC之前加载 所以需要在`META-INF/spring.factories`中关联一下

### 1.3 启动流程分析

![](https://gitee.com/thisisafigurebed/figure-bed/raw/master/SpringBoot/SpringBoot%E5%90%AF%E5%8A%A8%E6%B5%81%E7%A8%8B.png)







## 02_SpringBoot监控



SpringBoot自带监控功能Actuator，可以帮助实现对程序内部运行情况监控，比如监控状况、Bean加载情况、配置属性 、日志信息等。

### 2.1 使用:

1. 导入坐标

   ``` xml
   <dependency>
   <groupId>org.springframework.boot</groupId>
   <artifactId>spring-boot-starter-actuator</artifactId>
   </dependency>
   ```

2. 访问localhost:8080/acruator

### 2.2 SpringBoot 监控使用:

| /beans          | 描述应用程序上下文里全部的Bean，以及它们的关系               |
| --------------- | ------------------------------------------------------------ |
| /env            | 获取全部环境属性                                             |
| /env/{name}     | 根据名称获取特定的环境属性值                                 |
| /health         | 报告应用程序的健康指标，这些值由HealthIndicator的实现类提供  |
| /info           | 获取应用程序的定制信息，这些信息由info打头的属性提供         |
| /mappings       | 描述全部的URI路径，以及它们和控制器(包含Actuator端点)的映射关系 |
| /metrics        | 报告各种应用程序度量信息，比如内存用量和HTTP请求计数         |
| /metrics/{name} | 报告指定名称的应用程序度量值                                 |
| /trace          | 提供基本的HTTP请求跟踪信息(时间戳、HTTP头等)                 |



### 2.3 SpringBoot 监控 - Spring Boot Admin

+ Spring Boot Admin是一个开源社区项目，用于管理和监控SpringBoot应用程序。
+ Spring Boot Admin 有两个角色，客户端(Client)和服务端(Server)。
+ 应用程序作为Spring Boot Admin Client向为Spring Boot Admin Server注册
+ Spring Boot Admin Server 的UI界面将Spring Boot Admin Client的Actuator Endpoint上的一些监控信息。





### 2.4 SpringBoot 监控 - Spring Boot Admin

**使用步骤:**

+ admin-server： 
  1. 创建`admin-server`模块
  2. 导入依赖坐标`admin-starter-server`
  3. 在引导类上启用`@EnableAdminServer`
+ admin-client：
  1. 创建`admin-client`模块
  2. 导入依赖坐标`admin-starter-client`
  3. 配置相关信息: server地址等等
  4. 启动server, 和 client服务.访问server







## 03_SpringBoot项目部署









































