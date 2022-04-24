# 微服务



## 01_认识微服务

**架构分类**

1. 单体架构: 将业务功能集中在一个项目中开发, 打成一个包部署
2. 分布式架构: 根据业务功能对系统进行拆分, 每个业务模块作为独立项目开发, 称为一个服务

**分布式架构遇到的问题:**

+ 服务拆分粒度如何
+ 服务集群地址如何维护
+ 服务之间如何实现远程调用
+ 服务健康状态如何

为了解决上述问题,目前市面上流行的是微服务架构.

**微服务**

微服务架构特征:

+ 单一职责: 微服务粒度更小, 每一个服务都对应唯一的业务能力, 做到单一职责, 避免重复业务开发
+ 面向服务: 微服务对外暴露业务接口
+ 自治: 团队独立, 技术独立, 数据独立, 部署独立



**微服务架构**

微服务这种方案需要技术框架进行落地实施, 目前最知名的微服务架构是: SpringCloud和阿里巴巴的Dubbo

 

**技术栈:**

技术栈分为两大类, 大类中的小类可以通用互换

1. SpringCloud+Feign         SpringCloudAlibaba+Feign
   1. SpringCloud+Feign
      + 使用SpringCloud技术栈
      + 服务接口采用Resful风格
      + 服务调用采用Feign方式
   2. SpringCloudAlibaba+Feign
      + 使用SpringCloudAlibaba技术栈
      + 服务接口采用Resful风格
      + 服务调用采用Feign方式
2. SpringCloudAlibaba+Dubbo       Dubb原始模式
   1. SpringCloudAlibaba+Dubbo
      + 使用SpringCloudAlibaba技术栈
      + 服务接口采用Dubbo协议标准
      + 服务调用采用Dubbo方式
   2.  Dubb原始模式
      + 使用Dubbo老旧技术体系
      + 服务接口采用Dubbo协议标准
      + 服务调用采用Dubbo方式



## 02_SpringCloud简介



SpringCloud集成了各种微服务功能组件, 并基于SpringBoot实现了这些组件的自动装配, 从而提供了良好的开箱即用的体验

例如:

+ 服务注册发现 Eureka, Nacos, Consul
+ 服务远程调用  OpenFeign, Dubbo
+ 服务链路监控  Zipkin Sleuth
+ 统一配置管理  SpringCloudConfig,  Nacos
+ 统一网关路由  SpringCloudGateway, Zuul
+ 流控,降级,保护    Hystix, Sentinel

**SpringBoot和SpringCloud版本匹配:**

![image-20211222200503302](https://tva1.sinaimg.cn/large/008i3skNly1gxmv6sul1ej31p40qymzo.jpg)



### 2.1 服务拆分及远程调用



#### 2.1.1 拆分注意事项

1. 不同微服务,不要重复开发相同业务
2. 微服务数据独立, 不要访问其他微服务的数据库



#### 2.1.2 调用

使用SpringBoot提供的RestTemplate接口的方法使用  直接注入就好

**提供者与消费者:**

+ 服务提供者: 一次业务中, 被其他微服务调用的服务   (提供接口给其他微服务)
+ 服务消费者: 一次业务员中, 调用其他微服务的服务  (调用其他微服务提供的接口)



## 03_Eureka注册中心

 

### 3.1 远程调用问题

+ 服务消费者如何获得服务提供者的地址信息呢
+ 如果服务提供者是一个集群,  地址该如何维护获取
+ 消费者如何获取服务提供者的健康状态



### 3.2 eureka原理

Eureka分为两个部分:

1. eurka-server: 服务端
2. eurka-client: 客户端(包括服务的提供者和服务的消费者)

流程:

1. 每一个服务的提供者上线时候都会发消息给注册中心, 注册中心记录下服务提供者的接口名,ip地址,端口消息
2. 服务的消费者如果需要获取服务接口会请求注册中心, 注册中心将所有提供这个接口的服务提供者的地址返回给消费者
3. 服务的消费者获取到多个服务提供者的地址, 消费者根据一定的规则去选择服务提供者(由消费者去做负载均衡)
4. 消费者选择合适的提供者进行远程调用
5. 服务的提供者需要定时向注册中心发送消息(心跳)用于判断服务提供者的健康信息

![image-20211222232955150](https://tva1.sinaimg.cn/large/008i3skNly1gxn13x4knmj31gz0u00x4.jpg)



### 3.3 搭建EurekaServer



#### 3.3.1 实现步骤

1. 搭建注册中心

2. 服务注册

   将userInfo和Account注册到eurka

3. 服务发现

   在Account中完成服务拉取,然后通过负载均衡挑选一个服务, 实现远程调用



#### 3.3.2 实现(注册中心模块  注意版本匹配)

1. pom.xml

   ``` xml
   <?xml version="1.0" encoding="UTF-8"?>
   <project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
            xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
       <modelVersion>4.0.0</modelVersion>
       <parent>
           <groupId>org.springframework.boot</groupId>
           <artifactId>spring-boot-starter-parent</artifactId>
           <version>2.3.9.RELEASE</version>
           <relativePath/> <!-- lookup parent from repository -->
       </parent>
       <groupId>com.example</groupId>
       <artifactId>EurekaServer</artifactId>
       <version>0.0.1-SNAPSHOT</version>
       <name>EurekaServer</name>
       <description>Demo project for Spring Boot</description>
       <properties>
           <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
           <project.reporting.outputEncoding>UTF-8</project.reporting.outputEncoding>
           <java.version>1.8</java.version>
   
       </properties>
       <dependencies>
   
           <dependency>
               <groupId>org.springframework.boot</groupId>
               <artifactId>spring-boot-starter-web</artifactId>
           </dependency>
           
           <dependency>
               <groupId>org.springframework.cloud</groupId>
               <artifactId>spring-cloud-starter-netflix-eureka-server</artifactId>
           </dependency>
   
   
           <dependency>
               <groupId>org.springframework.boot</groupId>
               <artifactId>spring-boot-starter-test</artifactId>
               <scope>test</scope>
           </dependency>
           <dependency>
               <groupId>org.springframework.boot</groupId>
               <artifactId>spring-boot-autoconfigure</artifactId>
           </dependency>
           <dependency>
               <groupId>org.springframework.boot</groupId>
               <artifactId>spring-boot</artifactId>
           </dependency>
       </dependencies>
   
   
       <dependencyManagement>
           <dependencies>
               <dependency>
                   <groupId>org.springframework.cloud</groupId>
                   <artifactId>spring-cloud-dependencies</artifactId>
                   <version>Hoxton.SR10</version>
                   <type>pom</type>
                   <scope>import</scope>
               </dependency>
           </dependencies>
       </dependencyManagement>
   
       <build>
           <plugins>
               <plugin>
                   <groupId>org.springframework.boot</groupId>
                   <artifactId>spring-boot-maven-plugin</artifactId>
               </plugin>
           </plugins>
       </build>
   
   </project>
   
   ```

2. application.yml

   ``` yaml
   server:
     port: 10001
   spring:
     application:
       name: eurkaserver         # 微服务的名称
   
   
   
   eureka:
     client:         # eureka本身也是一个服务 配置eureka的地址
       service-url:
         defaultZone: http://127.0.0.1:10001/eureka
   
   ```

3. 启动类

   ``` java
   // 开启EurekaServer
   @EnableEurekaServer
   @SpringBootApplication
   public class EurekaServerApplication {
       public static void main(String[] args) {
           SpringApplication.run(EurekaServerApplication.class, args);
       }
   }
   ```

   







### 3.4 服务注册



每一个需要被注册的服务都需要

1. pom.xml

   ``` xml
   <?xml version="1.0" encoding="UTF-8"?>
   <project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
            xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
       <modelVersion>4.0.0</modelVersion>
       <parent>
           <groupId>org.springframework.boot</groupId>
           <artifactId>spring-boot-starter-parent</artifactId>
           <version>2.3.9.RELEASE</version>
           <relativePath/> <!-- lookup parent from repository -->
       </parent>
       <groupId>com.example</groupId>
       <artifactId>UserInfoManage</artifactId>
       <version>0.0.1-SNAPSHOT</version>
       <name>UserInfoManage</name>
       <description>Demo project for Spring Boot</description>
       <properties>
           <java.version>1.8</java.version>
       </properties>
       <dependencies>
           <dependency>
               <groupId>org.springframework.boot</groupId>
               <artifactId>spring-boot-starter-web</artifactId>
           </dependency>
   
   
           <dependency>
               <groupId>com.alibaba</groupId>
               <artifactId>fastjson</artifactId>
               <version>1.2.47</version>
           </dependency>
   
           <dependency>
               <groupId>org.springframework.boot</groupId>
               <artifactId>spring-boot-starter-test</artifactId>
               <scope>test</scope>
           </dependency>
   
           <dependency>
               <groupId>org.springframework.cloud</groupId>
               <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
           </dependency>
   
       </dependencies>
   
       <dependencyManagement>
           <dependencies>
               <dependency>
                   <groupId>org.springframework.cloud</groupId>
                   <artifactId>spring-cloud-dependencies</artifactId>
                   <version>Hoxton.SR10</version>
                   <type>pom</type>
                   <scope>import</scope>
               </dependency>
           </dependencies>
       </dependencyManagement>
   
   
   
       <build>
           <plugins>
               <plugin>
                   <groupId>org.springframework.boot</groupId>
                   <artifactId>spring-boot-maven-plugin</artifactId>
               </plugin>
           </plugins>
       </build>
   
   </project>
   
   ```

2. Application.yml

   ``` yaml
   server:
   	port: 8080
   spring:
     application:
       name: userInfoService         # 微服务的名称
   
   eureka:
     client:         # eureka本身也是一个服务 配置eureka的地址
       service-url:
         defaultZone: http://127.0.0.1:10001/eureka
   
   ```

   

#### 3.4.3 一个服务启动多次

![image-20211223145703821](https://tva1.sinaimg.cn/large/008i3skNly1gxnrwlewn3j31zq0rctet.jpg)



### 3.5 服务发现



1. 消费者的controller

   ``` java
   @RestController
   public class UserAccount {
       @Autowired
       private RestTemplate restTemplate;
       @RequestMapping("/account")
       public JSONObject test(){
           // 字符串后可以拼接 参数
           String url = "http://userInfoService/user";
           JSONObject forObject = restTemplate.getForObject(url, JSONObject.class);
           return forObject;
       }
   }
   ```

2. RestTemplate的负载均衡

   ``` java
   @Configuration
   public class RestTemplateCon {
       @Bean
       @LoadBalanced
       public RestTemplate test(){
           return new RestTemplate();
       }
   }
   ```

3. 浏览器访问127.0.0.1:8080/account就可以了   







## 04_Ribbon负载均衡

**原理图**

![image-20211223155437312](https://tva1.sinaimg.cn/large/008i3skNly1gxntkjp5goj31uv0u077d.jpg)



**代码流程图**

![image-20211223160507481](https://tva1.sinaimg.cn/large/008i3skNly1gxntvf7wd9j31lu0u0tcq.jpg)



**负载均衡策略**

![image-20211223161008033](https://tva1.sinaimg.cn/large/008i3skNly1gxnu0mcaplj31ki0u0tg4.jpg)



**继承图**

![image-20211223161602493](https://tva1.sinaimg.cn/large/008i3skNly1gxnu6rc45lj31r60ogq6y.jpg)





#### 4.1 修改负载均衡规则



1. 配置类的形式  全局配置 无论后续调用哪一个微服务都采用这种策略

   ``` java
   @Configuration
   pulic class conf{
     @Bean
     public IRule randomRule(){
       return new RandomRule();
     }
   }
   ```

2. 配置文件形式  

   ``` yaml
   userService:
   	ribbon:
   		NFLoadBanlancerRuleClassName: com.netflix.loadbalancer.RandomRule   # 负载均衡规则 
   ```

   

### 4.2 饥饿加载

Ribbon默认采用懒加载, 即第一次访问时才会去创建LoadBalanceClient, 请求时间会很长. 而饥饿加载则会在项目启动时候创建, 降低第一次访问的耗时, 通过如下配置开启:

``` yaml
ribbon:
	eager-load:
		enable: true     # 开启饥饿加载
    clients:    # 对指定的服务进行饥饿加载
    	- userService
    	- xxService
    	- xxxService
```





## 05_Nacos注册中心

Nacos是阿里巴巴的产品, 也是SpringCloud的一个组件. 相比Eureka功能更加丰富, 在国内很受欢迎



### 5.1 安装

1. 下载

```
https://github.com/alibaba/nacos/releases
```

2. 打开

   ```
   sh ${nacos}/bin/startup.sh  -m standalone
   ```

3. 浏览器打开   用户名密码 nacos

   ``` 
   http://127.0.0.1:8848/nacos/
   ```



### 5.2 服务注册

1. pom.xml

   ``` xml
   <?xml version="1.0" encoding="UTF-8"?>
   <project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
            xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
       <modelVersion>4.0.0</modelVersion>
       <parent>
           <groupId>org.springframework.boot</groupId>
           <artifactId>spring-boot-starter-parent</artifactId>
           <version>2.3.9.RELEASE</version>
           <relativePath/> <!-- lookup parent from repository -->
       </parent>
       <groupId>com.example</groupId>
       <artifactId>UserAccount</artifactId>
       <version>0.0.1-SNAPSHOT</version>
       <name>UserAccount</name>
       <description>Demo project for Spring Boot</description>
       <properties>
           <java.version>1.8</java.version>
       </properties>
       <dependencies>
           <dependency>
               <groupId>org.springframework.boot</groupId>
               <artifactId>spring-boot-starter-web</artifactId>
           </dependency>
           <dependency>
               <groupId>com.alibaba</groupId>
               <artifactId>fastjson</artifactId>
               <version>1.2.47</version>
           </dependency>
           <dependency>
               <groupId>org.springframework.boot</groupId>
               <artifactId>spring-boot-starter-test</artifactId>
               <scope>test</scope>
           </dependency>
   
   
           <dependency>
               <groupId>com.alibaba.cloud</groupId>
               <artifactId>spring-cloud-starter-alibaba-nacos-discovery</artifactId>
           </dependency>
       </dependencies>
       <dependencyManagement>
           <dependencies>
               <dependency>
                   <groupId>com.alibaba.cloud</groupId>
                   <artifactId>spring-cloud-alibaba-dependencies</artifactId>
                   <version>2.2.5.RELEASE</version>
                   <type>pom</type>
                   <scope>import</scope>
               </dependency>
           </dependencies>
       </dependencyManagement>
       <build>
           <plugins>
               <plugin>
                   <groupId>org.springframework.boot</groupId>
                   <artifactId>spring-boot-maven-plugin</artifactId>
               </plugin>
           </plugins>
       </build>
   
   </project>
   
   ```

2. Application.yaml

   ``` yaml
   spring:
     application:
       name: userAccountService         # 微服务的名称
     cloud:
       nacos:
         server-addr: localhost:8848
   ```

3. 启动项目



### 5.3 nacos服务分级存储模型

引入集群的概念, 一般情况下访问本地局域网的服务,  只有本地服务不可用的时候才会访问异地服务

![image-20211223195224540](https://tva1.sinaimg.cn/large/008i3skNly1gxo0fy332rj31kc0u077t.jpg)



### 5.4 配置集群

只需要在yml中增加集群名即可

``` yaml
spring:
  application:
    name: userAccountService         # 微服务的名称
  cloud:
    nacos:
      server-addr: localhost:8848
      discovery:
        cluster-name: XIAN   # 集群名称
```



### 5.5 负载均衡规则配置

1. 根据集群策略

  目的: 同一集群的优先访问   同一集群找不到的时候会找其他集群 但是会报警告

  ``` yaml
  userService:       # 服务名
    ribbon:      # 负载均衡规则
      NFLoadBanlancerRuleClassName: com.alibaba.cloud.nacos.ribbon.NacosRule   # 优先本地,本地内随机 
  ```

2. 根据权重负载均衡

   服务设备的性能有差异,我们希望性能好的设备访问的次数更多一些, Nacos提供了权重配置来控制访问频率,权重越大则访问的频率越高

3. 操作

   在网页服务中进行配置    数值在0~1之间     0则不会被访问



### 5.6 环境隔离 - namespace

> 不同环境下的服务不可以访问

目的:  例如开发环境, 生产环境, 测试环境进行隔离

业务相关度比较高的服务可以放在一个group中 

 ![image-20211223204448355](https://tva1.sinaimg.cn/large/008i3skNly1gxo1yf0hxqj31ji0u0dj9.jpg)



使用步骤:

1. 网页上 新建命名空间  id可以自动生成或者手动

2. 在代码中配置服务注册到新命名空间内

   ``` yaml
   spring:
     application:
       name: userAccountService         # 微服务的名称
     cloud:
       nacos:
         server-addr: localhost:8848
         discovery:
           cluster-name: XIAN   # 集群名称
           namespace: uuid        # 命名空间
           ephemeral: false   # 是否是临时实例
   ```

   

### 5.6 总结

1. 服务分为临时实例和非临时实例
   + 临时实例 每30秒发送一次心跳给注册中心 , 超时则会被注册中心移除
   + 非临时实例 不用向注册中心发送心跳, 注册中心每30秒给服务发送一次心跳确认是否健康.  如果不健康则辨识为不健康,但是不会移除服务, 等待服务上线
2. 消费者拉取一次服务时, 会将服务缓存到本地(存活时间为30秒) ,  在此期间 如果服务下线,则注册中心会主动发送信息给消费者通知服务链路不可达

3. Eureka和Nacos区别和联系

   ![image-20211223214347421](https://tva1.sinaimg.cn/large/008i3skNly1gxo3nrnxd9j316o0s878m.jpg)





## 06_Nacos配置管理



### 6.1 统一配置管理

**需求:**

+ 如果服务的配置需要修改,通常需要重启服务
+ 多个服务的配置通常相同,  代码重复

**解决:**

+ 服务主动去Nacos中拉取配置
+ 使用Nacos的统一配置管理, 当配置发生变化时候,主动通知各个服务
+ 配置的热更新, 不需要重启就可以更新

**流程图:**

![image-20211224120903410](https://tva1.sinaimg.cn/large/008i3skNly1gxoso33dtmj311e0qggnh.jpg)





#### 6.1.1 新建配置步骤:

新建配置(在网页中)

1. 配置管理点击`+`

2. 填写`DataId`:  配置文件名称 

   命名规范:  服务名称-环境名称(test,dev).yaml

3. `Group`一般默认不许改动

4. 描述:  describe 

5. 填写配置文件内容      把一些经常更换有热更新需求的配置填写到nacos中  

   例如: 模板  开关类配置



#### 6.1.2 获取配置步骤

**流程图:**

![image-20211224125358885](https://tva1.sinaimg.cn/large/008i3skNly1gxotytc1vdj31y00tiwhd.jpg)

**实现步骤:**

1. 导入依赖

   ``` xml
   <dependency>
     <groupId>com.alibaba.cloud</groupId>
     <artifactId>spring-cloud-starter-alibaba-nacos-config</artifactId>
   </dependency>
   ```

2. 新建`bootstrap.yml`,这个文件是引导文件, 优先级高于application.yaml

   ``` yaml
   spring:
     application:
       name: userAccountservice      # 服务名
     profiles:
       active: dev         # 环境
     cloud:
       nacos:
         server-addr: localhost:8848     # nacos地址
         config:
           file-extension: yaml        # 后缀
   ```


#### 6.1.3 注意注意注意

1. 版本匹配

   springCloud  springBoot SpringCloud-Alibaba  NacosServer

2. 匹配规则

   `${prefix}-${spring.profile.active}.${file-extension}`

   prefix 默认为 spring.application.name 的值，也可以通过配置项 spring.cloud.nacos.config.prefix来配置。
       spring.profile.active 即为当前环境对应的 profile，详情可以参考 Spring Boot文档。 注意：当 spring.profile.active 为空时，对应的连接符 - 也将不存在，dataId 的拼接格式变成 ${prefix}.${file-extension}
       file-exetension 为配置内容的数据格式，可以通过配置项 spring.cloud.nacos.config.file-extension 来配置。目前只支持 properties 和 yaml 类型。

3. 不可以重复配置 

   比如application中配置过得 bootStrap中不可以再出现

   



### 6.2 配置热更新

> 像server.port这样启动级别的配置  配置文件更改了 但是服务器本身不会改变

有两种方式实现:

1. 在`@Value`注解的类上加入`@RefreshScope`

2. 新建一个对象类,在对象类上使用注解`@ConfigurationProperties(prefix=“前缀”)`

   前缀说明, 比如 

   ``` yaml
   a:
   	b: 3
   ```

   则前缀为a

   ``` java
   @ConfigurationProperties(prefix=“a”)
   @Component
   @Data
   public class properties{
     private String b;
   }
   
   --------------
     使用的时候记得@Autowired
   ```

   

### 6.3 配置共享

微服务在启动时候, 会从nacos中读取多个配置文件:

+ `${prefix}-${spring.profile.active}.${file-extension}` 即 服务名-环境.yaml
+ `${prefix}.${file-extension}`   即 服务名.yaml

无论profile(环境名)怎么变化, 服务名.yaml一定会被加载

根据这个思路,  我们可以将一些共有的配置放在这个配置中

![image-20211224171723275](https://tva1.sinaimg.cn/large/008i3skNly1gxp1kw2ev0j31de0mgacn.jpg)











### 6.4 搭建Nacos集群

原理图:

![image-20211224174809899](https://tva1.sinaimg.cn/large/008i3skNly1gxp2gwlejnj30wi0u00vj.jpg)



实现步骤:

1. 搭建数据库
2. 安装nacos
3. 配置nacos
4. 启动nacos集群     启动命令: `./startup.sh -m cluster`
5. nginx反向代理
6. 负载均衡服务分发交给nginx去选择



视频: https://www.bilibili.com/video/BV1LQ4y127n4?p=29







## 07_Feign

http客户端Feign

### 7.1  Feign和RestTemplate

RestTemplate存在的问题:

RestTemplate编码比较复杂  难以统一

参数多的时候url过于长,且难以维护   不够优雅



**Feign的介绍**

Feign是一个声明式的http客户端   作用是帮助我们优雅的实现http请求的发送  从而解决上述问题



### 7.2 快速开始

1. 引入依赖

   ``` xml
   <dependency>
     <groupId>org.springframework.cloud</groupId>
     <artifactId>spring-cloud-starter-openfeign</artifactId>
   </dependency>
   ```

2. 启动类上添加注解`@EnableFeignClients`

3. 编写Feign客户端

   ``` java
   // 封装了所有user发起的远程调用
   @FeignClient("userservice")
   public interface UserController {
       @GetMapping("/user/{id}")
       User findById(@PathVariable("id") Long id);
   }
   ```

   主要是基于SpringMVC的注解来声明远程调用信息, 比如:

   + 服务名称:   userservice
   + 请求方式GET
   + 请求路径   /user/{id}
   + 请求参数   Long id
   + 返回值类型  User



### 7.3 Feign配置

自定义Feign配置

![image-20211224213618133](https://tva1.sinaimg.cn/large/008i3skNly1gxp92cgsr6j31ql0u0gr7.jpg)



实践:

配置Feign有两种方式

1. 配置文件方式

   1. 全局生效:

      ``` yaml
      feign:
      	client:
      		config:
      			default:     # 全局
      				loggerLevel: FULL
      ```

   2. 局部生效

      ``` yaml
      feign:
      	client:
      		config:
      			userservice:    # 单个service下生效
      				loggerLevel: FULL
      ```

2. java代码方式

   先声明一个Bean

   ``` java
   public class FeignClientConf{
     @Bean
     public Logger.Level feignLoggerLevel(){
       return Logger.Level.BASIC;
     }
   }
   ```

   1. 全局配置      在`@EnableFeignClients`注解中声明

      ``` java
      @EnableFeignClients(defaultConfiguration = FeignClientConf.class)
      ```

   2. 局部配置    在`@FeignClient`中声明

      ``` java
      @FeignClient(value = "userservice" , configuration = FeignClientConf.class)
      ```

      

### 7.4 Feigb性能优化

**Feign底层有三种客户端实现:**

+ URLConnection:   默认实现  不支持连接池
+ Apache HttpClient:   支持连接池
+ OKHttp:    支持连接池



**优化主要内容:**

1. 使用连接池代替默认的URLConnection
2. 日志级别  最好使用Basic或者None



**实践:**

1. 引入依赖

   ``` xml
   <dependency>
     <groupId>io.github.openfeign</groupId>
     <artifactId>feign-httpclient</artifactId>
   </dependency>
   ```

2. 配置连接池

   ``` yaml
   feign:
   	httpclient:
   		enable: true     # 开启HttpClient
   		max-connections:  200      # 连接池最大数量
   		max-connection-per-route: 50   # 每个路径最大连接数   给每一个请求最多分配50
   ```

   

   

### 7.5 最佳实践

方式一:

给消费者的FeignClient和提供者的controller定义统一的父接口作为标准 

图片说明:  

 提供者和调用者一个继承一个实现同一个接口,  使得双方代码标准统一.  但是过于紧耦合了  而且在controller中实现父接口不能获取到父接口的方法参数 需要再写一次

![image-20211224224320682](https://tva1.sinaimg.cn/large/008i3skNly1gxpb01wlapj31sf0u0afp.jpg)



方式二:  

将FeignClient抽取为独立模块, 并且把接口有关的POJO, 默认的Feign配置到放到这个模块中, 提供给消费者使用

图片说明:  

一个服务下有两个或者多个消费者, 原始情况下每个消费者都需要编写client去访问服务,

先提供一个jar包依赖, 其中已经提供了UserClient(客户端), 实体类, 默认的一些配置, 使用的时候只需要引入依赖使用就可以了

![image-20211224224908190](https://tva1.sinaimg.cn/large/008i3skNly1gxpb632oouj31v40pi41o.jpg)



**代码实现方法二步骤:**

1. 创建一个module, 命名为feign-api 引入feign的starter依赖
2. 将消费者中编写的Userclient, User实体类, DefaultFeignConfiguration复制到feign-api中
3. 在消费这种引入frign-api的依赖
4. 修改原来消费者中与上述三个组件有关的import部分,  改成feign-api的包
5. 重启测试

**代码实现方法二:**      消费者中 除了声明feign的配置之外 不需要任何配置

直接截图了

1. api部分代码
<img src="https://tva1.sinaimg.cn/large/008i3skNly1gxpbv0gc5ej30i013k0uh.jpg" alt="image-20211224231251810" style="zoom:50%;" />



![image-20211224231346375](https://tva1.sinaimg.cn/large/008i3skNly1gxpcakays7j323s0tc465.jpg)

![image-20211224231406701](https://tva1.sinaimg.cn/large/008i3skNly1gxpcag0gauj324a0tm44h.jpg)

![image-20211224231436687](https://tva1.sinaimg.cn/large/008i3skNly1gxpcacxs3mj31ld0u046s.jpg)



2. 消费者使用

   ![image-20211224231546658](https://tva1.sinaimg.cn/large/008i3skNly1gxpca9v4afj32d60k2n2e.jpg)

**不用写实体类, 配置, 客户端  在service中直接注入调用方法使用就可以了**

![image-20211224232727911](https://tva1.sinaimg.cn/large/008i3skNly1gxpca7295jj31es0u0gs4.jpg)

![image-20211224231719631](https://tva1.sinaimg.cn/large/008i3skNly1gxpca3lysjj31sk0u0dm0.jpg)



**出现的问题:**

@Autowired找不到对应的UserClient的Bean,  因为包范围不一样 所以消费者的模块中扫描不到api所在包的Bean

当定义的FeignClient不在SpringBootApplication的扫描范围时, 这些FeignClient则无法使用.

**解决: 两种方案**

1. 指定FeignClent所在的包

   `@EnableFeignClients(basePackage = “cn.itcast.feign.clients”)`

2. 指定FeignClient的字节码

   `@EnableFeignClients(clients = {UserClient.class})`

   

   

   

## 08_统一网关Gateway



**网关功能:**

+ 身份认证和权限校验
+ 服务路由, 负载均衡
+ 请求限流

![image-20211225115706495](https://tva1.sinaimg.cn/large/008i3skNly1gxpxxych4xj31n70u0q6n.jpg)



**网关技术的实现:**

SpringClooud中网关的实现包括两种:

1. gateway
2. zuul

Zuul是基于servlet实现的, 属于阻塞式编程.  而SpringCloudGateway属于Spring5中提供的WebFlux,  属于响应式编程的实现,具备更好的性能



**网关作用:**

+ 对用户请求做身份认证, 权限校验

+ 将用户请求路由到微服务, 并实现负载均衡

+ 对用户请求做限流

  

### 8.1 搭建网关服务

1. 创建新的module, 引入SpringCloudGateway依赖和nacos的服务发现依赖

   ``` xml
   <dependencies>
     <dependency>
       <groupId>org.springframework.cloud</groupId>
       <artifactId>spring-cloud-starter-gateway</artifactId>
     </dependency>
     <dependency>
       <groupId>com.alibaba.cloud</groupId>
       <artifactId>spring-cloud-starter-alibaba-nacos-discovery</artifactId>
     </dependency>
   </dependencies>
   <dependencyManagement>
     <dependencies>
       <dependency>
         <groupId>com.alibaba.cloud</groupId>
         <artifactId>spring-cloud-alibaba-dependencies</artifactId>
         <version>2.2.5.RELEASE</version>
         <type>pom</type>
         <scope>import</scope>
       </dependency>
     </dependencies>
   </dependencyManagement>
   ```

2. 编写路由配置及nacos地址  

   ``` yaml
   server:
     port: 10010   # 网关地址
   spring:
     application:
       name: gateway
     cloud:
       nacos:
         server-addr: localhost:8848
       gateway:
         routes:
           - id: user-service     # 路由id  唯一即可
             #uri: http://127.0.0.1:8081     # 路由的目标地址   http就是固定地址
             uri: lb://uservice  # 路由的目标地址  lb(loadBalance) 是负载均衡  后面根服务名
             predicates: # 路由断言，判断请求是否符合规则
               - Path=/user/** # 路径断言，判断路径是否是以/user开头，如果是则符合 
   ```

3. 访问127.0.0.1:10010/user/1 即跳转到

   1. 请求进入gateway做路由判断   符合断言结果
   2. 获取到nacos的服务列表 做负载均衡 
   3. 选择合适的服务 进行路由转发

   

图示:

注意: 外部访问走网关 内部访问可以绕过网关  直接拉取服务使用Feign访问由Nacos做负载均衡   网关主要用来做防火墙功能

![image-20211225125515424](https://tva1.sinaimg.cn/large/008i3skNly1gxpzmhzopqj31m50u0jug.jpg)



### 8.2 路由断言工厂Route Predicate Factory



Spring提供了11个基本的断言工厂:

![image-20211225130029464](https://tva1.sinaimg.cn/large/008i3skNly1gxpzrwu69zj31qv0u0gsh.jpg)



  写法可以在Spring官方网站查看:

https://docs.spring.io/spring-cloud-gateway/docs/2.2.9.RELEASE/reference/html/#gateway-request-predicates-factories



### 8.3 路由过滤器 GatewayFilter

GatewayFilter是网关中提供的一种过滤器, 可以对进入网关的请求和微服务返回的相应做处理

图示说明:

1. 请求进入网关
2. 网关做路由的同时, 经过一次断言工厂
3. 符合条件后进入各种各样的过滤器 到达微服务, 返回也是如此

![image-20211226133353853](https://tva1.sinaimg.cn/large/008i3skNly1gxr6cyzl86j31x20tw42s.jpg)



**Spring提供了31中不同的路由过滤工厂:**

![image-20211226133640858](https://tva1.sinaimg.cn/large/008i3skNly1gxr6fuv4hzj31y60ra42g.jpg)

写法可以在官网看:

https://docs.spring.io/spring-cloud-gateway/docs/2.2.9.RELEASE/reference/html/#gatewayfilter-factories



实操:

``` yaml
server:
  port: 10010
logging:
  level:
    cn.itcast: debug
  pattern:
    dateformat: MM-dd HH:mm:ss:SSS
spring:
  application:
    name: gateway
  cloud:
    nacos:
      server-addr: nacos:8848 # nacos地址
    gateway:
      routes:
        - id: user-service # 路由标示，必须唯一
          uri: lb://userservice # 路由的目标地址
          predicates: # 路由断言，判断请求是否符合规则
            - Path=/user/** # 路径断言，判断路径是否是以/user开头，如果是则符合
        	filters:
            - AddRequestHeader=name,this is JiaGuo    # 只对一个路由生效
      default-filters:       # 对所有的路由生效
        - AddRequestHeader=name,this is JiaGuo
```

+ 服务提供者 获取请求头

  ```java
  @GetMapping("test")
  // 获取请求头
  public String test3(@RequestHeader(value = "name", required = false) String name){
      System.out.println("name = " + name);
      return "null";
  }
  ```



### 8.4 全局过滤器 GlobalFilter

全局过滤器的作用也是处理一切进入网关的请求和微服务相应, 与GateWayFilter的作用一样

区别在于:

+ GatewayFilter通过配置定义, 处理逻辑是固定的
+ GlobalFilter的逻辑需要自己写   (比如判断用户身份)



实践:

![image-20211226161104714](https://tva1.sinaimg.cn/large/008i3skNly1gxrawmq43dj31x00pydjq.jpg)



+ java代码  需要把类生成一个Bean

  判断请求参数中是不是有一个autoorization参数 且等于admin 

  ``` java
  // @Order(-1)        // 定义过滤器优先级  值越小越优先
  @Component
  public class AuthorizeFilter implements GlobalFilter, Ordered {
      @Override
      public Mono<Void> filter(ServerWebExchange exchange, GatewayFilterChain chain) {
          // 1.获取请求参数
          ServerHttpRequest request = exchange.getRequest();
          MultiValueMap<String, String> params = request.getQueryParams();
          // 2.获取参数中的 authorization 参数
          String auth = params.getFirst("authorization");
          // 3.判断参数值是否等于 admin
          if ("admin".equals(auth)) {
              // 4.是，放行
              return chain.filter(exchange);
          }
          // 5.否，拦截
          // 5.1.设置状态码
          exchange.getResponse().setStatusCode(HttpStatus.UNAUTHORIZED);
          // 5.2.拦截请求
          return exchange.getResponse().setComplete();
      } 
      @Override
      public int getOrder() {
          return -1;
      }
  }
  
  ```

  

### 8.5 执行顺序

+ 每一个过滤器都必须指定一个int类型的order的值, 值越小, 优先级越高
+ GlobakFilter通过实现Ordered接口, 或者添加@Order注解来指定order值, 我们自定义就好
+ 路由过滤器和defaultFilter的order由Spring指定, 默认是按照声明顺序从1递增
+ 当过滤器的order值一样的时候, 按照defaultFilter>路由过滤器>GlobalFilter的顺序执行



### 8.6 跨域问题处理

什么是跨域问题:

+ 域名不同:  无论是一级二级还是三级 只有不同就算跨域
+ 域名相同,端口不同

跨域问题: 浏览器禁止请求的发起者与服务端发生跨域ajax请求,请求被浏览器拦截的问题

解决方案:

+ CORS:   浏览器询问服务器 是否允许跨域



网关实现:

Gateway网关已经做了CORS的实现, 我们只需要配置就可以了

``` yaml
spring:
  application:
    name: gateway
  cloud:
    nacos:
      server-addr: localhost:8848
    gateway:
      routes:
        - id: user-service     # 路由id  唯一即可
          #uri: http://127.0.0.1:8081     # 路由的目标地址   http就是固定地址
          uri: lb://userAccount  # 路由的目标地址  lb(loadBalance) 是负载均衡  后面根服务名
          predicates: # 路由断言，判断请求是否符合规则
            - Path=/** # 路径断言，判断路径是否是以/user开头，如果是则符合
          filters:
            - AddRequestHeader = name, This is Jia_Guo;
        globalcors:        # 全局跨域配置
          add-to-simple-url-handler-mapping: true  # 放行浏览器的跨域询问
          corsConfigurations:       # 具体的配置
            '[/**]':                # 对哪些请求进行处理  /** 所有请求
              allowedOrigns:        # 允许的地址
                - "http://127.0.0.1:8080"
                - "http://127.0.0.1:8090"
             allowedMethods:       # 允许的请求方式 
                - "GET"
                - "POST"
                - "DELETE"
                - "PUP"
                - "OPTIONS"
              allowedHeaders: "*"           # 允许在请求中带有头信息
              allowCredentuals: true            # 允许携带cookie
              maxAge: 36000           # 超时时间  这个时间内的同类请求都允许
```

























































