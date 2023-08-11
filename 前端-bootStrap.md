[TOC]

##  BootStrap前端框架

## 01_bootstrap概述

### 1.1什么是bootstrap？ bootstrap有什么作用

> Bootstrap是集html，javascript，css的一套前端框架

框架预定了一套html ，csss样式和对应的js代码

开发中只需要编写html结果并添加bootstrap固定对应class样式

+ 作用
  1. 使web开发更快捷
  2. 支持相应式开发，解决了移动互联网前端开发问题

## 02_bootstarp搭建

### 2.1 相关下载

+ bootstarp：https://v3.bootcss.com/getting-started/
+ jquerp：https://www.jq22.com/jquery-info122

### 2.2 搭建

1. 打开bootstrap/dist文件夹

2. 复制./dist/*到前端项目目录下

3. 将下载的jquery放到前端项目/js/目录下
   <img src="https://gitee.com/thisisafigurebed/figure-bed/raw/master/bootstrapNode/1.png" style="zoom:70%">

### 2.3 bootstrap模板 
+ 在线网页模板

  https://v3.bootcss.com/getting-started/#template

+ 本地导入模板

```js
<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="utf-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <!-- 上述3个meta标签*必须*放在最前面，任何其他内容都*必须*跟随其后！ -->
    <title>Bootstrap 101 Template</title>
    <!-- Bootstrap -->
    <link href="./css/bootstrap.min.css" rel="stylesheet">
    <!-- jQuery (Bootstrap 的所有 JavaScript 插件都依赖 jQuery，所以必须放在前边) -->
    <script src="./js/jquery-3.5.1.min.js"></script>
    <!-- 加载 Bootstrap 的所有 JavaScript 插件。你也可以根据需要只加载单个插件。 -->
    <script src="./js/bootstrap.min.js"></script>
    
</head>
<body>
<h1>你好，世界！</h1>

</body>
</html>
```

## 03_布局容器

> bootstrap必须需要至少一个布局容器
>
> 帮助手册：全局css样式---->概述---->布局容器

Bootstrap 需要为页面内容和栅格系统包裹一个 `.container` 容器。我们提供了两个作此用处的类。注意，由于 `padding` （css中内容与边框的距离）等属性的原因，这两种 容器类不能互相嵌套。

### 3.1 布局容器1

`.container` 类用于固定宽度并支持响应式布局的容器。

特点：居中，两端留白

```js
<div class="container">
  ...
</div>
```

### 3.2 布局容器2

`.container-fluid` 类用于 100% 宽度，占据全部视口（viewport）的容器。

```js
<div class="container-fluid">
  ...
</div>
```

### 3.3 示例

```js
<div class="container" style="border: 2px solid red;">
    <h1>你好世界</h1>
</div>
<div class="container-fluid" style="border: 2px solid blue;">
    <h1>你好世界</h1>
</div>
```
<img src="https://gitee.com/thisisafigurebed/figure-bed/raw/master/bootstrapNode/2.png" style="zoom:70%">

## 04_栅格系统

> Bootstrap 提供了一套响应式、移动设备优先的流式栅格系统，随着屏幕或视口（viewport）尺寸的增加，系统会自动分为最多12列。需要设定元素占了多少列

### 4.1栅格系统特点

+ 栅格特点
  + “.row行”必须包含在“.container”（固定宽度内） 或 ”container-fluid“中

  + 行使用的样式”.row"  列使用的样式`“.col-*-*”`   元素内容应当放在列中

  + 书写方式：容器-行-列

    参照html：表格-行-单元格 

+ 栅格参数 ： col-屏幕尺寸-占用列数

  <font color=red size=5>列数小于12还在一行   大于12另起一行</font>

  <font color=red size=5>行列可以无限嵌套</font>

+ 屏幕尺寸

  | 超小屏幕 手机 (<768px) | 小屏幕 平板 (≥768px)       | 中等屏幕 桌面显示器 (≥992px)                        | 大屏幕 大桌面显示器 (≥1200px) |            |
  | :--------------------- | :------------------------- | :-------------------------------------------------- | :---------------------------- | ---------- |
  | 栅格系统行为           | 总是水平排列               | 开始是堆叠在一起的，当大于这些阈值时将变为水平排列C |                               |            |
  | `.container` 最大宽度  | None （自动）              | 750px                                               | 970px                         | 1170px     |
  | 类前缀                 | `.col-xs-`                 | `.col-sm-`                                          | `.col-md-`                    | `.col-lg-` |
  | 列（column）数         | 12                         |                                                     |                               |            |
  | 最大列（column）宽     | 自动                       | ~62px                                               | ~81px                         | ~97px      |
  | 槽（gutter）宽         | 30px （每列左右均有 15px） |                                                     |                               |            |
  | 可嵌套                 | 是                         |                                                     |                               |            |
  | 偏移（Offsets）        | 是                         |                                                     |                               |            |
  | 列排序                 | 是                         |                                                     |                               |            |

+ 多个屏幕尺寸设置

  在<div class="col-sm-3 col-lg-3"></div>

  或者直接写希望尺寸的最小尺寸即可，大于它的会自动适配

+ 列偏移  

  `.col-md-offset-* *`属性实实现列偏移

   <div class="col-sm-1 col-sm-offset-1" >    //右偏移一个单位

+ 示例

  ```js
  <!--
          col-sm-列数   其中sm代表小尺寸  lg大尺寸 类似
  -->
  <div class="container">
      <!--  第一行  -->
      <div class="row" style="border: 1px solid red">
          <div class="col-sm-1" style="border: 1px solid red">
              你好
          </div>
          <div class="col-sm-3" style="border: 1px solid red">
              你好
          </div>
          <div class="col-sm-8" style="border: 1px solid red">
              你好
          </div>
      </div>
      <!--  第二行  -->
      <div class="row" style="border: 1px solid red">
          <div class="col-sm-4" style="border: 1px solid blue">
              你好
          </div>
          <div class="col-sm-2" style="border: 1px solid blue">
              你好
          </div>
          <div class="col-sm-6" style="border: 1px solid blue">
              你好
          </div>
      </div>
  </div>
  ```

  <img src="https://gitee.com/thisisafigurebed/figure-bed/raw/master/bootstrapNode/3.png" style="zoom:70%">

## 05_响应式工具

> 可以选择在在某些尺寸设备上的显示或隐藏

### 5.1 可用类

|                 | 超小屏幕手机 (<768px) | 小屏幕平板 (≥768px) | 中等屏幕桌面 (≥992px) | 大屏幕桌面 (≥1200px) |
| --------------- | --------------------- | ------------------- | --------------------- | -------------------- |
| `.visible-xs-*` | 可见                  | 隐藏                | 隐藏                  | 隐藏                 |
| `.visible-sm-*` | 隐藏                  | 可见                | 隐藏                  | 隐藏                 |
| `.visible-md-*` | 隐藏                  | 隐藏                | 可见                  | 隐藏                 |
| `.visible-lg-*` | 隐藏                  | 隐藏                | 隐藏                  | 可见                 |
| `.hidden-xs`    | 隐藏                  | 可见                | 可见                  | 可见                 |
| `.hidden-sm`    | 可见                  | 隐藏                | 可见                  | 可见                 |
| `.hidden-md`    | 可见                  | 可见                | 隐藏                  | 可见                 |
| `.hidden-lg`    | 可见                  | 可见                | 可见                  | 隐藏                 |

### 5.2 打印类（打印机上显示或隐藏）

## 06_列表

### 6.1 将所有列表放为一行   `.list-inline`

示例：

```html
<div class="container">
    <ul>
        <li>我是一</li>
        <li>我是二</li>
        <li>我是三</li>
    </ul>
    <ul class="list-inline">
        <li>我是一</li>
        <li>我是二</li>
        <li>我是三</li>
    </ul>
</div>
```

## 07_按钮

### 7.1 可作为按钮使用的标签或元素

为 `<a>`、`<button>` 或 `<input>` 元素添加按钮类（button class）即可使用 Bootstrap 提供的样式。

```html
<a class="btn btn-default" href="#" role="button">Link</a>
<button class="btn btn-default" type="submit">Button</button>
<input class="btn btn-default" type="button" value="Input">
<input class="btn btn-default" type="submit" value="Submit">
```

### 7.2 预定样式

```html
<!-- Standard button -->
<button type="button" class="btn btn-default">（默认样式）Default</button>

<!-- Provides extra visual weight and identifies the primary action in a set of buttons -->
<button type="button" class="btn btn-primary">（首选项）Primary</button>

<!-- Indicates a successful or positive action -->
<button type="button" class="btn btn-success">（成功）Success</button>

<!-- Contextual button for informational alert messages -->
<button type="button" class="btn btn-info">（一般信息）Info</button>

<!-- Indicates caution should be taken with this action -->
<button type="button" class="btn btn-warning">（警告）Warning</button>

<!-- Indicates a dangerous or potentially negative action -->
<button type="button" class="btn btn-danger">（危险）Danger</button>

<!-- Deemphasize a button by making it look like a link while maintaining button behavior -->
<button type="button" class="btn btn-link">（链接）Link</button>
```

<img src="https://gitee.com/thisisafigurebed/figure-bed/raw/master/bootstrapNode/4.png" style="zoom:70%">

### 7.3 尺寸

```html
<p>
  <button type="button" class="btn btn-primary btn-lg">（大按钮）Large button</button>
  <button type="button" class="btn btn-default btn-lg">（大按钮）Large button</button>
</p>
<p>
  <button type="button" class="btn btn-primary">（默认尺寸）Default button</button>
  <button type="button" class="btn btn-default">（默认尺寸）Default button</button>
</p>
<p>
  <button type="button" class="btn btn-primary btn-sm">（小按钮）Small button</button>
  <button type="button" class="btn btn-default btn-sm">（小按钮）Small button</button>
</p>
<p>
  <button type="button" class="btn btn-primary btn-xs">（超小尺寸）Extra small button</button>
  <button type="button" class="btn btn-default btn-xs">（超小尺寸）Extra small button</button>
</p>
```

### 7.4 激活状态

### 7.5 禁用状态

### 7.6 帮助文档

https://v3.bootcss.com/css/?#buttons-active

## 08_导航条

> 有关更多类型导航条：https://v3.bootcss.com/components/#navbar

 ```html
<!--导航条标签-->
<nav class="navbar navbar-default">
    <div class="container-fluid">
        <!--   导航图标和汉堡按钮     -->
        <div class="navbar-header">
            <button type="button" class="navbar-toggle collapsed" data-toggle="collapse" data-target="#bs-example-navbar-collapse-1" aria-expanded="false">
                <!--  阅读器（盲人）专用为汉堡盘扭设置名称  -->
                <span class="sr-only">这是原阅读器按钮</span>
                <!-- 汉堡盘扭的三个横线  -->
                <span class="icon-bar"></span>
                <span class="icon-bar"></span>
                <span class="icon-bar"></span>
            </button>
            <!-- 导航名称或导航图标（随着屏幕尺寸变化导航图标一直在）  -->
            <a class="navbar-brand" href="#">首页</a>
        </div>

        <!-- 导航条主体部分  -->
        <div class="collapse navbar-collapse" id="bs-example-navbar-collapse-1">
            <ul class="nav navbar-nav">
                <!--  当前页面（被激活状态） -->
                <li class="active"><a href="#">当前（被激活） <span class="sr-only">阅读器</span></a></li>
                <!--  未激活页面 -->
                <li><a href="#">没有被激活</a></li>
                <!--  下拉菜单 -->
                <li class="dropdown">
                    <a href="#" class="dropdown-toggle" data-toggle="dropdown" role="button" aria-haspopup="true" aria-expanded="false">更多 <span class="caret"></span></a>
                    <!-- 选项  -->
                    <ul class="dropdown-menu">
                        <li><a href="#">选项一</a></li>
                        <li><a href="#">选项一</a></li>
                        <li><a href="#">选项一</a></li>
                        <li role="separator" class="divider"></li>
                        <li><a href="#">另一种类</a></li>
                        <li role="separator" class="divider"></li>
                        <li><a href="#">另另一种类</a></li>
                    </ul>
                </li>
            </ul>
            <!-- 查询和提交按钮  -->
            <form class="navbar-form navbar-right">
                <div class="form-group">
                    <input type="text" class="form-control" placeholder="Search">
                </div>
                <button type="submit" class="btn btn-default">提交</button>
            </form>

        </div>
    </div>
</nav>
 ```

## 09_轮播图

> 详情：https://v3.bootcss.com/javascript/#carousel-examples

+ 轮播图自动定时循环`data-interval=毫秒值`写在轮播图div的class后即可
+ <font color=red size=5>多个轮播图必须更改其id值（因为一个页面id值是唯一的）</font>

```html
<!-- 轮播图  -->
<div id="carousel-example-generic" class="carousel slide" data-ride="carousel" style="height: 400px">
    <!-- 图片下的小圆点（代表当前是第几张） -->
    <ol class="carousel-indicators">
        <li data-target="#carousel-example-generic" data-slide-to="0" class="active"></li>
        <li data-target="#carousel-example-generic" data-slide-to="1"></li>
        <li data-target="#carousel-example-generic" data-slide-to="2"></li>
    </ol>

    <!-- 轮播图图片部分 -->
    <div class="carousel-inner" role="listbox">
        <div class="item active">
            <img src="..." alt="...">
            <div class="carousel-caption">
                图片1的说明信息
            </div>
        </div>
        <div class="item">
            <img src="..." alt="...">
            <div class="carousel-caption">
                ...
            </div>
        </div>
        ...
    </div>
    <!-- 左右控制按钮 -->
    <a class="left carousel-control" href="#carousel-example-generic" role="button" data-slide="prev">
        <span class="glyphicon glyphicon-chevron-left" aria-hidden="true"></span>
        <span class="sr-only">Previous</span>
    </a>
    <a class="right carousel-control" href="#carousel-example-generic" role="button" data-slide="next">
        <span class="glyphicon glyphicon-chevron-right" aria-hidden="true"></span>
        <span class="sr-only">Next</span>
    </a>
</div>
```

## 10_排版-对齐方式

> https://v3.bootcss.com/css/#type-alignment

控制组件位置排版  左中右(任意标签都行)

```html
<p class="text-left">Left aligned text.</p>
<p class="text-center">Center aligned text.</p>
<p class="text-right">Right aligned text.</p>
<p class="text-justify">Justified text.</p>
<p class="text-nowrap">No wrap text.</p>
```

## 11_表单

> https://v3.bootcss.com/css/#forms-example

+ 示例1

  <img src="https://gitee.com/thisisafigurebed/figure-bed/raw/master/bootstrapNode/5.png" style="zoom:70%">

代码：

md网站上自己找。老子不想写了

+ 表单校验（如如正确或者错误不同颜色表示）

  <img src="https://gitee.com/thisisafigurebed/figure-bed/raw/master/bootstrapNode/6.png" style="zoom:70%">

## 12_分页条

> https://v3.bootcss.com/components/#pagination