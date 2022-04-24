[TOC]



# javaScript学习笔记

## 01_javaScript基本

### 1.1 ECMASCRIPT

js的核心，ecmaScript描述了js的语法和基本对象

简单地说，ECMAScript 描述了以下内容：

- 语法 
- 类型
- 语句
- 关键字
- 保留字
- 运算符
- 对象

### 1.2 DOM

描述了网页处理的方法和接口

### 1.3 BOM

描述了与浏览器进行交互的方法和接口



   <img src="https://gitee.com/thisisafigurebed/figure-bed/raw/master/jsNode/5.png" style="zoom:70%;" />

## 02_语法规则

### 1. 简单输出案例

```javascript
var a = 10;
var b = 5;
var temp = a+b;
alert(temp);
```
   <img src="https://gitee.com/thisisafigurebed/figure-bed/raw/master/jsNode/1.png" style="zoom:70%;"  />

### 2. 基本数据类型

> 区分java and js (js自动区分数据格式声明var即可使用)

+ String，Char.js不区分字符和字符串

  ```js
  var st1 = "lalal";
  var st2 = 'lalal';
  //两者等价
  ```

+ boolean类型 java和js一样

    ```js
    var flag1 = true;
    var flag2 = flase;
    ```
    
+ number数字类型（js不区分整数浮点数）

    ```js
    var num1 = 10;
    var num2 = 1.11;
    var num3 = -111;
    ```
    
+ null空一个占位符号

    ```js
    var obj = null;
    ```

+ undefined未定义数据类型该类型仅有一个固定值undefined表示变量声明却没定义值

    ```js
    var a;
    var b = undefined;
    //两者等价
    ```
    
+ 变量混合使用
    ```js
        var a = 10;
        a = "DAWDWA";
        a = null;
        a = true;
        alert(a);
    ```
     <img src="https://gitee.com/thisisafigurebed/figure-bed/raw/master/jsNode/2.png" style="zoom:70%;" />
    
+ <font color=red size=5>查看变量类型方法typeof</font>
  
  ```js
  var a = 10;
  a = "lalal";
  alert(typeof(a));
  ```
     <img src="https://gitee.com/thisisafigurebed/figure-bed/raw/master/jsNode/3.png" style="zoom:70%;" />  

+ 引用数据类型

  java中引用数据类型成为 类，js引用数据类型成为对象，创建方式如下：

  ```js
  var str = new String(); //和jaav一样可以带参数
  var str = new String;  //js独有
  ```

### 3. 比较运算符

+ "=="，"===="

  大多数和java相同。因为js不区分数据类型，

  前者比较值相同返回true不一样返回false。后者比较值和数据类型相同true.....

  ```js
  var str1 = 10;
  var str2 = 10;
  var str3 = "10";
  str1 == str2;   //true
  str2 == str3;   //true
  str2 === str3;  //false
  ```

+  **逻辑运算符**

  与或非javaVSjs

  + java

    &.|,!,^,&&,||(查漏补缺单个与或两边都要运算，双与或只运算左边)

  + js仅有以下三个
  
    &&,||, !

###  4. 正则对象

+  RegExp对象

  + 直接量方式，必须每一个字符都符合正则否则返回false，一般用于表单校验

    ```js
      var reg = /^巴拉巴拉$/    
      ^是正则表达式开始    $是正则表达式结束
    ```
  + 普通方式，只要有一个符合都true除非全部字符都不符合返回false,一般用于字符串查找替换
    ```js
      var reg = /巴拉巴拉/;
    
    ```

+  test方法

  检索字符串中指定值返回true或者false

  ```js
  var reg = /^\s*$/;	//直接量方式0~n个空格
  var flag =  reg.test("  ");
  alert(flag);    //true
  var reg1 = /\s+/;	//普通方式1~n个空格
  flag = reg1.test("   a  );
  alert(flag);  // true
  ```

### 5. JS数组对象 

+  js数组特性

  **js数组可以看出java中ArrayList集合**

  1. 数组中每一个成员没有类型限制，即可以存放任意类型
  2. 数组的长度可以任意更改

+  js创建数组四种方式

  ```js
  //主要掌握前两种
  //第三种其实没意义，因为js数组长度可变
  var arr1 = [1, 2, "ddwad", 'daw', true]; //长度为5元素分别为......
  var arr2 = new Array();  //new一个数组
  var arr3 = new Array(4);  //长度为4
  var arr4 = new Array(1,2,"dawd"); //长度为3元素分别是1，2，dawd
  ```

+  数组使用和lenght方法

  ```js
  //lenght方法等同java
  var arr = [1,2,'das',true];
  alert(arr.lenght);    //输出4
  arr[1] = "dddd";
  arr[4] = "lalala";
  alert(arr.lenght);   //输出5
  arr[7] = 5;
  alert(arr.lenght);   //输出8  (5到7undefined)
  ```

+  js数组对象常用方法

  1. join(String)方法将数组元素按照指定字符拼接（不影响原先数组）
  2. pop()删除并返回数组最后一个元素
  3. push()向数组的末尾添加一个元素并返回新的长度
  4. reverse（）反转数组中元素的顺序(反转下标，影响原先数组)
  ```js
  var arr1 = [1, 2, "ddwad", 'daw', true,434];
  var st = arr1.join("+");
  alert(st);  //1+2+ddwad+daw+true+434
  var st1 = arr1.pop();  //返回删除的元素
  alert(st1);  //434
  alert("pop方法后原来数组长度"+arr1.length);  //5
  var st2 = arr1.push("new");  //返回长度
  alert("push方法后数组长度"+st2);  //6
  alert(arr1); //1,2,ddwad,daw,true,new
  alert(arr1.reverse()); //new,true,daw,ddwad,2,1
  ```

### 6.全局函数global

+  evel（String）函数：

  可以把传入的字符串当作js代码执行(可以拓展程序功能)

  只能传入基本的数据类型，不能传入字符串对象

  ```js
  evel(var x = 10);  //等价于var x = 10
  alert(x);   //10
  evel(new String("var a = 10"));
  alert(a); //报错 a未定义
  ```

  

+ 编码和解码

  URL编码：传递中文容易出错

  ```js
  var msg = "www.lalala.com/index.html?username=张三&passsword=123";
  alert(msg);  //www.lalala.com/index.html?username=张三&passsword=123
  var after = encodeURI(msg);  //编码
  alert(after);  //输出结果username=%E5%BC%A0%E4%B8%89&passsword=123
  var after1 = decodeURI(after);  //解码
  alert(after1);  //www.lalala.com/index.html?username=张三&passsword=123
  
  ```
   **URI和URL区别**

  URI统一资源标识符：标识资源详细名称

  URL统一资源定位符：定位网络资源位置

  

+ 字符串转数字

  parseInt();    //转数字(不带小数)

  parseFloat();  //转数组(带小数)

  ```js
          var x = "10.55";
          var temp1 = parseInt(x);
          var temp2 = parseFloat(x);
          alert(typeof(temp1)+":"+temp1);   //number:10
          alert(typeof (temp2) + ":" + temp2);  //number:10.55
  		//试错
  		// x = 10.a.55      结果10  10
  		// x = 1a0.55       结果1   1
  		// x = 10.55.a      结果10  10.55
  		// x = a10.55		结果NaN(Not A Number)这是一个数字类表示，表面值不是数字
  ```
### 7. 自定函数

+ 创建格式 function 方法名（参数列表）{函数体}

  ```js
  function getSum(a,b){
      return a+b;
  }
  ```

+ 调用

  ```js
  var result = getSum(1,2);
  alert(result);
  ```

+ 函数使用注意

  js调用完默认返回defined

  js方法名相同不存在重载，先来后到覆盖

  js仅仅依靠方法名识别方法，形参不匹配不影响

### 8. 自定对象

+ 创建创建格式 function 对象名（）{函数体}

+ 调用 var ob = new 对象名();

  ```js
          //1. 声明对象
          //2. 声明属性 this.对象名
          function persional() {
              this.name = "张三";
              this.age = 22;
          }
          var per = new persional();
          alert(per.name+per.age);    //张三22
  
		function persional1(a, b) {
              this.name = a;
              this.age = b;
          }
          var per1 = new persional1("我是参数调用", 100);
          alert(per1.name+per1.age);  //我是参数调用100
  		per1.sex = '男';  //为persional1声明sex属性
          alert(per1.sex);   //男
  ```
  
   
  
+ 对象直接量法创建，直接创建出实例不需要new

  var 对象名 = {属性1:"值",属性2:"值",属性3:"值",属性4:"值"，，，，，};
  
  ```js
          var per2 = {name:'我是直接量创建', age: 2020};
          per2.e = "我是追加属性";
          alert(per2.name+per2.age+per2.e);  //我是直接量创建2020我是追加属性
  ```
  
  

## 03_BOM对象

	浏览器对象模型BOM：BOM对浏览器窗口进行访问和操作。
	
	BOM对象有：

### 1. Windows对象

windwos对象是主要使用对象，windows是js内置对象，调用时可以省略不写例如alert（）方法

浏览器打开的窗口比如chrome，firefox，IE等

#### 1.1 消息框

+  alert()方法

警告框，用来弹出警告信息

     ```js
     alert("你好");
     ```

 <img src="https://gitee.com/thisisafigurebed/figure-bed/raw/master/jsNode/6.png" style="zoom:70%" />
+ confirm()方法

确认框，用于告知用户信息并收集用户选择

   ```js
           var b = confirm("你好");
           alert(b);    //返回用户点击确认还是取消
   ```

 <img src="https://gitee.com/thisisafigurebed/figure-bed/raw/master/jsNode/7.png" style="zoom:70%" />

#### 1.2 定时器

+ 循环定时器的设置和取消

  + 启动循环定时器 setInterval()

    返回定时器ID

    调用一次就会创建并循环执行一个定时器
    
  
  格式： setInterval(调用方法，毫秒值)；
  
  + 取消循环定时器 clearInterval()
  
    ```js
    //循环5次后停止
    var intervalId;
    var sum=0;
    function run1(){
      alert("我是run1");
        sum++;
        if (sum == 5) {
            clearInterval(intervalId);
        }
    }
    intervalId = setInterval("run1()", 2000);   //等价 setInterval(run1, 2000);
    ```
  
+ 一次性定时器设置和取消
  调用时创建并执行一个一次性定时器
  + 启动setTimeout(调用方法，毫秒值)；
    
    返回定时器id值
    
  + 取消clearTimeout(id);

    ```js
    function run() {
        alert("你好");
    }
    var timeOut = setTimeout("run()", 2000);
    clearTimeout(timeOut);
    ```




### 2. Naviqator



### 3. Screen



### 4.History



### 5. Location

>  **location对象包含浏览器地址栏的信息**
>
>  **常用属性URL，主机，路径，端口，协议，URL部分截取**
>
>  **对象方法加载新文档，重新加载当前，新文档替换当前**

	可通过windows.localtion属性访问

+ 常用属性

  + hrel  设置或返回完整的URL

    ```js
    var loca = window.location;
    alert(loca);    //返回当前页面地址编码后的URL
    var t = decodeURI(loca);   //解码
    alert(t);  //返回解码后的URL
    window.location.href = "http://www.baidu.com";  //跳转到百度页面

    //两秒后跳转百度页面
    function run() {
        window.location.href = "http://www.baidu.com";
    }
    setTimeout("run()", 2000);
    
    ```
## 03_DOM对象

> **DOM(Document Object Model)文档对象模型**
>
> **DOM将标记形文档中所所有的东西（标签文本属性）封装成对象**
>
> **通过操纵对象的属性和方法来控制改变HTML展示**
>
> **一个HTML文档加载到内存中会形成一个文档**

#### 3.1 获取元素对象四种方式

> 注意获取对象方法，应在元素之后

+ getElementByld()  --- 通过元素ID获取对应元素

+ getElementsByname() --- 通过元素name属性获取符合要求的对象  返回值是数组

+ getElementBsyTagName() --- 通过元素的元素名属性（标签名）获取对象 返回值是数组

+ getElementsByClassName() --- 通过元素的class属性来获取元素  返回值是数组

  ```js
  <!DOCTYPE html>
  <html lang="en">
  <head>
      <meta charset="UTF-8">
      <title>Title</title>
      <script>
          function run1() {
              alert("点了");
          }
      </script>
  </head>
  <body>
          <!--
          事件源  按钮button
          事件    点击onclick 
          监听器  run1()
          注册/绑定监听器  onclick="run1()"
          -->
          <input type="button" name="按钮" value="点我" onclick="run1()">
  </body>
  </html>
  ```
  
  

#### 3.2元素对象常见属性

+ value	元素对象.value 

   获取 元素对象的value属性  也可以重新赋值 元素对象.value 

+ className

  获取元素对象className属性  也可以赋值

+ checked       默认选中属性

  获取元素对象checked属性 也可以赋值  （checked是默认选中属性返回值只有true和flase）

  <img src="https://gitee.com/thisisafigurebed/figure-bed/raw/master/jsNode/8.png" style="zoom:70%">
  <img src="https://gitee.com/thisisafigurebed/figure-bed/raw/master/jsNode/9.png" style="zoom:70%">

+ innerHTML 

  获取元素对象内容体，使用方法同上除此以外还可以追加 元素对象.innerHTML+="sssss";
  <img src="https://gitee.com/thisisafigurebed/figure-bed/raw/master/jsNode/11.png" style="zoom:70%">
  <img src="https://gitee.com/thisisafigurebed/figure-bed/raw/master/jsNode/10.png" style="zoom:70%">

## 04_JS事件

> js事件是鼠标移动点击或者热键的动作
>
> 通过js事件可以完成页面的指定特效

```js
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <script>
        function run1() {
            alert("点了");
        }
    </script>
</head>
<body>
<input type="button" name="按钮" value="点我" onclick="run1()">
</body>
</html>
```
<img src="https://gitee.com/thisisafigurebed/figure-bed/raw/master/jsNode/12.png" style="zoom:70%">

#### 4.1js事件驱动机制简述

+ 事件源

  产生事件的组件

+ 事件

  由事件源产生的动作或者事情

+ 监听器

  处理事件源产生的事情

+ 注册/绑定监听器

  让监听器时刻监听事件源是否有指定事件产生，产生则执行事件动作

#### 4.2js常见事件

+ onclick() 	----点击事件

+ onfocus()    ----获取焦点事件

  焦点事件是整个页面的注意力 默认一个页面在最多只能有一个焦点

  反应的是用户目前的关注点组件（例如文本框中闪烁的小竖线）

  <img src="https://gitee.com/thisisafigurebed/figure-bed/raw/master/jsNode/13.png" style="zoom:70%" >

+ onblur()    ----失去焦点事件

  元素组件失去焦点时触发

  ```js
  <body>
  <input type="text" id="t1" value="请输入balabala" onfocus="run1()" onblur="run2()">
  <script>
      function run1() {
          let t1Fun = document.getElementById("t1");
          t1Fun.value = "获取焦点后";
      }
      function run2(){
          let t1Fun = document.getElementById("t1");
          t1Fun.value = "失去焦点后";
      }
  </script>
  </body>
  ```
 <img src="https://gitee.com/thisisafigurebed/figure-bed/raw/master/jsNode/14.png" style="zoom:70%">

+ <font color=red size=5>值得一提，写在body中切出网页触发，输入完用户名失去焦点时候查询用户名是否存在</font>

```js
<body onfocus="run3()">
<script>
    function run3() {
        alert("失去焦点")
    }
</script>
</body>
```

+ onchange()    ----域内容改变事件

  元素组件值发生改变时候触发

  ```js
  <select onchange="run3()">		//下拉选择框
      <option value="bj">北京</option>
      <option value="sh">上海</option>
      <option value="gz">广州</option>
  </select>
  <script>
      function run3() {
          alert("值发生改变了");   //点击下拉框选择框选项时触发
      }
  </script>
  ```

+ onload()    ----加载完毕事件

  元素组件加载完时触发   例如获取元素对象时候为防止数据丢失可以先等待元素加载完成

  ```js
  <body onload="run3()">
  <script>
      function run3() {
          alert("加载完成");
      }
  </script>
  </body>
  ```

+ onsubmit()    ----表单提交事件

  表单提交按钮点击时触发。可以进行表单校验

  返回布尔类型，true提交，false阻止提交	

  ```js
  <body >
  <form onsubmit="return  run1()">
  <input type="text" name="user" value="please input:">
  <input type="submit" value="提交1">
  </form>
  <script>
      function run1() {
              alert("点击按钮被点击了");
              return true;
      }
  </script>
  </body>
  ```

+ onkeyup() onkeydown()    ----键位弹起和按下事件

  组件中输入某些内容时键盘键位弹起按下时触发该事件

  onkeyup()  弹起会触发（bug如果摁下去不放就不会触发）

  onkeydown()   按下会触发 没有上面的bug

  ```js
  <body >
  <input type="text" id="t1" name="user" value="please input your name" onfocus="run1()" onkeyup="run2()" onkeydown="run3()">
  <script>
      function run1() {
          var name1 = document.getElementsByName("user");
          name1[0].value = "";
      }
      //弹起触发 比如最后的i按不放就不会触发
      function run2() {
          var name1 = document.getElementsByName("user");
  
          if (name1[0].value === "woshi") {
              alert("检测出来您输入woshi");    //必须从一开始输入woshi
          }
      }
      //按下触发
      function run3() {
          var name1 = document.getElementsByName("user");
          if (name1[0].value === "woshi") {
              alert("111111检测出来您输入woshi");    //必须从一开始输入woshi
          }
      }
  </script>
  </body>
  ```

+ onmouseover()    ----鼠标移入事件

+ onmouuseout()    ----鼠标移出事件

  ```js
  <body >
  <input type="text" name="input" value="please innout" onmouseover="run1()" onmouseout="run2()">
  <script>
      function run1() {
          var inp = document.getElementsByName("input");
          inp[0].value = "鼠标移入了";
      }
      function run2() {
          var inp = document.getElementsByName("input");
          inp[0].value = "鼠标移出了";
      }
  </script>
  </body>
  ```

  

#### 4.3两种事件绑定方式

+ 元素事件句柄绑定

  > 将事件以元素属性的方式写到标签内部，进而绑定对应函数
  >
  > 优点：开发快捷  可以绑定多个参数  传参方便
  >
  > 缺点: js和html高度融合在一起不利于多个部门的项目开发维护
  + 单个函数绑定
  
  ```js
  <body>
  <input type="text" name="input" value="1111" onclick="run1()"><br/>
  <input type="text" name="input" value="2222" onclick="run2('带参调用')"><br/>
  <input type="text" name="input" value="3333" onclick="run3(this)"><br/>
  <script>
      //无参调用
      function run1() {
          alert("无参调用");
      }
      //含参调用，双引号原则外双内单
      function run2(str) {
          alert(str);
      }
      //参数为对象  this指的是当前对象   
      function run3(obj) {
          alert(obj.value);
      }
  </script>
  </body>
  ```
  
  + 多个函数绑定    逗号隔开
  
    ```js
    <!--多个函数绑定-->
    <input type="text" name="input" value="1111" onclick="run1(),run2(),run3()"><br/>
    ```
  
+ DOM绑定方式

  > 使用DOM的属性方式绑定事件
  >
  > 优点： html和js完全分离
  >
  > 缺点： 一个事件只能绑定一个函数   解决：匿名函数
  >
  > 			不能传递参数    解决：匿名函数
  
  ```js
  <input type="text" name="input" value="1111" ><br/>
  <input type="text" id="t1" value="2222" ><br/>
  <script>
      function run1() {
          alert("窗口加载完毕");
      }
      //dom绑定1    对象名.属性
      window.onload = run1();
      //dom绑定2   对象名.属性 = 匿名函数
      window.onload = function () {
          run1();
      };
  
      //通过id值获取对象
      function run2() {
          var id = document.getElementById("t1");
          id.onclick = function () {
              run1();
          };
      }
      window.onload = run2();
  </script>
  </body>
  ```
  
  