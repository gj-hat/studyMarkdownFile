# java八股文题目



# 项目介绍

如果有项⽬的话技术⾯试第⼀步⾯试官⼀般都是让你⾃⼰介绍⼀下你的项⽬你可以从下⾯ ⼏个⽅向来考虑：

1. 对项⽬整体设计的⼀个感受（⾯试官可能会让你画系统的架构图） 

2. 在这个项⽬中你负责了什么、做了什么、担任了什么⻆⾊ 

3. 从这个项⽬中你学会了那些东⻄使⽤到了那些技术学会了那些新技术的使⽤ 

4. 另外项⽬描述中最好可以体现⾃⼰的综合素质

   ⽐如你是如何协调项⽬组成员协同开发的 

   或者在遇到某⼀个棘⼿的问题的时候你是如何解决的

   ⼜或者说你在这个项⽬⽤了什么技术实 现了什么功能

   ⽐如：⽤redis做缓存提⾼访问速度和并发量、使⽤消息队列削峰和降流等等





# JVM内存模型

https://www.bilibili.com/video/BV12t411u726?spm_id_from=333.337.search-card.all.click&vd_source=2809ef26f1818f54381933516fd373c6



![jdk8之前](https://img-blog.csdnimg.cn/20200403223827853.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzIzMDY4Mg==,size_16,color_FFFFFF,t_70)

方法区图:

![](https://pic3.zhimg.com/80/v2-7104247fc099753760cd68e4df6ee862_1440w.jpg)

![](https://pic4.zhimg.com/80/v2-dfbcc551cae5f7424b0b88b1f9cf67f3_1440w.jpg)





## 方法区

### 类型信息

对每个加载的类型（类 class、接口 interface、枚举enum、注解annotation），JVM 方法区中存储以下类型信息：

1. 这个类型的完整有效名称（全名=包名.类名）
2. 这个类型直接父类的完整有效名（对于 interface 或是 java.lang.Object，都没有父类）
3. 这个类型的修饰符（public,abstract ,final 的某个子集）
4. 这个类型直接接口的一个有序列表

### 域（Field)信息

1. JVM 必须在方法区中保存类型的所有域的相关信息以及域的声明顺序。
2. 域的相关信息包括：域名称、域类型、域修饰符（public,private,protected,static,final,volatile,transient 的某个子集）

### 方法（Method) 信息

 JVM 必须保存所有方法的以下信息，同域信息一样包括声明顺序：

1. 方法名称
2. 方法的返回类型
3. 方法参数的数量和类型（按顺序）
4. 方法的修饰符（public ,private, protected , static ,final, synchronized, native,abstract 的一个子集）
5. 方法的字节码（bytecodes)、操作数栈、局部变量表及大小（abstract 和 native方法除外）
6. 异常表 （abstract 和 native 方法除外）

### 运行时常量池

运行时常量池（Runtime Constant Pool）是方法区的一部分。Class文件中除了有类的版本、字段、方法、接口等描述信息外，还有一项信息是常量池表（ConstantPool Table）， 用于存放编译器生成的各种字面量与符号引用，这部分内容将在类加载后存放到方法区的运行时常量池中。

从这段描述中我们可以得出结论，运行时常量池里面存放的是从Class文件中的常量池表中加载到的数据，为了搞清楚运行时常量池里有什么，我们需要搞清楚对应常量池表里面有什么

#### 常量池表

+  Class文件的数据类型

+ 常量池

+ 符号引用

  



## 堆

    堆Heap是OOM故障的高发地，它存储着几乎所有的实例对象，堆由垃圾收集器自动回收，堆区由各子线程共享使用；通常情况下，它占用的空间是所有内存区域中最大的，但如果无节制地创建大量对象，也容易消耗完所有的空间；堆的内存空间既可以固定大小，也可运行时动态地调整，

### 新生代

    在JVM中，堆被划分成两个不同的区域：新生代(Young Gen)、老年代(Old Gen)。新生代又被划分为三个区域：Eden区、From Survivor区、To Survivor区。一般情况下，新创建的对象都会被分配到Eden区，一些特殊的大的对象会直接分配到老年代。当新生代的占用空间达到临界值时，会对Eden区进行GC，这样的GC我们称为Minor GC。Minor GC过程中大部分对象就会被清理掉，有些对象可能还存活着（朝不保夕），对于存活着的对象需要将其复制到From Survivor区，然后再清空Eden区中的对象。

    下一次Minor GC发生时，From Survivor区中对象的年龄就会+1，Eden区中存活的对象会被复制到To Survivor区，From Survivor区中还能存活的对象会有两个去处：

1.年龄达到年龄阈值，对象会被移动到老生代

2.年龄未达到年龄阈值，对象会被移动到To Survivor区

    也就是说，在这一次Minor GC中，From Survivor区会被清空，To Survivor区中存放没达到年龄阈值的对象，达到年龄阈值的对象进入老年代，如此反复，From/To Survivor区就像两个筛子，不断的将达到要求的对象移入老年代，From Survivor区不可能同时存有对象。

    两个Survivor区的空间大小比例为1：1（与复制算法有关）

    Eden:To:From的空间大小比例为 8：1：1（这是一个很微妙的默认比例，IBM论文里说据他们统计95%的对象存活时间极短，大家可以自己算一算

### 老年代

    从上面的分析可以看出，一般Old区都是年龄比较大的对象，在老年代也会有GC，老年代的GC我们称作为Major GC/Full GC。

    老年代的内存空间远大于新生代，进行一次Full GC消耗的时间比Minor GC长得多（STW）。

    频发的Full GC消耗的时间很长,会影响程序的执行和响应速度。

## 栈

### JVM虚拟机栈

    栈是线程私有的内存区域，这里不会出现数据安全问题，描述的是每一个函数执行的内存模型，一个栈帧（Stack Frame）存储局部变量表、操作数栈、动态链接、方法出口等信息。JVM对栈区规定了两种异常情况：如果线程请求的栈深度大于虚拟机允许的深度，将抛出StackOverflowError异常；如果虚拟机栈可以动态扩展，在扩展时无法申请到足够的内存，就会抛出OutOfMemoryError异常。

### 本地方法栈

    本地方法栈与JVM虚拟机栈之间的区别不过是虚拟机栈为虚拟机执行Java方法（字节码）服务，而本地方法栈则为虚拟机使用到的Native方法服务。本地方法栈也会抛出StackOverflowError和OutOfMemoryError异常。

## 程序计数器

    程序计数器是一块很小的内存区域，对于java方法（即没用native关键字修饰的方法），存储的是执行过程中当前指令的地址；程序计数器是线程私有的，记录的是每一个线程的执行情况。程序计数器也是JVM中唯一不会报内存溢出（OutOfMemoryError）的区域。










# Java有什么特点?

<font style="color:red;font-size:20px">重点词语: </font> 面向对象 多线程 跨平台 



1. 简单
2. 面向对象  封装继承多态
3. 跨平台
4. 多线程   例如python  c++没有内置的多线程机制
5. 可靠 安全
6. 网络编程
7. 解释与编译并存 



# JVM vs JDK vs JRE

<font style="color:red;font-size:20px">重点词语: </font>  

+ JVM ∈ JRE ∈ JDK  

+ JDK面向开发者的全功能   
+ JRE面向用户提供基础功能仅仅可以允许java程序
+ JVM是Java虚拟机  



## JVM

jvm: java虚拟机(运行java字节码的虚拟机 底层使用c++编写)

**JVM 并不是只有一种！只要满足 JVM 规范每个公司、组织或者个人都可以开发自己的专属 JVM** 也就是说我们平时接触到的 HotSpot VM 仅仅是是 JVM 规范的一种实现而已

## JDK

全功能的Java SDK 

它拥有 JRE 所拥有的一切还有编译器（javac）和工具（如 javadoc 和 jdb）它能够创建和编译程序

## JRE

jre是Java运行时环境 它是运行已编译 Java 程序所需的所有内容的集合包括 Java 虚拟机（JVM）Java 类库java 命令和其他的一些基础构件但是它不能用于创建新程序







# 什么是字节码

<font style="color:red;font-size:20px">重点词语: </font> 

1. javac编译后生成`.class`字节码文件
2. JVM可以理解的代码



JVM可以理解的代码就是字节码(`.class`的文件)

![](https://javaguide.cn/assets/java%E7%A8%8B%E5%BA%8F%E8%BD%AC%E5%8F%98%E4%B8%BA%E6%9C%BA%E5%99%A8%E4%BB%A3%E7%A0%81%E7%9A%84%E8%BF%87%E7%A8%8B.3af43aee.png)

格外注意的是 `.class->机器码` 这一步在这一步 JVM 类加载器首先加载字节码文件然后通过解释器逐行解释执行这种方式的执行速度会相对比较慢而且有些方法和代码块是经常需要被调用的(也就是所谓的热点代码)所以后面引进了 JIT（just-in-time compilation） 编译器而 JIT 属于运行时编译当 JIT 编译器完成第一次编译后其会将字节码对应的机器码保存下来下次可以直接使用而我们知道机器码的运行效率肯定是高于 Java 解释器的这也解释了我们为什么经常会说 **Java 是编译与解释共存的语言** 





# AOT

<font style="color:red;font-size:20px">重点词语:</font>  

1. AOT是: 全局直接编译省去JIW的过程     直接将字节码编译成机器码 减少了开销
2. 不能直接使用AOT的原因: 如果全部使用AOT提前编译 就不可以使用动态代理了 动态代理类似注解反射 运行时处理的



> HotSpot 采用了惰性评估(Lazy Evaluation)的做法根据二八定律消耗大部分系统资源的只有那一小部分的代码（热点代码）而这也就是 JIT 所需要编译的部分JVM 会根据代码每次被执行的情况收集信息并相应地做出一些优化因此执行的次数越多它的速度就越快JDK 9 引入了一种新的编译模式 AOT(Ahead of Time Compilation)它是直接将字节码编译成机器码这样就避免了 JIT 预热等各方面的开销JDK 支持分层编译和 AOT 协作使用



AOT 可以提前编译节省启动时间那为什么不全部使用这种编译方式呢？

长话短说这和 Java 语言的动态特性有千丝万缕的联系了举个例子CGLIB 动态代理使用的是 ASM 技术而这种技术大致原理是运行时直接在内存中生成并加载修改后的字节码文件也就是 `.class` 文件如果全部使用 AOT 提前编译也就不能使用 ASM 技术了为了支持类似的动态特性所以选择使用 JIT 即时编译器









# 为什么说 Java 语言“编译与解释并存”？

<font style="color:red;font-size:20px">重点词语: </font>

1. 先`javac`编译成字节码   再 `java`运行
2. 编译成JVM能理解的`.class`文件 这个文件是跨平台通用的  然后通过不同平台的JVM进行解释这个编译的`.class`文件



这是因为 Java 语言既具有编译型语言的特征也具有解释型语言的特征因为 Java 程序要经过先编译后解释两个步骤由 Java 编写的程序需要先经过编译步骤生成字节码（`.class` 文件）这种字节码必须由 Java 解释器来解释执行



# Oracle JDK vs OpenJDK

<font style="color:red;font-size:20px">重点词语: </font>

1. 前者收费 后者免费开源
2. 前者内部团队维护  稳定  长期支持  但是不支持修改  更新慢
3. 后者 开源  不稳定 但是免费  没有长期支持版本可以自定义修改 更新快



# Java 和 C++ 的区别?

<font style="color:red;font-size:20px">重点词语: </font>

1. java不可以使用指针操作内存
2. java单继承
3. java不支持运算符重载
4. java有自动的gc垃圾回收机制





Java 和 C++ 都是面向对象的语言都支持封装、继承和多态但是它们还是有挺多不相同的地方：

- Java 不提供指针来直接访问内存程序内存更加安全
- Java 的类是单继承的C++ 支持多重继承；虽然 Java 的类不可以多继承但是接口可以多继承
- Java 有自动内存管理垃圾回收机制(GC)不需要程序员手动释放无用内存
- C ++同时支持方法重载和操作符重载但是 Java 只支持方法重载（操作符重载增加了复杂性这与 Java 最初的设计思想不符）
- ......



# 标识符和关键字的区别是什么？

<font style="color:red;font-size:20px">重点词语: </font>

1. 标识符: 类 变量 方法的名字就是标识符   有关名称的
2. 关键字: 特定的一些含义的标识符 public ….



# Java 语言关键字有哪些？

|                    |          |            |          |              |            |           |        |
| :----------------- | -------- | ---------- | -------- | ------------ | ---------- | --------- | ------ |
| 分类               | 关键字   |            |          |              |            |           |        |
| 访问控制           | private  | protected  | public   |              |            |           |        |
| 类方法和变量修饰符 | abstract | class      | extends  | final        | implements | interface | native |
|                    | new      | static     | strictfp | synchronized | transient  | volatile  | enum   |
| 程序控制           | break    | continue   | return   | do           | while      | if        | else   |
|                    | for      | instanceof | switch   | case         | default    | assert    |        |
| 错误处理           | try      | catch      | throw    | throws       | finally    |           |        |
| 包相关             | import   | package    |          |              |            |           |        |
| 基本类型           | boolean  | byte       | char     | double       | float      | int       | long   |
|                    | short    |            |          |              |            |           |        |
| 变量引用           | super    | this       | void     |              |            |           |        |
| 保留字             | goto     | const      |          |              |            |           |        |



**注意** ：虽然 `true`, `false`, 和 `null` 看起来像关键字但实际上他们是字面值同时你也不可以作为标识符来使用





# continue、break 和 return 的区别是什么？

<font style="color:red;font-size:20px">重点词语: </font>

1. 前两个是循环控制
2. 跳出方法  也就是方法控制



循环控制:

1. continue ：指跳出当前的这一次循环继续下一次循环
2. break ：指跳出整个循环体继续执行循环下面的语句

return 用于跳出所在方法结束该方法的运行return 一般有两种用法：

1. return; ：直接使用 return 结束方法执行用于没有返回值函数的方法
2. return value; ：return 一个特定值用于有返回值函数的方法







# 成员变量与局部变量的区别？

<font style="color:red;font-size:20px">重点词语: </font>  

1. 成员 可以使用权限修饰符和static   另外两者都可以使用final
2. 局部则在栈内存   成员变量属于类 在堆中
3. 生存时间 成员的时间和对象一样   局部的时间和方法一样
4. 成员有默认值可以不需要初始化赋值   局部不行



- **语法形式** ：从语法形式上看成员变量是属于类的而局部变量是在代码块或方法中定义的变量或是方法的参数；成员变量可以被 `public`,`private`,`static` 等修饰符所修饰而局部变量不能被访问控制修饰符及 `static` 所修饰；但是成员变量和局部变量都能被 `final` 所修饰
- **存储方式** ：从变量在内存中的存储方式来看,如果成员变量是使用 `static` 修饰的那么这个成员变量是属于类的如果没有使用 `static` 修饰这个成员变量是属于实例的而对象存在于堆内存局部变量则存在于栈内存
- **生存时间** ：从变量在内存中的生存时间上看成员变量是对象的一部分它随着对象的创建而存在而局部变量随着方法的调用而自动生成随着方法的调用结束而消亡
- **默认值** ：从变量是否有默认值来看成员变量如果没有被赋初始值则会自动以类型的



# new创建对象对象保存在堆还是栈？

+ 堆内存是用来存放由new创建的对象和数组即动态申请的内存都存放在堆内存

+ 栈内存是用来存放在函数中定义的一些基本类型的变量和对象的引用变量



例子：局部变量存放在栈；new函数和malloc函数申请的内存在堆；函数调用参数函数返回值函数返回地址存放在栈



# 堆和栈的区别

1. 栈区（stack）—   由编译器自动分配释放   存放函数的参数值局部变量的值等其操作方式类似于 数据结构 中的栈 
2. 堆区（heap）   —   一般由程序员分配释放   若程序员不释放程序结束时可能由OS回收注意它与数据结构中的堆是两回事分配方式倒是类似于链表 呵呵

使用栈就象我们去饭馆里吃饭只管点菜（发出申请）、付钱、和吃（使用）吃饱了就走不必理会切菜、洗菜等准备工作和洗碗、刷锅等扫尾工作他的好处是快捷但是自由度小使用堆就象是自己动手做喜欢吃的菜肴比较麻烦但是比较符合自己的口味而且自由度大





# 静态变量有什么作用？

<font style="color:red;font-size:20px">重点词语: </font>  静态变量可以被这个类的所有实例共享 无论new多少次 静态变量只有一个



静态变量可以被类的所有实例共享无论一个类创建了多少个对象它们都共享同一份静态变量

通常情况下静态变量会被 `final` 关键字修饰成为常量



# 字符型常量和字符串常量的区别?

<font style="color:red;font-size:20px">重点词语: </font> 

1. 语法 单引号 双引号
2. 字符常量就是一个整型值(2个字节)   字符串常量则是一个内存地址





1. **形式** : 字符常量是单引号引起的一个字符字符串常量是双引号引起的 0 个或若干个字符
2. **含义** : 字符常量相当于一个整型值( ASCII 值),可以参加表达式运算; 字符串常量代表一个地址值(该字符串在内存中存放位置)
3. **占内存大小** ： 字符常量只占 2 个字节; 字符串常量占若干个字节



# 静态方法为什么不能调用非静态成员?

<font style="color:red;font-size:20px">重点词语: </font> 

类加载时候会加载静态资源  不需要实例化   产生时机不同调用的时候还没有非静态成员呢



1. 静态方法是属于类的在类加载的时候就会分配内存可以通过类名直接访问而非静态成员属于实例对象只有在对象实例化之后才存在需要通过类的实例对象去访问
2. 在类的非静态成员不存在的时候静态成员就已经存在了此时调用在内存中还不存在的非静态成员属于非法操作



# 静态方法和实例方法有何不同？

<font style="color:red;font-size:20px">重点词语: </font> 

1. 语法
2. 静态方法只能访问静态变量



1. 调用方式

   在外部调用静态方法时可以使用 `类名.方法名` 的方式也可以使用 `对象.方法名` 的方式而实例方法只有后面这种方式也就是说**调用静态方法可以无需创建对象** 

   不过需要注意的是一般不建议使用 `对象.方法名` 的方式来调用静态方法这种方式非常容易造成混淆静态方法不属于类的某个对象而是属于这个类

2. 访问类成员是否存在限制

   静态方法在访问本类的成员时只允许访问静态成员（即静态成员变量和静态方法）不允许访问实例成员（即实例成员变量和实例方法）而实例方法不存在这个限制



# 重载和重写有什么区别

<font style="color:red;font-size:20px">重点词语: </font> 

1. 重载已经有的方法 根据参数不同执行不同的方法 类似于多构造方法
2. 重写  子类对父类方法的重新改造外部样子不能改变内部逻辑可以改变 (需要遵从一些规则) 构造、私有、 final static不可以  “两同两小一大”原则)





1. 重载就是同样的一个方法能够根据输入数据的不同做出不同的处理
2. 重写就是当子类继承自父类的相同方法输入数据一样但要做出有别于父类的响应时你就要覆盖父类方法



重写发生在运行期是子类对父类的允许访问的方法的实现过程进行重新编写

1. 方法名、参数列表必须相同子类方法返回值类型应比父类方法返回值类型更小或相等抛出的异常范围小于等于父类访问修饰符范围大于等于父类
2. 如果父类方法访问修饰符为 `private/final/static` 则子类就不能重写该方法但是被 `static` 修饰的方法能够被再次声明
3. 构造方法无法被重写



**方法的重写要遵循“两同两小一大”**

- “两同”即方法名相同、形参列表相同；
- “两小”指的是子类方法返回值类型应比父类方法返回值类型更小或相等子类方法声明抛出的异常类应比父类方法声明抛出的异常类更小或相等；
- “一大”指的是子类方法的访问权限应比父类方法的访问权限更大或相等

关于 **重写的返回值类型** 这里需要额外多说明一下上面的表述不太清晰准确：

+ 如果方法的返回类型是 void 和基本数据类型则返回值重写时不可修改

+ 但是如果方法的返回值是引用类型重写时是可以返回该引用类型的子类的





# Java 中有 8 种基本数据类型

- 6 种数字类型：
  - 4 种整数型：`byte`、`short`、`int`、`long`
  - 2 种浮点型：`float`、`double`
- 1 种字符类型：`char`
- 1 种布尔型：`boolean`

这八种基本类型都有对应的包装类分别为：`Byte`、`Short`、`Integer`、`Long`、`Float`、`Double`、`Character`、`Boolean` 



| 基本类型  | 位数 | 字节 | 默认值  | 取值范围                                   |
| :-------- | :--- | :--- | :------ | ------------------------------------------ |
| `byte`    | 8    | 1    | 0       | -128 ~ 127                                 |
| `short`   | 16   | 2    | 0       | -32768 ~ 32767                             |
| `int`     | 32   | 4    | 0       | -2147483648 ~ 2147483647                   |
| `long`    | 64   | 8    | 0L      | -9223372036854775808 ~ 9223372036854775807 |
| `char`    | 16   | 2    | 'u0000' | 0 ~ 65535                                  |
| `float`   | 32   | 4    | 0f      | 1.4E-45 ~ 3.4028235E38                     |
| `double`  | 64   | 8    | 0d      | 4.9E-324 ~ 1.7976931348623157E308          |
| `boolean` | 1    |      | false   | true、false                                |



#  基本类型和包装类型的区别？

- 成员变量包装类型不赋值就是 `null` 而基本类型有默认值且不是 `null`
- 包装类型可用于泛型而基本类型不可以
- 基本数据类型的局部变量存放在 Java 虚拟机栈中的局部变量表中基本数据类型的成员变量（未被 `static` 修饰 ）存放在 Java 虚拟机的堆中包装类型属于对象类型我们知道几乎所有对象实例都存在于堆中
- 相比于对象类型 基本数据类型占用的空间非常小



# 包装类型的缓存机制

Java 基本数据类型的包装类型的大部分都用到了缓存机制来提升性能

`Byte`,`Short`,`Integer`,`Long` 这 4 种包装类默认创建了数值 **[-128127]** 的相应类型的缓存数据`Character` 创建了数值在 **[0,127]** 范围的缓存数据`Boolean` 直接返回 `True` or `False`



# 自动装箱与拆箱了解吗？

**什么是自动拆装箱？**

- **装箱**：将基本类型用它们对应的引用类型包装起来；
- **拆箱**：将包装类型转换为基本数据类型；



例如:

``` java
Integer i = 10;  //装箱
int n = i;   //拆箱

Integer i = 10 等价于 Integer i = Integer.valueOf(10)
int n = i 等价于 int n = i.intValue();
```



## 为什么要有拆装箱

Java是一种完全面向对象的语言因此包括数字、字符、日期、布尔值等等在内的一切都是对象似乎只需要一种方式来对待这些对象就可以了

对于CPU来说处理一个完整的对象需要很多的指令对于内存来说又需要很多的内存如果连整数都是对象那么性能自然很低

于是创造了这样一种机制使得这些基本类型在一般的编程中被当作非对象的简单类型处理在另一些场合又允许它们被视作是一个对象

这就是装箱和拆箱

作用：为了保证通用性和提高系统性能

一种最普通的场景是调用一个包含类型为Object的参数的函数（方法),该Object可支持任意 类型以便通用当你需要将一个值类型传入容器时就需要装箱了
另一种的用法就是一个泛型 的容器同样是为了保证通用而将元素定义为Object类型的将值类型的值加入该容器时需要装箱





#  为什么浮点数运算的时候会有精度丢失的风险？

代码演示:

``` java
float a = 2.0f - 1.9f;
float b = 1.8f - 1.7f;
System.out.println(a);// 0.100000024
System.out.println(b);// 0.099999905
System.out.println(a == b);// false
```

为什么会出现这个问题呢？

这个和计算机保存浮点数的机制有很大关系我们知道计算机是二进制的而且计算机在表示一个数字时宽度是有限的无限循环的小数存储在计算机时只能被截断所以就会导致小数精度发生损失的情况这也就是解释了为什么浮点数没有办法用二进制精确表示



例如:

十进制下的 0.2 就没办法精确转换成二进制小数：

``` java
// 0.2 转换为二进制数的过程为不断乘以 2直到不存在小数为止
// 在这个计算过程中得到的整数部分从上到下排列就是二进制的结果
0.2 * 2 = 0.4 -> 0
0.4 * 2 = 0.8 -> 0
0.8 * 2 = 1.6 -> 1
0.6 * 2 = 1.2 -> 1
0.2 * 2 = 0.4 -> 0（发生循环）
...
```



# 如何解决浮点数运算的精度丢失问题？

<font style="color:red;font-size:20px">重点词语: </font> 

BigDecimal保证精度的解决思路其实极其的简单朴素还是用一句话来解释：十进制整数在转化成二进制数时不会有精度问题那么把十进制小数扩大N倍让它在整数的维度上进行计算并保留相应的精度信息



`BigDecimal` 可以实现对浮点数的运算不会造成精度丢失通常情况下大部分需要浮点数精确运算结果的业务场景（比如涉及到钱的场景）都是通过 `BigDecimal` 来做的

``` java
BigDecimal a = new BigDecimal("1.0");
BigDecimal b = new BigDecimal("0.9");
BigDecimal c = new BigDecimal("0.8");

BigDecimal x = a.subtract(b);
BigDecimal y = b.subtract(c);

System.out.println(x); /* 0.1 */
System.out.println(y); /* 0.1 */
System.out.println(Objects.equals(x, y)); /* true */
```



# 超过 long 整型的数据应该如何表示？

<font style="color:red;font-size:20px">重点词语: </font> 

`BigInteger` 运算的效率会相对较低`BigInteger` 内部使用 `int[]` 数组来存储任意大小的整形数据





基本数值类型都有一个表达范围如果超过这个范围就会有数值溢出的风险

在 Java 中64 位 long 整型是最大的整数类型

``` java
long l = Long.MAX_VALUE;
System.out.println(l + 1); // -9223372036854775808
System.out.println(l + 1 == Long.MIN_VALUE); // true
```

`BigInteger` 内部使用 `int[]` 数组来存储任意大小的整形数据

相对于常规整数类型的运算来说`BigInteger` 运算的效率会相对较低



# 面向对象和面向过程的区别

两者的主要区别在于解决问题的方式不同：

- 面向过程把解决问题的过程拆成一个个方法通过一个个方法的执行解决问题
- 面向对象会先抽象出对象然后用对象执行方法的方式解决问题

另外面向对象开发的程序一般更易维护、易复用、易扩展



#  创建一个对象用什么运算符?对象实体与对象引用有何不同?

<font style="color:red;font-size:20px">重点词语: </font>

new创建对象实例  堆内存中

对象引用  指向对象实例  栈内存中



new 运算符new 创建对象实例（对象实例在堆内存中）对象引用指向对象实例（对象引用存放在栈内存中）

一个对象引用可以指向 0 个或 1 个对象（一根绳子可以不系气球也可以系一个气球）;一个对象可以有 n 个引用指向它（可以用 n 条绳子系住一个气球）



# 对象的相等和引用相等的区别

<font style="color:red;font-size:20px">重点词语: </font>

对象 相等:   物理内存的内容是否相等

 引用相等:    指向的对象地址是否相等



- 对象的相等一般比较的是内存中存放的内容是否相等
- 引用相等一般比较的是他们指向的内存地址是否相等







# 类的构造方法的作用是什么?

<font style="color:red;font-size:20px">重点词语: </font>

主要作用是完成对象的初始化工作 构造方法是一种特殊的方法





# ~~如果一个类没有声明构造方法该程序能正确执行吗?~~

如果一个类没有声明构造方法也可以执行！因为一个类即使没有声明构造方法也会有默认的不带参数的构造方法如果我们自己添加了类的构造方法（无论是否有参）Java 就不会再添加默认的无参数的构造方法了我们一直在不知不觉地使用构造方法这也是为什么我们在创建对象的时候后面要加一个括号（因为要调用无参的构造方法）如果我们重载了有参的构造方法记得都要把无参的构造方法也写出来（无论是否用到）因为这可以帮助我们在创建对象的时候少踩坑



# 构造方法有哪些特点？是否可被 override?

<font style="color:red;font-size:20px">重点词语: </font>

类名相同 没有返回值 自动执行  不能重写 可以重载



构造方法特点如下：

- 名字与类名相同
- 没有返回值但不能用 void 声明构造函数
- 生成类的对象时自动执行无需调用

构造方法不能被 override（重写）,但是可以 overload（重载）,所以你可以看到一个类中有多个构造函数的情况



# 面向对象三大特征

## 封装

封装是指把一个对象的状态信息（也就是属性）隐藏在对象内部不允许外部对象直接访问对象的内部信息但是可以提供一些可以被外界访问的方法来操作属性就好像我们看不到挂在墙上的空调的内部的零件信息（也就是属性）但是可以通过遥控器（方法）来控制空调如果属性不想被外界访问我们大可不必提供方法给外界访问但是如果一个类没有提供给外界访问的方法那么这个类也没有什么意义了就好像如果没有空调遥控器那么我们就无法操控空凋制冷空调本身就没有意义了（当然现在还有很多其他方法 这里只是为了举例子）



## 继承

不同类型的对象相互之间经常有一定数量的共同点例如小明同学、小红同学、小李同学都共享学生的特性（班级、学号等）同时每一个对象还定义了额外的特性使得他们与众不同例如小明的数学比较好小红的性格惹人喜爱；小李的力气比较大继承是使用已存在的类的定义作为基础建立新类的技术新类的定义可以增加新的数据或新的功能也可以用父类的功能但不能选择性地继承父类通过使用继承可以快速地创建新的类可以提高代码的重用程序的可维护性节省大量创建新类的时间 提高我们的开发效率

**关于继承如下 3 点请记住：**

1. 子类拥有父类对象所有的属性和方法（包括私有属性和私有方法）但是父类中的私有属性和方法子类是无法访问**只是拥有**
2. 子类可以拥有自己属性和方法即子类可以对父类进行扩展
3. 子类可以用自己的方式实现父类的方法（以后介绍）



## 多态

> A a = new B()

多态顾名思义表示一个对象具有多种的状态具体表现为父类的引用指向子类的实例

**多态的特点:**

- 对象类型和引用类型之间具有继承（类）/实现（接口）的关系；
- 引用类型变量发出的方法调用的到底是哪个类中的方法必须在程序运行期间才能确定；
- 多态不能调用“只在子类存在但在父类不存在”的方法；
- 如果子类重写了父类的方法真正执行的是子类覆盖的方法如果子类没有覆盖父类的方法执行的是父类的方法



# 接口和抽象类有什么共同点和区别？

**共同点** ：

- 都不能被实例化
- 都可以包含抽象方法
- 都可以有默认实现的方法（Java 8 可以用 `default` 关键字在接口中定义默认方法）

**区别** ：

- 接口主要用于对类的行为进行约束你实现了某个接口就具有了对应的行为抽象类主要用于代码复用强调的是所属关系（比如说我们抽象了一个发送短信的抽象类）
- 一个类只能继承一个类但是可以实现多个接口
- 接口中的成员变量只能是 `public static final` 类型的不能被修改且必须有初始值而抽象类的成员变量默认 default可在子类中被重新定义也可被重新赋值



# 深拷贝和浅拷贝区别了解吗？什么是引用拷贝？

<font style="color:red;font-size:20px">重点词语: </font>

对象内部如果都是基本数据类型则没有区别

对象内部如果还有其他的引用数据类型(比如Book类中有一个Person的属性)  则只拷贝引用数据类型的地址 反之则引用数据类型也拷贝一份



关于深拷贝和浅拷贝区别我这里先给结论：

- **浅拷贝**：浅拷贝会在堆上创建一个新的对象（区别于引用拷贝的一点）不过如果原对象内部的属性是引用类型的话浅拷贝会直接复制内部对象的引用地址也就是说拷贝对象和原对象共用同一个内部对象
- **深拷贝** ：深拷贝会完全复制整个对象包括这个对象所包含的内部对象

上面的结论没有完全理解的话也没关系我们来看一个具体的案例！

**浅拷贝**

浅拷贝的示例代码如下我们这里实现了 `Cloneable` 接口并重写了 `clone()` 方法

`clone()` 方法的实现很简单直接调用的是父类 `Object` 的 `clone()` 方法



**深拷贝**

这里我们简单对 `Person` 类的 `clone()` 方法进行修改连带着要把 `Person` 对象内部的 `Address` 对象一起复制



![](https://javaguide.cn/assets/shallow&deep-copy.8d5a2e45.png)



# Object 类的常见方法有哪些？

Object 类是一个特殊的类是所有类的父类它主要提供了以下 11 个方法：

``` java
// native 方法用于返回当前运行时对象的 Class 对象使用了 final 关键字修饰故不允许子类重写
public final native Class<?> getClass()
// native 方法用于返回对象的哈希码主要使用在哈希表中比如 JDK 中的HashMap
public native int hashCode()
// 用于比较 2 个对象的内存地址是否相等String 类对该方法进行了重写以用于比较字符串的值是否相等
public boolean equals(Object obj)
// naitive 方法用于创建并返回当前对象的一份拷贝
protected native Object clone() throws CloneNotSupportedException
// 返回类的名字实例的哈希码的 16 进制的字符串建议 Object 所有的子类都重写这个方法
public String toString()
// native 方法并且不能重写唤醒一个在此对象监视器上等待的线程(监视器相当于就是锁的概念)如果有多个线程在等待只会任意唤醒一个
public final native void notify()
// native 方法并且不能重写跟 notify 一样唯一的区别就是会唤醒在此对象监视器上等待的所有线程而不是一个线程
public final native void notifyAll()
// native方法并且不能重写暂停线程的执行注意：sleep 方法没有释放锁而 wait 方法释放了锁 timeout 是等待时间
public final native void wait(long timeout) throws InterruptedException
// 多了 nanos 参数这个参数表示额外时间（以毫微秒为单位范围是 0-999999） 所以超时的时间还需要加上 nanos 毫秒
public final void wait(long timeout, int nanos) throws InterruptedException
// 跟之前的2个wait方法一样只不过该方法一直等待没有超时时间这个概念
public final void wait() throws InterruptedException
//实例被垃圾回收器回收的时候触发的操作
protected void finalize() throws Throwable { }
```



#  hashCode() 有什么用？

<font style="color:red;font-size:20px">重点词语: </font>

用于更快的检索出class的地址  也可以判断class是否存在 是否重复加载



`hashCode()` 的作用是获取哈希码（`int` 整数）也称为散列码这个哈希码的作用是确定该对象在哈希表中的索引位置

`hashCode()`定义在 JDK 的 `Object` 类中这就意味着 Java 中的任何类都包含有 `hashCode()` 函数另外需要注意的是： `Object` 的 `hashCode()` 方法是本地方法也就是用 C 语言或 C++ 实现的该方法通常用来将对象的内存地址转换为整数之后返回

散列表存储的是键值对(key-value)它的特点是：**能根据“键”快速的检索出对应的“值”这其中就利用到了散列码！（可以快速找到所需要的对象）**



# 为什么要有 hashCode？

<font style="color:red; font-size:20px">重点词语: </font>

1. 新添加类后比较该类是否已经存在

2. 因为有哈希冲突所以还要使用equals方法

   

`hashCode()` 和 `equals()`都是用于比较两个对象是否相等

当你把对象加入 `HashSet` 时`HashSet` 会先计算对象的 `hashCode` 值来判断对象加入的位置同时也会与其他已经加入的对象的 `hashCode` 值作比较如果没有相符的 `hashCode``HashSet` 会假设对象没有重复出现但是如果发现有相同 `hashCode` 值的对象这时会调用 `equals()` 方法来检查 `hashCode` 相等的对象是否真的相同如果两者相同`HashSet` 就不会让其加入操作成功如果不同的话就会重新散列到其他位置这样我们就大大减少了 `equals` 的次数相应就大大提高了执行速度



# ~~为什么重写 equals() 时必须重写 hashCode() 方法？~~

<font style="color:red; font-size:20px">重点词语: </font>

哈希冲突



#  String、StringBuffer、StringBuilder 的区别？

在 Java 9 之后`String` 、`StringBuilder` 与 `StringBuffer` 的实现改用 `byte` 数组存储字符串

## 可变性

`String` 是不可变的（后面会详细分析原因）

`StringBuilder` 与 `StringBuffer` 都继承自 `AbstractStringBuilder` 类在 `AbstractStringBuilder` 中也是使用字符数组保存字符串不过没有使用 `final` 和 `private` 关键字修饰最关键的是这个 `AbstractStringBuilder` 类还提供了很多修改字符串的方法比如 `append` 方法

## 线程安全性

`String` 中的对象是不可变的也就可以理解为常量线程安全

`AbstractStringBuilder` 是 `StringBuilder` 与 `StringBuffer` 的公共父类定义了一些字符串的基本操作如 `expandCapacity`、`append`、`insert`、`indexOf` 等公共方法

`StringBuffer` 对方法加了同步锁或者对调用的方法加了同步锁所以是线程安全的

`StringBuilder` 并没有对方法进行加同步锁所以是非线程安全的



## 性能

每次对 `String` 类型进行改变的时候都会生成一个新的 `String` 对象然后将指针指向新的 `String` 对象

`StringBuffer` 每次都会对 `StringBuffer` 对象本身进行操作而不是生成新的对象并改变对象引用

相同情况下使用 `StringBuilder` 相比使用 `StringBuffer` 仅能获得 10%~15% 左右的性能提升但却要冒多线程不安全的风险



对于三者使用的总结：

1. 操作少量的数据: 适用 `String`
2. 单线程操作字符串缓冲区下操作大量数据: 适用 `StringBuilder`
3. 多线程操作字符串缓冲区下操作大量数据: 适用 `StringBuffer`



#### String 为什么是不可变的?

`String` 类中使用 `final` 关键字修饰字符数组来保存字符串所以`String` 对象是不可变的





# 字符串拼接用“+” 还是 StringBuilder?

Java 语言本身并不支持运算符重载“+”和“+=”是专门为 String 类重载过的运算符也是 Java 中仅有的两个重载过的运算符

<font style="color:red; font-size:20px">重点词语: </font>

虽然在代码中`String s = st1 + st2 + st3`

实际在底层调用的还是StringBuilder ().append()方法实现的  完成拼接后使用toString()返回

尤其在循环中使用拼接会创建多个StringBuilder()对象  很浪费内存. 直接使用 `StringBuilder` 对象进行字符串拼接的话就不会存在这个问题了



# String#equals() 和 Object#equals() 有何区别？

`String` 中的 `equals` 方法是被重写过的比较的是 String 字符串的值是否相等 `Object` 的 `equals` 方法是比较的对象的内存地址



#  字符串常量池的作用了解吗？

**字符串常量池** 是 JVM 为了提升性能和减少内存消耗针对字符串（String 类）专门开辟的一块区域主要目的是为了避免字符串的重复创建

若字符串常量池中存在该字符串则直接返回引用实例；若不存在先实例化该字符串并且将该字符串放入字符串常量池中以便于下次使用时直接取用达到缓存快速使用的效果

``` java
// 在堆中创建字符串对象”ab“
// 将字符串对象”ab“的引用保存在字符串常量池中
String aa = "ab";
// 直接返回字符串常量池中字符串对象”ab“的引用
String bb = "ab";
System.out.println(aa==bb);// true
```





# String s1 = new String("abc");这句话创建了几个字符串对象？

会创建 1 或 2 个字符串对象

1. 如果字符串常量池中不存在字符串对象“abc”的引用那么会在堆中创建 2 个字符串对象“abc”
2. 如果字符串常量池中已存在字符串对象“abc”的引用则只会在堆中创建 1 个字符串对象“abc”



#  intern 方法有什么作用?

`String.intern()` 是一个 native（本地）方法其作用是将指定的字符串对象的引用保存在字符串常量池中可以简单分为两种情况：

- 如果字符串常量池中保存了对应的字符串对象的引用就直接返回该引用
- 如果字符串常量池中没有保存了对应的字符串对象的引用那就在常量池中创建一个指向该字符串对象的引用并返回

``` java
// 在堆中创建字符串对象”Java“
// 将字符串对象”Java“的引用保存在字符串常量池中
String s1 = "Java";
// 直接返回字符串常量池中字符串对象”Java“对应的引用
String s2 = s1.intern();
// 会在堆中在单独创建一个字符串对象
String s3 = new String("Java");
// 直接返回字符串常量池中字符串对象”Java“对应的引用
String s4 = s3.intern();
// s1 和 s2 指向的是堆中的同一个对象
System.out.println(s1 == s2); // true
// s3 和 s4 指向的是堆中不同的对象
System.out.println(s3 == s4); // false
// s1 和 s4 指向的是堆中的同一个对象
System.out.println(s1 == s4); //true
```



# String 类型的变量和常量做“+”运算时发生了什么？

先来看字符串不加 `final` 关键字拼接的情况（JDK1.8）：

``` java
String str1 = "str";
String str2 = "ing";
String str3 = "str" + "ing";
String str4 = str1 + str2;
String str5 = "string";
System.out.println(str3 == str4);//false
System.out.println(str3 == str5);//true
System.out.println(str4 == str5);//false
```

>  **注意** ：比较 String 字符串的值是否相等可以使用 `equals()` 方法 `String` 中的 `equals` 方法是被重写过的 `Object` 的 `equals` 方法是比较的对象的内存地址而 `String` 的 `equals` 方法比较的是字符串的值是否相等如果你使用 `==` 比较两个字符串是否相等的话IDEA 还是提示你使用 `equals()` 方法替换



**对于编译期可以确定值的字符串也就是常量字符串 jvm 会将其存入字符串常量池并且字符串常量拼接得到的字符串常量在编译阶段就已经被存放字符串常量池这个得益于编译器的优化**

在编译过程中Javac 编译器（下文中统称为编译器）会进行一个叫做 **常量折叠(Constant Folding)** 的代码优化《深入理解 Java 虚拟机》中是也有介绍到：

![](https://guide-blog-images.oss-cn-shenzhen.aliyuncs.com/javaguide/image-20210817142715396.png)



常量折叠会把常量表达式的值求出来作为常量嵌在最终生成的代码中这是 Javac 编译器会对源代码做的极少量优化措施之一(代码优化几乎都在即时编译器中进行)

对于 `String str3 = "str" + "ing";` 编译器会给你优化成 `String str3 = "string";` 

并不是所有的常量都会进行折叠只有编译器在程序编译期就可以确定值的常量才可以：

- 基本数据类型( `byte`、`boolean`、`short`、`char`、`int`、`float`、`long`、`double`)以及字符串常量
- `final` 修饰的基本数据类型和字符串变量
- 字符串通过 “+”拼接得到的字符串、基本数据类型之间算数运算（加减乘除）、基本数据类型的位运算（<<、>>、>>> ）

**引用的值在程序编译期是无法确定的编译器无法对其进行优化**

对象引用和“+”的字符串拼接方式实际上是通过 `StringBuilder` 调用 `append()` 方法实现的拼接完成之后调用 `toString()` 得到一个 `String` 对象 



```java
String str4 = new StringBuilder().append(str1).append(str2).toString();
```



我们在平时写代码的时候尽量避免多个字符串对象拼接因为这样会重新创建对象如果需要改变字符串的话可以使用 `StringBuilder` 或者 `StringBuffer`

不过字符串使用 `final` 关键字声明之后可以让编译器当做常量来处理

示例代码：

``` java
final String str1 = "str";
final String str2 = "ing";
// 下面两个表达式其实是等价的
String c = "str" + "ing";// 常量池中的对象
String d = str1 + str2; // 常量池中的对象
System.out.println(c == d);// true
```



被 `final` 关键字修改之后的 `String` 会被编译器当做常量来处理编译器在程序编译期就可以确定它的值其效果就相当于访问常量

如果 编译器在运行时才能知道其确切值的话就无法对其优化

示例代码（`str2` 在运行时才能确定其值）：

``` java
final String str1 = "str";
final String str2 = getStr();
String c = "str" + "ing";// 常量池中的对象
String d = str1 + str2; // 在堆上创建的新的对象
System.out.println(c == d);// false
public static String getStr() {
      return "ing";
}
```





# 异常

**Java 异常类层次结构图概览** ：

![](https://javaguide.cn/assets/types-of-exceptions-in-java.75041da9.png)







# Exception 和 Error 有什么区别？

在 Java 中所有的异常都有一个共同的祖先 `java.lang` 包中的 `Throwable` 类`Throwable` 类有两个重要的子类:

+ Exception和Error都是继承了Throwable类在java中只有Throwable类型的实例才可以被抛出（throw）或者捕获（catch）他是异常处理机制的基本组成类型
+ Exception和Error体现了java平台设计者对不同异常情况的分类Exception是程序正常运行中可以预料的意外情况可能并且应该被捕获进行相应的处理
+ Error是指正常情况下不大可能出现的情况绝大部分的Error都会导致程序（比如JVM自身）处于非正常状态不可恢复状态既然是非正常情况所以不便于也不需要捕获常见的比如OutOfMemoryError之类都是Error的子类
+ Exception又分为可检查（checked）异常和不检查（unchecked）异常可检查异常在源码里必须显示的进行捕获处理这里是编译期检查的一部分前面我们介绍的不可查的Error是Throwable不是Exception
+ 不检查异常就是所谓的运行时异常类NullPointerException,ArrayIndexOutOfBoundsExceptin之类通常是可以编码避免的逻辑错误具体根据需要来判断是否需要捕获并不会在编译器强制要求



# Checked Exception 和 Unchecked Exception 



+ **Checked Exception** 即 受检查异常 Java 代码在编译过程中如果受检查异常没有被 `catch`或者`throws` 关键字处理的话就没办法通过编译

+ **Unchecked Exception** 即 **不受检查异常** Java 代码在编译过程中 我们即使不处理不受检查异常也可以正常通过编译



## 常见的CheckedException异常

除了`RuntimeException`及其子类以外其他的`Exception`类及其子类都属于受检查异常 常见的受检查异常有： IO 相关的异常、`ClassNotFoundException` 、`SQLException`...



## 常见的UncheckedException异常

`RuntimeException` 及其子类都统称为非受检查异常常见的有（建议记下来日常开发中会经常用到）：

- `NullPointerException`(空指针错误)
- `IllegalArgumentException`(参数错误比如方法入参类型错误)
- `NumberFormatException`（字符串转换为数字格式错误`IllegalArgumentException`的子类）
- `ArrayIndexOutOfBoundsException`（数组越界错误）
- `ClassCastException`（类型转换错误）
- `ArithmeticException`（算术错误）
- `SecurityException` （安全错误比如权限不够）
- `UnsupportedOperationException`(不支持的操作错误比如重复创建同一用户)
- ......



# Throwable 类常用方法有哪些？

- `String getMessage()`: 返回异常发生时的简要描述
- `String toString()`: 返回异常发生时的详细信息
- `String getLocalizedMessage()`: 返回异常对象的本地化信息使用 `Throwable` 的子类覆盖这个方法可以生成本地化信息如果子类没有覆盖该方法则该方法返回的信息与 `getMessage()`返回的结果相同
- `void printStackTrace()`: 在控制台上打印 `Throwable` 对象封装的异常信息



# try-catch-finally 如何使用？

- `try`块 ： 用于捕获异常其后可接零个或多个 `catch` 块如果没有 `catch` 块则必须跟一个 `finally` 块
- `catch`块 ： 用于处理 try 捕获到的异常
- `finally` 块 ： 无论是否捕获或处理异常`finally` 块里的语句都会被执行当在 `try` 块或 `catch` 块中遇到 `return` 语句时`finally` 语句块将在方法返回之前被执行



## try、catch、finally语句中有return 的各类情况

不管return关键字在哪finally一定会执行完毕

理论上来说try、catch、finally块中都允许书写return关键字

但是执行优先级较低的块中的return关键字定义的返回值将覆盖执行优先级较高的块中return关键字定义的返回值

优先级由高到低顺序: try ——> cache ——> finally     

也就是说finally块中定义的返回值将会覆盖catch块、try块中定义的返回值；catch块中定义的返回值将会覆盖try块中定义的返回值

再换句话说如果在finally块中通过return关键字定义了返回值那么之前所有通过return关键字定义的返回值都将失效——因为finally块中的代码一定是会执行的



# 异常使用有哪些需要注意的地方？

- 不要把异常定义为静态变量因为这样会导致异常栈信息错乱每次手动抛出异常我们都需要手动 new 一个异常对象抛出
- 抛出的异常信息一定要有意义
- 建议抛出更加具体的异常比如字符串转换为数字格式错误的时候应该抛出`NumberFormatException`而不是其父类`IllegalArgumentException`
- 使用日志打印异常之后就不要再抛出异常了（两者不要同时存在一段代码逻辑中）
- ......



# 什么是泛型？有什么作用？

> C++的模板

需要写一个类 方法 接口时候  参数不确定时候使用泛型 比如web的同一接口返回

- 自定义接口通用返回结果 `CommonResult<T>` 通过参数 `T` 可根据具体的返回类型动态指定结果的数据类型
- 定义 `Excel` 处理类 `ExcelUtil<T>` 用于动态指定 `Excel` 导出的数据类型
- 构建集合工



编译器可以对泛型参数进行检测并且通过泛型参数可以指定传入的对象类型比如 `ArrayList<Persion> persons = new ArrayList<Persion>()` 这行代码就指明了该 `ArrayList` 对象只能传入 `Persion` 对象如果传入其他类型的对象就会报错



# 泛型的使用方式有哪几种？

泛型一般有三种使用方式:**泛型类**、**泛型接口**、**泛型方法**

1. 泛型类:

   ``` java
   //此处T可以随便写为任意标识常见的如T、E、K、V等形式的参数常用于表示泛型
   //在实例化泛型类时必须指定T的具体类型
   public class Generic<T>{
       private T key;
       public Generic(T key) {
           this.key = key;
       }
       public T getKey(){
           return key;
       }
   }
   
   Generic<Integer> genericInteger = new Generic<Integer>(123)
   ```

2. 泛型接口

   ``` java
   public interface Generator<T> {
       public T method();
   }
   // 实现可以指定 也可以不指定
   class GeneratorImpl<T> implements Generator<T>{
       @Override
       public T method() {
           return null;
       }
   }
   class GeneratorImpl<T> implements Generator<String>{
       @Override
       public String method() {
           return "hello";
       }
   }
   ```

3. 泛型方法

   ``` java
   // 指定泛型是数组类型的
   public static < E > void printArray( E[] inputArray )
   {
     for ( E element : inputArray ){
       System.out.printf( "%s ", element );
     }
     System.out.println();
   }
   // 创建不同类型数组： Integer, Double 和 Character
   Integer[] intArray = { 1, 2, 3 };
   String[] stringArray = { "Hello", "World" };
   printArray( intArray  );
   printArray( stringArray  );
   ```

   

# 何为反射？优缺点

反射之所以被称为框架的灵魂主要是因为它赋予了我们在运行时分析类以及执行类中方法的能力通过反射你可以获取任意一个类的所有属性和方法你还可以调用这些方法和属性



- **优点** ： 可以让咱们的代码更加灵活、为各种框架提供开箱即用的功能提供了便利
- **缺点** ：让我们在运行时有了分析操作类的能力这同样也增加了安全问题比如可以无视泛型参数的安全检查（泛型参数的安全检查发生在编译时）另外反射的性能也要稍差点不过对于框架来说实际是影响不大的



# 反射的应用场景

像咱们平时大部分时候都是在写业务代码很少会接触到直接使用反射机制的场景

但是这并不代表反射没有用相反正是因为反射你才能这么轻松地使用各种框架像 Spring/Spring Boot、MyBatis 等等框架中都大量使用了反射机制

**这些框架中也大量使用了动态代理而动态代理的实现也依赖反射**

比如下面是通过 JDK 实现动态代理的示例代码其中就使用了反射类 `Method` 来调用指定的方法

**注解** 的实现也用到了反射

**加载jdbc**也用到了反射



# 获取 Class 对象的四种方式

如果我们动态获取到这些信息我们需要依靠 Class 对象Class 类对象将一个类的方法、变量等信息告诉运行的程序Java 提供了四种方式获取 Class 对象:

1. 知道具体类的情况下可以使用：

```java
Class alunbarClass = TargetObject.class;
```

但是我们一般是不知道具体类的基本都是通过遍历包下面的类来获取 Class 对象通过此方式获取 Class 对象不会进行初始化

2. 通过 `Class.forName()`传入类的全路径获取：

```java
Class alunbarClass1 = Class.forName("cn.javaguide.TargetObject");
```

3. 通过对象实例`instance.getClass()`获取：

```java
TargetObject o = new TargetObject();
Class alunbarClass2 = o.getClass();
```

4. 通过类加载器`xxxClassLoader.loadClass()`传入类路径获取:**

```java
ClassLoader.getSystemClassLoader().loadClass("cn.javaguide.TargetObject");
```

通过类加载器获取 Class 对象不会进行初始化意味着不进行包括初始化等一系列步骤静态代码块和静态对象不会得到执行

















# 注解

`Annotation` （注解） 是 Java5 开始引入的新特性可以看作是一种特殊的注释主要用于修饰类、方法或者变量

注解本质是一个继承了`Annotation` 的特殊接口：

```java
@Target(ElementType.METHOD)
@Retention(RetentionPolicy.SOURCE)
public @interface Override {

}
public interface Override extends Annotation{
}
```

注解只有被解析之后才会生效常见的解析方法有两种：

- **编译期直接扫描** ：编译器在编译 Java 代码的时候扫描对应的注解并处理比如某个方法使用`@Override` 注解编译器在编译的时候就会检测当前的方法是否重写了父类对应的方法
- **运行期通过反射处理** ：像框架中自带的注解(比如 Spring 框架的 `@Value` 、`@Component`)都是通过反射来进行处理的

JDK 提供了很多内置的注解（比如 `@Override` 、`@Deprecated`）同时我们还可以自定义注解



# 什么是序列化?什么是反序列化?

如果我们需要持久化 Java 对象比如将 Java 对象保存在文件中或者在网络传输 Java 对象这些场景都需要用到序列化

简单来说：

- **序列化**： 将数据结构或对象转换成二进制字节流的过程
- **反序列化**：将在序列化过程中所生成的二进制字节流转换成数据结构或者对象的过程



# Java 序列化中如果有些字段不想进行序列化怎么办？

对于不想进行序列化的变量使用 `transient` 关键字修饰

`transient` 关键字的作用是：阻止实例中那些用此关键字修饰的的变量序列化；当对象被反序列化时被 `transient` 修饰的变量值不会被持久化和恢复

关于 `transient` 还有几点注意：

- `transient` 只能修饰变量不能修饰类和方法
- `transient` 修饰的变量在反序列化后变量值将会被置成类型的默认值例如如果是修饰 `int` 类型那么反序列后结果就是 `0`
- `static` 变量因为不属于任何对象(Object)所以无论有没有 `transient` 关键字修饰均不会被序列化



#  获取用键盘输入常用的两种方法

1. 方法 1：通过 `Scanner`

   ```` java
   Scanner input = new Scanner(System.in);
   String s  = input.nextLine();
   input.close();
   ````

2. 方法 2：通过 `BufferedReader`

   ``` java
   BufferedReader input = new BufferedReader(new InputStreamReader(System.in));
   String s = input.readLine();
   ```

   

# Java 中 IO 流分为几种?



- 按照流的流向分可以分为输入流和输出流；
- 按照操作单元划分可以划分为字节流和字符流；
- 按照流的角色划分为节点流和处理流



Java IO 流共涉及 40 多个类这些类看上去很杂乱但实际上很有规则而且彼此之间存在非常紧密的联系 Java IO 流的 40 多个类都是从如下 4 个抽象类基类中派生出来的

- InputStream/Reader: 所有的输入流的基类前者是字节输入流后者是字符输入流
- OutputStream/Writer: 所有输出流的基类前者是字节输出流后者是字符输出流



# 既然有了字节流,为什么还要有字符流?

问题本质想问：**不管是文件读写还是网络发送接收信息的最小存储单元都是字节那为什么 I/O 流操作要分为字节流操作和字符流操作呢？**

回答：字符流是由 Java 虚拟机将字节转换得到的问题就出在这个过程还算是非常耗时并且如果我们不知道编码类型就很容易出现乱码问题所以 I/O 流就干脆提供了一个直接操作字符的接口方便我们平时对字符进行流操作如果音频文件、图片等媒体文件用字节流比较好如果涉及到字符的话使用字符流比较好



# 为什么 Java 中只有值传递？

开始之前我们先来搞懂下面这两个概念：

- 形参&实参  void a(这里的参数为形参)    XX.a(这里的参数为实参)
- 值传递&引用传递
  - **值传递** ：方法接收的是实参值的拷贝会创建副本
  - **引用传递** ：方法接收的直接是实参所引用的对象在堆中的地址不会创建副本对形参的修改将影响到实参

很多程序设计语言（比如 C++、 Pascal )提供了两种参数传递的方式不过在 Java 中只有值传递

Java 中将实参传递给方法（或函数）的方式是 **值传递** ：

- 如果参数是基本类型的话很简单传递的就是基本类型的字面量值的拷贝会创建副本
- 如果参数是引用类型传递的就是实参所引用的对象在堆中地址值的拷贝同样也会创建副本



# Java 序列化详解

如果我们需要持久化 Java 对象比如将 Java 对象保存在文件中或者在网络传输 Java 对象这些场景都需要用到序列化

- **序列化**： 将数据结构或对象转换成二进制字节流的过程
- **反序列化**：将在序列化过程中所生成的二进制字节流的过程转换成数据结构或者对象的过程







# 代理模式

**我们使用代理对象来代替对真实对象(real object)的访问这样就可以在不修改原目标对象的前提下提供额外的功能操作扩展目标对象的功能**

**代理模式的主要作用是扩展目标对象的功能比如说在目标对象的某个方法执行前后你可以增加一些自定义的操作**

举个例子：新娘找来了自己的姨妈来代替自己处理新郎的提问新娘收到的提问都是经过姨妈处理过滤之后的姨妈在这里就可以看作是代理你的代理对象代理的行为（方法）是接收和回复新郎的提问

代理模式有静态代理和动态代理两种实现方式我们 先来看一下静态代理模式的实现



## 静态代理

**静态代理中我们对目标对象的每个方法的增强都是手动完成的（\*后面会具体演示代码\*）非常不灵活（比如接口一旦新增加方法目标对象和代理对象都要进行修改\*）且麻烦(\*需要对每个目标类都单独写一个代理类)** 

从 JVM 层面来说 **静态代理在编译时就将接口、实现类、代理类这些都变成了一个个实际的 class 文件**

代码示例:

``` java
// 定义发送短信接口
public interface SmsService {
    String send(String message);
}
// 实现发送短信接口
public class SmsServiceImpl implements SmsService {
    public String send(String message) {
        System.out.println("send message:" + message);
        return message;
    }
}
// 创建代理类并实现发送短信接口
public class SmsProxy implements SmsService {
    private final SmsService smsService;
    public SmsProxy(SmsService smsService) {
        this.smsService = smsService;
    }
    @Override
    public String send(String message) {
        //调用方法之前我们可以添加自己的操作
        System.out.println("before method send()");
        smsService.send(message);
        //调用方法之后我们同样可以添加自己的操作
        System.out.println("after method send()");
        return null;
    }
}
// 具体使用
public class Main {
    public static void main(String[] args) {
        SmsService smsService = new SmsServiceImpl();
        SmsProxy smsProxy = new SmsProxy(smsService);
        smsProxy.send("java");
    }
}
```



## 动态代理

比于静态代理来说动态代理更加灵活我们不需要针对每个目标类都单独创建一个代理类并且也不需要我们必须实现接口我们可以直接代理实现类( *CGLIB 动态代理机制*)

**从 JVM 角度来说动态代理是在运行时动态生成类字节码并加载到 JVM 中的**

说到动态代理Spring AOP、RPC 框架应该是两个不得不提的它们的实现都依赖了动态代理

**动态代理在我们日常开发中使用的相对较少但是在框架中的几乎是必用的一门技术学会了动态代理之后对于我们理解和学习各种框架的原理也非常有帮助**

就 Java 来说动态代理的实现方式有很多种比如 **JDK 动态代理**、**CGLIB 动态代理**等等

guide-rpc-frameworkopen in new window 使用的是 JDK 动态代理我们先来看看 JDK 动态代理的使用

另外虽然 guide-rpc-frameworkopen in new window]没有用到 **CGLIB 动态代理** 我们这里还是简单介绍一下其使用以及和**JDK 动态代理**的对比



### JDK动态代理



### CGLIB动态代理

**JDK 动态代理有一个最致命的问题是其只能代理实现了接口的类**

**为了解决这个问题我们可以用 CGLIB 动态代理机制来避免**

[CGLIBopen in new window](https://github.com/cglib/cglib)(*Code Generation Library*)是一个基于[ASMopen in new window](http://www.baeldung.com/java-asm)的字节码生成库它允许我们在运行时对字节码进行修改和动态生成CGLIB 通过继承方式实现代理很多知名的开源框架都使用到了[CGLIBopen in new window](https://github.com/cglib/cglib) 例如 Spring 中的 AOP 模块中：如果目标对象实现了接口则默认采用 JDK 动态代理否则采用 CGLIB 动态代理



## JDK 动态代理和 CGLIB 动态代理对比

1. **JDK 动态代理只能代理实现了接口的类或者直接代理接口而 CGLIB 可以代理未实现任何接口的类** 另外 CGLIB 动态代理是通过生成一个被代理类的子类来拦截被代理类的方法调用因此不能代理声明为 final 类型的类和方法
2. 就二者的效率来说大部分情况都是 JDK 动态代理更优秀随着 JDK 版本的升级这个优势更加明显



## 动态代理和静态代理对比:

1. **灵活性** ：动态代理更加灵活不需要必须实现接口可以直接代理实现类并且可以不需要针对每个目标类都创建一个代理类另外静态代理中接口一旦新增加方法目标对象和代理对象都要进行修改这是非常麻烦的！
2. **JVM 层面** ：静态代理在编译时就将接口、实现类、代理类这些都变成了一个个实际的 class 文件而动态代理是在运行时动态生成类字节码并加载到 JVM 中的





# 集合简介

Java 集合 也叫作容器主要是由两大接口派生而来：一个是 `Collection`接口主要用于存放单一元素；另一个是 `Map` 接口主要用于存放键值对对于`Collection` 接口下面又有三个主要的子接口：`List`、`Set` 和 `Queue`

模型图

![](https://javaguide.cn/assets/java-collection-hierarchy.77b66a51.png)　

# 说说 List, Set, Queue, Map 四者的区别？

- `List`(对付顺序的好帮手): 存储的元素是有序的、可重复的
- `Set`(注重独一无二的性质): 存储的元素是无序的、不可重复的
- `Queue`(实现排队功能的叫号机): 按特定的排队规则来确定先后顺序存储的元素是有序的、可重复的
- `Map`(用 key 来搜索的专家): 使用键值对（key-value）存储类似于数学上的函数 y=f(x)"x" 代表 key"y" 代表 valuekey 是无序的、不可重复的value 是无序的、可重复的每个键最多映射到一个值



# 集合框架底层数据结构

先来看一下 `Collection` 接口下面的集合

## List

- `ArrayList`： `Object[]` 数组
- `Vector`：`Object[]` 数组
- `LinkedList`： 双向链表(JDK1.6 之前为循环链表JDK1.7 取消了循环)

## Set

- `HashSet`(无序唯一): 基于 `HashMap` 实现的底层采用 `HashMap` 来保存元素
- `LinkedHashSet`: `LinkedHashSet` 是 `HashSet` 的子类并且其内部是通过 `LinkedHashMap` 来实现的有点类似于我们之前说的 `LinkedHashMap` 其内部是基于 `HashMap` 实现一样不过还是有一点点区别的
- `TreeSet`(有序唯一): 红黑树(自平衡的排序二叉树)

## Queue

- `PriorityQueue`: `Object[]` 数组来实现二叉堆
- `ArrayQueue`: `Object[]` 数组 + 双指针



再来看看 `Map` 接口下面的集合

## Map

- `HashMap`： JDK1.8 之前 `HashMap` 由数组+链表组成的数组是 `HashMap` 的主体链表则是主要为了解决哈希冲突而存在的（“拉链法”解决冲突）JDK1.8 以后在解决哈希冲突时有了较大的变化当链表长度大于阈值（默认为 8）（将链表转换成红黑树前会判断如果当前数组的长度小于 64那么会选择先进行数组扩容而不是转换为红黑树）时将链表转化为红黑树以减少搜索时间
- `LinkedHashMap`： `LinkedHashMap` 继承自 `HashMap`所以它的底层仍然是基于拉链式散列结构即由数组和链表或红黑树组成另外`LinkedHashMap` 在上面结构的基础上增加了一条双向链表使得上面的结构可以保持键值对的插入顺序同时通过对链表进行相应的操作实现了访问顺序相关逻辑详细可以查看：[《LinkedHashMap 源码详细分析（JDK1.8）》open in new window](https://www.imooc.com/article/22931)
- `Hashtable`： 数组+链表组成的数组是 `Hashtable` 的主体链表则是主要为了解决哈希冲突而存在的
- `TreeMap`： 红黑树（自平衡的排序二叉树）



# 如何选用集合?

主要根据集合的特点来选用比如我们需要根据键值获取到元素值时就选用 `Map` 接口下的集合需要排序时选择 `TreeMap`,不需要排序时就选择 `HashMap`,需要保证线程安全就选用 `ConcurrentHashMap`

当我们只需要存放元素值时就选择实现`Collection` 接口的集合需要保证元素唯一时选择实现 `Set` 接口的集合比如 `TreeSet` 或 `HashSet`不需要就选择实现 `List` 接口的比如 `ArrayList` 或 `LinkedList`然后再根据实现这些接口的集合的特点来选用

### 如何选用集合?

主要根据集合的特点来选用比如我们需要根据键值获取到元素值时就选用 `Map` 接口下的集合需要排序时选择 `TreeMap`,不需要排序时就选择 `HashMap`,需要保证线程安全就选用 `ConcurrentHashMap`

当我们只需要存放元素值时就选择实现`Collection` 接口的集合需要保证元素唯一时选择实现 `Set` 接口的集合比如 `TreeSet` 或 `HashSet`不需要就选择实现 `List` 接口的比如 `ArrayList` 或 `LinkedList`然后再根据实现这些接口的集合的特点来选用



# ArrayList 与 LinkedList 区别?



1. **是否保证线程安全：** `ArrayList` 和 `LinkedList` 都是不同步的也就是不保证线程安全；
2. **底层数据结构：** `ArrayList` 底层使用的是 **`Object` 数组**；`LinkedList` 底层使用的是 **双向链表** 数据结构（JDK1.6 之前为循环链表JDK1.7 取消了循环注意双向链表和双向循环链表的区别下面有介绍到！）
3. 插入和删除是否受元素位置的影响：
   - `ArrayList` 采用数组存储所以插入和删除元素的时间复杂度受元素位置的影响 比如：执行`add(E e)`方法的时候 `ArrayList` 会默认在将指定的元素追加到此列表的末尾这种情况时间复杂度就是 O(1)但是如果要在指定位置 i 插入和删除元素的话（`add(int index, E element)`）时间复杂度就为 O(n-i)因为在进行上述操作的时候集合中第 i 和第 i 个元素之后的(n-i)个元素都要执行向后位/向前移一位的操作
   - `LinkedList` 采用链表存储所以如果是在头尾插入或者删除元素不受元素位置的影响（`add(E e)`、`addFirst(E e)`、`addLast(E e)`、`removeFirst()` 、 `removeLast()`）时间复杂度为 O(1)如果是要在指定位置 `i` 插入和删除元素的话（`add(int index, E element)``remove(Object o)`） 时间复杂度为 O(n) 因为需要先移动到指定位置再插入
4. **是否支持快速随机访问：** `LinkedList` 不支持高效的随机元素访问而 `ArrayList` 支持快速随机访问就是通过元素的序号快速获取元素对象(对应于`get(int index)`方法)
5. **内存空间占用：** `ArrayList` 的空 间浪费主要体现在在 list 列表的结尾会预留一定的容量空间而 LinkedList 的空间花费则体现在它的每一个元素都需要消耗比 ArrayList 更多的空间（因为要存放直接后继和直接前驱以及数据）



# 比较 HashSet、LinkedHashSet 和 TreeSet 三者的异同

- `HashSet`、`LinkedHashSet` 和 `TreeSet` 都是 `Set` 接口的实现类都能保证元素唯一并且都不是线程安全的
- `HashSet`、`LinkedHashSet` 和 `TreeSet` 的主要区别在于底层数据结构不同`HashSet` 的底层数据结构是哈希表（基于 `HashMap` 实现）`LinkedHashSet` 的底层数据结构是链表和哈希表元素的插入和取出顺序满足 FIFO`TreeSet` 底层数据结构是红黑树元素是有序的排序的方式有自然排序和定制排序
- 底层数据结构不同又导致这三者的应用场景不同`HashSet` 用于不需要保证元素插入和取出顺序的场景`LinkedHashSet` 用于保证元素的插入和取出顺序满足 FIFO 的场景`TreeSet` 用于支持对元素自定义排序规则的场景



# Queue

## Queue 与 Deque 的区别

+ `Queue` 是单端队列只能从一端插入元素另一端删除元素实现上一般遵循 **先进先出（FIFO）** 规则

  `Queue` 扩展了 `Collection` 的接口根据 **因为容量问题而导致操作失败后处理方式的不同** 可以分为两类方法: 一种在操作失败后会抛出异常另一种则会返回特殊值

+ `Deque` 是双端队列在队列的两端均可以插入或删除元素

  `Deque` 扩展了 `Queue` 的接口, 增加了在队首和队尾进行插入和删除的方法同样根据失败后处理方式的不同分为两类：



## ArrayDeque 与 LinkedList 的区别

`ArrayDeque` 和 `LinkedList` 都实现了 `Deque` 接口两者都具有队列的功能但两者有什么区别呢？

- `ArrayDeque` 是基于可变长的数组和双指针来实现而 `LinkedList` 则通过链表来实现
- `ArrayDeque` 不支持存储 `NULL` 数据但 `LinkedList` 支持
- `ArrayDeque` 是在 JDK1.6 才被引入的而`LinkedList` 早在 JDK1.2 时就已经存在
- `ArrayDeque` 插入时可能存在扩容过程, 不过均摊后的插入操作依然为 O(1)虽然 `LinkedList` 不需要扩容但是每次插入数据时均需要申请新的堆空间均摊性能相比更慢

从性能的角度上选用 `ArrayDeque` 来实现队列要比 `LinkedList` 更好此外`ArrayDeque` 也可以用于实现栈



# 说一说 PriorityQueue

`PriorityQueue` 是在 JDK1.5 中被引入的, 其与 `Queue` 的区别在于元素出队顺序是与优先级相关的即总是优先级最高的元素先出队

这里列举其相关的一些要点：

- `PriorityQueue` 利用了二叉堆的数据结构来实现的底层使用可变长的数组来存储数据
- `PriorityQueue` 通过堆元素的上浮和下沉实现了在 O(logn) 的时间复杂度内插入元素和删除堆顶元素
- `PriorityQueue` 是非线程安全的且不支持存储 `NULL` 和 `non-comparable` 的对象
- `PriorityQueue` 默认是小顶堆但可以接收一个 `Comparator` 作为构造参数从而来自定义元素优先级的先后

`PriorityQueue` 在面试中可能更多的会出现在手撕算法的时候典型例题包括堆排序、求第K大的数、带权图的遍历等所以需要会熟练使用才行





# HashMap 和 Hashtable 的区别

1. **线程是否安全：** `HashMap` 是非线程安全的，`Hashtable` 是线程安全的,因为 `Hashtable` 内部的方法基本都经过`synchronized` 修饰。（如果你要保证线程安全的话就使用 `ConcurrentHashMap` 吧！）；
2. **效率：** 因为线程安全的问题，`HashMap` 要比 `Hashtable` 效率高一点。另外，`Hashtable` 基本被淘汰，不要在代码中使用它；
3. **对 Null key 和 Null value 的支持：** `HashMap` 可以存储 null 的 key 和 value，但 null 作为键只能有一个，null 作为值可以有多个；Hashtable 不允许有 null 键和 null 值，否则会抛出 `NullPointerException`。
4. **初始容量大小和每次扩充容量大小的不同 ：**
   1. 创建时如果不指定容量初始值，`Hashtable` 默认的初始大小为 11，之后每次扩充，容量变为原来的 2n+1。`HashMap` 默认的初始化大小为 16。之后每次扩充，容量变为原来的 2 倍。
   2.  创建时如果给定了容量初始值，那么 Hashtable 会直接使用你给定的大小，而 `HashMap` 会将其扩充为 2 的幂次方大小 (更平均  更散列)
5. **底层数据结构：** JDK1.8 以后的 `HashMap` 在解决哈希冲突时有了较大的变化，当链表长度大于阈值（默认为 8）（将链表转换成红黑树前会判断，如果当前数组的长度小于 64，那么会选择先进行数组扩容，而不是转换为红黑树）时，将链表转化为红黑树，以减少搜索时间。Hashtable 没有这样的机制。





# HashMap 和 HashSet 区别

一个存储Key-Value一个存储对象  后者底层使用前者实现  



# HashMap 和 TreeMap 区别

`TreeMap` 和`HashMap` 都继承自`AbstractMap` ，但是需要注意的是`TreeMap`它还实现了`NavigableMap`接口和`SortedMap` 接口。

+ 实现 `NavigableMap` 接口让 `TreeMap` 有了对集合内元素的搜索的能力。
+ 实现`SortedMap`接口让 `TreeMap` 有了对集合中的元素根据键排序的能力。默认是按 key 的升序排序，不过我们也可以指定排序的比较器。



# HashSet 如何检查重复

`HashSet` 会先计算对象的`hashcode`值来判断对象加入的位置，同时也会与其他加入的对象的 `hashcode` 值作比较，如果没有相符的 `hashcode`，`HashSet` 会假设对象没有重复出现。但是如果发现有相同 `hashcode` 值的对象，这时会调用`equals()`方法来检查 `hashcode` 相等的对象是否真的相同。如果两者相同，`HashSet` 就不会让加入操作成功。



# HashMap 的底层实现

JDK1.8 之前 `HashMap` 底层是 **数组和链表** 结合在一起使用也就是 **链表散列**。**HashMap 通过 key 的 hashCode 经过扰动函数处理过后得到 hash 值，然后通过 (n - 1) & hash 判断当前元素存放的位置（这里的 n 指的是数组的长度），如果当前位置存在元素的话，就判断该元素与要存入的元素的 hash 值以及 key 是否相同，如果相同的话，直接覆盖，不相同就通过拉链法解决冲突。**

**所谓扰动函数指的就是 HashMap 的 hash 方法。使用 hash 方法也就是扰动函数是为了防止一些实现比较差的 hashCode() 方法 换句话说使用扰动函数之后可以减少碰撞。**



JDK1.8 之后 相比于之前的版本， JDK1.8 之后在解决哈希冲突时有了较大的变化，当链表长度大于阈值（默认为 8）（将链表转换成红黑树前会判断，如果当前数组的长度小于 64，那么会选择先进行数组扩容，而不是转换为红黑树）时，将链表转化为红黑树，以减少搜索时间。



# HashMap 的长度为什么是 2 的幂次方

核心在于取模运算 比如数组长度n  将其与hash散列值取模 可以将数组的下标控制在hash散列的地址空间内

取模 的除数如果是2的幂次方 则相当于直接进行位运算  位运算可以提高效率  位运算的结果就是2的幂次方



# HashMap 多线程操作导致死循环问题

主要原因在于并发下的 Rehash 会造成元素之间会形成一个循环链表。不过，jdk 1.8 后解决了这个问题，但是还是不建议在多线程下使用 HashMap,因为多线程下使用 HashMap 还是会存在其他问题比如数据丢失。并发环境下推荐使用 ConcurrentHashMap 。



# ConcurrentHashMap 和 Hashtable 的区别

`ConcurrentHashMap` 和 `Hashtable` 的区别主要体现在实现线程安全的方式上不同。

- **底层数据结构：** JDK1.7 的 `ConcurrentHashMap` 底层采用 **分段的数组+链表** 实现，JDK1.8 采用的数据结构跟 `HashMap1.8` 的结构一样，数组+链表/红黑二叉树。`Hashtable` 和 JDK1.8 之前的 `HashMap` 的底层数据结构类似都是采用 **数组+链表** 的形式，数组是 HashMap 的主体，链表则是主要为了解决哈希冲突而存在的；
- **实现线程安全的方式（重要）：** 
  1. **在 JDK1.7 的时候，`ConcurrentHashMap`（分段锁）** 对整个桶数组进行了分割分段(`Segment`)，每一把锁只锁容器其中一部分数据，多线程访问容器里不同数据段的数据，就不会存在锁竞争，提高并发访问率。 **到了 JDK1.8 的时候已经摒弃了 `Segment` 的概念，而是直接用 `Node` 数组+链表+红黑树的数据结构来实现，并发控制使用 `synchronized` 和 CAS 来操作。（JDK1.6 以后 对 `synchronized` 锁做了很多优化）** 整个看起来就像是优化过且线程安全的 `HashMap`，虽然在 JDK1.8 中还能看到 `Segment` 的数据结构，但是已经简化了属性，只是为了兼容旧版本；
  2.  **`Hashtable`(同一把锁)** :使用 `synchronized` 来保证线程安全，效率非常低下。当一个线程访问同步方法时，其他线程也访问同步方法，可能会进入阻塞或轮询状态，如使用 put 添加元素，另一个线程不能使用 put 添加元素，也不能使用 get，竞争会越来越激烈效率越低。



# Collections 工具类

Collections 工具类常用方法:

1. 排序
2. 查找,替换操作
3. 同步控制(不推荐效率低，需要线程安全的集合类型时请考虑使用 JUC 包下的并发集合)





# 集合判空

**判断所有集合内部的元素是否为空，使用 `isEmpty()` 方法，而不是 `size()==0` 的方式。**



这是因为 `isEmpty()` 方法的可读性更好，并且时间复杂度为 O(1)。

绝大部分我们使用的集合的 `size()` 方法的时间复杂度也是 O(1)，不过，也有很多复杂度不是 O(1) 的，比如 `java.util.concurrent` 包下的某些集合（`ConcurrentLinkedQueue` 、`ConcurrentHashMap`...）。



# 集合转 Map

**在使用 `java.util.stream.Collectors` 类的 `toMap()` 方法转为 `Map` 集合时，一定要注意当 value 为 null 时会抛 NPE 异常。**



# 集合遍历

**不要在 foreach 循环里进行元素的 `remove/add` 操作。remove 元素请使用 `Iterator` 方式，如果并发操作，需要对 `Iterator` 对象加锁。**



通过反编译你会发现 foreach 语法底层其实还是依赖 `Iterator` 。不过， `remove/add` 操作直接调用的是集合自己的方法，而不是 `Iterator` 的 `remove/add`方法

这就导致 `Iterator` 莫名其妙地发现自己有元素被 `remove/add` ，然后，它就会抛出一个 `ConcurrentModificationException` 来提示用户发生了并发修改异常。这就是单线程状态下产生的 **fail-fast 机制**。

> **fail-fast 机制** ：多个线程对 fail-fast 集合进行修改的时候，可能会抛出`ConcurrentModificationException`。 即使是单线程下也有可能会出现这种情况，上面已经提到过。

Java8 开始，可以使用 `Collection#removeIf()`方法删除满足特定条件的元素,如

``` java
List<Integer> list = new ArrayList<>();
for (int i = 1; i <= 10; ++i) {
    list.add(i);
}
list.removeIf(filter -> filter % 2 == 0); /* 删除list中的所有偶数 */
System.out.println(list); /* [1, 3, 5, 7, 9] */
```

除了上面介绍的直接使用 `Iterator` 进行遍历操作之外，你还可以：

- 使用普通的 for 循环
- 使用 fail-safe 的集合类。`java.util`包下面的所有的集合类都是 fail-fast 的，而`java.util.concurrent`包下面的所有的类



# 集合去重

**可以利用 `Set` 元素唯一的特性，可以快速对一个集合进行去重操作，避免使用 `List` 的 `contains()` 进行遍历去重或者判断包含操作。**





# 集合转数组

**使用集合转数组的方法，必须使用集合的 `toArray(T[] array)`，还需要指定类型  传入的是类型完全一致、长度为 0 的空数组**



# 数组转集合

**使用工具类 `Arrays.asList()` 把数组转换成集合时，不能使用其修改集合相关的方法， 它的 `add/remove/clear` 方法会抛出 `UnsupportedOperationException` 异常。**



**那我们如何正确的将数组转换为 `ArrayList` ?**

1. 手动实现工具类

```java
//JDK1.5+
static <T> List<T> arrayToList(final T[] array) {
  final List<T> l = new ArrayList<T>(array.length);

  for (final T s : array) {
    l.add(s);
  }
  return l;
}
Integer [] myArray = { 1, 2, 3 };
System.out.println(arrayToList(myArray).getClass());//class java.util.ArrayList
```

2. 最简便的方法

```java
List list = new ArrayList<>(Arrays.asList("a", "b", "c"))
```

3. 使用 Java8 的 `Stream`(推荐)

```java
Integer [] myArray = { 1, 2, 3 };
List myList = Arrays.stream(myArray).collect(Collectors.toList());
//基本类型也可以实现转换（依赖boxed的装箱操作）
int [] myArray2 = { 1, 2, 3 };
List myList = Arrays.stream(myArray2).boxed().collect(Collectors.toList());
```

4. 使用 Guava

对于不可变集合，你可以使用`ImmutableList`类及其`of()`与`copyOf()工厂方法：（参数不能为空）

```java
List<String> il = ImmutableList.of("string", "elements");  // from varargs
List<String> il = ImmutableList.copyOf(aStringArray);      // from array
```

对于可变集合，你可以使用`Lists`类及其`newArrayList()`工厂方法：

```java
List<String> l1 = Lists.newArrayList(anotherListOrCollection);    // from collection
List<String> l2 = Lists.newArrayList(aStringArray);               // from array
List<String> l3 = Lists.newArrayList("or", "string", "elements"); // from varargs
```

5. 使用 Apache Commons Collections

```java
List<String> list = new ArrayList<String>();
CollectionUtils.addAll(list, str);
```

6. 使用 Java9 的 `List.of()`方法

```java
Integer[] array = {1, 2, 3};
List<Integer> list = List.of(array);
```





# IO 流简介

<font style="color:red; font-size:20px">重点词语: </font>

1. IO流  输入和输出流 计算机内存和外部存储之间的过程  
2. 分为字节和字符流
3. InputStream  字节输入流
4. OutPutStream 字节输出流
5. Reader  字符输入流
6. Writer    字符输出流



IO 即 `Input/Output`，输入和输出。数据输入到计算机内存的过程即输入，反之输出到外部存储（比如数据库，文件，远程主机）的过程即输出。数据传输过程类似于水流，因此称为 IO 流。IO 流在 Java 中分为输入流和输出流，而根据数据的处理方式又分为字节流和字符流。

Java IO 流的 40 多个类都是从如下 4 个抽象类基类中派生出来的。

- `InputStream`/`Reader`: 所有的输入流的基类，前者是字节输入流，后者是字符输入流。
- `OutputStream`/`Writer`: 所有输出流的基类，前者是字节输出流，后者是字符输出流。



# 字节流

以字节为单位读取  传输二进制数据  比较快 不用考虑编码问题

+ `InputStream`字节输入流用于从源头（通常是文件）读取数据（字节信息）到内存，一般搭配BufferedImputStream使用
+ `OutputStream`字节输出流用于将数据（字节信息）写入到目的地（通常是文件），



#  字符流

不管是文件读写还是网络发送接收，信息的最小存储单元都是字节。 **那为什么 I/O 流操作要分为字节流操作和字符流操作呢？**

个人认为主要有两点原因：

- 字符流是由 Java 虚拟机将字节转换得到的，这个过程还算是比较耗时。
- 如果我们不知道编码类型就很容易出现乱码问题。

I/O 流就干脆提供了一个直接操作字符的接口，方便我们平时对字符进行流操作。如果音频文件、图片等媒体文件用字节流比较好，如果涉及到字符的话使用字符流比较好。

字符流默认采用的是 `Unicode` 编码，我们可以通过构造方法自定义编码。

顺便分享一下之前遇到的笔试题：常用字符编码所占字节数？`utf8` :英文占 1 字节，中文占 3 字节，`unicode`：任何字符都占 2 个字节，`gbk`：英文占 1 字节，中文占 2 字节。



+ `Reader`字符输入流 用于从源头（通常是文件）读取数据（字符信息）到内存中

+ `Writer`字符输出流用于将数据（字符信息）写入到目的地（通常是文件）



# 字节缓冲流

O 操作是很消耗性能的，缓冲流将数据加载至缓冲区，一次性读取/写入多个字节，从而避免频繁的 IO 操作，提高流的传输效率。

字节缓冲流这里采用了装饰器模式来增强 `InputStream` 和`OutputStream`子类对象的功能。



+ 字节流和字节缓冲流的性能差别主要体现在我们使用两者的时候都是调用 `write(int b)` 和 `read()` 这两个一次只读取一个字节的方法的时候。由于字节缓冲流内部有缓冲区（字节数组），因此，字节缓冲流会先将读取到的字节存放在缓存区，大幅减少 IO 次数，提高读取效率。
+ 如果是调用 `read(byte b[])` 和 `write(byte b[], int off, int len)` 这两个写入一个字节数组的方法的话，只要字节数组的大小合适，两者的性能差距其实不大，基本可以忽略。



## BufferedInputStream（字节缓冲输入流）

`BufferedInputStream` 从源头（通常是文件）读取数据（字节信息）到内存的过程中不会一个字节一个字节的读取，而是会先将读取到的字节存放在缓存区，并从内部缓冲区中单独读取字节。这样大幅减少了 IO 次数，提高了读取效率。

`BufferedInputStream` 内部维护了一个缓冲区，这个缓冲区实际就是一个字节数组，通过阅读 `BufferedInputStream` 源码即可得到这个结论。缓冲区的大小默认为 **8192** 字节，当然了，你也可以通过 `BufferedInputStream(InputStream in, int size)` 这个构造方法来指定缓冲区的大小。



## BufferedOutputStream（字节缓冲输出流）

`BufferedOutputStream` 将数据（字节信息）写入到目的地（通常是文件）的过程中不会一个字节一个字节的写入，而是会先将要写入的字节存放在缓存区，并从内部缓冲区中单独写入字节。这样大幅减少了 IO 次数，提高了读取效率类似于 `BufferedInputStream` ，`BufferedOutputStream` 内部也维护了一个缓冲区，并且，这个缓存区的大小也是 **8192** 字节。



# 字符缓冲流

`BufferedReader` （字符缓冲输入流）和 `BufferedWriter`（字符缓冲输出流）类似于 `BufferedInputStream`（字节缓冲输入流）和`BufferedOutputStream`（字节缓冲输入流），内部都维护了一个字节数组作为缓冲区。不过，前者主要是用来操作字符信息。



# 打印流

``` java
System.out.println("Hello！");
```

`System.out` 实际是用于获取一个 `PrintStream` 对象，`print`方法实际调用的是 `PrintStream` 对象的 `write` 方法。

`PrintStream` 属于字节打印流，与之对应的是 `PrintWriter` （字符打印流）。`PrintStream` 是 `OutputStream` 的子类，`PrintWriter` 是 `Writer` 的子类。



# 随机访问流

随机访问流指的是支持随意跳转到文件的任意位置进行读写的 `RandomAccessFile` 。



`RandomAccessFile` 比较常见的一个应用就是实现大文件的 **断点续传** 。何谓断点续传？简单来说就是上传文件中途暂停或失败（比如遇到网络问题）之后，不需要重新上传，只需要上传那些未成功上传的文件分片即可。分片（先将文件切分成多个文件分片）上传是断点续传的基础。

`RandomAccessFile` 可以帮助我们合并文件分片



# Java IO设计模式总结

1. 装饰器模式

   **装饰器（Decorator）模式** 可以在不改变原有对象的情况下拓展其功能。

   装饰器模式通过组合替代继承来扩展原始类的功能，在一些继承关系比较复杂的场景（IO 这一场景各种类的继承关系就比较复杂）更加实用。

2. 适配器模式

   **适配器（Adapter Pattern）模式** 主要用于接口互不兼容的类的协调工作，你可以将其联想到我们日常经常使用的电源适配器。

   适配器模式中存在被适配的对象或者类称为 **适配者(Adaptee)** ，作用于适配者的对象或者类称为**适配器(Adapter)** 。适配器分为对象适配器和类适配器。类适配器使用继承关系来实现，对象适配器使用组合关系来实现。

3. 工厂模式

   工厂模式用于创建对象，NIO 中大量用到了工厂模式，比如 `Files` 类的 `newInputStream` 方法用于创建 `InputStream` 对象（静态工厂）、 `Paths` 类的 `get` 方法创建 `Path` 对象（静态工厂）、`ZipFileSystem` 类（`sun.nio`包下的类，属于 `java.nio` 相关的一些内部实现）的 `getPath` 的方法创建 `Path` 对象（简单工厂）。

4. 观察者模式

   NIO 中的文件目录监听服务使用到了观察者模式。

   NIO 中的文件目录监听服务基于 `WatchService` 接口和 `Watchable` 接口。`WatchService` 属于观察者，`Watchable` 属于被观察者。

   `Watchable` 接口定义了一个用于将对象注册到 `WatchService`（监控服务） 并绑定监听事件的方法 `register` 。



# 什么是I/O

计算机结构分为 5 大部分：运算器、控制器、存储器、输入设备、输出设备。

![](https://img-blog.csdnimg.cn/20190624122126398.jpeg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9pcy1jbG91ZC5ibG9nLmNzZG4ubmV0,size_16,color_FFFFFF,t_70)

**从计算机结构的视角来看的话， I/O 描述了计算机系统与外部设备之间通信的过程。**

**再从应用程序的角度来解读一下 I/O。**

为了保证操作系统的稳定性和安全性，一个进程的地址空间划分为 **用户空间（User space）** 和 **内核空间（Kernel space ）** 。

像我们平常运行的应用程序都是运行在用户空间，只有内核空间才能进行系统态级别的资源有关的操作，比如文件管理、进程通信、内存管理等等。也就是说，我们想要进行 IO 操作，一定是要依赖内核空间的能力。

用户空间的程序不能直接访问内核空间。

当想要执行 IO 操作时，由于没有执行这些操作的权限，只能发起系统调用请求操作系统帮忙完成。

用户进程想要执行 IO 操作的话，必须通过 **系统调用** 来间接访问内核空间

我们在平常开发过程中接触最多的就是 **磁盘 IO（读写文件）** 和 **网络 IO（网络请求和响应）**。

**从应用程序的视角来看的话，我们的应用程序对操作系统的内核发起 IO 调用（系统调用），操作系统负责的内核执行具体的 IO 操作。也就是说，我们的应用程序实际上只是发起了 IO 操作的调用而已，具体 IO 的执行是由操作系统的内核来完成的。**

当应用程序发起 I/O 调用后，会经历两个步骤：

1. 内核等待 I/O 设备准备好数据
2. 内核将数据从内核空间拷贝到用户空间。



#  有哪些常见的 IO 模型?

UNIX 系统下， IO 模型一共有 5 种： **同步阻塞 I/O**、**同步非阻塞 I/O**、**I/O 多路复用**、**信号驱动 I/O** 和**异步 I/O**。

<img src="https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/6a9e704af49b4380bb686f0c96d33b81~tplv-k3u1fbpfcp-watermark.image" style="zoom:50%;" />

<img src="https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/bb174e22dbe04bb79fe3fc126aed0c61~tplv-k3u1fbpfcp-watermark.image" style="zoom:50%;" />







<img src="https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/88ff862764024c3b8567367df11df6ab~tplv-k3u1fbpfcp-watermark.image" style="zoom:50%;" />



<img src="https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/3077e72a1af049559e81d18205b56fd7~tplv-k3u1fbpfcp-watermark.image" style="zoom:50%;" />







# Java 中 3 种常见 IO 模型

<img src="https://images.xiaozhuanlan.com/photo/2020/33b193457c928ae02217480f994814b6.png" style="zoom:50%;" />

##  BIO (Blocking I/O)

**BIO 属于同步阻塞 IO 模型** 。

同步阻塞 IO 模型中，应用程序发起 read 调用后，会一直阻塞，直到内核把数据拷贝到用户空间。

在客户端连接数量不高的情况下，是没问题的。但是，当面对十万甚至百万级连接的时候，传统的 BIO 模型是无能为力的。因此，我们需要一种更高效的 I/O 处理模型来应对更高的并发量。





## NIO (Non-blocking/New I/O)

Java 中的 NIO 于 Java 1.4 中引入，对应 `java.nio` 包，提供了 `Channel` , `Selector`，`Buffer` 等抽象。NIO 中的 N 可以理解为 Non-blocking，不单纯是 New。它是支持面向缓冲的，基于通道的 I/O 操作方法。 对于高负载、高并发的（网络）应用，应使用 NIO 。

Java 中的 NIO 可以看作是 **I/O 多路复用模型**。也有很多人认为，Java 中的 NIO 属于同步非阻塞 IO 模型。

Java 中的 NIO ，有一个非常重要的**选择器 ( Selector )** 的概念，也可以被称为 **多路复用器**。通过它，只需要一个线程便可以管理多个客户端连接。当客户端数据到了之后，才会为其服务。

<img src="https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/0f483f2437ce4ecdb180134270a00144~tplv-k3u1fbpfcp-watermark.image" style="zoom:50%;" />



## AIO (Asynchronous I/O)

AIO 也就是 NIO 2。Java 7 中引入了 NIO 的改进版 NIO 2,它是异步 IO 模型。

异步 IO 是基于事件和回调机制实现的，也就是应用操作之后会直接返回，不会堵塞在那里，当后台处理完成，操作系统会通知相应的线程进行后续的操作。

目前来说 AIO 的应用还不是很广泛。Netty 之前也尝试使用过 AIO，不过又放弃了。这是因为，Netty 使用了 AIO 之后，在 Linux 系统上的性能并没有多少提升。





# 什么是线程和进程?

## 何为进程?

进程是程序的一次执行过程，是**系统运行程序的基本单位**，因此进程是动态的。系统运行一个程序即是一个进程从创建，运行到消亡的过程。

在 Java 中，当我们启动 main 函数时其实就是启动了一个 JVM 的进程，而 main 函数所在的线程就是这个进程中的一个线程，也称主线程。



#  何为线程?

线程与进程相似，但线程是一个比进程更小的执行单位。一个进程在其执行的过程中可以产生多个线程。与进程不同的是同类的多个线程共享进程的**堆**和**方法区**资源，但每个线程有自己的**程序计数器**、**虚拟机栈**和**本地方法栈**，所以系统在产生一个线程，或是在各个线程之间作切换工作时，负担要比进程小得多，也正因为如此，线程也被称为轻量级进程。



# 请简要描述线程与进程的关系,区别及优缺点？

从JVM来说:

一个进程中可以有多个线程，多个线程共享进程的**堆**和**方法区 (JDK1.8 之后的元空间)资源，但是每个线程有自己的程序计数器**、**虚拟机栈** 和 **本地方法栈**。

**总结：** **线程是进程划分成的更小的运行单位。线程和进程最大的不同在于基本上各进程是独立的，而各线程则不一定，因为同一进程中的线程极有可能会相互影响。线程执行开销小，但不利于资源的管理和保护；而进程正相反。**



#  程序计数器为什么是私有的?

程序计数器主要有下面两个作用：

1. 字节码解释器通过改变程序计数器来依次读取指令，从而实现代码的流程控制，如：顺序执行、选择、循环、异常处理。
2. 在多线程的情况下，程序计数器用于记录当前线程执行的位置，从而当线程被切换回来的时候能够知道该线程上次运行到哪儿了。

需要注意的是，如果执行的是 native 方法，那么程序计数器记录的是 undefined 地址，只有执行的是 Java 代码时程序计数器记录的才是下一条指令的地址。

所以，程序计数器私有主要是为了**线程切换后能恢复到正确的执行位置**。



# 虚拟机栈和本地方法栈为什么是私有的?

- **虚拟机栈：** 每个 Java 方法在执行的同时会创建一个栈帧用于存储局部变量表、操作数栈、常量池引用等信息。从方法调用直至执行完成的过程，就对应着一个栈帧在 Java 虚拟机栈中入栈和出栈的过程。
- **本地方法栈：** 和虚拟机栈所发挥的作用非常相似，区别是： **虚拟机栈为虚拟机执行 Java 方法 （也就是字节码）服务，而本地方法栈则为虚拟机使用到的 Native 方法服务。** 在 HotSpot 虚拟机中和 Java 虚拟机栈合二为一。



#  一句话简单了解堆和方法区

堆和方法区是所有线程共享的资源，其中堆是进程中最大的一块内存，主要用于存放新创建的对象 (几乎所有对象都在这里分配内存)，方法区主要用于存放已被加载的类信息、常量、静态变量、即时编译器编译后的代码等数据。



# 说说并发与并行的区别?

- **并发**：两个及两个以上的作业在同一 **时间段** 内执行。
- **并行**：两个及两个以上的作业在同一 **时刻** 执行。



# 为什么要使用多线程呢?

先从总体上来说：

- **从计算机底层来说：** 线程可以比作是轻量级的进程，是程序执行的最小单位,线程间的切换和调度的成本远远小于进程。另外，多核 CPU 时代意味着多个线程可以同时运行，这减少了线程上下文切换的开销。
- **从当代互联网发展趋势来说：** 现在的系统动不动就要求百万级甚至千万级的并发量，而多线程并发编程正是开发高并发系统的基础，利用好多线程机制可以大大提高系统整体的并发能力以及性能。

再深入到计算机底层来探讨：

- **单核时代**： 在单核时代多线程主要是为了提高单进程利用 CPU 和 IO 系统的效率。 假设只运行了一个 Java 进程的情况，当我们请求 IO 的时候，如果 Java 进程中只有一个线程，此线程被 IO 阻塞则整个进程被阻塞。CPU 和 IO 设备只有一个在运行，那么可以简单地说系统整体效率只有 50%。当使用多线程的时候，一个线程被 IO 阻塞，其他线程还可以继续使用 CPU。从而提高了 Java 进程利用系统资源的整体效率。
- **多核时代**: 多核时代多线程主要是为了提高进程利用多核 CPU 的能力。举个例子：假如我们要计算一个复杂的任务，我们只用一个线程的话，不论系统有几个 CPU 核心，都只会有一个 CPU 核心被利用到。而创建多个线程，这些线程可以被映射到底层多个 CPU 上执行，在任务中的多个线程没有资源竞争的情况下，任务执行的效率会有显著性的提高，约等于（单核时执行时间/CPU 核心数）。



# 使用多线程可能带来什么问题?

并发编程的目的就是为了能提高程序的执行效率提高程序运行速度，但是并发编程并不总是能提高程序运行速度的，而且并发编程可能会遇到很多问题，比如：内存泄漏、死锁、线程不安全等等。



# 线程的生命周期和状态?

Java 线程在运行的生命周期中的指定时刻只可能处于下面 6 种不同状态的其中一个状态

![](https://my-blog-to-use.oss-cn-beijing.aliyuncs.com/19-1-29/Java%E7%BA%BF%E7%A8%8B%E7%9A%84%E7%8A%B6%E6%80%81.png)



![](https://javaguide.cn/assets/java-life-cycle.3c0f5067.png)

由上图可以看出：线程创建之后它将处于 **NEW（新建）** 状态，调用 `start()` 方法后开始运行，线程这时候处于 **READY（可运行）** 状态。可运行状态的线程获得了 CPU 时间片（timeslice）后就处于 **RUNNING（运行）** 状态。

在操作系统中层面线程有 READY 和 RUNNING 状态，而在 JVM 层面只能看到 RUNNABLE 状态，所以 Java 系统一般将这两个状态统称为 **RUNNABLE（运行中）** 状态 。



当线程执行 `wait()`方法之后，线程进入 **WAITING（等待）** 状态。进入等待状态的线程需要依靠其他线程的通知才能够返回到运行状态，而 **TIMED_WAITING(超时等待)** 状态相当于在等待状态的基础上增加了超时限制，比如通过 `sleep（long millis）`方法或 `wait（long millis）`方法可以将 Java 线程置于 TIMED_WAITING 状态。当超时时间到达后 Java 线程将会返回到 RUNNABLE 状态。当线程调用同步方法时，在没有获取到锁的情况下，线程将会进入到 **BLOCKED（阻塞）** 状态。线程在执行 Runnable 的`run()`方法之后将会进入到 **TERMINATED（终止）** 状态。



# 什么是上下文切换?

线程在执行过程中会有自己的运行条件和状态（也称上下文），比如上文所说到过的程序计数器，栈信息等。当出现如下情况的时候，线程会从占用 CPU 状态中退出。

- 主动让出 CPU，比如调用了 `sleep()`, `wait()` 等。
- 时间片用完，因为操作系统要防止一个线程或者进程长时间占用CPU导致其他线程或者进程饿死。
- 调用了阻塞类型的系统中断，比如请求 IO，线程被阻塞。
- 被终止或结束运行

这其中前三种都会发生线程切换，线程切换意味着需要保存当前线程的上下文，留待线程下次占用 CPU 的时候恢复现场。并加载下一个将要占用 CPU 的线程上下文。这就是所谓的 **上下文切换**。

上下文切换是现代操作系统的基本功能，因其每次需要保存信息恢复信息，这将会占用 CPU，内存等系统资源进行处理，也就意味着效率会有一定损耗，如果频繁切换就会造成整体效率低下。



# 什么是线程死锁?

线程死锁描述的是这样一种情况：多个线程同时被阻塞，它们中的一个或者全部都在等待某个资源被释放。

由于线程被无限期地阻塞，因此程序不可能正常终止。如下图所示，线程 A 持有资源 2，线程 B 持有资源 1，他们同时都想申请对方的资源，所以这两个线程就会互相等待而进入死锁状态。

![](https://my-blog-to-use.oss-cn-beijing.aliyuncs.com/2019-4/2019-4%E6%AD%BB%E9%94%811.png)



**产生死锁的四个必要条件：**

1. 互斥条件：该资源任意一个时刻只由一个线程占用。
2. 请求与保持条件：一个线程因请求资源而阻塞时，对已获得的资源保持不放。
3. 不剥夺条件:线程已获得的资源在未使用完之前不能被其他线程强行剥夺，只有自己使用完毕后才释放资源。
4. 循环等待条件:若干线程之间形成一种头尾相接的循环等待资源关系。



# 如何预防和避免线程死锁?

**如何预防死锁？** 破坏死锁的产生的必要条件即可：

1. **破坏请求与保持条件** ：一次性申请所有的资源。
2. **破坏不剥夺条件** ：占用部分资源的线程进一步申请其他资源时，如果申请不到，可以主动释放它占有的资源。
3. **破坏循环等待条件** ：靠按序申请资源来预防。按某一顺序申请资源，释放资源则反序释放。破坏循环等待条件。



> 银行家算法: 
>
> 银行家算法的实质就是**要设法保证系统动态分配资源后不进入不安全状态，以避免可能产生的死锁。**即没当进程提出资源请求且系统的资源能够满足该请求时，系统将判断满足此次资源请求后系统状态是否安全，如果判断结果为安全，则给该进程分配资源，否则不分配资源，申请资源的进程将阻塞。
>
> 银行家算法的执行有个前提条件，即要求进程预先提出自己的最大资源请求，并假设系统拥有固定的资源总量。





# 说说 sleep() 方法和 wait() 方法区别和共同点?

- 两者最主要的区别在于：**`sleep()` 方法没有释放锁，而 `wait()` 方法释放了锁** 。
- 两者都可以暂停线程的执行。
- `wait()` 通常被用于线程间交互/通信，`sleep() `通常被用于暂停执行。
- `wait()` 方法被调用后，线程不会自动苏醒，需要别的线程调用同一个对象上的 `notify() `或者 `notifyAll()` 方法。`sleep() `方法执行完成后，线程会自动苏醒。或者可以使用 `wait(long timeout)` 超时后线程会自动苏醒。





# 调用 start() 时会执行 run() ，为什么不直接调用 run() ？

这是另一个非常经典的 Java 多线程面试问题，而且在面试中会经常被问到。很简单，但是很多人都会答不上来！

new 一个 Thread，线程进入了新建状态。调用 `start()`方法，会启动一个线程并使线程进入了就绪状态，当分配到时间片后就可以开始运行了。 `start()` 会执行线程的相应准备工作，然后自动执行 `run()` 方法的内容，这是真正的多线程工作。 但是，直接执行 `run()` 方法，会把 `run()` 方法当成一个 main 线程下的普通方法去执行，并不会在某个线程中执行它，所以这并不是多线程工作。

**总结： 调用 `start()` 方法方可启动线程并使线程进入就绪状态，直接执行 `run()` 方法的话不会以多线程的方式执行。**



# 讲一下 JMM(Java 内存模型)

Java 内存模型抽象了线程和主内存之间的关系(定制了一些规范)，就比如说线程之间的共享变量必须存储在主内存中。Java 内存模型主要目的是为了屏蔽系统和硬件的差异，避免一套代码在不同的平台下产生的效果不一致。

- **主内存** ：所有线程创建的实例对象都存放在主内存中，不管该实例对象是成员变量还是方法中的本地变量(也称局部变量)
- **本地内存** ：每个线程都有一个私有的本地内存来存储共享变量的副本，并且，每个线程只能访问自己的本地内存，无法访问其他线程的本地内存。本地内存是 JMM 抽象出来的一个概念，存储了主内存中的共享变量副本。

![](https://javaguide.cn/assets/jmm2.81e18ab3.png)

从上图来看，线程 1 与线程 2 之间如果要进行通信的话，必须要经历下面 2 个步骤：

1. 线程 1 把本地内存中修改过的共享变量副本的值同步到主内存中去。
2. 线程 2 到主存中读取对应的共享变量的值。

也就是说，JMM 为共享变量提供了可见性的保障。

关于主内存与工作内存直接的具体交互协议，即一个变量如何从主内存拷贝到工作内存，如何从工作内存同步到主内存之间的实现细节，Java 内存模型定义了八种同步操作



# Java 内存区域和内存模型(JMM)有何区别？

**Java 内存区域和内存模型是完全不一样的两个东西！！！**

- Java 内存区域定义了JVM 在运行时如何分区存储程序数据，就比如说堆主要用于存放对象实例。
- Java 内存模型和 Java 的并发编程相关，抽象了线程和主内存之间的关系就比如说线程之间的共享变量必须存储在主内存中，规定了从 Java 源代码到 CPU 可执行指令的这个转化过程要遵守哪些和并发相关的原则和规范，其主要目的是为了简化多线程编程，增强程序可移植性的。



# volatile

+ 保证变量可见性
+ 防止JVM进行指令的重排序

`volatile` 关键字可以保证变量的可见性，如果我们将变量声明为 **`volatile`** ，这就指示 JVM，这个变量是共享且不稳定的，每次使用它都到主存中进行读取。

**`volatile` 关键字除了可以保证变量的可见性，还有一个重要的作用就是防止 JVM 的指令重排序。**

如果我们将变量声明为 **`volatile`** ，在对这个变量进行读写操作的时候，编译器会通过 **内存屏障** 来禁止指令重排序。

内存屏障（Memory Barrier，或有时叫做内存栅栏，Memory Fence）是一种CPU指令，用于控制特定条件下的重排序和内存可见性问题。Java编译器也会根据内存屏障的规则禁止重排序。

在 Java 中，`Unsafe` 类提供了三个开箱即用的内存屏障相关的方法，屏蔽了操作系统底层的差异：

```java
public native void loadFence();
public native void storeFence();
public native void fullFence();
```

理论上来说，你通过这个三个方法也可以实现和`volatile`禁止重排序一样的效果。



## 指令重排序

为了提升执行速度/性能，计算机在执行程序代码的时候，会对指令就行重排序。

指令重排序**简单来说就是系统在执行代码的时候并不一定是按照你写的代码的顺序依次执行**。

常见的重排序有下面 2 种情况：

- **编译器优化重排** ：编译器（包括 JVM、JIT 编译器等）在不改变单线程程序语义的前提下，重新安排语句的执行顺序。
- **指令并行重排** ：和编译器优化重排类似，

另外，内存系统也会有“重排序”，但有不是真正意义上的重排序。在 JMM 里表现为主存和本地内存，而主存和本地内存的内容可能不一致，所以这也会导致程序表现出乱序的行为。

另外，内存系统也会有“重排序”，但有不是真正意义上的重排序。在 JMM 里表现为主存和本地内存，而主存和本地内存的内容可能不一致，所以这也会导致程序表现出乱序的行为。

**指令重排序可以保证串行语义一致，但是没有义务保证多线程间的语义也一致**。所以在多线程下，指令重排序可能会导致一些问题。



#  synchronized 关键字的了解

`synchronized` 翻译成中文是同步的的意思，主要解决的是多个线程之间访问资源的同步性，可以保证被它修饰的方法或者代码块在任意时刻只能有一个线程执行。

在 Java 早期版本中，`synchronized` 属于 **重量级锁**，效率低下。 因为监视器锁（monitor）是依赖于底层的操作系统的 `Mutex Lock` 来实现的，Java 的线程是映射到操作系统的原生线程之上的。如果要挂起或者唤醒一个线程，都需要操作系统帮忙完成，而**操作系统实现线程之间的切换时需要从用户态转换到内核态**，这个状态之间的转换需要相对比较长的时间，时间成本相对较高。

不过，在 Java 6 之后，Java 官方对从 JVM 层面对 `synchronized` 较大优化，所以现在的 `synchronized` 锁效率也优化得很不错了。JDK1.6 对锁的实现引入了大量的优化，如自旋锁、适应性自旋锁、锁消除、锁粗化、偏向锁、轻量级锁等技术来减少锁操作的开销。所以，你会发现目前的话，不论是各种开源框架还是 JDK 源码都大量使用了 `synchronized` 关键字。



# 如何使用 synchronized 关键字？

synchronized 关键字最主要的三种使用方式：修饰实例方法、修饰静态方法、修饰代码块

### 修饰实例方法（锁当前对象实例）

给当前对象实例加锁，进入同步代码前要获得 **当前对象实例的锁** 。

``` java
synchronized void method(){// 业务代码}
```



### 修饰静态方法(锁当前类)

给当前类加锁，会作用于类的所有对象实例 ，进入同步代码前要获得 **当前 class 的锁**。

这是因为静态成员不属于任何一个实例对象，归整个类所有，不依赖于类的特定实例，被类的所有实例共享。

``` java
synchronized static void method(){}
```

静态 `synchronized` 方法和非静态 `synchronized` 方法之间的调用互斥么？不互斥！如果一个线程 A 调用一个实例对象的非静态 `synchronized` 方法，而线程 B 需要调用这个实例对象所属类的静态 `synchronized` 方法，是允许的，不会发生互斥现象，因为访问静态 `synchronized` 方法占用的锁是当前类的锁，而访问非静态 `synchronized` 方法占用的锁是当前实例对象锁。



### 修饰代码块(锁指定对象/类)

对括号里指定的对象/类加锁：

- `synchronized(object)` 表示进入同步代码库前要获得 **给定对象的锁**。
- `synchronized(类.class)` 表示进入同步代码前要获得 **给定 Class 的锁**

``` java
synchronized(this){// 业务代码}
```

**总结：**

- `synchronized` 关键字加到 `static` 静态方法和 `synchronized(class)` 代码块上都是是给 Class 类上锁；
- `synchronized` 关键字加到实例方法上是给对象实例上锁；
- 尽量不要使用 `synchronized(String a)` 因为 JVM 中，字符串常量池具有缓存功能。



### 面试题 单例实现

``` java
public class Singleton {
    private volatile static Singleton uniqueInstance;
    private Singleton() {}
    public  static Singleton getUniqueInstance() {
       //先判断对象是否已经实例过，没有实例化过才进入加锁代码
        if (uniqueInstance == null) {
            //类对象加锁
            synchronized (Singleton.class) {
                if (uniqueInstance == null) {
                    uniqueInstance = new Singleton();
                }
            }
        }
        return uniqueInstance;
    }
}
```

`uniqueInstance` 采用 `volatile` 关键字修饰也是很有必要的， `uniqueInstance = new Singleton();` 这段代码其实是分为三步执行：

1. 为 `uniqueInstance` 分配内存空间
2. 初始化 `uniqueInstance`
3. 将 `uniqueInstance` 指向分配的内存地址

但是由于 JVM 具有指令重排的特性，执行顺序有可能变成 1->3->2。指令重排在单线程环境下不会出现问题，但是在多线程环境下会导致一个线程获得还没有初始化的实例。例如，线程 T1 执行了 1 和 3，此时 T2 调用 `getUniqueInstance`() 后发现 `uniqueInstance` 不为空，因此返回 `uniqueInstance`，但此时 `uniqueInstance` 还未被初始化。

使用 `volatile` 可以禁止 JVM 的指令重排，保证在多线程环境下也能正常运行。



# 构造方法可以使用 synchronized 关键字修饰么？

先说结论：**构造方法不能使用 synchronized 关键字修饰。**

构造方法本身就属于线程安全的，不存在同步的构造方法一说。



# 讲一下 synchronized 关键字的底层原理

<font style="color:red; font-size:20px">重点词语: </font>

`synchronized` 同步语句块的实现使用的是 `monitorenter` 和 `monitorexit` 指令，其中 `monitorenter` 指令指向同步代码块的开始位置，`monitorexit` 指令则指明同步代码块的结束位置。

`synchronized` 修饰的方法并没有 `monitorenter` 指令和 `monitorexit` 指令，取得代之的确实是 `ACC_SYNCHRONIZED` 标识，该标识指明了该方法是一个同步方法。



## synchronized 同步语句块的情况

**`synchronized` 同步语句块的实现使用的是 `monitorenter` 和 `monitorexit` 指令，其中 `monitorenter` 指令指向同步代码块的开始位置，`monitorexit` 指令则指明同步代码块的结束位置。**

当执行 `monitorenter` 指令时，线程试图获取锁也就是获取 **对象监视器 `monitor`** 的持有权。



在 Java 虚拟机(HotSpot)中，Monitor 是基于 C++实现的，由`ObjectMonitor`实现的。每个对象中都内置了一个 `ObjectMonitor`对象。

另外，`wait/notify`等方法也依赖于`monitor`对象，这就是为什么只有在同步的块或者方法中才能调用`wait/notify`等方法，否则会抛出`java.lang.IllegalMonitorStateException`的异常的原因。



## synchronized 修饰方法的的情况

`synchronized` 修饰的方法并没有 `monitorenter` 指令和 `monitorexit` 指令，取得代之的确实是 `ACC_SYNCHRONIZED` 标识，该标识指明了该方法是一个同步方法。JVM 通过该 `ACC_SYNCHRONIZED` 访问标志来辨别一个方法是否声明为同步方法，从而执行相应的同步调用。



# JDK1.6 之后的 synchronized 关键字底层做了哪些优化？

JDK1.6 对锁的实现引入了大量的优化，如偏向锁、轻量级锁、自旋锁、适应性自旋锁、锁消除、锁粗化等技术来减少锁操作的开销。

锁主要存在四种状态，依次是：无锁状态、偏向锁状态、轻量级锁状态、重量级锁状态，他们会随着竞争的激烈而逐渐升级。注意锁可以升级不可降级，这种策略是为了提高获得锁和释放锁的效率



# synchronized 和 volatile 的区别？

`synchronized` 关键字和 `volatile` 关键字是两个互补的存在，而不是对立的存在！

- `volatile` 关键字是线程同步的轻量级实现，所以 `volatile `性能肯定比`synchronized`关键字要好 。但是 `volatile` 关键字只能用于变量而 `synchronized` 关键字可以修饰方法以及代码块 。
- `volatile` 关键字能保证数据的可见性，但不能保证数据的原子性。`synchronized` 关键字两者都能保证。
- `volatile`关键字主要用于解决变量在多个线程之间的可见性，而 `synchronized` 关键字解决的是多个线程之间访问资源的同步性。



# synchronized 和 ReentrantLock 的区别



## 两者都是可重入锁

**“可重入锁”** 指的是自己可以再次获取自己的内部锁。比如一个线程获得了某个对象的锁，此时这个对象锁还没有释放，当其再次想要获取这个对象的锁的时候还是可以获取的，如果是不可重入锁的话，就会造成死锁。同一个线程每次获取锁，锁的计数器都自增 1，所以要等到锁的计数器下降为 0 时才能释放锁。



## synchronized 依赖于 JVM 而 ReentrantLock 依赖于 API

`synchronized` 是依赖于 JVM 实现的，前面我们也讲到了 虚拟机团队在 JDK1.6 为 `synchronized` 关键字进行了很多优化，但是这些优化都是在虚拟机层面实现的，并没有直接暴露给我们。`ReentrantLock` 是 JDK 层面实现的（也就是 API 层面，需要 lock() 和 unlock() 方法配合 try/finally 语句块来完成），所以我们可以通过查看它的源代码，来看它是如何实现的。



## ReentrantLock 比 synchronized 增加了一些高级功能

相比`synchronized`，`ReentrantLock`增加了一些高级功能。主要来说主要有三点：

- **等待可中断** : `ReentrantLock`提供了一种能够中断等待锁的线程的机制，通过 `lock.lockInterruptibly()` 来实现这个机制。也就是说正在等待的线程可以选择放弃等待，改为处理其他事情。
- **可实现公平锁** : `ReentrantLock`可以指定是公平锁还是非公平锁。而`synchronized`只能是非公平锁。所谓的公平锁就是先等待的线程先获得锁。`ReentrantLock`默认情况是非公平的，可以通过 `ReentrantLock`类的`ReentrantLock(boolean fair)`构造方法来制定是否是公平的。
- **可实现选择性通知（锁可以绑定多个条件）**: `synchronized`关键字与`wait()`和`notify()`/`notifyAll()`方法相结合可以实现等待/通知机制。`ReentrantLock`类当然也可以实现，但是需要借助于`Condition`接口与`newCondition()`方法。





# 并发编程的三个重要特性

1. **原子性** : 一次操作或者多次操作，要么所有的操作全部都得到执行并且不会受到任何因素的干扰而中断，要么都不执行。`synchronized` 可以保证代码片段的原子性。
2. **可见性** ：当一个线程对共享变量进行了修改，那么另外的线程都是立即可以看到修改后的最新值。`volatile` 关键字可以保证共享变量的可见性。
3. **有序性** ：代码在执行的过程中的先后顺序，Java 在编译器以及运行期间的优化，代码的执行顺序未必就是编写代码时候的顺序。`volatile` 关键字可以禁止指令进行重排序优化。



# ThreadLocal 简介

通常情况下，我们创建的变量是可以被任何一个线程访问并修改的。**如果想实现每一个线程都有自己的专属本地变量该如何解决呢？** JDK 中提供的`ThreadLocal`类正是为了解决这样的问题。 **`ThreadLocal`类主要解决的就是让每个线程绑定自己的值，可以将`ThreadLocal`类形象的比喻成存放数据的盒子，盒子中可以存储每个线程的私有数据。**

**如果你创建了一个`ThreadLocal`变量，那么访问这个变量的每个线程都会有这个变量的本地副本，这也是`ThreadLocal`变量名的由来。他们可以使用 `get（）` 和 `set（）` 方法来获取默认值或将其值更改为当前线程所存的副本的值，从而避免了线程安全问题。**

再举个简单的例子：

比如有两个人去宝屋收集宝物，这两个共用一个袋子的话肯定会产生争执，但是给他们两个人每个人分配一个袋子的话就不会出现这样的问题。如果把这两个人比作线程的话，那么 ThreadLocal 就是用来避免这两个线程竞争的。



# ThreadLocal 原理

**最终的变量是放在了当前线程的 `ThreadLocalMap` 中，并不是存在 `ThreadLocal` 上，`ThreadLocal` 可以理解为只是`ThreadLocalMap`的封装，传递了变量值。** `ThrealLocal` 类中可以通过`Thread.currentThread()`获取到当前线程对象后，直接通过`getMap(Thread t)`可以访问到该线程的`ThreadLocalMap`对象。

**每个`Thread`中都具备一个`ThreadLocalMap`，而`ThreadLocalMap`可以存储以`ThreadLocal`为 key ，Object 对象为 value 的键值对。**



> 从`Thread`类 源代码可以看出`Thread` 类中有一个 `threadLocals` 和 一个 `inheritableThreadLocals` 变量，它们都是 `ThreadLocalMap` 类型的变量,我们可以把 `ThreadLocalMap` 理解为`ThreadLocal` 类实现的定制化的 `HashMap`。默认情况下这两个变量都是 null，只有当前线程调用 `ThreadLocal` 类的 `set`或`get`方法时才创建它们，实际上调用这两个方法的时候，我们调用的是`ThreadLocalMap`类对应的 `get()`、`set()`方法。



# 为什么要用线程池？

> 池化技术想必大家已经屡见不鲜了，线程池、数据库连接池、Http 连接池等等都是对这个思想的应用。池化技术的思想主要是为了减少每次获取资源的消耗，提高对资源的利用率。

**线程池**提供了一种限制和管理资源（包括执行一个任务）的方式。 每个**线程池**还维护一些基本统计信息，例如已完成任务的数量。 

**好处:**

- **降低资源消耗**。通过重复利用已创建的线程降低线程创建和销毁造成的消耗。

- **提高响应速度**。当任务到达时，任务可以不需要等到线程创建就能立即执行。

- **提高线程的可管理性**。线程是稀缺资源，如果无限制的创建，不仅会消耗系统资源，还会降低系统的稳定性，使用线程池可以进行统一的分配，调优和监控。

  

# 实现 Runnable 接口和 Callable 接口的区别

`Runnable`自 Java 1.0 以来一直存在，但`Callable`仅在 Java 1.5 中引入,目的就是为了来处理`Runnable`不支持的用例。**`Runnable` 接口** 不会返回结果或抛出检查异常，但是 **`Callable` 接口** 可以。所以，如果任务不需要返回结果或抛出异常推荐使用 **`Runnable` 接口** ，这样代码看起来会更加简洁。

工具类 `Executors` 可以实现将 `Runnable` 对象转换成 `Callable` 对象。（`Executors.callable(Runnable task)` 或 `Executors.callable(Runnable task, Object result)`）。



# 执行 execute()方法和 submit()方法的区别是什么呢？

1. **`execute()`方法用于提交不需要返回值的任务，所以无法判断任务是否被线程池执行成功与否；**
2. **`submit()`方法用于提交需要返回值的任务。线程池会返回一个 `Future` 类型的对象，通过这个 `Future` 对象可以判断任务是否执行成功**，并且可以通过 `Future` 的 `get()`方法来获取返回值，`get()`方法会阻塞当前线程直到任务完成，而使用 `get(long timeout，TimeUnit unit)`方法则会阻塞当前线程一段时间后立即返回，这时候有可能任务没有执行完。



# Executor 框架

## 简介

`Executor` 框架是 Java5 之后引进的，在 Java 5 之后，通过 `Executor` 来启动线程比使用 `Thread` 的 `start` 方法更好，除了更易管理，效率更好（用线程池实现，节约开销）外，还有关键的一点：有助于避免 this 逃逸问题。

`Executor` 框架不仅包括了线程池的管理，还提供了线程工厂、队列以及拒绝策略等，`Executor` 框架让并发编程变得更加简单。



## Executor 框架结构(主要由三大部分组成)



### 任务(`Runnable` /`Callable`)

执行任务需要实现的 **`Runnable` 接口** 或 **`Callable`接口**。**`Runnable` 接口**或 **`Callable` 接口** 实现类都可以被 **`ThreadPoolExecutor`** 或 **`ScheduledThreadPoolExecutor`** 执行。



### 任务的执行(`Executor`)

<img src="https://javaguide.cn/assets/%E4%BB%BB%E5%8A%A1%E7%9A%84%E6%89%A7%E8%A1%8C%E7%9B%B8%E5%85%B3%E6%8E%A5%E5%8F%A3.27457eb8.png" style="zoom:75%;" />

如图所示，包括任务执行机制的核心接口 **`Executor`** ，以及继承自 `Executor` 接口的 **`ExecutorService` 接口。`ThreadPoolExecutor`** 和 **`ScheduledThreadPoolExecutor`** 这两个关键类实现了 **ExecutorService 接口**。

**这里提了很多底层的类关系，但是，实际上我们需要更多关注的是 `ThreadPoolExecutor` 这个类，这个类在我们实际使用线程池的过程中，使用频率还是非常高的。**



### 异步计算的结果(`Future`)

**`Future`** 接口以及 `Future` 接口的实现类 **`FutureTask`** 类都可以代表异步计算的结果。

当我们把 **`Runnable`接口** 或 **`Callable` 接口** 的实现类提交给 **`ThreadPoolExecutor`** 或 **`ScheduledThreadPoolExecutor`** 执行。（调用 `submit()` 方法时会返回一个 **`FutureTask`** 对象）



## Executor 框架的使用示意图

![](https://javaguide.cn/assets/Executor%E6%A1%86%E6%9E%B6%E7%9A%84%E4%BD%BF%E7%94%A8%E7%A4%BA%E6%84%8F%E5%9B%BE.36e59afa.png)

1. **主线程首先要创建实现 `Runnable` 或者 `Callable` 接口的任务对象。**
2. **把创建完成的实现 `Runnable`/`Callable`接口的 对象直接交给 `ExecutorService` 执行**: `ExecutorService.execute（Runnable command）`）或者也可以把 `Runnable` 对象或`Callable` 对象提交给 `ExecutorService` 执行（`ExecutorService.submit（Runnable task）`或 `ExecutorService.submit（Callable <T> task）`）。
3. **如果执行 `ExecutorService.submit（…）`，`ExecutorService` 将返回一个实现`Future`接口的对象**（我们刚刚也提到过了执行 `execute()`方法和 `submit()`方法的区别，`submit()`会返回一个 `FutureTask 对象）。由于 FutureTask` 实现了 `Runnable`，我们也可以创建 `FutureTask`，然后直接交给 `ExecutorService` 执行。
4. **最后，主线程可以执行 `FutureTask.get()`方法来等待任务执行完成。主线程也可以执行 `FutureTask.cancel（boolean mayInterruptIfRunning）`来取消此任务的执行。**



# ThreadPoolExecutor 类简单介绍



## ThreadPoolExecutor 类分析

`ThreadPoolExecutor` 类中提供的四个构造方法。我们来看最长的那个，其余三个都是在这个构造方法的基础上产生（其他几个构造方法说白点都是给定某些默认参数的构造方法比如默认制定拒绝策略是什么）。

``` java
/**
* 用给定的初始参数创建一个新的ThreadPoolExecutor。
*/
public ThreadPoolExecutor(
  int corePoolSize,//线程池的核心线程数量
  int maximumPoolSize,//线程池的最大线程数
  long keepAliveTime,//当线程数大于核心线程数时，多余的空闲线程存活的最长时间
  TimeUnit unit,//时间单位
  BlockingQueue<Runnable> workQueue,//任务队列，用来储存等待执行任务的队列
  ThreadFactory threadFactory,//线程工厂，用来创建线程，一般默认即可
  RejectedExecutionHandler handler//拒绝策略，当提交的任务过多而不能及时处理时，我们可以定制策略来处理任务
) {}
```

**ThreadPoolExecutor3 个最重要的参数：**

- **`corePoolSize` :** 核心线程数线程数定义了最小可以同时运行的线程数量。

  <font style="font-size:12px">这就好比古代大户人家，会长期雇佣一些“长工”来给他们干活，这些人一般比较稳定，无论这一年的活多活少，这些人都不会被辞退，都是长期生活在大户人家的。</font>

- **`maximumPoolSize` :** 当队列中存放的任务达到队列容量的时候，当前可以同时运行的线程数量变为最大线程数。

  <font style="font-size:12px">这是古代大户人家最多可以雇佣的人数，比如某个节日或大户人家有人过寿时，因为活太多，仅靠“长工”是完不成任务，这时就会再招聘一些“短工”一起来干活，这个最大线程数就是“长工”+“短工”的总人数，也就是招聘的人数不能超过 maximumPoolSize。</font>

- **`workQueue`:**  阻塞队列： 当新任务来的时候会先判断当前运行的线程数量是否达到核心线程数，如果达到的话，新任务就会被存放在队列中。

  

`ThreadPoolExecutor`其他常见参数 :

1. **`keepAliveTime`**:当线程池中的线程数量大于 `corePoolSize` 的时候，如果这时没有新的任务提交，核心线程外的线程不会立即销毁，而是会等待，直到等待的时间超过了 `keepAliveTime`才会被回收销毁；

   <font style="font-size:12px">还是以大户人家为例，当大户人家比较忙的时候就会雇佣一些“短工”来干活，但等干完活之后，不忙了，就会将这些“短工”辞退掉，而 keepAliveTime 就是用来描述没活之后，短工可以在大户人家待的（最长）时间。</font>

2. **`unit`** : `keepAliveTime` 参数的时间单位。

   <font style="font-size:12px">参数 keepAliveTime 是一个 long 类型的值，比如参数 3 传递的是 1，那么这个 1 表示的是 1 天？还是 1 小时？还是 1 秒钟？是由参数 4 说了算的。
   TimeUnit 有7 个值</font>

3. **`threadFactory`** :executor 创建新线程的时候会用到。

4. **`handler`** :饱和策略。关于饱和策略下面单独介绍一下。

**`ThreadPoolExecutor` 饱和策略定义:**

如果当前同时运行的线程数量达到最大线程数量并且队列也已经被放满了任务时，`ThreadPoolTaskExecutor` 定义一些策略:

- **`ThreadPoolExecutor.AbortPolicy`** ：抛出 `RejectedExecutionException`来拒绝新任务的处理。
- **`ThreadPoolExecutor.CallerRunsPolicy`** ：调用执行自己的线程运行任务，也就是直接在调用`execute`方法的线程中运行(`run`)被拒绝的任务，如果执行程序已关闭，则会丢弃该任务。因此这种策略会降低对于新任务提交速度，影响程序的整体性能。如果您的应用程序可以承受此延迟并且你要求任何一个任务请求都要被执行的话，你可以选择这个策略。
- **`ThreadPoolExecutor.DiscardPolicy`** ：不处理新任务，直接丢弃掉。
- **`ThreadPoolExecutor.DiscardOldestPolicy`** ： 此策略将丢弃最早的未处理的任务请求。





## 如何创建线程池

《阿里巴巴 Java 开发手册》中强制线程池不允许使用 Executors 去创建，而是通过 ThreadPoolExecutor 的方式，这样的处理方式让写的同学更加明确线程池的运行规则，规避资源耗尽的风险

## 方式一：通过构造方法实现

![](https://my-blog-to-use.oss-cn-beijing.aliyuncs.com/2019-6/ThreadPoolExecutor%E6%9E%84%E9%80%A0%E6%96%B9%E6%B3%95.png)

## 方式二：通过 Executor 框架的工具类 Executors 来实现

我们可以创建三种类型的 ThreadPoolExecutor：

- **FixedThreadPool** ： 该方法返回一个固定线程数量的线程池。该线程池中的线程数量始终不变。当有一个新的任务提交时，线程池中若有空闲线程，则立即执行。若没有，则新的任务会被暂存在一个任务队列中，待有线程空闲时，便处理在任务队列中的任务。
- **SingleThreadExecutor：** 方法返回一个只有一个线程的线程池。若大于一个任务被提交到该线程池，任务会被保存在一个任务队列中，待线程空闲，按先入先出的顺序执行队列中的任务。
- **CachedThreadPool：** 该方法返回一个可根据实际情况调整线程数量的线程池。线程池的线程数量不确定，但若有空闲线程可以复用，则会优先使用可复用的线程。若所有线程均在工作，又有新的任务提交，则会创建新的线程处理任务。所有线程在当前任务执行完毕后，将返回线程池进行复用。



## 构造方式代码实例

> 我们在代码中模拟了 10 个任务，我们配置的核心线程数为 5 、等待队列容量为 100 ，所以每次只可能存在 5 个任务同时执行，剩下的 5 个任务会被放到等待队列中去。当前的5个任务中如果有任务被执行完了，线程池就会去拿新的任务执行。

``` java
// ----------------------------
    public class MyRunnable implements Runnable {
        private String command;
        public MyRunnable(String s) {
            this.command = s;
        }
        @Override
        public void run() {
            System.out.println(Thread.currentThread().getName() + " Start. Time = " + new Date());
            processCommand();
            System.out.println(Thread.currentThread().getName() + " End. Time = " + new Date());
        }
        private void processCommand() {
            try {
                Thread.sleep(5000);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
        @Override
        public String toString() {
            return this.command;
        }
    }

// ----------------------------
public class ThreadPoolExecutorDemo {
    private static final int CORE_POOL_SIZE = 5;
    private static final int MAX_POOL_SIZE = 10;
    private static final int QUEUE_CAPACITY = 100;
    private static final Long KEEP_ALIVE_TIME = 1L;
    public static void main(String[] args) {
        //使用阿里巴巴推荐的创建线程池的方式
        //通过ThreadPoolExecutor构造函数自定义参数创建
        ThreadPoolExecutor executor = new ThreadPoolExecutor(
                CORE_POOL_SIZE,
                MAX_POOL_SIZE,
                KEEP_ALIVE_TIME,
                TimeUnit.SECONDS,
                new ArrayBlockingQueue<>(QUEUE_CAPACITY),
                new ThreadPoolExecutor.CallerRunsPolicy());
        for (int i = 0; i < 10; i++) {
            //创建WorkerThread对象（WorkerThread类实现了Runnable 接口）
            Runnable worker = new MyRunnable("" + i);
            //执行Runnable
            executor.execute(worker);
        }
        //终止线程池
        executor.shutdown();
        while (!executor.isTerminated()) {
        }
        System.out.println("Finished all threads");
    }
}
```



## 工具类 Executors实现

### FixedThreadPool

`FixedThreadPool` 被称为可重用固定线程数的线程池。通过 Executors 类中的相关源代码来看一下相关实现：

``` java
/**
     * 创建一个可重用固定数量线程的线程池
     */
public static ExecutorService newFixedThreadPool(int nThreads, ThreadFactory threadFactory) {
  return new ThreadPoolExecutor(nThreads, nThreads,
                                0L, TimeUnit.MILLISECONDS,
                                new LinkedBlockingQueue<Runnable>(),
                                threadFactory);
}
```

 为什么不推荐使用`FixedThreadPool`？

**`FixedThreadPool` 使用无界队列 `LinkedBlockingQueue`（队列的容量为 Integer.MAX_VALUE）作为线程池的工作队列会对线程池带来如下影响 ：**

1. 当线程池中的线程数达到 `corePoolSize` 后，新任务将在无界队列中等待，因此线程池中的线程数不会超过 corePoolSize；
2. 由于使用无界队列时 `maximumPoolSize` 将是一个无效参数，因为不可能存在任务队列满的情况。所以，通过创建 `FixedThreadPool`的源码可以看出创建的 `FixedThreadPool` 的 `corePoolSize` 和 `maximumPoolSize` 被设置为同一个值。
3. 由于 1 和 2，使用无界队列时 `keepAliveTime` 将是一个无效参数；
4. 运行中的 `FixedThreadPool`（未执行 `shutdown()`或 `shutdownNow()`）不会拒绝任务，在任务比较多的时候会导致 OOM（内存溢出）。



### SingleThreadExecutor

`SingleThreadExecutor` 方法返回一个只有一个线程的线程池。若多余一个任务被提交到该线程池，任务会被保存在一个任务队列中，待线程空闲，按先入先出的顺序执行队列中的任务。

``` java
/**
     *返回只有一个线程的线程池
     */
public static ExecutorService newSingleThreadExecutor(ThreadFactory threadFactory) {
  return new FinalizableDelegatedExecutorService
    (new ThreadPoolExecutor(1, 1,
                            0L, TimeUnit.MILLISECONDS,
                            new LinkedBlockingQueue<Runnable>(),
                            threadFactory));
}
// --------------------------------
public static ExecutorService newSingleThreadExecutor() {
  return new FinalizableDelegatedExecutorService
    (new ThreadPoolExecutor(1, 1,
                            0L, TimeUnit.MILLISECONDS,
                            new LinkedBlockingQueue<Runnable>()));
}
```

从上面源代码可以看出新创建的 `SingleThreadExecutor` 的 `corePoolSize` 和 `maximumPoolSize` 都被设置为 1.其他参数和 `FixedThreadPool` 相同。

1. 如果当前运行的线程数少于 `corePoolSize`，则创建一个新的线程执行任务；
2. 当前线程池中有一个运行的线程后，将任务加入 `LinkedBlockingQueue`
3. 线程执行完当前的任务后，会在循环中反复从`LinkedBlockingQueue` 中获取任务来执行；

**为什么不推荐使用`SingleThreadExecutor`？**

`SingleThreadExecutor` 使用无界队列 `LinkedBlockingQueue` 作为线程池的工作队列（队列的容量为 Intger.MAX_VALUE）。`SingleThreadExecutor` 使用无界队列作为线程池的工作队列会对线程池带来的影响与 `FixedThreadPool` 相同。说简单点就是可能会导致 OOM，



### CachedThreadPool

该方法返回一个可根据实际情况调整线程数量的线程池。线程池的线程数量不确定，但若有空闲线程可以复用，则会优先使用可复用的线程。若所有线程均在工作，又有新的任务提交，则会创建新的线程处理任务。所有线程在当前任务执行完毕后，将返回线程池进行复用。

``` java
/**
     * 创建一个线程池，根据需要创建新线程，但会在先前构建的线程可用时重用它。
     */
public static ExecutorService newCachedThreadPool(ThreadFactory threadFactory) {
  return new ThreadPoolExecutor(0, Integer.MAX_VALUE,
                                60L, TimeUnit.SECONDS,
                                new SynchronousQueue<Runnable>(),
                                threadFactory);
}
// --------------------------------
public static ExecutorService newCachedThreadPool() {
  return new ThreadPoolExecutor(0, Integer.MAX_VALUE,
                                60L, TimeUnit.SECONDS,
                                new SynchronousQueue<Runnable>());
}
```

`CachedThreadPool` 的`corePoolSize` 被设置为空（0），`maximumPoolSize`被设置为 `Integer.MAX.VALUE`，即它是无界的，这也就意味着如果主线程提交任务的速度高于 `maximumPool` 中线程处理任务的速度时，`CachedThreadPool` 会不断创建新的线程。极端情况下，这样会导致耗尽 cpu 和内存资源。

1. 首先执行 `SynchronousQueue.offer(Runnable task)` 提交任务到任务队列。如果当前 `maximumPool` 中有闲线程正在执行 `SynchronousQueue.poll(keepAliveTime,TimeUnit.NANOSECONDS)`，那么主线程执行 offer 操作与空闲线程执行的 `poll` 操作配对成功，主线程把任务交给空闲线程执行，`execute()`方法执行完成，否则执行下面的步骤 2；
2. 当初始 `maximumPool` 为空，或者 `maximumPool` 中没有空闲线程时，将没有线程执行 `SynchronousQueue.poll(keepAliveTime,TimeUnit.NANOSECONDS)`。这种情况下，步骤 1 将失败，此时 `CachedThreadPool` 会创建新线程执行任务，execute 方法执行完成；

**为什么不推荐使用`CachedThreadPool`？**

`CachedThreadPool`允许创建的线程数量为 `Integer.MAX_VALUE` ，可能会创建大量线程，从而导致 OOM。



# 线程池大小确定

**线程池数量的确定一直是困扰着程序员的一个难题，大部分程序员在设定线程池大小的时候就是随心而定。**

很多人甚至可能都会觉得把线程池配置过大一点比较好！我觉得这明显是有问题的。就拿我们生活中非常常见的一例子来说：**并不是人多就能把事情做好，增加了沟通交流成本。你本来一件事情只需要 3 个人做，你硬是拉来了 6 个人，会提升做事效率嘛？我想并不会。** 线程数量过多的影响也是和我们分配多少人做事情一样，对于多线程这个场景来说主要是增加了**上下文切换**成本。

有一个简单并且适用面比较广的公式：

- **CPU 密集型任务(N+1)：** 这种任务消耗的主要是 CPU 资源，可以将线程数设置为 N（CPU 核心数）+1，比 CPU 核心数多出来的一个线程是为了防止线程偶发的缺页中断，或者其它原因导致的任务暂停而带来的影响。一旦任务暂停，CPU 就会处于空闲状态，而在这种情况下多出来的一个线程就可以充分利用 CPU 的空闲时间。
- **I/O 密集型任务(2N)：** 这种任务应用起来，系统会用大部分的时间来处理 I/O 交互，而线程在处理 I/O 的时间段内不会占用 CPU 来处理，这时就可以将 CPU 交出给其它线程使用。因此在 I/O 密集型任务的应用中，我们可以多配置一些线程，具体的计算方法是 2N。

**如何判断是 CPU 密集任务还是 IO 密集任务？**

CPU 密集型简单理解就是利用 CPU 计算能力的任务比如你在内存中对大量数据进行排序。但凡涉及到网络读取，文件读取这类都是 IO 密集型，这类任务的特点是 CPU 计算耗费时间相比于等待 IO 操作完成的时间来说很少，大部分时间都花在了等待 IO 操作完成上。



## 几个常见的对比

### `Runnable` vs `Callable`

`Runnable`自 Java 1.0 以来一直存在，但`Callable`仅在 Java 1.5 中引入,目的就是为了来处理`Runnable`不支持的用例。**`Runnable` 接口**不会返回结果或抛出检查异常，但是 **`Callable` 接口**可以。所以，如果任务不需要返回结果或抛出异常推荐使用 **`Runnable` 接口**，这样代码看起来会更加简洁。

工具类 `Executors` 可以实现将 `Runnable` 对象转换成 `Callable` 对象。（`Executors.callable(Runnable task)` 或 `Executors.callable(Runnable task, Object result)`）。



### `execute()` vs `submit()`

- `execute()`方法用于提交不需要返回值的任务，所以无法判断任务是否被线程池执行成功与否；
- `submit()`方法用于提交需要返回值的任务。线程池会返回一个 `Future` 类型的对象，通过这个 `Future` 对象可以判断任务是否执行成功，并且可以通过 `Future` 的 `get()`方法来获取返回值，`get()`方法会阻塞当前线程直到任务完成，而使用 `get（long timeout，TimeUnit unit）`方法的话，如果在 `timeout` 时间内任务还没有执行完，就会抛出 `java.util.concurrent.TimeoutException`。



### `shutdown()`VS`shutdownNow()`

- **`shutdown（）`** :关闭线程池，线程池的状态变为 `SHUTDOWN`。线程池不再接受新任务了，但是队列里的任务得执行完毕。
- **`shutdownNow（）`** :关闭线程池，线程的状态变为 `STOP`。线程池会终止当前正在运行的任务，并停止处理排队的任务并返回正在等待执行的 List。



### `isTerminated()` VS `isShutdown()`

- **`isShutDown`** 当调用 `shutdown()` 方法后返回为 true。
- **`isTerminated`** 当调用 `shutdown()` 方法后，并且所有提交的任务完成后返回为 true



# 线程池在实际项目的使用场景

**线程池一般用于执行多个不相关联的耗时任务，没有多线程的情况下，任务顺序执行，使用了线程池的话可让多个不相关联的任务同时执行。**



# 线程池最佳实践

1. 使用 `ThreadPoolExecutor` 的构造函数声明线程池

2. 监测线程池运行状态

3. 建议不同类别的业务用不同的线程池

4. 别忘记给线程池命名

5. 正确配置线程池参数



# Java 常见并发容器总结

JDK 提供的这些容器大部分在 `java.util.concurrent` 包中。

- **`ConcurrentHashMap`** : 线程安全的 `HashMap`
- **`CopyOnWriteArrayList`** : 线程安全的 `List`，在读多写少的场合性能非常好，远远好于 `Vector`。
- **`ConcurrentLinkedQueue`** : 高效的并发队列，使用链表实现。可以看做一个线程安全的 `LinkedList`，这是一个非阻塞队列。
- **`BlockingQueue`** : 这是一个接口，JDK 内部通过链表、数组等方式实现了这个接口。表示阻塞队列，非常适合用于作为数据共享的通道。
- **`ConcurrentSkipListMap`** : 跳表的实现。这是一个 Map，使用跳表的数据结构进行快速查找。



## ConcurrentHashMap

我们知道 `HashMap` 不是线程安全的，在并发场景下如果要保证一种可行的方式是使用 `Collections.synchronizedMap()` 方法来包装我们的 `HashMap`。但这是通过使用一个全局的锁来同步不同线程间的并发访问，因此会带来不可忽视的性能问题。

所以就有了 `HashMap` 的线程安全版本—— `ConcurrentHashMap` 的诞生。

在 `ConcurrentHashMap` 中，无论是读操作还是写操作都能保证很高的性能：在进行读操作时(几乎)不需要加锁，而在写操作时通过锁分段技术只对所操作的段加锁而不影响客户端对其它段的访问。



# CopyOnWriteArrayList

在很多应用场景中，读操作可能会远远大于写操作。由于读操作根本不会修改原有的数据，因此对于每次读取都进行加锁其实是一种资源浪费。我们应该允许多个线程同时访问 `List` 的内部数据，毕竟读取操作是安全的。

这和我们之前在多线程章节讲过 `ReentrantReadWriteLock` 读写锁的思想非常类似，也就是读读共享、写写互斥、读写互斥、写读互斥。JDK 中提供了 `CopyOnWriteArrayList` 类比相比于在读写锁的思想又更进一步。为了将读取的性能发挥到极致，`CopyOnWriteArrayList` 读取是完全不用加锁的，并且更厉害的是：写入也不会阻塞读取操作。只有写入和写入之间需要进行同步等待。这样一来，读操作的性能就会大幅度提升。**那它是怎么做的呢？**

`CopyOnWriteArrayList` 类的所有可变操作（add，set 等等）都是通过创建底层数组的新副本来实现的。当 List 需要被修改的时候，我并不修改原有内容，而是对原有数据进行一次复制，将修改的内容写入副本。写完之后，再将修改完的副本替换原来的数据，这样就可以保证写操作不会影响读操作了。

从 `CopyOnWriteArrayList` 的名字就能看出 `CopyOnWriteArrayList` 是满足 `CopyOnWrite` 的。所谓 `CopyOnWrite` 也就是说：在计算机，如果你想要对一块内存进行修改时，我们不在原有内存块中进行写操作，而是将内存拷贝一份，在新的内存中进行写操作，写完之后呢，就将指向原来内存指针指向新的内存，原来的内存就可以被回收掉了。



## ConcurrentLinkedQueue

Java 提供的线程安全的 `Queue` 可以分为**阻塞队列**和**非阻塞队列**，其中阻塞队列的典型例子是 `BlockingQueue`，非阻塞队列的典型例子是 `ConcurrentLinkedQueue`，在实际应用中要根据实际需要选用阻塞队列或者非阻塞队列。 **阻塞队列可以通过加锁来实现，非阻塞队列可以通过 CAS 操作实现。**

从名字可以看出，`ConcurrentLinkedQueue`这个队列使用链表作为其数据结构．`ConcurrentLinkedQueue` 应该算是在高并发环境中性能最好的队列了。它之所有能有很好的性能，是因为其内部复杂的实现。

`ConcurrentLinkedQueue` 内部代码我们就不分析了，大家知道 `ConcurrentLinkedQueue` 主要使用 CAS 非阻塞算法来实现线程安全就好了。

`ConcurrentLinkedQueue` 适合在对性能要求相对较高，同时对队列的读写存在多个线程同时进行的场景，即如果对队列加锁的成本较高则适合使用无锁的 `ConcurrentLinkedQueue` 来替代



## BlockingQueue

上面我们己经提到了 `ConcurrentLinkedQueue` 作为高性能的非阻塞队列。下面我们要讲到的是阻塞队列——`BlockingQueue`。阻塞队列（`BlockingQueue`）被广泛使用在“生产者-消费者”问题中，其原因是 `BlockingQueue` 提供了可阻塞的插入和移除的方法。当队列容器已满，生产者线程会被阻塞，直到队列未满；当队列容器为空时，消费者线程会被阻塞，直至队列非空时为止。

`BlockingQueue` 是一个接口，继承自 `Queue`，所以其实现类也可以作为 `Queue` 的实现来使用，而 `Queue` 又继承自 `Collection` 接口。

下面主要介绍一下 3 个常见的 `BlockingQueue` 的实现类：

1. **ArrayBlockingQueue**

   `ArrayBlockingQueue` 是 `BlockingQueue` 接口的有界队列实现类，底层采用数组来实现。

2. **LinkedBlockingQueue**

   `LinkedBlockingQueue` 底层基于**单向链表**实现的阻塞队列，可以当做无界队列也可以当做有界队列来使用，同样满足 FIFO 的特性，与 `ArrayBlockingQueue` 相比起来具有更高的吞吐量，为了防止 `LinkedBlockingQueue` 容量迅速增，损耗大量内存。通常在创建 `LinkedBlockingQueue` 对象时，会指定其大小，如果未指定，容量等于 `Integer.MAX_VALUE` 。

3. **PriorityBlockingQueue**

   `PriorityBlockingQueue` 是一个支持优先级的无界阻塞队列。默认情况下元素采用自然顺序进行排序，也可以通过自定义类实现 `compareTo()` 方法来指定元素排序规则，或者初始化时通过构造器参数 `Comparator` 来指定排序规则。

   `PriorityBlockingQueue` 并发控制采用的是可重入锁 `ReentrantLock`，队列为无界队列（`ArrayBlockingQueue` 是有界队列，`LinkedBlockingQueue` 也可以通过在构造函数中传入 `capacity` 指定队列最大的容量，但是 `PriorityBlockingQueue` 只能指定初始的队列大小，后面插入元素的时候，**如果空间不够的话会自动扩容**）。

   简单地说，它就是 `PriorityQueue` 的线程安全版本。不可以插入 null 值，同时，插入队列的对象必须是可比较大小的（comparable），否则报 `ClassCastException` 异常。它的插入操作 put 方法不会 block，因为它是无界队列（take 方法在队列为空的时候会阻塞）。



## ConcurrentSkipListMap

> 跳表
>
> 最低层的链表维护了跳表内所有的元素，每上面一层链表都是下面一层的子集。
>
> 跳表内的所有链表的元素都是排序的。查找时，可以从顶级链表开始找。一旦发现被查找的元素大于当前链表中的取值，就会转入下一层链表继续找。这也就是说在查找过程中，搜索是跳跃式的。如图所示，在跳表中查找元素 18。
>
> ![](https://my-blog-to-use.oss-cn-beijing.aliyuncs.com/18-12-9/32005738.jpg)

使用跳表实现 `Map` 和使用哈希算法实现 `Map` 的另外一个不同之处是：哈希并不会保存元素的顺序，而跳表内所有的元素都是排序的。因此在对跳表进行遍历时，你会得到一个有序的结果。所以，如果你的应用需要有序性，那么跳表就是你不二的选择。JDK 中实现这一数据结构的类是 `ConcurrentSkipListMap`。





# AQS 简单介绍

AQS 的全称为 `AbstractQueuedSynchronizer` ，翻译过来的意思就是抽象队列同步器。这个类在 `java.util.concurrent.locks` 包下面。

AQS 就是一个抽象类，主要用来构建锁和同步器。

``` java
public abstract class AbstractQueuedSynchronizer extends AbstractOwnableSynchronizer implements java.io.Serializable {
}
```

AQS 为构建锁和同步器提供了一些通用功能的是实现，因此，使用 AQS 能简单且高效地构造出应用广泛的大量的同步器，比如我们提到的 `ReentrantLock`，`Semaphore`，其他的诸如 `ReentrantReadWriteLock`，`SynchronousQueue`，`FutureTask`(jdk1.7) 等等皆是基于 AQS 的。



# AQS 原理

> 在面试中被问到并发知识的时候，大多都会被问到“请你说一下自己对于 AQS 原理的理解”。下面给大家一个示例供大家参考，面试不是背题，大家一定要加入自己的思想，即使加入不了自己的思想也要保证自己能够通俗的讲出来而不是背出来。

AQS 核心思想是，如果被请求的共享资源空闲，则将当前请求资源的线程设置为有效的工作线程，并且将共享资源设置为锁定状态。如果被请求的共享资源被占用，那么就需要一套线程阻塞等待以及被唤醒时锁分配的机制，这个机制 AQS 是用 **CLH 队列锁**实现的，即将暂时获取不到锁的线程加入到队列中。

> CLH(Craig,Landin,and Hagersten)队列是一个虚拟的双向队列（虚拟的双向队列即不存在队列实例，仅存在结点之间的关联关系）。AQS 是将每条请求共享资源的线程封装成一个 CLH 锁队列的一个结点（Node）来实现锁的分配。

![](https://my-blog-to-use.oss-cn-beijing.aliyuncs.com/Java%20%E7%A8%8B%E5%BA%8F%E5%91%98%E5%BF%85%E5%A4%87%EF%BC%9A%E5%B9%B6%E5%8F%91%E7%9F%A5%E8%AF%86%E7%B3%BB%E7%BB%9F%E6%80%BB%E7%BB%93/CLH.png)

AQS 使用一个 int 成员变量来表示同步状态，通过内置的 FIFO 队列来完成获取资源线程的排队工作。AQS 使用 CAS 对该同步状态进行原子操作实现对其值的修改。

``` java
private volatile int state; // 共享变量 使用volatile修饰保证线程可见性
```

状态信息通过 `protected` 类型的`getState()`，`setState()`，`compareAndSetState()` 进行操作



## AQS 对资源的共享方式

AQS 定义两种资源共享方式

### **Exclusive**（独占）

只有一个线程能执行，如 `ReentrantLock`。又可分为公平锁和非公平锁，`ReentrantLock` 同时支持两种锁，下面以 `ReentrantLock` 对这两种锁的定义做介绍：

- **公平锁** ：按照线程在队列中的排队顺序，先到者先拿到锁
- **非公平锁** ：当线程要获取锁时，先通过两次 CAS 操作去抢锁，如果没抢到，当前线程再加入到队列中等待唤醒。(默认)

总结：公平锁和非公平锁只有两处不同：

1. 非公平锁在调用 lock 后，首先就会调用 CAS 进行一次抢锁，如果这个时候恰巧锁没有被占用，那么直接就获取到锁返回了。
2. 非公平锁在 CAS 失败后，和公平锁一样都会进入到 `tryAcquire` 方法，在 `tryAcquire` 方法中，如果发现锁这个时候被释放了（state == 0），非公平锁会直接 CAS 抢锁，但是公平锁会判断等待队列是否有线程处于等待状态，如果有则不去抢锁，乖乖排到后面。

公平锁和非公平锁就这两点区别，如果这两次 CAS 都不成功，那么后面非公平锁和公平锁是一样的，都要进入到阻塞队列等待唤醒。

相对来说，非公平锁会有更好的性能，因为它的吞吐量比较大。当然，非公平锁让获取锁的时间变得更加不确定，可能会导致在阻塞队列中的线程长期处于饥饿状态。



### **Share**（共享）

多个线程可同时执行，如 `Semaphore/CountDownLatch`。`Semaphore`、`CountDownLatch`、 `CyclicBarrier`、`ReadWriteLock` 我们都会在后面讲到。

`ReentrantReadWriteLock` 可以看成是组合式，因为 `ReentrantReadWriteLock` 也就是读写锁允许多个线程同时对某一资源进行读。

不同的自定义同步器争用共享资源的方式也不同。自定义同步器在实现时只需要实现共享资源 state 的获取与释放方式即可，至于具体线程等待队列的维护（如获取资源失败入队/唤醒出队等），AQS 已经在上层已经帮我们实现好了。



## AQS底层使用了模板方法模式

同步器的设计是基于模板方法模式的，如果需要自定义同步器一般的方式是这样（模板方法模式很经典的一个应用）：

1. 使用者继承 `AbstractQueuedSynchronizer` 并重写指定的方法。（这些重写方法很简单，无非是对于共享资源 state 的获取和释放）
2. 将 AQS 组合在自定义同步组件的实现中，并调用其模板方法，而这些模板方法会调用使用者重写的方法。

这和我们以往通过实现接口的方式有很大区别，这是模板方法模式很经典的一个运用。

**AQS 使用了模板方法模式，自定义同步器时需要重写下面几个 AQS 提供的钩子方法：**

``` java
protected boolean tryAcquire(int)//独占方式。尝试获取资源，成功则返回true，失败则返回false。
protected boolean tryRelease(int)//独占方式。尝试释放资源，成功则返回true，失败则返回false。
protected int tryAcquireShared(int)//共享方式。尝试获取资源。负数表示失败；0表示成功，但没有剩余可用资源；正数表示成功，且有剩余资源。
protected boolean tryReleaseShared(int)//共享方式。尝试释放资源，成功则返回true，失败则返回false。
protected boolean isHeldExclusively()//该线程是否正在独占资源。只有用到condition才需要去实现它。
```

> **什么是钩子方法呢？** 钩子方法是一种被声明在抽象类中的方法，一般使用 `protected` 关键字修饰，它可以是空方法（由子类实现），也可以是默认实现的方法。模板设计模式通过钩子方法控制固定步骤的实现。

除了上面提到的钩子方法之外，AQS 类中的其他方法都是 `final` ，所以无法被其他类重写。

以 `ReentrantLock` 为例，state 初始化为 0，表示未锁定状态。A 线程 `lock()` 时，会调用 `tryAcquire()` 独占该锁并将 `state+1` 。此后，其他线程再 `tryAcquire()` 时就会失败，直到 A 线程 `unlock()` 到 `state=`0（即释放锁）为止，其它线程才有机会获取该锁。当然，释放锁之前，A 线程自己是可以重复获取此锁的（state 会累加），这就是可重入的概念。但要注意，获取多少次就要释放多少次，这样才能保证 state 是能回到零态的。

再以 `CountDownLatch` 以例，任务分为 N 个子线程去执行，state 也初始化为 N（注意 N 要与线程个数一致）。这 N 个子线程是并行执行的，每个子线程执行完后` countDown()` 一次，state 会 CAS(Compare and Swap) 减 1。等到所有子线程都执行完后(即 `state=0` )，会 `unpark()` 主调用线程，然后主调用线程就会从 `await()` 函数返回，继续后余动作。

一般来说，自定义同步器要么是独占方法，要么是共享方式，他们也只需实现`tryAcquire-tryRelease`、`tryAcquireShared-tryReleaseShared`中的一种即可。但 AQS 也支持自定义同步器同时实现独占和共享两种方式，如`ReentrantReadWriteLock`。



# Semaphore(信号量)

`synchronized` 和 `ReentrantLock` 都是一次只允许一个线程访问某个资源，`Semaphore`(信号量)可以指定多个线程同时访问某个资源。

``` java
public class SemaphoreExample1 {
  // 请求的数量
  private static final int threadCount = 550;
  public static void main(String[] args) throws InterruptedException {
    // 创建一个具有固定线程数量的线程池对象（如果这里线程池的线程数量给太少的话你会发现执行的很慢）
    ExecutorService threadPool = Executors.newFixedThreadPool(300);
    // 一次只能允许执行的线程数量。
    final Semaphore semaphore = new Semaphore(20);
    for (int i = 0; i < threadCount; i++) {
      final int threadnum = i;
      threadPool.execute(() -> {// Lambda 表达式的运用
        try {
          semaphore.acquire();// 获取一个许可，所以可运行线程数量为20/1=20
          test(threadnum);
          semaphore.release();// 释放一个许可
        } catch (InterruptedException e) {
          // TODO Auto-generated catch block
          e.printStackTrace();
        }
      });
    }
    threadPool.shutdown();
    System.out.println("finish");
  }
  public static void test(int threadnum) throws InterruptedException {
    Thread.sleep(1000);// 模拟请求的耗时操作
    System.out.println("threadnum:" + threadnum);
    Thread.sleep(1000);// 模拟请求的耗时操作
  }
}
```

执行 `acquire()` 方法阻塞，直到有一个许可证可以获得然后拿走一个许可证；每个 `release` 方法增加一个许可证，这可能会释放一个阻塞的 `acquire()` 方法。然而，其实并没有实际的许可证这个对象，`Semaphore` 只是维持了一个可获得许可证的数量。 `Semaphore` 经常用于限制获取某种资源的线程数量。

当然一次也可以一次拿取和释放多个许可，不过一般没有必要这样做：

``` java
semaphore.acquire(5);// 获取5个许可，所以可运行线程数量为20/5=4
test(threadnum);
semaphore.release(5);// 释放5个许可
```

除了 `acquire()` 方法之外，另一个比较常用的与之对应的方法是 `tryAcquire()` 方法，该方法如果获取不到许可就立即返回 false。

`Semaphore` 有两种模式，公平模式和非公平模式。

- **公平模式：** 调用 `acquire()` 方法的顺序就是获取许可证的顺序，遵循 FIFO；
- **非公平模式：** 抢占式的。



# CountDownLatch （倒计时器）

`CountDownLatch` 允许 `count` 个线程阻塞在一个地方，直至所有线程的任务都执行完毕。

`CountDownLatch` 是共享锁的一种实现,它默认构造 AQS 的 `state` 值为 `count`。当线程使用 `countDown()` 方法时,其实使用了`tryReleaseShared`方法以 CAS 的操作来减少 `state`,直至 `state` 为 0 。当调用 `await()` 方法的时候，如果 `state` 不为 0，那就证明任务还没有执行完毕，`await()` 方法就会一直阻塞，也就是说 `await()` 方法之后的语句不会被执行。然后，`CountDownLatch` 会自旋 CAS 判断 `state == 0`，如果 `state == 0` 的话，就会释放所有等待的线程，`await()` 方法之后的语句得到执行。

## CountDownLatch 的两种典型用法

1. **某一线程在开始运行前等待 n 个线程执行完毕。**
2. **实现多个线程开始执行任务的最大并行性。**



## CountDownLatch 的不足

`CountDownLatch` 是一次性的，计数器的值只能在构造方法中初始化一次，之后没有任何机制再次对其设置值，当 `CountDownLatch` 使用完毕后，它不能再次被使用。



## CountDownLatch 相常见面试题

 

- `CountDownLatch` 怎么用？应用场景是什么？
- `CountDownLatch` 和 `CyclicBarrier` 的不同之处？
- `CountDownLatch` 类中主要的方法？



# CyclicBarrier(循环栅栏)

`CyclicBarrier` 和 `CountDownLatch` 非常类似，它也可以实现线程间的技术等待，但是它的功能比 `CountDownLatch` 更加复杂和强大。主要应用场景和 `CountDownLatch` 类似。

> `CountDownLatch` 的实现是基于 AQS 的，而 `CycliBarrier` 是基于 `ReentrantLock`(`ReentrantLock` 也属于 AQS 同步器)和 `Condition` 的。

`CyclicBarrier` 的字面意思是可循环使用（Cyclic）的屏障（Barrier）。它要做的事情是：让一组线程到达一个屏障（也可以叫同步点）时被阻塞，直到最后一个线程到达屏障时，屏障才会开门，所有被屏障拦截的线程才会继续干活。

`CyclicBarrier` 默认的构造方法是 `CyclicBarrier(int parties)`，其参数表示屏障拦截的线程数量，每个线程调用 `await()` 方法告诉 `CyclicBarrier` 我已经到达了屏障，然后当前线程被阻塞。



##  CyclicBarrier **的应用场景**

`CyclicBarrier` 可以用于多线程计算数据，最后合并计算结果的应用场景。比如我们用一个 Excel 保存了用户所有银行流水，每个 Sheet 保存一个帐户近一年的每笔银行流水，现在需要统计用户的日均银行流水，先用多线程处理每个 sheet 里的银行流水，都执行完之后，得到每个 sheet 的日均银行流水，最后，再用 barrierAction 用这些线程的计算结果，计算出整个 Excel 的日均银行流水。







# CyclicBarrier 和 CountDownLatch 的区别

`CountDownLatch` 是计数器，只能使用一次，而 `CyclicBarrier` 的计数器提供 `reset` 功能，可以多次使用。

对于 `CountDownLatch` 来说，重点是“一个线程（多个线程）等待”，而其他的 N 个线程在完成“某件事情”之后，可以终止，也可以等待。而对于 `CyclicBarrier`，重点是多个线程，在任意一个线程没有完成，所有的线程都必须等待。

`CountDownLatch` 是计数器，线程完成一个记录一个，只不过计数不是递增而是递减，而 `CyclicBarrier` 更像是一个阀门，需要所有线程都到达，阀门才能打开，然后继续执行。





# Atomic 原子类介绍

Atomic 翻译成中文是原子的意思。 这里 Atomic 是指一个操作是不可中断的。即使是在多个线程一起执行的时候，一个操作一旦开始，就不会被其他线程干扰。

所以，所谓原子类说简单点就是具有原子/原子操作特征的类。

并发包 `java.util.concurrent` 的原子类都存放在`java.util.concurrent.atomic`

根据操作的数据类型，可以将 JUC 包中的原子类分为 4 类

**基本类型**

使用原子的方式更新基本类型

- AtomicInteger：整型原子类
- AtomicLong：长整型原子类
- AtomicBoolean ：布尔型原子类

**数组类型**

使用原子的方式更新数组里的某个元素

- AtomicIntegerArray：整型数组原子类
- AtomicLongArray：长整型数组原子类
- AtomicReferenceArray ：引用类型数组原子类

**引用类型**

- AtomicReference：引用类型原子类
- AtomicMarkableReference：原子更新带有标记的引用类型。该类将 boolean 标记与引用关联起来，也可以解决使用 CAS 进行原子更新时可能出现的 ABA 问题。
- AtomicStampedReference ：原子更新带有版本号的引用类型。该类将整数值与引用关联起来，可用于解决原子的更新数据和数据的版本号，可以解决使用 CAS 进行原子更新时可能出现的 ABA 问题。

**对象的属性修改类型**

- AtomicIntegerFieldUpdater:原子更新整型字段的更新器
- AtomicLongFieldUpdater：原子更新长整型字段的更新器
- AtomicReferenceFieldUpdater：原子更新引用类型里的字段





## 原子操作

## CAS(比较并交换)

是原子操作的一种，可用于在多线程编程中实现不被打断的数据交换操作，从而避免多线程同时改写某一数据时由于执行顺序不确定性以及中断的不可预知性产生的数据不一致问题。 该操作通过将内存中的值与指定数据进行比较，当数值一样时将内存中的数据替换为新的值。















































#  CPU 缓存模型说起

**为什么要弄一个 CPU 高速缓存呢？** 类比我们开发网站后台系统使用的缓存（比如 Redis）是为了解决程序处理速度和访问常规关系型数据库速度不对等的问题。 **CPU 缓存则是为了解决 CPU 处理速度和内存处理速度不对等的问题。**

**内存可以看作外存的高速缓存**

**CPU Cache 缓存的是内存数据用于解决 CPU 处理速度和内存不匹配的问题，内存缓存的是硬盘数据用于解决硬盘访问速度过慢的问题。**

![](https://javaguide.cn/assets/cpu-cache.45d4b8b6.jpg)

**CPU Cache 的工作方式：** 先复制一份数据到 CPU Cache 中，当 CPU 需要用到的时候就可以直接从 CPU Cache 中读取数据，当运算完成后，再将运算得到的数据写回 Main Memory 中。但是，这样存在 **内存缓存不一致性的问题** ！比如我执行一个 i++操作的话，如果两个线程同时执行的话，假设两个线程从 CPU Cache 中读取的 i=1，两个线程做了 1++运算完之后再写回 Main Memory 之后 i=2，而正确结果应该是 i=3。

**CPU 为了解决内存缓存不一致性问题可以通过制定缓存一致协议（比如 [MESI 协议）或者其他手段来解决。** 这个缓存缓存一致性协议指的是在 CPU 高速缓存与主内存交互的时候需要准守的原则和规范。不同的 CPU 中，使用的缓存一致性协议通常也会有所不同。

![](https://javaguide.cn/assets/cpu-cache-protocol.b83e957c.jpg)



我们的程序运行在操作系统之上，操作系统屏蔽了底层硬件的操作细节，将各种硬件资源虚拟化。于是，操作系统也就同样需要解决内存缓存不一致性问题。

操作系统通过 **内存模型（Memory Model）** 定义一系列规范来解决这个问题。无论是 Windows 系统，还是 Linux 系统，它们都有特定的内存模型。



# 指令重排序

说完了 CPU 缓存模型，我们再来看看另外一个比较重要的概念 **指令重排序** 。

为了提升执行速度/性能，计算机在执行程序代码的时候，会对指令进行重排序。

**什么是指令重排序？** 简单来说就是系统在执行代码的时候并不一定是按照你写的代码的顺序依次执行。

**常见的指令重排序有下面 2 种情况**：

- **编译器优化重排** ：编译器（包括 JVM、JIT 编译器等）在不改变单线程程序语义的前提下，重新安排语句的执行顺序。
- **指令并行重排** ：现代处理器采用了指令级并行技术(Instruction-Level Parallelism，ILP)来将多条指令重叠执行。如果不存在数据依赖性，处理器可以改变语句对应机器指令的执行顺序。

内存系统也会有“重排序”，但有不是真正意义上的重排序。在 JMM 里表现为主存和本地内存的内容可能不一致，进而导致程序在多线程下执行可能出现问题。

Java 源代码会经历 **编译器优化重排 —> 指令并行重排 —> 内存系统重排** 的过程，最终才变成操作系统可执行的指令序列。

**指令重排序可以保证串行语义一致，但是没有义务保证多线程间的语义也一致** ，所以在多线程下，指令重排序可能会导致一些问题。





# happens-before 原则是什么？

> **happens-before 原则表达的意义其实并不是一个操作发生在另外一个操作的前面，虽然这从程序员的角度上来说也并无大碍。更准确地来说，它更想表达的意义是前一个操作的结果对于后一个操作是可见的，无论这两个操作是否在同一个线程里。**



分布式环境中，通过一系列规则来定义逻辑时钟的变化，从而能通过逻辑时钟来对分布式系统中的事件的先后顺序进行判断。**逻辑时钟并不度量时间本身，仅区分事件发生的前后顺序，其本质就是定义了一种 happens-before 关系。**

 happens-before 原则的诞生是为了程序员和编译器、处理器之间的平衡。程序员追求的是易于理解和编程的强内存模型，遵守既定规则编码即可。编译器和处理器追求的是较少约束的弱内存模型，让它们尽己所能地去优化性能，让性能最大化。happens-before 原则的设计思想其实非常简单：

- 为了对编译器和处理器的约束尽可能少，只要不改变程序的执行结果（单线程程序和正确执行的多线程程序），编译器和处理器怎么进行重排序优化都行。
- 对于会改变程序执行结果的重排序，JMM 要求编译器和处理器必须禁止这种重排序。

![](https://guide-blog-images.oss-cn-shenzhen.aliyuncs.com/github/javaguide/java/concurrent/image-20220731155332375.png)

- 如果一个操作 happens-before 另一个操作，那么第一个操作的执行结果将对第二个操作可见，并且第一个操作的执行顺序排在第二个操作之前。
- 两个操作之间存在 happens-before 关系，并不意味着 Java 平台的具体实现必须要按照 happens-before 关系指定的顺序来执行。如果重排序之后的执行结果，与按 happens-before 关系来执行的结果一致，那么 JMM 也允许这样的重排序。

我们看下面这段代码：

```java
int userNum = getUserNum(); 	// 1
int teacherNum = getTeacherNum();	 // 2
int totalNum = userNum + teacherNum;	// 3
```

虽然 1 happens-before 2，但对 1 和 2 进行重排序不会影响代码的执行结果，所以 JMM 是允许编译器和处理器执行这种重排序的。但 1 和 2 必须是在 3 执行之前，也就是说 1,2 happens-before 3 。

**happens-before 原则表达的意义其实并不是一个操作发生在另外一个操作的前面，虽然这从程序员的角度上来说也并无大碍。更准确地来说，它更想表达的意义是前一个操作的结果对于后一个操作是可见的，无论这两个操作是否在同一个线程里。**



# happens-before 常见规则有哪些？谈谈你的理解？

happens-before 的规则就 8 条，说多不多，重点了解下面列举的 5 条即可。全记是不可能的，很快就忘记了，意义不大，随时查阅即可。

1. **程序顺序规则** ：一个线程内，按照代码顺序，书写在前面的操作 happens-before 于书写在后面的操作；
2. **解锁规则** ：解锁 happens-before 于加锁；
3. **volatile 变量规则** ：对一个 volatile 变量的写操作 happens-before 于后面对这个 volatile 变量的读操作。说白了就是对 volatile 变量的写操作的结果对于发生于其后的任何操作都是可见的。
4. **传递规则** ：如果 A happens-before B，且 B happens-before C，那么 A happens-before C；
5. **线程启动规则** ：Thread 对象的 `start（）`方法 happens-before 于此线程的每一个动作。

如果两个操作不满足上述任意一个 happens-before 规则，那么这两个操作就没有顺序的保障，JVM 可以对这两个操作进行重排序。



# happens-before 和 JMM 什么关系？

JMM是JVM封装过的对程序员不可见  程序员也不可以做出更改自定义

happens-before是一套“规则” 用来建立 JMM和程序员的桥梁



![](https://guide-blog-images.oss-cn-shenzhen.aliyuncs.com/github/javaguide/java/concurrent/image-20220731084604667.png)

















# **什么是OOM？**

内存用完了”，来源于java.lang.OutOfMemoryError

当JVM因为没有足够的内存来为对象分配空间并且垃圾回收器也已经没有空间可回收时，就会抛出这个error（注：非exception，因为这个问题已经严重到不足以被应用处理）。

为什么会没有内存了呢？原因不外乎有两点：

1. 分配的少了：比如虚拟机本身可使用的内存（一般通过启动时的VM参数指定）太少。
2. 应用用的太多，并且用完没释放，浪费了。此时就会造成内存泄露或者内存溢出。

**内存泄露**：申请使用完的内存没有释放，导致虚拟机不能再次使用该内存，此时这段内存就泄露了，因为申请者不用了，而又不能被虚拟机分配给别人用。

**内存溢出**：申请的内存超出了JVM能提供的内存大小，此时称之为溢出。













































