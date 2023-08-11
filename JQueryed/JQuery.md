[TOC]

# JQuery

## 01_简介

1. jQuery是一个快速、简洁的JavaScript框架，是继Prototype之后又一个优秀的JavaScript代码库（框架）于2006年1月由John Resig发布。jQuery设计的宗旨是“write Less，Do More”，即倡导写更少的代码，做更多的事情。它封装JavaScript常用的功能代码，提供一种简便的JavaScript设计模式，优化HTML文档操作、事件处理、动画设计和Ajax交互。
2. JQuery有丰富的插件,

## 02_下载

https://code.jquery.com/jquery-1.12.4.js

https://code.jquery.com/jquery-1.12.4.min.js

项目和引入普通js一样引入jquery包


## 03_ 入口函数

1. 项目加载完毕后的方法 对比js

```js
<script src="js/jquery-1.12.4.js"></script>
<script>
    // 1. 原生js
    window.onload = function (ev) {
        //网页加载完毕后执行的方法
    };
    // 2. JQuery方法
    $(document).ready(function () {
        //网页加载完毕后执行的方法
    });
</script>
```

2. 加载顺序
   + 原生js会等待DOM对象和图片等资源加载完毕再启动入口函数
   + jquery等待DOM对象加载完毕就启动,不等待资源加载完毕

3. 编写多个入口函数
   + 原生JS编写多个会覆盖
   + JQuery不会覆盖
   
4. 入口函数几种写法

   ```js
   <script src="js/jquery-1.12.4.js"></script>
   <script>
       $(document).ready(function () {
           alert("NO.1")
       });
       jQuery(document).ready(function () {
           alert("NO.2")
       });
       //推荐
       $(function () {
           alert("NO.3")
       });
       jQuery(function () {
           alert("NO.4")
       });
   </script>
   ```

5. jQuery冲突问题

   假如还使用了其他框架也使用了'$'关键字则会冲突

   解决两种方式:

   1. 释放$符号

      ```js
          jQuery.noConflict();
          jQuery(function () {
              alert("NO.3")
          });
      ```

   2. 更换关键字

      ```js
          let lalala = jQuery.noConflict();
          lalala(function () {
              alert("NO.3")
          });
      ```

## 04_核心函数$();

1. 接收一个函数作为入口函数: 例如入口函数
2. 接收一个字符串
   1. 选择器字符串 比如class或者id(用于查找元素)
   2. 接收一个代码片段 
   3. 接收一个DOM元素

```js
<script src="js/jquery-1.12.4.js"></script>
<script>
    $(function () {
        // 接收字符串
        // 1. 字符串选择器   返回一个jQuery对象,对象中保存了找到的DOM对象
        let $box1 = $(".box1");
        let $box2 = $("#box2");
        console.log($box1);
        console.log($box2);

        // 2. 接收一个代码片段  返回一个jQuery对象,对象中保存了创建的DOM元素
        let $p = $("<p>我是段落</p>");
        $box1.append($p);

        // 3. 接收一个DOM元素  返回一个jQuery对象,对象中包装了jQuery对象返回 
        let byTagName = document.getElementsByTagName("span");
        console.log(byTagName);
        let $span = $("span");
        console.log($span);
    });
</script>
<body>
<div></div>
<div class="box1"></div>
<div id="box2"></div>
<span>我是span</span>
```

## 05_jQuery对象

1. jQuery对象是一种伪数组

   1. 伪数组对象:有0到length-1属性,有length属性

2. 代码同上 我们来看看浏览器console打印
   <img src="https://gitee.com/thisisafigurebed/figure-bed/raw/master/jqueryNode/1.png">

## 06_静态和示例方法(解释)

1. 直接给类添加的方法是静态方法
2. 通过类的原型(prototype)添加,

```js
<script src="js/jquery-1.12.4.js"></script>
<script>
    //静态方法演示
    function TestSta(){
    }
    TestSta.staticFunc = function (){
        alert("testSta")
    }
    TestSta.staticFunc();

    //示例方法
    TestSta.prototype.intanceMethod = function () {
        alert("intanceMethod");
    };
    var a = new TestSta();
    a.intanceMethod();
</script>
```

## 07_jQuery的静态方法

### 7.1 each遍历方法

返回一个数组(遍历谁就返回谁),不支持对遍历的数组进行处理。

```js
<script src="js/jquery-1.12.4.js"></script>
<script>
    //数组
    let arr = [1,3,5,7,9];
    let arr2 = {0:2,1:3,2:5,3:87,length:4};
    //each遍历静态方法
    // js 原生  不能遍历伪数组
    arr.forEach(function (value, index) {
        // console.log(index+":"+value);
    });
    //jQuery
    $.each(arr, function (index,value ) {
        console.log(index+":"+value);
    });
    $.each(arr2, function (index,value ) {
        console.log(index+":"+value);
    });
</script>
```

### 7.2 map方法

默认返回值是一个空数组,通过return可以对数组进行加工,生成一个新的数组并返回

```js
<script src="js/jquery-1.12.4.js"></script>
<script>
    //数组
    let arr = [1,3,5,7,9];
    let arr2 = {0:2,1:3,2:5,3:87,length:4};
    //map静态方法

    //js原生 不能遍历伪数组
    arr.map(function (value, index, array) {
        // console.log(value, index, array);
    });

    //jQuery
    $.map(arr, function (value, index) {
        // console.log(value, index);
    });
    $.map(arr2, function (value, index) {
        console.log(value, index);
    });
</script>
```

### 7.3 trim方法去除字符串中的空格

```js
<script src="js/jquery-1.12.4.js"></script>
<script>
    let s = "    lalala    ";
    console.log("----"+s+"----");
    let trim = $.trim(s);
    console.log("----"+trim+"----");
</script>
```

### 7.4 各种is方法

1. isWindow():判断是不是window对象
2. isArray()
3. isFunction()

### 7.5 holdReady()方法

js的入口函数等待页面所有资源加载完毕才执行。

jQuery入口函数是等待DOM对象加载完毕就执行

使用holdReady()方法可以暂停入口函数。 

需要时可以给事件添加方法使其恢复

## 06_操纵属性节点的方法

1. attr()

2. removeAttr()

   ```js
   <script src="js/jquery-1.12.4.js"></script>
   <script>
       /*
       *获取:无论找到多少只会返回第一个
       *设置值:找到多少设置多少
       *设置不存在的属性节点:就会新增属性节点
       * */
       $(function () {
           //设置节点属性值
           $("input").attr({name: "hhhh", value: "hhhhhhh"});
           //设置节点不存在的属性值
           $("span").attr({name: "asasas", id: "qq"})
           //得到值
           console.log($("input").attr("name"));
           console.log($("span").attr("id"));
           //移除多个值用空格间隔
           $("span").removeAttr("class id");
           //得到值
           console.log($("span").attr("id"));
       });
   </script>
   <body>
   <input type="text" class="i1" name="t1" value="lalala">
   <span class="s1"></span>
   </body>
   </html>
   ```

3. prop()

4. removeProp()

5. attr和prop基本上使用一样,返回值不一样

   ```js
   <script src="js/jquery-1.12.4.js"></script>
   <script>
       /*
       *获取:无论找到多少只会返回第一个
       *设置值:找到多少设置多少
       *设置不存在的属性节点:就会新增属性节点
       * */
       $(function () {
           //attr
           console.log($("input").eq(0).attr("checked"));  //checked
           console.log($("input").eq(1).attr("checked"));  //undefined
           //prop
           console.log($("input").eq(0).prop("checked"));  //true
           console.log($("input").eq(1).prop("checked"));  //false
       });
   </script>
   <body>
   <input type="checkbox" checked="checked">
   <input type="checkbox">
   </body>
   </html>
   ```

   