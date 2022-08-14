# 一 并发编程

多线程的罪魁祸首就是JMM模型  工作线程和主内存之间的交互

多线程的解决方案:  (从线程到主内存的情景)

1. Volatile :   
   + 仅限于解决  一写多读  的场景
2. CAS操作与原子操作类
   + AtomicLong   下面讲
   + LongAdder   JDK8自带的  比AtomicLong性能更好 减少乐观锁的重试次数
3. 锁 
   + synchronized 
   + ReentrantLock   
   + ReentrantReadWriteLock
4. 其他



## 主要知识点

1. CPU多核并发缓存架构解析
2. java多线程内存模型JMM底层
3. 内存模型底层八大原子操作
4. CPU缓存一致性协议
5. Volatile关键字
6. CPU指令重排
7. 高并发下双重检测锁DCL指令重排问题
8. As-if-serial与happens-before原则
9. Hotspot源码理解内存屏障  如何禁止指令重排
10. SpringCloud微服务框架下 并发编程的应用



# 图片

学习路线图

https://www.processon.com/view/link/617be37ee0b34d7894fcf545#map





## 01_多核并发缓存架构

<img src="https://tva1.sinaimg.cn/large/e6c9d24ely1h508j63e6ej20we0tgacb.jpg" style="zoom:50%;" /> 

## 02_JMM多线程内存模型

>   和上面cpu缓存模型很类似 是基于CPU缓存模型来建立的 java内存模型是标准化的,屏蔽了底层不同计算机的区别

<img src="https://tva1.sinaimg.cn/large/e6c9d24ely1h508jb524vj21830u0q6j.jpg" style="zoom:30%;" /> 
<img src="https://tva1.sinaimg.cn/large/e6c9d24ely1h508je9wj6j20le0ekwg5.jpg" style="zoom:75%;" /> 





# 代码



## 01_JMM两个线程修改同一个数据

解析: 

1. 线程二修改了initFlag的值 线程一没有感知到  一直死循环
2. initFlag加入了volatile关键字  线程二改了值 线程一可以通过内存穿透看到值被修改

``` java
// -------------------错误实例 -------------------------------------
public class Thread2 {
    public static boolean initFlag = false;
    public static void main(String[] args) throws InterruptedException {
        new Thread((() -> {
            System.out.println("Thread1 start");
            while (!initFlag) {
            }
            System.out.println("Thread1 end");
        })).start();

        Thread.sleep(2000);
        new Thread((() -> {
            System.out.println("Thread2 start");
            initFlag = true;
            System.out.println("Thread2 end");
        })).start();
    }
}
// -------------------正确实例   -------------------------------------
public class Thread2 {
    public static volatile boolean initFlag = false;
    public static void main(String[] args) throws InterruptedException {
        new Thread((() -> {
            System.out.println("Thread1 start");
            while (!initFlag) {
            }
            System.out.println("Thread1 end");
        })).start();

        Thread.sleep(2000);
        new Thread((() -> {
            System.out.println("Thread2 start");
            initFlag = true;
            System.out.println("Thread2 end");
        })).start();
    }
}
```

## 02_DLC双重锁实现单例

解释:

多线程同时调用这个对象  可以保证new出来的是同一个

1. 线程1调用时候  DLC为空 进入锁中
2. 线程1还没结束 线程2进入  这时候1还没new出来 所以这个时候第一层if还是会进去
3. 线程2在锁这一行代码时候 会 `Happens-before`原则限制 线程1没解锁前这个锁不能进入 必须等待1释放
4. 线程2等待1释放了锁 线程2进入锁时候其实1已经new出来了 所以再进行一下判断

``` java
//  --------------多线程单例模式  原始版有隐患  半初始化问题隐患-----------------------
public class DLC {
    private static DLC instance = null;
    private DLC() {
    }
    public static DLC getInstance() {
        if (instance == null) {
            synchronized (DLC.class) {
                if (instance == null) {
                    instance = new DLC();
                }
            }
        }
        return instance;
    }
  //  -------volatile进行内存屏障  上述代码可能会发生指令重排序 导致上述说的问题隐患------------
  public class DLC {
    private static volatile DLC instance = null;
    private DLC() {
    }
    public static DLC getInstance() {
        if (instance == null) {
            synchronized (DLC.class) {
                if (instance == null) {
                    instance = new DLC();
                }
            }
        }
        return instance;
    }
    
    public static void main(String[] args) {
        DLC dlc1 = DLC.getInstance();
        DLC dlc2 = DLC.getInstance();
        System.out.println(dlc1 == dlc2);
    }
}
```



## 03_AtomicLong解决Volatile不足

解释: 开启五十个线程  每个线程都对一个整形数字进行+1  结束后返回整形计数器

1. 不使用多线程原子操作

   返回的结果随机

2. 使用`AtomicLong`原子操作

   返回正确的值 50 * 200

``` java
public class AtomicRefDemo {

//    int count = 0;
    AtomicLong accessCount = new AtomicLong(0);
    public void access() {
        //        count++;   如果不使用原子操作 返回的值 是大于10000的
        // 原子性操作 让accessCount加1  线程安全
        accessCount.incrementAndGet();
    }
    public static void main(String[] args) throws InterruptedException {
        AtomicRefDemo atomicRefDemo = new AtomicRefDemo();
        for (int i = 0; i < 50; i++) {
            new Thread(() -> {
                for (int j = 0; j < 200; j++) {
                    atomicRefDemo.access();
                }
            }).start();
        }
        Thread.sleep(1000);
        System.out.println("接口访问的次数"+atomicRefDemo.accessCount);
//        System.out.println("接口访问的次数"+atomicRefDemo.count);
    }
}
```



## 04_ABA问题代码

解释: 

1. 使用`AtomicReference` 只能保证读取的对象相同 但是不能保证这个对象有没有被其他线程使用过
2. 使用`AtomicStampedReference`加入版本号 每次操作对象都检测版本号是否是期望值 否则则回滚

``` java
//    -----------------会产生脏读-------------------------
public class AtomicRefDemo {
    static AtomicReference<String> accessString = new AtomicReference<>("abc");
    public static void main(String[] args) throws InterruptedException {
        new Thread(() -> {
          // 当原始值为abc时候  改为123  原始值如果不是则不操作
            accessString.compareAndSet("abc", "123");
            System.out.printf("1. value: ", accessString.get());
            accessString.compareAndSet("123", "abc");
        }).start();
        Thread.sleep(1000);
        System.out.printf("2. value: ", accessString.get());
        new Thread(() -> {
            accessString.compareAndSet("abc", "321");
            System.out.printf("3. value: ", accessString.get());
            accessString.compareAndSet("321", "abc");
        }).start();
        Thread.sleep(1000);
        System.out.println("4. value: " + accessString.get());
    }
}
// ---------------------加入版本属性 可以解决--------------------------------
public class AtomicRefDemo {
    static AtomicStampedReference<String> accessString = new AtomicStampedReference<>("abc",0);
    public static void main(String[] args) throws InterruptedException {
        int stamp = accessString.getStamp();
        new Thread(() -> {
            accessString.compareAndSet("abc", "123", accessString.getStamp(), accessString.getStamp() + 1);
            System.out.println("1. value: " + accessString.getReference() + "版本:" + accessString.getStamp());
            accessString.compareAndSet("123", "abc", accessString.getStamp(), accessString.getStamp() + 1);
        }).start();
        Thread.sleep(1000);
        System.out.println("2. value: " + accessString.getReference() + "版本:" + accessString.getStamp());
        new Thread(() -> {
            accessString.compareAndSet("abc", "321", stamp , stamp + 1);
            System.out.println("3. value: " + accessString.getReference() + "版本:" + accessString.getStamp());
            accessString.compareAndSet("321", "abc", accessString.getStamp(), accessString.getStamp() + 1);
        }).start();
        Thread.sleep(1000);
        System.out.println("4. value: " + accessString.getReference() + "版本:" + accessString.getStamp());
    }
}
```



## 05_ConcurrentHashMap底层 

> 下面CAS中会有

<img src="https://tva1.sinaimg.cn/large/e6c9d24ely1h50nwucug4j20nm0r8acn.jpg" style="zoom:75%;" />



## 06_ThteadLocals测试

> 线程1对t进行更改  线程2 无法感知到

``` java
public class ThreadLocalsTest {
    static ThreadLocal<String> t = new ThreadLocal<String>();
    public static void main(String[] args) throws InterruptedException {

        new Thread(() -> {
            t.set("1");
            System.out.println(t.get());
        }).start();

        Thread.sleep(1000);

        new Thread(() -> {
            System.out.println(t.get());  // null 未检测到t的更改
        }).start();
    }
}
```







# 知识点

## 01_volatile的作用

底层使用了操作系统的Lock实现的  使用C++实现的

1. 保证可见性 一个线程修改了主内存的值 其他现成立刻就能看到

   当写线程写一个volatile变量时，JMM会把该线程对应的本地工作内存中的共享变量值刷新到主内存。当读线程读一个volatile变量时，JMM会把该线程对应的本地工作内存置为无效，线程将到主内存中重新读取共享变量。

2. 保证有序性(禁止指令[重排序])  内存屏障 `上述代码02的解决部分`

   volatile有序性的保证就是通过禁止指令重排序来实现的。指令重排序包括编译器和处理器重排序，JMM会分别限制这两种指令重排序。

3. 不保证[原子性]

   **volatile是不能保证原子性的，即执行过程中是可以被其他线程打断甚至是加塞的。**

   **原子性需要借助`synchronized`锁机制**

   volatile变量的原子性与synchronized的原子性是不同的。synchronized的原子性是指，只要声明为synchronized的方法或代码块，在执行上就是原子操作的。而volatile是不修饰方法或代码块的，它只用来修饰变量，对于单个volatile变量的读和写操作都具有原子性，但类似于volatile++这种复合操作不具有原子性。所以volatile的原子性是受限制的。并且在多线程环境中，volatile并不能保证原子性。



### 1.1 关于上述代码01的解释

![](https://tva1.sinaimg.cn/large/e6c9d24ely1h4zq9k8hm2j21m20u00yk.jpg)



### 1.2 java规范的四种内存屏障

**先了解两种命令：** 在java中规范了一下两种命令  (这只是一个规范而已 编译后jdk运行看看到下面的命令会调用处理器的一些机制去实现内存屏障  比如说`lock前缀`)

- Store装载：将处理器缓存的数据刷新到内存中。
- Load 写(刷新)：将内存存储的数据拷贝到处理器的缓存中。

两两组合就形成了下面的

| 屏障类型   | 指令示例                 | 说明                                                         |
| ---------- | ------------------------ | ------------------------------------------------------------ |
| LoadLoad   | Load1;LoadLoad;Load2     | 该屏障确保Load1数据的装载先于Load2及其后所有装载指令的的操作 |
| StoreStore | Store1;StoreStore;Store2 | 该屏障确保Store1立刻刷新数据到内存(使其对其他处理器可见)的操作先于Store2及其后所有存储指令的操作 |
| LoadStore  | Load1;LoadStore;Store2   | 确保Load1的数据装载先于Store2及其后所有的存储指令刷新数据到内存的操作 |
| StoreLoad  | Store1;StoreLoad;Load2   | 该屏障确保Store1立刻刷新数据到内存的操作先于Load2及其后所有装载装载指令的操作。它会使该屏障之前的所有内存访问指令(存储指令和访问指令)完成之后,才执行该屏障之后的内存访问指令 |

**通过上述屏障结合`volatile`**

Java虚拟机会在`volatile`修饰的变量前后增加一些内存屏障 实例如下
``` java
StoreStore  // 写屏障
  a = 1;   // Volatile写  给变量a写(刷新数据时候)  之前写屏障 之后读写屏障
SotreLoad   // 写度屏障
  b = a;   // Volatile读   读取a的值   后面加读读屏障 和 读写屏障
LoadLoad    // 读读
LoadStore   // 读写
```

内存屏障底层使用到汇编语言`Lock`前缀锁机制



## 02_JMM的8大原子操作

用于JMM的  主内存和工作内存的交互操作

1. **lock(锁定)**：作用于主内存的变量，它把一个变量标识为一条线程独占的状态。
2. **unlock(解锁)**:作用于主内存的变量，它把一个处于锁定状态的变量释放出来，释放后的变量 才可以被其他线程锁定。
3. **read(读取)**:作用于主内存的变量，它把一个变量的值从主内存传输到线程的工作内存中，以 便随后的load动作使用。
4. **load(载入)**:作用于工作内存的变量，它把read操作从主内存中得到的变量值放入工作内存的 变量副本中。
5. **use(使用)**:作用于工作内存的变量，它把工作内存中一个变量的值传递给执行引擎，每当虚 拟机遇到一个需要使用变量的值的字节码指令时将会执行这个操作。
6. **assign(赋值)**:作用于工作内存的变量，它把一个从执行引擎接收的值赋给工作内存的变量， 每当虚拟机遇到一个给变量赋值的字节码指令时执行这个操作。
7. **store(存储):**作用于工作内存的变量，它把工作内存中一个变量的值传送到主内存中，以便随 后的write操作使用。
8. **write(写入)**:作用于主内存的变量，它把store操作从工作内存中得到的变量的值放入主内存的变量中。



  ## 03_缓存不一致问题

+ 缓存一致性协议(MESI)

  多个cpu从主内存读取同一个数据到各自的高速缓存，当其中某个cpu修改了缓存里
  的数据，该数据会马上同步回主内存，其它cpu通过总线嗅探机制可以感知到数据的
  变化从而将自己缓存里的数据失效

+ 缓存加锁

  缓存锁的核心机制是基于缓存一致性协议来实现的，一个处理器的缓存回写到内存会
  导致其他处理器的缓存无效，IA-32和Intel 64处理器使用MES/实现缓存一致性协议



## 04_并发编程特点

1. 可见性
2. 有序性
3. 原子性



## 05_指令重排序

指令重排序: 在不影响单线程程序执行结果的前提下, 计算机为了最大限度的发挥机器性能 会对机器指令进行重排序优化

![image-20220808224142204](https://tva1.sinaimg.cn/large/e6c9d24ely1h4zqmaxr2wj21jc09cwfl.jpg)



### 5.1 指令重排序遵循两个原则

java语言的规范

#### 5.1.1 as-if-serival 

**单线程内的顺序不会改变, 单线程内如果几句话之间没有依赖 是可以改变的 比如a=1 b=2**

as-if-serial语义的意思是：不管怎么重排序（编译器和处理器为了提高并行度），（单线程）程序的执行结果不能被改变。编译器、runtime和处理器都必须遵守as-if-serial语义。为了遵守as-if-serial语义，编译器和处理器不会对存在数据依赖关系的操作做重排序，因为这种重排序会改变执行结果。但是，如
果操作之间不存在数据依赖关系，这些操作就可能被编译器和处理器重排序。

#### 5.1.2 Happens-before哪些代码可以在哪些代码之后运行

遵循一下原则:

1. **程序次序规则**

   一个线程中 按照代码顺序

2. **volatile变量规则**

3. **传递规则**

4. **锁定规则**

   同一个锁对象时候  第一个锁的解锁(unlock)一定 在 下一个加锁(lock)之前

5. **线程启动规则**

6. **线程终结规则**

7. **线程中断规则**

8. **对象终结原则**



## 06_DLC双重锁实现单例

上面代码部分已经讲过了



## 07_对象创建的过程

> 必要知识储备:
>
> 堆内存规整与否取决于GC收集器的算法是标记-清除、还是标记-整理
>
> **堆内存规整:**  通俗的说就是用过的内存被整齐充分的利用，用过的内存放在一边，没有用过的放在另外一边，而中间利用一个分界值指针对这两边的内存进行分界，从而掌握内存分配情况
>
> **堆内存不规整:**  虚拟机维护一个可以记录内存块是否可以用的列表来了解内存分配情况

1. **类加载检查**   

   **Java虚拟机（jvm）**在读取一条new指令时候，首先检查能否在常量池中定位到这个类的符号引用，并且检查这个符号引用代表的类**是否被加载、解析和初始化**。如果没有，则会先执行相应的类加载过程。

2. **内存分配**   

   在通过`1`后，则开始为新生的对象分配内存。该对象所需的内存大小在类加载完成后便可确定，因此为每个对象分配的内存大小是确定的。而分配方式主要有两种，分别为：指针碰撞、空闲列表

   + 指针碰撞: 应用场景 堆内存连续规整 开辟新空间的时候将分界线指针往没用过的一侧移动 属于这种堆内存的GC收集器算法有serial、ParNew
   + 空间列表: 应用场景 堆内存不连续不规整 开辟新空间的时候 找到一个足够大的内存分配给该对象即可 同时更新列表记录  对应的垃圾收集器算法有: CMS

3. **初始化默认值**     

   第`2`步完成后，紧接着，虚拟机需要将分配到的内存空间都进行初始化（即给一些默认值），这将做是为了保证对象实例的字段在Java代码中可以在不赋初值的情况下使用。程序可以访问到这些字段对用数据类型的默认值。

4. **设置对象头**

   初始化`3`完成后，虚拟机对对象进行一些简单设置，如标记该对象是哪个类的实例，这个对象的hash码，该对象所处的年龄段等等（这些可以理解为对象实例的基本信息）。这些信息被写在对象头中。jvm根据当前的运行状态，会给出不同的设置方式。

5. **执行初始化方法**     (invodespecial)执行init初始化方法

   在`4`完成后，最后执行由开发人员编写的对象的初始化方法，把对象按照开发人员的设计进行初始化，一个对象便创建出来了。





## 08_常见JVM指令

### 8.1基本常用

| 指令            | 解释                                                         |
| --------------- | ------------------------------------------------------------ |
| new             | 创建一个对象并将地址放入虚拟机栈   分配一块内存                    |
| dup             | 复制一个对象地址放入虚拟机栈                                 |
| invokespecial   | 用于调用私有方法及final方法，调用构造方法                    |
| invokestatic    | 用于调用静态方法                                             |
| invokeinterface | 用于调用接口方法                                             |
| checkcast       | 确定对象为所给定的类型并强制类型转换                         |
| putstatic       | 设置类中静态字段的值                                         |
| getstatic       | 从类中获取静态字段                                           |
| putfield        | 设置对象中字段的值                                           |
| getfield        | 从对象中获取字段                                             |
| instanceof      | 判断对象是否为给定的类型                                     |
| pop             | 将上面执行的最近的栈帧弹出栈                                 |
| istore_0        | 将上面执行最近的引用地址放入局部变量表地零个槽位（相应的 i 可以替换为s，l，f，d，a，ia，ba，sa，la，fa，da，ca，aa，分别指代int，short，long，float，double，引用类型，int数组，boolean数组，short数组，long数组，float数组，double数组，char数组，引用类型数组） |
| iload_1         | 将局部变量表中第一个槽位的值或地址放入虚拟机栈（相应的 i 可以替换为s，l，f，d，a，ia，ba，sa，la，fa，da，ca，aa，分别指代int，short，long，float，double，引用类型，int数组，boolean数组，short数组，long数组，float数组，double数组，char数组，引用类型数组） |
| iconst_1        | 当int取值1-5时，取一个常量放到虚拟机栈（相应的前面的i：int类型，可替换为除了byte的其他几种基本数据类型）。 |
| bipush          | int取值-128到127时，认为是一个byte类型的值放入虚拟机栈。     |
| sipush          | int取值-32768到32767时，认为是short类型的值放入虚拟机栈      |
| ldc             | 当int取值-2147483648到2147483647时，认为是一个long类型的值放入虚拟机栈中。 |
| goto            | 跳转到某行指令                                               |
| return            | 返回方法命令。                                               |
| montiorenter            | 进入并获取对象监视器                                               |
| monitorexit            | 释放并退出对象监视器                                               |



## 09_CAS操作与原子操作类

>CAS，比较并交换（Compare-and-Swap，CAS），如果期望值和主内存值一样，则交换要更新的值，
>
>CAS操作不会阻塞当前资源 所以是一个乐观锁

解决的问题:

1. CAS可以多个线程同时对一个资源进行操作(相比volatile  )  只能保证一个共享变量的原子操作
2. ABA问题   结合`AtomicStampedReference`解决
3. CPU开销  循环时间长，开销比较大



### 9.1 ABA问题

> 一般ABA问题出现在对象中 
>
> 如果是钱 借出去还回来没什么问题 但如果是女朋友借出去 换回来 就不好了 对吧

如果一个变量初次读取的时候是 A 值，它的值被改成了 B，后来又被改回为 A，那 CAS 操作就会误认为它从来没有被改变过。    会出现脏读

比如 CAS只是比较了主内存的值是否相等  

1. 如果线程1对一个资源进行操作 恰好操作完的值和初始值一样
2. 这中间线程2也对这个资源进行操作  线程操作完 写回主内存时候发现和原来的值一样 就正常写入
3. 所以线程2 无法感知到线程1是否对这个资源进行操作过

解决:  解决代码如上`代码04`

1. 互斥同步锁synchronized
2. 如果项目只在乎数值是否正确， 那么ABA 问题不会影响程序并发的正确性。
3. JUC 包提供了一个带有时间戳的原子引用类 `AtomicStampedReference` 来解决该问题，它通过**控制变量的版本**来保证 CAS 的正确性。





### 9.2 AtomicLong

**流程图:** 上面代码`03`  `AtomicLong`流程图

![](https://tva1.sinaimg.cn/large/e6c9d24ely1h50hjgdx8bj21cy0u00vx.jpg)

**解释:**

当多个操作同时写主内存中的一个变量时候 

1. 当前线程 执行到对应的语句时候 会从主内存中 读取到相应的值

2. 当前线程操作完成后 往 主内存 写的时候 会进行一步判断

3. 如果 主内存 的值和线程刚开始时候读取的值一样(说明没有其他线程在中间插入进行更改) 则直接写回主内存中

4. 如果 主内存 的值和线程刚开始时候读取的值不一样  那就从`1`开始重新来一遍

   

### 9.2 LongAdder

> 减少了乐观锁重试次数   分段的思想

JUC给我们提供了一个类，LongAdder, 它的作用和AtomicLong是一样的，都是一个实现了原子操作的累加器，LongAdder通过维护一个基准值base和 Cell 数组，多线程的时候多个线程去竞争Cell数组的不同的元素，进行cas累加操作，并且每个线程竞争的Cell的下标不是固定的，如果CAS失败，会重新获取新的下标去更新，从而极大地减少了CAS失败的概率，最后在将base 和 Cell数组的元素累加，获取到我们需要的结果。

图示:

<img src="/Volumes/Data/often/图片/临时图片缓存/image-20220809171958256.png" style="zoom:50%;" />

解释:

1. LongAdder底层两个维护两个元素 一个是基准值 另一个是Cell数组
2. 多线程下如果有多个线程竞争一个基准值 那么LongAdder会将其赋值成一个数组 
3. 这个数组的长度会随着线程的失败错误次数而增加 2的倍数
4. 每个竞争到的线程拥有自己的数组下标元素
5. 执行完后释放  最后将base和Cell的数组元素累加
6. 然后等待下一波线程进入  

缺点:

1. 使用CAS机制不是一个锁机制  但 最后的汇总value值 会有延迟



## 10_锁方式解决多线程

### 10.1 ConcurrentHashMap

+ jdk1.7  分段锁 ReentrantLock   使用了16个线程
+ jdk1.8  CAS+sync

解释1.8:

1. 在没有hash冲突时候是CAS机制
2. 出现了hash冲突时候是sync机制   但是仅仅只锁当前的元素



### 10.2 Synchronized

> 锁的本质就是 把多线程变成单线程

synchronized是一个Java关键字，是jvm层级的，它是一种互斥锁，一次只能允许一个线程进入被锁住的代码块。

**本质上说是**:  对象锁  即在对象中锁的一个过程

**java层面上说**:  可以修饰在代码块(锁住参数对象) 普通方法(锁this对象)  静态方法(锁当前类对象)

 **JVM来说**: 每个对象有一个对象头 synchronized存在对象头当众以下是对象头里Mark Word的存储结构

#### 10.2.1 锁升级(锁膨胀)

Synchronized锁有四种状态: 无锁  偏向锁  轻量级锁  重量级锁 

1. **无锁**

   当一个对象被创建之后，还没有线程进入，这个时候对象处于无锁状态，

2. **偏向锁**

   当锁处于无锁状态时，有一个线程A访问同步块并获取锁时，会在对象头和栈帧中的锁记录记录线程ID，以后该线程在进入和退出同步块时不需要进行CAS操作来进行加锁和解锁，只需要简单的测试一下啊对象头中的线程ID和当前线程是否一致。

3. **轻量级锁**

   在偏向锁的基础上，又有另外一个线程B进来，这时判断对象头中存储的线程A的ID和线程B不一致，就会使用CAS竞争锁，并且升级为轻量级锁，会在线程栈中创建一个锁记录(lock Record)，将Mark Word复制到锁记录中，然后线程尝试使用CAS将对象头的Mark Word替换成指向锁记录的指针，如果成功，则当前线程获得锁；失败，表示其他线程竞争锁，当前线程便尝试CAS来获取锁。

4. **重量级锁**

    当线程没有获得轻量级锁时，线程会CAS自旋来获取锁，当一个线程自旋10次之后(超过了一次上下文切换的时间)，仍然未获得锁，那么就会升级成为重量级锁。 成为重量级锁之后，线程会进入阻塞队列(EntryList)，线程不再自旋获取锁，而是由CPU进行调度，线程串行执行。

![](https://tva1.sinaimg.cn/large/e6c9d24ely1h50t5gxdjsj218k0m6di2.jpg) 



 ## 11_线程池



















## 12_Java引用

> 这一节是服务于 下面的ThreadLocal的

在JDK1.2以前，java中的引用的定义还是比较传统的:如果reference类型的数据中存储的数值代表的是另一块内存的起始地址，就称这块内存代表着一个引用。引用指向对象的内存地址，对象只有被引用和没被引用两种状态。

实际上，我们更希望存在这样的一类对象：当内存空间还足够的时候，这些对象能够保留在内存空间中；如果当内存空间在进行了垃圾收集之后还是非常紧张，则可以抛弃这些对象。基于这种特性，可以满足很多系统的缓存功能的使用场景。

所以，java对引用的概念进行了扩充。

`Reference`是上述几种引用的父类，`SoftReference` ，`WeakReference`，`PhantomReference`都继承了`Reference`。因为强引用不需要指定，所以java中没有`StrongReference` 这个类。

引用类型跟JVM的垃圾回收行为有关。

### 12.1 强引用

> 只要还存在引用关系 就不能被清理 哪怕OOM

通常我们开发中创建的对象都是强引用，不用显式的指明。

``` java
// O对象在堆内存中的空间被 a 变量 强引用
O a = new O();
// a和O的强引用关系结束  也就是说这个对象不具备强引用 可以被回收了
a = null
```

**回收机制:** 如果一个对象具备强引用，垃圾回收器绝不会回收它。当内存空间不足，JVM宁愿抛出`OutOfMemoryError`错误，使程序异常终止，也不会出现回收具有强引用的对象来解决内存不足的情况。



### 12.2 软引用(SoftReference)

> 软引用的东西 告诉JVM如果内存不够了  就可以直接清理 哪怕还有引用关系  
>
> 作用:  用作 图片 网页等缓存

 ``` java
 SoftReference<byte[]> soft = new SoftReference<>(new byte[1024 * 1024 * 10 ]);
 System.out.println("1. " + soft.get());  // 一个地址
 byte[] bytes2 = new byte[1024 * 1024 * 10];
 System.out.println("2. " + soft.get());   // null
 ```

**回收机制:** 对于软引用关联着的对象，在JVM应用即将发生内存溢出异常之前，将会把这些软引用关联的对象列进去回收对象范围之中进行第二次回收。如果这次回收之后还是没有足够的内存，才会抛出内存溢出异常。

### 12.3 弱引用(WeakReference)

> 比软引用更容易回收  只要有gc就清理掉所有的弱引用
>
> 作用:  和软引用一样 用来做缓存使用    ThreadLocal的key是弱引用

``` java
WeakReference<byte[]> weak = new WeakReference<>(new byte[1024 * 1024 * 10 ]);
System.out.println("1. " + weak.get());  // 一个地址
byte[] bytes2 = new byte[1024 * 1024 * 10];
System.out.println("2. " + weak.get());   // null
```



**回收机制:** 被弱引用关联的对象只能生存到下一次垃圾收集发生之前，简言之就是：一旦发生GC必定回收被弱引用关联的对象，不管当前的内存是否足够。也就是弱引用只能活到下次GC之时。



### 12.4 虚引用(PhantomReference)



**回收机制:** 一个对象是否关联到虚引用，完全不会影响该对象的生命周期，也无法通过虚引用来获取一个对象的实例(`PhantomReference`覆盖了`Reference#get()`并且总是返回null)。为对象设置一个虚引用的唯一目的是：能在此对象被垃圾收集器回收的时候收到一个**系统通知**。






## 13_ThreadLocal

> 每个线程有自己独立的空间对象 线程之间不可见 
>
> 应用: Spring框架  协调多线程之间的隔离  比如SpringMvc 多个访问进来之间互补干扰

![](https://img-blog.csdn.net/20171020172424043)

**解释:**

1. 一个线程`Thread`有很多`ThreadLocal` 每个`ThreadLocal`对象又有一个`ThreadLocalMap`集合

2. ThreadLocal对象里有三个比较重要的东西 
   1. `set() `设置值
   2. `get() `获取值
   3. `ThreadLocalMap` 这是一个`Map`集合  这个集合有一个特点 `key`都为`ThreadLocals`
   4. 所以由于`set()`方法进行了封装(下面的源码) 所以对外看起来`ThreadLocal`像一个`set`集合,其实底层是`map`



`ThreadLocalMap`是一个`ThreadLocal`的静态内部类,而`ThreadLocalMap` 实例里维护的是一个或者多个`Entry<k,v>`,每个`Entry`的`key`就是`ThreadLocal`实例的弱引用,`value`就是线程的专属变量.

``` java
public void set(T value) {
  Thread t = Thread.currentThread();   // 获取当前的线程
  ThreadLocalMap map = getMap(t);       //  获取当前threadLocals的ThreadLocalMap
  if (map != null) {
    map.set(this, value);              //  将值放入当前ThreadLocal中
  } else {
    createMap(t, value);
  }
}
```

解释:

1. 系统中有很多个线程 比如 线程1 线程2 线程3  每个线程又有很多个`threadLocals`对象

   通过`getMap(threadLocals)` 获得当前线程的`threadLocals`的`ThreadLocalMap`

2. 通过`set()`可以给当前线程设置值  其他线程不可见  

   为什么不可见 可以参考上面的代码  

**作用**: 用于隔离隔个线程 每个线程的操作对其他线程不可见

**代码演示**:  上述代码`06`



### 13.1 `ThreadLocal`的 key 是**弱引用**，那么在 `ThreadLocal.get()`的时候，发生**GC**之后，key 是否为**null**？

<img src="https://img-blog.csdnimg.cn/5d99c4f6c97f4014a87e2648d3235ae2.png" style="zoom:67%;" /> 

如果 threadlocalmap 的 key 是强引用, 那么只要线程存在, threadlocalmap 就存在, 而 threadlocalmap 结构就是 entry 数组. 即对应的 entry 数组就存在, 而 entry 数组元素的 key 是 threadLocal.

即便我们在代码中显式赋值 threadlocal 为 null, 告诉 gc 要垃圾回收该对象. 由于上面的强引用存在, threadlocal 即便赋值为 null, 只要线程存在, threadlocal 并不会被回收
而设置为弱引用, gc 扫描到时, 发现 threadlocal 没有强引用, 会回收该 threadlocal 对象







### 13.2 `ThreadLocalMap`的**Hash 算法**？



### 13.3 `ThreadLocalMap`中**Hash 冲突**如何解决？



### 13.4 `ThreadLocalMap`的**扩容机制**？

在`ThreadLocalMap.set()`方法的最后，如果执行完启发式清理工作后，未清理到任何数据，且当前散列数组中`Entry`的数量已经达到了列表的扩容阈值`(len*2/3)`，就开始执行`rehash()`逻辑：





### 13.5 `ThreadLocalMap`中**过期 key 的清理机制**？**探测式清理**和**启发式清理**流程？

ThreadLocalMap的每次get、set、remove，都会清理过期的Entry，如果不调用remove则会导致value不会被GC回收

ThreadLocalMap的每次get、set、remove，都会清理过期的Entry（key为null的entry）



`ThreadLocalMap`的两种过期`key`数据清理方式：**探测式清理**和**启发式清理**。

探测式清理，也就是`expungeStaleEntry`方法，遍历散列数组，从开始位置向后探测清理过期数据，将过期数据的`Entry`设置为`null`，沿途中碰到未过期的数据则将此数据`rehash`后重新在`table`数组中定位，如果定位的位置已经有了数据，则会将未过期的数据放到最靠近此位置的`Entry=null`的桶中，使`rehash`后的`Entry`数据距离正确的桶的位置更近一些。操作逻辑如下： 

![img](https://img-community.csdnimg.cn/images/df45b6b14ed742cfbc5acac2c62a41b7.png)

 如上图，`set(27)` 经过 hash 计算后应该落到`index=4`的桶中，由于`index=4`桶已经有了数据，所以往后迭代最终数据放入到`index=7`的桶中，放入后一段时间后`index=5`中的`Entry`数据`key`变为了`null`

![img](https://img-community.csdnimg.cn/images/538bb3ba180c472c80243a2dddbe52ac.png)

 如果再有其他数据`set`到`map`中，就会触发**探测式清理**操作。

如上图，执行**探测式清理**后，`index=5`的数据被清理掉，继续往后迭代，到`index=7`的元素时，经过`rehash`后发现该元素正确的`index=4`，而此位置已经有了数据，往后查找离`index=4`最近的`Entry=null`的节点(刚被探测式清理掉的数据：`index=5`)，找到后移动`index= 7`的数据到`index=5`中，此时桶的位置离正确的位置`index=4`更近了。 

经过一轮探测式清理后，`key`过期的数据会被清理掉，没过期的数据经过`rehash`重定位后所处的桶位置理论上更接近`i= key.hashCode & (tab.len - 1)`的位置。这种优化会提高整个散列表查询性能。 



### 13.6 内存泄露

软引用断开了  但是map中的对象依然在  则发生泄露  建议每次用完自己清理





# HashMap

## 主要知识点

1. Put方法底层源码解析
2. 链表扩容
3. 链表转红黑树
4. Get方法



## HashMap数据结构

+ JDK1.8之前的HashMap由数组+链表组成的

+ JDK1.8之后 数组+红黑树 + 单向链表 + 双向链表:   

  解决哈希冲突时有了较大的变化，当链表长度大于阈值（或者红黑树的边界值，默认为8）并且当前数组的长度大于64时，此时此索引位置上的所有数据改为使用红黑树存储

 



























