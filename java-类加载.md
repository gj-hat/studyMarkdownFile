## java类加载机制

Java虚拟机把描述类的数据从Class文件加载到内存，并对数据进行校验、转换解析和初始化，最终形成可以被虚拟机直接使用的Java类型，这个过程被称作虚拟机的类加载机制。



## 类的生命周期

类从被加载到虚拟机内存中开始，到卸载出内存为止，

**它的整个生命周期包括：加载，验证，准备，解析，初始化,使用,卸载**

这7个阶段.其中其中验证、准备、解析3个部分统称为连接.

![](https://tva1.sinaimg.cn/large/e6c9d24ely1h3yceabh3vj212m0dymy9.jpg)

加载、验证、准备、初始化和卸载这五个阶段的顺序是确定的，

类型的加载过程必须按照这种顺序按部就班地开始.

解析阶段则不一定：它在某些情况下可以在初始化阶段之后再开始，这是为了支持Java语言的运行时绑定特性（也称为动态绑定或晚期绑定）

**注意，这里的几个阶段是按顺序开始，而不是按顺序进行或完成，因为这些阶段通常都是互相交叉地混合进行的，通常在一个阶段执行的过程中调用或激活另一个阶段。(简单讲这里是多线程进行的  不比等某一任务结束才开始下一阶段)**



### 加载

**加载：**查找并加载类的二进制数据

在加载阶段，虚拟机需要完成以下3件事情：

1. 通过一个类的全限定名来获取定义此类的二进制字节流。
2. 将这个字节流所代表的静态存储结构转化为方法区的运行时数据结构。
3. 在内存中(堆区)生成一个代表这个类的java.lang.Class对象，作为方法区这个类的各种数据的访问入口。

![](https://tva1.sinaimg.cn/large/e6c9d24ely1h3yckc2blsj20ya0icgna.jpg)

> 注释:
>
> + 全限定类名：类名全称，带包路径的用点隔开，例如: `java.lang.String。`
>   即全限定名 = 包名+类型(类名)
>
> + 非限定类名也叫短名，就是我们平时说的类名，不带包的，例如：String。
>
>   
>
>   非限定类名是相对于限定类名来说的，在Java中有很多类，不同的类之间会存在相同的函数或者方法，所以有时候就需要限定类名来调包。
>   而如果不存在相同的函数或者方法 ，就可以使用非限定类名。



### 验证

**验证：**确保被加载的类的正确性

验证是连接阶段的第一步，这一阶段的目的是为了确保Class文件的字节流中包含的信息符合当前虚拟机的要求，并且不会危害虚拟机自身的安全。验证阶段大致会完成4个阶段的检验动作:

- **文件格式验证:** 验证字节流是否符合Class文件格式的规范；例如: 是否以`0xCAFEBABE`开头、主次版本号是否在当前虚拟机的处理范围之内、常量池中的常量是否有不被支持的类型。
- **元数据验证：**对字节码描述的信息进行语义分析(注意: 对比`javac`编译阶段的语义分析)，以保证其描述的信息符合Java语言规范的要求；例如: 这个类是否有父类，除了`java.lang.Object`之外。
- **字节码验证：**通过数据流和控制流分析，确定程序语义是合法的、符合逻辑的。
- **符号引用验证：**确保解析动作能正确执行。

验证阶段是非常重要的，但不是必须的，它对程序运行期没有影响，如果所引用的类经过反复验证，那么可以考虑采用`-Xverifynone`参数来关闭大部分的类验证措施，以缩短虚拟机类加载的时间。



### 准备

**准备：**为类的静态变量分配内存，并将其初始化为默认值

**准备阶段是正式为类变量分配内存并设置类变量初始值的阶段，这些变量所使用的内存都将在方法区中进行分配**

该阶段的注意事项：

- 这时候进行内存分配的仅包括类变量（被static修饰的变量），而不包括实例变量，实例变量将会在对象实例化时随着对象一起分配在Java堆中。
- 这里所设置的初始值通常情况下是数据类型默认的零值(如`0`、`0L`、`null`、`false`等)，而不是被在Java代码中被显式地赋予的值。

比如：假设一个类变量的定义为: `public static int value = 3`；那么变量value在准备阶段过后的初始值为`0`，而不是`3`，因为这时候尚未开始执行任何Java方法，而把value赋值为3的`put static`指令是在程序编译后，存放于类构造器`()`方法之中的，所以把value赋值为3的动作将在初始化阶段才会执行。

- 对基本数据类型来说，对于类变量(static)和全局变量，如果不显式地对其赋值而直接使用，则系统会为其赋予默认的零值，而对于局部变量来说，在使用前必须显式地为其赋值，否则编译时不通过。
- 对于同时被`static`和`final`修饰的常量，必须在声明的时候就为其显式地赋值，否则编译时不通过；而只被final修饰的常量则既可以在声明时显式地为其赋值，也可以在类初始化时显式地为其赋值，总之，在使用前必须为其显式地赋值，系统不会为其赋予默认零值。
- 对于引用数据类型`reference`来说，如数组引用、对象引用等，如果没有对其进行显式地赋值而直接使用，系统都会为其赋予默认的零值，即`null`。
- 如果在数组初始化时没有对数组中的各元素赋值，那么其中的元素将根据对应的数据类型而被赋予默认的零值。
- 如果类字段的字段属性表中存在ConstantValue属性，即同时被final和static修饰，那么在准备阶段变量value就会被初始化为ConstValue属性所指定的值。假设上面的类变量value被定义为: `public static final int value = 3；`编译时Javac将会为value生成ConstantValue属性，在准备阶段虚拟机就会根据ConstantValue的设置将value赋值为3。我们可以理解为`static final`常量在编译期就将其结果放入了调用它的类的常量池中



### 解析

**解析：**把类中的符号引用转换为直接引用

**解析阶段是虚拟机将常量池内的符号引用替换为直接引用的过程**，解析动作主要针对`类`或`接口`、`字段`、`类方法`、`接口方法`、`方法类型`、`方法句柄`和`调用点`限定符7类符号引用进行。符号引用就是一组符号来描述目标，可以是任何字面量。

`直接引用`就是直接指向目标的指针、相对偏移量或一个间接定位到目标的句柄。



### 初始化

**初始化：**对类的静态变量，静态代码块执行初始化操作

初始化，为类的静态变量赋予正确的初始值，JVM负责对类进行初始化，主要对类变量进行初始化。

在Java中对类变量(静态变量)进行初始值设定有两种方式:

- 声明类变量(静态变量)时指定初始值
- 使用静态代码块为类变量(静态变量)指定初始值

> 注释: 
>
> + 类变量:在java中，类变量（也叫静态变量）是类中独立于方法之外的变量，用static 修饰。（static表示“全局的”、“静态的”，用来修饰成员变量和成员方法，或静态代码块（静态代码块独立于类成员，jvm加载类时会执行静态代码块，每个代码块只执行一次，按顺序执行））。



#### 类初始化的步骤

- 假如这个类还没有被加载和连接，则程序先加载并连接该类
- 假如该类的直接父类还没有被初始化，则先初始化其直接父类
- 假如类中有初始化语句，则系统依次执行这些初始化语句



#### 触发类初始化的时机

只有当对类的主动使用的时候才会导致类的初始化，类的主动使用包括以下六种:

1. 使用new关键字实例化对象的时候。
2. 读取或设置一个类(型)的静态字段（被final修饰、已在编译期把结果放入常量池的静态字段除外）的时候。
3. 调用一个类(型)的静态方法的时候。
4. 使用java.lang.reflect包的方法对类型进行反射调用的时候，如果类型没有进行过初始化，则需要先触发其初始化。
5. 当初始化类的时候，如果发现其父类还没有进行过初始化，则需要先触发其父类的初始化。
6. 当虚拟机启动时，用户需要指定一个要执行的主类（包含main()方法的那个类），虚拟机会先初始化这个主类。



#### 以下几种情况不会执行类初始化

1. 通过子类引用父类的静态字段，只会触发父类的初始化，而不会触发子类的初始化。
2. 定义对象数组，不会触发该类的初始化。
3. 常量在编译期间会存入调用类的常量池中，本质上并没有直接引用定义常量的类，不会触发定义常量所在的类。
4. 通过类名获取 Class 对象，不会触发类的初始化。
5. 通过 Class.forName 加载指定类时，如果指定参数 initialize 为 false 时，也不会触发类初始化，其实这个参数是告诉虚拟机，是否要对类进行初始化。
6. 通过 ClassLoader 默认的 loadClass 方法，也不会触发初始化动作。



### 使用

类访问方法区内的数据结构的接口， 对象是Heap区(堆区)的数据。



### 卸载

**Java虚拟机将结束生命周期的几种情况**

- 执行了System.exit()方法
- 程序正常执行结束
- 程序在执行过程中遇到了异常或错误而异常终止
- 由于操作系统出现错误而导致Java虚拟机进程终止



## 类加载器

### 什么是类加载器

虚拟机设计团队把类加载阶段中的`通过一个类的全限定名来获取描述此类的二进制字节流`这个动作放到Java虚拟机外部去实现，以便让应用程序自己决定如何去获取所需要的类。 实现这个动作的代码模块称为`类加载器`。

### 类加载器的层次

 <img src="https://tva1.sinaimg.cn/large/e6c9d24ely1h3ydjq13uqj20vx0u0ace.jpg" style="zoom:30%;" />

双亲委派模型要求除了顶层的启动类加载器外，其余的类加载器都应有自己的父类加载器。不过这里类加载器之间的父子关系一般不是以继承（Inheritance）的关系来实现的，而是通常使用组合（Composition）关系来复用父加载器的代码。

从Java虚拟机的角度来讲，只存在两种不同的类加载器：一种是启动类加载器（Bootstrap ClassLoader），这个类加载器使用C++语言实现，是虚拟机自身的一部分；另一种就是所有其他的类加载器，这些类加载器都由Java语言实现，独立于虚拟机外部，并且全都继承自抽象类java.lang.ClassLoader。

从Java开发人员的角度来看，类加载器还可以划分得更细致一些，绝大部分Java程序都会使用到以下3种系统提供的类加载器:

1. 启动类加载器(Bootstrap ClassLoader)

   这个类加载器负责将存放在`＜JAVA_HOME＞\lib`目录中的，或者被`-Xbootclasspath`参数所指定的路径中的，并且是虚拟机识别的（按照文件名识别，如`rt.jar、tools.jar`，名字不符合的类库即使放在lib目录中也不会被加载）类库加载到虚拟机内存中。

2. 扩展类加载器(Extension ClassLoader)

   这个加载器由`sun.misc.Launcher$ExtClassLoader`实现，它负责加载`＜JAVA_HOME＞\lib\ext`目录中的，或者被`java.ext.dirs`系统变量所指定的路径中的所有类库，开发者可以直接使用扩展类加载器。

3. 应用程序类加载器(Application ClassLoader)

   这个类加载器由`sun.misc.Launcher$AppClassLoader`来实现。由于应用程序类加载是`ClassLoader`类中的`getSystem-ClassLoader()`方法的返回值，所以有些场合中也称它为“系统类加载器”。

   它负责加载用户类路径（ClassPath）上所有的类库，开发者同样可以直接在代码中使用这个类加载器。如果应用程序中没有自定义过自己的类加载器，一般情况下这个就是程序中默认的类加载器。

   我们的应用程序都是由这3种类加载器互相配合进行加载的，如果有必要，还可以加入自己定义的类加载器。

**类加载的三种方式**

1. 命令行启动应用时候由JVM初始化加载
2. 通过Class.forName()方法动态加载
3. 通过ClassLoader.loadClass()方法动态加载

``` java
public class loaderTest { 
  public static void main(String[] args) throws ClassNotFoundException { 
    ClassLoader loader = HelloWorld.class.getClassLoader(); 
    System.out.println(loader); 
    //1. 使用ClassLoader.loadClass()来加载类，不会执行初始化块 
    loader.loadClass("Test2"); 
    //2. 使用Class.forName()来加载类，默认会执行初始化块 
    //Class.forName("Test2"); 
    //3. 使用Class.forName()来加载类，并指定ClassLoader，初始化时不执行静态块 
    //Class.forName("Test2", false, loader); 
  } 
}

public class Test2 { 
  static { 
    System.out.println("静态初始化块执行了！"); 
  } 
}
```

`Class.forName()`和`ClassLoader.loadClass()`区别

- `Class.forName()`: 将类的`.class`文件加载到jvm中之外，还会对类进行解释，执行类中的`static`块；
- `ClassLoader.loadClass()`: 只干一件事情，就是将`.class`文件加载到jvm中，不会执行static中的内容,只有在`newInstance`才会去执行`static`块。
- `Class.forName(name, initialize, loader)`带参函数也可控制是否加载static块。并且只有调用了`newInstance()`方法采用调用构造函数，创建类的对象 。



## JVM类加载机制

### 全盘负责

当一个类加载器负责加载某个`Class`时，该`Class`所依赖的和引用的其他`Class`也将由该类加载器负责载入，除非显式使用另外一个类加载器来载入。

### 父类委托

先让父类加载器试图加载该类，只有在父类加载器无法加载该类时才尝试从自己的类路径中加载该类。

### 缓存机制

缓存机制将会保证所有加载过的Class都会被缓存，当程序中需要使用某个`Class`时，类加载器先从缓存区寻找该`Class`，只有缓存区不存在，系统才会读取该类对应的二进制数据，并将其转换成`Class`对象，存入缓存区。这就是为什么修改了`Class`后，必须重启JVM，程序的修改才会生效。

### 双亲委派机制

双亲委派模型的工作过程是：如果一个类加载器收到了类加载的请求，它首先不会自己去尝试加载这个类，而是把这个请求委派给父类加载器去完成，每一个层次的类加载器都是如此，因此所有的加载请求最终都应该传送到最顶层的启动类加载器中，只有当父加载器反馈自己无法完成这个加载请求（它的搜索范围中没有找到所需的类）时，子加载器才会尝试自己去完成加载。

举例如下：

1. 当`AppClassLoader`加载一个`class`时，它首先不会自己去尝试加载这个类，而是把类加载请求委派给父类加载器`ExtClassLoader`去完成。
2. 当`ExtClassLoader`加载一个`class`时，它首先也不会自己去尝试加载这个类，而是把类加载请求委派给`BootStrapClassLoader`去完成。
3. 如果`BootStrapClassLoader`加载失败(例如在`$JAVA_HOME/jre/lib`里未查找到该`class`)，会使用`ExtClassLoader`来尝试加载；
4. 若`ExtClassLoader`也加载失败，则会使用`AppClassLoader`来加载，如果`AppClassLoader`也加载失败，则会报出异常`ClassNotFoundException`。

> 注释:
>
> “双亲委派”这个词估计也就是翻译错误的问题，或者是这样一种可能性。相对于AppClassLoader，即应用程序类加载器。它加载我们项目（工程）下的 CLASSPATH 路径下的类，它会委托 ExtClassLoader 标准扩展（Extension）类加载器（也有称作扩展类加载器），这时 ExtClassLoader 会再次委派 BootstrapClassLoader 启动类加载器。BootstrapClassLoader 是 Java 虚拟机的第一个类加载器，它不能再向上委托了。因此，根据这个过程，我们发现一共委托了两次，所以“双亲委派”中有一个双。而“亲”字，在中国代表的是亲人的意思，而委托两次，都是交给父类来处理，因此都算得上叫亲人。所以“双亲委派”中的双亲应该就是这样来的。
>
> 总结（对 Parents Delegation Model 翻译为的 双亲委派机制 理解）：
>
> 1. 翻译的问题，parents 翻译为父母，即双亲
>
> 2. AppClassLoader 向上委托了两次，即“双”，“亲”代表亲人的意思
>
> 3. 可以直接理解成父委派模型


![](https://tva1.sinaimg.cn/large/007S8ZIlly1gi5brab180j31iy0m87qk.jpg)



双亲委派优势

- 系统类防止内存中出现多份同样的字节码
- 保证Java程序安全稳定运行

双亲委派代码实现:

``` java
public abstract class ClassLoader {
    //每个类加载器都有个父加载器
    private final ClassLoader parent;
    public Class<?> loadClass(String name) {
        //查找一下这个类是不是已经加载过了
        Class<?> c = findLoadedClass(name);
        //如果没有加载过
        if( c == null ){
          //先委派给父加载器去加载，注意这是个递归调用
          if (parent != null) {
              c = parent.loadClass(name);
          }else {
              // 如果父加载器为空，查找Bootstrap加载器是不是加载过了
              c = findBootstrapClassOrNull(name);
          }
        }
        // 如果父加载器没加载成功，调用自己的findClass去加载
        if (c == null) {
            c = findClass(name);
        }
        return c；
    }
    protected Class<?> findClass(String name){
       //1. 根据传入的类名name，到在特定目录下去寻找类文件，把.class文件读入内存
          ...
       //2. 调用defineClass将字节数组转成Class对象
       return defineClass(buf, off, len)；
    }
    // 将字节码数组解析成一个Class对象，用native方法实现
    protected final Class<?> defineClass(byte[] b, int off, int len){
       ...
    }
}
```

这段代码的逻辑清晰易懂：先检查请求加载的类型是否已经被加载过，若没有则调用父加载器的loadClass()方法，若父加载器为空则默认使用启动类加载器作为父加载器。假如父类加载器加载失败，抛出ClassNotFoundException异常的话，才调用自己的findClass()方法尝试进行加载。

### 自定义类加载器

通常情况下，我们都是直接使用系统类加载器。但是，有的时候，我们也需要自定义类加载器。比如应用是通过网络来传输 Java 类的字节码，为保证安全性，这些字节码经过了加密处理，这时系统类加载器就无法对其进行加载，这样则需要自定义类加载器来实现。自定义类加载器一般都是继承自 `ClassLoader` 类，从上面对 `loadClass` 方法来分析来看，我们只需要重写 `findClass` 方法即可。

下面我们通过一个示例来演示自定义类加载器的流程:

自定义类加载器的核心在于对字节码文件的获取，如果是加密的字节码则需要在该类中对文件进行解密。由于这里只是演示，我并未对`class`文件进行加密，因此没有解密的过程。

``` java
public class MyClassLoader extends ClassLoader {
    private String root;
    protected Class<?> findClass(String name) throws ClassNotFoundException {
        byte[] classData = loadClassData(name);
        if (classData == null) {
            throw new ClassNotFoundException();
        } else {
            return defineClass(name, classData, 0, classData.length);
        }
    }
    private byte[] loadClassData(String className) {
        String fileName = root + File.separatorChar
                + className.replace('.', File.separatorChar) + ".class";
        try {
            InputStream ins = new FileInputStream(fileName);
            ByteArrayOutputStream baos = new ByteArrayOutputStream();
            int bufferSize = 1024;
            byte[] buffer = new byte[bufferSize];
            int length = 0;
            while ((length = ins.read(buffer)) != -1) {
                baos.write(buffer, 0, length);
            }
            return baos.toByteArray();
        } catch (IOException e) {
            e.printStackTrace();
        }
        return null;
    }
    public String getRoot() {
        return root;
    }
    public void setRoot(String root) {
        this.root = root;
    }
    public static void main(String[] args)  {
        MyClassLoader classLoader = new MyClassLoader();
        classLoader.setRoot("D:\\temp");
        Class<?> testClass = null;
        try {
            testClass = classLoader.loadClass("com.pdai.jvm.classloader.Test2");
            Object object = testClass.newInstance();
            System.out.println(object.getClass().getClassLoader());
        } catch (ClassNotFoundException e) {
            e.printStackTrace();
        } catch (InstantiationException e) {
            e.printStackTrace();
        } catch (IllegalAccessException e) {
            e.printStackTrace();
        }
    }
}
```

**这里有几点需要注意** :

1. 这里传递的文件名需要是类的全限定性名称，即`com.pdai.jvm.classloader.Test2`格式的，因为 `defineClass` 方法是按这种格式进行处理的。
2. 最好不要重写`loadClass`方法，因为这样容易破坏双亲委托模式。
3. 这类Test 类本身可以被 `AppClassLoader` 类加载，因此我们不能把`com/pdai/jvm/classloader/Test2.class `放在类路径下。否则，由于双亲委托机制的存在，会直接导致该类由 `AppClassLoader` 加载，而不会通过我们自定义类加载器来加载。







