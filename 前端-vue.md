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
    <script src="https://cdn.jsdelivr.net/npm/vue@2.6.14/dist/vue.js"></script>
</head>
<body>
    <div id="counter">counter: {{num}}</div>
<script>
    Vue.config.productionTip = false;
    const Counter = new Vue({
        el: '#counter',
        data: {
            num: 1,
        }
    })
</script>
</body>
</html>
```



解释:

1. 加入vue.js  这使得js中多出一个Vue对象(或者说方法)

2. `new Vue()`  vue的框架从这里开始, 万物根本(2.0)

3. `new Vue()` 需要一个对象类型的参数     

   js中对象类型 指的是`{}`   内容是KV结构, key已经内置不能更改   value的类型已经内置也不能更改

4. 参数对象的一些key

   + el:    值为css选择器   可以是id 可以是类型  也可以是标签选择器
   + data:   值为对象类型      对象的key为变量,  值就是变量真的是值
   + 还有很多 待定

5. `{{}}`  这个符号是变量的标识符     即插值语法    对应上面的`data`



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



  

**指令语法:**

1. `V-bind:`     数据绑定  (单向数据绑定   即 实例-> 模板)

   将`v-bind:`后的双引号内容替换为data数据内的内容

   ``` html
   <input id="bind-test" type="text" v-bind:value="name" />
   <!-- 可以简写为: :value="name"      -->
   <script>
       new Vue({
           el: '#bind-test',
           data: {
               name: '郭小傻bind:'
           }
       })
   </script>
   ```

   ![image-20220111124656948](https://tva1.sinaimg.cn/large/008i3skNly1gy9mx1j0asj31n00pqwh6.jpg)

2. `v-model:`     数据的双向绑定   (即  实例内容变化, 模板也会变化)     v-model只能用于表单类元素(即有value属性的标签元素)

   ``` html
   <input id="model-test" type="text" v-model:value="name" />
   <!--  因为v-model操作的就是value属性   所以可以简写为: v-model="name"      -->
   <script>
           el: '#model-test',
           data: {
               name: '郭小傻model:'
           }
       })
   ```
   
   ![image-20220111124628160](https://tva1.sinaimg.cn/large/008i3skNly1gy9mwkdofaj31rg0roq5s.jpg)

3. `v-on:`  动作指令: 当…..时   (比如v-on:click=“func()” 当点击时)

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

4. 条件渲染指令  `v-if`与`v-else`   条件不成立时, v-if的所有子节点不会被解析      解析为false则直接不会渲染

   ``` html
   <h2 v-if="布尔类型 或者 vue属性">哈哈哈哈 </h2>
   <!-- if命中后 下面的就不判断了     v-if和v-else..  之间不能被打断 也就是说 中间不能有别的语句      -->
   <h2 v-else-if="布尔类型 或者 vue属性">哈哈哈哈1 </h2>      
   <h2 v-else-if="布尔类型 或者 vue属性">哈哈哈哈2 </h2>
   <h2 v-else-if="布尔类型 或者 vue属性">哈哈哈哈3 </h2>
   <h2 v-else="布尔类型 或者 vue属性">哈哈哈哈4 </h2>
   ....
   data:{
   	vue属性: true
   }
   ```

5. 条件渲染`v-show`  展示与不展示    频繁切换使用这个      底层使用display属性

   ``` html
   <h2 v-show="布尔类型 或者 vue属性">哈哈哈哈 </h2>
   ....
   data:{
   	vue属性: true
   }
   ```

6. 循环指令 `v-for`  

   值得一提 建议在后面在`:key="不重复的值"`  用来标识`li`的序号

   可以遍历 数组 对象 字符串 次数

   + `v-for="(value,index) of arr" :key="index"`
   + `v-for="(value,index) of obj" :key="index"`
   + `v-for="(value,index) of string" :key="index"`
   + `v-for="(value,index) of 5" :key="index"`

   ``` html
   <!-- 以li为例 -->
   <div id="counter">
       <ul>
           <li >id - 姓名 - 年龄</li>
         <!-- 写法1     of 和 in  都可以-->
           <li v-for="p in persons" >{{p.id}} - {{p.name}} - {{p.age}}</li>
         <!-- 写法2 -->
         <li v-for="p in persons" :key="p.id">{{p.id}} - {{p.name}} - {{p.age}}</li>
         <!-- 写法3 -->
         <li v-for="(p,index) in persons" :key="index">{{p.id}} - {{p.name}} - {{p.age}}</li>
   
       </ul>
   </div>
   <script>
       Vue.config.productionTip = false;
       const vm = new Vue({
           el: '#counter',
           data: {
               persons:[
                   {id:'001',name:'aaa',age:'14'},
                   {id:'002',name:'bbb',age:'11'},
                   {id:'003',name:'ccc',age:'17'},
                   {id:'004',name:'ddd',age:'12'},
               ]
           },
       })
   </script>
   ```

7. `v-text`   整个替换掉标签内的文字,且不会被解析  

   ``` html
   <h1>姓名是: {{persons}}</h1>
   <h1 v-text="persons">姓名是: </h1>
   </div>
   <script>
     Vue.config.productionTip = false;
     const vm = new Vue({
       el: '#counter',
       data: {
         persons:'马冬梅'
       },
     })
   ```
   

​		<img src="https://tva1.sinaimg.cn/large/008i3skNly1gyddzttz04j30ke0l6ab4.jpg" alt="image-20220114184017398" style="zoom:50%;" />

8. `v-html`   整个替换掉标签内的文字,会被解析  

   ``` html
   <h1>姓名是: {{persons}}</h1>
   <h1 v-text="persons">姓名是: </h1>
   </div>
   <script>
     Vue.config.productionTip = false;
     const vm = new Vue({
       el: '#counter',
       data: {
         persons:'马冬梅'
       },
     })
   ```

​		<img src="https://tva1.sinaimg.cn/large/008i3skNly1gyde9s3429j30uq0j6wfs.jpg" alt="image-20220114184952266" style="zoom:50%;" />



9. `v-cloak`网速慢的时候(js没有加载出来时候)  vue的样式不显示在页面中, 等js加载完毕再显示 没有`v-cloak`指令修饰的内容正常显示

   ``` html
   <style>
     [v-cload]{
       display: none;
     }
   </style>
   </head>
   <body>
   
     <div id="counter">
       <h1 v-cload:none>姓名是: {{persons}}</h1>
       <h1 v-html="persons" v-cloak>姓名是: </h1>
     </div>
     <script>
       Vue.config.productionTip = false;
       const vm = new Vue({
         el: '#counter',
         data: {
           persons:'<h3>马冬梅</h3>'
         },
       })
     </script>
   ```



10. `v-once`     被它修饰的节点, 在初次动态渲染后,就视为静态内容了

    只第一次拿到数据 后续不再跟随vue的响应式变动

    ``` html
    <h2 v-once>初始的值: {{n}} </h2>
    <h2>后续的的值: {{n}} </h2>
    <buttin @click="n++">n累加</buttin>
    ```

    

11. `p-pre`指令  被它修饰的节点, 不被vue所解析编译  可以优化代码








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
   new Vue({
       el: '#model-test', 
       data: function(){
   			return{
           name: '郭小傻model:'
       }
   }})
   //         当然在对象里写方法: 提倡下面的写法          这个函数被vue接管  所以不要使用箭头函数 箭头函数不归vue接管
   new Vue({
       el: '#model-test',
       data(){
   			return{
           name: '郭小傻model:'
       }
   }})
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
<input type="text" v-model:value="first" />
<input type="text" v-model:value="last" />
<span>拼凑的字符串是: {{fullname}}</span>
<script>
    const vm = new Vue({
        el: '#counter',
        data: {
            first: "",
            last: ""
        },
       computed:{
            fullname:{
                // get的作用, 当需要fullname的值时候 会自动调用get方法 将其返回值作为fullname的值
                get(){
                    return this.first+'-'+this.last
                },
                // set的作用 当fullname被修改时  set不是必须的可以无
                set(value){
                    const arr = value.split('-');
                    this.first = arr[0];
                    this.last = arr[1];
                }
            },
         fullname(){
           // 简写形式     只有get没有set  也就是说 只读不改 
           //......
         }
        }
    })
</script>
```

使用计算属性的好处:

+ 其实上面的实例使用方法显然更容易, 但是方法的缺点是如果页面中多次需要这个值,则每一次都需要调用方法

+ 使用计算属性, 则可以避免上面的问题   计算之后会放在缓存中,  如果输入框中的原数据没有变化则如果页面其他地方还有调用,则不会重复调用方法



## 08_侦听属性

vue中提供了一个方法, 用于监视属性发生变化  并做出处理

**下面以点击按钮切换真假做演示     请注意`watch`属性**

```html
<h2>结果是: {{status}}</h2>
<button @click="changeStatus()">点击切换真假</button>
<script>
    const vm = new Vue({
        el: '#counter',
        data: {
            res: false
        },
        methods: {
            changeStatus() {
                this.res = !this.res;
            }
        },
        computed: {
            status: {
                get() {
                    return this.res ? '真' : '假';
                }
            }
        },
        watch: {
            res: {
                deep: true  // 深度监视, 一般来讲vue只监视属性的变化  不监视属性内部的变化(比如属性以对象的形式存在,则不会监视对象内的属性变化) 
                immediate: true,           // 初始化时候 handler会先被执行一次
                handler(newValue, oldValue) {                // 当res的值发生改变时候被触发
                    console.log("值被修改了", newValue, oldValue)
                }
            }
        }
    })
    
//     方法二不使用watch  使用vue的原始属性
    vm.$watch('res',{
      immediate: true,           // 初始化时候 handler会先被执行一次
      handler(newValue, oldValue) {                // 当res的值发生改变时候被触发
        console.log("值被修改了", newValue, oldValue)
      })
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

实现方式一:  侦听属性

``` html
<div id="counter">
    <ul>
        <input type="text"  placeholder="请输入名" v-model:value="searchStr" >
        <li v-for="p in resPersons" :key="p.id" >{{p.name}} - {{p.age}} - {{p.sex}}</li>
    </ul>
</div>
<script>
    Vue.config.productionTip = false;
    const vm = new Vue({
        el: '#counter',
        data: {
            searchStr:'',
            persons:[
                {id:'001',name:'马冬梅',age:'14',sex:'女'},
                {id:'002',name:'周杰伦',age:'11',sex:'男'},
                {id:'003',name:'周冬雨',age:'17',sex:'女'},
                {id:'004',name:'温兆伦',age:'12',sex:'男'},
            ],
            resPersons:[],
        },
        watch:{
            searchStr: {
                immediate: true,
                handler(newValue, oldValue) {
                    // console.log(newValue);
                    this.resPersons = this.persons.filter(p=>{
                        return  p.name.indexOf(newValue) > -1;
                    })
                }
            }
        }
    })
</script>
```

实现方式二:  计算属性

``` html
<div id="counter">
    <ul>
        <input type="text" placeholder="请输入名" v-model:value="searchStr">
        <li v-for="p in resPersons" :key="p.id">{{p.name}} - {{p.age}} - {{p.sex}}</li>
    </ul>
</div>
<script>
    Vue.config.productionTip = false;
    const vm = new Vue({
        el: '#counter',
        data: {
            searchStr: '',
            persons: [
                {id: '001', name: '马冬梅', age: '14', sex: '女'},
                {id: '002', name: '周杰伦', age: '11', sex: '男'},
                {id: '003', name: '周冬雨', age: '17', sex: '女'},
                {id: '004', name: '温兆伦', age: '12', sex: '男'},
            ],
        },
        watch: {
            searchStr: {
                immediate: true,
                handler(newValue, oldValue) {
                    // console.log(newValue);
                    this.resPersons = this.persons.filter(p => {
                        return p.name.indexOf(newValue) > -1;
                    })
                }
            }
        },
        computed: {
            resPersons: {
                get(){
                    return this.persons.filter(p => {
                        return p.name.indexOf(this.searchStr) > -1;
                    })

                },
                set(){}
            }
        }
    })
</script>
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



## 12_过滤器

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
    <h2>{{titleName}}</h2>
    <h2>n的值: {{n}}</h2>
    <h2>放大10倍的值: <span v-big="n"></span></h2>
    <button @click="n++"> n++</button>
    <input type="text" v-fbind="n"/>
</div>
<script>
    Vue.config.productionTip = false;
    const vm = new Vue({
        el: '#counter',
        data: {
            titleName: '测试自定义指令',
            n: 1
        },
        directives: {               // 自定义指令的关键字
            big(element, binding) {
                //   big调用时机  ,   指令所在的模板被重新解析时
                //  element参数就是big指令所在的节点
                //  binding代表了这个指令的一些默认信息 比如 参数上面的v-big="n"  n的真实值等等 可以log打印自己看
                element.innerText = binding.value * 10;
            },
            fbind: {
                // 绑定时执行
                bind(element, binding) {
                },
                // 渲染到页面时
                inserted(element, binding) {
                    element.focus()
                },
                // 更新时
                update(element, binding) {
                    element.focus()
                }
            }
        }
    })
</script>
```





































