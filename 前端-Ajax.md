# Ajax

> Ajax即Asynchronous Javascript And XML（异步JavaScript和XML）在 2005年被Jesse James Garrett提出的新术语，用来描述一种使用现有技术集合的‘新’方法，包括: HTML 或 XHTML, CSS, JavaScript, DOM, XML, XSLT, 以及最重要的XMLHttpRequest。 使用Ajax技术网页应用能够快速地将增量更新呈现在用户界面上，而不需要重载（刷新）整个页面，这使得程序能够更快地回应用户的操作。

## 01_同步和异步请求

1. 同步请求

   浏览器发出请求,等待服务器做出回应。等待期间浏览器不能做任何事

   浏览器整体刷新

2. 异步请求

   浏览器发出请求后等待服务器做出回应期间可以处理其他事务

   局部刷新

   本质来讲,异步请求就是浏览器开启一个新的异步线程。主线程可以继续执行操作,等到异步线程回来之后和主线程合并处理。

## 02_Ajax对象

### 2.1 XMLHttpRequest对象

专门用来发送异步请求的对象,也可以发送同步请求但是没必要,同步请求使用超链接就好。

### 2.2 XMLHttpRequest对象属性

1. readyState属性

   > 此属性有0到4的值,五种状态

   + 0  :  请求未初始化
   + 1  :  服务器连接已建立
   + 2  :  请求已接收
   + 3  :  请求处理中
   + 4  :  请求已完成,且相应已就绪

2. status属性

   > 相应的状态码,也有五种值

   + 1xx  :
   + 2xx  :  服务端正常响应客户端
   + 3xx  :  服务端资源没有发生变化
   + 4xx  :  资源错误
   + 5xx  :  代表服务端错误

3. responseText和responseXML

   > responseText以文本信息获取服务端响应即字符串,一般要转为JSON(JSON.parse(str))
   >
   > responseXML当服务端以XML格式返回给客户端时则使用此属性去接收。获取的就是DOM对象(xml就是DOM对象)



## 03_Ajax代码编写步骤

1. 创建XMLHttpRequest对象
2. 注册回调函数(很重要),回调函数可以感知异步线程做出相应
3. 建立链接
4. 发送请求

```js
var xmlhttp;
function ajax_demo(){
	//1.
    if(windows.XMLHttpRequest){
        //IE7以上   ff safari chrome
        xmlhttp = new XMLHttpRequest;
    }else{
        xmlhttp =  new ActionXObject("Micrsisift.XMLHTTP");
    }   
    //2.
    xmlhttp.onreadystatechange = function(){
        //执行你的异步代码逻辑
        if(xmlhttp.readyState == 4  || xmlhttp.status == 200){
            //  .......
            var str = xmlhttp.responseText;
            //如果服务端传回的是JSON对象则需要解析JSON对象
            var json = JSON.parse(str);
            ....
              
        }
        //  .......
    }
    //3.
    xmlhttp.open("post|get","目标url",true|false);   //true表示异步,反之同步
    //4.
    xmlhttp.send();   //如果有数据,将数据作为参数传递过去
}
```



