# SpringBoot常用注解

### 是什么



### 怎么用



### 使用场景





## @Scope注解

### 是什么

@Scope注解是springIoc容器中的一个作用域，在 Spring IoC 容器中具有以下几种作用域：基本作用域singleton（单例）、prototype(多例)，Web 作用域（reqeust、session、globalsession），自定义作用域

+ singleton单例模式 -- 全局有且仅有一个实例
+ prototype原型模式 -- 每次获取Bean的时候会有一个新的实例
+ request -- request表示该针对每一次HTTP请求都会产生一个新的bean，同时该bean仅在当前HTTP request内有效
+ session -- session作用域表示该针对每一次HTTP请求都会产生一个新的bean，同时该bean仅在当前HTTP session内有效
+ globalsession -- global session作用域类似于标准的HTTP Session作用域，不过它仅仅在基于portlet的web应用中才有意义



### 怎么用

1. 通过源码可知 可以用在类或者方法上

   ``` java
   @Target({ElementType.TYPE, ElementType.METHOD})
   @Retention(RetentionPolicy.RUNTIME)
   @Documented
   public @interface Scope {
       @AliasFor("scopeName")
       String value() default "";
       @AliasFor("value")
       String scopeName() default "";
       ScopedProxyMode proxyMode() default ScopedProxyMode.DEFAULT;
   }
   ```

2. 使用的时候需要结合`@Bean`等可以放入spring容器中的注解

3. 例如:

   ``` java
   //  原始Bean
   @Component
   @Scope("prototype")
   @Data
   public class ScopeTest {
       public int a = 0;
   }
   
   // 测试时候先访问第一个 再访问第二个Controller
   // 下面是第一个Controller类  没有写RestController等注解
   @Autowired
   private ScopeTest scopeTest;
   public String test(){
     scopeTest.a = 3;
   	System.out.println("scopeTest.a = " + scopeTest.a);
   }
   // 下面是第二个Controller类  没有写RestController等注解
   @Autowired
   private ScopeTest scopeTest;
   public String test(){
   	System.out.println("scopeTest.a = " + scopeTest.a);
   }
   ```

4. 如果`@Scope("prototype")`则两个类注入的ScopeTest相互独立  一个打印3 一个打印0

5. 如果`@Scope("singleton")`则两个类注入的ScopeTest是同一个  都打印3 



### 使用场景

几乎90%以上的业务使用singleton单实例就可以，所以spring默认的类型也是singleton，singleton虽然保证了全局是一个实例，对性能有所提高，但是如果实例中有非静态变量时，会导致线程安全问题，共享资源的竞争

当设置为prototype时：每次连接请求，都会生成一个bean实例，也会导致一个问题，当请求数越多，性能会降低，因为创建的实例，导致GC频繁，gc时长增加



## @SpringBootApplication



这个注解是SpringBoot主类的注解 标识SpringBoot的启动类



## @SpringBootTest

描述SpringBoot工程测试类的特定注解
要不要用容器的方式启动
使用条件：如果需要在测试类中，引入spring容器机制，这是才是用该注解，否则没必要加。
引入spring容器机制：比如说，我们要在测试类中，通过spring容器来注入某个类的实例时，就需要使用该注解

``` java
@RunWith(SpringRunner.class)
@SpringBootTest(classes = 启动类.class)
public class 测试的类名+Test {
   @Test
    public void test1() {}
}
```



## 自动注入类 Autowired, Qualifier, Resource

+ @Autowired  默认按type注入
+ @Qualifier("Bean名字")   一般作为@Autowired()的修饰用
+ @Resource(name="Bean名字")   默认按name注入，可以通过name和type属性进行选择性注入



前面两者是Spring的注解 后面的是J2EE的注解



一般@Autowired和@Qualifier一起用，@Resource单独用。

当然没有冲突的话@Autowired也可以单独用

``` java
@Autowired   
@Qualifier("userServiceImpl")   
public IUserService userService; 

@Autowired   
public void setUserDao(@Qualifier("userDao") UserDao userDao) {   
    this.userDao = userDao;   
} 

// 表明 可能不存在下面的Bean 不报错
@Autowired(required = false)   
public IUserService userService  
  
@Resource(name="baseDao")
public IUserService userService  
```



# @Component产生Bean

不明确这个类属于哪层时可以用该注解，将该类 的实 列交给spring容器创建



# @Controller产生Bean

SpringBoot中已经不用了

该注解用于描述控制层，将控制层的实例创建权限 交给spring容器
返回的是view



# @Service产生Bean

该注解用于描述业务层，由spring来创建@Service 描述的类的实例



# @Lazy

该注解用于延迟对象的创建， 因为单例的Bean在项目启动时候就加载 会浪费资源 使用这个注解可以在需要的时候加载



# @Test

单元测试的注解

满足以下条件

1. 访问修饰符不能为private
2. 返回值必须为void
3. 方法参数必须时无参
4. 测试方法允许抛出异常



# @Bean产生Bean

一般写在配置类的方法中

通常用于配置类中，与@Configuration配合使用

@Bean描述的方法的返回值（返回的是一个对象）交给spring管理。此时可以配合@Lazy注解使用



# @Mapper产生Bean

添加在数据层的接口上，

通知spring，该接口的实现类有mybatis负责实现，

该实现类的对象有mybatis创建，但是交给spring管理



# @MapperScan("接口文件的包名")

添加在主启动类上，利用包扫描的方式，为指定包下的接口创建代理类以及代理对象并交给spring管理

**讲人话: 在启动类上写  引号下的所有interface类型的类都会生成Bean**

优势：不用在每个映射的接口上使用@Mapper注解了

前提：要将所有的映射文件放在同一个包下。



# @ComponentScan

`@ComponentScan(basePackages={"包的路径"})`

扫描指定路径下的类，看是否有类上@Component、@Controller、@Service、@Mapper等，若有则指明该类的实例由Spring容器创建。



# @Repository

添加在数据层接口的实现类上，将这个类交给Spring管理







# @Param

这是mybatis的注解 用于获取方法参数 有四种用法



1. 方法有多个参数，需要 @Param 注解

   ``` java
   @Mapper
   public interface UserMapper {
       Integer insert(@Param("username") String username, @Param("address") String address);
   }
   
   //  对应的xml
   <insert id="insert" parameterType="org.javaboy.helloboot.bean.User">
       insert into user (username,address) values (#{username},#{address});
   </insert>
   ```

   

2. 方法参数要取别名，需要 @Param 注解

   ``` java
   @Mapper
   public interface UserMapper {
       User getUserByUsername(@Param("name") String username);
   }
   
   // xml
   <select id="getUserByUsername" parameterType="org.javaboy.helloboot.bean.User">
       select * from user where username=#{name};
   </select>
   ```



3. XML 中的 SQL 使用了 $ ，那么参数中也需要 @Param 注解

   $ 会有注入漏洞的问题，但是有的时候你不得不使用 $ 符号，例如要传入列名或者表名的时候，这个时候必须要添加 @Param 注解，例如：

   ```java
   @Mapper
   public interface UserMapper {
       List<User> getAllUsers(@Param("order_by")String order_by);
   }
   
   <select id="getAllUsers" resultType="org.javaboy.helloboot.bean.User">
       select * from user
       <if test="order_by!=null and order_by!=''">
           order by ${order_by} desc
       </if>
   </select>
   ```



4. 那就是动态 SQL 

   如果在动态 SQL 中使用了参数作为变量，那么也需要 @Param 注解，即使你只有一个参数。

   如果我们在动态 SQL 中用到了 参数作为判断条件，那么也是一定要加 @Param 注解的，例如如下方法：

   ``` java
   @Mapper
   public interface UserMapper {
       List<User> getUserById(@Param("id")Integer id);
   }
   
   <select id="getUserById" resultType="org.javaboy.helloboot.bean.User">
       select * from user
       <if test="id!=null">
           where id=#{id}
       </if>
   </select>
   ```

   



# @ReponseBody

使得方法的方法的返回值是一个字符串而不是view(一般需要搭配@Controller使用)

1. 描述的方法的返回值不是view（不是一个页面），
2. 比如说可以是一个字符串String---直接返回该字符串
3. 当方法的返回值是一个或多个pojo对象（也可以是map）时，springmvc去查看这个方法上是否有该注解，若有，就会pojo对象转成JSON格式的字符串（字符串数组）



# @RestController

等同于@Controller+@ReponseBody





# @PathVariable

当我们的方法参数要从url中获取参数时，就需要使用该注解--restful风格中常用！
如果url中的名字与方法参数名不一致，还可以指定

``` java
//  页面url="menu/menulist"


// 1.url中的名字与方法参数名一致，可以直接加上@PathVariable，不用指定别名
@RequestMapping("{do}/{dolist}")
public String domain(@PathVariable String dolist){
return dolist;
}

@RequestMapping("{do}/{dolist}")
public String domain(@PathVariable String do){
return do;
}


// 2.url中的名字与方法参数名不一致，还可以指定，表明方法参数中的变量时来自url中的哪一个
（一般不用这样的，直接写成一样就可以了（第一种））
@RequestMapping("{do}/{dolist}")
public String domain(@PathVariable("dolist")String name){
return name;
}
@RequestMapping("{do}/{dolist}")
public String domain(@PathVariable("do")String name){
return name;
}
```





# @RequestMapping

SpringMVC的注解 用于匹配发布请求路径

可用于方法或者类上

``` java
@RequestMapping("/dolist")
public String domain(String dolist){
return dolist;
}
```





# @ControllerAdvice

## 没深究  回头遇到了深究

该注解描述类为全局异常处理类
当Spring mvc（web）的控制层出现异常，首先会检查controller中是否有对应的异常处理方法，有则直接执行，若没有，就会检索是否有@ControllerAdvice描述的类，若有，则再去检索类中是否有对应的异常处理方法。。



这是一个增强的 Controller。使用这个 Controller ，可以实现三个方面的功能：

1. 全局异常处理
2. 全局数据绑定
3. 全局数据预处理



# @ExceptionHandler

## 没深究  回头遇到了深究

1. 该注解描述的方法，为一个异常处理的方法，注解中需要指定能处理的异常类型（RuntimException.class-字节码对象），表示该方法能够处理该类型的异常以及子类异常。
2. 在异常处理方法中，通常需要一个异常参数，用来接收异常对象。
3. 方法的返回值通常是一个对象（json格式-满足响应式布局），通常与@ReponseBody注解结合使用



# @Aspect

该注解描述的类为一个切面（aop）





# @Around（环绕通知）优先级最高

执行目标方法之前Spring会检测是否有切面（@Aspect描述的类），然后检测是否有通知方法（@Around注解描述的方法），有该方法则执行该方法，在方法中调用通过ProceedingJoinPoint对象jp调用proceed（）方法（该方法会执行目标方法）



# @Pointcut

## 简单示例

1. 先看一个极简的例子：所有的get请求被调用前在控制台输出一句"get请求的advice触发了"。

2. 创建一个AOP切面类，只要在类上加个 @Aspect 注解即可。@Aspect 注解用来描述一个切面类，定义切面类的时候需要打上这个注解。@Component 注解将该类交给 Spring 来管理。在这个类里实现advice：

   ``` java
   @Aspect
   @Component
   public class LogAdvice {
       // 定义一个切点:所有被 GetMapping 注解修饰的方法都会被织入 advice
       @Pointcut("@annotation(org.springframework.web.bind.annotation.GetMapping)")
       private void logAdvicePointcut(){}
       @Before("logAdvicePointcut()")
       public void logAdvice(){
           System.out.println("切面 @Before 执行了");
       }
   }
   ```

3. 创建一个接口类，内部创建一个get请求：

   ``` java
   @RestController
   @RequestMapping(value = "/aop")
   public class AopController {
       @GetMapping("/getTest")
       public JSONObject aopTest() {
           System.out.println("Get 方法代码执行中");
           return JSON.parseObject("{\"message\":\"SUCCESS\",\"code\":200}");
       }
      @PostMapping("/postTest")
       public JSONObject aopTestTwo(){
          System.out.println("Post 方法代码执行中");
           return JSON.parseObject("{\"message\":\"SUCCESS\",\"code\":200}");
       }
   }
   ```

4. 打印 Get 方法代码执行中



## 深入示例(常用)

自定义一个注解 然后使用切面去拦截

1.  使用@Target、@Retention、@Documented自定义一个注解：

   ``` java
   @Target(ElementType.METHOD)
   @Retention(RetentionPolicy.RUNTIME)
   @Documented
   public @interface PermissionAnnotation {
   }
   ```

2. 创建第一个AOP切面类，，只要在类上加个 @Aspect 注解即可。@Aspect 注解用来描述一个切面类，定义切面类的时候需要打上这个注解。@Component 注解将该类交给 Spring 来管理。在这个类里实现第一步权限校验逻辑：

   ``` java
   @Aspect
   @Component
   @Order(1)
   public class PermissionFirstAdvice {
   
    @Pointcut("@annotation(com.example.zhangchonghu.demo.controller.aop3.PermissionAnnotation)")
    private void permissionCheck(){}
   
    @Around("permissionCheck()")
    public Object permissionCheckFirst(ProceedingJoinPoint joinPoint) throws Throwable {
        System.out.println("===================第一个切面===================：" + System.currentTimeMillis());
   
        //获取请求参数，详见接口类
        Object[] objects = joinPoint.getArgs();
        Long id = ((JSONObject) objects[0]).getLong("id");
        String name = ((JSONObject) objects[0]).getString("name");
        System.out.println("id1->>>>>>>>>>>>>>>>>>>>>>" + id);
        System.out.println("name1->>>>>>>>>>>>>>>>>>>>>>" + name);
   
        return joinPoint.proceed();
    }
   }
   ```

   

3. 创建接口类，并在目标方法上标注自定义注解 PermissionsAnnotation：

   ``` java
   @RestController
   @RequestMapping("/permission")
   public class TestController {
    @RequestMapping(value = "/check", method = RequestMethod.POST)
    // 添加自定义注解
    @PermissionAnnotation()
    public JSONObject getGroupList(@RequestBody JSONObject request) {
        System.out.println("方法中打印请求参数"+request);
        return JSON.parseObject("{\"message\":\"SUCCESS\",\"code\":200}");
    }
   }
   ```



4. 到此已经完成了  下面介绍一下多个切面的顺序问题

5. 如果我一个接口想设置多个切面类进行校验怎么办？这些切面的执行顺序如何管理？

   很简单，一个自定义的AOP注解可以对应多个切面类，这些切面类执行顺序由@Order注解管理，该注解后的数字越小，所在切面类越先执行。

   ``` java
   @Aspect
   @Component
   @Order(0)
   public class PermissionSecondAdvice {
       /**
        * 自定义注解作为切面,凡是被这个注解定义的方法,都会被切面拦截
        */
      @Pointcut("@annotation(com.example.zhangchonghu.demo.controller.aop3.PermissionAnnotation)")
       private void permissionCheck(){}
   
       @Around("permissionCheck()")
       public Object permissionCheckSecond(ProceedingJoinPoint joinPoint) throws Throwable {
           System.out.println("===================第二个切面===================：" + System.currentTimeMillis());
   
           //获取请求参数，详见接口类
           Object[] objects = joinPoint.getArgs();
           Long id = ((JSONObject) objects[0]).getLong("id");
           String name = ((JSONObject) objects[0]).getString("name");
           System.out.println("id->>>>>>>>>>>>>>>>>>>>>>" + id);
           System.out.println("name->>>>>>>>>>>>>>>>>>>>>>" + name);
   
           // name不是管理员则抛出异常
           if (!"admin".equals(name)) {
               return JSON.parseObject("{\"message\":\"not admin\",\"code\":403}");
           }
           return joinPoint.proceed();
       }
   }
   ```

   

## 匹配表达式

@Pointcut 注解指定一个切面，定义需要拦截的东西，这里介绍两个常用的表达式：一个是使用 execution()，另一个是使用 annotation()。

1. execution表达式：

   以 execution(* * com.mutest.controller..*.*(..))) 表达式为例：

   第一个 * 号的位置：表示返回值类型，* 表示所有类型。

   包名：表示需要拦截的包名，后面的两个句点表示当前包和当前包的所有子包，在本例中指 com.mutest.controller包、子包下所有类的方法。

   第二个 * 号的位置：表示类名，* 表示所有类。

   *(..)：这个星号表示方法名，* 表示所有的方法，后面括弧里面表示方法的参数，两个句点表示任何参数。

   ``` java
   @Aspect
   @Component
   public class LogAspectHandler {
       /**
        * 定义一个切面，拦截 com.itcodai.course09.controller 包和子包下的所有方法
        */
       @Pointcut("execution(* com.mutest.controller..*.*(..))")
       public void pointCut() {}
   }
   ```

   

2. annotation() 表达式：

   annotation() 方式是针对某个注解来定义切面，比如我们对具有 @PostMapping 注解的方法做切面，可以如下定义切面：

   ```java
   @Pointcut("@annotation(org.springframework.web.bind.annotation.PostMapping)")
   public void annotationPointcut() {}
   ```

   然后使用该切面的话，就会切入注解是 @PostMapping 的所有方法。这种方式很适合处理 @GetMapping、@PostMapping、@DeleteMapping不同注解有各种特定处理逻辑的场景。

   还有就是如上面案例所示，针对自定义注解来定义切面:

   ```java
   @Pointcut("@annotation(com.example.demo.PermissionsAnnotation)")
   private void permissionCheck() {}
   ```



# @Order(1)

该注解用于指定切面的优先级，数字越小，优先级越高，默认值是整数的最大值即默认值的优先级是最低的



# @AfterThrowing

@Around注解描述的方法中的目标方法执行时抛出异常了，就直接执行该注解描述的方法。





# @Before生命周期

当@Around注解描述的方法中，要执行目标方法之前（proceed方法执行之前），执行该注解描述的方法



# @After生命周期



目标方法执行之后，执行此注解描述的方法（如果目标方法有异常该方法不执行）

# @AfterReturnning生命周期



@After注解描述的方法执行之后，（这个方法执行了，说明目标方法没有抛出异常）执行该注解描述的方法。





# @Slf4j(lombok中)

应用在类上，此类中就可以使用log对象
就相当于在这个类中添加了一行如下代码

```java
private static final Logger log=LoggerFactory.getLogger(类名.class);
```





# @Transactional事务

该注解可以描述类和方法，

+ 当描述类时，表示类中的所有方法在执行之前开启事务，方法执行之后结束事务。
+ 当描述方法时，表示该方法在执行之前开启事务，方法执行之后结束事务。

优先级问题：当类上和方法上都有该注解时，方法上的优先级高于类上的。

注解中的属性:

+ timeout：设置超时时间
+ isolation：设置隔离级别
+ rollbackfor：当遇到什么异常以及子类异常时要执行回滚操作。
+ read-only：指定事务是否为只读事务，默认值为 false；为了忽略那些不需要事务的方法，比如读取数据，可以设置read-only为true。对添加，修改，删除业务read-only的值应该为false。
+ propagation：Propagation.REQUIRED（如果没有事务创建新事务, 如果当前有事务参与当前事务, Spring 默认的事务传播行为是PROPAGATION_REQUIRED，它适合于绝大多数的情况。）
  Propagation.REQUIRES_NEW如果有当前事务, 挂起当前事务并且开启新事务





# @EnableAsync异步相关

注解应用到启动类上，表示开启异步，spring容器启动时会创建线程池
spring中线程池的配置：

``` yaml
spring:
    task:
        execution:
          pool:
            queue-capacity: 128
            core-size: 5
            max-size: 128
            keep-alive: 60000
            thread-name-prefix: db-service-task-
```



# @Async异步相关

在需要异步执行的业务方法上，使用@Async方法进行异步声明。

说明：AsyncResult对象可以对异步方法的执行结果进行封装，假如外界需要异步方法结果时，可以通过Future对象的get方法获取结果。
为了简便某些情况下，我们可以将方法的返回值写成void，这样就不要这么复杂的封装（前提时方法的返回值我们不使用）。

``` java
@Transactional(propagation = Propagation.REQUIRES_NEW) 
@Async
@Override
public Future<Integer> saveObject(SysLog entity) {
    System.out.println("SysLogServiceImpl.save:"+Thread._currentThread_().getName());
    int rows = sysLogDao.insertObject(entity);
    //try{Thread._sleep_(5000);}catch(Exception e) {}
     return new AsyncResult<Integer>(rows);
}
```



# @EnableCaching

在项目(SpringBoot项目)的启动类上添加@EnableCaching注解,以启动缓存配置。

> 在需要进行缓存的业务方法上通过@Cacheable注解对方法进行相关描述.表示方法的
> 返回值要存储到Cache中,假如在更新操作时需要将cache中的数据移除,可以在更新方法上使用@CacheEvict注解对方法进行描述。

```java
// value属性的值表示要使用的缓存对象,名字自己指定，其中底层为一个map对象，当向cache中添加数据时，key默认为方法实际参数的组合。
@Cacheable(value = "menuCache")
@Transactional(readOnly = true)
public List<Map<String,Object>> findObjects() {
....
}
```



# @Value

说明：该注解单独使用（不配合@PropertySource注解）时，此时配置文件已经被加载到spring容器中，表示从spring容器（加载的配置文件）中取值。

``` java
// 这个port是yml的值
@value（“${server.port}”）
private String port;
```



# @PropertySource

当配置文件已经被加载到spring容器中了，就不要使用该注解指明加载的配置文件了，容器中有直接用@Value取值即可。
说明：

+ 该注解用于类上，

作用：

+ 读取指定位置的配置文件信息，加载该文件到spring容器中
+ value属性用来指定配置文件的路径
+ classpath:/ 指定类目录下的路径（resource目录下）
+ encoding：用来指定配置文件的编码类型

``` java
@PropertySource(value="classpath:/properties/image.properties",encoding="utf-8")
public class ReadProperties{
}
```



# @Data(lombok)

该注解用在pojo类上，作用是生成对应的get/set/toString...方法
toString（）方法只输出自己的属性，不会添加继承自父类的属性。



# @Setter(lombok)

该注解用在pojo类上，生成set方法





# @Getter(lombok)

该注解用在pojo类上，生成get方法



# @NoArgsConstructor(lombok)

无参构造



# @ArgsConstructor(lombok)

全参构造



# @Accessors(lombok提供)

@Accessors用于改变@Data生成的getter和setter方法的生成结果

## 参数fluent=true

getter和setter方法的方法名直接使用属性名，且set方法可以返回当前对象

``` java
@Data
@Accessors(fluent = true)
public class User {
    private Long id;
    private String name;
    //带上注解后生成的getter和setter方法如下，方法体省略不写了
    public Long id() {}	//驼峰是getId
    public User id(Long id) {}	//原来的setId
    public String name() {}	//原来的getName
    public User name(String name) {}//是原来的setName
}
```



调用时：

``` java
User user = new User()
user.id(); /相当于user.getId();
User user = user.name(“1”) //返回当前对象
```



## 参数chain=true

只影响set，方法会返回当前对象。
get的方法不变，且get和set还是原来的小驼峰

``` java
@Data
@Accessors(chain = true)
public class User {
    private Long id;
    private String name;
    // 生成的setter方法如下，方法体略
    public User setId(Long id) {}
    public User setName(String name) {}
}
```

提供链式加载（对于set方法）

``` java
User user=new User();
user.setName("张三").setAge(26).setScore(98);
```



## prefix=“字符串”

方法还是小驼峰，set和get在忽略指定前缀后命名

用于生成getter和setter方法的字段名会忽视指定前缀。如下

``` java
@Data
@Accessors(prefix = "p")
class User {
	private Long pId;
	private String pName;
	
	//带上注解后生成的getter和setter方法如下，方法体省略不写了(和默认)
	public Long getId() {}
	public void setId(Long id) {}
	public String getName() {}
	public void setName(String name) {}
}
```



# @TableName(mybatisplus)

` @TableName（“表名”）`

该注解用在pojo类上，用来指定该类的对象，与数据库中的哪张表映射
当pojo类名忽略大小写时，与数据库中要映射的表名一致时，可以不指定表名。

如直接写@TableName注解即可。但不能省略这个注解。



# @TableId(mybatisplus)

`@TableId（type=IdType.Auto）`

> 该注解用于pojo类中的属性上，用来指定表中的主键要映射到pojo类中的哪个属性。



# @TableField(mybatisplus)

``` java
@TableField（fill = FieldFill.INSERT）指定哪个属性，在新增时有效，一般是创建时间
@TableField（fill = FieldFill.INSERT_UPDATE）指定哪个属性，在新增和修改时有效，一般是创建时间/修改时间
```



该注解用于pojo类中的属性上，用来指定表中的字段（除主键外的其他字段）映射到pojo类中的哪个属性。
当字段名与属性名一致时，或者，驼峰命名后字段名与属性名一致时，可以省略这个注解@TableField，即pojo中的属性上不写该注解。



# @GetMapping("/page/{moduleName}")

该注解用来处理查询请求，其他请求无效。
该注解相当于

``` less
@RequestMapping(value="/page/{moduleName}",method=RequestMethod.GET)
restful风格：可以根据请求的方式，来执行特定的业务功能。
TYPE=GET     处理查询业务
TYPE=POST    处理新增业务
TYPE=PUT     处理修改业务
TYPE=DELETE  处理删除业务
result风格总结：
1.可以获取url中的参数配合@PathVariable注解
2.根据请求方式，执行特定的业务功能
```



# @RestControllerAdvice

等同于 `@RestControllerAdvide=@ResponseBody+@ControllerAdvide`

说明：
该注解应用于类上，表示为通知方法。类中的方法的返回值都不是view，返回的是JSON格式的数据。
该注解描述的类为全局异常处理类，处理Controller层出现的异常。



# @Resource

@Resource的作用相当于@Autowired，

只不过@Autowired按byType自动注入，而@Resource默认按 byName自动注入罢了。

@Resource有两个属性是比较重要的，分是name和type，

+ Spring将@Resource注解的name属性解析为bean的名字
+ type属性则解析为bean的类型。

所以如果使用name属性，则使用byName的自动注入策略，

使用type属性时则使用byType自动注入策略。

如果既不指定name也不指定type属性，这时将通过反射机制使用byName自动注入策略。



# @RequestBody

1. @requestBody注解常用来处理content-type不是默认的application/x-www-form-urlcoded编码的内容，比如说：application/json或者是application/xml等。一般情况下来说常用其来处理application/json类型。
2. 通过@requestBody可以将请求体中的JSON字符串绑定到相应的bean上，当然，也可以将其分别绑定到对应的字符串上。


@RequestBody接收的参数是来自requestBody中，即请求体。一般用于处理非 Content-Type: application/x-www-form-urlencoded编码格式的数据，比如：application/json、application/xml等类型的数据。就application/json类型的数据而言，使用注解@RequestBody可以将body里面所有的json数据传到后端，后端再进行解析。

GET请求中，因为没有HttpEntity，所以@RequestBody并不适用。
POST请求中，通过HttpEntity传递的参数，必须要在请求头中声明数据的类型Content-Type，SpringMVC通过使用HandlerAdapter 配置的HttpMessageConverters来解析HttpEntity中的数据，然后绑定到相应的bean上。由于@RequestBody可用来处理 Content-Type 为 application/json 编码的内容，所以在postman中，选择body的类型为rowJSON(application/json)，这样在 Headers 中也会自动变为 Content-Type : application/json 编码格式。



# @RequestParam

@RequestParam接收的参数是来自HTTP请求体或请求url的QueryString中。

RequestParam可以接受简单类型的属性，也可以接受对象类型。

@RequestParam有三个配置参数：

+ required 表示是否必须，默认为 true，必须。
+ defaultValue 可设置请求参数的默认值。
+ value 为接收url的参数名（相当于key值）。

作用:

用来处理Content-Type为application/x-www-form-urlencoded编码的内容，Content-Type默认为该属性，也可以接收​​​​​​​application/json。@RequestParam也可用于其它类型的请求，例如：POST、DELETE等请求.所以在postman中，要选择body的类型为 x-www-form-urlencoded，这样在headers中就自动变为了 Content-Type : application/x-www-form-urlencoded 编码格式。



# @JsonFormat

在你需要查询出来的时间的数据库字段对应的实体类的属性上添加@JsonFormat

使用场景：后台到前台时间格式保持一致的问题(从数据库中取值传到前端)

`@JsonFormat(pattern="yyyy-MM-dd",timezone = "GMT+8")`

+ pattern:是你需要转换的时间日期的格式

+ timezone：是时间设置为东八区，避免时间在转换中有误差

提示：@JsonFormat注解可以在属性的上方，同样可以在属性对应的get方法上，两种方式没有区别



# @DateTimeFormat

在你需要查询出来的时间的数据库字段对应的实体类的属性上添加@DataTimeFormat
使用场景:前台到后台时间格式保持一致的问题(存入数据库的格式)

``` java
@DateTimeFormat(pattern ="yyyy-MM-dd")
@JsonFormat(pattern ="yyyy-MM-dd HH:mm:ss",timezone="GMT+8")
private Date systemTime;
```













