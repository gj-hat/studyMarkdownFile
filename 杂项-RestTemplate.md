# 2RestTemplate

> 本文着重点讲解 
>
> 1. 使用HttpComponentsAsyncClientHttpRequestFactory连接工厂代替默认的SimpleClientHttpRequestFactory
> 2. 如何使用RestTemplate进行http”模拟”请求



## 什么是RestTemplate

在java中访问restful服务使用到的类RestTemplate



## 为什么需要有RestTemplate

本质上而言RestTemplate就是一个http请求,和通过浏览器访问controller没差。但是我们我们在使用http请求时(上网的时候)浏览器已经帮我们做了很多有关http协议的封装我们只需要输入url就可以正常上网。

**那么问题来了**

如果我们想在java代码中通过http协议访问其他地址呢?

又或者说 爬虫? ....

**解决思路**

1. 自己封装一个http协议,其中包含请求头,行,体。响应头,行体
2. 符合restful巴拉巴拉规范的配置

**简化解决思路**

这时候`RestTemplate`帮我们完成了这其中的封装,我们可以像使用浏览器一样去在java代码中访问其他地址(仅仅需要url就可以)



## 如何使用RestTemplate

RestTemplate提供了多种便捷访问远程Http服务的方法.是一种简单便捷的访问restful服务模板类，是spring提供的用于访问Rest服务的客户端模板工具集
使用：
		使用restTemplate访问restful接口非常的简单粗暴无脑。
		(url,requestMap,ResponseBean.class)这是三个参数分别代表
		REST请求地址、请求参数、HTTP响应转换被转换成的对象类型。

### 先来看看http协议有关请求的东西

众所周知http请求有很多种例如post,get,delete....

下面列出常用的请求方式和`RestTemplate`的实现类:

| HTTP method | RestTemplate methods |
| ----------- | -------------------- |
| DELETE      | delete               |
| HEAD        | headForHeaders       |
| OPTIONS     | optionsForAllow      |
| PUT         | put                  |
| GET         | getForObject         |
|             | getForEntity         |
| POST        | postForLocation      |
|             | postForObject        |
| any         | exchange             |
|             | execute              |

### 简单实例: 不带参GET请求

1. 配置类

```java
@Configuration
public class RestTemplateConfiguration {
    @Bean
    public RestTemplate newRestTemplate(ClientHttpRequestFactory simpleClientHttpRequestFactory) {
        return new RestTemplate(simpleClientHttpRequestFactory);
    }
    // 设置连接池
    @Bean
    public ClientHttpRequestFactory simpleClientHttpRequestFactory(){
        SimpleClientHttpRequestFactory factory = new SimpleClientHttpRequestFactory();
        factory.setConnectTimeout(15000);
        factory.setReadTimeout(5000);
        return factory;
    }
}
```

2. controller

```java
@Autowired
private RestTemplate restTemplate;

// 获取 http请求头,行,体
@RequestMapping("/test1")
public String test1() {
    ResponseEntity<String> responseEntity = restTemplate.getForEntity("http://127.0.0.1:8080/templateManage/findNotAll", String.class);
    String body = responseEntity.getBody();
    HttpStatus statusCode = responseEntity.getStatusCode();
    int statusCodeValue = responseEntity.getStatusCodeValue();
    HttpHeaders headers = responseEntity.getHeaders();
    StringBuffer result = new StringBuffer();
    result.append("responseEntity.getBody()：").append(body).append("<hr>")
            .append("responseEntity.getStatusCode()：").append(statusCode).append("<hr>")
            .append("responseEntity.getStatusCodeValue()：").append(statusCodeValue).append("<hr>")
            .append("responseEntity.getHeaders()：").append(headers).append("<hr>");
    return result.toString();
}
```





## 深入理解RestTemplate

> 正如上文所说RestTemplate是一个封装http请求的模板, 里面提供了一些请求http和配置请求http的方法 

### RestTemplate方法

 ![image-20211104110443435](E:\giteeFigurebed\figure-bed\other\image-20211104110443435.png)

**主要使用到其中的`Operations`,这里封装着常用的http请求方法 详情见上[http请求方式](#先来看看http协议有关请求的东西)**

点开之后可以看到大量的诸如此类的方法

![image-20211104111409353](E:\giteeFigurebed\figure-bed\other\image-20211104111409353.png)

可以看到每一个方法都及其类似 参数也正是url,返回值,url参数等等  返回值为`excute`不难理解 这是进行请求的意思

**再来看看`excute`干了什么事情呢**

![image-20211104111740756](E:\giteeFigurebed\figure-bed\other\image-20211104111740756.png)

`execute`将参数进行封装传入了`doExcute`中

**最后来看看`doExcute`内部**

![image-20211104112118605](E:\giteeFigurebed\figure-bed\other\image-20211104112118605.png)

doExecute 当中调用了 `RequestCallback` & `ResponseExtractor`  

>  下面的来自转载Spring RestTemplate详解

**RequestCallback**

> 用于在ClientHttpRequest上操作的代码的回调接口。允许操作请求头，并写入请求主体。

RequestCallback有两个实现类，都是内部类

| 实现类                      | 功能                                                         |
| --------------------------- | ------------------------------------------------------------ |
| AcceptHeaderRequestCallback | 只处理请求头，用于restTemplate.getXXX()方法。                |
| HttpEntityRequestCallback   | 继承于AcceptHeaderRequestCallback可以处理请求头和body，用于restTemplate.putXXX()、restTemplate.postXXX()和restTemplate.exchange()方法。 |


提示：DELETE、HEAD、OPTIONS没有使用这个接口

**ResponseExtractor**

> restTemplate对此接口的检索方法实现使用的通用回调接口执行从clienthttpresponse提取数据的实际工作(解析HTTP响应的数据)，但不需要担心异常处理或关闭资源。RequestCallback有三个实现类

| 实现类                          | 功能                                                         |
| ------------------------------- | ------------------------------------------------------------ |
| HeadersExtractor                | 用于提取请求头。                                             |
| HttpMessageConverterExtractor   | 用于提取响应body。                                           |
| ResponseEntityResponseExtractor | 使用HttpMessageConverterExtractor提取body（委托模式），然后将body和响应头、状态封装成ResponseEntity对象。 |

**至此中级RestTemplate就讲解完了 使用方式见上[简单实例](#简单实例: 不带参GET请求)**



## RestTemplate继承关系

> 上文看了RestTemplate的方法  现在再来看看继承树和父类提供了哪些方法

### RestTemplate继承关系

 ![image-20211104104821456](E:\giteeFigurebed\figure-bed\other\image-20211104104821456.png)

1. HttpAccessor 类
    RestTemplate的基类，是一个抽象类，提供了一些公共的属性，如 `ClientHttpRequestFactory`,ClientHttpRequestFactory 是clientHttpRequest的工厂类，通过 createRequest 来创建http请求的方法

2. InterceptingHttpAccessor 类
    这个是 HttpAccessor的一个抽象子类，多增加了 请求拦截器相关的属性，`ClientHttpRequestInterceptor`,而这个`ClientHttpRequestInterceptor`是 spring cloud中各种服务治理的关键，如客户端负载均衡，链路跟踪等



### 父类方法

 ![image-20211104104911736](E:\giteeFigurebed\figure-bed\other\image-20211104104911736.png)

+ 构造方法

  ``` java
  public InterceptingHttpAccessor() {
  }
  ```

+ 设置拦截器

  ``` java
  // 该方法传入一个clientHtpp拦截器的List集合 并执行拦截操作
  public void setInterceptors(List<ClientHttpRequestInterceptor> interceptors) {
      Assert.noNullElements(interceptors, "'interceptors' must not contain null elements");
      if (this.interceptors != interceptors) {
          this.interceptors.clear();
          this.interceptors.addAll(interceptors);
          AnnotationAwareOrderComparator.sort(this.interceptors);
      }
  }
  ```

+ 获得拦截器   没什么好说的 就是一个get方法

  ``` java
  public List<ClientHttpRequestInterceptor> getInterceptors() {
      return this.interceptors;
  }
  ```

+ 设置连接工厂

  ``` java
  // 方法接收一个ClientHttpRequestFactory类型参数
  public void setRequestFactory(ClientHttpRequestFactory requestFactory) {
      super.setRequestFactory(requestFactory);
      this.interceptingRequestFactory = null;
  }
  ```

+ 得到连接工厂

  ``` java
  public ClientHttpRequestFactory getRequestFactory() {
      // 获取拦截器
      List<ClientHttpRequestInterceptor> interceptors = this.getInterceptors();
      if (!CollectionUtils.isEmpty(interceptors)) {
          // 使用ClientHttpRequestFactory进行http请求
          ClientHttpRequestFactory factory = this.interceptingRequestFactory;
          if (factory == null) {
              factory = new InterceptingClientHttpRequestFactory(super.getRequestFactory(), interceptors);
              this.interceptingRequestFactory = (ClientHttpRequestFactory)factory;
          }
          return (ClientHttpRequestFactory)factory;
      } else {
          return super.getRequestFactory();
      }
  }
  ```

  

## 使用HttpClient连接池

> 使用HttpComponentsAsyncClientHttpRequestFactory连接工厂代替默认的SimpleClientHttpRequestFactory

### 老规矩再看看工厂

下期在讲

