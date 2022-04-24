[TOC]
# Servet

## 01_servet概述

servet其实就是运行在web服务器上的一个小型java程序,用于处理从web客户端发送的请求,并且对请求做出相应。

<img src="https://gitee.com/thisisafigurebed/figure-bed/raw/master/servletNode/6.png" >

### 1.1使用Servet

+ 编写一个java类实现servet的接口
+ 配置servet

## 02_servet入门

遇到的问题:

+ servlet包没有:alt+ctrl+shift+s添加library->tomcat

1. 创建包和类(servet是java项目所以在src下创建)

2. 右键项目添加addFrameWork->web Application.... 

   <img src="https://gitee.com/thisisafigurebed/figure-bed/raw/master/servletNode/1.png" style="zoom:70%">

3. 实现接口

   <img src="https://gitee.com/thisisafigurebed/figure-bed/raw/master/servletNode/2.png" style="zoom:70%">

4. 配置这个类在web.xml中

   访问localhost:8080/hello

   ```xml
       <!-- 配置servelet   -->
       <servlet>
           <!--   配置servlet名称     -->
           <servlet-name>ServletDemo</servlet-name>
           <!--   配置servlet类的全路径     -->
           <servlet-class>com.Servlet.Demo.ServletDemo</servlet-class>
       </servlet>
   
       <!-- 配置servlet映射   -->
       <servlet-mapping>
           <!--   配置servlet名称     -->
           <servlet-name>ServletDemo</servlet-name>
           <!--   配置servlet类的访问路径     -->
           <url-pattern>/hello</url-pattern>
       </servlet-mapping>
   ```

## 03_Servlet执行流程

   <img src="https://gitee.com/thisisafigurebed/figure-bed/raw/master/servletNode/3.png" style="zoom:100%">

## 04_Servlet实现类

   > 因为我们只需要实现Service方法,如果采用接口方法需要把所以类都实现

   Servlet实现关系:

   <img src="https://gitee.com/thisisafigurebed/figure-bed/raw/master/servletNode/4.png" style="zoom:70%">

   重新简写后的代码:

   ```java
   public class ServletDemo extends HttpServlet {
       @Override
       protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
           doPost(req, resp);
       }
       @Override
       protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
           resp.getWriter().println("Hello Servlet....");
       }
   }
   //记得写xml文件
   ```

## 05_servlet生命周期

   <img src="https://gitee.com/thisisafigurebed/figure-bed/raw/master/servletNode/5.png">

+ 生命周期代码演示

  ```java
  public class ServletDemo implements Servlet {
      /*
      * Servlet对象被实例化时候init方法就会执行,而且只执行一次(Serlvet是单例的)
      * */
      @Override
      public void init(ServletConfig servletConfig) throws ServletException {
          System.out.println("Servlet初始化了.....");
      }
      /*
       * 每一次 请求都会调用一次service方法
       * */
      @Override
      public void service(ServletRequest servletRequest, ServletResponse servletResponse) throws ServletException, IOException {
          System.out.println("Servlet被执行了.....");
      }
      /*
       * Serlet从服务器关闭时候或者移出时候销毁Servlet,只执行一次
       * */
      @Override
      public void destroy() {
          System.out.println("Servlet被销毁.....");
      }
      @Override
      public ServletConfig getServletConfig() {
          return null;
      }
      @Override
      public String getServletInfo() {
          return null;
      }
  }
  ```

## 06_配置servlet启动时加载

### 6.1为什么要这么配置

因为请求时候会调用init方法,如果init方法里有一些比如配置信息等加载就会很慢。用户就会等待很久(只要第一个用户会等待,后面的由于第一个用户请求过则不会再初始化)

### 6.2如何配置

1. 配置xml文件

2. 配置优先级

   2.1 在需要设置启动时加载的servlet标签中添加

   2.2 <load-on-startup>2</load-on-startup>

   2.3 中间数字为任意大于2的正整数

   2.4 因为tomcat中有一个默认的servlet优先级是1

3. 设置完成

```xml
   <!-- config servelet   -->
    <servlet>
        <servlet-name>ServletDemo</servlet-name>
        <servlet-class>com.Servlet.Demo.ServletDemo</servlet-class>
        <load-on-startup>2</load-on-startup>
    </servlet>
    <servlet-mapping>
        <servlet-name>ServletDemo</servlet-name>
        <url-pattern>/hello</url-pattern>

    </servlet-mapping>
    <servlet>
        <servlet-name>ServletDemo2</servlet-name>
        <servlet-class>com.Servlet.Demo.ServletDemo2</servlet-class>
    </servlet>
    <servlet-mapping>
        <servlet-name>ServletDemo2</servlet-name>
        <url-pattern>/hello1</url-pattern>
    </servlet-mapping>
```

## 07_Servlet访问路径配置

> 有三种配置
>
> 即就是xml中<Servlet-mapping>标签中的<url-pattern>属性

1. 完全路径配置

   + 以/开始:例如/hello    /hello1
   + 则访问的方式是http://localhost:8080/hello
   + 或者http://localhost:8080/hello1

2. 目录匹配

   + 以/开始*结尾
   + 则访问的方式是http://localhost:8080/*
   + 或者http://localhost:8080/hello1/*
   + *指的是任意输入都可以

3. 拓展名访问

   + 不能以/开始   以*.action  *.jsp等的拓展名
   + 则访问的方式是http://localhost:8080/*.action
   + 或者http://localhost:8080/hello1/*.jsp
   + *指的是任意输入

4. 优先级是 1->2->3

 ```xml
       <servlet>
           <servlet-name>ServletDemo2</servlet-name>
           <servlet-class>com.Servlet.Demo.ServletDemo2</servlet-class>
       </servlet>
       <servlet-mapping>
           <servlet-name>ServletDemo2</servlet-name>
           <url-pattern>/hello1</url-pattern>
       </servlet-mapping>
       <servlet>
           <servlet-name>ServletDemo3</servlet-name>
           <servlet-class>com.Servlet.Demo.ServletDemo2</servlet-class>
       </servlet>
       <servlet-mapping>
           <servlet-name>ServletDemo3</servlet-name>
           <url-pattern>/*</url-pattern>
       </servlet-mapping>    
       <servlet>
       <servlet-name>ServletDemo4</servlet-name>
       <servlet-class>com.Servlet.Demo.ServletDemo2</servlet-class>
   </servlet>
       <servlet-mapping>
           <servlet-name>ServletDemo4</servlet-name>
           <url-pattern>*.action</url-pattern>
       </servlet-mapping>
 ```

   

## 07_Servlet对象(重要)

### 7.1 ServletConfig对象

概述:ServletConfig用来获得servlet相关配置的对象。

```java
ServletConfig getServletConfig()  
```

#### 7.1.1ServletConfig的API

```java
String getInitParameter(String name)	   //获得servlet初始化参数
Enumeration getInitParameterNames() 	   //获得初始化参数的名称
ServletContext getServletContext() 		   //获得getServletContext对象
String getServletName() 				 //获得servlet的名称
```

#### 7.1.2代码演示

1. String getInitParameter(String name)	   //获得servlet初始化参数

   1. 首先配置servlet初始化参数

   2. xml的servlet中配置

   3. ```xml
          <servlet>
              <servlet-name>ServletDemo2</servlet-name>
              <servlet-class>com.Servlet.Demo.ServletDemo2</servlet-class>
              <init-param>
                  <param-name>username</param-name>
                  <param-value>root</param-value>
              </init-param>
              <init-param>
                  <param-name>password</param-name>
                  <param-value>123123</param-value>
              </init-param>
          </servlet>
      ```

   4. ServletDemo2代码

   5. ```java
      public class ServletDemo2 extends HttpServlet {
          @Override
          protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
              doPost(req, resp);
          }
          @Override
          protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
              ServletConfig config = this.getServletConfig();
              //getInitParameter
              String s1 = config.getInitParameter("username");
              String s2 = config.getInitParameter("password");
              resp.getWriter().println(s1);
              resp.getWriter().println(s2);
      
              //getInitParameterNames
              Enumeration<String> name = config.getInitParameterNames();
              while (name.hasMoreElements()) {
                  String s3 = name.nextElement();
                  String s4 = config.getInitParameter(s3);
                  resp.getWriter().println(s3+" "+s4);
              }
              //getServletName
              String s5 = config.getServletName();
              resp.getWriter().println("Servlet的名称是: "+s5);
          }
      }
      ```

### 7.2 ServletContext对象

> ServletContext对象:是Servlet上下文对象。Servlet对象知道Servlet之前和之后的东西。这个对象每一个Servlet只要一个,服务器启动时候为每个Web项目创建一个单独的ServletContext对象。
> ServletContext作用范围:整个Web应用

#### 7.2.1作用

1. 用来获取web项目信息

   因为一个web项目只有一个ServletConfig对象,所以这个对象对整个项目的信息都了解

   + 获取文件的MIME类型:(用于文件上传和下载)

   ```java
    String getMimeType(String file)  	//Returns the MIME type of the specified file, or null if the MIME type is not known. 
   ```

   + 获取web项目请求的工程名

     ```java
     String getContextPath()  	//Returns the context path of the web application. 
     ```

     

   + 获取web初始化参数

     ```java
     String getInitParameter(String name)  	// 获取全局初始化参数
     Enumeration getInitParameterNames()  	//  获取全局初始化参数名字
     
     ```

   + 配置全局初始化参数

     ```xml
         <!--  全局初始化参数  -->
         <context-param>
             <param-name>name</param-name>
             <param-value>aaaaa</param-value>
         </context-param>
         <context-param>
             <param-name>password</param-name>
             <param-value>123456</param-value>
         </context-param>
     
     ```

   + 代码

     ```java
     public class ServletDemo2 extends HttpServlet {
         @Override
         protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
             doPost(req, resp);
         }
         @Override
         protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
             //获取mime类型
             String s = getServletContext().getMimeType("a.txt");
             System.out.println(s);
             //获取访问路径名
             String s1 = getServletContext().getContextPath();
             System.out.println(s1);
             //获取全局参数
             String name = getServletContext().getInitParameter("name");
             String name1 = getServletContext().getInitParameter("password");
             System.out.println(name + " " + name1);
             //获取全局参数名
             Enumeration<String> names = getServletContext().getInitParameterNames();
             while (names.hasMoreElements()) {
                 String names1 = names.nextElement();
                 System.out.print(names1+" ");
             }
         }
     }
     ```

2. 使用ServletContext对象读取web项目下的文件

   > 原来IO流获取根据路径获取 而web项目发布在tomcat上路径有所不同不能使用传统io流去获取(传统IO流使用相对路径相对项目路径,而web项目相对的是java虚拟机的bin位置)

   ```java
   //此处使用绝对路径获取的是IO流
   InputStream stream = getServletContext().getResourceAsStream("/WEB-INF/classes/......");
   
   //获得文件磁盘上的绝对路径
   String path = getServletContext().getRealPath("/WEB-INF/classes/文件具体名");
   System.out.println(path);
   InputStream stream = new FileInputStream(path);
   //接下来就获取到文件肆意操作了
   ```

3. ServletContext对象作为域对象存取数据

   > 域对象:指的是将数据存入域对象中这个数据就会有一定的作用范围。域指的就是作用范围。  ServletContext作用范围:整个Web应用

   ```java
    void setAttribute(String name, Object object)   //将数据存入域
    Object getAttribute(String name)	//将数据取出 
    void removeAttribute(String name)     //移除数据
    Enumeration getAttributeNames()    
   ```

   代码演示:

   ```java
   public class ServletDemo2 extends HttpServlet {
       @Override
       public void init() throws ServletException {
           getServletContext().setAttribute("name", "张三");
       }
       @Override
       protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
           doPost(req, resp);
       }
       @Override
       protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
           Object o = getServletContext().getAttribute("name");
           System.out.println(o);
           
       }
   }
   ```
   
 ## 08_请求和响应对象

### 8.1 Response响应对象

#### 8.1.1response对象API

1. 相应行
   1. 设置状态码(200,302,304,404,500.....)

```java
 void setStatus(int sc)  //设置状态码
```

2. 相应头:

   1. set开头的方法是一个Key对应一个Value
      1. 举例:有一个头content-Type:text/html setHeader("content-Type","text/plain")
      2. 则最终结果是:content-Type:text/plain
   2. add开头的方法是一个key对应多个value
      1. 举例:有一个头content-Type:text/html addHeader("content-Type","text/plain")
      2. 则最终结果是:content-Type:text/html,text/plain

   ```java
   void addDateHeader(String name, long date) 
   void addHeader(String name, String value) 
   void addIntHeader(String name, int value) 
    
   void setDateHeader(String name, long date) 
   void setHeader(String name, String value) 
   void setIntHeader(String name, int value) 
   ```


3. 响应体:

   1. 设置响应体的方法:向页面输出内容

   ```java
   ServletOutputStream getOutputStream()    //字节流
   PrintWriter getWriter()    //字符流
   ```

4. Response其他的一些API

   1. 重定向(简化setHeader的方法)

      ```java
      void sendRedirect(String location) 
      ```

   2. 页面字符集

      ```java
      void setContentType(String type) 
      ```

   3. 响应缓冲区字符集

      ```java
      void setCharacterEncoding(String charset) 
      ```

   4. 服务器向浏览器回写cookie

      ```java
      void addCookie(Cookie cookie) 
      ```

5. 代码实现

   ```java
   public class ServletDemo extends HttpServlet {
       @Override
       protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
           doPost(req, resp);
       }
       @Override
       protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
           /*响应行
           *1. 设置状态码单独出现没有意义
            * resp.setStatus(302);
            * */
   
           
           /*响应头
           *2.1 重定向 使用302状态码和Location完成定向到hh(hh是新的servlet类需要写xml文件)
           *resp.setStatus(302);
           *resp.setHeader("Location","/WebStudy/hh");
           * */
           
           /*
           *2.2.定时刷新重定向
           *resp.setContentType("text/html;charset=UTF-8");
           *resp.getWriter().println("5秒后刷新");
           *resp.setHeader("Refresh","5;url=/WebStudy/hh");
           * */
   
           /*
            *2.3. 简化重定向
            *resp.sendRedirect("/WebStudy/hh");
            * */
   
           
           /*响应体
           *1.1 字节流给页面输出中文
           * resp.setContentType("text/html;charset=UTF-8");
           * //字节流方式给网页输出内容
           * resp.getOutputStream().write("输出中文".getBytes("UTF-8"));
           * */
           
           /*
           * 1.2字符流给页面输出中文
           * //设置浏览器头 名:编码 值:text/html;chrset=UTF-8;
           * resp.setHeader("Content-Type", "text/html;chrset=UTF-8");
           * //设置字符缓冲区编码
           * resp.setCharacterEncoding("UTF-8");
           * resp.getWriter().println("中文");
           * */
       }
   }
   ```

### 8.2 Request请求对象

1. 获取客户端相关信息

   ````java
String getMethod()     //获得请求的方式  get post
String getQueryString()   //获得链接后的请求参数字符串
String getRequestURI()   //获得请求路径 
StringBuffer getRequestURL()  //获得请求路径
String getRemoteAddr()   //获得请求客户机的ip
String getHeader(String name)  //获取1对1 的请求头
Enumeration getHeaders(String name)   //获得所有的头
long getDateHeader(String name)  //获取1对n 的请求头
    
    //获取参数
String getParameter(String name)   //获得提交的参数(一个name对应一个value)
String[] getParameterValues(String name)  //获得提交的参数(一个name对应多个value)
Map getParameterMap()   //获得提交的参数(1to1 OR 1toN)保存在一个map中
    
	//Request作为域对象存取数据
void setAttribute(String name, Object o)   //向request域中存储数据
Object getAttribute(String name)   //从request域中得到数据
void removeAttribute(String name)    //移除
 
   ````

 	

1. 获取客户端信息代码

   ```java
   protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
       //获取客户机信息
       System.out.println("请求方式" + " " + req.getMethod());
       System.out.println("客户机ip地址" + " " + req.getRemoteAddr());
       System.out.println("请求路径URI" + " " + req.getRequestURI());
       System.out.println("请求路径URL" + " " + req.getRequestURL());
       System.out.println("客户机浏览器信息" + " " + req.getHeader("User-Agent"));
   }
   ```

2. 获取浏览器参数(h5方面get和post随意,都不影响服务器代码)

```html
<!DOCTYPE html>
<html lang="en" xmlns="http://www.w3.org/1999/html">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<h1>使用Request接收表单</h1>
<form action="/WebStudy/ServletDemo2" method="get">
    用户名:<input type="text" name="username"><br>
    密码:<input type="password" name="password"><br>
    性别:<input type="radio" name="sex" value="man">男
    <input type="radio" name="sex" value="woman">女<br>
    籍贯:<select name="city">
    <option value="beijing">北京</option>
    <option value="shanghai">上海</option>
    <option value="guangzhou">广州</option>
    <option value="shenzhen">深圳</option>
</select><br>
    爱好:<input type="checkbox" name="hobby" value="basketball">篮球
    <input type="checkbox" name="hobby" value="football">足球
    <input type="checkbox" name="hobby" value="volleyball">排球
    <br>
    自我介绍:<textarea name="info" rows="3" cols="8"></textarea>
    <br>
    <input type="submit" value="提交">
</form>
</body>
</html>
```

```java
package com.Servlet.Demo;
import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;
import java.util.Arrays;
import java.util.Map;
public class ServletDemo2 extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
       doPost(req, resp);
    }
    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        req.setCharacterEncoding("utf-8");
        String username = req.getParameter("username");
        System.out.println("用户名:"+username);
        String password = req.getParameter("password");
        System.out.println("密码:"+password);
        String sex = req.getParameter("sex");
        System.out.println("性别:"+ sex);
        String city = req.getParameter("city");
        System.out.println("city:"+city);
        String[] hobby = req.getParameterValues("hobby");
        System.out.println("hobby:"+Arrays.toString(hobby));
        String info = req.getParameter("info");
        System.out.println("info:"+info);
        System.out.println("--------");
        Map<String, String[]> map = req.getParameterMap();
        for (String key : map.keySet()) {
            String[] strings = map.get(key);
            System.out.println(key+": "+ Arrays.toString(strings));
        }
    }
}
```

### 8.3Request请求中文乱码问题

tomcat8之后以及不用区分get和post了

1. get方式

   + 原因:get方式提交的数据在url后面.在地址栏以及进行了一次编码这个编码不是UTF也不是ISO
   + 解决:将request缓冲区的值以ISO-8859-1取出,以UTF-8方式编码

   + 代码:

     ```java
     String name = req.getParameter("name");
     String encode = URLEncoder.encode(name,"ISO-8859-1");
     String decode = URLDecoder.decode(encode,"UTF-8");
     
     //方法二 String有一个构造方法可以编码
     String name = req.getParameter("name");
     String value = new String(name.getBytes(ISO-8859-1),"UTF-8")
     ```

2. post方式

   + 原因:提交后数据存放在缓冲区中,缓冲区进行了ISO-8859-1编码了

   + 解决:将req缓冲区的编码修改即可

     ```java
     req.setCharacterEncoding("utf-8");
     ```



## 09_会话

### 9.1 什么是会话

	在计算机术语中，会话是指一个终端用户与交互系统进行通讯的过程，比如从输入账户密码进入操作系统到退出操作系统就是一个会话过程。会话较多用于网络上，TCP的三次握手就创建了一个会话，TCP关闭连接就是关闭会话。
	
	浏览器点击链接键入网站后多个超链接后关闭浏览器就是一次会话。


### 9.2 会话技术分类

1. cookie技术

   cookie是客户端的技术,程序会把每个用户的数据以cookie的形式报保存到各自的浏览器当中,当用户再次访问服务器中的web资源时候就带着数据去访问的,这样web资源处理的就是每个用户自己的信息了。

2. session技术

   session是服务器的技术,服务器为每个用户创建独有的session对象。由于session为用户浏览器独享,所以用户在访问服务器时就可以把数据放在session中。当用户再次访问时就可以从自己的session中取出数据。 



## 10_Cookie相关 

### 10.1相关方法(切记cookie写入不支持空格等)

```java
Cookie[] getCookies()     //HttpServletRequest获取cookie
Cookie(String name, String value)  //Cookie的构造方法
void addCookie(Cookie cookie)  //HttpServletResponse回写cookie
String getValue()		//获得cookie的值
void setDomain(String pattern)  	//设置cookie的有效域名
void setPath(String uri)  		//设置cookie有效路径
void setMaxAge(int expiry) 	//设置cookie有效时长
```
### 10.2 使用cookie的细节

1. 一个cookie只能标识一种信息和值(也就是说类似于map键值对形式)
2. 一个web站点可以给浏览器发送多个cookie,一个浏览器也可以存储多个web站点的cookie
3. 浏览器可以存放的cookie大小和数量是有限制的。
4. 创建并发送会浏览器的cookie默认是会话级别的,重新打开就没了。
5. 如果需要手动删除cookie可以把有效时长设置为0。删除的时候必须Path一致否则无法删除。
6. 

### 10.3 使用cookie记录用户访问时间

+ 工具类

  ```java
  package com.Servlet.Demo;
  import javax.servlet.http.Cookie;
  /*
  * 查找cookie工具类
  * */
  public class CookieUtils {
      public static Cookie findCookie(Cookie[] cookies, String name) {
          //放回的cookie是否为空
          if (cookies == null) {
              return null;
          } else {
              //遍历cookies数组
              for (Cookie cookie : cookies) {
                  //比对每一个cookie和name是否相等返回cookie否则null
                  if (name.equals(cookie.getName())) {
                      return cookie;
                  }
              }
              return null;
          }
      }
  }
  ```

+ 实现类
```java
 package com.Servlet.Demo;

 import javax.servlet.ServletException;
 import javax.servlet.http.Cookie;
 import javax.servlet.http.HttpServlet;
 import javax.servlet.http.HttpServletRequest;
 import javax.servlet.http.HttpServletResponse;
 import java.io.IOException;
 import java.net.URLDecoder;
 import java.net.URLEncoder;
 import java.text.SimpleDateFormat;
 import java.util.Date;

 public class CookieDemo1 extends HttpServlet {
     /*
      * Cookie记录用户上次访问时间实现
      * 1. 用户第一次访问:
      *   1. 如果是第一次访问,服务器回显欢迎访问
      *   2. 记录当前时间,存入到Cookie返回给浏览器
      *2. 用户不是第一次访问
      *   1. 从Cookie中获取上次访问的时间,并显示在浏览器中
      *   2. 记录当前访问的时间,写入Cookie中返回给浏览器
      * */
     @Override
     protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
         //判断是否是第一次  从指定Cookie数组中获取指定名称的Cookie
         //从请求中获取Cookie
         Cookie[] cookies = req.getCookies();
         //调用工具类判断和获取用户是否是第一次访问,如果是返回欢迎balabala,不是则返回上次访问的时间
         Cookie cookie = CookieUtils.findCookie(cookies, "lastVisit");
         if (cookie == null) {
             resp.setContentType("text/html;charset=UTF-8");

             resp.getWriter().println("<h1>欢迎来到本网站</h1>");
         } else {
             resp.setContentType("text/html;charset=UTF-8");
             String value = URLDecoder.decode(cookie.getValue());
             resp.getWriter().println("<h1>您上次访问的时间是:" + value + "</h1>");
         }
 //        //如果不是第一次获取时间写入cookie
         SimpleDateFormat df = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");
         String format = df.format(new Date());
         String encode = URLEncoder.encode(format);
         Cookie c = new Cookie("lastVisit", encode);
         
        //设置cookie有效路径
        c.setPath("/WebStudy");
        //设置cookie有效时长  持续时间一小时
        c.setMaxAge(60 * 60);
         
         //写回浏览器
         resp.addCookie(c);
     }
     @Override
     protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
         doGet(req, resp);
     }
 }
```

## 11_Session会话相关

> 相比cookie而言  session将数据存在服务器,默认一个浏览器独占一个session对象。
>
> 他会存储使用范围广是一个域对象。

### 11.1 session作用范围

session是一个域对象,作用范围是一个会话的范围。



### 11.2 主要API

```java
 void setAttribute(String name, Object value)   //向session存入数据
 Object getAttribute(String name)     //从session中获取值
 void removeAttribute(String name)    //移除session
 
```



## 12_Servlet域对象的数据访问范围总结


### 12.1 请求范围(ServletResquest)

1. 何时创建和销毁的

   1. 创建:用户向服务器发送一次请求,服务器就会创建一个Resquest对象
   2. 销毁:服务器对这次请求做出了响应后,服务器就会销毁

2. 如何存取数据

   1. 存取: void setAttribute(String name,Object value)
   2. 获取: Object getAttribute(String name) 

3. 作用范围

   一次请求。(转发就是一次请求,重定向是两次)

### 12.2 会话范围(HttpSession)

1. 何时创建和销毁的

   1. 创建: 服务器第一次调用getSession()方法时候,服务器就创建了

   2. 销毁:  2.1 session过期了:默认三十分钟(可以在tomcat中配置)

      	   2.2 服务器非正常关闭,会销毁。
			
      	   2.3 服务器正常关闭:session会被序列化(可以在tomcat目录下找到)
			
      	   2.4 手动销毁:调用session.invalidate()

2. 如何存取数据

   1. 存取: void setAttribute(String name,Object value)
   2. 获取: Object getAttribute(String name) 

3. 作用范围

   一次会话的范围(即多次请求)

### 12.3 应用范围(ServletContext)

1. 何时创建和销毁的
   1. 创建: 服务器启动时就会为每个web项目创建一个单独的ServletContext对象。
   2. 销毁: 服务器关闭的时候,或者项目从服务器移出的时候。  
2. 如何存取数据

   1. 存取: void setAttribute(String name,Object value)
   2. 获取: Object getAttribute(String name) 

3. 作用范围

   整个应用(多个浏览器都可以操作)



## 13_案例实践

### 13.1 需求分析

<img src="https://gitee.com/thisisafigurebed/figure-bed/raw/master/servletNode/7.png" style="zoom:70%">

# Servlet中的监听器

## 01_Servlet中监听器简介

	servlet中定义了多种类型的监听器,他们监听的事件源分别是:ServletContext,HttpServlet,ServletRequest这三个域对象。

## 02_Servlet监听器的分类

1. 一类: 监听送几个域对象的创建和销毁的监听器(三个)
2. 二类: 监听三个域对象属性变更(包括属性的添加,移除,,替换)的监听器(三个)
3. 三类: 监听HttpSession中JavaBean状态改变(钝化,活化,绑定和解除)的监听器(两个)

## 03_对象的创建和销毁的监听器

### 3.1 ServletContextListener监听器

1. 作用

    + 监听ServletContext域对象创建和销毁的监听器。

2. ServletContext对象

    + 创建:服务器启动时候会对每个web应用创建单独的ServletContext对象
    + 销毁:服务器关闭或者项目从web服务器中移除的时候

3. ServletContextListener监听器的方法

    + 监听创建

  `void contextInitialized(ServletContextEvent sce)  `

    + 监听销毁

  `void contextDestroyed(ServletContextEvent sce)`

4. 代码实现

    + ServletContextListener类
   ```java
   /*
   * 事件源ServletContext
   * 监听器ServletContextListener
   * 事件两个sout
   * 绑定 通过配置绑定
   * */
   public class ServleteContextListenerDemo implements ServletContextListener {
       @Override
       public void contextInitialized(ServletContextEvent sce) {
           System.out.println("ServleteContext对象被创建.....");
       }
       @Override
       public void contextDestroyed(ServletContextEvent sce) {
           System.out.println("ServleteContext对象被销毁.....");
       }
   }
   ```

    +  web.xml
   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
            xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
            xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
            version="4.0">
       <listener>
           <listener-class>com.ServleteContextListenerDemo</listener-class>
       </listener>
   </web-app>
   ```

### 3.2 HttpSessionListener监听器

1. 作用

    + 监听HttpSession对象创建和销毁的监听器。

2. HttpSession对象

    + 创建:服务器第一次调用`getSession()`方法时候
    + 销毁:
        1. 非正常关闭服务器(正常关闭会被序列化)
        2. Session过期(默认30分钟)
        3. 手动调用`session.invalidate()`方法


3. HttpSessionListener监听器的方法

    + 监听创建

  `void sessionCreated(HttpSessionEvent se)    `

    + 监听销毁

  `void sessionDestroyed(HttpSessionEvent se)  `

4. session对象创建时机

    1. 访问html: 不会
    2. 访问jsp:  会
    3. 访问servlet: 不会

5. 代码实现

    + `HttpSessionListenerDemo`类
    
   ```java
   public class HttpSessionListenerDemo implements HttpSessionListener {
       @Override
       public void sessionCreated(HttpSessionEvent se) {
           System.out.println("监听器被创建......");
       }
       @Override
       public void sessionDestroyed(HttpSessionEvent se) {
           System.out.println("监听器被销毁......");
       }
   }
   
   ```
   
    + web.xml
   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
            xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
            xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
            version="4.0">
       <listener>
           <listener-class>com.HttpSessionListenerDemo</listener-class>
       </listener>
           <!--设置session1分钟过期-->
    <session-config>
        <session-timeout>1</session-timeout>
    </session-config>
   </web-app>
   ```

### 3.3 ServletRequestListener监听器

1. 作用

	+ 监听ServletRequest域对象创建和销毁的监听器。

2. ServletRequest对象

    + 创建:客户端向服务器发送一次请求
    + 销毁:服务器对客户端做出一次响应

3. ServletRequestListener监听器的方法

    + 监听创建

      `void requestInitialized(ServletRequestEvent sre)`

    + 监听销毁

      `void requestDestroyed(ServletRequestEvent sre)  `

4. servletRequest对象创建时机

    1. 访问html: 会
    2. 访问jsp:  会
    3. 访问servlet: 会

5. 代码实现

    + ServletContextListener类

   ```java
   public class ServletRequestListenerDemo implements ServletRequestListener {
       @Override
       public void requestDestroyed(ServletRequestEvent sre) {
           System.out.println("监听器被创建......");
       }
       @Override
       public void requestInitialized(ServletRequestEvent sre) {
           System.out.println("监听器被销毁......");
       }
   }
   ```
   
    + web.xml
   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
            xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
            xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
            version="4.0">
       <listener>
           <listener-class>com.ServletRequestListenerDemo</listener-class>
       </listener>
   </web-app>
   ```

## 04_统计在线人数示例

1. OnlineCountServletContextListener.java

   ```java
   public class OnlineCountServletContextListener implements ServletContextListener {
       @Override
       public void contextInitialized(ServletContextEvent sce) {
           sce.getServletContext().setAttribute("count", 0);
       }
       @Override
       public void contextDestroyed(ServletContextEvent sce) {
       }
   }
   ```

2. OnlineCountHttpSessionListener.java

   ```java
   public class OnlineCountHttpSessionListener implements HttpSessionListener {
       @Override
       public void sessionCreated(HttpSessionEvent se) {
           //在线了
           //获得原来的值+1
           HttpSession session = se.getSession();
           int a = (Integer)session.getServletContext().getAttribute("count");
           a++;
           session.getServletContext().setAttribute("count",a);
       }
       @Override
       public void sessionDestroyed(HttpSessionEvent se) {
           //离线了
           HttpSession session = se.getSession();
           int a = (Integer)session.getServletContext().getAttribute("count");
           a--;
           session.getServletContext().setAttribute("count",a);
       }
   }
   ```

3. index.jap

   ```jsp
   <%@ page contentType="text/html;charset=UTF-8" language="java" %>
   <%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
   <html>
   <head>
       <title>list</title>
   </head>
   <body>
   <h1>在线人数:${applicationScope.count}</h1>
   </table>
   </body>
   </html>
   ```

4. xml自己写

## 05_对象属性变更监听器

1. ServletContextAttributeListener
   1. 作用

   + 监听ServletContext对象属性变更(添加,移出,替换)的监听器

   2. 方法

   + void attributeAdded(ServletContextAttributeEvent scab)
   + void attributeRemoved(ServletContextAttributeEvent scab)  
   + void attributeReplaced(ServletContextAttributeEvent scab)  

2. HttpSessionAttributeListener

   1. 作用

   + 监听HttpSession对象属性变更(添加,移出,替换)的监听器

   2. 方法

   + void attributeAdded(HttpSessionBindingEvent se)
   + void attributeRemoved(HttpSessionBindingEvent se)  
   + void attributeReplaced(HttpSessionBindingEvent se)  

3. ServletRequestAttributeListener

   1. 作用

   + 监听ServletRequest对象属性变更(添加,移出,替换)的监听器

   2. 方法

   + void attributeAdded(ServletRequestAttributeEvent srae)
   + void attributeRemoved(ServletRequestAttributeEvent srae)  
   + void attributeReplaced(ServletRequestAttributeEvent srae) 
   
4. 代码演示

   1. ServletContextAttributeListener

   ```java
   public class OnlineCountServletContextListener implements ServletContextAttributeListener {
       @Override
       public void attributeAdded(ServletContextAttributeEvent scab) {
           System.out.println("添加了...");
       }
       @Override
       public void attributeRemoved(ServletContextAttributeEvent scab) {
           System.out.println("移除...");
       }
       @Override
       public void attributeReplaced(ServletContextAttributeEvent scab) {
           System.out.println("替换...");
       }
   }
   ```

   2. jsp

   ```jsp
   <%@ page contentType="text/html;charset=UTF-8" language="java" %>
   <%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
   <html>
   <head>
       <title>list</title>
   </head>
   <body>
   <%
       pageContext.getServletContext().setAttribute("a", 1);
       pageContext.getServletContext().setAttribute("a", 4);
       pageContext.getServletContext().removeAttribute("a");
   %>
   </table>
   </body>
   </html>
   ```

   3.xml自己写

## 06_java类状态改变的监听器

> 把java对象添加进属性就是绑定,普通对象添加进属性就是添加属性

1. 概述

   	保存在session域中的java类可以有多种状态:绑定到session中,从session中解除绑定,随session对象持久化到一个存储设备中(钝化),随session对象从存储设备中恢复(活化)。就是序列化

      	session对象中定义了两个特殊的监听接口来帮助java类了解自己在session域中的状态。

    + HttpSessionBindingListener接口

    + HttpSessionActivationListener接口

      实现这两个接口不需要在xml中配置。

2. HttpSessionBindingListener监听器

   1. 绑定状态

      void valueBound(HttpSessionBindingEvent event)  

   2. 未绑定状态

      void valueUnbound(HttpSessionBindingEvent event)  

   3. 代码演示

   + HttpSessionBindingListener1.java

     ```java
     public class HttpSessionBindingListener1 implements HttpSessionBindingListener {
         @Override
         public void valueBound(HttpSessionBindingEvent event) {
             System.out.println("绑定了...");
         }
         @Override
         public void valueUnbound(HttpSessionBindingEvent event) {
             System.out.println("解除了...");
         }
     }
     ```

   + index.jsp

     ```jsp
     <%@ page contentType="text/html;charset=UTF-8" language="java" %>
     <%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
     <html>
     <head>
         <title>list</title>
     </head>
     <body>
     <%
         HttpSessionBindingListener1 h =  new HttpSessionBindingListener1();
         //事件绑定  设置域对象将
         session.setAttribute("h",h);
         session.removeAttribute("h");
     %>
     </table>
     </body>
     ```

3. HttpSessionActivationListener监听器

   > 监听HttpSession钝化和活化状态的监听器。
   >
   > 服务器内存不够了，把最近不活动的session序列化到磁盘，在序列化之前你会收到监听事件
   > 如果这个钝化的用户某个时候又访问了，服务器在内存没找到session，就去磁盘找，再反序列化到内存，这个时候你又会收到监听事件
   > 你放入session的一切变量都必须是可序列化的，否则失败

   1. 活化

      void sessionDidActivate(HttpSessionEvent se)  

   2. 钝化

      void sessionWillPassivate(HttpSessionEvent se)  

   3. 代码演示

   + HttpSessionActivationListener1.java

     ```java
     public class HttpSessionActivationListener1 implements HttpSessionActivationListener, Serializable {
         private String name ;
     
         public String getName() {
             return name;
         }
     
         public void setName(String name) {
             this.name = name;
         }
     
         @Override
         public void sessionWillPassivate(HttpSessionEvent se) {
             System.out.println("被session钝化了");
         }
     
         @Override
         public void sessionDidActivate(HttpSessionEvent se) {
             System.out.println("被session活化了");
         }
     }
     ```

   + index.jsp

     ```jsp
     <%@ page contentType="text/html;charset=UTF-8" language="java" %>
     <%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
     <html>
     <head>
         <title>list</title>
     </head>
     <body>
     <%
         HttpSessionActivationListener1 h =  new HttpSessionActivationListener1();
         //事件绑定  设置域对象将
         h.setName("张三");
         session.setAttribute("h",h);
     %>
     </table>
     </body>
     </html>
     ```

4. 高级配置

    服务器内存不够了，把最近不活动的session序列化到磁盘，在序列化之前你会收到监听事件
如果这个钝化的用户某个时候又访问了，服务器在内存没找到session，就去磁盘找，再反序列化到内存，这个时候你又会收到监听事件。

	context.xml有三种不同的位置,表示不同的序列化范围,用时可以百度。

	web下新建META-INF文件夹新建context.xml写入如下内容:

```xml
<?xml version='1.0' encoding='UTF-8'?>
<Context>
    <Manager className="org.apache.catalina.session.PersistentManager" maxIdleSwap="1">
        <Store className="org.apache.catalina.session.FileStore" directory="/web"/>
    </Manager>
</Context>

```

	其中maxIdleSwap=1设置钝化时间为一分钟,directory=钝化的路径。

即就是一分钟未操作,session序列化到硬盘里,重新发出session时反序列化出来.

其他代码不变。



# Filter过滤器

## 01_什么是Filter

	Filter也称之为过滤器，它是Servlet技术中最实用的技术，Web开发人员通过Filter技术，对web服务器管理的所有web资源(JSP,Servlet,静态图片或者静态html文件)进行拦截,从而实现一些特殊功能。
	
	Filter就是过滤从客户端向服务器发送的请求。

## 02_Filter入门

1. step1:编写一个类实现Filter接口

   ```java
   public class FileDemo1 implements Filter {
       @Override
       public void init(FilterConfig filterConfig) throws ServletException {
       }
       @Override
       public void doFilter(ServletRequest servletRequest, ServletResponse servletResponse, FilterChain filterChain) throws IOException, ServletException {
           System.out.println("FIlterDemo1执行了...");
           //放行
           filterChain.doFilter(servletRequest, servletResponse);
       }
       @Override
       public void destroy() {
       }
   }
   ```

2. 对过滤器进行配置

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
            xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
            xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
            version="4.0">
       <filter>
           <filter-name>FilterDemo1</filter-name>
           <filter-class>com.Filter.FileDemo1</filter-class>
       </filter>
       <filter-mapping>
           <filter-name>FilterDemo1</filter-name>
   <!--   表示拦截所有请求     -->
           <url-pattern>/*</url-pattern>
       </filter-mapping>
   </web-app>
   ```

3. 测试index.jsp

   不加入放行代码,页面无显示。加入放行代码后页面显示。

## 03_FilterChain对象

	FilterChain:过滤器链,开发过程中多个filter组合起来就是一个过滤器链。
	
	web服务器根据filter注册顺序(mapping的顺序)来决定FilterChain的执行顺序,如果没有下一个过滤器了就会对用目标资源。响应资源会根据filter注册顺序的逆序响应会浏览器。

### 3.1 FilterChain入门

1. 新建FilterDemo1.java

2. 新建FilterDemo2.java
3. 新建FilterDemo3.java

4. 分别按顺序写入三个xml文件

5. jsp输出任意东西

6. 实现图
   <img src="https://gitee.com/thisisafigurebed/figure-bed/raw/master/servletNode/8.png" style="zoom:70%">

## 04_Filter什么周期

filter创建和销毁是由web服务器决定的。web应用程序启动时候会创建Filter实例对象。并调用init()方法。(Filter之后创建一次init一次)。

每次过滤器进行拦截时候都会执行doFilter()方法。

当服务器关闭时,或者应用从服务器移除时候会销毁。

## 05_FilterConfig对象

用来获得Filter相关配置的对象。

### 5.1 常用方法

```java
 String getFilterName()      	//获取过滤器名称
 String getInitParameter(String name) 	//获取过滤器初始化参数值
 Enumeration getInitParameterNames() 	//获取过滤器初始化参数名
 ServletContext getServletContext()  
```
<img src="https://gitee.com/thisisafigurebed/figure-bed/raw/master/servletNode/9.png" style="zoom:70%">

## 06_Filter过滤器的配置

> 下面都是mapping的配置

### 6.1 `<url-pattern>`配置

+ 完全路径匹配: 以/开始,   比如/aa /bb
+ 目录匹配:  以/开始以*结束    比如 /aa/星号
+ 拓展名匹配:  不能以/开始,以*开始     比如:星.jsp  

### 6.2 `<servlet-name>`配置

按照servlet名称拦截servlet(也可以使用上面的路径代替)

### 6.3 `<dispatcher>`的配置

+ 默认情况下过滤器拦截的是请求。如果进行转发(需要拦截这次转发)

+ dispatcher取值
  + REQUEST: 拦截请求
  + FORWD:  拦截转发  
  + INCLUDE:  拦截页面包含
  + ERROR:   拦截错误
<img src="https://gitee.com/thisisafigurebed/figure-bed/raw/master/servletNode/10.png" stylr="zoom:70%">

## 07_案例:权限验证过滤器

网站上有登录功能,登录成功后跳转到后台页面。如果用户直接访问后台页面也是可以的,需要使用filter过滤器拦截。没有登录的用户非法访问后台跳转回登录页面。如果已经登录就放行。

1. 数据库准备

   ```mssql
   create database web05;
   use web05;
   create table user(
   id int primary key auto_increment,
   username varchar(20),
   password varchar(20)
   );
   insert into user values(null,"aaa","321321");
   insert into user values(null,"bbb","321321");
   ```

2. 建立路径导包
   <img src="https://gitee.com/thisisafigurebed/figure-bed/raw/master/servletNode/11.png" style="zoom:50%">

3. domain/User.java实体类

   ```java
   package damain;
   public class User {
       private String username;
       private String password;
       public User() {
       }
       public User(String username, String password) {
           this.username = username;
           this.password = password;
       }
       public String getUsername() {
           return username;
       }
       public void setUsername(String username) {
           this.username = username;
       }
       public String getPassword() {
           return password;
       }
       public void setPassword(String password) {
           this.password = password;
       }
   }
   ```

4. UserServlet.java处理用户登录

   ```java
   /*
   * 用户登录
   * */
   public class UserServlet extends HttpServlet {
       @Override
       protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
          doPost(req, resp);
       }
       @Override
       protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
           try {
               //获取数据
               String username = req.getParameter("username");
               String password = req.getParameter("password");
               //封装数据
               User user = new User();
               user.setUsername(username);
               user.setPassword(password);
               //处理数据
               UserModel userModel = new UserModel();
               User existUser = userModel.login(user);
               //页面跳转
               if (existUser == null) {
                   //登录失败
                   req.setAttribute("msg", "用户名或密码错误");
                   req.getRequestDispatcher("/Login/login.jsp").forward(req, resp);
               } else {
                   //登录成功
                   req.getSession().setAttribute("existUser", existUser);
                   resp.sendRedirect(req.getContextPath()+"/jsp/success.jsp");
               }
           } catch (Exception e) {
               e.printStackTrace();
           }
       }
   }
   ```

5. UserModel.java用户模型层

   ```java
   /*
   * 用户模型层
   * */
   public class UserModel {
       public User login(User user) throws SQLException {
           QueryRunner queryRunner = new QueryRunner(jdbcUtils.getDataSource());
           String sql = "select * from user where username = ? and password = ?";
           User existUser = queryRunner.query(sql, new BeanHandler<User>(User.class), user.getUsername(), user.getPassword());
           return existUser;
       }
   }
   ```

6. PrivilegeFilter.java权限过滤器

   ```java
   /*
    *权限验证过滤器
    *
    * */
   public class PrivilegeFilter implements Filter {
       @Override
       public void init(FilterConfig filterConfig) throws ServletException {
       }
       @Override
       public void destroy() {
       }
       @Override
       public void doFilter(ServletRequest servletRequest, ServletResponse servletResponse, FilterChain filterChain) throws IOException, ServletException {
           //判断用户已经登录成功 放行 未登录返回登录界面
           HttpServletRequest req = (HttpServletRequest) servletRequest;
           User user = (User)req.getSession().getAttribute("existUser");
           if (user == null) {
               req.getSession().setAttribute("msg", "您还没有登录");
               req.getRequestDispatcher("/Login/login.jsp").forward(servletRequest,servletResponse);
           } else {
               filterChain.doFilter(servletRequest,servletResponse);
           }
       }
   }
   ```

7. xml文件

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
            xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
            xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
            version="4.0">
       <servlet>
           <servlet-name>UserServlet</servlet-name>
           <servlet-class>controller.UserServlet</servlet-class>
       </servlet>
       <servlet-mapping>
           <servlet-name>UserServlet</servlet-name>
           <url-pattern>/UserServlet</url-pattern>
       </servlet-mapping>
       <filter>
           <filter-name>PrivilegeFilter</filter-name>
           <filter-class>Filter.PrivilegeFilter</filter-class>
       </filter>
       <filter-mapping>
           <filter-name>PrivilegeFilter</filter-name>
           <url-pattern>/jsp/*</url-pattern>
       </filter-mapping>
   </web-app>
   ```

8. login.jsp

   ```jsp
   <%@ page contentType="text/html;charset=UTF-8" language="java" %>
   <html>
   <head>
       <title>login</title>
   </head>
   <body>
   <font color="red">${msg}</font>
   <h1>登录页面</h1>
   <form action="${pageContext.request.contextPath}/UserServlet" method="post">
       <table border="1" width="400">
           <tr>
               <td>用户名:</td>
               <td><input type="text" name="username" /></td>
           </tr>
           <tr>
               <td>密码:</td>
               <td><input type="password" name="password" /></td>
           </tr>
           <tr>
               <td colspan="2"><input type="submit" value="提交">
               </td>
           </tr>
       </table>
   </form>
   </body>
   </html>
   ```

9. success.jsp

   ```jsp
   <%@ page contentType="text/html;charset=UTF-8" language="java" %>
   <html>
   <head>
       <title>Title</title>
   </head>
   <body>
   <h1>${existUser.username}您已经登录成功</h1>
   </body>
   </html>
   ```

   ## 07_案例:通用字符集编码过滤器

   > 浏览器向后台提交数据时候有可能提交中文会乱码 (可能是get可能是post)

### 7.1 增强request中的getParameter方法

	增强的过程要写在过滤器中
	
	增强使用装饰者模式;
<img src="https://gitee.com/thisisafigurebed/figure-bed/raw/master/servletNode/12.png" style="zoom:50%">

1. sub.jsp

   ```jsp
   <%@ page contentType="text/html;charset=UTF-8" language="java" %>
   <html>
   <head>
       <title>login</title>
   </head>
   <body>
   <form action="${pageContext.request.contextPath}/UserServlet" method="get">
       <h1>GET方式:</h1><br/>
       姓名: <input type="text" name="name"/> <input type="submit" value="提交"><br/>
   </form>
   <form action="${pageContext.request.contextPath}/UserServlet" method="post">
   
       <h1>POST方式</h1><br/>
       姓名: <input type="text" name="name"/> <input type="submit" value="提交"><br/>
   </form>
   </body>
   </html>
   ```

2. UserServlet.java

   ```java
   public class UserServlet extends HttpServlet {
       @Override
       protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
           String n = req.getParameter("name");
           System.out.println("Get方式提交"+n);
       }
       @Override
       protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
           String n1 = req.getParameter("name");
           System.out.println("Post方式提交"+n1);
       }
   }
   ```

3. GenericEncodingFilter.java

   ```java
   public class GenericEncodingFilter implements Filter {
       @Override
       public void init(FilterConfig filterConfig) throws ServletException {
       }
       @Override
       public void doFilter(ServletRequest servletRequest, ServletResponse servletResponse, FilterChain filterChain) throws IOException, ServletException {
           //在过滤器中增强request对象,并将增强后的request对象传递回servlet
           HttpServletRequest req = (HttpServletRequest) servletRequest;
           //增强原有的request
          MyHttpServletRequest myHttpServletRequest = new MyHttpServletRequest(req);
           filterChain.doFilter(myHttpServletRequest,servletResponse);
       }
       @Override
       public void destroy() {
       }
   }
   ```

4. MyHttpServletRequest.java

   ```java
   /*
    * 增强类
    *
    * */
   public class MyHttpServletRequest extends HttpServletRequestWrapper {
       private HttpServletRequest request;
   
       public MyHttpServletRequest(HttpServletRequest request) {
           super(request);
           this.request = request;
       }
       //增强request.getParameter方法
       @Override
       public String getParameter(String name) {
           String method = request.getMethod();
           if ("GET".equals(method)) {
               String value = super.getParameter(name);
               try {
                   value = new String(value.getBytes("ISO-8859-1"), "UTF-8");
               } catch (UnsupportedEncodingException e) {
                   e.printStackTrace();
               }
               return value;
   
           } else if ("POST".equals(method)) {
               try {
                   request.setCharacterEncoding("UTF-8");
               } catch (Exception e) {
                   e.printStackTrace();
               }
           }
           return super.getParameter(name);
       }
   }
   ```

5. xml

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
            xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
            xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
            version="4.0">
       <servlet>
           <servlet-name>UserServlet</servlet-name>
           <servlet-class>controller.UserServlet</servlet-class>
       </servlet>
       <servlet-mapping>
           <servlet-name>UserServlet</servlet-name>
           <url-pattern>/UserServlet</url-pattern>
       </servlet-mapping>
       <filter>
           <filter-name>GenericEncodingFilter</filter-name>
           <filter-class>Filter.GenericEncodingFilter</filter-class>
       </filter>
       <filter-mapping>
           <filter-name>GenericEncodingFilter</filter-name>
           <url-pattern>/*</url-pattern>
       </filter-mapping>
   </web-app>
   ```


# 文件上传

## 01_常用技术

1. JspSmartUpload		应用在Jsp上的文件上传和下载
2. FileUpload                 应用在java坏境上的文件上传功能。 (常用)
3. Servlet3.0                  提供了文件上传
4. Struts2                       框架提供了文件上传

## 02_文件上传要素

1. 表单提交必须是post
2. 表单中需要有文件上传组件还需要有name属性和值<input type="file">
3. 表单 enctype="multipart/form-data"

## 03_入门案例

1. 导包
   <img src="13.png" style="zoom:50%">
   
2. jsp页面

   ```jsp
   <%@ page contentType="text/html;charset=UTF-8" language="java" %>
   <html>
     <head>
       <title>$Title$</title>
     </head>
     <body>
     <form action="${ pageContext.request.contextPath}/UploadServlet" method="post" enctype="multipart/form-data">
       文件描述:<input type="text" name="info" /><br>
       文件上传:<input type="file" name="upload" /><br>
       <input type="submit" value="上传">
     </form>
     </body>
   </html>
   ```

3. servlet(值得一提使用注解可以不用配置xml    @WebServlet(name = "UploadServlet", value = "/UploadServlet"))

   ```java
   @WebServlet(name = "UploadServlet", value = "/UploadServlet")
   public class UploadServlet extends HttpServlet {
       @Override
       protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
           doPost(request, response);
       }
       @Override
       protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
           try {
               // 1. 创建磁盘文件工厂
               DiskFileItemFactory diskFileItemFactory = new DiskFileItemFactory();
               // 2. 创建核心的解析类
               ServletFileUpload servletFileUpload = new ServletFileUpload(diskFileItemFactory);
               // 3. 利用核心类解析Resquest,解析后得到多个部分。返回list集合。集合中为封装的FileItem文件项
               List<FileItem> list = null;
               list = servletFileUpload.parseRequest(request);
               // 4. 遍历list集合,得到每个FileItem文件项。根据FileItem判断是否是文件上传项。
   
               for (FileItem fi : list) {
                   //判断是普通项还是文件上传项
                   if (fi.isFormField()) {
                       //判断表单项
                       //接收普通项的值(接收值不能再使用request.getParameter())
                       String name = fi.getFieldName();//获得普通项名称
                       String value = fi.getString("UTF-8");
                       System.out.println(name + "     " + value);
                   } else {
                       //文件上传项目
                       //获得上传项名
                       String name = fi.getName();
                       //获得文件内容
                       InputStream is = fi.getInputStream();
                       //写入到upload文件夹中  (因为部署后文件路径会变所以要获得绝对磁盘路径)
                       String path = getServletContext().getRealPath("/WEB-INF/upload");
                       //创建一个输出流,写入到设置的路径中。
                       File savePathDir = new File(path);
                       if (!savePathDir.exists()) {
                           savePathDir.mkdir();
                       }
                       FileOutputStream os = new FileOutputStream(path + "/" + name);
                       request.setAttribute("path",path + "/" + name);
                       System.out.println(path + "/" + name);
                       //两个流对接
                       int len = 0;
                       byte[] b = new byte[1024];
                       while ((len = is.read(b)) != -1) {
                           os.write(b, 0, len);
                       }
                       is.close();
                       os.close();
                   }
               }
           } catch (FileUploadException e) {
               e.printStackTrace();
           }
       }
   }
   ```

4. 注意写入的文件目录在(3)中的path变量中

## 04_文件上传的API

### 4.1 DiskFileItemFactory:磁盘工厂

1. 构造方法

   ```java
   public DiskFileItemFactory()
   public DiskFileItemFactory(int sizeThreshold,File repository)
         采用参数指定临界值和系统临时文件夹构造文件项工厂对象。
   ```

2. 方法:

   + void setSizeThreshold(int sizeThreshold)  设置缓冲区大小
   + void setRepository(File repository)   设置临时文件存放位置

### 4.2 ServletFileUpload: 核心解析类

1. 构造方法

   ```java
   ServletFileUpload()
   ServletFileUpload(FileItemFactoey fileItemFactory)
   ```

2. 方法

   + static boolean isMultipartContent(javax.servlet.http.HttpServletRequest request)

     用来判断表单的enctype属性是否正确(是否为enctype="multipart/form-data")

   + java.util.List parseRequest(javax.servlet.http.HttpServletRequest request)

     返回list集合(每个部分的对象FileItem)

   + void setFileSizeMax(long fileSizeMax)

     设置单个文件大小

   + void setSizeMax(long sizeMax)

     设置总文件大小

   + void setProgressListener(ProgressListener pListener)

     监听文件上传进度

   + void setHeaderEncoding(java.lang.Strig encoding)

     解决文件上传中文乱码问题

### 4.3 FileItem:文件项

1. 构造方法

   无构造方法因为他是解析request得到的

2. 方法

   + boolean isFormField()

     判断是普通项还是文件上传项

   + java.lang.String getFileName()

     得到普通项的名

   + java.lang.String getString()

     获得普通项目值

   + java.lang.String getString(java.lang.String encoding)

     解决普通项中文乱码

   + java.lang.String getName()

     获得文件项文件名

   + java.io.InputStream getInputStream()

     获得文件内容方法

   + long getSize()

     获得文件上传的大小

   + void delete()

     用于删除缓冲区的文件(磁盘工厂)

   

## 05_多文件上传案例

> 后端代码和上面一样,只有前端js不一样

```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>Title</title>

    <script>
        function add(){
            var div1Element = document.getElementById("div1");
            div1Element.innerHTML += "<div><input type='file' name='upload' /><input type='submit'  value='删除' onclick='del(this)' /></div> ";
        }
        function del(who){
            who.parentNode.parentNode.removeChild(who.parentNode);
        }
    </script>
</head>
<body>
<h1>多文件上传</h1>
<form action="${ pageContext.request.contextPath}/UploadServlet" method="post" enctype="multipart/form-data">
    <input type="button" value="添加" onclick="add()" />
    <input type="submit"  value="提交" /><br>
    <div id="div1">
    </div>
</form>
</body>
</html>
```

## 06_文件上传遇到的问题

### 6.1 文件上传浏览器兼容问题

1. 问题描述

   Chrome点击上传时候会显示文件名(正常)

   IE等老版本浏览器显示的是整个文件的路径(不正常)

   因为后面需要依据文件名在磁盘新建文件所以如果含有路径的文件名会新建失败

2. 解决

   在else后获得文件名的String字符串后加入

   ```java
   else {
       //文件上传项目
       //获得上传项名
       String name = fi.getName();
       //兼容老版本浏览器去除路径名只要文件名
       //检索最后一个\字符出现的索引
       int index = name.lastIndexOf("\\");
       //如果\字符不等于-1则说明有\字符即老版本浏览器
       if (index != -1) {
           //使用subString取出最后一个\的索引(+1则去掉\)重新赋值给name
           name = name.substring(index + 1);
       }
   ```
### 6.2 文件上传同路径文件名重复

1. 问题描述

   磁盘目录中存在一个已经存在的文件,则会覆盖

2. 解决

   新建一个工具类UploadUtils返回一个唯一文件名



### 6.3 同一目录中文件过多

1. 问题描述

   同一目录中文件过导致索引过慢

2. 目录分离

   1. 按时间分离: 月周天小时
   2. 用户分离: 用户名分离
   3. 个数分离: 达到某个数量后分离
   4. 目录算法分离:(举例其中一种算法)
      1. 上传一个文件,得到一个唯一的文件名
      2. 取其hashCode值。----int类型的值(32位)
      3. 让hashCode值 & 0xf;-----得出的值作为一级目录
      4. 让hashCode值右移四位 & 0xf;----得出的值作为二级目录
      5. 以此类推

3. 算法实现

   1. 在工具类中(实例为二级目录可以更多)

      ```java
      //传入一个文件名,返回一个唯一的文件名
      public class UploadUtils {
          public static String getUuidFilename(String filename) {
              //java中提供了一个API有一个UUid类可以随机生成字符串
              int index = filename.lastIndexOf(".");
              String s = filename.substring(index);
              return UUID.randomUUID().toString().replace("-", "") + s;
          }
          //算法目录分离
          public static String getRealPath(String uuidFilename) {
              //将随机文件名hashcode一下
              int code1 = uuidFilename.hashCode();
              //制作一级目录
              int d1 = code1 & 0xf;
              //右移四位
              int code2 = d1 >>> 4;
              //制作二级目录
              int d2 = code2 & 0xf;
              //返回制作好的目录
              return "/"+d1+"/"+d2;
          }
      }
      ```

   2. servlet中(添加了回车多的地方)

      ```java
      @WebServlet(name = "UploadServlet", value = "/UploadServlet")
      public class UploadServlet extends HttpServlet {
          @Override
          protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
              doPost(request, response);
          }
          @Override
          protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
              try {
                  // 1. 创建磁盘文件工厂
                  DiskFileItemFactory diskFileItemFactory = new DiskFileItemFactory();
                  // 2. 创建核心的解析类
                  ServletFileUpload servletFileUpload = new ServletFileUpload(diskFileItemFactory);
                  servletFileUpload.setHeaderEncoding("UTF-8");
                  // 3. 利用核心类解析Resquest,解析后得到多个部分。返回list集合。集合中为封装的FileItem文件项
                  List<FileItem> list = null;
                  list = servletFileUpload.parseRequest(request);
                  // 4. 遍历list集合,得到每个FileItem文件项。根据FileItem判断是否是文件上传项。
                  for (FileItem fi : list) {
                      //判断是普通项还是文件上传项
                      if (fi.isFormField()) {
                          //判断表单项
                          //接收普通项的值(接收值不能再使用request.getParameter())
                          String name = fi.getFieldName();//获得普通项名称
                          String value = fi.getString("UTF-8");
                          System.out.println(name + "     " + value);
                      } else {
                          //文件上传项目
                          //获得上传项名
                          String name = fi.getName();
                          //兼容老版本浏览器去除路径名只要文件名
                          //检索\字符的个数
                          int index = name.lastIndexOf("\\");
                          //如果\字符不等于-1则说明有\字符即老版本浏览器
                          if (index != -1) {
                              //使用subString取出最后一个\之后的字符重新赋值给name
                              name = name.substring(index + 1);
                          }
      
      
                          name = UploadUtils.getUuidFilename(name);
      
      
                          //获得文件内容
                          InputStream is = fi.getInputStream();
                          //写入到upload文件夹中  (因为部署后文件路径会变所以要获得绝对磁盘路径)
                          String path = getServletContext().getRealPath("/WEB-INF/upload");
      
                          //目录分离
                          String path1 = UploadUtils.getRealPath(name);
                          String newPath = path + path1;
                          //创建一个输出流,写入到设置的路径中。
                          File savePathDir = new File(newPath);
                          if (!savePathDir.exists()) {
                              savePathDir.mkdirs();
                          }
                          FileOutputStream os = new FileOutputStream(newPath + "/" + name);
      
      
                          //两个流对接
                          int len = 0;
                          byte[] b = new byte[1024];
                          while ((len = is.read(b)) != -1) {
                              os.write(b, 0, len);
                          }
                          is.close();
                          os.close();
                      }
                  }
              } catch (FileUploadException e) {
                  e.printStackTrace();
              }
          }
      }
      ```

# 文件下载

## 01_文件下载的方式

1. 使用超链接方式实现文件下载

   + `<a href="#">点击即可下载</a>`
   + 注意:如果浏览器不能识别这种格式的文件会提示下载,支持的话会直接打开

2. 通过手动编写代码的方式实现文件的下载

   + 设置两个头和一个流

     + Content-Type	文件的MIME类型

     + Content-Disposition    无论浏览器是否支持都会下载
     + 设置代表该文件的输入流(因为输入流是固定的response.getOutputStream())

## 02_手动编码的方式进行下载

1. jsp页面

   ```jsp
   <%@ page contentType="text/html;charset=UTF-8" language="java" %>
   <html>
   <head>
       <title>Title</title>
   </head>
   <body>
   <h1>文件下载示例</h1>
   <a href="${pageContext.request.contextPath}/DownloadServlet?filename=a.txt">a.txt</a>&nbsp;&nbsp;
   <a href="${pageContext.request.contextPath}/DownloadServlet?filename=aa.zip">aa.zip</a>
   <h1>l;ala</h1>
   <a href="${pageContext.request.contextPath}/download/a.txt">a.txt</a>&nbsp;&nbsp;
   <a href="${pageContext.request.contextPath}/download/aa.zip">aa.zip</a>
   <br>
   </body>
   </html>
   ```

2. servlet代码(注意文件路径是部署路径)

   ```java
   @WebServlet(name = "DownloadServlet", value = "/DownloadServlet")
   public class DownloadServlet extends HttpServlet {
       @Override
       protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
           doPost(req, resp);
       }
       @Override
       protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
           // 1. 接收参数
           String filename = req.getParameter("filename");
   
           // 2.实现文件下载(两个头一个流)
           //设置Content-Type头
           //拿到文件的MIME值
           String type = getServletContext().getMimeType(filename);
           //设置下载文件的MIME值
           resp.setContentType(type);
   
           //设置Content-Disposition头
           //里面双引号内是固定的
           resp.setHeader("Content-Disposition", "attachment;filename=" + filename);
   
           //设置一个代表文件的输入流
           String path = getServletContext().getRealPath("/download");
           var is = new FileInputStream(path + "/" +filename);
           OutputStream os = resp.getOutputStream();
           //两个流对接
           int len = 0;
           byte[] bytes = new byte[1024];
           while ((len = is.read(bytes))!= -1) {
               os.write(bytes, 0, len);
           }
           is.close();
       }
   }
   ```


## 03_遇到的问题

1. 中文文件下载乱码

2. 解决思路

   + 不同浏览器编码方式不一样
   + IE使用的URL编码
   + 火狐使用Base64编码
   + 判断客户端使用的浏览器类型
   + 请求头User-Agent可以获得客户端浏览器的信息

3. 代码实现

   1. 新建Base64工具类

      ```java
      public class DownUtils {
          public static String base64EncodeFileName(String fileName) {
              BASE64Encoder base64Encoder = new BASE64Encoder();
              try {
                  return "=?UTF-?8B?" + new String(base64Encoder.encode(fileName.getBytes("UTF-8"))) + "?=";
              } catch (UnsupportedEncodingException e) {
                  e.printStackTrace();
              }
              return "10";
          }
      }
      ```

   2. servlet

      ```java
      @WebServlet(name = "DownloadServlet", value = "/DownloadServlet")
      public class DownloadServlet extends HttpServlet {
          @Override
          protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
              doPost(req, resp);
          }
          @Override
          protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
              // 1. 接收参数
              String filename = new String(req.getParameter("filename").getBytes("ISO-8859-1"),"UTF-8") ;
              // 2.实现文件下载(两个头一个流)
              //设置Content-Type头
              //拿到文件的MIME值
              String type = getServletContext().getMimeType(filename);
              //设置下载文件的MIME值
              resp.setContentType(type);
      
              //因为后面还需要用到源码的filename所以提前
              //设置一个代表文件的输入流
              String path = getServletContext().getRealPath("/download");
              //定义一个代表该文件路径
              var file = new File(path + "/" +filename);
              //判断浏览器类型
              String agent = req.getHeader("User-Agent");
              if (agent.contains("Firefox")) {
                  DownUtils.base64EncodeFileName(filename);
              } else {
                  URLEncoder.encode(filename, "UTF-8");
              }
              //设置Content-Disposition头
              //里面双引号内是固定的
              resp.setHeader("Content-Disposition", "attachment;filename=" + filename);
              var is = new FileInputStream(file);
      
      
              OutputStream os = resp.getOutputStream();
              //两个流对接
              int len = 0;
              byte[] bytes = new byte[1024];
              while ((len = is.read(bytes))!= -1) {
                  os.write(bytes, 0, len);
              }
              is.close();
          }
      }
      ```

      

## 04_案例

### 4.1 介绍

将一个文件夹下的所有文件列出来提供下载

### 4.2 简介

+ 使用数据结构:树结构进行遍历所有文件夹的子文件夹

+ 从根节点到叶子节点

  <img src="https://gitee.com/thisisafigurebed/figure-bed/raw/master/servletNode/14.png" style="zoom:50%">



### 4.3 代码实现一(超链接列出)

```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>Title</title>
</head>
<body>
<h1>树形遍历</h1>
<%
    // 1. 创建一个队列
    Queue<File> queue = new LinkedList<File>();
    // 2. 先将根节点入列
    File root = new File("E:\\study");
    queue.offer(root);
    // 3. 判断根节点是否为空
    while (!queue.isEmpty()) {
        // 4. 如果不为空则出列
        File file = queue.poll();
        // 5. 获得根节点下所有子节点
        File[] files = file.listFiles();
        // 6. 遍历所有子节点
        for (File f : files) {
            if (f.isFile()) {
                //如果子节点是一个文件(即就是叶子节点)
                %>
<h4><a href="#" ><%= f.getName() %>></a></h4>
<%
            } else {
                //子节点是一个目录 则入列
                queue.offer(f);
            }
        }
    }

%>
</body>
</html>
```

### 4.4代码实现二

1. jsp页面

   ```java
   <%@ page contentType="text/html;charset=UTF-8" language="java" %>
   <html>
   <head>
       <title>Title</title>
   </head>
   <body>
   <h1>树形遍历</h1>
   <%
       // 一般情况下, 这类代码写在 Java 中,而不直接写在 jsp 中,不好维护
       // jsp 一般只负责展示内容,而不负责处理数据
       // 1. 创建一个队列
       Queue<File> queue = new LinkedList<File>();
       // 2. 先将根节点入列
       File root = new File("E:\\study");
       queue.offer(root);
       // 3. 判断根节点是否为空
       while (!queue.isEmpty()) {
           // 4. 如果不为空则出列
           File file = queue.poll();
           // 5. 获得根节点下所有子节点
           File[] files = file.listFiles();
           // 6. 遍历所有子节点
           for (File f : files) {
               if (f.isFile()) {
                   //如果子节点是一个文件(即就是叶子节点)
   %>
   <h4>
       <a href="${pageContext.request.contextPath}/DownloadListServlet?filename=<%= f.getCanonicalPath()%>"><%= f.getName() %>
           ></a></h4>
           ></a></h4>
   <%
               } else {
                   //子节点是一个目录 则入列
                   queue.offer(f);
               }
           }
       }
   
   %>
   </body>
   </html>
   ```

2. servlet(其实差不多)

   ```java
   /*
    * 树形文件下载代码实现
    *
    * */
   @WebServlet(name = "DownloadListServlet", value = "/DownloadListServlet")
   public class DownloadListServlet extends HttpServlet {
   
       protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
           doPost(req, resp);
       }
   
       protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
   //        System.out.println( "sadawd");
           // 接收参数
           String filePath = req.getParameter("filename");
           System.out.println(filePath);
           // 中文转化
           if (filePath != null) {
               filePath = new String(filePath.getBytes("ISO-8859-1"), "UTF-8");
           }
           var file = new File(filePath);
           System.out.println(file.getCanonicalPath());
           //设置两个头一个流
   
           //传回的filename是带路径的需要去除一下  获得文件名
           var filename = file.getName();
           resp.setContentType(getServletContext().getMimeType(filename));
           //设置另一个头
           //判断浏览器类型
           String agent = resp.getHeader("User-Agent");
           if (agent.contains("Firefox")) {
               filename = DownUtils.base64EncodeFileName(filename);
           } else {
               filename = URLEncoder.encode(filename, "UTF-8");
               //URL编码会把空格变成加号
               filename = filename.replace("+", " ");
           }
           resp.setHeader("Content-Dispostition","attachment:filename"+filename);
           //设置输入流和输出流然后对接
           var is = new FileInputStream(filePath);
           var os = resp.getOutputStream();
           int len = 0;
           byte[] bytes = new byte[1024];
           while ((len = is.read(bytes)) != -1) {
               os.write(bytes, 0, len);
           }
           is.close();
       }
   }
   ```

3. 工具类

   ```java
   public class DownUtils {
       public static String base64EncodeFileName(String fileName) {
           BASE64Encoder base64Encoder = new BASE64Encoder();
           try {
               return "=?UTF-?8B?" + new String(base64Encoder.encode(fileName.getBytes("UTF-8"))) + "?=";
           } catch (UnsupportedEncodingException e) {
               e.printStackTrace();
           }
           return "10";
       }
   }
   ```

   