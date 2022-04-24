# Volecity引擎

## 01_Velocity简介

### 1.1 简介

Velocity是一个基于Java的模板引擎，通过特定的语法，Velocity可以获取在java语言中定义的对象，从而实现界面和java代码的真正分离。

即就是：   数据模型  +  Velocity模板 = （合并后）HTML

### 1.2应用场景

+ web应用程序： 作为应用程序的视图展示数据
+ 源代码生成： Velocity可用于基于模版生成java源代码
+ 自动电子邮件： 网站注册，认证等的电子邮件模版
+ 网页静态化： 基于Velocity模板生成静态网页



### 1.3 组成结构

1. app模块：

   主要封装了一些接口，暴露给使用者使用。主要有两个类，分别是Velocity(单例)和VelocityEngine。

2. Context模块：

   封装模板渲染时需要的变量

3. Runtime模块：

   整个Velocity的核心模块在runtime package下，这里会将加载的模板解析成JavaCC语法树，Velocity调用mergeTemplate方法时会渲染整棵树，并输出最终的渲染结果。

4. RuntimeInstance类

   RuntimeInstance类提供了一个单例模式，拿到这个实例就可以完成渲染过程了

5. 结构图：
<img src="http://images2015.cnblogs.com/blog/990532/201610/990532-20161025151421546-1123892540.png" alt="img" style="zoom:50%;" />

 

## 02_快速入门

### 2.1 需求分析

使用velocity定义HTML模板，在 不将数据填充进去

### 2.2 实现步骤

1. 创建Maven项目
2. 引入依赖
3. 定义模板
4. 输出html

### 2.3 代码实现

1. 创建maven项目

2. pom.xml

   ``` xml
   <?xml version="1.0" encoding="UTF-8"?>
   <project xmlns="http://maven.apache.org/POM/4.0.0"
            xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
            xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
       <modelVersion>4.0.0</modelVersion> 
       <groupId>com.gjStudy</groupId>
       <artifactId>velocityStudy</artifactId>
       <version>1.0-SNAPSHOT</version>
       <properties>
           <maven.compiler.source>8</maven.compiler.source>
           <maven.compiler.target>8</maven.compiler.target>
       </properties>
       <dependencies>
           <dependency>
               <groupId>org.apache.velocity </groupId>
               <artifactId>velocity-tools</artifactId>
               <version>2.0</version>
           </dependency>
           <dependency>
               <groupId>junit</groupId>
               <artifactId>junit</artifactId>
               <version>4.12</version>
               <scope>test</scope>
           </dependency>
       </dependencies>
       <build>
           <plugins>
               <plugin>
                   <groupId>org.apache.maven.plugins</groupId>
                   <artifactId>maven-compiler-plugin</artifactId>
                   <version>3.8.1</version>
                   <configuration>
                       <source>1.8</source>
                       <target>1.8</target>
                       <encoding>utf-8</encoding>
                   </configuration>
               </plugin>
           </plugins>
       </build>
   </project>
   ```

3. 定义模版（注意文件拓展名 和 路径）

   ![image-20211110142220480](https://tva1.sinaimg.cn/large/008i3skNly1gwa19939jjj31hc0rqq6m.jpg)

4. 输出html

   ``` java
   public class VelocityTest {
       @Test
       public void test1() throws Exception {
           // 1. 设置资源加载器
           Properties prop = new Properties();
           prop.put("file.resource.loader.class", "org.apache.velocity.runtime.resource.loader.ClasspathResourceLoader");
           // 2. 初始化Velocity引擎
           Velocity.init(prop);
           // 3. 创建Velocity容器
           VelocityContext velocityContext = new VelocityContext();
           velocityContext.put("name", "世界");
           // 4. 加载Velocity模版文件
           Template template = Velocity.getTemplate("vms/01-quickStart.vm", "utf-8");
           // 5. 合并数据到模版   新生成一个文件
           FileWriter fw = new FileWriter("/Volumes/SoftWare/JetBrains/IDEA/velocityStudy/src/main/resources/HTML/01-quickStart.html");
           template.merge(velocityContext, fw);
           // 6. 释放资源
           fw.close();
       }
   }
   ```

   

## 03_基础语法

### 3.1 VTL是什么

Velocity使用模板语言——VTL，用于前端的开发，接下来就来总结下常见的一些VTL语法。具体可以参考官方的[翻译文档](http://ifeve.com/apache-velocity-dev/)

### 3.2 注释

1. 单行注释   行注释

   ``` java
   # 注释内容
   ```

2. 多行注释    块注释

   ``` java
   #*
     行1
     行2
     行3
   *# 
   ```

3. 文档注释

   ``` java
   #**
     行1
     行2
   *#
   ```



### 3.3  非解析内容

这其中的东西不会被解析  但是会显示

语法: 

``` java
#[[
  行1
  行2
]]#
```



### 3.4 引用

#### 3.4.1 变量引用

引用语句就是对引擎上下文对象(Context中存的变量)中的属性进行操作.

语法方面分为常规语法`$属性`和正规语法`${属性}`

**语法**

``` java
$变量       若容器中没有想要的值  则输出  $变量
${变量}		  若容器中没有想要的值  则输出  ${变量}


$!变量       若容器中没有想要的值  则输出  空字符串
$!{变量}     若容器中没有想要的值  则输出  空字符串
```





#### 3.4.2 属性引用

引用语句就是对引擎上下文对象(Context中存的对象的属性)中的属性进行操作.

**语法**

``` java
$对象名.属性       若容器中没有想要的值  则输出  $对象名.属性
${对象名.属性}		  若容器中没有想要的值  则输出  ${对象名.属性}


$!对象名.属性       若容器中没有想要的值  则输出  空字符串
$!{对象名.属性}     若容器中没有想要的值  则输出  空字符串
```



#### 3.4.3 方法引用

引用语句就是对引擎上下文对象(Context中存的对象的方法)中的属性进行操作.

方法引用实际上指方法的调用, 注意方法的**参数**和**返回值**

**语法**

```
$对象.方法(参数,参数,参数)       若容器中没有想要的值  则输出  $对象.方法(参数,参数,参数)
${变量}     若容器中没有想要的值  则输出  ${对象.方法(参数,参数,参数)}


$!对象.方法(参数,参数,参数)       若容器中没有想要的值  则输出  空字符串
$!{对象.方法(参数,参数,参数)}     若容器中没有想要的值  则输出  空字符串
```



### 3.5 指令

指令主要用于定义重用模块,引入外部资源,流程控制,指令以`#`作为起始符



#### 3.5.1 流程控制

> #set  #if/#elseif/#else   #foreach

**#set**

作用: 页面中定义变量

语法: #Set($变量 = 值)

类型: 基本同JAVA   字符串 整数 浮点数   数组  布尔   map   list

实例(高级一点点): 

``` java
#set($name = "zhangsan")
#set($address = "shanxi xian  ${name}")
```



**#if/#elseif/#else**

``` java
#if(条件)
  ........
#elseif(条件)
  ......
#else
  ......
#end
```



**#foreach**

作用: 遍历循环或者集合   增强for

语法:

``` java
#foreach($item in $items)
  ......
  [#break]
#end
```

内置属性:   (没尝试成功  待定  但是`velocityCount`这个可以替换index)

+ $foreach.index:   获取遍历的索引,  从0开始
+ $foreach.count:   获取遍历的次数, 从1开始

示例:

``` java
#set($cities = ["xian","beijing","shanghai"])

#foreach($city in $cities)
    $!velocityCount ----  $city
    $!velocityHasNext ----  $city
#end
```



#### 3.5.2 引入资源

**#include**

作用: 引入外部资源,  引入得资源不会被解析

语法: `#include(resource)`

+ resource可以为单引号或者双引号得字符, 也可以是`$变量` , 内容是外部资源路径
+ 注意: 如果是相对路径, 则以引擎的配置文件加载器加载路径作为参考

示例:

``` java
# 不会被解析  把vm原文件的东西原封不动显示出来
# 注意从类加载器路径开始  即resource目录下
#include("vms/other.vm")
```



**#parse**

作用: 引入外部资源,  引入得资源会被解析

语法: `#parse(resource)`

+ resource可以为单引号或者双引号得字符, 也可以是`$变量` , 内容是外部资源路径
+ 注意: 如果是相对路径, 则以引擎的配置文件加载器加载路径作为参考

示例:

``` java
# 不会被解析  把vm文件解析后加载进去
# 注意从类加载器路径开始  即resource目录下
#parse("vms/other.vm")
```



**#define**

作用: 定义重用模块(不带参数)

语法:

``` java
#define($模块名称)
  模块内容
  例如下面是一个表格
  <table>
  <tr>
  <td>.....
#end
```

使用:

```java
# 同一页面中使用
$模块名称
```



**#evaluate**

作用: 动态计算,  动态计算可以让我们在字符串中使用变量   

说明: 字符串中的内容也会被引擎解析, 例如后端传回的字符串是`#foreach($city in $cities) $!velocityCount ----  $city $!velocityHasNext ----  $city #end`  我们希望传回来的东西也被引擎解析 就可以使用`#evaluate`指令

语法: `#evaluate(“计算语句”)`



#### 3.5.3 宏指令

作用: 定义重用模块(可带参数)

语法:    上述表格为例

``` java
#macro(table $list)
    模块内容
  例如下面是一个表格
  参数是$list 下面可以使用foreach进行遍历 生成动态表格
  <table>
  <tr>
  <td>.....
```

使用:

``` java
# 同一文件中
# userList是一个集合
$table($userList)
```



## 04_代码生成器CURD实现

### 4.1 需求说明

使用Velocity实现一个简单的代码生成器,生成项目开发过程中基础的CURD代码,导出到桌面zip压缩包



### 4.2 实现步骤

1. 创建项目
2. 导入依赖
3. 编写模板
4. 生成代码



### 4.3 代码实现

#### 4.3.1 创建项目

创建一个SpringBoot+Maven项目

#### 4.3.2 代码

1. 文件结构
     <img src="https://tva1.sinaimg.cn/large/008i3skNly1gwb9wwnbi4j30mm0ymwgi.jpg" alt="image-20211111160642475" style="zoom:50%;" />

   .

   ├── pom.xml

   ├── src

   │  ├── main

   │  │  ├── java

   │  │  │  └── com

   │  │  │    └── gjstudy

   │  │  │      └── velocitystudy

   │  │  │        ├── VelocityStudyApplication.java

   │  │  │        ├── util

   │  │  │        │  └── GenUtils.java

   │  │  │        └── web

   │  │  │          ├── controller

   │  │  │          ├── domain

   │  │  │          │  └── User.java

   │  │  │          ├── mapper

   │  │  │          └── service

   │  │  └── resources

   │  │    ├── application.properties

   │  │    ├── static

   │  │    │  └── mapper

   │  │    ├── templates

   │  │    └── vms

   │  │      ├── Controller.java.vm

   │  │      ├── Mapper.java.vm

   │  │      └── Service.java.vm

   │  └── test

   │    └── java

   │      └── com

   │        └── gjstudy

   │          └── velocitystudy

   │            └── VelocityStudyApplicationTests.java

2. vms下的三个模板

   ``` java
   //1. Controller
   ## 替换模板
   ## package com.gjstudy.velocitystudy.web.controller;
   package ${package}.controller;
   import ${package}.web.domain.${className};
   import ${package}.web.service.${className}Service;
   import org.springframework.beans.factory.annotation.Autowired;
   import org.springframework.web.bind.annotation.RequestMapping;
   import org.springframework.web.bind.annotation.RestController;
   import java.util.List;
   @RestController
   @RequestMapping("/${classname}")
   public class ${className}Controller {
       @Autowired
       private ${className}Service ${classname}Service;
       @RequestMapping("/list")
       public List<${className}> list() {
           return ${classname}.list();
       }
       @RequestMapping("/save")
       public void save(@RequestBody ${className} ${classname}) {
               ${classname}Service.save();
       }
       @RequestMapping("/update")
       public void update(@RequestBody ${className} ${classname}) {
               ${classname}Service.update(${classname});
       }
       @RequestMapping("/del")
       public void del(@RequestBody int[] ids) {
               ${classname}Service.del(ids);
       }
   }
   
   // 2. Service
   ##package com.gjstudy.velocitystudy.web.service;
   package ${package}.web.service;
   import ${package}.web.domain.User;
   import ${package}.web.mapper.UserMapper;
   import org.springframework.beans.factory.annotation.Autowired;
   import org.springframework.stereotype.Service;
   import java.util.List;
   @Service
   public class ${className}Service} {
       @Autowired
       private ${className}Mapper ${classname}Mapper;
   
       public List<${className}> list() {
           return ${classname}Mapper.list();
       }
   
       public void save() {
           ${classname}Mapper.save()};
       }
   
       public void update(${className} ${classname}) {
           ${classname}Mapper.update(${classname})};
       }
   
       public void del(int[] ids) {
           ${classname}Mapper.del(ids)};
       }
   }
   
   
   // 3. Mapper
   ##package com.gjstudy.velocitystudy.web.mapper;
   package ${package}.web.mapper;
   import ${package}.web.domain.User;
   import org.springframework.stereotype.Repository;
   import java.util.List;
   @Repository
   public interface ${className}Mapper} {
       List<${className}> list();
       void save();
       void update(${className} ${classname});
       void del(int[] ids);
   }
   ```

3. 工具类 GenUtils

   ``` java
   /**
    * @author ：JiaGuo
    * @date ：Created in 2021/11/11 13:50
    * @description：生成代码工具类
    * @modified By：
    * @version: $
    */
   public class GenUtils {
       /**
        * 生成java代码  打包输出
        * @param data           传入的数据
        * @param templates      模板库
        * @param zipOutputStream      zip输出流
        */
       public static void generatorCode(Map<String, Object> data, List<String> templates, ZipOutputStream zipOutputStream) throws Exception {
           // 1. 设置资源加载器
           Properties prop = new Properties();
           prop.put("file.resource.loader.class", "org.apache.velocity.runtime.resource.loader.ClasspathResourceLoader");
           // 2. 初始化Velocity引擎
           Velocity.init(prop);
           // 3. 创建Velocity容器    并存入容器中
           VelocityContext velocityContext = new VelocityContext(data);
           // 4. 加载Velocity模版文件  遍历每一个模板
           for (String template : templates) {
               // 4. 加载Velocity模版文件
               Template tpl = Velocity.getTemplate(template, "utf-8");
               // 合并代码
               StringWriter sw = new StringWriter();
               tpl.merge(velocityContext, sw);
               // 把代码数据输出到zip中
               //zip 文件名
               String fileName = getFileName(template, data.get("className").toString(), data.get("package").toString());
               // 创建一个Zip流
               zipOutputStream.putNextEntry(new ZipEntry(fileName));
               // 往zip流里写数据
               IOUtils.write(sw.toString(), zipOutputStream, "UTF-8");
               // 6. 释放资源
               IOUtils.closeQuietly(sw);
           }
       }
       /**
        * 生成路径名
        * 根据模板名称 类名 包名拼接成一个完整的文件名
        * @param template      模板名称
        * @param className     实体类的类名
        * @param packageName   包名
        * @return  /main/java/com/XXXX/controller/UserController.java
        */
       public static String getFileName(String template, String className, String packageName) {
           // 包名 main/java   因为main/java是通用的
           String packagePath = "main" + File.separator + "java" + File.separator;
           // 如果传入的包名不为空  则把包名中的.全部换成/
           // 例如com.gj.study -> main/java/com/gj/study/
           if (StringUtils.isNoneEmpty(packageName)) {
               packagePath += packageName.replace(".", File.separator)+File.separator;
           }
           // 控制层
           // 传入的模板字符串包含Controller.java.vm
           // 则制作成 main/java/com/gj/study/Controller/UserController.java
           if (template.contains("Controller.java.vm")) {
               return packagePath + "Controller" + File.separator + className + "Controller.java";
           }
           // 业务层
           if (template.contains("Service.java.vm")) {
               return packagePath + "Service" + File.separator + className + "Service.java";
           }
           // 持久层
           if (template.contains("Mapper.java.vm")) {
               return packagePath + "Mapper" + File.separator + className + "Mapper.java";
           }
           return null;
       }
   }
   ```

4. 测试类

   ``` java
   //@SpringBootTest
   @RunWith(SpringRunner.class)
   @SpringBootTest(classes = VelocityStudyApplication.class)
   class VelocityStudyApplicationTests {
       @Test
       public void test1() throws Exception {
           // 数据
           Map<String, Object> data = new HashMap<String, Object>();
           data.put("package", "com.gj.study");
           data.put("className", "User");
           data.put("classname", "user");
           // 模板列表
           List<String> templates = new ArrayList<String>();
           templates.add("vms/Controller.java.vm");
           templates.add("vms/Service.java.vm");
           templates.add("vms/Mapper.java.vm");
           // 输出流
           File file = new File("/Users/jiaguo/Desktop/aa.zip");
           FileOutputStream outputStream = new FileOutputStream(file);
           ZipOutputStream zipOutputStream = new ZipOutputStream(outputStream);
           // 输出
           GenUtils.generatorCode(data,templates,zipOutputStream);
           zipOutputStream.close();
           outputStream.close();
       }
   }
   ```

   

# VelocityTools



## 01_Velocity介绍

**简介:**

​		Velocity Tools 是 Velocity 模板引擎的一个子项目，用来将 Velocity 与 Web开发环境集成的工具包。例如你可以利用 VelocityTools 来集成 Velocity 和 Struts 框架，同时 VelocityTools 还提供 Velocity 的布局模板，以及很多常用的工具包。

**组成:**

VelocityTools项目分为两部分: GenericTools和VelocityView

+ GenericTools: GenericTools是一组工具类,它们提供在标准Vvelociry项目中使用工具的基本基础结构，以及在通用velocity模板中使用的一组工具。例如：DateTool.NumberTool和RenderTool很多其他可用的工具
+ VelocityView: 包括所有的通用工具结构和在web应用程序的视图层中使用velocity的专用工具。这包括用于处理Velocity模板请求的velocityviewservlet或velocityLayoutservlet、用于在JSP中嵌入velocity的velocityviewTag和用于在velocity模板中嵌
  入SP标记/库的Maven插件。这里流行的工具是LinkTool和ParameterTool.



## 02_GenericTools使用



### 2.1 GenericTools介绍

GenericTools是一组工具类,它们提供在标准Vvelociry项目中使用工具的基本基础结构，以及在通用velocity模板中使用的一组工具。例如：DateTool.NumberTool和RenderTool很多其他可用的工具

GenericTools就是Velocity官方提供的一组可以在模板中使用的工具类库,  [GenericTools官方Docs]([https://velocity.apache.org/tools/3.0/apidocs/]:)





### 2.2 GenericTools项目搭建

#### 2.2.1 项目搭建

``` xml
<dependency>
  <groupId>org.apache.velocity</groupId>
  <artifactId>velocity-engine-core</artifactId>
  <version>2.3</version>
</dependency>
<dependency>
  <groupId>org.apache.velocity.tools</groupId>
  <artifactId>velocity-tools-generic</artifactId>
  <version>3.1</version>
</dependency>
<dependency>
  <groupId>junit</groupId>
  <artifactId>junit</artifactId>
  <version>4.12</version>
  <scope>test</scope>
</dependency>
</dependencies>
```



### 2.2.2 创建模板

path: resource/vms/xxxxx.vm

``` html
<!DOCTYPE html>
<html lang="en">
<body>
## 本质而言date是VelocityTools的一个key  $date代替的是DateTools
当前时间: $date.get('yyyy-M-d H:m:s')
</body>
</html>
```



### 2.2.3 配置

path: resource/tools.xml

``` xml
<?xml version="1.0" encoding="UTF-8"?>
<tools>
    <toolbox scope="application">
        <tool class="org.apache.velocity.tools.generic.DateTool"
              format="yyyy-MM-dd"/>
    </toolbox>
</tools>
```



### 2.2.4 使用

测试类中

``` java
/**
 * @author ：JiaGuo
 * @date ：Created in 2021/11/12 09:35
 * @description：this is a test class
 * @modified By：
 * @version: 1.0
 */
public class GenericToolsTest {
    @Test
    public  void test1() throws IOException {
        // 1. 创建一个velocity引擎
        VelocityEngine velocityEngine = new VelocityEngine();
        // 2. 设置模板路径
        velocityEngine.setProperty(Velocity.RESOURCE_LOADER, "class");
        velocityEngine.setProperty("class.resource.loader.class", "org.apache.velocity.runtime.resource.loader.ClasspathResourceLoader");
        // 3. 初始化velocity引擎
        velocityEngine.init();
        // 4. 加载tools.xml配置文件
        ToolManager toolManager = new ToolManager();
        toolManager.configure("tools.xml");
        // 5. 加载模板
        Template template = velocityEngine.getTemplate("vms/1.dateToolsTest.vm");
        // 6. 设置数据
        ToolContext context = toolManager.createContext();
        // context.put("now", new Date());
        // 7. 合并数据到模板
        FileWriter fileWriter = new FileWriter("/Volumes/SoftWare/JetBrains/IDEA/GenericToolsStudy/src/main/resources/html/1.dateToolsTest.html");
        template.merge(context, fileWriter);
        // 8. 释放资源
        fileWriter.close();
    }
}
```



### 2.3 工具类 及案例

格式化工具类的主要作用是对数据进行格式化后输出, 例如 日期, 数字 等等超级多 下面随便讲几个



#### DateTool

用于访问和格式化日期及格式化Date和Calender对象. 

<img src="https://tva1.sinaimg.cn/large/008i3skNly1gwc66al103j318k0s8dk7.jpg" alt="image-20211112104250335" style="zoom:50%;" />

``` java
<!DOCTYPE html>
<html lang="en">
<body>
## 本质而言date是VelocityTools的一个key  $date代替的是DateTools
当前时间: $date.get('yyyy-M-d H:m:s')
获取年:  $date.getYear()
获取年:  $date.getYear(${now})
获取月:  $date.getMonth()
获取月:  $date.getMonth(${now})
获取日:  $date.getDay()
获取日:  $date.getDay(${now})
默认格式化当前时间:   $date.format(${now})    ----  具体怎么格式化在tools.xml中配置
自定义格式化当前时间:   $date.format("yyyy-M-d H:m:s",${now})   
格式化
    $date                         -> Oct 19, 2003 9:54:50 PM
    $date.long                    -> October 19, 2003 9:54:50 PM PDT
    $date.medium_time             -> 9:54:50 PM
    $date.full_date               -> Sunday, October 19, 2003
    $date.get('default','short')  -> Oct 19, 2003 9:54 PM
    $date.get('yyyy-M-d H:m:s')   -> 2003-10-19 21:54:50
    $date.iso                     -> 2003-10-19T21:54:50-07:00
    $date.iso_tz_time             -> 21:54:50-07:00
    $date.intl_tz                 -> 2003-10-19 21:54:50 CET
</body>
</html>
```



#### NumberTool

访问和格式化各种数字类型

<img src="https://tva1.sinaimg.cn/large/008i3skNly1gwcccdgttfj31f00owadt.jpg" alt="image-20211112122708485" style="zoom:50%;" />





#### MathTool

对于数字的运算  加减乘除  数字运算巴拉巴拉的  

四舍五入  绝对值  平均数  max   min   

<img src="https://tva1.sinaimg.cn/large/008i3skNly1gwccc9t4xzj31oc0padka.jpg" alt="image-20211112122939084" style="zoom:50%;" />



#### DisplayTool

控制数据的显示和隐藏   数据格式的处理

xml自己在官网找

<img src="https://tva1.sinaimg.cn/large/008i3skNly1gwccc53rvfj31o60j2ae4.jpg" alt="image-20211112124819531" style="zoom:50%;" />

#### EscapeTool

 对特殊字符进行转义处理   如 $ # & ….

<img src="https://tva1.sinaimg.cn/large/008i3skNly1gwca19orguj30u017ctcc.jpg" alt="image-20211112125709785" style="zoom:50%;" />



#### FiledTool

访问类中public定义的静态常量   include中可以使用`,`声明多个类

<img src="https://tva1.sinaimg.cn/large/008i3skNly1gwccbxpzz8j31l90u0jx5.jpg" alt="image-20211112140857268" style="zoom:50%;" />

![image-20211112142328958](https://tva1.sinaimg.cn/large/008i3skNly1gwccizyhq1j31m00lmwio.jpg)

#### ClassTool

ClassTools用于访问一个类的class对象信息以及其`Filed`,`Method`,`Constructor`等信息,   但是不支持反射执行代码行为

<img src="https://tva1.sinaimg.cn/large/008i3skNly1gwcej3mmcqj30u00u5whl.jpg" alt="image-20211112143159834" style="zoom:50%;" />





#### ContextTool

获取Context中保存的数据和元数据

<img src="https://tva1.sinaimg.cn/large/008i3skNly1gwceizc494j314s0ogacv.jpg" alt="image-20211112152213500" style="zoom:50%;" />

<img src="https://tva1.sinaimg.cn/large/008i3skNly1gwce9xytk5j30tk0l2wgl.jpg" alt="image-20211112152347981" style="zoom:50%;" />



#### RenderTool

将给定的字符串当VTL执行

<img src="https://tva1.sinaimg.cn/large/008i3skNly1gwceit0kfwj31go0rw77y.jpg" alt="image-20211112153208255" style="zoom:50%;" />

#### SortTool

对集合和数组数据进行排序,  底层: 在排序期间通过comparaTo()来比较, 但要调用comparaTolgnoreCase()的字符除外, 将集合数据转化为合适的类型后, 通过调用Collection.sort()来执行排序

<img src="https://tva1.sinaimg.cn/large/008i3skNly1gwcezkwzs7j319q0ryn1w.jpg" alt="image-20211112154828748" style="zoom:50%;" />



#### CollectionTool

CollectionTool允许用户对集合中包含的对象公开的任意属性集对集合(数组迭代器等)进行排序, 并通过拆分字符串来生成数组

 <img src="https://tva1.sinaimg.cn/large/008i3skNly1gwcfhsrr6fj30y00u0q66.jpg" alt="image-20211112160602109" style="zoom:50%;" />



#### XmlTool

XmlTools用于读取  和浏览XML

<img src="https://tva1.sinaimg.cn/large/008i3skNly1gwcfrl8y24j312u08ut9x.jpg" alt="image-20211112161527994" style="zoom:50%;" />











