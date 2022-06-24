#    vue核心

 

## 01_简介



### 1.1 什么是VUE

……



### 1.2 特点

1. 采用组件化模式. 提高代码复用率. 且容易维护
2. 声明式编程, 不需要直接操作DOM

3. 使用虚拟DOM+优秀的Diff算法, 优化对DOM的操作



## 02_简单使用



快速入门:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <script src="../vue3.0.js"></script>
</head>
<body>
<div id="counter">counter: {{num}}</div>
</body>
<script>
    const helloVueApp = {
        data(){
            return{
                num: 1
            }
        }
    }
    Vue.createApp(helloVueApp).mount('#counter')
</script>

</html>

```



解释:

1. 引入vue3.js

1. 新建一个变量  变量内容如上 

1. 在Vue中新建一个容器,将 上面新建的对象存入Vue中  并声明这个对象作用的标签是counter




**流程分析**

1. 现有容器  再有vue实例
2. 当vue实例开始工作的时候,  查找容器内是否有vue的特殊语法, 如果有则开始解析并替换 



总结:

1. 想让vue工作, 就必须创建一个vue实例,  并且需要传入一个配置对象
2. `<div id="counter"></div>` 内称为容器  容器内的代码是html+特殊的vue语法
3. 容器内的代码称为模板

4. 容器和实例需要是一对一的关系 
5. 真实开发中只有一个Vue实例, 并且会搭配很多组件一起使用
6. `{{}}`中写的js的表达式(不是代码语句) 





## 03_VUE指令



 Vue的模板语法分为两大类:

+ 插值语法 

  功能: 用于解析标签体内的内容

  写法:  `{{}}`内的js表达式

+ 指令语法

  功能: 用于解析标签 (包括: 标签属性. 标签体内容 , 绑定事件 ……)

  备注: vue中有很多的指令. 且形式都是以v-??? 开头, 



Vue的模板数据绑定语法有三种:

+ 插值 {{}}
+ V-bind:
+ V-model=



**指令语法:**

1. `V-bind:`     数据绑定  (单向数据绑定   即 实例(js的值)-> 模板(页面现实的值))

   >  将`v-bind:`后的双引号内容替换为data数据内的内容

   从页面不能改变js变量的值  但是可以从js变量的值动态的改变页面的值 比如下面的鼠标悬停显示时间

   ``` html
       <h1>--------v-bind指令测试: -------------</h1>
       <h2>--------将vue的变量值绑定到html标签内的变量 -------------</h2>
       <span v-bind:title="message" style="border: 1px solid">
       将鼠标悬停一会 可以看到当前时间
     </span>
       <br> <br>
       <h2>--------单向绑定 将输入框的值->(单向绑定)到后面的值绑定在一起 如果要改变input的值需要改变js的data -------------</h2>
           <input type="text" v-bind:value="bindVal"> &nbsp;  {{ bindVal }}
   
       const helloVueApp = {
           data() {
               return {
                   message: '现在时间: ' + new Date().toLocaleString(),
                   bindVal: 1,
               }
           }
       }
       Vue.createApp(helloVueApp).mount('#counter')
   ```

   ![image-20220606132536080](https://tva1.sinaimg.cn/large/e6c9d24ely1h2ygib2d5nj22540fywij.jpg)

2. `v-model:`     数据的双向绑定   (即  实例内容变化, 模板也会变化)     

   v-model只能用于表单类元素(即有value属性的标签元素)  

   input，textarea，select

   ``` vue
       <h1>--------v-model指令测试: -------------</h1>
       <h2>--------将输入框的值和后面的值绑定在一起(注意这是双向绑定) 改变input的值会改变后面的变量-------------</h2>
       <input type="text" v-model="modelVal"> &nbsp;  {{ modelVal }}
   
       const helloVueApp = {
           data() {
               return {
                   modelVal: '',
               }
           }
       }
       Vue.createApp(helloVueApp).mount('#counter')
   ```

   ![image-20220606132956913](https://tva1.sinaimg.cn/large/e6c9d24ely1h2ygmsl6ssj22380a8wgs.jpg)

3. `v-on:`  动作指令: 当…..时   (比如v-on:click=“func()” 当点击时)

   值得一提:  vue有六个事件修饰符:

   1. Prevent     阻止默认事件
   2. stop    阻止事件冒泡      默认容器内的所有匹配的事件都会触发  使用stop后只匹配一个
   3. once    事件只触发一次
   4. capture   调整事件的触发为捕获阶段
   5. self   只有event.target是当前操作的元素时才触发事件
   6. passive    事件的默认行为是立即执行, 无须等待事件回调执行完毕

   ```html
       <h1>--------v-on指令测试: -------------</h1>
       <button v-on:click="showInfo">showInfo</button>
       <br>
       <a href="http://baidu.com" @click.prevent>showInfo</a>    // 阻止默认事件 点击超链接无反应 <br>
       <a href="http://baidu.com" @click.prevent="showInfo()">showInfo</a>    // 阻止默认事件转为指定的事件 和 点击按钮一样
   
   <script>
       const helloVueApp = {
           methods: {showInfo}
       }
       function showInfo() {
           alert("this is Info")
       }
       Vue.createApp(helloVueApp).mount('#counter')
   </script>
   ```

4. 条件渲染指令  `v-if`与`v-else`   条件不成立时, v-if的所有子节点不会被解析      解析为false则直接不会渲染

   ``` html
       <h1>--------v-if指令测试: v-if后面的值 vue属性值 ||一个返回值为布尔类型的表达式-------------</h1>
       <span v-if="vIfVal == false">哈哈哈哈 </span>
       <!-- if命中后 下面的就不判断了     v-if和v-else..  之间不能被打断 也就是说 中间不能有别的语句      -->
       <span v-else-if="vIfVal == true">哈哈哈哈1 </span>
       <span v-else-if="vIfVal">哈哈哈哈2 </span>
       <span v-else-if="vIfVal">哈哈哈哈3 </span>
       <span v-else="vIfVal">哈哈哈哈4 </span>
   ....
   	vIfVal: true
   }
   ```

   ![image-20220606134853754](https://tva1.sinaimg.cn/large/e6c9d24ely1h2yh6hihjpj225408odhw.jpg)

   

5. 条件渲染`v-show`  展示与不展示    频繁切换使用这个      底层使用display属性

   ``` html
       <h1>--------v-show指令测试: v-show后面的值 vue属性值 ||一个返回值为布尔类型的表达式-------------</h1>
       <span v-show="vIfVal ">哈哈哈哈 </span>
   ```

   

6. 循环指令 `v-for`  

   值得一提 建议在后面在`:key="不重复的值"`  用来标识`li`的序号

   可以遍历 数组 对象 字符串 次数

   + `v-for="(value,index) of arr" :key="index"`
   + `v-for="(value,index) of obj" :key="index"`
   + `v-for="(value,index) of string" :key="index"`
   + `v-for="(value,index) of 5" :key="index"`

   ``` html
       <h1>--------v-for指令测试: 建议写key  key是一个不重复的值 -------------</h1>
       <ul>
           <li >id - 姓名 - 年龄</li>
         <!-- 写法1     of 和 in  都可以-->
           <li v-for="p in persons" >{{p.id}} - {{p.name}} - {{p.age}}</li>
         <!-- 写法2 -->
         <li v-for="p in persons" :key="p.id">{{p.id}} - {{p.name}} - {{p.age}}</li>
         <!-- 写法3 -->
         <li v-for="(p,index) in persons" :key="index">{{p.id}} - {{p.name}} - {{p.age}}</li>
       </ul>
   <script>
   ......
   persons: [
     {id: '001', name: 'aaa', age: '14'},
     {id: '002', name: 'bbb', age: '11'},
     {id: '003', name: 'ccc', age: '17'},
     {id: '004', name: 'ddd', age: '12'},
   ......
   </script>
   ```

   ![image-20220606141427317](https://tva1.sinaimg.cn/large/e6c9d24ely1h2yhx3128dj21qo0kkju0.jpg)

   

7. `v-text`   整个替换掉标签内的文字,且不会被解析  

   ``` html
       <h1>--------v-text指令测试: 会整个替换掉标签的值 且不会被编译 -------------</h1>
       <span>姓名是: {{vTextVal}}</span> <br>
       <span v-text="vTextVal">这里的文字不会被显示 </span>
   <script>
   .....
   vTextVal:'<h2>这个是v-text解析的内容 </h2>',
   .....
   </script>
   ```


​		![image-20220606142321674](https://tva1.sinaimg.cn/large/e6c9d24ely1h2yi6dhje9j21v6084763.jpg)

8. `v-html`   整个替换掉标签内的文字,会被解析  

   ``` html
       <h1>--------v-html指令测试: 会整个替换掉标签的值 如果值是html的标签会被编译 -------------</h1>
       <span v-html="vHtmlVal">这里的文字不会被显示 </span>
   
   <script>
   .....
   vHtmlVal: '<h2>这个是v-html解析的内容 </h2>'
   .....
   </script>
   ```

​		![image-20220606142618000](https://tva1.sinaimg.cn/large/e6c9d24ely1h2yi9euc1nj22540amtba.jpg)



9. `v-cloak`网速慢的时候(js没有加载出来时候)  vue的样式不显示在页面中, 等js加载完毕再显示 没有`v-cloak`指令修饰的内容正常显示

   当网络受阻或页面加载完毕而没有初始化得到 Vue 实例时，DOM 中的 {{}} 则会展示出来，此时就会出现页面闪烁的问题。
   为了防止此现象，可以使用 CSS 配合 Vue v-cloak 指令实现获取 Vue 实例前的隐藏。和 CSS 规则如 [v-cloak] { display: none } 一起用时，这个指令可以隐藏未编译的 Mustache 标签直到实例准备完毕。
   
   ``` html
   [v-cloak] {
   	display: none;
   } 
   <div v-cloak>
   	{{ message }}
   </div>
   ```



10. `v-once`     被它修饰的节点, 在初次动态渲染后,就视为静态内容了

    只第一次拿到数据 后续不再跟随vue的响应式变动

    ``` html
    <h2 v-once>初始的值: {{n}} </h2>
    <h2>后续的的值: {{n}} </h2>
    <button @click="n++">n累加</button>
    ```

    ![image-20220606143801498](https://tva1.sinaimg.cn/large/e6c9d24ely1h2yilmjw1qj218s06s0ti.jpg)

11. `p-pre`指令  被它修饰的节点, 不被vue所解析编译  可以优化代码





### 3.1 指令代码完整版

``` html
<!DOCTYPE html>
<html lang="en" xmlns:v-bind="http://www.w3.org/1999/xhtml" xmlns:v-on="http://www.w3.org/1999/xhtml"
      xmlns:v-model="http://www.w3.org/1999/xhtml" xmlns:v-cload="http://www.w3.org/1999/xhtml">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <!-- 引入vue3   -->
    <script src="../vue3.0.js"></script>
</head>
<body>
<div id="counter">
    
    <h1>--------vue变量测试: 将vue属性的值绑定到html页面内容-------------</h1>
    counter: {{num}}
    
    <h1>--------v-bind指令测试: -------------</h1>
    <h2>--------将vue属性的值单向绑定到html标签内的变量 vue属性的值变 页面显示的就变-------------</h2>
    <span v-bind:title="message" style="border: 1px solid">
    将鼠标悬停一会 可以看到当前时间
  </span>
    <br> <br>
    <h2>--------单向绑定 将输入框的值->(单向绑定)到vue属性的值绑定在一起 如果要改变input的值需要改变js的data -------------</h2>
    <input type="text" v-bind:value="bindVal"> &nbsp; {{ bindVal }}
    
    <h1>--------v-model指令测试: -------------</h1>
    <h2>--------将输入框的值和vue属性的值绑定在一起(注意这是双向绑定) 改变input的值会改变vue属性的变量-------------</h2>
    <input type="text" v-model="modelVal"> &nbsp; {{ modelVal }}
    
    <h1>--------v-on指令测试: -------------</h1>
    <button v-on:click="showInfo">showInfo</button>
    <br>
    <a href="http://baidu.com" @click.prevent>showInfo</a> // 阻止默认事件 点击超链接无反应 <br>
    <a href="http://baidu.com" @click.prevent="showInfo()">showInfo</a> // 阻止默认事件转为指定的事件 和 点击按钮一样
    
    <h1>--------v-if指令测试: v-if后面的值 vue属性值 ||一个返回值为布尔类型的表达式-------------</h1>
    <span v-if="vIfVal == false">哈哈哈哈 </span>
    <!-- if命中后 下面的就不判断了     v-if和v-else..  之间不能被打断 也就是说 中间不能有别的语句      -->
    <span v-else-if="vIfVal == true">哈哈哈哈1 </span>
    <span v-else-if="vIfVal">哈哈哈哈2 </span>
    <span v-else-if="vIfVal">哈哈哈哈3 </span>
    <span v-else="vIfVal">哈哈哈哈4 </span>
    
    <h1>--------v-show指令测试: v-show后面的值 vue属性值 ||一个返回值为布尔类型的表达式-------------</h1>
    <span v-show="vIfVal ">哈哈哈哈 </span>
    
    <h1>--------v-for指令测试: 建议写key key是一个不重复的值 -------------</h1>
    <ul>
        <li>id - 姓名 - 年龄</li>
        <!-- 写法1     of 和 in  都可以-->
        <li v-for="p in persons">{{p.id}} - {{p.name}} - {{p.age}}</li>
        <!-- 写法2 -->
        <li v-for="p in persons" :key="p.id">{{p.id}} - {{p.name}} - {{p.age}}</li>
        <!-- 写法3 -->
        <li v-for="(p,index) in persons" :key="index">{{p.id}} - {{p.name}} - {{p.age}}</li>
    </ul>
    
    <h1>--------v-text指令测试: 会整个替换掉标签的值 且不会被编译 -------------</h1>
    <span>姓名是: {{vTextVal}}</span> <br>
    <span v-text="vTextVal">这里的文字不会被显示 </span>
    
    <h1>--------v-html指令测试: 会整个替换掉标签的值 如果值是html的标签会被编译 -------------</h1>
    <span v-html="vHtmlVal">这里的文字不会被显示 </span>
    
    <h1>--------v-cload指令测试: 网速慢的时候 其他标签正常加载 不必等这个标签加载完毕再统一显示 -------------</h1>
    <span>姓名是: {{vCload}}</span> <br>
    <span v-html="vCload" v-cloak>姓名是: </span>
    
    <h1>--------v-once指令测试: -------------</h1>
    <span v-once>初始的值: {{n}} </span>
    <span>后续的的值: {{n}} </span>
    <button @click="n++">n累加</button>
</div>
</body>
<script>
    const helloVueApp = {
        data() {
            return {
                message: '现在时间: ' + new Date().toLocaleString(),
                num: 2,
                bindVal: 1,
                modelVal: '',
                vIfVal: true,
                persons: [
                    {id: '001', name: 'aaa', age: '14'},
                    {id: '002', name: 'bbb', age: '11'},
                    {id: '003', name: 'ccc', age: '17'},
                    {id: '004', name: 'ddd', age: '12'},
                ],
                vTextVal: '<ul>这个是v-text解析的内容 </ul>',
                vHtmlVal: '<ul>这个是v-html解析的内容 </ul>',
                vCload: '测试v-cload',
                n: 1,
            }
        },
        methods: {showInfo}
    }
    function showInfo() {
        alert("this is Info")
    }
    Vue.createApp(helloVueApp).mount('#counter')
</script>
<style>
    h1 {
        color: #bd362f;
    }
    h2 {
        color: #42b983;
    }
    [v-cloak] {
        display: none;
    }
</style>
</html>
```










## 04_el和模板绑定的写法





**el的两种写法**

1. 2.0专属  实例一个Vue对象 通过参数去绑定模板和属性

   ```html
   new Vue({
       el: '#model-test',
       data: {
           name: '郭小傻model:'
       }
   })
   ```

2. 第二种 

   ```html
   const vm = new Vue({
       // el: '#model-test',
       data: {
           name: '郭小傻model:'
       }
   })
   console.log(vm)            // 这里可以看到vm的属性  其中包含$mount属性  用来绑定模板
   vm.$mount("#model-test");
   ```



**data的两种写法:**

1. 对象式

   ``` html
   new Vue({
       el: '#model-test',
       data: {
           name: '郭小傻model:'
       }
   })
   ```

2. 对象式:                  3.0提倡这种写法

   ``` html
   // 当然在对象里写方法: 提倡下面的写法这个函数被vue接管  所以不要使用箭头函数 箭头函数不归vue接管
   new Vue({
       el: '#model-test',
       data(){
   			return{
           name: '郭小傻model:'
       }
   }})
   ```
   
   

**3.X的标准写法**

``` js
const funName = {
  data(){},
  methods:{},
  ....
}
Vue.createApp(funName).mount('el标签 记得带前面的. 或者 #')
```



 

## 05_数据代理



回顾`Object.defineProperty`方法 :

实例:

```js
let person = {
    name: '张三',
    sex: '男',
    // age: 14
}
Object.defineProperty(person, 'age', {
  Object.defineProperty(person, 'age', {
  // value: 18,
  //enumable: true      // 属性是否可以枚举    默认是false
  //writable: true        // 属性是否可以更改    默认是false
  //configable: true      // 属性是否可以被删除   默认是false

  // 当有人读取person的age属性时, get函数(getter)就会被调用, 返回值就是age的值
  get() {
  return number;
	},
  // 当有人需要修改person的age属性时, set函数(setter)就会被调用, 参数就是需要修改的值
  set(value){
    number =  value;
  }
})

console.log(person);
```

讲解:

1. `Object.defineProperty`可以给变量添加属性
2. 和直接添加属性不同的是, 
   1. 用这个方法添加的属性是不可以被枚举的(遍历)   但是可以使用其enumavle属性进行控制其可以枚举
   2. 不可以被修改,    但是可以使用 writable属性进行控制
   3. 不可以被删除,   但是可以使用configable属性进行控制

3. `setter()和getter()`这两个属性类似于java的set,get  用于修改对象的属性



总结: 

什么是数据代理:  通过一个对象代理对另一个对象中属性的操作(read/write)    称为数据代理





Vue的数据代理:

1. vue中的数据代理:

   通过vm对象来代理data对象中属性的操作

2. vue中数据代理的好处:

   更加方便的操作data中的数据

3. 基本原理:

   通过Object.definePropertu()把data对象中的所有属性添加到vm上

   为每一个添加到vm的属性, 都指定一个getter/setter

   在getter/setter内部去操作data中对应的属性

![image-20220112134324126](https://tva1.sinaimg.cn/large/008i3skNly1gyau66sd8gj31h90u0jvb.jpg)



## 06_事件处理

`v-on:`  动作指令: 当…..时  

+  点击事件:   v-on:click=“func()” 当点击时     可以简写为@click=“func()”

+ 键盘事件:   v-on:keyup=“func()”当键盘抬起时候     @keydown=“func()”  按下时候

+ 滚动事件:   @scroll=“func()”鼠标或者滚动条事件触发      @wheel=“func()”仅仅鼠标滚动触发

   

### 6.1 事件修饰符

值得一提:  vue有六个事件修饰符:

1. Prevent     阻止默认事件
2. stop    阻止事件冒泡      默认容器内的所有匹配的事件都会触发  使用stop后只匹配一个
3. once    事件只触发一次
4. capture   调整事件的触发为捕获阶段
5. self   只有event.target是当前操作的元素时才触发事件
6. passive    事件的默认行为是立即执行, 无须等待事件回调执行完毕

```html
<div id="counter">
  <button v-on:click="showInfo">showInfo</button>
  <button @click="showInfo()">showInfo</button>    // @是简写
  <a href="http://baidu.com" @click.prevent>showInfo</a>    // 阻止默认事件
  <a href="http://baidu.com" @click.prevent="showInfo()">showInfo</a>    // 阻止默认事件
</div>
<script>
    Vue.config.productionTip = false;
    new Vue({
        el: '#counter',
        data: {
          //......
        },
        methods:{
            showInfo
        }
    })
    function showInfo(){
        alert("this is Info") 
    }
</script>
```





### 6.2 键盘事件

键盘事件:   v-on:keyup=“func()”当键盘抬起时候     @keydown=“func()”  按下时候

常见按键别名

1. 回车  => enter
2. 删除  => delete    捕获删除和退格键
3. 退出  => esc 
4. 空格  => space
5. 换行  => tab
6. 上  => up
7. 下  => down
8. 左  => left
9. 右  => righ

值得一提:

1. vue为了方便取了一些别名  记不住的话也可以使用原始名 原始名也记不住的话可以使用e.key查看

2. 使用原始按键key去绑定, 要注意 如果是组合单词要变为短横线命名法   大小写切换: caps-lock

3.  tab  有系统自定义功能要使用 keydown捕获

4. 一些系统修饰键  ctrl alt shift meta最好使用keydown捕获

   修饰键的使用方法 自己找吧  懒得写了

5. 也可以使用keyCode去指定  但是不建议

6. Vue.config.keyCodes.自定义键名 = 键码  可以去定制按键别名

实例:

```html
<!--   vue提供的别名    -->
<input type="text" @keyup.enter="keyInfo()">
<!--   键盘原始名  可以用keyInfo()下面的控制台输出看    -->
<input type="text" @keyup.Enter="keyInfo()">
<input type="text" @keyup.Controll="keyInfo()">
</div>
<script>
    Vue.config.productionTip = false;
    const vm = new Vue({
        el: '#counter',
        methods:{
            keyInfo(e) {
                console.log(e.key, e.keyCode)
            }
        }
    })
</script>
```





## 07_计算属性

Vue对象的参数对象的属性是: `computed`

实例:

页面中两个输入框的内容进行拼接

```html
<body>
<div id="app">
<!-- h1 分割线   -->
    <h1>----------------计算属性--------------------</h1>
<input type="text" v-model="first" /> &nbsp;
<input type="text" v-model="last" />&nbsp;
<span>拼凑的字符串是(简写): {{fullName}}</span>&nbsp;
<span>拼凑的字符串是(完整): {{fullName2}}</span>
</div>

</body>
<script>
    <!--初始化vue3-->
    const container = {
        data(){
            return {
                first:'',
                last:'',
            }
        },
        computed:{
            fullName(){// 简写形式  其实默认是getter
                return this.first+' '+this.last
            },
            fullName2:{  // 完整形式
                set(val){ // 当修改fullName2的值时，会调用set方法
                    alert("set:"+ val);
                },
                get(){
                    return this.first+' '+this.last
                }
            }
        }
    }
    Vue.createApp(container).mount('#app')
```

使用计算属性的好处:

+ 其实上面的实例使用方法显然更容易, 但是方法的缺点是如果页面中多次需要这个值,则每一次都需要调用方法

+ 使用计算属性, 则可以避免上面的问题   计算之后会放在缓存中,  如果输入框中的原数据没有变化则如果页面其他地方还有调用,则不会重复调用方法



## 08_侦听属性

vue中提供了一个方法, 用于监视属性发生变化  并做出处理

**下面以点击按钮切换真假做演示     请注意`watch`属性**

```html
<div id="app">
    <h1>----------------侦听属性--------------------</h1>
    <span>结果是: {{status}}</span> &nbsp;
    <button @click="changeStatus()">点击切换真假</button>
</div>
</body>
<script>
    <!--初始化vue3-->
    const container = {
        data() {
            return {
                res: false,
            }
        },
        methods:{
            changeStatus(){
                return this.res = !this.res;
            }
        },
        computed: {
            status() {
                return this.res ? '真' : '假'
            },
        },
        watch:{
            res:{
                deep:true ,  // 深度监听  比如监听的是对象的属性可以侦听对象内部的属性变化
                immediate:true,  // 立即执行 初始化的时候 下面的handler方法执行一次
                handler(newVal,oldVal){
                    console.log(newVal,oldVal);
                }
            },
        }
    }
    Vue.createApp(container).mount('#app')
</script>
```



## 09_插曲箭头函数和普通函数

不同点:

+ 箭头函数: 自身不带有this参数  如需使用则js会从箭头函数的外部去找普通函数,调用其this
+ 普通函数: 参数中自带this参数,    vue接管的函数this代表vue    没有被接管的函数this则指系统的windows对象



原则:

1. 被Vue接管的函数, 最好写成普通函数, 
2. 不被vue接管的函数, 如定时器的回调函数. ajax的回调函数等 最好写成箭头函数,这样函数内的this值得还是vue对象









## 10_绑定样式



绑定样式





### class绑定

**基础变量写法:**
效果说明:    vue解析后 会把两个class合成一个输出到最终的页面模板

``` 
<div class="基础样式1" v-bind:class="动态样式1" ></div>
<!--   当然也可以写成  :class="动态样式1"       -->
```



对于样式的操控, 有三种

1. 绑定class样式   –  字符串写法   适用于样式的类名不确定, 需要动态绑定
2. 绑定class样式   –  数组写法   适用于样式的个数不确定, 名字也不确定
3. 绑定class样式   –  对象写法   适用于样式的个数确定, 名字确定, 但要动态决定用不用

实现:   

``` html
<div id="counter">
<!-- 绑定class样式   –  字符串写法   适用于样式的类名不确定, 需要动态绑定   -->
    <div class="base"  :class="strAtt">字符串写法</div>
<!-- 绑定class样式   –  数组写法   适用于样式的个数不确定, 名字也不确定   -->
    <div class="base" :class="arrAtt">数组写法</div>
<!-- 绑定class样式   –  对象写法   适用于样式的个数确定, 名字确定, 但要动态决定用不用   -->
    <div class="base" :class="objAtt">对象写法</div>
</div>
<script>
    Vue.config.productionTip = false;
    const vm = new Vue({
        el: '#counter',
        data: {
            strAtt: 'dynamic1',
            arrAtt: ['dynamic2','dynamic3'],
            objAtt: {
                dynamic4: true,
                dynamic5: true,
                dynamic6: true,
            },
        },
    })
</script>
```

测试:

![image-20220113163832440](https://tva1.sinaimg.cn/large/008i3skNly1gyc4unx4v3j311i0l2tbk.jpg)





### style绑定

**基础变量写法:**
效果说明:    vue解析后 会把两个class合成一个输出到最终的页面模板

``` 
<div v-bind:style="动态样式1" ></div>
<!--   当然也可以写成  :style="动态样式1"       -->
```



可以使用字符串, 数组,  对象  都可以

```html
<div id="d" :style="styleAtt">style写法</div>
<script>
    Vue.config.productionTip = false;
    const vm = new Vue({
        el: '#counter',
        data: {
            strAtt: 'dynamic1',
            arrAtt: ['dynamic2','dynamic3'],
            objAtt: {
                dynamic4: true,
                dynamic5: true,
                dynamic6: false,
            },
            styleAtt:{
                // 属性还是原来的 但是需要加小驼峰
                fontSize: '30px',
                height: '300px',
                width: '400px',
                border: 'solid'
            }
        },
    })
</script>
```





## 11_列表渲染



### 列表的过滤(搜索)

>  侦听属性和 计算属性 两种实现方式

``` html
<body>

<div id="counter">
    <h1>-------------列表过滤 搜索  计算属性--------------------------------</h1>
    <ul>
        <input type="text" placeholder="请输入名" v-model="searchStr">
        <li v-for="p in resPersons" :key="p.id">{{p.name}} - {{p.age}} - {{p.sex}}</li>
    </ul>
</div>

<div id="counter1">
    <h1>-------------列表过滤 搜索   监视属性--------------------------------</h1>
    <ul>
        <input type="text" placeholder="请输入名" v-model="searchStr">
        <li v-for="p in resPersons" :key="p.id">{{p.name}} - {{p.age}} - {{p.sex}}</li>
    </ul>
</div>
<script>
    const app = {
        data() {
            return {
                searchStr: '',
                persons: [
                    {id: '001', name: '马冬梅', age: '14', sex: '女'},
                    {id: '002', name: '周杰伦', age: '11', sex: '男'},
                    {id: '003', name: '周冬雨', age: '17', sex: '女'},
                    {id: '004', name: '温兆伦', age: '12', sex: '男'},
                ],
            }
        },
        computed: {
            resPersons(){
                    return this.persons.filter(p => {
                        return p.name.indexOf(this.searchStr) > -1;
                    })
            }
        }
    }
    const app1 = {
        data() {
            return {
                searchStr: '',
                persons: [
                    {id: '001', name: '马冬梅', age: '14', sex: '女'},
                    {id: '002', name: '周杰伦', age: '11', sex: '男'},
                    {id: '003', name: '周冬雨', age: '17', sex: '女'},
                    {id: '004', name: '温兆伦', age: '12', sex: '男'},
                ],
                resPersons: [],
            }
        },
        watch: {
            searchStr: {
                immediate: true,
                handler(newValue, oldValue) {
                    this.resPersons = this.persons.filter(p => {
                        return p.name.indexOf(newValue) > -1;
                    })
                }
            }
        },
    }
    Vue.createApp(app).mount('#counter')
    Vue.createApp(app1).mount('#counter1')
</script>
</body>
</html>
```



### 列表的排序:

 实时排序:   

``` html
<div id="counter">
    <ul>
        <input type="text" placeholder="请输入名" v-model:value="searchStr">
        <button @click="sortFun(2)">年龄升序</button>
        <button @click="sortFun(1)">年龄降序</button>
        <button @click="sortFun(0)">原顺序</button>
        <li v-for="p in resPersons" :key="p.id">{{p.name}} - {{p.age}} - {{p.sex}}</li>
    </ul>
</div>
<script>
  Vue.config.productionTip = false;
  const vm = new Vue({
    el: '#counter',
    data: {
      searchStr: '',
      sortType: 0,
      persons: [
        {id: '001', name: '马冬梅', age: '14', sex: '女'},
        {id: '002', name: '周杰伦', age: '11', sex: '男'},
        {id: '003', name: '周冬雨', age: '17', sex: '女'},
        {id: '004', name: '温兆伦', age: '12', sex: '男'},
      ],
    },
    methods:{
      sortFun(num){
        this.sortType = num
      }
    },
    computed: {
      resPersons(){
        let res =  this.persons.filter(p => {
          return p.name.indexOf(this.searchStr) > -1;
        })
        if (this.sortType) {
          return  res.sort((a,b)=>{
            return this.sortType === 1 ? (b.age-a.age) : (a.age-b.age)
          });

        }
        else return res
      },
    }
  })
</script>
```



## 12_过滤器(3.0已移除)

借助于管道通道符, 将参数传给过滤器 由过滤器返回后使用    过滤器默认有一个参数就是管道符前的数据, 如果手动写参数, 则过滤器的参数为`管道符前的数据, 自己手动写的参数....`    

值得一提:  过滤器可以链式传递 `{{源数据 | 过滤器1 | 过滤器2 | .....}}`

例如:  传入一个数组`  [1,’zhangsan’,’男’]` 返回姓名展示回页面

```html
<div id="counter">
    <h1>姓名是: {{persons | strForm}}</h1>
</div>
<script>
    Vue.config.productionTip = false;
    const vm = new Vue({
        el: '#counter',
        data: {
            persons: ['001', '马冬梅', '14', '女']
        },
        filters: {    //  局部过滤器
            strForm(value) {
                return value[1]
            }
        }
    })
    // 全局过滤器
    Vue.filter('过滤器名',function(){过滤器的回调函数})
</script>
```



## 13_自定义指令



理论:

1. 指令使用时候需要前加`v-`

2. 指令本质上就是一个函数, 在vue对象的`directives`属性中进行编写

3. 一般情况下写法: (即不需要调用时机的情况下)

   方法自带两个参数: 一个是指令所在的节点的DOM元素    另一个是这个指令的一些默认信息

4. 不是一般情况下:  (即 指令需要手动控制调用时机  比如绑定时  渲染到页面时   后续更新模板时 等等 会有很多)

   例如:     页面打开时候 焦点自动定在输入框上  (坑: 只有在input渲染到页面之后执行获取焦点才有效  )

5. 当然  指令也可以是全局指令   写法同过滤器



**实例:**

```html
<div id="counter">
    <h1>---------------下面是自定义指令v-big---------------</h1>
    <h2>{{titleName}}</h2>
    <h2>n的值: {{n}}</h2>
    <h2>放大10倍的值: <span v-big="n"></span></h2>
    <button @click="n++"> 点击n++</button>

    <h1>---------------下面是自定义指令v-fbind 自动聚焦input框 ---------------</h1>
    <input type="text" v-fbind="n"/>
</div>
<script>
    const app = {
        data() {
            return {
                titleName: '自定义指令',
                n: 0,
            }
        },
        directives: {
            big(element, binding) {
                console.log(element, binding);
                element.innerText = binding.value * 10;
            },
            fbind(element, binding) {
                element.focus()
            }
        }
    }
    Vue.createApp(app).mount('#counter')
</script>
```





## 14_生命周期函数



指的是Vue解析和执行的过程中,不同时期会调用指定的函数称为声明周期函数

1. 又名: 生命周期回调函数, 生命周期函数, 声明周期钩子
2. 是什么: vue在关键的时刻,帮我们调用的一些特殊名称的函数
3. 生命周期函数的名字不可更改. 但函数的具体内容由成程序员根据需求手动编写
4. 声明周期函数中的this值得是vm或组件实例对象



例如:

1. `mounted(){}`   Vue完成模板的解析, 并把真实DOM元素放入页面后(完成挂载) 调用mounted

   ``` html
   <div id="counter">
       <h1>---------------下面是钩子生命周期函数 mounted ---------------</h1>
       <h3>下面的标题闪烁(利用opacity更改透明度)</h3>
       <h1 :style="{opacity: opacityVal}">是我在改变透明度</h1>
   
   </div>
   <script>
       const vm = {
           data() {
               return {
                   opacityVal: 1,
               }
           },
           methods:{
               show(msg) {
                   console.log('这是show方法的调用:',msg)
               },
           },
           mounted(){     //  表示内存中的模板已经真实的挂载到页面中，用户已经可以看到渲染好的页面
               this.show('mounted')
               setInterval(()=>{
                   console.log(this.opacityVal)
                   this.opacityVal -= 0.01;
                   if (this.opacityVal <= 0.2) {
                       this.opacityVal = 1
                   }
               },16)
           },
       Vue.createApp(vm).mount('#counter')
   </script>
   ```
   
   

生命周期详解:



**组件创建期间的4个钩子函数**

一、第一个生命周期函数，表示实例完全被创建之前，会执行这个函数
     在beforeCreate生命周期函数执行的时候，data 和 methods 中的数据还没有被初始化。

``` js
beforeCreate() {   
  console.log(this.msg)   //undefind
  this.show()   //is not defind
},
```



 二、第二个生命周期函数
     在 created 中，data 和 methods 都已经被初始化好了！
     如果要调用 methods 中的方法，或者操作 data 中的数据，最早，只能在 created 中操作

``` js
created() {    
  console.log(this.msg)   //ok
  this.show()        //执行了show方法
},
```



 三、第三个生命周期函数，表示模板已经在内存中编译完成，但是尚未把模板渲染到页面中
     在beforeMount执行的时候，页面中的元素没有被真正替换过来，知识之前写的一些模板字符串

  ``` js
  beforeMount() {  
    console.log(document.getElementById('h3').innerText)  //{{msg}}
  }, 
  ```



四、第四个生命周期函数，表示内存中的模板已经真实的挂载到页面中，用户已经可以看到渲染好的页面
     这个mounted是实例创建期间的最后一个生命周期函数，当执行完 mounted 就表示，实例已经被完
     全创建好了，此时，如果没有其它操作的话，这个实例，就静静地在内存中不动

``` js
mounted() {    
  console.log(document.getElementById('h3').innerText)   //ok
}, 
```



**组件运行阶段的2个钩子函数**  
五、第五个生命周期函数，表示 界面还没有被更新，但是数据肯定被更新了
     得出结论：当执行 beforeUpdate 的时候，页面中的显示的数据，还是旧的，此时data 数据是最
              新的，页面尚未和 最新的数据保持同步

``` js
beforeUpdate() { 
  console.log('界面上元素的内容'+ document.getElementById('h3').innerText)  //没有执行，因为数据没改变
  console.log('data 中的msg数据是：' + this.msg)
},
```



 六、第六个生命周期函数
     updated事件执行的时候，页面和 data 数据已经保持同步了，都是最新的

``` js
updated() {
  console.log('界面上元素的内容'+ document.getElementById('h3').innerText)   
    console.log('data 中的msg数据是：' + this.msg)   
},
```





第七个 和 第八个销毁阶段的函数在下面的图片中



<img src="https://img-blog.csdnimg.cn/7e691d8815944fcc8350d12e7f52327b.png"></img>



 **总结的话就是**

1. beforeCreate（创建前）
2. created（创建后）
3. beforeMount(载入前)
4. mounted（载入后）
5. beforeUpdate（更新前）
6. updated（更新后）
7. beforeDestroy（销毁前）
8. destroyed（销毁后）



简单图解:

![image-20220117170453326](https://tva1.sinaimg.cn/large/008i3skNly1gygs3acygtj318p0u0aeo.jpg)





# 组件化编程(2.0)



## 01_相关定义:



1. 模块:

   1. 理解: 向外提供特定功能的js程序, 一般就是一个js文件
   2. 为什么:  因为js文件很多很复杂
   3. 作用: 复用js,  简化js的编写, 提高js的编写效率

2. 组件:

   1. 理解: 用来实现局部功能效果的代码集合  html css img mp3 ttf ….
   2. 为什么: 一个界面的功能很复杂
   3. 作用: 复用编码, 键位项目编码, 提高运行效率

3. 模块化

   当应用中的js都是以模块来编写的, 那这个应用就是模块化的应用

4. 组件化

   当应用中的功能都是多组件的方式来编写的, 那这个项目就是一个组件化的应用

5. 非单文件组件:

   一个文件中有很多个组件

6. 单文件组件:

   一个文件中只包含一个组件    后缀一般是`*.vue`

   

## 02_非单文件组件

步骤:

1. 创建组件
2. 注册组件
3. 放到页面对应位置

实操:

``` html
<div id="counter">
    <h1>{{name}}</h1>
    <school></school>
    <hr>
    <student></student>
</div>
<hr>
<script>
    const school = {
            template: `
              <ul>
              <li>{{ idName }}</li>
              <li>{{ schoolName }}</li>
              <li>{{ address }}</li>
              </ul>
            `,
            data() {
                return {
                    idName: '这是组件一  学校组件',
                    schoolName: '学校名',
                    address: '西安'
                }
            },
            methods: {}
        }
    const student = {
            template: `
              <ul>
              <li>{{ idName }}</li>
              <li>{{ studentName }}</li>
              <li>{{ address }}</li>
              </ul>
            `,
            data() {
                return {
                    idName: '这是组件二  学生组件',
                    studentName: '张三',
                    address: '哈哈哈'
                }
            }
        }
    const app = {
        data() {
            return {
                name: '非单文件组件'
            }
        },
        components: {
            school,
            student
        }
    }
    Vue.createApp(app).mount('#counter')
</script>
```





值得注意:



有关命名:

1. 关于组件名

   1. 一个单词组成
      1. 首字母小写: school
      2. 首字母大写: School
   2. 多个单词组成
      1. Kebab-case命名法: my-school
      2. CamelCase命名法: MySchool   (需要使用vue的脚手架)
   3. 备注:
      1. 组件名尽可能的回避HTML中已经有的标签名: h1 h2 ……
      2. 可以使用name配置项指定组件在开发者工具中呈现的名字  组件创建时候的name属性中指定

2. 关于组件标签

   1. 第一种写法:` <school></school>`
   2. 第二种写发: ` <school />`  一定要在vue脚手架环境下

3. 一个简写方式

   `const school = Vue.extend(options)` 可以简写为 `const school = options` 





## 03_单文件组件

单文件组件的后缀名是: `.vue`

vue文件内有三个标签:

1. `<template> // 组件的结构 </template`
2. `<script>   // 组件交互的相关代码, (数据, 方法等等)    </script>`
3. `<style>  // 组件的样式    </style>`

注意:

1. vue文件需要有对外暴露的接口  有三种暴露方式  建议使用默认暴露



**编写步骤:**

1. 编写单文件组件

单文件组件编写实践(多文件组件的学校组件为例   学生的自己写):

```vue
<template>
  <!-- 原template属性内的东西  -->
  <div class="demo">
    <ul>
      <li>{{ idName }}</li>
      <li>{{ schoolName }}</li>
      <li>{{ address }}</li>
    </ul>
  </div>
</template>

<script>
// 对外暴露组件
export default {   // 其实完整的写法是 export default  Vue.extend({......})
  name: 'School',      // 建议和文件名一致 大驼峰
  data() {
    return {
      idName: '这是组件一  学校组件',
      schoolName: '学校名',
      address: '西安'
    }
  },
  methods: {}
}
// 暴露组件有三种方式 一般情况使用上面的默认暴露
// 方式一       分别暴露
export const 组件名 = Vue.extend({//......})
// 方式二        统一暴露
const 组件名2 = Vue.extend({//......})
export {暴露的组件名}
</script>


<style>
.demo {
  border: black solid;
}
</style>
```



2. 编写App.vue组件  用来管理所有的组件

```html
<template>
  <div>
    <School></School>
    <Student></Student>
  </div>
</template>
<script>
import School from './School';
import Student from './Student';
export default {
  name: 'App',
  computed:{
    School,
    Student
  }
}
</script>
```



3. 创建main.js用来统一管理vue

```js
import App from './App';

new Vue({
    el: '#root',    // 服务的容器名
    comments: {App}
})
```





4. 创建页面html

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>

<div id="root">

</div>

<script type="text/javascript" src="https://cdn.jsdelivr.net/npm/vue@2.6.14/dist/vue.js"></script>
<script type="text/javascript" src="main.js"></script>
</body>
</html>
```





## 04_ref属性

 

ref属性:    之前要想获取DOM元素就需要document.getById…..  现在给元素加上ref属性就可以快速拿到DOM元素

1. 被用来给元素或子组件注册引用信息 (id的替代品)
2. 应用在HTML标签时获取的是真实DOM元素, 应用在组件标签上是组件的实例对象(vc)
3. 使用方式:
   1. 打标识: `<h1 red="xxxx">......</h1>` 或者`<School ref="xxx"></School> `
   2. 获取: `this.$refs.xxx`



## 05_prop属性

出现的问题:

1. vue的组件是可以复用的, 如果两个容器都需要这个组件, 但是另一个组件希望更改组件的data元素, 只能把组件完整的重写一遍

解决:

1. 使用`props`属性, 动态的接收组件使用者传入的参数放入到组件的data中

使用:

### 原始版

``` html
<!-- index -->
<body>
  <div id="app"></div>
  <!-- built files will be auto injected -->
</body>
<!-- main.js -->
import Vue from 'vue'
import App from './App.vue'
Vue.config.productionTip = false
new Vue({
  render: h => h(App),
}).$mount('#app')

<!-- app.vue  -->
<template>
<div>
  <hr style="border: brown solid; height: 10px">
  <h1>信息展示</h1>
    <School></School>
    <Student></Student>
</div>
</template>
<script>
import School from './components/School.vue'
import Student from './components/Student.vue'
export default {
  name: 'App',
  components: {
    School,
    Student
  }
}
</script>
<!-- Student.vue -->
<template>
  <div>
  <h1>msg: {{msg}}</h1>
  <h1>学生名: {{name}}</h1>
  <h1>学生地址:  {{address}}</h1>
  </div>
</template>
<script>
export default {
  name: 'Student',
  data(){
    return {
    msg: '这是学生的信息',
    name: '张三',
    address: '西安'}
  }
}
</script>
<!-- School.vue -->
<template>
  <div>
  <h1>msg: {{msg}}</h1>
  <h1>学校名: {{name}}</h1>
  <h1>学校地址:  {{address}}</h1>
  </div>
</template>
<script>
export default {
  name: 'School',
  data(){
    return {
    msg: '这是学校的信息',
    name: '西安石油大学',
    address: '雁塔区'}
  }
}
</script>
```



### 复用Student 

``` html
只能再写一个Student.vue  因为组件引用后就不能更改data内容了
就不写了 太多了
```



### 使用prop方式引入

简单说明:

1. 将data的东西当做参数传进去
2. props属性内的属性原则上不可以更改  如果想更改参照下一条
3. props属性有三种使用方式
   1. `prop: ['参数1' | '参数2' | ....]`: 注意此时每一个参数为必须,且参数的值都是字符串类型
   2. `prop: {参数1:String , 参数2: Number ....}`  此时只允许接收指定类型的参数, 否则视为没有传参, 然后控制台报错
   3. `prop: {参数1:{type: String, required:true // 说明参数1是必须要的 } , 参数2: {type: Number, default: 99 // 参数2可以不要 没有参数2的情况下 默认值为99 }}`    可以设置每一个参数的具体信息

代码:

``` html
<!-- index -->
<body>
  <div id="app"></div>
  <!-- built files will be auto injected -->
</body>
<!-- main.js -->
import Vue from 'vue'
import App from './App.vue'
Vue.config.productionTip = false
new Vue({
  render: h => h(App),
}).$mount('#app')

<!-- app.vue  -->
<template>
<div>
  <h1>信息展示</h1>
    <School></School>
  这里使用数据绑定后 模板获取的是引号内的数据(此时引号内的东西是表达式 也就是说可以做简单运算) 并自动做数据转化  否则永远默认是字符串
    <Student name="张三" :age="18" sex="女"></Student>
    <Student name="李四" :age="48" ></Student>
</div>
</template>
<script>
import School from './components/School.vue'
import Student from './components/Student.vue'
export default {
  name: 'App',
  components: {
    School,
    Student
  }
}
</script>
<!-- Student.vue -->
<template>
  <div>
  <hr>
  <h1>msg: {{msg}}</h1>
  <h1>学生名: {{name}}</h1>
  <h1>学生年龄:  {{age}}</h1>
  </div>
</template>
<script>
export default {
  name: 'Student',
  data(){
    return {
    msg: '这是学生的信息',}
  },
  // props: [`name`,`sex`,`age`], // 方法一
  // props: {         // 方法二
  //   name: String,
  //   sex: String,
  //   age: Number
  // },
  props: {
    name: {
      type: String,
      required: true
    },
    sex: {
      type: String,
      default: '男'
    },
    age: {
      type: Number,
      required: true
    }
  },
}
</script>

<!-- School.vue -->
<template>
  <div>
  <h1>msg: {{msg}}</h1>
  <h1>学校名: {{name}}</h1>
  <h1>学校地址:  {{address}}</h1>
  </div>
</template>
<script>
export default {
  name: 'School',
  data(){
    return {
    msg: '这是学校的信息',
    name: '西安石油大学',
    address: '雁塔区'}
  }
}
</script>
```



### 使用props方式数据更改问题

出现的问题:

1. 使用props传入的参数(属性)的值是不可以更改的   要想更改提供一种思路
2. props属性的优先级高于data属性  所以可以做一个数据代理

 代码:

``` html
<template>
  <div>
    <hr>
    <h1>msg: {{ msg }}</h1>
    <h1>学生名: {{ name }}</h1>
    <h1>学生年龄: {{ dynamicAge }}</h1>
    <h1>学生性别 {{ sex }}</h1>
    <button @click="ageAdd()">年龄++</button>
  </div>
</template>
<script>
export default {
  name: 'Student',
  data() {
    return {
      msg: '这是学生的信息',
      dynamicAge: this.age
    }
  },
  props: ['name', 'sex', 'age'],
  methods: {
    ageAdd() {
      this.dynamicAge++;
    }
  }
}
</script>
```



## 06_混合属性

作用: 多个组件需要完成相同的内容(或者说多个组件中相同的地方, data,methods, mounts….)可提取出来, 其他组件只需要引入即可

写法:

1. 提取共有东西 以methods为例:

   ``` js
   // 新建一个js
   export const 对象名 = {
     methods:{
       showName(){
         alert("this.name")
       }
     },
     mounted:{
       console.log("挂载完毕要做的事情")
     }
   }
   ```

2. 需要使用的组件vue文件中:mixins:`[引入的内容数组]`

   ``` vue
   <template>......</template>
   <script>
     import {对象名} from '上面js的路径'
     export defalut{
       data(){},
       mixins:[对象名]              // 然后就可以使用上面js的内容了
     }
   </script>
   ```

   

值得一提:  和前面的配置一样 也可以全局配置  百度去





## 07_插件

作用: 插件可以对vue进行增强, 大多数情况下 插件里写的都是全局的一些配置, 比如过滤器, 默认声明周期, 自定义指令等等所有的全局配置都可以

步骤:

1. 新建一个插件的js  例如`src/plugs.js`

   ``` js
   export const 插件名 = {
     install(vue , 也可以传参数){
   		  Vue.巴拉巴拉   // 都是一些全局的配置       
    		  Vue.巴拉巴拉   // 都是一些全局的配置          
   		  Vue.巴拉巴拉   // 都是一些全局的配置      
     }
   }
   ```

2. 在main.js中引入

   ``` js
   import {对象名}  from 'js文件路径'
   Vue.use(对象名, 也可以传参数)
   ```

   

## 08_样式作用域

出现的问题: 解析完成后 所有的vue文件的样式都进行了汇总  带来的问题就是 要是出现同名的样式  会出现覆盖

解决:    vue文件的style中增加 `<style scoped></style>`

 



## 09_自定义事件

解决的问题: 父组件给子组件的通信

出现的问题: 儿子要给父亲传输数据  之前使用的prop属性, 但是不合适, prop属性主要用来父给子计算传输数据

解决:

1. 父亲处:  将原数据绑定改为 事件绑定  即 `:数据  改为 @自定义事件名`
2. 儿子处:  原来使用`props`接收,  现在不需要接收 在需要使用的地方 `this.$emit('自定义事件名',参数.....)`



## 10_全局事件总线:

解决的问题: 任意组件间通信

解决: 没有用到新的API, 只是一种编程思路

思路: 独立出一个新的组件, 将所有的事件和事件的回调写在这个新的组件上  这个组件应该具备几个要求

1. 所有的组件都可以看见 即 可见的
2. 可以调用到几个属性:  $on $off  $emit 

思路图:

<img src="https://tva1.sinaimg.cn/large/008i3skNly1gyzeot1653j31kz0u0dlq.jpg" alt="image-20220202194641888" style="zoom:50%;" />



实践思路:

1. 在main.js中声明一个声明周期钩子, 添加一个全局事件总线的的属性  使其值为当前的vue对象

   ``` js
   import Vue from 'vue'
   import App from './App.vue' 
   Vue.config.productionTip = false
   new Vue({
     render: h => h(App),
     beforeCreate() {
       Vue.prototype.$bus = this;
     }
   }).$mount('#app')
   ```

2. 接受方:

   ``` js
   this.$bus.$on('事件名1',(data){console.log('这里是事件的回调',data)})       // 添加一个事件   当然事件的回调函数可以写在methods中
   ```

3. 发送方:

   ``` js
   this.$bud.$emit('事件1','传入的参数 也就是需要发送的数据')       // 触发事件1 
   ```

4. 事件总线是全局的, 所以使用完后记得销毁   接受方(因为是接受方进行创建的事件)

   ``` js
   beforeDestroy(){        // 生命周期  在这个组件的生命周期走到尽头的时候 去触发下面的方法
     this.$bus.$off('事件1')
   }
   ```

   



## 11_$nextTick(回调函数)

作用: 下一次DOM更新结束后执行其指定的回调函数

使用场景: 点击按钮显示input框并获取焦点   

​	出现的问题: 点击按钮后执行方法, 方法执行完才会重新渲染DOM  所以方法里不能获取input的焦点

语法: this.$nextTick(回调函数)



# cli消息订阅与发布

作用: 组件间通信

准备工作: 安装`npm i pubsub-js`

简单使用:

1. 接收和发布消息的包中导入模块`import pubsub from 'pubsub-js'`  注意pubsub是一个对象

2. 接收消息方法: 

   ``` vue
   pubsub.subscribe('消息名',function(第一个参数是默认消息名参数, 第二个是接收到的数据){})
   ```

3. 发送消息方法:

   ``` vue
   pubsub.publish('消息名',数据参数)
   ```

代码示例:

``` vue
// 1. Schoole.vue
<template>
  <div>
    <hr>
    <h1>msg: {{ msg }}</h1>
    <h2>学校名: {{ name }}</h2>
    <h2>学校地址: {{ address }}</h2>
    <h2 v-if="studentName!==''">消息订阅与发布-接收学生姓名: {{ studentName }}</h2>
  </div>
</template>
<script>
import pubsub from 'pubsub-js'
export default {
  name: 'School',
  data() {
    return {
      msg: '这是学校的信息',
      name: '西安石油大学',
      address: '雁塔区',
      studentName: ''
    }
  },
  mounted() {
    pubsub.subscribe('studentName', (subName, stName) => {
      this.studentName = stName

    })
  }
}
</script>

// 2. Student.vue
<template>
  <div>
    <hr>
    <h2>msg: {{ msg }}</h2>
    <h2>学生名: {{ name }}</h2>
    <h2>学生年龄: {{ dynamicAge }}</h2>
    <h2>学生性别 {{ sex }}</h2>
    <button @click="sendName()">发送学生姓名</button>
  </div>
</template>
<script>
import pubsub from 'pubsub-js'

export default {
  name: 'Student',
  data() {
    return {
      msg: '这是学生的信息',
      dynamicAge: this.age
    }
  },
  props: [`name`, `sex`, `age`],
  methods: {
    sendName(){
      pubsub.publish('studentName',this.name)
    }
  }
}
</script>
```



## 05_CORS和axios

出现的问题: 违背同源原则时会出现跨域问题

解决思路:  通过配置vue代理服务器, 由代理服务器进行发送请求可以绕过CORS问题

代码实例:

1. vue.config.js

   ``` js
   // vue.config.js
   module.exports = {
       devServer: {
           overlay: {
               warnings: false, //不显示警告
               errors: false	//不显示错误
           },
           proxy:{
               '/serve1':{           // 代理服务器名  前缀
                   target: 'http://localhost:8081',    // 实际服务器地址
                   pathRewrite:{'^/serve1':''},      // 转化后的地址
                   ws: true,                          //  开启websocket
                   changeOrigin: true               //  隐藏真实请求
               },
               '/serve2':{           // 代理服务器名  前缀
                   target: 'http://localhost:8082',    // 实际服务器地址
                   pathRewrite:{'^/serve1':''},      // 转化后的地址
                   ws: true,                          //  开启websocket
                   changeOrigin: true               //  隐藏真实请求
               },
           }
       },
       lintOnSave:false //关闭eslint检查,
   }
   ```

   

2. App.vue

   ``` vue
   <template>
     <button @click="cors1()">跨域请求1</button>
   </template>
   <script>
   import axios from 'axios'
   
   export default {
     name: 'App',
     methods: {
       cors1() {
         axios.get('http://localhost:8080/serve1/aaa').then(
             response => {
               console.log('成功', response.data)
             },
             error => {
               console.log('失败', error.message
               )
             }
         )
       }
     }
   }
   </script>
   <style>
   </style>
   ```

   

## 06_引入bootstrap

> 引入bootstrap有两种方式
>
> 1. npm安装包的方式
> 2. 以静态页面的形式引入

### 6.1 npm安装包的方式

1. 安装

   ``` js
   npm install jquery --save
   npm install bootstrap --save
   npm install popper.js --save
   ```

2. 配置插件

   ``` js
   // vue.config.js
   const webpack = require('webpack')
   // vue.config.js
   module.exports = {
       devServer: {
           overlay: {
               warnings: false, //不显示警告
               errors: false	//不显示错误
           },
       },
       lintOnSave:false, //关闭eslint检查,
       configureWebpack: {
           plugins: [
               new webpack.ProvidePlugin({
                   $: "jquery",
                   jQuery: "jquery"
               })
           ]
       }
   }
   ```

3. 在main.js中引入

   ``` js
   import "bootstrap/dist/css/bootstrap.min.css";
   import "bootstrap/dist/js/bootstrap.min.js";
   ```

4. 正常使用即可

   ``` vue
   <template>
     <div class="container">
       <div class="row">
         <div class="col-3"></div>
         <div class="col-6">
           <form>
             <div class="form-group">
               <label for="exampleInputEmail1">Email address</label>
               <input type="email" class="form-control" id="exampleInputEmail1" placeholder="Email">
             </div>
             <div class="form-group">
               <label for="exampleInputPassword1">Password</label>
               <input type="password" class="form-control" id="exampleInputPassword1" placeholder="Password">
             </div>
             <div class="form-group">
               <label for="exampleInputFile">File input</label>
               <input type="file" id="exampleInputFile">
               <p class="help-block">Example block-level help text here.</p>
             </div>
             <div class="checkbox">
               <label>
                 <input type="checkbox"> Check me out
               </label>
             </div>
             <button type="submit" class="btn btn-default">Submit</button>
           </form>
         </div>
       </div>
     </div>
   </template>
   <script>
   export default {
     name: 'App',
   }
   </script>
   <style>
   </style>
   ```




### 6.2 引入静态页面的方式

1. 在public文件夹建立css/xx.css  放入
2. 在index.html里进行引入  记得改路径为`<%= BASE_URL %>/css/xx.css`

3. 在vue内就可以使用了

   



# 案例合集2.0

组件化编程流程(通用):

1. 实现静态组件: 抽取组件, 使用组件实现静态页面效果
2. 展示动态数据
   1. 数据的类型. 名称是什么
   2. 数据保存在那个组件
3. 交互 —— 从绑定时间监听开始





## 01_Todo-list案例



### 基础版本

使用props进行组件间通信

1. app.vue

   ``` vue
   // app.vue
   <template>
     <div id="root">
       <div class="todo-container">
         <div class="todo-wrap">
           <MyHeader :addTodo="addTodo"></MyHeader>
   
           <MyList :todos="todos" :updateTodo="updateTodo" :delTodo="delTodo"></MyList>
   
           <MyFooter :todos="todos" :selectAllTodo="selectAllTodo" :cleanDone="cleanDone" ></MyFooter>
         </div>
       </div>
     </div>
   </template>
   
   <script>
   import MyHeader from './components/MyHeader.vue'
   import MyFooter from './components/MyFooter.vue'
   import MyList from './components/MyList.vue'
   
   export default {
     name: 'App',
     components: {
       MyHeader,
       MyFooter,
       MyList,
     },
     data() {
       return {
         todos: [
           {id: '001', title: '吃饭', done: false},
           {id: '002', title: '休息', done: true},
           {id: '003', title: '工作', done: false},
         ],
       }
     },
     methods: {
       // 增
       addTodo(value) {
         this.todos.unshift(value)
       },
       // 改
       updateTodo(id) {
         this.todos.forEach(todo => {
           if (todo.id === id) {
             todo.done = !todo.done;
           }
         })
       },
       // 删
       delTodo(id) {
         this.todos = this.todos.filter(todo => todo.id !== id)    // 用过滤替代删除
       },
       selectAllTodo(status) {
         this.todos.forEach(todo=>todo.done = status)
       },
       cleanDone(){
         this.todos = this.todos.filter(todo => !todo.done)    // 用过滤替代删除
       }
     },
   }
   </script>
   
   <style>
   /*base*/
   body {
     background: #fff;
   }
   .btn {
     display: inline-block;
     padding: 4px 12px;
     margin-bottom: 0;
     font-size: 14px;
     line-height: 20px;
     text-align: center;
     vertical-align: middle;
     cursor: pointer;
     box-shadow: inset 0 1px 0 rgba(255, 255, 255, 0.2), 0 1px 2px rgba(0, 0, 0, 0.05);
     border-radius: 4px;
   }
   .btn-danger {
     color: #fff;
     background-color: #da4f49;
     border: 1px solid #bd362f;
   }
   .btn-danger:hover {
     color: #fff;
     background-color: #bd362f;
   }
   .btn:focus {
     outline: none;
   }
   .todo-container {
     width: 600px;
     margin: 0 auto;
   }
   .todo-container .todo-wrap {
     padding: 10px;
     border: 1px solid #ddd;
     border-radius: 5px;
   }
   </style>
   ```

2. MyFoolter.vue

   ``` vue
   <template>
     <div class="todo-footer" v-if="total">
       <label>
         <input type="checkbox" v-model="checkStatus"/>
       </label>
       <span>
             <span>已完成{{ doneTotal }}</span> / 全部{{ total }}
           </span>
       <button class="btn btn-danger" @click="cleanAll">清除已完成任务</button>
     </div>
   </template>
   <script>
   export default {
     name: "MyFooter",
     props: ['todos', 'selectAllTodo', 'cleanDone'],
     methods: {
       cleanAll() {
         this.cleanDone()
       }
     },
     data() {
       return {}
     },
     mounted() {
     },
     computed: {
       total() {
         return this.todos.length
       },
       doneTotal() {
         return this.todos.reduce((prev, curr) => prev + (curr.done ? 1 : 0), 0)
       },
       checkStatus: {
         // this.selectAllTodo(e.target.checked)
         get() {
           return this.total === this.doneTotal & this.total > 0
         },
         set(e) {
           this.selectAllTodo(e)
         }
       },
     },
   }
   </script>
   <style scoped>
   .todo-footer {
     height: 40px;
     line-height: 40px;
     padding-left: 6px;
     margin-top: 5px;
   }
   .todo-footer label {
     display: inline-block;
     margin-right: 20px;
     cursor: pointer;
   }
   .todo-footer label input {
     position: relative;
     top: -1px;
     vertical-align: middle;
     margin-right: 5px;
   }
   .todo-footer button {
     float: right;
     margin-top: 5px;
   }
   </style>
   ```

3. MyHeader.vue

   ``` vue
   <template>
     <div class="todo-header">
       <input type="text" placeholder="请输入你的任务名称，按回车键确认" v-model="title" @keyup.enter="add()"/>
     </div>
   </template>
   <script>
   import {nanoid} from 'nanoid'
   
   export default {
     name: "MyHeader",
     data() {
       return {
         title: ''
       }
     },
     props: ['addTodo'],
     methods: {
       add() {
         if (this.title.trim() === '') {
           alert('输入不能为空')
         } else {
           //将用户的输入包装成一个对象
           const todoObj = {id: nanoid(), title: this.title.trim(), done: false};
           this.addTodo(todoObj);
           this.title = '';
         }
       }
     }
   }
   </script>
   <style scoped>
   .todo-header input {
     width: 560px;
     height: 28px;
     font-size: 14px;
     border: 1px solid #ccc;
     border-radius: 4px;
     padding: 4px 7px;
   }
   
   .todo-header input:focus {
     outline: none;
     border-color: rgba(82, 168, 236, 0.8);
     box-shadow: inset 0 1px 1px rgba(0, 0, 0, 0.075), 0 0 8px rgba(82, 168, 236, 0.6);
   }
   </style>
   ```

4. MyItem.vue

   ``` vue
   <template>
     <li>
       <label>
         <input type="checkbox" v-model="checked" @click="changeCheck(id)"/>
         <span>{{ title }}</span>
       </label>
       <button class="btn btn-danger" @click="del(id)">删除</button>
     </li>
   </template>
   <script>
   export default {
     name: "MyItem",
     props: ['todoObj', 'updateTodo', 'delTodo'],
     data() {
       return {
         id: this.todoObj.id,
         title: this.todoObj.title,
         // checked: this.todoObj.done
       }
     },
     methods: {
       changeCheck(id) {
         this.updateTodo(id)
       },
       del(id) {
         if (confirm('确定删除吗?')) {
   
           this.delTodo(id);
         }
       }
     },
     mounted() {
     },
     computed: {
       checked: {
         get() {
           return this.todoObj.done
         },
         set(v) {
           this.todoObj.done = v
         }
       }
   
     }
   }
   </script>
   <style scoped>
   li {
     list-style: none;
     height: 36px;
     line-height: 36px;
     padding: 0 5px;
     border-bottom: 1px solid #ddd;
   }
   li label {
     float: left;
     cursor: pointer;
   }
   li label li input {
     vertical-align: middle;
     margin-right: 6px;
     position: relative;
     top: -1px;
   }
   li button {
     float: right;
     display: none;
     margin-top: 3px;
   }
   li:before {
     content: initial;
   }
   li:hover {
     background-color: #dddddd;
   }
   li:hover button {
     display: block;
   }
   li:last-child {
     border-bottom: none;
   }
   </style>
   ```

5. MyList.vue

   ``` vue
   <template>
     <ul class="todo-main">
       <MyItem v-for="todoObj in myTodos" :key="todoObj.id" :todoObj="todoObj" :updateTodo="updateTodo"
               :delTodo="delTodo"></MyItem>
     </ul>
   </template>
   <script>
   import MyItem from './MyItem'
   export default {
     name: "MyList",
     components: {
       MyItem,
     },
     props: ['todos', 'updateTodo', 'delTodo'],
     data() {
       return {
         // myTodos: this.todos
       }
     },
     computed:{
       myTodos(){
         return this.todos;
       }
     }
   }
   </script>
   <style scoped>
   .todo-main {
     margin-left: 0px;
     border: 1px solid #ddd;
     border-radius: 2px;
     padding: 0px;
   }
   .todo-empty {
     height: 40px;
     line-height: 40px;
     border: 1px solid #ddd;
     border-radius: 2px;
     padding-left: 5px;
     margin-top: 10px;
   }
   </style>
   ```






### 新增webStorage版本

问题: 浏览器关闭后数据会丢失

解决: 使用webStorage配合vue的监视属性, 监视到todos数据变化 则操作一次webStorage

代码:  只更改了app.vue

``` vue
<script>
import MyHeader from './components/MyHeader.vue'
import MyFooter from './components/MyFooter.vue'
import MyList from './components/MyList.vue'
export default {
  name: 'App',
  components: {.......},
  data() {
    return {
      todos: JSON.parse(localStorage.getItem('todos')) || []
    }
  },
  methods: { ...... },
  watch:{
    // todos(value){           // 只能监视todos  不能监视todos的内部
    //   localStorage.setItem('todos', JSON.stringify(value));
    // }
    todos:{
      deep: true,
      handler(value) {
        localStorage.setItem('todos', JSON.stringify(value));
      }
    }
  },
}
</script>

<style>.....
</style>
```



## 新增自定义事件

> 使用场景:   儿子需要给父亲传输数据时候
>
> 以app.vue和Foot.vue为例  其他类似

1. 父亲处:  将原数据绑定改为 事件绑定  即 `:数据  改为 @自定义事件名`
2. 儿子处:  原来使用`props`接收,  现在不需要接收 在需要使用的地方 `this.$emit('自定义事件名',参数.....)`

**代码:**

``` vue
//  1. app.vue
<template>
  <div id="root">
    <div class="todo-container">
      <div class="todo-wrap">
        //  注意这里
        <MyHeader @addTodo="addTodo"></MyHeader>
        <MyList :todos="todos" :updateTodo="updateTodo" :delTodo="delTodo"></MyList>
        <MyFooter :todos="todos" @selectAllTodo="selectAllTodo" @cleanDone="cleanDone" ></MyFooter>
      </div>
    </div>
  </div>
</template>

//  2. foot.vue
<template>
  <div class="todo-footer" v-if="total">
    <label>
      <input type="checkbox" v-model="checkStatus"/>
    </label>
    <span>
          <span>已完成{{ doneTotal }}</span> / 全部{{ total }}
        </span>
    <button class="btn btn-danger" @click="cleanAll">清除已完成任务</button>
  </div>
</template>

<script>
export default {
  name: "MyFooter",
  // 注意这里
  props: ['todos',],
  methods: {
    cleanAll() {
      // 注意这里
      this.$emit('cleanDone');
    }
  },
  data() {
    return {}
  },
  mounted() {
  },
  computed: {
    total() {
      return this.todos.length
    },
    doneTotal() {
      return this.todos.reduce((prev, curr) => prev + (curr.done ? 1 : 0), 0)
    },
    checkStatus: {
      get() {
        return this.total === this.doneTotal & this.total > 0
      },
      set(e) {
        // 注意这里
        this.$emit('selectAllTodo',e)
      }
    },
  },
}
</script>
```



## 新增全局事件总线

> 注意main.js  app.vue item.vue   list.vue

1. main.js

   ``` js
   import Vue from 'vue'
   import App from './App.vue'
   Vue.config.productionTip = false
   let vm = new Vue({
       render: h => h(App),
       beforeCreate() {
           Vue.prototype.$bus = this;
       },
   }).$mount('#app')
   ```

1. app.vue

   ``` vue
   <template>
     <div id="root">
       <div class="todo-container">
         <div class="todo-wrap">
           <MyHeader @addTodo="addTodo"></MyHeader>
           <MyList :todos="todos" :updateTodo="updateTodo" :delTodo="delTodo"></MyList>
           <MyFooter :todos="todos" @selectAllTodo="selectAllTodo" @cleanDone="cleanDone"></MyFooter>
         </div>
       </div>
     </div>
   </template>
   
   <script>
   import MyHeader from './components/MyHeader.vue'
   import MyFooter from './components/MyFooter.vue'
   import MyList from './components/MyList.vue'
   
   export default {
     name: 'App',
     components: {
       MyHeader,
       MyFooter,
       MyList,
     },
     data() {
       return {
         todos: JSON.parse(localStorage.getItem('todos')) || []
       }
     },
     methods: {
       // 增
       addTodo(value) {
         this.todos.unshift(value)
       },
       // 改
       updateTodo(id) {
         console.log("aaaas")
         this.todos.forEach(todo => {
           if (todo.id === id) {
             todo.done = !todo.done;
           }
         })
       },
       // 删
       delTodo(id) {
         this.todos = this.todos.filter(todo => todo.id !== id)    // 用过滤替代删除
       },
       selectAllTodo(status) {
         this.todos.forEach(todo => todo.done = status)
       },
       cleanDone() {
         this.todos = this.todos.filter(todo => !todo.done)    // 用过滤替代删除
       }
     },
     watch: {
       todos: {
         deep: true,
         handler(value) {
           localStorage.setItem('todos', JSON.stringify(value));
         }
       }
     },
     mounted() {
       this.$bus.$on('updateTodo', this.updateTodo);
       this.$bus.$on('delTodo', this.delTodo);
   
     },
     beforeDestroy() {
       this.$bus.$off('updateTodo');
       this.$bus.$off('delTodo');
     }
   }
   </script>
   
   <style>
   ......
   </style>
   ```

2. MyFoolter.vue

   ``` vue
   <template>
     <div class="todo-footer" v-if="total">
       <label>
         <input type="checkbox" v-model="checkStatus"/>
       </label>
       <span>
             <span>已完成{{ doneTotal }}</span> / 全部{{ total }}
           </span>
       <button class="btn btn-danger" @click="cleanAll">清除已完成任务</button>
     </div>
   </template>
   
   <script>
   export default {
     name: "MyFooter",
     props: ['todos',],   
     methods: {
       cleanAll() {
         this.$emit('cleanDone');
       }
     },
     data() {
       return {}
     },
     mounted() {
     },
     computed: {
       total() {
         return this.todos.length
       },
       doneTotal() {
         return this.todos.reduce((prev, curr) => prev + (curr.done ? 1 : 0), 0)
       },
       checkStatus: {
         // this.selectAllTodo(e.target.checked)
         get() {
           return this.total === this.doneTotal & this.total > 0
         },
         set(e) {
           this.$emit('selectAllTodo',e)
         }
       },
     },
   }
   </script>
   <style scoped>......
   </style>
   ```

3. MyHeader.vue

   ``` vue
   <template>
     <div class="todo-header">
       <input type="text" placeholder="请输入你的任务名称，按回车键确认" v-model="title" @keyup.enter="add()"/>
     </div>
   </template>
   <script>
   import {nanoid} from 'nanoid'
   
   export default {
     name: "MyHeader",
     data() {
       return {
         title: ''
       }
     },
     // props: ['addTodo'],
     methods: {
       add() {
         if (this.title.trim() === '') {
           alert('输入不能为空')
         } else {
           //将用户的输入包装成一个对象
           const todoObj = {id: nanoid(), title: this.title.trim(), done: false};
           // this.addTodo(todoObj);
           this.$emit('addTodo',todoObj)
           this.title = '';
         }
       }
     }
   }
   </script>
   <style scoped>......
   </style>
   ```

4. MyItem.vue

   ``` vue
   <template>
     <li>
       <label>
         <input type="checkbox" v-model="checked" />
         <span>{{ title }}</span>
       </label>
       <button class="btn btn-danger" @click="del(id)">删除</button>
     </li>
   </template>
   
   <script>
   export default {
     name: "MyItem",
     props: ['todoObj',],    // 注意
     data() {
       return {
         id: this.todoObj.id,
         title: this.todoObj.title,
       }
     },
     methods: {
       del(id) {
         if (confirm('确定删除吗?')) {
           this.$bus.$emit('delTodo', id);     //注意
         }
       }
     },
     mounted() {
     },
     computed: {
       checked: {
         get() {
           return this.todoObj.done
         },
         set(v) {
           this.todoObj.done = v
         }
       }
     }
   }
   </script>
   
   <style scoped>......
   </style>
   ```

5. MyList.vue

   ``` vue
   <template>
     <ul class="todo-main">
       <MyItem v-for="todoObj in myTodos" :key="todoObj.id" :todoObj="todoObj"></MyItem>    //注意
     </ul>
   </template>
   
   <script>
   import MyItem from './MyItem'
   export default {
     name: "MyList",
     components: {
       MyItem,
     },
     props: ['todos', ],   // 注意
     data() {
       return {
         // myTodos: this.todos
       }
     },
     computed:{
       myTodos(){
         return this.todos;
       }
     }
   }
   </script>
   
   <style scoped>......
   </style>
   ```



## 终极版,使用消息订阅与发布

1. 安装包`npm i pubsub-js`

2. vue-config.js

   ``` js
   // vue.config.js
   module.exports = {
       devServer: {
           overlay: {
               warnings: false, //不显示警告
               errors: false	//不显示错误
           }
       },
       lintOnSave:false //关闭eslint检查
   }
   ```

3. Main.js

   ``` js
   import Vue from 'vue'
   import App from './App.vue'
   Vue.config.productionTip = false
   new Vue({
       render: h => h(App),
   }).$mount('#app')
   ```

4. App.vue

   ``` vue
   <template>
     <div id="root">
       <div class="todo-container">
         <div class="todo-wrap">
           <MyHeader></MyHeader>
           <MyList :todos="todos"></MyList>
           <MyFooter :todos="todos"></MyFooter>
         </div>
       </div>
     </div>
   </template>
   
   <script>
   import MyHeader from './components/MyHeader.vue'
   import MyFooter from './components/MyFooter.vue'
   import MyList from './components/MyList.vue'
   import pubsub from 'pubsub-js'
   
   export default {
     name: 'App',
     components: {
       MyHeader,
       MyFooter,
       MyList,
     },
     data() {
       return {
         todos: JSON.parse(localStorage.getItem('todos')) || []
       }
     },
     methods: {
       // 增
       addTodo(value) {
         this.todos.unshift(value)
       },
       // 改
       updateTodo(id) {
         console.log("aaaas")
         this.todos.forEach(todo => {
           if (todo.id === id) {
             todo.done = !todo.done;
           }
         })
       },
       // 删
       delTodo(id) {
         this.todos = this.todos.filter(todo => todo.id !== id)    // 用过滤替代删除
       },
       selectAllTodo(status) {
         this.todos.forEach(todo => todo.done = status)
       },
       cleanDone() {
         this.todos = this.todos.filter(todo => !todo.done)    // 用过滤替代删除
       }
     },
     watch: {
       todos: {
         deep: true,
         handler(value) {
           localStorage.setItem('todos', JSON.stringify(value));
         }
       }
     },
     mounted() {
       pubsub.subscribe('delTodo', (news, id) => {
         this.delTodo(id)
       });
       pubsub.subscribe('selectAllTodo', (news, e) => {
         this.selectAllTodo(e)
       });
       pubsub.subscribe('cleanDone', () => {
         this.cleanDone()
       });
       pubsub.subscribe('addTodo', (news, value) => {
         this.addTodo(value)
       });
     },
   
   }
   </script>
   
   <style>// 参照上面
   </style>
   ```

5. header.vue

   ``` vue
   <template>
     <div class="todo-header">
       <input type="text" placeholder="请输入你的任务名称，按回车键确认" v-model="title" @keyup.enter="add()"/>
     </div>
   </template>
   
   <script>
   import {nanoid} from 'nanoid'
   import pubsub from 'pubsub-js'
   export default {
     name: "MyHeader",
     data() {
       return {
         title: ''
       }
     },
     methods: {
       add() {
         if (this.title.trim() === '') {
           alert('输入不能为空')
         } else {
           //将用户的输入包装成一个对象
           const todoObj = {id: nanoid(), title: this.title.trim(), done: false};
           // this.addTodo(todoObj);
           pubsub.publish('addTodo',todoObj)
           this.title = '';
         }
       }
     }
   }
   </script>
   <style scoped>// 参照上面
   </style>
   ```

6. List.vue

   ``` vue
   <template>
     <ul class="todo-main">
       <MyItem v-for="todoObj in myTodos" :key="todoObj.id" :todoObj="todoObj"></MyItem>
     </ul>
   </template>
   
   <script>
   import MyItem from './MyItem'
   
   export default {
     name: "MyList",
     components: {
       MyItem,
     },
     props: ['todos', ],
     },
     computed:{
       myTodos(){
         return this.todos;
       }
     }
   }
   </script>
   <style scoped>
   </style>
   ```

7. item.vue

   ``` vue
   <template>
     <li>
       <label>
         <input type="checkbox" v-model="checked" />
         <span>{{ title }}</span>
       </label>
       <button class="btn btn-danger" @click="del(id)">删除</button>
     </li>
   </template>
   <script>
   import pubsub from 'pubsub-js'
   export default {
     name: "MyItem",
     props: ['todoObj',],
     data() {
       return {
         id: this.todoObj.id,
         title: this.todoObj.title,
       }
     },
     methods: {
       del(id) {
         if (confirm('确定删除吗?')) {
           pubsub.publish('delTodo',id)
         }
       }
     },
     mounted() {
     },
     computed: {
       checked: {
         get() {
           return this.todoObj.done
         },
         set(v) {
           this.todoObj.done = v
         }
       }
     }
   }
   </script>
   ```

8. footer.vue

   ``` vue
   <template>
     <div class="todo-footer" v-if="total">
       <label>
         <input type="checkbox" v-model="checkStatus"/>
       </label>
       <span>
             <span>已完成{{ doneTotal }}</span> / 全部{{ total }}
           </span>
       <button class="btn btn-danger" @click="cleanAll">清除已完成任务</button>
     </div>
   </template>
   
   <script>
   import pubsub from 'pubsub-js'
   export default {
     name: "MyFooter",
     props: ['todos',],
     methods: {
       cleanAll() {
         pubsub.publish('cleanDone');
       }
     },
     data() {
       return {}
     },
     mounted() {
     },
     computed: {
       total() {
         return this.todos.length
       },
       doneTotal() {
         return this.todos.reduce((prev, curr) => prev + (curr.done ? 1 : 0), 0)
       },
       checkStatus: {
         get() {
           return this.total === this.doneTotal & this.total > 0
         },
         set(e) {
           pubsub.publish('selectAllTodo',e);
         }
       },
     },
   }
   </script>
   ```

   

## 新增编辑功能

https://www.bilibili.com/video/BV1Zy4y1K7SH?p=89

1. 不动原数组情况下新增属性
2. 添加的属性没有响应式怎么办
3. 判断身上有没有某个属性  
4. 传递事件 $event   获取事件的值$event.target.value

1. vue-config.js

   ``` js
   // vue.config.js
   module.exports = {
       devServer: {
           overlay: {
               warnings: false, //不显示警告
               errors: false	//不显示错误
           }
       },
       lintOnSave:false //关闭eslint检查
   }
   ```

2. Main.js

   ``` js
   import Vue from 'vue'
   import App from './App.vue'
   Vue.config.productionTip = false
   new Vue({
       render: h => h(App),
   }).$mount('#app')
   ```

3. App.vue

   ``` vue
   <template>
     <div id="root">
       <div class="todo-container">
         <div class="todo-wrap">
           <MyHeader></MyHeader>
           <MyList :todos="todos"></MyList>
           <MyFooter :todos="todos"></MyFooter>
         </div>
       </div>
     </div>
   </template>
   
   <script>
   import MyHeader from './components/MyHeader.vue'
   import MyFooter from './components/MyFooter.vue'
   import MyList from './components/MyList.vue'
   import pubsub from 'pubsub-js'
   
   export default {
     name: 'App',
     components: {
       MyHeader,
       MyFooter,
       MyList,
     },
     data() {
       return {
         todos: JSON.parse(localStorage.getItem('todos')) || []
       }
     },
     methods: {
       // 增
       addTodo(value) {
         this.todos.unshift(value)
       },
       // 改
       updateTodo(id,title) {
         this.todos.forEach(todo => {
           if (todo.id === id) {
             todo.title = title;
           }
         })
       },
       // 删
       delTodo(id) {
         this.todos = this.todos.filter(todo => todo.id !== id)    // 用过滤替代删除
       },
       selectAllTodo(status) {
         this.todos.forEach(todo => todo.done = status)
       },
       cleanDone() {
         this.todos = this.todos.filter(todo => !todo.done)    // 用过滤替代删除
       }
     },
     watch: {
       todos: {
         deep: true,
         handler(value) {
           localStorage.setItem('todos', JSON.stringify(value));
         }
       }
     },
     mounted() {
       pubsub.subscribe('delTodo', (news, id) => {
         this.delTodo(id)
       });
       pubsub.subscribe('updateTodo', (news, todo) => {
         this.updateTodo(todo.id,todo.title)
       });
       pubsub.subscribe('selectAllTodo', (news, e) => {
         this.selectAllTodo(e)
       });
       pubsub.subscribe('cleanDone', () => {
         this.cleanDone()
       });
       pubsub.subscribe('addTodo', (news, value) => {
         this.addTodo(value)
       });
     },
   
   }
   </script>
   
   <style>
   /*base*/
   body {
     background: #fff;
   }
   .btn {
     display: inline-block;
     padding: 4px 12px;
     margin-bottom: 0;
     font-size: 14px;
     line-height: 20px;
     text-align: center;
     vertical-align: middle;
     cursor: pointer;
     box-shadow: inset 0 1px 0 rgba(255, 255, 255, 0.2), 0 1px 2px rgba(0, 0, 0, 0.05);
     border-radius: 4px;
   }
   .btn-danger {
     color: #fff;
     background-color: #c91f17;
     border: 1px solid #bd362f;
   }
   .btn-danger:hover {
     color: #fff;
     background-color: #bd362f;
   }
   .btn-edit {
     color: #fff;
     margin-right: 10px;
     background-color: #4677ec;
     border: 1px solid #4677ec;
   }
   .btn-edit:hover {
     color: #fff;
     background-color: #4677ec;
   }
   .btn:focus {
     outline: none;
   }
   .todo-container {
     width: 600px;
     margin: 0 auto;
   }
   .todo-container .todo-wrap {
     padding: 10px;
     border: 1px solid #ddd;
     border-radius: 5px;
   }
   </style>
   ```

4. header.vue

   ``` vue
   <template>
     <div class="todo-header">
       <input type="text" placeholder="请输入你的任务名称，按回车键确认" v-model="title" @keyup.enter="add()"/>
     </div>
   </template>
   
   <script>
   import {nanoid} from 'nanoid'
   import pubsub from 'pubsub-js'
   export default {
     name: "MyHeader",
     data() {
       return {
         title: ''
       }
     },
     methods: {
       add() {
         if (this.title.trim() === '') {
           alert('输入不能为空')
         } else {
           //将用户的输入包装成一个对象
           const todoObj = {id: nanoid(), title: this.title.trim(), done: false};
           // this.addTodo(todoObj);
           pubsub.publish('addTodo',todoObj)
           this.title = '';
         }
       }
     }
   }
   </script>
   <style scoped>// 参照上面
   </style>
   ```

5. List.vue

   ``` vue
   <template>
     <ul class="todo-main">
       <MyItem v-for="todoObj in myTodos" :key="todoObj.id" :todoObj="todoObj"></MyItem>
     </ul>
   </template>
   
   <script>
   import MyItem from './MyItem'
   
   export default {
     name: "MyList",
     components: {
       MyItem,
     },
     props: ['todos', ],
     },
     computed:{
       myTodos(){
         return this.todos;
       }
     }
   }
   </script>
   <style scoped>
   </style>
   ```

6. item.vue

   ``` vue
   <template>
     <li>
       <label>
         <input type="checkbox" v-model="checked"/>
         <span v-show="!todo.isEdit">{{ todo.title }}</span>
         <input type="text" class="editInput" v-show="todo.isEdit" v-model="todo.title"
                @blur="saveItem(todo)"
                ref="inputEdit" />
       </label>
       <button class="btn btn-danger" @click="del(todo.id)">删除</button>
       <button class="btn btn-edit" @click="editName(todo)">编辑</button>
     </li>
   </template>
   <script>
   import pubsub from 'pubsub-js'
   export default {
     name: "MyItem",
     props: ['todoObj',],
     data() {
       return {
         todo: this.todoObj
       }
     },
     methods: {
       del(id) {
         if (confirm('确定删除吗?')) {
           pubsub.publish('delTodo', id)
         }
       },
       editName(todo) {
         if (todo.hasOwnProperty('isEdit')) {
           todo.isEdit=true
         }else this.$set(todo, 'isEdit', true);
         this.$nextTick(()=>{
           this.$refs.inputEdit.focus();})
       },
       saveItem(todo){
         pubsub.publish('updateTodo', todo)
         todo.isEdit=false
       },
     },
     computed: {
       checked: {
         get() {
           return this.todo.done
         },
         set(v) {
           this.todo.done = v
         }
       }
     }
   }
   </script>
   <style scoped>
   li {
     list-style: none;
     height: 36px;
     line-height: 36px;
     padding: 0 5px;
     border-bottom: 1px solid #ddd;
   }
   li label {
     float: left;
     cursor: pointer;
   }
   li label li input {
     vertical-align: middle;
     margin-right: 6px;
     position: relative;
     top: -1px;
   }
   li button {
     float: right;
     display: none;
     margin-top: 3px;
   }
   li:before {
     content: initial;
   }
   li:hover {
     background-color: #dddddd;
   }
   li:hover button {
     display: block;
   }
   li:last-child {
     border-bottom: none;
   }
   .editInput {
     height: 20px;
     font-size: 15px;
   }
   </style>
   ```

7. footer.vue

   ``` vue
   <template>
     <div class="todo-footer" v-if="total">
       <label>
         <input type="checkbox" v-model="checkStatus"/>
       </label>
       <span>
             <span>已完成{{ doneTotal }}</span> / 全部{{ total }}
           </span>
       <button class="btn btn-danger" @click="cleanAll">清除已完成任务</button>
     </div>
   </template>
   
   <script>
   import pubsub from 'pubsub-js'
   export default {
     name: "MyFooter",
     props: ['todos',],
     methods: {
       cleanAll() {
         pubsub.publish('cleanDone');
       }
     },
     data() {
       return {}
     },
     mounted() {
     },
     computed: {
       total() {
         return this.todos.length
       },
       doneTotal() {
         return this.todos.reduce((prev, curr) => prev + (curr.done ? 1 : 0), 0)
       },
       checkStatus: {
         get() {
           return this.total === this.doneTotal & this.total > 0
         },
         set(e) {
           pubsub.publish('selectAllTodo',e);
         }
       },
     },
   }
   </script>
   ```








# 浏览器本地存储webStorage

需求: 用户不登录账户的情况下 搜索的历史  在用户本地保存一份

思路: 使用js的API`window.localStorage.xxx()`和`window.sessionStorage.xxx()`

区别: 

1. window.localStorage.xxx()         浏览器关闭依然在
2. window.sessionStorage.xxx()        浏览器关了就没了

代码:

1. localStorage

   ``` html
   <body>
   <h1>测试 浏览器本地缓存  查看方式开发者模式下-> 应用</h1>
   
   <button onclick="a()">点击</button>
   <script>
       function a() {
           // 新增方法
           window.localStorage.setItem('msg','这里必须是字符串 如果不是会自动转字符串')
           let st = {'msg':'当然如果不是字符串 就要使用JSON.stringify()进行转译'}
           window.localStorage.setItem('msg1', JSON.stringify(st));
           // 查找
           let item = window.localStorage.getItem('msg1');           // 读取没有的key 返回null
           console.log(JSON.parse(item));    // 解析一个json对象
           // 删除
           window.localStorage.removeItem('msg')     // 删除一个
           window.localStorage.clear()    // 清空
       }
   </script>
   </body>
   </html>
   ```

   

2. sessionStorage

   ``` html
   <body>
   <h1>测试 浏览器本地缓存  查看方式开发者模式下-> 应用</h1>
   <button onclick="a()">点击</button>
   <script>
       function a() {
           // 新增方法
           window.sessionStorage.setItem('msg','这里必须是字符串 如果不是会自动转字符串')
           let st = {'msg':'当然如果不是字符串 就要使用JSON.stringify()进行转译'}
           window.sessionStorage.setItem('msg1', JSON.stringify(st));
           // 查找
           let item = window.sessionStorage.getItem('msg1');           // 读取没有的key 返回null
           console.log(JSON.parse(item));    // 解析一个json对象
           // 删除
           window.sessionStorage.removeItem('msg')     // 删除一个
           window.sessionStorage.clear()    // 清空
       }
   </script>
   </body>
   </html>
   ```



# Vue构建工具



## 01_环境准备

### 1.下载nodejs

    http://nodejs.cn/download/
        windows系统:   .msi  安装包(exe)指定安装位置   .zip(压缩包)直接解压缩指定目录
        mac os 系统:   .pkg  安装包格式自动配置环境变量  .tar.gz(压缩包)解压缩安装到指定名

### 2.配置nodejs环境变量

    1.windows系统:
        计算上右键属性---->  高级属性 ---->环境变量 添加如下配置:
        NODE_HOME=  nodejs安装目录
        PATH    = xxxx;%NODE_HOME%
    2.macos 系统
        推荐使用.pkg安装直接配置node环境

### 3.验证nodejs环境是否成功

    node -v 

### 4.npm介绍

    node package mangager   nodejs包管理工具       前端主流技术  npm 进行统一管理
    maven 管理java后端依赖    远程仓库(中心仓库)      阿里云镜像
    npm   管理前端系统依赖     远程仓库(中心仓库)      配置淘宝镜像

### 5.配置淘宝镜像

      npm config set registry https://registry.npm.taobao.org
      npm config get registry

### 6.配置npm下载依赖位置

     windows:
        npm config set cache "D:\nodereps\npm-cache"
        npm config set prefix "D:\nodereps\npm_global"
     mac os:
        npm config set cache "/Users/chenyannan/dev/nodereps"
        npm config set prefix "/Users/chenyannan/dev/nodereps"

### 7.验证nodejs环境配置

    npm config ls
    ; userconfig /Users/chenyannan/.npmrc
    cache = "/Users/chenyannan/dev/nodereps"
    prefix = "/Users/chenyannan/dev/nodereps"
    registry = "https://registry.npm.taobao.org/"

## 2.安装脚手架

### 0.卸载脚手架

    npm uninstall -g @vue/cli  //卸载3.x版本脚手架
    npm uninstall -g vue-cli  //卸载2.x版本脚手架

### 1.Vue Cli官方网站

    https://cli.vuejs.org/zh/guide/

### 2.安装vue Cli

    npm install -g @vue/cli

## 3.第一个vue脚手架项目

### 1.创建vue脚手架第一个项目

    vue create  项目名
### 2.创建第一个项目

    |-node_modules       -- 所有的项目依赖包都放在这个目录下
    |-public             -- 公共文件夹
    ---|favicon.ico      -- 网站的显示图标
    ---|index.html       -- 入口的html文件
    |-src                -- 源文件目录，编写的代码基本都在这个目录下
    ---|assets           -- 放置静态文件的目录，比如logo.pn就放在这里
    ---|components       -- Vue的组件文件，自定义的组件都会放到这
    ---|router           -- vue-router vue路由的配置文件，
    ---|store            -- 存放 vuex 为vue专门开发的状态管理器，
    ---|views            -- 存放视图文件，
    ---|App.vue          -- 根组件，这个在Vue2中也有
    ---|main.ts          -- 入口文件，因为采用了TypeScript所以是ts结尾
    ---|shims-vue.d.ts   -- 类文件(也叫定义文件)，因为.vue结尾的文件在ts中不认可，所以要有定义文件
    |-.browserslistrc    -- 在不同前端工具之间公用目标浏览器和node版本的配置文件，作用是设置兼容性
    |-.eslintrc.js       -- Eslint的配置文件，不用作过多介绍
    |-.gitignore         -- 用来配置那些文件不归git管理
    |-package.json       -- 命令配置和包管理文件
    |-README.md          -- 项目的说明文件，使用markdown语法进行编写
    |-vue.config.json    -- 请求代理, webpack 配置, 打包输出等都会在这里配置
    |-yarn.lock          -- 使用yarn后自动生成的文件，由Yarn管理，安装yarn包时的重要信息存储到yarn.lock文件中

### 3.如何运行在项目的根目录中执行

    npm run 运行前端系统

### 4.如何访问项目

    http://localhost:8081    

### 5.Vue Cli中项目开发方式

     注意: 一切皆组件   一个组件中   js代码  html代码  css样式
        1. VueCli开发方式是在项目中开发一个一个组件对应一个业务功能模块,日后可以将多个组件组合到一起形成一个前端系统
        2. 日后在使用vue Cli进行开发时不再书写html,编写的是一个个组件(组件后缀.vue结尾的文件),日后打包时vue cli会将组件编译成运行的html文件 





## 02_Vue-Cli

官方文档：https://cli.vuejs.org/zh/guide/creating-a-project.html#vue-create

```bash
## 查看@vue/cli版本，确保@vue/cli版本在4.5.0以上
vue --version
## 安装或者升级你的@vue/cli
npm install -g @vue/cli
## 创建
vue create vue_test
## 启动
cd vue_test
npm run serve
```





## 03_Vite



官方文档：https://v3.cn.vuejs.org/guide/installation.html#vite

vite官网：https://vitejs.cn

- 什么是vite？—— 新一代前端构建工具。
- 优势如下：
  - 开发环境中，无需打包操作，可快速的冷启动。
  - 轻量快速的热重载（HMR）。
  - 真正的按需编译，不再等待整个应用编译完成。
- 传统构建 与 vite构建对比图

<img src="https://cn.vitejs.dev/assets/bundler.37740380.png" style="width:500px;height:280px;float:left" /><img src="https://cn.vitejs.dev/assets/esm.3070012d.png" style="width:480px;height:280px" />

```bash
## 创建工程
npm init vite-app <project-name>
## 进入工程目录
cd <project-name>
## 安装依赖
npm install
## 运行
npm run dev
```



# 常用 Composition API

官方文档: https://v3.cn.vuejs.org/guide/composition-api-introduction.html

## 1.拉开序幕的setup

1. 理解：Vue3.0中一个新的配置项，值为一个函数。
2. setup是所有<strong style="color:#DD5145">Composition API（组合API）</strong><i style="color:gray;font-weight:bold">“ 表演的舞台 ”</i>。
3. 组件中所用到的：数据、方法等等，均要配置在setup中。
4. setup函数的两种返回值：
   1. 若返回一个对象，则对象中的属性、方法, 在模板中均可以直接使用。（重点关注！）
   2. <span style="color:#aad">若返回一个渲染函数：则可以自定义渲染内容。（了解）</span>
5. 注意点：
   1. 尽量不要与Vue2.x配置混用
      - Vue2.x配置（data、methos、computed...）中<strong style="color:#DD5145">可以访问到</strong>setup中的属性、方法。
      - 但在setup中<strong style="color:#DD5145">不能访问到</strong>Vue2.x配置（data、methos、computed...）。
      - 如果有重名, setup优先。
   2. setup不能是一个async函数，因为返回值不再是return的对象, 而是promise, 模板看不到return对象中的属性。（后期也可以返回一个Promise实例，但需要Suspense和异步组件的配合）

##  2.ref函数

- 作用: 定义一个响应式的数据

- 语法: ```const xxx = ref(initValue)``` 

  - 创建一个包含响应式数据的<strong style="color:#DD5145">引用对象（reference对象，简称ref对象）</strong>。
  - JS中操作数据： ```xxx.value```
  - 模板中读取数据: 不需要.value，直接：```<div>{{xxx}}</div>```

- 备注：

  - 接收的数据可以是：基本类型、也可以是对象类型。
  - 基本类型的数据：响应式依然是靠``Object.defineProperty()``的```get```与```set```完成的。
  - 对象类型的数据：内部 <i style="color:gray;font-weight:bold">“ 求助 ”</i> 了Vue3.0中的一个新函数—— ```reactive```函数。

- 实例:

  ``` js
  <template>
  <h1>app组件</h1>
    <h2>姓名: {{name}}</h2>
    <h2>年龄:{{age}}</h2>
    <h2>岗位:{{job.type}}</h2>
    <h2>公司:{{job.company}}</h2>
  <button @click="sayHello">张三说</button>
  <button @click="changeInfo">修改信息 响应式的</button>
  </template>
  <script>
  import {ref} from 'vue'
  
  export default { // vue3的形式
    name: 'App',
    // 此处只是这是setup  暂时不考虑响应式
    setup(){
      // 数据 之前是data  现在直接在这里写
      let name = ref( '张三')
      let age =  ref(13)
      let job = ref({
        type: '前端',
        company: '腾讯'
      })
      // 方法  之前在methods中写
      function sayHello(){
        alert(name.value+': hello')
      }
      function changeInfo(){
        name.value = '王五'
        age.value = 14
        job.value.type = '后端'
        job.value.company = '百度'
      }
      return {
        // 将内部的数据和方法暴露出去
        name,
        age,
        job,
        sayHello,
        changeInfo
      }
    }
  }
  ```

  

## 3.reactive函数

- 作用: 定义一个<strong style="color:#DD5145">对象类型</strong>的响应式数据（基本类型不要用它，要用```ref```函数）

- 语法：```const 代理对象= reactive(源对象)```接收一个对象（或数组），返回一个<strong style="color:#DD5145">代理对象（Proxy的实例对象，简称proxy对象）</strong>

- reactive定义的响应式数据是“深层次的”。

- 内部基于 ES6 的 Proxy 实现，通过代理对象操作源对象内部数据进行操作。

- 实例:  对于对象类型可以不用使用`value`

  ``` js
  <template>
  <h1>app组件</h1>
    <h2>姓名: {{name}}</h2>
    <h2>年龄:{{age}}</h2>
    <h2>岗位:{{job.type}}</h2>
    <h2>公司:{{job.company}}</h2>
  <button @click="sayHello">张三说</button>
  <button @click="changeInfo">修改信息 响应式的</button>
  </template>
  <script>
  import {ref,reactive} from 'vue'
  
  export default { // vue3的形式
    name: 'App',
    // 此处只是这是setup  暂时不考虑响应式
    setup(){
      // 数据 之前是data  现在直接在这里写
      let name = ref( '张三')
      let age =  ref(13)
      let job = reactive({
        type: '前端',
        company: '腾讯'
      })
      // 方法  之前在methods中写
      function sayHello(){
        alert(name.value+': hello')
      }
      function changeInfo(){
        name.value = '王五'
        age.value = 14
        job.type = '后端'
        job.company = '百度'
      }
      return {
        // 将内部的数据和方法暴露出去
        name,
        age,
        job,
        sayHello,
        changeInfo
      }
    }
  }
  ```



## 4.Vue3.0中的响应式原理



### vue2.x的响应式

- 实现原理：

  - 对象类型：通过```Object.defineProperty()```对属性的读取、修改进行拦截（数据劫持）。

  - 数组类型：通过重写更新数组的一系列方法来实现拦截。（对数组的变更方法进行了包裹）。

    ```js
    Object.defineProperty(data, 'count', {
        get () {}, 
        set () {}
    })
    ```

- 存在问题：

  - 新增属性、删除属性, 界面不会更新。
  - 直接通过下标修改数组, 界面不会自动更新。

### Vue3.0的响应式

- 实现原理: 

  - 通过Proxy（代理）:  拦截对象中任意属性的变化, 包括：属性值的读写、属性的添加、属性的删除等。

  - 通过Reflect（反射）:  对源对象的属性进行操作。

  - MDN文档中描述的Proxy与Reflect：

    - Proxy：https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Proxy

    - Reflect：https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Reflect

      ```js
      new Proxy(data, {
      	// 拦截读取属性值
          get (target, prop) {
          	return Reflect.get(target, prop)
          },
          // 拦截设置属性值或添加新属性
          set (target, prop, value) {
          	return Reflect.set(target, prop, value)
          },
          // 拦截删除属性
          deleteProperty (target, prop) {
          	return Reflect.deleteProperty(target, prop)
          }
      })
      
      proxy.name = 'tom'   
      ```

## 5.reactive对比ref

-  从定义数据角度对比：

   -  ref用来定义：<strong style="color:#DD5145">基本类型数据</strong>。
   -  reactive用来定义：<strong style="color:#DD5145">对象（或数组）类型数据</strong>。
   -  备注：ref也可以用来定义<strong style="color:#DD5145">对象（或数组）类型数据</strong>, 它内部会自动通过```reactive```转为<strong style="color:#DD5145">代理对象</strong>。

-  从原理角度对比：

   -  ref通过``Object.defineProperty()``的```get```与```set```来实现响应式（数据劫持）。
   -  reactive通过使用<strong style="color:#DD5145">Proxy</strong>来实现响应式（数据劫持）, 并通过<strong style="color:#DD5145">Reflect</strong>操作<strong style="color:orange">源对象</strong>内部的数据。

-  从使用角度对比：

   -  ref定义的数据：操作数据<strong style="color:#DD5145">需要</strong>```.value```，读取数据时模板中直接读取<strong style="color:#DD5145">不需要</strong>```.value```。
   -  reactive定义的数据：操作数据与读取数据：<strong style="color:#DD5145">均不需要</strong>```.value```。

-  实例代码:

   ``` js
   <template>
     <h1>---------响应式增删改查数据组件---------------</h1>
     <ul>
       <li>姓名: {{name1}}</li>
       <li>年龄: {{age}}</li>
       <li v-show="sex">性别: {{sex}}</li>
       <li>岗位:{{job.type}}</li>
       <li>公司:{{job.company}}</li>
       <li v-show="job.address">地址:{{job.address}}</li>
     </ul>
   <button @click="delSex()">删除性别</button>
   <button @click="addAddress()">新增地址</button>
   <button @click="delAddress()">删除地址</button>
   </template>
   <script>
   import {ref, reactive} from "vue";
   export default {
     name: "app_responsive",   //响应式
     setup(){
       let name1 = ref('张三')
       let age = ref(11)
       let sex = ref('男')
       let job = reactive({
         type: '前端',
         company: '腾讯'
       })
       function delSex() {
         sex.value = undefined
       }
       function addAddress() {
         job.address = '西安'
       }
       function delAddress(){
         delete job.address;
       }
       return{
         name1,
         age,
         sex,
         job,
         delSex,
         addAddress,
         delAddress
       }
     }
   }
   ```

   

## 6.setup的两个注意点

- setup执行的时机
  - 在beforeCreate之前执行一次，this是undefined。

- setup的参数
  - props：值为对象，包含：组件外部传递过来，且组件内部声明接收了的属性。
  - context：上下文对象
    - attrs: 值为对象，包含：组件外部传递过来，但没有在props配置中声明的属性, 相当于 ```this.$attrs```。
    - slots: 收到的插槽内容, 相当于 ```this.$slots```。
    - emit: 分发自定义事件的函数, 相当于 ```this.$emit```。



## 7.vue3组件通信

> 生命周期钩子函数自定义事件

  vue3的组件通信有四种

1. props / emits
2. provide / inject
3. vuex全局缓存
4. mixin局部混入



### 7.1 props和emits

+ Props：父组件传值子组件接受
  Props：可以是数组也可以是对象
  +  可以传递任何数据类型，函数，布尔，字符串等等等
  + 可以根据需求传静态或者动值
  + 可以对传递的值进行验证
  +  可以设置默认值

+ emits 子组件传值给父组件
  +  子组件触发父组件自定义函数传入参数给父组件, 父组件调用函数得到子组件传来的值

  

### 7.2 provide / inject



### 7.3 vuex全局缓存



### 7.4 mixin局部混入





### 7.5插槽

+ 理解: 在子组件写一个插槽(挖一个坑 但是坑的内容等父组件来填充)

+ 使用: 

  + 默认插槽: 当子组件只有一个插槽时

    ``` html
    // 这是子组件部分
    <slot>这里可以写插槽的默认值 即父组件没有使用的时候</slot>
    // 这是父组件部分
      <slots_test>
        这里写的任何东西都可以被渲染到插槽中  只要是父组件作用域的所有都可以写 
        <span>甚至可以使用变量如: {{value11}}</span>
    	</slots_test>
    ```

     

  + 命名插槽: 当子组件有多个插槽时

    ``` html
    // 这是子组件部分
      <slot name="head"></slot>
      <slot name="main"></slot>
      <slot name="foot"></slot>
    // 这是父组件部分
      <slots_test>
        <template v-slot:head>这是头</template>
        <template v-slot:main>这是中</template>
        <template v-slot:foot>这是下</template>
    	</slots_test>
    ```
  
  
    + 作用域插槽: 可以用于子组件向父组件传输数据 
  
      ``` html
      // 这是子组件部分
        <span v-for="(item, index) in dataVal">
          // 这里的子组件提供了一个data接口  供父组件使用
          <slot :data="item" name="dataSlot"></slot>  
          <br>
        </span>
      // 这是父组件部分
        <slots_test>  // 这里父组件接收了data数据
          <template v-slot:dataSlot="{data}">{{ data.name }}--{{ data.age }}		   		 			</template>
      	</slots_test>
      ```
  
      
  
    + 动态插槽名
  
      ``` html
      // 这是子组件部分
        <slot name="dySlot"></slot>
      // 这是父组件部分  下面记得写dySlot的变量
        <slots_test> // #是v-slot:的缩写
          <template #[dySlot]>这是动态插槽名</template>
      	</slots_test>
      ```
  
      
  






### 7.6 代码实例 

1. app.vue

   ``` vue
   <template>
     <h1>app组件</h1>
     <app_responsive></app_responsive>
     <h1>---------组件间通信自定义事件---------------</h1>
     <app_test :value1=value11 :value2=value12 @emitSon1="emitFather1" @emitSon2="emitFather2"></app_test>
   
   <h1>---------插槽---------------</h1>
     <slots_test>
       这里写的任何东西都可以被渲染到插槽中
       <br>
       <span>甚至可以使用变量如: {{value11}}</span>
       <span>变量的做同于和父组件相同(当前) 子组件的内容一般不可以使用(除了通信之外)</span>
       <template v-slot:head>这是头</template>
       <template v-slot:main>这是中</template>
       <template #foot>#是v-slot:的缩写  这是下</template>
       <template v-slot:dataSlot="{data}">{{ data.name }}--{{ data.age }}</template>
       <template #[dySlot]>这是动态插槽名</template>
     </slots_test>
   </template>
   <script>
   import app_responsive from './components/响应式增删改查数据.vue'
   import app_test from './components/生命周期组件间通信自定义事件.vue'
   import slots_test from  './components/插槽.vue'
   import {reactive, ref, toRefs} from "vue";
   export default { // vue3的形式
     name: 'App',
     components: {
       app_responsive,
       app_test,
       slots_test,
     },
     setup() {
       let dySlot = ref("dySlot")
       let app_test_value = reactive(
           {
             value11: "props组件间通信动态绑定",
             value12: "props组件间通信方法动态绑定"
           }
       )
       function emitFather1(data) {
         console.log('我收到子组件的数据了:',  data);
   
       }
       function emitFather2(data) {
         console.log('我收到子组件的数据了:',  data);
       }
       return {
         ...toRefs(app_test_value),   // 结构化后 使用可以不写变量名.变量
         emitFather1,
         emitFather2,
         dySlot
       }
   
     },
   }
   </script>
   <style>
   #app {
     font-family: Avenir, Helvetica, Arial, sans-serif;
     -webkit-font-smoothing: antialiased;
     -moz-osx-font-smoothing: grayscale;
     text-align: center;
     color: #2c3e50;
     margin-top: 60px;
   }
   </style>
   ```

2. 生命周期组件间通信自定义事件

   ``` vue
   <template>
     <ul>
       <li v-show="value1">appName: {{ value1 }}</li>
       <li v-show="value2">appFun: {{ value2 }}</li>
       <li></li>
     </ul>
     <button @click="emitdiy1">emit给父组件传数据1</button>
     <button @click="emitdiy2">emit给父组件传数据2</button>
   </template>
   <script>
   import {getCurrentInstance, onMounted} from "vue";
   export default {
     name: 'app_test',
     props: ['value1','value2'],
     setup(props, emit) {
       const {proxy} = getCurrentInstance();
       function emitdiy1(data) {
         proxy.$emit('emitSon1', '子组件数据1');
       }
       function emitdiy2(data) {
         proxy.$emit('emitSon2', '子组件数据2');
       }
       onMounted(() => {  // 生命周期函数
         console.log('我是子组件生命周期,我被挂载了');
         // console.log(props);
         // console.log(proxy);
         // console.log(props.value1);    // 获取父组件传过来的值
         // console.log(emit);
       });
       return {emitdiy1, emitdiy2}
     },
   }
   </script>
   <style scoped>
   li {
     list-style-type: none;
   }
   </style>
   ```

3. 插槽

   ``` vue
   <template>
     <slot>这里可以写插槽的默认值 即父组件没有使用的时候</slot>
     <span>当然也可以写好几个插槽 比如网页的头 中 下 部分 可以像下面这种使用</span>
     <br>
     <slot name="head"></slot>
     <br>
     <slot name="main"></slot>
     <br>
     <slot name="foot"></slot>
     <br>
     <span v-for="(item, index) in dataVal">
       <slot :data="item" name="dataSlot"></slot>
       <br>
     </span>
     <br>
     <slot name="dySlot"></slot>
   </template>
   <script>
   import {reactive, toRefs} from "vue";
   export default {
     name: 'slots_test',
     setup() {
       let dataVal = reactive([
         {name: '张三', age: '18'},
         {name: '李四', age: '19'},
         {name: '王五', age: '20'},
       ])
       return {
         ...toRefs(dataVal),dataVal
       }
     },
   }
   </script>
   <style scoped>
   </style>
   ```
   
   

## 7.计算属性与监视

### 1.computed函数

- 与Vue2.x中computed配置功能一致

- 写法

  ```js
  import {computed} from 'vue'
  
  setup(){
      ...
  	//计算属性——简写
      let fullName = computed(()=>{
          return person.firstName + '-' + person.lastName
      })
      //计算属性——完整
      let fullName = computed({
          get(){
              return person.firstName + '-' + person.lastName
          },
          set(value){
              const nameArr = value.split('-')
              person.firstName = nameArr[0]
              person.lastName = nameArr[1]
          }
      })
  }
  ```

### 2.watch函数

- 与Vue2.x中watch配置功能一致

- 两个小“坑”：

  - 监视reactive定义的响应式数据时：oldValue无法正确获取、强制开启了深度监视（deep配置失效）。
  - 监视reactive定义的响应式数据中某个属性时：deep配置有效。

  ```js
  //情况一：监视ref定义的响应式数据
  watch(sum,(newValue,oldValue)=>{
  	console.log('sum变化了',newValue,oldValue)
  },{immediate:true})
  
  //情况二：监视多个ref定义的响应式数据
  watch([sum,msg],(newValue,oldValue)=>{
  	console.log('sum或msg变化了',newValue,oldValue)
  }) 
  
  /* 情况三：监视reactive定义的响应式数据
  			若watch监视的是reactive定义的响应式数据，则无法正确获得oldValue！！
  			若watch监视的是reactive定义的响应式数据，则强制开启了深度监视 
  */
  watch(person,(newValue,oldValue)=>{
  	console.log('person变化了',newValue,oldValue)
  },{immediate:true,deep:false}) //此处的deep配置不再奏效
  
  //情况四：监视reactive定义的响应式数据中的某个属性
  watch(()=>person.job,(newValue,oldValue)=>{
  	console.log('person的job变化了',newValue,oldValue)
  },{immediate:true,deep:true}) 
  
  //情况五：监视reactive定义的响应式数据中的某些属性
  watch([()=>person.job,()=>person.name],(newValue,oldValue)=>{
  	console.log('person的job变化了',newValue,oldValue)
  },{immediate:true,deep:true})
  
  //特殊情况
  watch(()=>person.job,(newValue,oldValue)=>{
      console.log('person的job变化了',newValue,oldValue)
  },{deep:true}) //此处由于监视的是reactive素定义的对象中的某个属性，所以deep配置有效
  ```

### 3.watchEffect函数

- watch的套路是：既要指明监视的属性，也要指明监视的回调。

- watchEffect的套路是：不用指明监视哪个属性，监视的回调中用到哪个属性，那就监视哪个属性。

- watchEffect有点像computed：

  - 但computed注重的计算出来的值（回调函数的返回值），所以必须要写返回值。
  - 而watchEffect更注重的是过程（回调函数的函数体），所以不用写返回值。

  ```js
  //watchEffect所指定的回调中用到的数据只要发生变化，则直接重新执行回调。
  watchEffect(()=>{
      const x1 = sum.value
      const x2 = person.age
      console.log('watchEffect配置的回调执行了')
  })
  ```





## 8.生命周期

+ Vue2的生命周期

<img src="https://tva1.sinaimg.cn/large/e6c9d24ely1h36r5atzhuj20u023zn2v.jpg" style="zoom:50%;" />



+ Vue3生命周期

  <img src="https://v3.cn.vuejs.org/images/lifecycle.svg" style="zoom:50%;" />





- Vue3.0中可以继续使用Vue2.x中的生命周期钩子，但有有两个被更名：
  - ```beforeDestroy```改名为 ```beforeUnmount```
  - ```destroyed```改名为 ```unmounted```
- Vue3.0也提供了 Composition API 形式的生命周期钩子，与Vue2.x中钩子对应关系如下：
  - `beforeCreate`===>`setup()`
  - `created`=======>`setup()`
  - `beforeMount` ===>`onBeforeMount`
  - `mounted`=======>`onMounted`
  - `beforeUpdate`===>`onBeforeUpdate`
  - `updated` =======>`onUpdated`
  - `beforeUnmount` ==>`onBeforeUnmount`
  - `unmounted` =====>`onUnmounted`

## 9.自定义hook函数

- 什么是hook？—— 本质是一个函数，把setup函数中使用的Composition API进行了封装。

- 类似于vue2.x中的mixin。

- 自定义hook的优势: 复用代码, 让setup中的逻辑更清楚易懂。



+ 不使用hooks:

  ``` js
  // app组件
  // 定义一个button联动下面的组件 控制隐藏和显示  (v-if)
  
  // vue组件
  <template>
  <span>当前鼠标点击的坐标为: x:{{point.x}} --  y:{{point.y}}</span>
  </template>
  
  <script>
  import { reactive, onMounted, onBeforeUnmount} from "vue";
  export default {
    name: "hooks_test",
    setup() {
  
      // 实现鼠标获取鼠标点击的坐标
      let point = reactive({
        x: 0,
        y: 0
      })
      function getPoint(e){
        point.x = e.clientX
        point.y = e.clientY
        console.log( point.x, point.y)
      }
      onMounted(() => {   // 挂在时候触发
        document.addEventListener('click', getPoint )
      })
      onBeforeUnmount(() => {   // 卸载时候触发
        document.removeEventListener('click',getPoint)
      })
      return {point}
    }
  }
  </script>
  <style scoped>
  </style>
  ```

  

+ 使用hooks

  单独将公共部分提取到一个新的js中 然后暴露出来即可  (一般在src/hooks下  命名为`useXXX.js`)

  ``` js
  // src/hooks/usePoint.js
  import {onBeforeUnmount, onMounted, reactive} from "vue";
  export default function (){
      // 实现鼠标获取鼠标点击的坐标
      let point = reactive({
          x: 0,
          y: 0
      })
      function getPoint(e){
          point.x = e.clientX
          point.y = e.clientY
          console.log( point.x, point.y)
      }
      onMounted(() => {   // 挂在时候触发
          document.addEventListener('click', getPoint )
      })
      onBeforeUnmount(() => {   // 卸载时候触发
          document.removeEventListener('click',getPoint)
      })
      return point
  }
  
  // hooks组件
  <template>
  <span>当前鼠标点击的坐标为: x:{{point.x}} --  y:{{point.y}}</span>
  </template>
  <script>
  import { reactive, onMounted, onBeforeUnmount} from "vue";
  import usePoint from "../hooks/usePoint";
  export default {
    name: "hooks_test",
    setup() {
      let point = usePoint();
      return {point}
    }
  }
  </script>
  <style scoped>
  </style>
  
  // App.vue代码和之前一样 不影响  创建一个按钮 点击控制组件隐藏显示即可
  ```

  

## 10.toRef

- 作用：创建一个 ref 对象，其value值指向另一个对象中的某个属性。
- 语法：```const name = toRef(person,'name')```
- 应用:   要将响应式对象中的某个属性单独提供给外部使用时。


- 扩展：```toRefs``` 与```toRef```功能一致，但可以批量创建多个 ref 对象，语法：```toRefs(person)```



# 其它 Composition API

## 1.shallowReactive 与 shallowRef

- shallowReactive：只处理对象最外层属性的响应式（浅响应式）。
- shallowRef：只处理基本数据类型的响应式, 不进行对象的响应式处理。

- 什么时候使用?
  -  如果有一个对象数据，结构比较深, 但变化时只是外层属性变化 ===> shallowReactive。
  -  如果有一个对象数据，后续功能不会修改该对象中的属性，而是生新的对象来替换 ===> shallowRef。

## 2.readonly 与 shallowReadonly

- readonly: 让一个响应式数据变为只读的（深只读）。
- shallowReadonly：让一个响应式数据变为只读的（浅只读）。
- 应用场景: 不希望数据被修改时。



## 3.toRaw 与 markRaw

- toRaw：
  - 作用：将一个由```reactive```生成的<strong style="color:orange">响应式对象</strong>转为<strong style="color:orange">普通对象</strong>。
  - 使用场景：用于读取响应式对象对应的普通对象，对这个普通对象的所有操作，不会引起页面更新。
- markRaw：
  - 作用：标记一个对象，使其永远不会再成为响应式对象。
  - 应用场景:
    1. 有些值不应被设置为响应式的，例如复杂的第三方类库等。
    2. 当渲染具有不可变数据源的大列表时，跳过响应式转换可以提高性能。



## 4.customRef

- 作用：创建一个自定义的 ref，并对其依赖项跟踪和更新触发进行显式控制。

- 实现防抖效果：

  ```vue
  <template>
  	<input type="text" v-model="keyword">
  	<h3>{{keyword}}</h3>
  </template>
  
  <script>
  	import {ref,customRef} from 'vue'
  	export default {
  		name:'Demo',
  		setup(){
  			// let keyword = ref('hello') //使用Vue准备好的内置ref
  			//自定义一个myRef
  			function myRef(value,delay){
  				let timer
  				//通过customRef去实现自定义
  				return customRef((track,trigger)=>{
  					return{
  						get(){
  							track() //告诉Vue这个value值是需要被“追踪”的
  							return value
  						},
  						set(newValue){
  							clearTimeout(timer)
  							timer = setTimeout(()=>{
  								value = newValue
  								trigger() //告诉Vue去更新界面
  							},delay)
  						}
  					}
  				})
  			}
  			let keyword = myRef('hello',500) //使用程序员自定义的ref
  			return {
  				keyword
  			}
  		}
  	}
  </script>
  ```




## 5.provide 与 inject

<img src="https://v3.cn.vuejs.org/images/components_provide.png" style="width:300px" />

- 作用：实现<strong style="color:#DD5145">祖与后代组件间</strong>通信

- 套路：父组件有一个 `provide` 选项来提供数据，后代组件有一个 `inject` 选项来开始使用这些数据

- 具体写法：

  1. 祖组件中：

     ```js
     setup(){
     	......
         let car = reactive({name:'奔驰',price:'40万'})
         provide('car',car)
         ......
     }
     ```

  2. 后代组件中：

     ```js
     setup(props,context){
     	......
         const car = inject('car')
         return {car}
     	......
     }
     ```

## 6.响应式数据的判断

- isRef: 检查一个值是否为一个 ref 对象
- isReactive: 检查一个对象是否是由 `reactive` 创建的响应式代理
- isReadonly: 检查一个对象是否是由 `readonly` 创建的只读代理
- isProxy: 检查一个对象是否是由 `reactive` 或者 `readonly` 方法创建的代理





# 新的组件

## 1.Fragment

- 在Vue2中: 组件必须有一个根标签
- 在Vue3中: 组件可以没有根标签, 内部会将多个标签包含在一个Fragment虚拟元素中
- 好处: 减少标签层级, 减小内存占用

## 2.Teleport

> 使得子组件的样式定位等等不必参照父元素 可以指定标签进行参照

- 什么是Teleport？—— `Teleport` 是一种能够将我们的<strong style="color:#DD5145">组件html结构</strong>移动到指定位置的技术。

  ```vue
  // app.vue 或者父组件 正常引入和展示在页面就行
  
  // 子组件代码
  <template>
    <button @click="isShow = true">点我显示弹窗</button>
    <teleport to="body">
    <div  v-if="isShow" class = "mask">
      <div class = "dialog">
        <h3>Teleport自定义组件位置</h3>
        <h4>注意这个居中不是通过app.vue进行控制的 而是由当前组件进行控制的</h4>
        <button @click="isShow = false">点我关闭弹窗</button>
      </div>
    </div>
    </teleport>
  
  </template>
  
  <script>
  import {ref} from "vue";
  export default {
    name: "Dialog",
    setup(){
      let isShow = ref(false)
      return {isShow}
    }
  
  }
  </script>
  
  <style scoped>
  
  .mask{
    position: fixed;
    top: 0;
    bottom: 0;
    left: 0;
    right: 0;
    background: rgba(0,0,0,0.5);
  }
  .dialog{
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%,-50%);
    text-align: center;
    width: 300px;
    height: 300px;
    background: #fff;
  }
  </style>
  ```



## 3.Suspense

- 等待异步组件时渲染一些额外内容，让应用有更好的用户体验

- 使用步骤：

  - 异步引入组件

    ```js
    import {defineAsyncComponent} from 'vue'
    const Child = defineAsyncComponent(()=>import('./components/Child.vue'))
    ```

  - 使用```Suspense```包裹组件，并配置好```default``` 与 ```fallback```

    ```vue
    <template>
    	<div class="app">
    		<h3>我是App组件</h3>
    		<Suspense>
    			<template v-slot:default>
    				<Child/>
    			</template>
    			<template v-slot:fallback>
    				<h3>加载中.....</h3>
    			</template>
    		</Suspense>
    	</div>
    </template>
    ```

# 其他

## 1.全局API的转移

- Vue 2.x 有许多全局 API 和配置。

  - 例如：注册全局组件、注册全局指令等。

    ```js
    //注册全局组件
    Vue.component('MyButton', {
      data: () => ({
        count: 0
      }),
      template: '<button @click="count++">Clicked {{ count }} times.</button>'
    })
    
    //注册全局指令
    Vue.directive('focus', {
      inserted: el => el.focus()
    }
    ```

- Vue3.0中对这些API做出了调整：

  - 将全局的API，即：```Vue.xxx```调整到应用实例（```app```）上

    | 2.x 全局 API（```Vue```） | 3.x 实例 API (`app`)                        |
    | ------------------------- | ------------------------------------------- |
    | Vue.config.xxxx           | app.config.xxxx                             |
    | Vue.config.productionTip  | <strong style="color:#DD5145">移除</strong> |
    | Vue.component             | app.component                               |
    | Vue.directive             | app.directive                               |
    | Vue.mixin                 | app.mixin                                   |
    | Vue.use                   | app.use                                     |
    | Vue.prototype             | app.config.globalProperties                 |

## 2.其他改变

- data选项应始终被声明为一个函数。

- 过度类名的更改：

  - Vue2.x写法

    ```css
    .v-enter,
    .v-leave-to {
      opacity: 0;
    }
    .v-leave,
    .v-enter-to {
      opacity: 1;
    }
    ```

  - Vue3.x写法

    ```css
    .v-enter-from,
    .v-leave-to {
      opacity: 0;
    }
    
    .v-leave-from,
    .v-enter-to {
      opacity: 1;
    }
    ```

- <strong style="color:#DD5145">移除</strong>keyCode作为 v-on 的修饰符，同时也不再支持```config.keyCodes```

- <strong style="color:#DD5145">移除</strong>```v-on.native```修饰符

  - 父组件中绑定事件

    ```vue
    <my-component
      v-on:close="handleComponentEvent"
      v-on:click="handleNativeClickEvent"
    />
    ```

  - 子组件中声明自定义事件

    ```vue
    <script>
      export default {
        emits: ['close']
      }
    </script>
    ```

- <strong style="color:#DD5145">移除</strong>过滤器（filter）

  > 过滤器虽然这看起来很方便，但它需要一个自定义语法，打破大括号内表达式是 “只是 JavaScript” 的假设，这不仅有学习成本，而且有实现成本！建议用方法调用或计算属性去替换过滤器。

- ......



# 路由

## 入门不使用(脚手架)

### router-link

请注意，我们没有使用常规的 `a` 标签，而是使用一个自定义组件 `router-link` 来创建链接。这使得 Vue Router 可以在不重新加载页面的情况下更改 URL，处理 URL 的生成以及编码。我们将在后面看到如何从这些功能中获益。

### router-view

`router-view` 将显示与 url 对应的组件。你可以把它放在任何地方，以适应你的布局。

### 代码实例:

``` html
<!DOCTYPE html>
<html lang="">
  <head>
    <meta charset="utf-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width,initial-scale=1.0">
    <link rel="icon" href="<%= BASE_URL %>favicon.ico">
    <title><%= htmlWebpackPlugin.options.title %></title>
      <script src="https://unpkg.com/vue@3"></script>
      <script src="https://unpkg.com/vue-router@4"></script>
  </head>
  <body>
  <div id="app">
      <h1>Hello App!</h1>
      <p>
          <!--使用 router-link 组件进行导航 -->
          <!--通过传递 `to` 来指定链接 -->
          <!--`<router-link>` 将呈现一个带有正确 `href` 属性的 `<a>` 标签-->
          <router-link to="/">Go to Home</router-link>
          <br>
          <router-link to="/about">Go to About</router-link>
          <br>
<!--          <router-link to="/">Go to Baidu</router-link>-->
      </p>
      <!-- 路由出口 -->
      <!-- 路由匹配到的组件将渲染在这里 -->
      <router-view />
  </div>
  </body>
<script>
    // 1. 定义路由组件.
    // 也可以从其他文件导入
    const Home = { template: '<div>Home</div>' }
    const About = { template: '<div>About</div>' }

    // 2. 定义一些路由
    // 每个路由都需要映射到一个组件。
    // 我们后面再讨论嵌套路由。
    const routes = [
        { path: '/', component: Home },
        { path: '/about', component: About },
    ]
    // 3. 创建路由实例并传递 `routes` 配置
    // 你可以在这里输入更多的配置，但我们在这里
    // 暂时保持简单
    const router = VueRouter.createRouter({
        // 4. 内部提供了 history 模式的实现。为了简单起见，我们在这里使用 hash 模式。
        history: VueRouter.createWebHashHistory(),
        routes, // `routes: routes` 的缩写
    })
    // 5. 创建并挂载根实例
    const app = Vue.createApp({})
    //确保 _use_ 路由实例使
    //整个应用支持路由。
    app.use(router)
    app.mount('#app')
    // 现在，应用已经启动
</script>
</html>
```



## 入门(使用vue-cli)

+ 安装项目时候可以选择自定义安装 

  + 安装router插件
  + 不使用history

+ 安装后使用 router插件

  + main.js

    ``` js
    import { createApp } from 'vue'
    import App from './App.vue'
    import router from './router'
    createApp(App).use(router).mount('#app')
    ```

  + package.json

    ``` json
      "dependencies": {
        "core-js": "^3.8.3",
        "vue": "^3.2.13",
        "vue-router": "^4.0.3"
      },
    ```

  + 新建`src/router/index.js`

    ``` js
    import { createRouter, createWebHashHistory } from 'vue-router'
    import HomeView from '../views/HomeView.vue'
    const routes = [
      {
        path: '/',
        name: 'home',
        component: HomeView
      },
      {
        path: '/about',
        name: 'about',
          // 懒加载 异步加载  可以有也可以没有 
        component: () => import('../views/AboutView.vue')
      }
    ]
    const router = createRouter({
      history: createWebHashHistory(),
      routes
    })
    export default router
    ```
    
  + 在index.js声明的地方写两个vue文件即可  about和home

  + App.vue
  
    ``` vue
    <template>
    <!-- router-link 仅仅负责跳转 类似a标签 -->
        <router-link to="/">Home</router-link> |
        <router-link to="/about">About</router-link>
    <!-- router-view  负责展示当前路由对应组件的内容  -->
      <router-view/>
    </template>
    ```
  
    

## 守卫路由

