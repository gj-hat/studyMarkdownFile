[TOC]

# Django操作数据库

## Django访问流程

<img src="https://img-blog.csdnimg.cn/20181114205711636.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQyODQyMzM1,size_16,color_FFFFFF,t_70" style="zoom:80%;" />

1. 用户通过浏览器发送请求,到达WSGI服务器,使用其handler方法来处理HTTP请求(其实最终是调用wsgiref.handlers.BaseHandler中的run方法处理);
2. 请求到达request中间件,中间件对request请求进行预处理或者直接返回response;
3. 如果没有response,到达url进行匹配,找到相应的视图函数;
4. 视图函数做出相应处理或者直接返回response;
5. 视图层View 可以通过Model层与数据库进行交互;
6. 取得相应数据后返回给template模板;
7. 通过response展示给客户.

## 空项目结构

```
|-- ProjectName
|   |-- __init__.py
|   |-- asgi.py
|   |-- settings.py
|   |-- urls.py
|   |-- wsgi.py
|-- manage.py
```

- **ProjectName:** 项目的容器。
- **manage.py:** 一个实用的命令行工具，可让你以各种方式与该 Django 项目进行交互。
- **ProjectName/__init__.py:** 一个空文件，告诉 Python 该目录是一个 Python 包。
- **ProjectName/asgi.py:** 一个 ASGI 兼容的 Web 服务器的入口，以便运行你的项目。
- **ProjectName/settings.py:** 该 Django 项目的设置/配置。
- **ProjectName/urls.py:** 该 Django 项目的 URL 声明; 一份由 Django 驱动的网站"目录"。
- **ProjectName/wsgi.py:** 一个 WSGI 兼容的 Web 服务器的入口，以便运行你的项目。

###  ASGI和WSGI区别

以上，WSGI是基于HTTP协议模式的，不支持WebSocket，而ASGI的诞生则是为了解决Python常用的WSGI不支持当前Web开发中的一些新的协议标准。同时，ASGI对于WSGI原有的模式的支持和WebSocket的扩展，即ASGI是WSGI的扩展。



## HelloWord

### views和url配置

1. 在先前创建的 ProjectName 目录下的 ProjectName目录新建一个 views.py 文件，并输入代码：

    ```python
    from django.http import HttpResponse
    
    def hello(request):
        return HttpResponse("你好世界!")
    
    def hello2(request):
        return HttpResponse("Hello Word!")
    
    ```

2. 绑定 URL 与视图函数。打开 urls.py 文件，删除原来代码，将以下代码复制粘贴到 urls.py 文件中：

   ```python
   from django.urls import include, re_path
   from . import views
   
   # re_path是django2的写法
   # ^匹配开始  $匹配结束
   urlpatterns = [re_path(r'^$', views.hello), re_path('hello2/', views.hello2)]
   ```

    ### 目录

   |-- ProjectName
   |   |-- `__init__`.py
   |   |-- asgi.py
   |   |-- settings.py
   |   |-- urls.py
   |   |-- wsgi.py
   |   |-- views.py

   |-- manage.py

   ### 讲解

   1. 浏览器访问localhost:8000
   2. 浏览器默认通过get请求访问服务器
   3. 到达WSGI服务器,使用wsgiref.handlers.BaseHandler中的run方法处理
   4. 进入urls.py匹配hello视图
   5. 在views.py中找到hello类
   6. hello类继承了request并做出response相应
   7. 返回给浏览器

   ### 效果

   1. 访问localhost:8000返回views.hello的响应(你好世界)
   2. 访问localhost:8000/hello2返回views.hello2的响应(Hello Word)

## 模板

> 上一章节中我们使用 django.http.HttpResponse() 来输出 "Hello World！"。该方式将数据与视图混合在一起，不符合 Django 的 MVC 思想。
>
> 本章节我们将为大家详细介绍 Django 模板的应用，模板是一个文本，用于分离文档的表现形式和内容。

### 模板应用实例

> 使用模板给HTML中输出数据

在 **ProjectName**目录底下创建 templates 目录并建立 runoob.html文件

#### 目录结构

|-- ProjectName
|   |-- `__init__`.py
|   |-- asgi.py
|   |-- settings.py
|   |-- urls.py
|   |-- wsgi.py
|   |-- views.py
|-- manage.py
|-- templates
|   |-- runoob.html

#### 新建html文件

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<h1>{{ hello }}</h1>
</body>
</html>
```

**变量**模板语法:

```html
view：｛"HTML变量名" : "views变量名"｝
HTML：｛｛变量名｝｝
```



#### 向Django说明模板文件的路径

修改HelloWorld/settings.py，修改 TEMPLATES 中的 DIRS 为 **[os.path.join(BASE_DIR, 'templates')]**，如下所示:

```python
...
TEMPLATES = [
    {
        'BACKEND': 'django.template.backends.django.DjangoTemplates',
        'DIRS': [os.path.join(BASE_DIR, 'templates')],       # 修改位置
        'APP_DIRS': True,
        'OPTIONS': {
            'context_processors': [
                'django.template.context_processors.debug',
                'django.template.context_processors.request',
                'django.contrib.auth.context_processors.auth',
                'django.contrib.messages.context_processors.messages',
            ],
        },
    },
]
...
```

#### 修改 views.py

增加一个新的对象，用于向模板提交数据：

```python
def runoob(request):
    return render(request, 'runoob.html', {'hello': 'Hello World!'})
```

#### 修改urls.py

```python
# ^匹配开始  $匹配结束
urlpatterns = [re_path(r'^$', views.hello),
               re_path('hello2/', views.hello2),
               re_path('runoob/', views.hello2)]
```

### 过滤器标签

模板语法：{{ 变量名 | 过滤器：可选参数 }}

模板过滤器可以在变量被显示前修改它，过滤器使用管道字符，如下所示：

```python
{{ name|lower }}
```

{{ name }} 变量被过滤器 lower 处理后，文档大写转换文本为小写。

过滤管道可以被* 套接* ，既是说，一个过滤器管道的输出又可以作为下一个管道的输入：

```python
{{ my_list|first|upper }}
```

#### 常用过滤器函数

1. truncatewords

   ```python
   {{ bio|truncatewords:"30" }}
   # 显示bio变量的前30个字
   ```

2. length

   返回对象的长度，适用于字符串和列表。

   字典返回的是键值对的数量，集合返回的是去重后的长度。

3. filesizeformat

   以更易读的方式显示文件的大小（即'13 KB', '4.1 MB', '102 bytes'等）。

   字典返回的是键值对的数量，集合返回的是去重后的长度。

4. date

   根据给定格式对一个日期变量进行格式化。

   格式 Y-m-d H:i:s返回 年-月-日 小时:分钟:秒 的格式时间。

5. truncatechars如果字符串包含的字符总个数多于指定的字符数量，那么会被截断掉后面的部分。

   截断的字符串将以 **...** 结尾。

6. safe(可以定义html)

   将字符串标记为安全，不需要转义。

   要保证 views.py 传过来的数据绝对安全，才能用 safe。

   和后端 views.py 的 mark_safe 效果相同。

   Django 会自动对 views.py 传到HTML文件中的标签语法进行转义，令其语义失效。加 safe 过滤器是告诉 Django 该数据是安全的，不必对其进行转义，可以让该数据语义生效。

   ```python
   from django.shortcuts import render
   
   def runoob(request):
       views_str = "<a href='https://www.runoob.com/'>点击跳转</a>"
       return render(request, "runoob.html", {"views_str": views_str})
   ```

   ```html
   {{ views_str|safe }}
   ```

### if/else 标签

基本语法格式如下：

```html
{% if condition %}
... display
{% endif %}
```

或者：

```html
{% if condition1 %}
... display 1
{% elif condition2 %}
... display 2
{% else %}
... display 3
{% endif %}
```

**根据条件判断是否输出。if/else 支持嵌套。**

{% if %} 标签接受 and ， or 或者 not 关键字来对多个变量做判断 ，或者对变量取反（ not )，例如：

```html
{% if athlete_list and coach_list %}
athletes 和 coaches 变量都是可用的。
{% endif %}
```

#### 示例

1. views.py

```python
from django.shortcuts import render

def runoob(request):
views_num = 88
return render(request, "runoob.html", {"num": views_num})
```

2. 模板html

```html
{%if num > 90 and num <= 100 %}
优秀
{% elif num > 60 and num <= 90 %}
合格
{% else %}
一边玩去～
{% endif %}
```

 ### for 标签

{% for %} 允许我们在一个序列上迭代。

与 Python 的 for 语句的情形类似，循环语法是 for X in Y ，Y 是要迭代的序列而 X 是在每一个特定的循环中使用的变量名称。

每一次循环中，模板系统会渲染在 **{% for %}** 和 **{% endfor %}** 之间的所有内容。

例如，给定一个运动员列表 athlete_list 变量，我们可以使用下面的代码来显示这个列表：

```html
<ul>
{% for athlete in athlete_list %}
    <li>{{ athlete.name }}</li>
{% endfor %}
</ul>
```

**{% empty %}**

可选的 {% empty %} 从句：在循环为空的时候执行（即 in 后面的参数布尔值为 False ）。

```html
{% for i in listvar %}
    {{ forloop.counter0 }}
{% empty %}
    空空如也～
{% endfor %}
```

### ifequal/ifnotequal 标签

{% ifequal %} 标签比较两个值，当他们相等时，显示在 {% ifequal %} 和 {% endifequal %} 之中所有的值。

下面的例子比较两个模板变量 user 和 currentuser :

```html
{% ifequal user currentuser %}
    <h1>Welcome!</h1>
{% endifequal %}
```

和 {% if %} 类似， {% ifequal %} 支持可选的 {% else%} 标签：8

```html
{% ifequal section 'sitenews' %}
    <h1>Site News</h1>
{% else %}
    <h1>No News Here</h1>
{% endifequal %}
```

### 注释标签

Django 注释使用 {# #}。

```html
{# 这是一个注释 #}
```

### include 标签

{% include %} 标签允许在模板中包含其它的模板的内容。

下面这个例子都包含了 nav.html 模板：

```html
{% include "nav.html" %}
```

### csrf_token

csrf_token 用于form表单中，作用是跨站请求伪造保护。

如果不用｛% csrf_token %｝标签，在用 form 表单时，要再次跳转页面会报403权限错误。

用了｛% csrf_token %｝标签，在 form 表单提交数据时，才会成功。

**解析：**

首先，向服务器发送请求，获取登录页面，此时中间件 csrf 会自动生成一个隐藏input标签，该标签里的 value 属性的值是一个随机的字符串，用户获取到登录页面的同时也获取到了这个隐藏的input标签。

然后，等用户需要用到form表单提交数据的时候，会携带这个 input 标签一起提交给中间件 csrf，原因是 form 表单提交数据时，会包括所有的 input 标签，中间件 csrf 接收到数据时，会判断，这个随机字符串是不是第一次它发给用户的那个，如果是，则数据提交成功，如果不是，则返回403权限错误。

## Django 模型

Django 对各种数据库提供了很好的支持，包括：PostgreSQL、MySQL、SQLite、Oracle。

Django 为这些数据库提供了统一的调用API。 我们可以根据自己业务需求选择不同的数据库。

MySQL 是 Web 应用中最常用的数据库。本章节我们将以 Mysql 作为实例进行介绍。

如果你没安装 mysql 驱动，可以执行以下命令安装：

```python
sudo pip3 install pymysql
```

在项目的 settings.py 文件中找到 DATABASES 配置项，将其信息修改为：

```python
DATABASES = { 
    'default': 
    { 
        'ENGINE': 'django.db.backends.mysql',    # 数据库引擎
        'NAME': 'runoob', # 数据库名称
        'HOST': '127.0.0.1', # 数据库地址，本机 ip 地址 127.0.0.1 
        'PORT': 3306, # 端口 
        'USER': 'root',  # 数据库用户名
        'PASSWORD': '123456', # 数据库密码
    }  
}
```



### Django ORM

Django 模型使用自带的 ORM。

对象关系映射（Object Relational Mapping，简称 ORM ）用于实现面向对象编程语言里不同类型系统的数据之间的转换。

ORM 在业务逻辑层和数据库层之间充当了桥梁的作用。

ORM 是通过使用描述对象和数据库之间的映射的元数据，将程序中的对象自动持久化到数据库中。

<img src="https://www.runoob.com/wp-content/uploads/2020/05/django-orm1.png" >

**ORM 对应关系表：**

<img src="https://www.runoob.com/wp-content/uploads/2020/05/orm-object.png">

### 定义数据库模型

1. 创建 APP

Django 规定，如果要使用模型，必须要创建一个 app。我们使用以下命令创建一个 TestModel 的 app:(命令行项目路径下)

```shell
django-admin.py startapp TestModel
```

2. 目录结构如下：

```
ProjectName
|-- ProjectName 
|-- manage.py
...
|-- TestModel
|   |-- __init__.py
|   |-- admin.py
|   |-- models.py
|   |-- tests.py
|   `-- views.py
```

3. 声明数据库表

   修改 TestModel/models.py 文件，代码如下：

   ```python
   # models.py
   from django.db import models
    
   class Test(models.Model):
       name = models.CharField(max_length=20)
   ```

   以上的类名代表了数据库表名，且继承了models.Model，类里面的字段代表数据表中的字段(name)，数据类型则由CharField（相当于varchar）、DateField（相当于datetime）， max_length 参数限定长度。

4. 安装app

   在 settings.py 中找到INSTALLED_APPS这一项，如下：

   ```python
   INSTALLED_APPS = (
       'django.contrib.admin',
       'django.contrib.auth',
       'django.contrib.contenttypes',
       'django.contrib.sessions',
       'django.contrib.messages',
       'django.contrib.staticfiles',
       'TestModel',               # 添加此项
   )
   ```

   5. 命令行运行

      ```shell
      $ python36 manage.py migrate   # 创建表结构
      $ python36 manage.py makemigrations TestModel  # 让 Django 知道我们在我们的模型有一些变更
      $ python36 manage.py migrate TestModel   # 创建表结构
      ```

      看到几行 "Creating table…" 的字样，你的数据表就创建好了。

      ```sql
      Creating tables ...
      ……
      Creating table TestModel_test  #我们自定义的表
      ……
      ```

      表名组成结构为：应用名_类名（如：TestModel_test）。

      **注意：**尽管我们没有在 models 给表设置主键，但是 Django 会自动添加一个 id 作为主键。

    

### 数据库操作

接下来我们在 HelloWorld 目录中添加 testdb.py 文件（下面介绍），并修改 urls.py：

```python
from django.urls import path
from . import views,testdb
urlpatterns = [
    path('runoob/', views.runoob),
    path('testdb/', testdb.testdb),
]
```

#### 添加数据

添加数据需要先创建对象，然后再执行 save 函数，相当于SQL中的INSERT：

```python
# -*- coding: utf-8 -*-
 
from django.http import HttpResponse
 
from TestModel.models import Test
 
# 数据库操作
def testdb(request):
    test1 = Test(name='runoob')
    test1.save()
    return HttpResponse("<p>数据添加成功！</p>")
```

#### 修改数据

修改数据可以使用 save() 或 update():

```python
# -*- coding: utf-8 -*-
 
from django.http import HttpResponse
 
from TestModel.models import Test
 
# 数据库操作
def testdb(request):
    # 修改其中一个id=1的name字段，再save，相当于SQL中的UPDATE
    test1 = Test.objects.get(id=1)
    test1.name = 'Google'
    test1.save()
    
    # 另外一种方式
    #Test.objects.filter(id=1).update(name='Google')
    
    # 修改所有的列
    # Test.objects.all().update(name='Google')
    
    return HttpResponse("<p>修改成功</p>")
```



#### 删除数据

删除数据库中的对象只需调用该对象的delete()方法即可：

```python
# -*- coding: utf-8 -*-
 
from django.http import HttpResponse
 
from TestModel.models import Test
 
# 数据库操作
def testdb(request):
    # 删除id=1的数据
    test1 = Test.objects.get(id=1)
    test1.delete()
    
    # 另外一种方式
    # Test.objects.filter(id=1).delete()
    
    # 删除所有数据
    # Test.objects.all().delete()
    
    return HttpResponse("<p>删除成功</p>")
```



#### 查询数据

Django提供了多种方式来获取数据库的内容，如下代码所示：

```python
# -*- coding: utf-8 -*-
 
from django.http import HttpResponse
 
from TestModel.models import Test
 
# 数据库操作
def testdb(request):
    # 初始化
    response = ""
    response1 = ""
    
    
    # 通过objects这个模型管理器的all()获得所有数据行，相当于SQL中的SELECT * FROM
    list = Test.objects.all()
        
    # filter相当于SQL中的WHERE，可设置条件过滤结果
    response2 = Test.objects.filter(id=1) 
    
    # 获取单个对象
    response3 = Test.objects.get(id=1) 
    
    # 限制返回的数据 相当于 SQL 中的 OFFSET 0 LIMIT 2;
    Test.objects.order_by('name')[0:2]
    
    #数据排序
    Test.objects.order_by("id")
    
    # 上面的方法可以连锁使用
    Test.objects.filter(name="runoob").order_by("id")
    
    # 输出所有数据
    for var in list:
        response1 += var.name + " "
    response = response1
    return HttpResponse("<p>" + response + "</p>")
```

