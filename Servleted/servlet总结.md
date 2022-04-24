[TOC]
# 1. servlet


## servlet接口



1. servlet运行流程

   <img src="https://gitee.com/thisisafigurebed/figure-bed/raw/master/servletNode/15.png" >

2. 接口方法

   + public void init(ServletConfig config)
     + 第一次请求该servlet时，就会初始化一个Servlet对象，也就是会执行初始化方法init()
   +  public void service(ServletRequest req, ServletResponse res)
     + 该servlet对象去处理所有客户端请求，在service()方法中执行
   +  public void destroy();   
     + 服务器关闭时，才会销毁这个servlet对象，执行destroy()方法。
   + public String getServletInfo();   
   + public ServletConfig getServletConfig(); 
     + 获取ServletConfig对象
     + 包括的方法
       + ServletName
       + ServletContext
       + InitParameter
       + InitParameterNames

3. 接口详解

```java
public interface Servlet {       
  
  //负责初始化Servlet对象。容器一旦创建好Servlet对象后，就调用此方法来初始化Servlet对象      
     public void init(ServletConfig config) throws ServletException;        
  
   //方法负责响应客户的请求,提供服务。当容器接收到客户端要求访问特定的servlet请求时，就会调用Servlet的service方法      
      public void service(ServletRequest req, ServletResponse res) throws ServletException, IOException;        
    /*   
      *Destroy()方法负责释放Servlet 对象占用的资源，当servlet对象结束生命周期时，servlet容器调用此方法来销毁servlet对象.   
    **/    
      public void destroy();    
      /*   
      说明：Init(),service(),destroy() 这三个方法是Servlet生命周期中的最重要的三个方法。   
      **/       
      /*   
      *返回一个字符串，在该字符串中包含servlet的创建者，版本和版权等信息   
      **/    
      public String getServletInfo();      
          
      /*   
     GetServletConfig: 返回一个ServletConfig对象，该对象中包含了Servlet初始化参数信息    
     **/        
      public ServletConfig getServletConfig();      
  } 
```



# 2. GenericServlet抽象类

> 由于servlet是一个接口类,需要我们自己实现。但是实现过于复杂且冗余,所以GenericServlet抽象类实现了servlet接口,值得一提该类未实现service方法

GenericServlet是一个无协议的servlet

为了简化serlvet的编写，在javax.servlet包中提供了一个抽象类GenericServlet，它给出了除service()方法以外的简单实现。

GenericServlet定义了一个通用的，不依赖具体协议的Servlet，它实现了Servlet接口和ServletConfig接口。

`public abstract class GenericServlet implements Servlet, ServletConfig, java.io.Serializable{}`

# 3. HttpServlet抽象类

> 网页使用http协议就需要使用httpservlet。其中HttpServlet的service方法已经实现过

HttpServlet主要是应用于HTTP协议的请求和响应，为了快速开发HTTP协议的serlvet，sun提供了一个继承自GenericServlet的抽象类HttpServlet，用于创建适合Web站点的HTTP Servlet。

`public abstract class HttpServlet extends GenericServlet implements java.io.Serializable`

## 主要私有方法

1. void service(HttpServletRequest req, HttpServletResponse resp)
2. HttpServlet实现Servlet接口的service方法是将ServletResponse对象和ServletRequest对象转化成httpServletResponse对象和HttpservletRequest对象，然后再调用私有方法service。它根据request获取Http method(get、post等)的名称，根据http method调用不同的方法执行操作。

# 4. 几个主要的对象



## 1. ServletConfig

1. 获取途径：getServletConfig();
2. 功能：能得到四个东西
   + getServletName();
     + 获取servlet的名称，也就是我们在web.xml中配置的servlet-name
   + getServletContext;
     + 获取ServletContext对象,servlet的上下文对象
   + getInitParameter(String);
     + 获取在servlet中初始化参数的值。这里注意与全局初始化参数的区分。这个获取的只是在该servlet下的初始化参数(xml文件中`<int-param>`中初始化的参数)
   + getInitParameterNames();
     + 获取在Servlet中所有初始化参数的名字，也就是key值，可以通过key值，来找到各个初始化参数的value值。注意返回的是枚举类型(集合)
3. 注意:
   + 在上面我们所分析的源码过程中，我们就知道，其实可以不用先获得ServletConfig，然后在获取其各种参数，可以直接使用其方法，比如上面我们用的ServletConfig().getServletName();可以直接写成getServletName();而不用在先获取ServletConfig();了，原因就是在GenericServlet中，已经帮我们获取了这些数据，我们只需要直接拿就行。



## 2. ServletContext(功能很强大)

1. 获取途径：

   + getServletContext(); 
   + getServletConfig().getServletContext();
   + 原因同上3的注意

2. 功能

   tomcat为每个web项目都创建一个ServletContext实例，tomcat在启动时创建，服务器关闭时销毁，在一个web项目中共享数据，管理web项目资源，为整个web配置公共信息等，通俗点讲，就是一个web项目，就存在一个ServletContext实例，每个Servlet读可以访问到它。

   1. web项目中共享数据
      + getAttribute(String name)

        在web项目范围内存放内容，以便让在web项目中所有的servlet读能访问到

      + setAttribute(String name, Object obj)

        通过指定名称获得内容

      + removeAttribute(String name)

        通过指定名称移除内容

   2. 整个web项目初始化参数 ,就是全局初始化参数，每个Servlet中都能获取到该初始化值

      + getInitPatameter(String name)

        通过指定名称获取初始化值

      + getInitParameterNames()

        获得枚举类型

   3. 获取web项目资源

      + 获取web项目下指定资源的路径：getServletContext().getRealPath("/WEB-INF/web.xml")
      + 获取web项目下指定资源的内容，返回的是字节输入流。InputStream getResourceAsStream(java.lang.String path)

   4. getResourcePaths(java.lang.String path) 指定路径下的所有内容。

## 3. request

> request就是将请求文本封装而成的对象，所以通过request能获得请求文本中的所有内容，请求头、请求体、请求行 。

1. 请求行内容的获取

2. 请求头的获取

3. 请求体的获取(分为GET,POST)

   1. String request.getParameter(String) 获得指定名称，一个请求参数值。
   2. String[] request.getParameterValues(String) 获得指定名称，所有请求参数值。例如：checkbox、select等
   3. Map<String , String[]> request.getParameterMap() 获得所有的请求参数　　

4. 请求转发

   1. request.getRequestDispatcher(String path).forward(req,resq);　　

      path:转发后跳转的页面，这里不管用不用"/"开头，都是以web项目根开始，因为这是请求转发，请求转发只局限与在同一个web项目下使用，所以这里一直都是从web项目根下开始的，

   2. 特点

      浏览器中url不会改变，也就是浏览器不知道服务器做了什么，是服务器帮我们跳转页面的，并且在转发后的页面，能够继续使用原先的request，因为是原先的request，所以request域中的属性都可以继续获取到。

## 4. response

> 类似上边请求,  response响应包含响应行,响应头,响应体

1. 响应行

   1. 设置响应的状态码 `resp.setStatus(int status)`
      1. 200 正确
      2. 302 重定向（就是页面的跳转）
      3. 304 查找本地缓存
      4. 404 请求资源不存在
      5. 500 服务器内部错误

2. 响应头

   1. response.setHeader(java.lang.String name, java.lang.String value) 设置指定的头，一般常用。

   2. 例如:response.setHeader("Refresh",3);   //设置每隔3秒就自动刷新一次

      <img src="https://gitee.com/thisisafigurebed/figure-bed/raw/master/servletNode/16.png" >

3. 响应体

   1. getOutputStream(); 输出内容
   2. getWriter(); 输出内容