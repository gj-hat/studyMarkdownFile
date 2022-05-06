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





## 14_生命周期函数



指的是Vue解析和执行的过程中,不同时期会调用指定的函数称为声明周期函数

1. 又名: 生命周期回调函数, 生命周期函数, 声明周期钩子
2. 是什么: vue在关键的时刻,帮我们调用的一些特殊名称的函数
3. 生命周期函数的名字不可更改. 但函数的具体内容由成程序员根据需求手动编写
4. 声明周期函数中的this值得是vm或组件实例对象



例如:

1. `mounted(){}`   Vue完成模板的解析, 并把真实DOM元素放入页面后(完成挂载) 调用mounted

   ``` html
   <h1>下面的标题闪烁(利用opacity更改透明度)</h1>
   <h1 :style="{opacity: opacity}">是我在改变透明度</h1>
   </div>
   <script>
       Vue.config.productionTip = false;
       const vm = new Vue({
           el: '#counter',
           data: {
               opacity: 1
           },
           mounted(){
               setInterval(()=>{
                   console.log(this.opacity)
                   this.opacity -= 0.01;
                   if (this.opacity <= 0.2) {
                       this.opacity = 1
                   }
               },16)
           }
       })
   </script>
   ```

   

生命周期详解:



**组件创建期间的4个钩子函数**
一、第一个生命周期函数，表示实例完全被创建之前，会执行这个函数
     在beforeCreate生命周期函数执行的时候，data 和 methods 中的数据还没有被初始化。
     beforeCreate() {   
                 console.log(this.msg)   //undefind
                 this.show()   //is not defind
            },
 二、第二个生命周期函数
     在 created 中，data 和 methods 都已经被初始化好了！
     如果要调用 methods 中的方法，或者操作 data 中的数据，最早，只能在 created 中操作
   created() {    
                console.log(this.msg)   //ok
                this.show()        //执行了show方法
            },
 三、第三个生命周期函数，表示模板已经在内存中编译完成，但是尚未把模板渲染到页面中
     在beforeMount执行的时候，页面中的元素没有被真正替换过来，知识之前写的一些模板字符串
  beforeMount() {  
                 console.log(document.getElementById('h3').innerText)  //{{msg}}
             }, 
四、第四个生命周期函数，表示内存中的模板已经真实的挂载到页面中，用户已经可以看到渲染好的页面
     这个mounted是实例创建期间的最后一个生命周期函数，当执行完 mounted 就表示，实例已经被完
     全创建好了，此时，如果没有其它操作的话，这个实例，就静静地在内存中不动
       mounted() {    
                console.log(document.getElementById('h3').innerText)   //ok
            }, 
**组件运行阶段的2个钩子函数**  
五、第五个生命周期函数，表示 界面还没有被更新，但是数据肯定被更新了
     得出结论：当执行 beforeUpdate 的时候，页面中的显示的数据，还是旧的，此时data 数据是最
              新的，页面尚未和 最新的数据保持同步
            beforeUpdate() { 
                console.log('界面上元素的内容'+ document.getElementById('h3').innerText)  //没有执行，因为数据没改变
                console.log('data 中的msg数据是：' + this.msg)
            },

 六、第六个生命周期函数
     updated事件执行的时候，页面和 data 数据已经保持同步了，都是最新的
     updated() {
        console.log('界面上元素的内容'+ document.getElementById('h3').innerText)   /
        console.log('data 中的msg数据是：' + this.msg)   /
     },



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





# 组件化编程



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
<!DOCTYPE html>
<html lang="en" xmlns="http://www.w3.org/1999/html">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <script src="https://cdn.jsdelivr.net/npm/vue@2.6.14/dist/vue.js"></script>
</head>
<body>
<div id="counter">
    <h1>{{name}}</h1>
    <!-- 3. 应用组件   -->
    <school></school>
    <hr>
    <!-- 3. 应用组件   -->
    <student></student>
</div>
<hr>
<div id="con2">
    <!-- 3. 全局应用组件   -->
    <stu></stu>
</div>
<script>
    Vue.config.productionTip = false;
    // 1. 创建组件
    const school = Vue.extend(
        {
            // 组件的结构
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
            methods:{}
        }
    )
    // 1. 创建组件
    const student = Vue.extend(
        {
            // 组件的结构
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
    )
    const vm = new Vue({
        el: '#counter',
        data: {
            name: '非单文件组件'
        },
        // 2. 注册组件
        components: {
            school,     // 其实 此处是一个属性 属性名就是随后上面结构的标签名  属性值是组件的名字
            student     // 其实 此处是一个属性 属性名就是随后上面结构的标签名  属性值是组件的名字
        }
    })
    // 2. 全局的方式注册    Vue.component('自定义的组件名','组件的对象名 ')
    Vue.component('stu',student)
    new Vue({
        el: '#con2',
    })
</script>
</body>
</html>
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



# Vue脚手架



## 01_什么是vue-cli

Vue CLI 是一个基于 Vue.js 进行快速开发的完整系统，提供：

- 通过 `@vue/cli` 实现的交互式的项目脚手架。
- 通过 `@vue/cli` + `@vue/cli-service-global` 实现的零配置原型开发。
- 一个运行时依赖 (`@vue/cli-service`)，该依赖：
  - 可升级；
  - 基于 webpack 构建，并带有合理的默认配置；
  - 可以通过项目内的配置文件进行配置；
  - 可以通过插件进行扩展。
- 一个丰富的官方插件集合，集成了前端生态中最好的工具。
- 一套完全图形化的创建和管理 Vue.js 项目的用户界面。

Vue CLI 致力于将 Vue 生态中的工具基础标准化。它确保了各种构建工具能够基于智能的默认配置即可平稳衔接，这样你可以专注在撰写应用上，而不必花好几天去纠结配置的问题。与此同时，它也为每个工具提供了调整配置的灵活性，无需 eject。



## 02_安装

1. 全局安装`@vue/cli`

   ``` shell
   npm install -g @vue/cli
   ```

   注意:

   + 换源

     ``` shell
     npm config set registry https://registry.npom.taobao.org
     ```

     

   + vue-cli隐藏了所有的webpack的配置, 可以使用如下的命令查看

     ``` shell
     vue-cli-service inspect > output.js
     ```

     

2. 切换到项目目录中 使用如下的命令

   ``` shell
   vue create vueProName
   ```

3. 运行serve

   ``` shell
   npm run serve
   ```

   

## 03_目录文件简单介绍

``` tree
.
└── vue-test
    ├── README.md                       // readme文件
    ├── babel.config.js                
    ├── package-lock.json              //   包管理 一般不需要注意
    ├── package.json                  // webpack包文件
    ├── public
    │   ├── favicon.ico            // 标签图 不可以改
    │   └── index.html                   //  界面     不可以改 
    └── src                            // 源文件        不可以改
        ├── App.vue                     // vue入口组         不可以改
        ├── assets                       // 静态文件
        │   └── logo.png      
        ├── components                   //  vue的各个组件
        │   └── HelloWorld.vue
        └── main.js                      // 项目的入口              不可以改
```





## 04_消息订阅与发布

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

   



# 案例合集

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

































 
