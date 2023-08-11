# javaWeb相关小问题



## 后台传数据 ajax接收

1. 发送回前端
   resp.getWriter().println(b);

2. ajax接收

   ```js
   $.ajax({
       url: "/UserServlet",
       data: {method: "ajaxValidateVerifyCode", verifyCode: $('#verifyCode').val()},
       type: "POST",
       dataType: "json",
       async: false,//是否异步请求，如果是异步，那么不会等服务器返回，我们这个函数就向下运行了。
       success: (result) => {
           if (!result) {
               $("#errMsg").text("验证码错误");
           } else {
               $("#errMsg").text("")
   ```

3. result用来接收后台的b

4. 关于dataType: "json",参数

   后台传布尔 格式为json

   后台传文本 格式为text

### ajax返回值的问题

```js
function checkVerifyCode() {
    var res
    $.ajax({
        url: "/UserServlet",
        data: {method: "ajaxValidateVerifyCode", verifyCode: $('#verifyCode').val()},
        type: "POST",
        dataType: "json",
        async: false,
        success: (result) => {
            if (!result) {
                $("#errMsg").text("验证码错误");
                res = false
            } else {
                $("#errMsg").text("")
                res = true
            }
        }
    })
    alert(res)
    return res
}
```



## 页面嵌入

```html
<iframe class='col-sm-12' frameborder='0' height='700px' src='./user/repasswd.jsp' ></iframe>
```

### 问题1

1. 先导入再编译

2. 所以repasswd.jsp可以使用原页面的context对象 可以读取父页面的对象

3. 新页面的css js等资源文件目录按照父页面去写

4. 也可以使用jsp获取当前路径然后写相对路径

   ```html
   <%--    servlet重定向的页面 css js等资源目录需要重写--%>
       <%
           String path = request.getContextPath();
           String basePath = request.getScheme() + "://" + request.getServerName() + ":" +
                   request.getServerPort() + path + "/";
       %>
       <base href="<%=basePath%>">
       <!-- Bootstrap -->
       <link rel="stylesheet" href="<%=basePath%>/jsps/css/bootstrap.css">
   ```





