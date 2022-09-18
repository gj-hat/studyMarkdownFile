# JVM



## 01_ JVM基础

### 各种各样的图

![image-20220830134808080](https://tva1.sinaimg.cn/large/e6c9d24ely1h5oqtyamtaj21a60u0djf.jpg)

<img src="https://tva1.sinaimg.cn/large/e6c9d24ely1h5or9vnxevj20l615kgnt.jpg" alt="image-20220830140329181" style="zoom:50%;" /> 

![image-20220830150122987](https://tva1.sinaimg.cn/large/e6c9d24ely1h5osy40lw5j21do0ee0up.jpg) 

![image-20220830150348920](https://tva1.sinaimg.cn/large/e6c9d24ely1h5ot0ndrbfj21e40r0adh.jpg) 



![image-20220830152355723](https://tva1.sinaimg.cn/large/e6c9d24ely1h5otlkpgbgj224o0jon30.jpg) 

解释:

### 1.1 大致解释

1. JVM包括三部分: 类装载   运行时数据区(存放数据)   执行引擎
2. 紫色部分是线程独享   橙色部分线程共享  



#### 1.2 程序计数器

记录当前代码执行到那一行(指的是已经编译的指令代码)



### 1.3 堆

>  方法区在逻辑上是属于堆的一部分  方法区看作是一块独立于Java堆的内存空间

 new出来的对象在堆里 字符串常量池也在堆里



### 1.4 栈

栈帧内存空间

1. 一般存放

   + 局部变量  操作数栈中操作完成的值 进入到局部变量表中 (包括数字和对象的引用)

   + 操作数栈  临时内存空间 操作完后一般赋值到局部变量表中(比如：复制、交换、求和等)

   + 动态连接  方法区中对应的方法代码地址(把符号引用变为直接引用) 

     ``` java
     Person a = person.A()   // 其中a指向Person类的A的方法
     ```

   + 方法出口 用来保存同一个线程之间方法切换的上下文 

2. 每个线程都是一个栈帧 

3. 各个栈帧之间相互隔离 栈帧之间FILO

4. 局部变量生命周期结束了 当前的空间会被销毁(出栈)  





### 1.5 元空间方法区

>  方法区在逻辑上是属于堆的一部分  方法区看作是一块独立于Java堆的内存空间
>
> 元空间不在虚拟机设置的内存中，而是使用本地内存。

``` java
Person p = new Person()  // Person在方法区  p在栈帧的局部变量表 new Person()堆中
```

方法区主要存: `Class`  堆中存放的是实例化的对象

jdk8之后 元空间的内存不占用堆的内存 而是物理系统的内存

类卸载的时候会释放元空间的内存(即 这个类加载器的所有对象都没有存活 没有引用就会被GC)





### 1.6 本地方法栈

几乎用不到 都是native方法





## 02_GC

1. 什么样的对象可以被回收:  堆栈中 没有引用连接的对象



### 2.1 判断垃圾对象算法

1. **引用计数法:**  但是**JAVA没有使用这种算法** python使用的就是这种

   每有一个引用 那么就把这个对象的引用标记+1  引用结束-1  直到为0 

    但是对象之间引用问题无法解决

2. **可达性分析算法:**  

   将GC Roots对象作为起点  一直向下搜索 路径上的所有对象都标记为非垃圾 其余的对象就是垃圾

   GCRoots一般是: 线程栈的本地变量 静态变量 本地方法栈变量 



### 2.2 垃圾回收算法

1. 标记清理
2. 标记复制
3. 标记整理 



### 2.3 垃圾回收流程

![image-20220830150348920](https://tva1.sinaimg.cn/large/e6c9d24ely1h5xutcd1qkj21p60j8415.jpg) 

> ```java
> Java堆 = 老年代 + 新生代  = 2:1
> 新生代 = Eden + from S0 + to S1 = 8:1:1
> ```
>

1. 通常来讲 对象产生在伊甸园区
2. 伊甸园区满后触发一次youngGC(MinorGC) 
3. 存活的对象
   1. 伊甸园区->Survivor0 同时年龄+1  采用的算法 标记-复制
   2. Survivor0->Survivor1 同时年龄+1  采用的算法 标记-复制

4. 年龄到16放入老年代  这个采用的垃圾算法一般是标记清理 每个垃圾回收器不一样
5. 老年代要是满了 则触发一次fullGC(这个时间代价非常长)



注意:

1. **在Eden区满了的时候，才会触发MinorGC**，而幸存者区满了后，**不会触发MinorGC操作**。
2. 如果Survivor区满了后，**将会触发一些特殊的规则，也就是可能直接晋升老年代**。
3. minorGC时间非常短  fullGC时间非常长
4. 两个GC都会导致卡顿 但是卡顿时间不一样
5. 为什么需要两个幸存者区?   避免碎片化 一个s区永远为空 另一个永远没有碎片



### 2.4 常见垃圾回收器

> **如果说收集算法是内存回收的方法论，那么垃圾收集器就是内存回收的具体实现。**
>
> 所有的垃圾回收都会产生会stw(stop the world)  问题

![image-20220907113314868](https://tva1.sinaimg.cn/large/e6c9d24ely1h5xvw0uslnj223o0ss41o.jpg) 

1. Serial收集器: 单线程  新生代复制 老年代 整理
2. ParNew: `1`的多线程  除此之外和`1`一样 在`3`的基础上进行改造为了配合CMS
3. Parallel Scavenge收集器: 和`2`一样   **jdk8默认**
4. SerialOld  单线程 Serial的老年代版本   jdk1.5之前
5. Parallel Old 和上述一样 是Parallel的老年代版本   **jdk8默认**
6. CMS收集器: 并发工作 几乎没有stw情况 垃圾清理和用户线程几乎同时  **最牛逼**
7. G1: 面向服务器的 主要针对多处理器  大容量内存的机器
8. ZGC: 与 CMS 中的 ParNew 和 G1 类似，ZGC 也采用标记-复制算法，不过 ZGC 对该算法做了重大改进。





## 03_类加载

### 图

1. 类加载器

   ![](https://tva1.sinaimg.cn/large/e6c9d24ely1h5r60c8uscj21v80u0jx6.jpg)

2. 类加载流程 手动加载走上面 自动加载走下面

   ![image-20220830163818603](https://tva1.sinaimg.cn/large/e6c9d24ely1h5ovqyn1dfj21j20u0q6r.jpg) 

3. 完整大体流程  `上述2的classLoad.loadClass`其实完成的就是下面红框的部分

   ![image-20220830164042763](https://tva1.sinaimg.cn/large/e6c9d24ely1h5ovtgj52bj215w0u0tbe.jpg) 



### 3.1 类加载过程

1. 加载:

   查找二进制文件   在方法区生成对应的数据结构   堆区生成一个对象作为方法区的入口

2. 验证: 可以省略 

   文件格式  元数据  字节码  符号引用

3. 准备:

   静态变量赋初值

4. 解析:

   符号引用变直接引用

5. 初始化: 

   静态变量 赋正确的值

6. 使用   类访问方法区内的数据结构  对象是堆区的数据

7. 卸载



### 3.2 类加载流程

上图2  手动和自动加载类

``` java
public class TestClassLoader{
    private static int aaa = 1;
    private static int bbb = 1;
    public static void main(String[] args) throws ClassNotFoundException, InstantiationException, IllegalAccessException {
        // 创建一个类加载启动器
        Launcher launcher = Launcher.getLauncher();
        // 创建一个类加载器 默认是AppClassLoader
        ClassLoader classLoader = launcher.getClassLoader();
        System.out.println(classLoader.getClass().getName());
        ClassLoader parent = classLoader.getParent();
        System.out.println(parent.getClass().getName());
        ClassLoader parent1 = parent.getParent();
        System.out.println(parent1);
        // 加载 TestClassLoader 类
        Class<?> aClass = classLoader.loadClass("com.generaltestitems.TestClassLoader");
              // 省略了上面创建类加载器的过程 直接使用默认的加载器 应用类加载器
//Class<?> aClass = ClassLoader.getSystemClassLoader().loadClass("com.generaltestitems.TestClassLoader");
        // 获取所有的成员变量  注意这里是没有初始化的  因为类还没有实例化
        Field[] declaredFields = aClass.getDeclaredFields();
        for(Field field : declaredFields){
            System.out.println(field.getName());
        }
        // 实例化aClass对象
        TestClassLoader o = (TestClassLoader) aClass.newInstance();
        // 调用aClass的成员方法
        o.aaa = 2;
        // 执行aClass对象的方法
        o.testMed();
    }
    public  void testMed() {
        System.out.println(" jjjjjjjjjjj ");
    }
}
```



### 3.3 类加载器

1. `BootStrap ClassLoader`: 引导类加载器 `jre/lib`
2. `Extension ClassLoader`: 扩展类加载器 `jre/lib/ext`
3. `Application ClassLoader`: 应用程序类加载器 `ClassPath`

补充:

1. 扩展类和应用程序类加载器都是`java.lang.ClassLoader`的子类实例

2. 自定义类加载是直接继承`java.lang.ClassLoder`

![image-20220901162122469](https://tva1.sinaimg.cn/large/e6c9d24ely1h5r6hykgmzj20ay09smxj.jpg) 



### 3.4 双亲委派机制

> 值得一提 三个类加载器是没有继承关系的 只是委托关系

#### 1. 什么是双亲委派概述:

**当一个类加载器收到了类加载的请求的时候，他不会直接去加载指定的类，而是把这个请求委托给自己的父加载器去加载。只有父加载器无法加载这个类的时候，才会由当前这个加载器来负责类的加载。**

1. 先检查类是否已经被加载过
2. 若没有加载则调用父加载器的loadClass()方法进行加载 
3. 若父加载器为空则默认使用启动类加载器作为父加载器。 
4. 如果父类加载失败，抛出ClassNotFoundException异常后，再调用自己的findClass()方法进行加载。



#### 2. 为什么需要双亲委派？

1. **通过委派的方式，可以避免类的重复加载**
2. **通过双亲委派的方式，还保证了安全性:** 防止核心Java API被篡改。



#### 3. "父子加载器"之间的关系是继承吗？

**类加载器之间的父子关系一般不会以继承（Inheritance）的关系来实现，而是都使用组合（Composition）关系来复用父加载器的代码的。**



#### 4. loadClass、findClass、defineClass区别

- loadClass()
  - 就是主要进行类加载的方法，默认的双亲委派机制就实现在这个方法中。
- findClass()
  - 根据名称或位置加载.class字节码
- definclass()
  - 把字节码转化为Class



#### 5. 如何主动破坏双亲委派机制？

**想要破坏这种机制，那么就自定义一个类加载器，重写其中的loadClass方法，使其不进行双亲委派即可**



#### 6. JDBC破坏双亲委派:

1. 不破坏的方法: `ClassforName(“com.jdbc….”)`

   jdk提供了一个DriverManager来管理所有的driver 但是这个包在lib/rt.jar下 但是jdbc的驱动在classPath下 所以DriverManager使用的是启动类加载器无法加载应用程序目录下的类

   就需要在ApplicationLoader的类下 使用ClassforName手动加载

2. 破坏的方法: `Connection conn= DriverManager.getConnection("jdbc:mysql://localhost:330....);`

   使用上下文线程类加载器构建一个应用程序类加载器 直接加载`META-INF/services/java.sql.Driver文件中获取具体的实现类名“com.mysql.cj.jdbc.Driver”`



#### 7. Tomcat破坏双亲委派:

> 注意: 同一个JVM中 两个相同的包和类名的允许共存的 前提是他们的类加载得不一样 即看一个对象是否是同一个 除了看包名类名  还要看类加载也是同一个

同一个tomcat下会有多个web容器(war包)  每个war包之间可能是同一个应用的不同版本(即 class文件可能一样 但是里面的实现不一样)

如果不破坏双亲委派 则只能保证一个系统中同一个class文件只有一个





























