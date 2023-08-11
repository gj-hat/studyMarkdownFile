[TOC]

# 1_JSP

> JSP运行时会被翻译吃Servlet执行

## 01_JSP脚本元素

<%! %>:声明 翻译成Servlet成员的内容。声明变量,方法,内部类。

<%= %>:翻译成 out.print(),在Service方法内部。用于生成HTML页面源码。

<%  %>:嵌入java代码 翻译成service方法内部的代码块。声明变量(局部类),内部类。(不能写方法了)

## 02_注释

1. html注释:jsp存在servlet存在,html也存在。
   + <!--HTML注释-->
2. java注释:翻译成servlet后还存在,但是翻译成html后就不存在了
   + <%//单行注释    %>
   + <%/*多行注释      */%>
   + <%/**文档注释       */ %>

2. jsp注释:翻译成servlet后就不存在了
   + <%--JSP注释--%>

## 03_JSP指令元素

### 3.1 作用

+ 用于指示jsp执行的某些步骤
+ 用于指示jsp表现特定行为

### 3.2 指令元素的语法

+ <%@ 指令名称 属性名称=属性值 属性名称=属性值%>

### 3.3 JSP指令的分类

+ page指令:用于指示jsp页面的设置属性和一些行为
+ include指令:指示jsp包含哪些页面
+ taglib指令:指示JSP页面引入哪些标签库。

## 04_page指令

### 4.1 page指令简介

1. 写法:<%@ page 属性名=属性值 属性名=属性值 %>
2. page指令用来指示JSP文件的全局属性。
3. 属性是可以单独或者多个同时使用。
4. JSP页面中,只要import属性可以出现多次,其他属性只能出现一次。

### 4.2 page指令属性

+ language属性:声明脚本使用的语言。只能是java
+ extends属性:表明JSP编译成Servlet时候所继承的类。默认值继承HttpJspBase
+ session属性:表明jsp中是否可以直接使用session对象(true/false)。
+ buffer属性:标明JSp对客户端输出缓冲区大小的。默认8Kb
+ autoFlush属性: 如果缓冲区大小溢出了,是否自动刷出。默认是true
+ import属性: 引入java包或者java类。
+ contentType属性:表明JSP被浏览器解析或打开时候的默认字符集
+ pageEncoding属性:JSP文件以及翻译的Servlet保存到硬盘上的字符集。
+ isErrorPage属性:处理JSP页面异常
+ errorPage属性:处理JSP页面异常
+ isELIgnored属性:通知JSP是否忽视EL表达式。

## 05_include指令

### 5.1指令简介

+ 写法: <%@ include 属性名=属性值 属性名=属性值 %>
+ 作用: 在JSP页面中静态包含一个文件,同时由该JSP解析包含的文件内容。

### 5.2 指令属性

+ file属性: 指示JSP页面包含的路径。(比如logo页脚等好几张网页都一样的部分可以单独拎出来)

  + logo.jsp

    ```jsp
    <%@ page contentType="text/html;charset=UTF-8" language="java"  %>
    <html>
    <head>
        <title>Title</title>
    </head>
    <body>
    <h1>LOGO</h1>
    </body>
    </html>
    ```

  + FOOT.jsp

    ```jsp
    <%@ page contentType="text/html;charset=UTF-8" language="java" %>
    <html>
    <head>
        <title>Title</title>
    </head>
    <body>
    <h1>FOOT</h1>
    </body>
    </html>
    ```

  + BODY.jsp

    ```jsp
    <%@ page contentType="text/html;charset=UTF-8" language="java" %>
    <html>
    <head>
        <title>Title</title>
    </head>
    <body>
    <%@include file="LOGO.jsp" %>
    <h1>BODY</h1>~
    <%@include file="FOOT.jsp"%>
    </body>
    </html>
    ```

  + 效果显示<img src="https://gitee.com/thisisafigurebed/figure-bed/raw/master/jspNode/1.png" style="zoom:40%">

### 5.3 Include指令原理(静态包含)

简单粗暴的复制到一个jsp中。

<font color=red size=5>注意:</font>

+ 被包含的页面删去结构代码(HTML,TITLE...标签只留下需要的东西)
+ 被包含的页面中定义的变量在包含的页面中还可以使用

## 06_Taglib指令

### 6.1 指令简介

+ 写法: <%@taglib 属性名=属性值  %>
+ 作用: 在JSP页面中引入标签库

### 6.2指令属性

+ uri指令: 引入的标签库的路径
+ prefix指令: 引入标签库的别名

## 07_JSP内置对象

### 7.1 什么是jsp内置对象

可以在<%  %>中直接写不需要创建的对象;

### 7.2 JSP中九大内置对象

1. request对象:	客户端向服务器发送请求对象 
2. response:   服务器发给客户端的响应对象
3. session:    服务器为客户端创建的会话对象
4. application:   代表整个应用(即就是ServletContext对象)
5. out:    向输出流写入内容的对象
6. page:    当前的JSP翻译成Servlet后的对象的引用
7. pageContext:   当前JSP页面的上下文对象
8. config:    本JSP的ServletConfig对象
9. exception:    表示JSP页面运行时长产生的异常对象

### 7.3 JSP内置对象的具体类型

1. request:     HttpServletRequest
2. response:    HttpServletResponse
3. session:     HttpSession
4. application: ServletContext
5. config:      ServletConfig
6. page:        Object
7. pageContext: PageContext
8. out:         JspWriter
9. exception:   THrowbale

### 7.4 pageContext对象的概述

#### 7.4.1简介

可以翻译为页面的上下文对象,代表的是当前页面的运行的一些属性。是javax.servlet.jsp.PageContext的一个实例对象。

#### 7.4.2作用

1. 提供了page范围内的数据存取方法
   + abstract  Object getAttribute(String name) 
   + abstract  void setAttribute(String name, Object value)  
   + abstract  void removeAttribute(String name) 
   + abstract  Object findAttribute(String name) 
2. 通过这个对象可以获得其他8个对象
   + abstract  JspWriter getOut() 
   + abstract  Exception getException() 
   + abstract  Object getPage() 
   + abstract  ServletRequest getRequest() 
   + abstract  ServletResponse getResponse() 
   + abstract  ServletConfig getServletConfig() 
   + abstract  ServletContext getServletContext() 
   + abstract  HttpSession getSession() 

## 08_JSP的四个作用范围

### 8.1四个作用范围概述

1. static int PAGE_SCOPE  

   页面范围,指的是PAGE_SCOPE保存从的数据仅当前页面内有效。

2. static int REQUEST_SCOPE  

   请求范围,客户端发送一次和服务器响应一次。

3. static int SESSION_SCOPE  

   会话范围:每个浏览器向浏览器发送的多次请求,会话结束之内。

4. static int APPLICATION_SCOPE  

   应用范围:整个项目中任意地方。

### 8.2 演示及操作说明

1. demo1定义四个域变量测试其使用范围

2. demo1中调用查看这四个值都可以查看

3. demo1中使用jsp转发,在demo2中只有page对象值读取不到(因为转发是一次请求)

   `request.getRequestDispatcher("/Demo2/demo2.jsp").forward(request, response);`

4. demo1中使用`<a>`标签超链接去demo2,page和request读取不到(超链接不是转发所以request对象也读取不到)

5. 浏览器关闭后直接访问demo2,page,request,session对象都读取不到(关闭浏览器是一次会话)

6. demo1.jsp

   ```jsp
   <%@ page contentType="text/html;charset=UTF-8" language="java" %>
   <html>
   <head>
       <title>Title</title>
   </head>
   <body>
   <%
       //pageContext作用范围
       pageContext.setAttribute("name", "张三");
       //request作用范围  下面两句等价
   //    request.setAttribute("name", "李四");
       pageContext.setAttribute("name","李四",PageContext.REQUEST_SCOPE);
   
       //session作用范围
   //    session.setAttribute("name", "王五");
       pageContext.setAttribute("name", "王五", PageContext.SESSION_SCOPE);
       //application作用范围
   //    application.setAttribute("name", "赵六");
       pageContext.setAttribute("name", "赵六", PageContext.APPLICATION_SCOPE);
   %>
   <h1>测试四个作用范围 下面两句等价</h1>
   <%= pageContext.getAttribute("name") %>
   <%= request.getAttribute("name") %>
   <%= pageContext.getAttribute("name",PageContext.REQUEST_SCOPE)%>><br/>
   
   <%= session.getAttribute("name") %>
   <%= pageContext.getAttribute("name",PageContext.SESSION_SCOPE)%>><br/>
   
   <%= application.getAttribute("name") %>
   <%= pageContext.getAttribute("name",PageContext.APPLICATION_SCOPE)%>><br/>
   
   <a href="demo2.jsp">跳转到demo2</a>
   <%
   //转发到demo2
       request.getRequestDispatcher("/Demo2/demo2.jsp").forward(request, response);
   %>
   </body>
   </html>
   
   ```

7. demo2.jsp

   ```jsp
   <%@ page contentType="text/html;charset=UTF-8" language="java" %>
   <html>
   <head>
       <title>Title</title>
   </head>
   <body>
   <h1>转发后</h1>
   <%= pageContext.getAttribute("name") %>
   <%= request.getAttribute("name") %>
   <%= session.getAttribute("name") %>
   <%= application.getAttribute("name") %>
   </body>
   </html>
   ```
### 8.3 findAttribute方法

#### 8.3.1简介

查找属性的方法:先根据名称小范围查找,如果找到了就返回,找不到就去大一个域的的范围查找。

   `pageContext.findAttribute("属性名")`

## 09_JSP的动作标签

### 9.1 简介

JSP动作标签:用于在JSP页面中提供业务逻辑功能,避免在JSP页面中编写JAVA代码(造成界面难以维护)。

### 9.2 常用的动作标签

写法`<jsp:标签/>`

+ `<jsp:forward/>`:进行请求转发(以前写request.getRequestDispatcher("页面路径").forward(request, response);)
+ `<jsp:include/>`:包含(动态包含)
+ `<jsp:param/>`:传递参数

### 9.3 代码演示

1. 转发

   `<jsp:forward page="demo2.jsp"></jsp:forward>`

2. 包含(5.2中代码logo和foot代码一样)

   ```java
   <%@ page contentType="text/html;charset=UTF-8" language="java" %>
   <html>
   <head>
       <title>Title</title>
   </head>
   <body>
   <jsp:include page="LOGO.jsp"></jsp:include>
   <h1>BODY</h1>~
   <jsp:include page="FOOT.jsp"></jsp:include>
   </body>
   </html>
   ```

### 9.4 静态包含和动态包含区别

#### 9.4.1 静态包含基本原理

单纯复制,复制后进行翻译执行。 (目录只有一个.java和一个.class)

#### 9.4.2 动态包含基本原理

被包含的jsp会先翻译执行,把执行的结构包含进原jsp。(目录结构则有两个.java两个.class)

# 2_EL

## 01_EL概述

### 1.1 EL的简介(Expression Language)

EL（Expression Language） 是为了使JSP写起来更加简单。表达式语言的灵感来自于 ECMAScript 和 XPath 表达式语言，它提供了在 JSP 中简化表达式的方法，让Jsp的代码更加简化。

### 1.2 EL的作用

EL和JSTL一起使用可以替代JSP页面中嵌入java代码写法。

### 1.3 EL功能

+ 获取域中的数据
+ 执行运算
+ 获取web开发常用的对象
+ 调用java的方法(不常用)

### 1.4 EL语法

+ `${表达式}`

## 02_EL获取数据

### 2.1 EL如何获取数据呢

EL表达式在执行的过程中会调用pageContext.findAttribute()方法。分别从page,request,session,application范围查找对象,找到就会返回对象,找不到返回空字符串。

EL获取对象只能在四个域对象中获取。

### 2.2 代码演示

+ 写法${域范围Scope.属性名}

+ 也可以直接写属性名,会按照域范围从小到大一次查找

+ 代码

  ```jsp
  <%@ page contentType="text/html;charset=UTF-8" language="java" %>
  <html>
  <head>
      <title>Title</title>
  </head>
  <body>
  <h1>EL获取数据</h1>
  <% pageContext.setAttribute("name","张三");
   pageContext.setAttribute("name","李四",PageContext.REQUEST_SCOPE);
   pageContext.setAttribute("name","王五",PageContext.SESSION_SCOPE);
   pageContext.setAttribute("name","赵六",PageContext.APPLICATION_SCOPE);
  %>
  <%= pageContext.getAttribute("name") %>-
  <%= pageContext.getAttribute("name",PageContext.REQUEST_SCOPE) %>-
  <%= pageContext.getAttribute("name",PageContext.SESSION_SCOPE) %>-
  <%= pageContext.getAttribute("name",PageContext.APPLICATION_SCOPE) %>-
  
  ${pageScope.name}
  ${requestScope.nama}
  ${sessionScope.name}
  ${applicationScope.name}
  </body>
  </html>
  ```

  

## 03_EL获取数组集合中的数据

+ 数组演示

  ```jsp
  <%@ page contentType="text/html;charset=UTF-8" language="java" %>
  <html>
  <head>
      <title>Title</title>
  </head>
  <body>
  <%
      String[] st = {"aa", "bb", "cc"};
      pageContext.setAttribute("array", st);
  %>
  ${pageScope.array[0]}
  ${array[1]}
  ${array[2]}
  </body>
  </html>
  ```

+ List集合中获取

  ```jsp
  <%@ page contentType="text/html;charset=UTF-8" language="java" %>
  <html>
  <head>
      <title>Title</title>
  </head>
  <body>
  <%
      List<String> list = new ArrayList<String>();
      list.add("aa");
      list.add("bb");
      list.add("cc");
      pageContext.setAttribute("list", list);
  %>
  ${list.get(0)}
  ${list[1]}
  ${list[2]}
  </body>
  </html>
  ```

+ Map集合中获取

  ```jsp
  <%@ page contentType="text/html;charset=UTF-8" language="java" %>
  <html>
  <head>
      <title>Title</title>
  </head>
  <body>
  <%
      Map<String, Integer> map = new HashMap<String, Integer>();
      map.put("aaa", 111);
      map.put("bbb.c", 222);
      map.put("ccc", 333);
      pageContext.setAttribute("map", map);
  %>
  ${map.aaa}
  ${map["bbb.c"]}
  ${map.get("ccc")}
  </body>
  </html>
  ```

## 04_EL执行运算

  ### 4.1支持的运算

  + 算数运算(支持字符串自动转int,没有的值为0)

    ```jsp
    <%
        pageContext.setAttribute("n1", 10);
        pageContext.setAttribute("n2", "120");
    %>
    ${n1+n2+n3}
    ```

  + 关系运算

    ```jsp
    <%
        pageContext.setAttribute("n1", 10);
        pageContext.setAttribute("n2", "120");
    %>
    ${n1>n2}
    ${n1==n2}
    ${n1!=n2}
    ```

  + 逻辑运算

    ```jsp
    <%
        pageContext.setAttribute("n1", 10);
        pageContext.setAttribute("n2", "120");
    %>
    ${(n1>n2)&&(n1==n2)}
    ```

  + 三元运算

    ```jsp
    <%
        pageContext.setAttribute("n1", 10);
        pageContext.setAttribute("n2", "120");
    %>
    ${(n1>n2)? "n1>n2" : "n1<=n2"}
    ```

## 05_ELweb开发常用对象

### 5.1 WEL常用web开发对象有哪些

EL表达式定义了11个web开发常用对象。使用这些对象可以很轻松的获取到web开发中常用的对象.并且可以读取这些数据

1. pageContext:
2. pageScpoe:
3. requestScope:
4. sessionScope:
5. applictionScope:
6. param:    接收请求参数(接收一个名字对应一个值的)
7. paramValues:  接收请求参数(接收一个名字对应多个值的)
8. header:    页面中获取请求头(接收一个key对应一个value的)
9. headerValues:  页面中获取请求头(接收一个key对应多个value的)
10. cookie:     获得cookie
11. initParam:    获得全局初始化参数值的

### 5.2使用

1. param和paramValues

   ```jsp
   <%@ page contentType="text/html;charset=UTF-8" language="java" %>
   <html>
   <head>
       <title>Title</title>
   </head>
   <body>
   ${param.name}  -等同于-<!-- <% request.getParameter("name");   %> -->
   ${paramValues.hobby[0]}-${paramValues.hobby[1]}
   -等同于-<!-- <% request.getParameterValues("hobby");   %> -->
   </body>
   </html>>
   ```
   <img src="https://gitee.com/thisisafigurebed/figure-bed/raw/master/jspNode/2.png" style="zoom:70%">

2. header

   ```jsp
   ${header["User-Agent"]}
   ```

3. cookie

   ```jsp
   ${cookie.key.name}
   或者
   ${cookie.key.value}
   ```

4. initParam

   ```xml
   <!--配置一个全局参数 -->    
   <context-param>
           <param-name>user</param-name>
           <param-value>root</param-value>
       </context-param>
   ```

   ```jsp
    ${initParam.user}
   ```

   

# 3_JSTL

> JSTL（Java server pages standarded tag library，即JSP标准标签库）是由JCP（Java community Proces）所制定的标准规范，它主要提供给Java Web开发人员一个标准通用的标签库，并由Apache的Jakarta小组来维护。开发人员可以利用这些标签取代JSP页面上的Java代码，从而提高程序的可读性，降低程序的维护难度。

JSTL:又称为JSP的标准标签库

通过JSTL+EL可以取代传统页面上直接嵌入java代码的写法。

## 01_JSTL标签库

> 二三四几乎不使用了

### 1.1 配置

+ 将两个jar包导入项目
+ 在jsp页面中使用`taglib`引入标签库

### 1.1 C标签(core标签库)

1. set

2. if

3. foreach 

4. 其他标签可以查看文档得知

5. 代码:

   ```jsp
   <%@ page contentType="text/html;charset=UTF-8" language="java" %>
   <%@taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
   <html>
   <head>
       <title>Title</title>
   </head>
   <body>
   <%--<%
       request.setAttribute("name", "张三");
   %>--%>
   <c:set var="name" value="10" scope="page"></c:set>
       
   <c:if test="${name>5}">
       <font color="red">name是大于10的 </font>
   </c:if>
       
   <%
     String[] st = {"aaa", "bbb", "ccc"};
     pageContext.setAttribute("st", st);
   %>
   <c:forEach var="s" items="${st}">
       ${s}
   </c:forEach>
       
   <%-- 类似for循环 --%>
   <c:forEach var="i" begin="0" end="10" step="1">
       ${i}
   </c:forEach> 
       
   <%-- ifEach属性记录循环过程中的状态,具体值方法看文档 --%>
   <%-- 循环过程中每三个数数则变蓝 --%>
   <c:forEach var="aa" begin="1" end="100" step="2" varStatus="status">
       <c:if test="${status.count % 3 != 0}">aa:${aa}---status:${status.count}<br/></c:if>
           <c:if test="${status.count % 3 == 0}">
               <font color="blue" size="5">aa:${aa}---status:${status.count}<br/></font>
   </c:if></c:forEach>
       
   </body>
   </html>
   ```

   

### 1.2 fmt标签(国际化标签库)

### 1.3 xml标签(core标签库)

### 1.4 sql标签(core标签库)

### 1.5 jstl函数库(EL函数)

# 4_案例实现

> 数据库中获取数据发送给前端jsp

1. 导包建目录拷贝c3p0-config.xml到src/下,还有jdbcUtils.java工具类
   <img src="https://gitee.com/thisisafigurebed/figure-bed/raw/master/jspNode/3.png" style="zoom:70%">

   

2. 建立学生实体类

   ```java
   package com.damain;
   public class Student {
       private int sid;
       private String sname;
       private String sex;
       private int age;
       public Student() {
       }
       public Student(int sid, String sname, String sex, int age) {
           this.sid = sid;
           this.sname = sname;
           this.sex = sex;
           this.age = age;
       }
       public int getSid() {
           return sid;
       }
       public void setSid(int sid) {
           this.sid = sid;
       }
       public String getSname() {
           return sname;
       }
       public void setSname(String sname) {
           this.sname = sname;
       }
       public String getSex() {
           return sex;
       }
       public void setSex(String sex) {
           this.sex = sex;
       }
       public int getAge() {
           return age;
       }
       public void setAge(int age) {
           this.age = age;
       }
   }
   ```

   

3. model获取数据库信息

   ```java
   public class StudentModel {
   	// 查询所有学生信息的方法：
   	public List<Student> findAll() throws SQLException {
   		QueryRunner queryRunner = new QueryRunner(jdbcUtils.getDataSource());
   		List<Student> list = queryRunner.query("select * from student", new BeanListHandler<>(Student.class));
   		return list;
   	}
   }
   ```

4. studentServlet类

   ```java
   public class studentServlet extends HttpServlet {
       @Override
       protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
          doPost(req, resp);
       }
   
       @Override
       protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
           try {
               //调用java处理数据
               StudentModel s = new StudentModel();
               //查询到所有学生信息
               List<Student> list = s.findAll();
               //显示回jsp页面
               req.setAttribute("list", list);
               req.getRequestDispatcher("/jsp/list.jsp").forward(req, resp);
           } catch (Exception e) {
               e.printStackTrace();
           }
       }
   }
   ```

5. list.jsp

   ```jsp
   <%@ page contentType="text/html;charset=UTF-8" language="java" %>
   <%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
   <html>
   <head>
       <title>list</title>
   </head>
   <body>
   <h1>学生信息显示:</h1>
   <table border="1">
       <tr>
           <td>学生编号:</td>
           <td>学生姓名:</td>
           <td>学生性别:</td>
           <td>学生年龄:</td>
       </tr>
       <c:forEach var="student" items="${list}">
           <tr>
               <td>${student.sid}</td>
               <td>${student.sname}</td>
               <td>${student.sex}</td>
               <td>${student.age}</td>
           </tr>
       </c:forEach>
   </table>
   </body>
   </html>
   ```

6. 效果
   <img src="https://gitee.com/thisisafigurebed/figure-bed/raw/master/jspNode/4.png" style="zoom:70%">

