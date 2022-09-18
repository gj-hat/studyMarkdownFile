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



## 07_AQS测试1

1. CountDownLatch直接实现AQS
2. ReentrantLock内部有一个Sync 实现了AQS
3. ReentrantLock同步控制器 保证正确的结果



``` java
public class AQStest1 {
    private static int total = 0;
  	// 基于AQS的一个锁  其实不用他的话 也可以使用 Synchronized
    private static ReentrantLock lock = new ReentrantLock();
    public static void main(String[] args) throws InterruptedException {
				//  这个是AQS的实现
        CountDownLatch countDownLatch = new CountDownLatch(1);
        for (int i = 0; i < 100; i++) {
            new Thread(() -> {
                try {
                    countDownLatch.await();   // 单纯创建线程，不会执行下面的代码
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
                for (int j = 0; j < 100; j++) {
                    lock.lock();  // 使用 ReentrantLock加锁
                    total++;
                    lock.unlock(); // 使用 ReentrantLock解锁
                }
            }).start();
        }
        countDownLatch.countDown();  // 唤醒所有线程
        Thread.sleep(1000);  // 10000
        System.out.println(total);
    }
}
```



## 08_公平和飞公平锁实现

``` java
// ------------------------------公平锁的 lock 方法： -----------------
static final class FairSync extends Sync {
    final void lock() {
        acquire(1);
    }
    // AbstractQueuedSynchronizer.acquire(int arg)
    public final void acquire(int arg) {
        if (!tryAcquire(arg) &&
            acquireQueued(addWaiter(Node.EXCLUSIVE), arg))
            selfInterrupt();
    }
    protected final boolean tryAcquire(int acquires) {
        final Thread current = Thread.currentThread();
        int c = getState();
        if (c == 0) {
            // 1\. 和非公平锁相比，这里多了一个判断：是否有线程在等待
            if (!hasQueuedPredecessors() &&
                compareAndSetState(0, acquires)) {
                setExclusiveOwnerThread(current);
                return true;
            }
        }
        else if (current == getExclusiveOwnerThread()) {
            int nextc = c + acquires;
            if (nextc < 0)
                throw new Error("Maximum lock count exceeded");
            setState(nextc);
            return true;
        }
        return false;
    }
}
// ------------------------------非公平锁的 lock 方法： -----------------
static final class NonfairSync extends Sync {
    final void lock() {
        // 2\. 和公平锁相比，这里会直接先进行一次CAS，成功就返回了
        if (compareAndSetState(0, 1))
            setExclusiveOwnerThread(Thread.currentThread());
        else
            acquire(1);
    }
    // AbstractQueuedSynchronizer.acquire(int arg)
    public final void acquire(int arg) {
        if (!tryAcquire(arg) &&
            acquireQueued(addWaiter(Node.EXCLUSIVE), arg))
            selfInterrupt();
    }
    protected final boolean tryAcquire(int acquires) {
        return nonfairTryAcquire(acquires);
    }
}
/**
 * Performs non-fair tryLock.  tryAcquire is implemented in
 * subclasses, but both need nonfair try for trylock method.
 */
final boolean nonfairTryAcquire(int acquires) {
    final Thread current = Thread.currentThread();
    int c = getState();
    if (c == 0) {
        // 这里没有对阻塞队列进行判断
        if (compareAndSetState(0, acquires)) {
            setExclusiveOwnerThread(current);
            return true;
        }
    }
    else if (current == getExclusiveOwnerThread()) {
        int nextc = c + acquires;
        if (nextc < 0) // overflow
            throw new Error("Maximum lock count exceeded");
        setState(nextc);
        return true;
    }
    return false;
}
```

总结在下面`14.3`



## 09_多线程和线程池对比



``` java
public class ThreadTest1 {
    public static void main(String[] args) throws InterruptedException {
        threadTest();  //  18756ms
        System.out.println("-----------------------");
        threadPoolTest(); // 10ms
    }
    // 多线程测试
    public static void threadTest() throws InterruptedException {
        long l = System.currentTimeMillis();
        final Random random = new Random();
        final List<Integer> list = new ArrayList<>();
        for (int i = 0; i < 10000; i++) {
            Thread thread = new Thread(){
                @Override
                public void run() {
                    list.add(random.nextInt());
                }
            };
            thread.start();
            thread.join();   // 一直等待线程执行完 再执行下面的 
        }
        System.out.println(System.currentTimeMillis()-l);
        System.out.println(list.size());
    }
    // 线程池测试
    public static void threadPoolTest() throws InterruptedException {
        long l = System.currentTimeMillis();
        final Random random = new Random();
        final List<Integer> list = new ArrayList<>();
        ExecutorService executorService = Executors.newSingleThreadExecutor();
        for (int i = 0; i < 10000; i++) {
            executorService.execute(
                    new Runnable() {
                        @Override
                        public void run() {
                            list.add(random.nextInt());
                        }
                    }
            );
        }
        executorService.shutdown();
        executorService.awaitTermination(1, TimeUnit.DAYS);  // 等待上面的100000线程执行一天,一天内没执行完 就直接开始执行下面的代码  线程后台一直运行
        System.out.println(System.currentTimeMillis()-l);
        System.out.println(list.size());
    }
}
```

















# 知识点

## 01_volatile的作用

底层使用了操作系统的Lock实现的  使用C++实现的

1. **保证可见性** 一个线程修改了主内存的值 其他现成立刻就能看到

   当写线程写一个volatile变量时，JMM会把该线程对应的本地工作内存中的共享变量值刷新到主内存。当读线程读一个volatile变量时，JMM会把该线程对应的本地工作内存置为无效，线程将到主内存中重新读取共享变量。

2. **保证有序性(**禁止指令[重排序])  内存屏障 `上述代码02的解决部分`

   volatile有序性的保证就是通过禁止指令重排序来实现的。指令重排序包括编译器和处理器重排序，JMM会分别限制这两种指令重排序。

3. **不保证[原子性]**

   **volatile是不能保证原子性的，即执行过程中是可以被其他线程打断甚至是加塞的。**

   **原子性需要借助`synchronized`锁机制**

   volatile变量的原子性与synchronized的原子性是不同的。synchronized的原子性是指，只要声明为synchronized的方法或代码块，在执行上就是原子操作的。而volatile是不修饰方法或代码块的，它只用来修饰变量，对于单个volatile变量的读和写操作都具有原子性，但类似于volatile++这种复合操作不具有原子性。所以volatile的原子性是受限制的。并且在多线程环境中，volatile并不能保证原子性。



### 1.1 关于上述代码01的解释(内存穿透)

![](https://tva1.sinaimg.cn/large/e6c9d24ely1h4zq9k8hm2j21m20u00yk.jpg)



### 1.2 代码02指令重排







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
StoreStore  // 写屏障  之前的数据立即写入到内存中 之前的的操作优先级大于后面所有的操作
  a = 1;   // Volatile写  给变量a写(刷新数据时候)   
SotreLoad   // 写读屏障  前面的写操作要先结束了  才会执行后面的读操作
  b = a;   // Volatile读   读取a的值   后面加读读屏障 和 读写屏障
LoadLoad    // 读读    前面读的操作优先级高于后面读的操作
LoadStore   // 读写    前面读的操作优先级高于后面写的操作
```

内存屏障底层使用到汇编语言`Lock`前缀锁机制



## 02_JMM的8大原子操作

"原子操作(atomic operation)是不需要synchronized"，这是多线程编程的老生常谈了。所谓原子操作是指不会被`线程调度`机制打断的操作；这种操作一旦开始，就一直运行到结束，中间不会有任何切换到另一个线程。

用于JMM的  主内存和工作内存的交互操作  上面的内存屏障使用到了原子操作

1. **lock(锁定)**：作用于主内存的变量，它把一个变量标识为一条线程独占的状态。
2. **read(读取)**:作用于主内存的变量，它把一个变量的值从主内存传输到线程的工作内存中，以 便随后的load动作使用。
3. **load(载入)**:作用于工作内存的变量，它把read操作从主内存中得到的变量值放入工作内存的 变量副本中。
4. **use(使用)**:作用于工作内存的变量，它把工作内存中一个变量的值传递给执行引擎，每当虚 拟机遇到一个需要使用变量的值的字节码指令时将会执行这个操作。
5. **assign(赋值)**:作用于工作内存的变量，它把一个从执行引擎接收的值赋给工作内存的变量， 每当虚拟机遇到一个给变量赋值的字节码指令时执行这个操作。
6. **store(存储):**作用于工作内存的变量，它把工作内存中一个变量的值传送到主内存中，以便随 后的write操作使用。
7. **write(写入)**:作用于主内存的变量，它把store操作从工作内存中得到的变量的值放入主内存的变量中。
8. **unlock(解锁)**:作用于主内存的变量，它把一个处于锁定状态的变量释放出来，释放后的变量 才可以被其他线程锁定。

**注意：**
1、如果需要把变量总主内存赋值给工作内存：read和load必须是连续；read只是把主内存的变量值从主内存加载到工作内存中，而load是真正把工作内存的值放到工作内存的变量副本中。
2、如果需要把变量从工作内存同步回主内存；就需要执行顺序执行store跟write操作。store作用于工作内存，将工作内存变量值加载到主内存中，write是将主内存里面的值放入主内存的变量中。



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

#### 5.1.1 as-if-serial

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

解决的问题:  保证原子性

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



下面介绍这各种原子操作类

1. **原子更新基本类型：使用原子方式更新基本类型**
   + AtomicBoolean：原子更新布尔变量
   + AtomicInteger：原子更新整型变量
   + AtomicLong：原子更新长整型变量
2. 原子更新数组：通过原子更新数组里的某个元素
   + AtomicIntegerArray：原子更新整型数组的某个元素
   + AtomicLongArray：原子更新长整型数组的某个元素
   + AtomicReferenceArray：原子更新引用类型数组的某个元素
3. **原子更新引用类型：更新引用类型**
   + AtomicReference：原子更新引用类型
   + AtomicReferenceFieldUpdater：原子更新引用类型里的字段
   + AtomicMarkableReference：原子更新带有标记位的引用类型
4. **原子更新字段：原子更新某个类的某个字段**
   + AtomicIntegerFieldUpdater：原子更新整型字段
   + AtomicLongFieldUpdater：原子更新长整型字段
   + AtomicStampedReference：原子更新带有版本号的引用类型



### 9.2 AtomicLong(随便挑一个说说)

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

 **JVM来说**: 每个对象有一个对象头     synchronized存在对象头当中

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



#### 10.2.2 对比

![](https://tva1.sinaimg.cn/large/e6c9d24ely1h58pykg0jsj20qu0h4acj.jpg)



## 11_线程池

>1. 线程底层和线程池
>2. newCheThreadPool可缓存线程池
>3. newFixedThreadPool固定个数线程池原理
>4. newSingleThreadExecutor单线程池
>5. 阿里巴巴为什么不推荐自带的线程池工具类
>6. 提交任务时execute和submit的对比
>7. 阿里巴巴生产环境中线程池如何调优
>8. 处理不可捕获的异常 

### 11.1 知识储备

进程与线程:  

> 背景:  处理器太快了 除了处理器之外的寄存器等等太慢了

1. …… 概念
2. 两者都是对时间段的一个描述  只是颗粒度不同
3. 进程主要是针对资源进行管理 不在乎处理器的状态个数等等, 线程不在乎资源 仅仅是对处理器的调度  所以又说**线程是处理器的最小单位  进程是操作系统的最小单元 因为操作系统会给每个进程提前分配好资源**   又说进程之前的资源几乎是严格隔离的  线程之间可以共享
4. 上下文切换的速度:  两者都需要进行上下文切换 但是因为线程主要在乎的是处理器不在乎资源,处理器进行切换的速度是非常快的 所以 **线程的切换远远快于进程**  **基于这个事实所以才会将进程再细分为线程**
5. 扩展到多核下: 每一次的上下文切换 无论是线程还是进程都需要有处理器的参与 **所以单核下的多线程意义不大, 只能做到并发 不能做到并行 因为在同一进程下 处理器的时间片都是给一个进程的 多线程的意义就是使处理器“忙起来”** 



JVM使用的KLT模型(原生不支持协程)  Java线程与OS线程保持1:1的映射关系 也就是说每都一个Java线程 OS也就都一个线程

协程 使用的ULT模型 go python 所以说他们不是真正的多线程

![](https://tva1.sinaimg.cn/large/e6c9d24ely1h5egdeuqg7j21zm0s8dlt.jpg)

线程的六种生命状态:

1. new  新建
2. Runnable  运行
3. Blocked  阻塞
4. Waitting  等待
5. Timed_Waitting  超时等待
6. Terminated  终结

![](https://tva1.sinaimg.cn/large/e6c9d24ely1h5eg4ywljhj20pd0kaacf.jpg)



### 14.2 线程池原理

> 线程属于是稀缺资源  每一次创建销毁都需要很大的代价  所以线程池解决的一个很大的问题就是 如何对已经有的线程进行复用

什么时候使用呢:

+ 单个任务处理时间比较短
+ 需要处理的任务数很大

使用线程池的好处：

- **降低资源消耗**。通过重复利用已创建的线程降低线程创建和销毁造成的消耗。
- **提高响应速度**。当任务到达时，任务可以不需要等到线程创建就能立即执行。
- **提高线程的可管理性**。线程是稀缺资源，如果无限制的创建，不仅会消耗系统资源，还会降低系统的稳定性，使用线程池可以进行统一的分配，调优和监控。

 所有的线程池都有一个父类接口: `Executor`  

![image-20220821171448605](https://tva1.sinaimg.cn/large/e6c9d24ely1h5ei87i3v6j209109iq35.jpg) 

线程池的工具类: 

```java
// 诸如此类 很多  其原理都是ThreadPoolExecutor的构造方法
Executors.newSingleThreadExecutor();   
```

上文说过  线程是很珍贵的资源 所以不要使用线程池的工具类 推荐自己根据实际情况

``` java
public ThreadPoolExecutor(int corePoolSize,
                          int maximumPoolSize,
                          long keepAliveTime,
                          TimeUnit unit,
                          BlockingQueue<Runnable> workQueue,
                          ThreadFactory threadFactory,
                          RejectedExecutionHandler handler)
```

参数讲解:

1. corePoolSize:  核心线程数 
2. maximumPoolSize:  最大线程数  核心+非核心
3. keepAliveTime:  最大运行线程休息时间  (休眠超时就停止)
4. TimeUnit:  时间单位  上述3的单位
5. workQueue:  存放未来得及执行的任务
6. ThreadFactory:  创建线程的工厂 (创建线程不是我们new的 是使用工厂创建)
7. RejectedExecutionHandler:  拒绝策略



主要方法:

1. **execute(Runnable command)** 执行给定的Runnable任务
2. **submit( …)**提交给定的 任务进行执行，在任务执行完成后，返回相应的 Futur
3. **shutdown()**关闭线程池 正在执行的继续执行 没执行的不执行
4. **shutdownNow()**立即关闭

 流程:有任务进来时

![](https://tva1.sinaimg.cn/large/e6c9d24ely1h5ejatvqv0j21d20u0tca.jpg)

1. 如果核心线程没有满 调用上面的线程工厂创建线程 然后放入`workQueue阻塞队列中`等待执行
2. 如果核心线程满了 但是`workQueue阻塞队列`没有满 则会加入到阻塞队列中
3. 核心线程满了 但是最大线程数没有满 返回上述`1`操作 直到最大线程数和所有队列都满了
4. 如果最大线程数和所有队列都满了  则会走上述`RejectedExecutionHandler阻塞策略`















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







## 14_AQS

> Java中的大部分同步类（Lock、Semaphore、ReentrantLock等）都是基于AbstractQueuedSynchronizer（简称为AQS）实现的。AQS是一种提供了原子式管理同步状态、阻塞和唤醒线程功能以及队列模型的简单框架。

**全称:** `AbstractQueuedSynchronizer` 抽象队列同步器框架-基于信号量的工作方式, 是用来构建锁或者其他同步组件的基础框架。

**功能:**  封装了下面的几个东西

- 状态的原子性管理
- 线程的阻塞与解除阻塞
- 队列的管理

**AQS三大精华:**

+ spin:  自旋(死循环)
+ LocalSupport: 阻塞与唤醒线程
+ cas:  原子比较与交换

两大特性锁:(知识储备)

1. 可重入锁  :   同一个锁可以加入多次 (synchronized)
2. 不可重入锁:   同一个锁不可以加入多次

AQS 定义两种资源共享方式:

1. **Exclusive**(独享)

   只有一个线程能执行，如 `ReentrantLock`。又可分为公平锁和非公平锁，`ReentrantLock` 同时支持两种锁

2. **Share**（共享）

   多个线程可同时执行，如 `Semaphore/CountDownLatch`。`Semaphore`、`CountDownLatch`、 `CyclicBarrier`、`ReadWriteLock` 我们都会在后面讲到。

   `ReentrantReadWriteLock` 可以看成是组合式，因为 `ReentrantReadWriteLock` 也就是读写锁允许多个线程同时对某一资源进行读。

   不同的自定义同步器争用共享资源的方式也不同。自定义同步器在实现时只需要实现共享资源 state 的获取与释放方式即可，至于具体线程等待队列的维护（如获取资源失败入队/唤醒出队等），AQS 已经在上层已经帮我们实现好了。



### 14.1 AQS核心变量

> 构建队列是用了Node()内部类   双向链表

1. state:  同步器状态  (信号量的体现): `private volatile int state;`

   **记录当前同步器被加锁的次数**

2. exclusiveOwnerThread:   互斥锁持有的线程  **指向获取锁的线程对象**

   ``` java
   private transient Thread exclusiveOwnerThread;  // 这个变量在父类中
   ```

3. head:  同步等待队列头

4. tail: 同步等待队列尾



### 14.2 工作流程图

AQS流程图:

1. 10个线程抢资源

2. 第一个线程抢到了 

   1. 将AQS的`state=state+1`  
   2. exclusiveOwnerThread指向自己  表明是自己拿到了锁

3. 其他九个线程进入队列中

   队列的`next`指向下一个等待的线程

4. 线程对资源使用完成 `state=state-1`

5. 因为AQS是可重入锁  所以必须等到`state=0`时候才可以完全释放资源等待队列下一个元素



### 14.3 公(非)公平锁 抢锁:

>  ReentrantLock 默认采用非公平锁，除非你在构造方法中传入参数 true 。
>
> CAS操作保证原子性 保证state值的唯一

总结：公平锁和非公平锁只有两处不同：

1. 非公平锁在调用 lock 后，首先就会调用 CAS 进行一次抢锁，如果这个时候恰巧锁没有被占用，那么直接就获取到锁返回了。
2. 非公平锁在 CAS 失败后，和公平锁一样都会进入到 tryAcquire 方法，在 tryAcquire 方法中，如果发现锁这个时候被释放了（state == 0），非公平锁会直接 CAS 抢锁，但是公平锁会判断等待队列是否有线程处于等待状态，如果有则不去抢锁，乖乖排到后面。

公平锁和非公平锁就这两点区别，如果这两次 CAS 都不成功，那么后面非公平锁和公平锁是一样的，都要进入到阻塞队列等待唤醒。

相对来说，非公平锁会有更好的性能，因为它的吞吐量比较大。当然，非公平锁让获取锁的时间变得更加不确定，可能会导致在阻塞队列中的线程长期处于饥饿状态。





### 14.4 自旋插入队列

```java
private Node enq(final Node node) {
    for (;;) {
        Node t = tail;
        if (t == null) { // Must initialize
            if (compareAndSetHead(new Node()))
                tail = head;
        } else {
            node.prev = t;
            if (compareAndSetTail(t, node)) {
                t.next = node;
                return t;
            }
        }
    }
}
```

没有拿到锁的线程 插入到队列中, 这里保障线程不丢失 采用了自旋+Cas的操作 直到插入成功为止

对了 这是一个双向链表的队列 (CLH队列的一个变种)



### 14.5 唤醒与阻塞

> 需要阻塞或者唤醒一个线程的时候，AQS都是使用LockSupport这个工具类来完成的。
>
> LockSupport是用来创建锁和其他同步类的基本线程阻塞原语。

1. 在线程获取同步状态失败后，会加入到CHL队列中去，并且该节点会自旋式的不断的获取同步状态，在获取同步状态的过程中，需要判断当前线程是否需要被阻塞。`acquireQueued(final Node node, int arg)方法`
2. 判断当前线程是否需要被阻塞。检查是否需要阻塞的方法shouldParkAfterFailedAcquire(p, node)，
3. 是否进入阻塞条件:
   1. 如果当前节点的前驱节点的等待状态为SIGNAL，则返回true
   2. 如果当前节点的前驱节点的等待状态为CALCLE，则表示该线程的前驱节点已经被中断或者超时，需要从CHL中删除，直到回溯到ws <= 0,返回false
   3. 若果当前节点的前驱节点的等待状态为非SIGNAL,非CANCLE，则以CAS的方式设置其前驱节点为的状态为SIGNAL，返回false.

4. 前驱节点使用完资源 需要唤醒下一个节点线程

5. LockSupport定义了一系列以park开头的方法来阻塞当前线程，unpark (Thread thread) 方法来唤醒一个被阻塞的线程。

   

### 14.6 CountDownLatch （倒计时器）

``` java
public CountDownLatch(int count) {}  // 初始阻塞线程数
public void await() {}     // 加入队列中  直到count=0自动开始执行
public void countDown() {}   // 线程执行完手动调用 作用是count-- 一般在final中执行
public long getCount(){}
```



`CountDownLatch` 允许 `count` 个线程阻塞在一个地方，直至所有线程的任务都执行完毕。

`CountDownLatch` 是共享锁的一种实现,它默认构造 AQS 的 `state` 值为 `count`。当线程使用 `countDown()` 方法时,其实使用了`tryReleaseShared`方法以 CAS 的操作来减少 `state`,直至 `state` 为 0 。当调用 `await()` 方法的时候，如果 `state` 不为 0，那就证明任务还没有执行完毕，`await()` 方法就会一直阻塞，也就是说 `await()` 方法之后的语句不会被执行。然后，`CountDownLatch` 会自旋 CAS 判断 `state == 0`，如果 `state == 0` 的话，就会释放所有等待的线程，`await()` 方法之后的语句得到执行。



两种用法

1. **某一线程在开始运行前等待 n 个线程执行完毕。**

   将 `CountDownLatch` 的计数器初始化为 n （`new CountDownLatch(n)`），每当一个任务线程执行完毕，就将计数器减 1 （`countdownlatch.countDown()`），当计数器的值变为 0 时，在 `CountDownLatch 上 await()` 的线程就会被唤醒。一个典型应用场景就是启动一个服务时，主线程需要等待多个组件加载完毕，之后再继续执行。

2. **实现多个线程开始执行任务的最大并行性。**

   注意是并行性，不是并发，强调的是多个线程在某一时刻同时开始执行。类似于赛跑，将多个线程放到起点，等待发令枪响，然后同时开跑。做法是初始化一个共享的 `CountDownLatch` 对象，将其计数器初始化为 1 （`new CountDownLatch(1)`），多个线程在开始执行任务前首先 `coundownlatch.await()`，当主线程调用 `countDown()` 时，计数器变为 0，多个线程同时被唤醒。



### 14.7 CyclicBarrier(循环栅栏)

`CyclicBarrier` 和 `CountDownLatch` 非常类似，它也可以实现线程间的技术等待，但是它的功能比 `CountDownLatch` 更加复杂和强大。主要应用场景和 `CountDownLatch` 类似。

> `CountDownLatch` 的实现是基于 AQS 的，而 `CycliBarrier` 是基于 `ReentrantLock`(`ReentrantLock` 也属于 AQS 同步器)和 `Condition` 的。

`CyclicBarrier` 的字面意思是可循环使用（Cyclic）的屏障（Barrier）。它要做的事情是：让一组线程到达一个屏障（也可以叫同步点）时被阻塞，直到最后一个线程到达屏障时，屏障才会开门，所有被屏障拦截的线程才会继续干活。

`CyclicBarrier` 默认的构造方法是 `CyclicBarrier(int parties)`，其参数表示屏障拦截的线程数量，每个线程调用 `await()` 方法告诉 `CyclicBarrier` 我已经到达了屏障，然后当前线程被阻塞。

``` java
// parties 就代表了有拦截的线程的数量，当拦截的线程数量达到这个值的时候就打开栅栏，让所有线程通过。
public CyclicBarrier(int parties) { 
    this(parties, null);
}

public CyclicBarrier(int parties, Runnable barrierAction) {
    if (parties <= 0) throw new IllegalArgumentException();
    this.parties = parties;
    this.count = parties;
    this.barrierCommand = barrierAction;
}
```

下面的代码:  栅栏可以一次阻挡五个线程 也就是说线程阻塞达到五个时候一起执行  下面的100个线程相当于执行二十次

``` java
public static void testCyclicBarrier() throws InterruptedException {
  CyclicBarrier cyclicBarrier = new CyclicBarrier(10);
  for (int i = 0; i < 100; i++) {
    Thread.sleep(1000);
    new Thread(() -> {
      try {
        cyclicBarrier.await(10, TimeUnit.SECONDS);
        System.out.println(Thread.currentThread().getName() + "等待中");
      } catch (Exception e) {
        e.printStackTrace();
      }
      System.out.println(Thread.currentThread().getName() + "执行完");
    }).start();
  }
}
```



#### 14.8 CyclicBarrier 和 CountDownLatch 的区别

下面这个是国外一个大佬的回答：

`CountDownLatch` 是计数器，只能使用一次，而 `CyclicBarrier` 的计数器提供 `reset` 功能，可以多次使用。但是我不那么认为它们之间的区别仅仅就是这么简单的一点。我们来从 jdk 作者设计的目的来看，javadoc 是这么描述它们的：

> CountDownLatch: A synchronization aid that allows one or more threads to wait until a set of operations being performed in other threads completes.(CountDownLatch: 一个或者多个线程，等待其他多个线程完成某件事情之后才能执行；) CyclicBarrier : A synchronization aid that allows a set of threads to all wait for each other to reach a common barrier point.(CyclicBarrier : 多个线程互相等待，直到到达同一个同步点，再继续一起执行。)

对于 `CountDownLatch` 来说，重点是“一个线程（多个线程）等待”，而其他的 N 个线程在完成“某件事情”之后，可以终止，也可以等待。而对于 `CyclicBarrier`，重点是多个线程，在任意一个线程没有完成，所有的线程都必须等待。

`CountDownLatch` 是计数器，线程完成一个记录一个，只不过计数不是递增而是递减，而 `CyclicBarrier` 更像是一个阀门，需要所有线程都到达，阀门才能打开，然后继续执行。

### 14.9 ReentrantLock 和 ReentrantReadWriteLock

**ReentrantReadWriteLock**是共享的: 支持多个读读操作

**ReentrantLock**: 独享  哪怕是读也只运行一个线程

![](https://tva1.sinaimg.cn/large/e6c9d24ely1h58pykg0jsj20qu0h4acj.jpg)

`ReentrantLock` 和 `synchronized` 的区别在上面已经讲过了这里就不多做讲解。另外，需要注意的是：读写锁 `ReentrantReadWriteLock` 可以保证多个线程可以同时读，所以在读操作远大于写操作的时候，读写锁就非常有用了。





## 15 常见并发容器





































