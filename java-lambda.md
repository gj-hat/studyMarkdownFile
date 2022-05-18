## 一、背景

Lambda表达式是Java SE 8中一个重要的新特性。lambda表达式允许你通过表达式来代替功能接口。 lambda表达式就和方法一样,它提供了一个正常的参数列表和一个使用这些参数的主体(body,可以是一个表达式或一个代码
块)。 Lambda 表达式（Lambda expression）可以看作是一个匿名函数，基于数学中的λ演算得名，也可称为闭包（Closure）



### 1.Lambda表达式的语法

**基本语法:** `(parameters) -> expression 或 (parameters) ->{ statements; }`

**Lambda表达式由三部分组成：**

- 1.paramaters：类似方法中的形参列表，这里的参数是函数式接口里的参数。这里的参数类型可以明确的声明 也可不声明而由JVM隐含的推断。另外当只有一个推断类型时可以省略掉圆括号。
- 2.->：可理解为“被用于”的意思
- 3.方法体：可以是表达式也可以代码块，是函数式接口里方法的实现。代码块可返回一个值或者什么都不反 回，这里的代码块块等同于方法的方法体。如果是表达式，也可以返回一个值或者什么都不反回。

```java
// 1. 不需要参数,返回值为 2
()->2
// 2. 接收一个参数(数字类型),返回其2倍的值
x->2*x
// 3. 接受2个参数(数字),并返回他们的和
(x,y) -> x+y
// 4. 接收2个int型整数,返回他们的乘积
(int x,int y) -> x * y
// 5. 接受一个 string 对象,并在控制台打印,不返回任何值(看起来像是返回void)
(String s) -> System.out.print(s)
```



### 2.函数式接口

要了解`Lambda`表达式,首先需要了解什么是函数式接口，函数式接口定义：一个接口有且只有一个抽象方法 。

> **注意：**
>
> 1.如果一个接口只有一个抽象方法，那么该接口就是一个函数式接口
> 2.如果我们在某个接口上声明了@FunctionalInterface注解，那么编译器就会按照函数式接口的定义来要求该接口，这样如果有两个抽象方法，程序编译就会报错的。所以，从某种意义上来说，只要你保证你的接口 中只有一个抽象方法，你可以不加这个注解。加上就会自动进行检测的。

**定义方式：**

```java
@FunctionalInterface
interface NoParameterNoReturn {
    //注意：只能有一个抽象方法
    void test();
}
```

**但是这种方式也是可以的：**

```java
@FunctionalInterface
interface NoParameterNoReturn {
    void test();
    default void test2() {
        System.out.println("JDK1.8新特性，default默认方法可以有具体的实现");
    }
}
```



## 二、Lambda表达式的基本使用

**首先，我们实现准备好几个接口：**

```java
@FunctionalInterface
interface NoParameterNoReturn {
    //注意：只能有一个抽象方法
    void test();
}

//无返回值一个参数
@FunctionalInterface
interface OneParameterNoReturn {void test(int a);}

//无返回值多个参数
@FunctionalInterface
interface MoreParameterNoReturn {void test(int a, int b);}

//有返回值无参数
@FunctionalInterface
interface NoParameterReturn {int test();}

//有返回值一个参数
@FunctionalInterface
interface OneParameterReturn {int test(int a);}

//有返回值多参数
@FunctionalInterface
interface MoreParameterReturn {int test(int a, int b);}
```

我们在上面提到过，`Lambda`表达式本质是一个匿名函数，函数的方法是：返回值 方法名 参数列表 方法体。在，Lambda表达式中我们只需要关心：参数列表 方法体。

**具体使用见以下示例代码：**

```java
@FunctionalInterface
interface NoParameterNoReturn {
    //注意：只能有一个抽象方法
    void test();
}

//无返回值一个参数
@FunctionalInterface
interface OneParameterNoReturn {void test(int a);}

//无返回值多个参数
@FunctionalInterface
interface MoreParameterNoReturn {void test(int a, int b);}

//有返回值无参数
@FunctionalInterface
interface NoParameterReturn {int test();}

//有返回值一个参数
@FunctionalInterface
interface OneParameterReturn {int test(int a);}

//有返回值多参数
@FunctionalInterface
interface MoreParameterReturn {int test(int a, int b);}


public class TestDemo2 {
    public static void main(String[] args) {

        NoParameterNoReturn noParameterNoReturn = () -> {
            System.out.println("无参数无返回值");
        };
        //test方法的主体内容在上述括号内
        noParameterNoReturn.test();

        OneParameterNoReturn oneParameterNoReturn = (int a) -> {
            System.out.println("无参数一个返回值：" + a);
        };
        oneParameterNoReturn.test(10);

        MoreParameterNoReturn moreParameterNoReturn = (int a, int b) -> {
            System.out.println("无返回值多个参数：" + a + " " + b);
        };
        moreParameterNoReturn.test(20, 30);

        NoParameterReturn noParameterReturn = () -> {
            System.out.println("有返回值无参数！");
            return 40;
        };
        //接收函数的返回值
        int ret = noParameterReturn.test();
        System.out.println(ret);

        OneParameterReturn oneParameterReturn = (int a) -> {
            System.out.println("有返回值有参数！");
            return a;
        };

        ret = oneParameterReturn.test(50);
        System.out.println(ret);

        MoreParameterReturn moreParameterReturn = (int a, int b) -> {
            System.out.println("有返回值多个参数！");
            return a + b;
        };
        ret = moreParameterReturn.test(60, 70);
        System.out.println(ret);
    }
}
```



## 三、语法精简

- 参数类型可以省略，如果需要省略，每个参数的类型都要省略。
- 参数的小括号里面只有一个参数，那么小括号可以省略
- 如果方法体当中只有一句代码，那么大括号可以省略
- 如果方法体中只有一条语句，其是return语句，那么大括号可以省略，且去掉return关键字。

**示例代码:**

```java
@FunctionalInterface
interface NoParameterNoReturn {
    //注意：只能有一个抽象方法
    void test();
}

//无返回值一个参数
@FunctionalInterface
interface OneParameterNoReturn {void test(int a);}

//无返回值多个参数
@FunctionalInterface
interface MoreParameterNoReturn {void test(int a, int b);}

//有返回值无参数
@FunctionalInterface
interface NoParameterReturn {int test();}

//有返回值一个参数
@FunctionalInterface
interface OneParameterReturn {int test(int a);}

//有返回值多参数
@FunctionalInterface
interface MoreParameterReturn {int test(int a, int b);}

public class TestDemo2 {
    public static void main(String[] args) {

        //方法参数有多个且方法体中无返回值，则可以省略参数类型
        MoreParameterNoReturn moreParameterNoReturn = (a, b) -> {
            System.out.println("无返回值多个参数，省略参数类型：" + a + " " + b);
        };
        moreParameterNoReturn.test(20, 30);

        //方法中只有一个参数，那么小括号可以省略
        OneParameterNoReturn oneParameterNoReturn = a -> {
            System.out.println("方法中只有一个参数，那么小括号可以省略：" + a);
        };
        oneParameterNoReturn.test(10);

        //无参数无返回值，方法体中只有 一行代码的时候，可以去掉方法体的大括号
        NoParameterNoReturn noParameterNoReturn = () -> System.out.println("无参数无返回值，方法体中只有 一行代码");
        noParameterNoReturn.test();

        //方法体中只有一条语句，且是return语句，且无参数
        NoParameterReturn noParameterReturn = () -> 40;
        int ret = noParameterReturn.test();
        System.out.println(ret);
    }
}
```



## 四、变量捕获

Lambda 表达式中存在变量捕获 ，了解了变量捕获之后，我们才能更好的理解Lambda 表达式的作用域 。Java当中的匿名类中，会存在变量捕获。

**下面我们来讲下在Lambda当中也可以进行变量的捕获，具体我们看一下代码：**

```java
@FunctionalInterface
interface NoParameterNoReturn {
    void test();
}
public class TestDemo2 {
    public static void main(String[] args) {
        int a = 10;
        NoParameterNoReturn noParameterNoReturn = () -> {
            /*
            注意此处不能够修改a的值，与匿名内部类中相同
            a = 99;
            */
            System.out.println("捕获变量：" + a);
        };
        noParameterNoReturn.test();
    }
}
```



## 五、Lambda在集合当中的使用

为了能够让Lambda和Java的集合类集更好的一起使用，集合当中，也新增了部分接口，以便与Lambda表达式对接。

![img](https://img.jbzj.com/file_images/article/202202/20222311162015.png?202213111824)

以上方法的作用可自行查看我们发的帮助手册。我们这里会示例一些方法的使用。注意：Collection的forEach()方法是从接口 java.lang.Iterable 拿过来的。



### 1.Collection接口

forEach() 方法演示

**该方法在接口 Iterable 当中，原型如下：**

![img](https://img.jbzj.com/file_images/article/202202/202223111620150%20.png?20221311217)

**forEach()**方法表示：对容器中的每个元素执行action指定的动作

**可以看到我们的参数Consumer其实是一个函数式接口：**

![img](https://img.jbzj.com/file_images/article/202202/202223111620152.png?202213112134)



**这个函数式接口中有一个抽象方法accept:**

![img](https://img.jbzj.com/file_images/article/202202/202223111620153.png?202213112223)

```java
public class TestDemo2 {
    public static void main(String[] args) {
        ArrayList<String> list = new ArrayList<>();
        list.add("Hello");
        list.add("bit");
        list.add("hello");
        list.add("lambda");
        list.forEach(new Consumer<String>() {
            @Override
            public void accept(String s) {
                //简单遍历集合中的元素
                System.out.println(s);
            }
        });
    }
}
```

**输出结果：**

> Hello bit hello lambda

**我们可以修改为如下代码：**

```java
public class TestDemo2 {
    public static void main(String[] args) {
        ArrayList<String> list = new ArrayList<>();
        list.add("Hello");
        list.add("bit");
        list.add("hello");
        list.add("lambda");
         list.forEach((String s) -> {
            System.out.println(s);
        });
    }
}
```

**同时还可以简化代码：**

```java
public class TestDemo2 {
    public static void main(String[] args) {
        ArrayList<String> list = new ArrayList<>();
        list.add("Hello");
        list.add("bit");
        list.add("hello");
        list.add("lambda");
        list.forEach(s -> System.out.println(s));
    }
}
```



## 六、List接口



### 1.sort()方法的演示

**sort方法源码：**该方法根据c指定的比较规则对容器元素进行排序。

**可以看到其参数是Comparator，我们点进去看下：**又是一个函数式接口

![img](https://img.jbzj.com/file_images/article/202202/202223111620154.png?202213112413)

**这个接口中有一个抽象方法叫做compare方法：**

![img](https://img.jbzj.com/file_images/article/202202/202223111620155.png?202213112441)

**使用示例：**

```java
public class TestDemo2 {
    public static void main(String[] args) {
        ArrayList<String> list = new ArrayList<>();
        list.add("Hello");
        list.add("bit");
        list.add("hello");
        list.add("lambda");
        /*
        对list集合中的字符串按照长度进行排序
         */
        list.sort(new Comparator<String>() {
            @Override
            public int compare(String o1, String o2) {
                return o1.length() - o2.length();
            }
        });
        /*
        输出排序后最终的结果
         */
        list.forEach(s -> System.out.println(s));
    }
}
```

**输出结果为：**

> bit Hello hello lambda

**修改为lambda表达式：**

```java
public class TestDemo2 {
    public static void main(String[] args) {
        ArrayList<String> list = new ArrayList<>();
        list.add("Hello");
        list.add("bit");
        list.add("hello");
        list.add("lambda");
        /*
        对list集合中的字符串按照长度进行排序
         */
        list.sort((String o1, String o2) -> {
                    return o1.length() - o2.length();
                }
        );
        /*
        输出排序后最终的结果:
        bit
        Hello
        hello
        lambda
         */
        list.forEach(s -> System.out.println(s));
    }
}
```

**此时还可以对代码进行简化：**

```java
public class TestDemo2 {
    public static void main(String[] args) {
        ArrayList<String> list = new ArrayList<>();
        list.add("Hello");
        list.add("bit");
        list.add("hello");
        list.add("lambda");
        /*
        对list集合中的字符串按照长度进行排序
         */
        list.sort((o1, o2) ->
                o1.length() - o2.length()
        );
        /*
        输出排序后最终的结果:
        bit
        Hello
        hello
        lambda
         */
        list.forEach(s -> System.out.println(s));
    }
}
```



## 七、Map接口

**HashMap 的 forEach()方法：**

![img](https://img.jbzj.com/file_images/article/202202/202223111620156.png?202213112558)

**这个函数式接口中有一个抽象方法叫做accept方法：**

![img](https://img.jbzj.com/file_images/article/202202/202223111620157.png?202213112656)

**代码示例：**

```java
public class TestDemo2 {
    public static void main(String[] args) {
        HashMap<Integer, String> map = new HashMap<>();
        map.put(1, "hello");
        map.put(2, "bit");
        map.put(3, "hello");
        map.put(4, "lambda");
        map.forEach(new BiConsumer<Integer, String>() {
            @Override
            public void accept(Integer integer, String s) {
                System.out.println(integer + " " + s);
            }
        });
    }
}
```

**输出结果：**

> 1 hello
> 2 bit
> 3 hello
> 4 lambda

**使用lambda表达式后的代码：**

```java
public class TestDemo2 {
    public static void main(String[] args) {
        HashMap<Integer, String> map = new HashMap<>();
        map.put(1, "hello");
        map.put(2, "bit");
        map.put(3, "hello");
        map.put(4, "lambda");

        map.forEach((Integer integer, String s) -> {
                    System.out.println(integer + " " + s);
                }
        );
    }
}
```

**还可以对代码继续简化：**

```java
public class TestDemo2 {
    public static void main(String[] args) {
        HashMap<Integer, String> map = new HashMap<>();
        map.put(1, "hello");
        map.put(2, "bit");
        map.put(3, "hello");
        map.put(4, "lambda");
 
        map.forEach((integer, s) ->
                System.out.println(integer + " " + s)
        );
    }
}
```

**总结：**

Lambda表达式的优点很明显，在代码层次上来说，使代码变得非常的简洁。缺点也很明显，代码不易读。

**优点：**

代码简洁，开发迅速
方便函数式编程
非常容易进行并行计算
Java 引入 Lambda，改善了集合操作

**缺点：**

代码可读性变差
在非并行计算中，很多计算未必有传统的 for 性能要高
不容易进行调试