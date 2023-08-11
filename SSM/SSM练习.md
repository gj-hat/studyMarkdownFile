# Spring练习





## 环境搭建

1. 创建工程
2. 新建静态页面(已经有也可以重写)
3. 导入坐标
4. 创建包结构
5. 创建数据库(已经有也可以自己重写)
6. 创建POJO类(已经有也可以自己重写)
7. 创建配置文件
[TOC]


### 创建工程

1. new Project
2. Maven
3. webapp
4. com.SpringPractice

### 新建静态页面

太麻烦了 代码太多 到时候打包直接用就好了

### 导入坐标

```xml
<dependencies>
    <dependency>
      <groupId>mysql</groupId>
      <artifactId>mysql-connector-java</artifactId>
      <version>8.0.18</version>
    </dependency>
    <dependency>
      <groupId>com.mchange</groupId>
      <artifactId>c3p0</artifactId>
      <version>0.9.5.5</version>
    </dependency>
    <dependency>
      <groupId>com.mchange</groupId>
      <artifactId>mchange-commons-java</artifactId>
      <version>0.2.19</version>
    </dependency>
    <dependency>
      <groupId>junit</groupId>
      <artifactId>junit</artifactId>
      <version>4.12</version>
      <scope>test</scope>
    </dependency>
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-context</artifactId>
      <version>5.0.5.RELEASE</version>
    </dependency>
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-test</artifactId>
      <version>5.0.5.RELEASE</version>
    </dependency>
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-web</artifactId>
      <version>5.0.5.RELEASE</version>
    </dependency>
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-webmvc</artifactId>
      <version>5.0.5.RELEASE</version>
    </dependency>
    <dependency>
      <groupId>javax.annotation</groupId>
      <artifactId>javax.annotation-api</artifactId>
      <version>1.2</version>
    </dependency>
    <dependency>
      <groupId>javax.servlet</groupId>
      <artifactId>javax.servlet-api</artifactId>
      <version>3.0.1</version>
      <scope>provided</scope>
    </dependency>
    <dependency>
      <groupId>javax.servlet.jsp</groupId>
      <artifactId>javax.servlet.jsp-api</artifactId>
      <version>2.2.1</version>
      <scope>provided</scope>
    </dependency>
    <dependency>
      <groupId>com.fasterxml.jackson.core</groupId>
      <artifactId>jackson-core</artifactId>
      <version>2.9.0</version>
    </dependency>
    <dependency>
      <groupId>com.fasterxml.jackson.core</groupId>
      <artifactId>jackson-databind</artifactId>
      <version>2.9.0</version>
    </dependency>
    <dependency>
      <groupId>com.fasterxml.jackson.core</groupId>
      <artifactId>jackson-annotations</artifactId>
      <version>2.9.0</version>
    </dependency>
    <dependency>
      <groupId>commons-fileupload</groupId>
      <artifactId>commons-fileupload</artifactId>
      <version>1.3.1</version>
    </dependency>
    <dependency>
      <groupId>commons-io</groupId>
      <artifactId>commons-io</artifactId>
      <version>2.3</version>
    </dependency>
    <dependency>
      <groupId>commons-logging</groupId>
      <artifactId>commons-logging</artifactId>
      <version>1.2</version>
    </dependency>
    <dependency>
      <groupId>org.slf4j</groupId>
      <artifactId>slf4j-log4j12</artifactId>
      <version>1.7.7</version>
    </dependency>
    <dependency>
      <groupId>log4j</groupId>
      <artifactId>log4j</artifactId>
      <version>1.2.17</version>
    </dependency>
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-jdbc</artifactId>
      <version>5.0.5.RELEASE</version>
    </dependency>
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-tx</artifactId>
      <version>5.0.5.RELEASE</version>
    </dependency>
    <dependency>
      <groupId>jstl</groupId>
      <artifactId>jstl</artifactId>
      <version>1.2</version>
    </dependency>
  </dependencies>
```

### 创建包结构

> controller , service , dao , domain , utils

1. 创建包结构

   ├─Controller
   ├─dao
   ├─domain
   ├─service
   └─utils

2. 总的包结构(全部写完时候的)

   java/com/
   ├── controller
   │   ├── RoleController.java
   │   └── UserController.java
   ├── dao
   │   ├── RoleDao.java
   │   ├── UserDao.java
   │   └── impl
   │       ├── RoleDaoImpl.java
   │       └── UserDaoImpl.java
   ├── domain
   │   ├── Role.java
   │   └── User.java
   ├── service
   │   ├── RoleService.java
   │   ├── UserService.java
   │   └── impl
   │       ├── RoleServiceImpl.java
   │       └── UserServiceImpl.java
   └── utils

### 创建数据库

``` sql
CREATE DATABASE /*!32312 IF NOT EXISTS*/`test` /*!40100 DEFAULT CHARACTER SET utf8 */;
USE `test`;
/*Table structure for table `sys_role` */
DROP TABLE IF EXISTS `sys_role`;
CREATE TABLE `sys_role` (
  `id` bigint(20) NOT NULL AUTO_INCREMENT,
  `roleName` varchar(50) DEFAULT NULL,
  `roleDesc` varchar(50) DEFAULT NULL,
  PRIMARY KEY (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=5 DEFAULT CHARSET=utf8;
/*Data for the table `sys_role` */
insert  into `sys_role`(`id`,`roleName`,`roleDesc`) values (1,'院长','负责全面工作'),(2,'研究员','课程研发工作'),(3,'讲师','授课工作'),(4,'助教','协助解决学生的问题');
/*Table structure for table `sys_user` */
DROP TABLE IF EXISTS `sys_user`;
CREATE TABLE `sys_user` (
  `id` bigint(20) NOT NULL AUTO_INCREMENT,
  `username` varchar(50) DEFAULT NULL,
  `email` varchar(50) DEFAULT NULL,
  `password` varchar(80) DEFAULT NULL,
  `phoneNum` varchar(20) DEFAULT NULL,
  PRIMARY KEY (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=4 DEFAULT CHARSET=utf8;
/*Data for the table `sys_user` */
insert  into `sys_user`(`id`,`username`,`email`,`password`,`phoneNum`) values (1,'zhangsan','zhangsan@itcast.cn','123','13888888888'),(2,'lisi','lisi@itcast.cn','123','13999999999'),(3,'wangwu','wangwu@itcast.cn','123','18599999999');
/*Table structure for table `sys_user_role` */
DROP TABLE IF EXISTS `sys_user_role`;
CREATE TABLE `sys_user_role` (
  `userId` bigint(20) NOT NULL,
  `roleId` bigint(20) NOT NULL,
  PRIMARY KEY (`userId`,`roleId`),
  KEY `roleId` (`roleId`),
  CONSTRAINT `sys_user_role_ibfk_1` FOREIGN KEY (`userId`) REFERENCES `sys_user` (`id`),
  CONSTRAINT `sys_user_role_ibfk_2` FOREIGN KEY (`roleId`) REFERENCES `sys_role` (`id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8;
/*Data for the table `sys_user_role` */
insert  into `sys_user_role`(`userId`,`roleId`) values (1,1),(1,2),(2,2),(2,3);
```

### 创建POJO类

> User.java , Role.java

```java
package com.domain;
public class Role {
    private Long id;
    private String roleName;
    private String roleDesc;
// set get toString
}

package com.domain;
public class User {
    private Long id;
    private String username;
    private String email;
    private String password;
    private String phoneNum;
	// set get toString
}
```



### 创建配置文件

> applicationContext.xml , spring-mvc.xml , jdbc.properties , log4j.properties

1. applicationContext.xml    初步基础款配置

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns="http://www.springframework.org/schema/beans"
          xmlns:context="http://www.springframework.org/schema/context"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xsi:schemaLocation="
          http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
          http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd">
   <!-- 1. 加载jdbc.properties   -->
   <context:property-placeholder location="classpath:jdbc.properties" />
   <!-- 2. 配置数据源对象   -->
       <bean id="dataSource" class="com.mchange.v2.c3p0.ComboPooledDataSource" >
           <property name="driverClass" value="${jdbc.Driver}" />
           <property name="jdbcUrl" value="${jdbc.Url}" />
           <property name="user" value="${jdbc.Username}" />
           <property name="password" value="${jdbc.Password}" />
       </bean>
   <!-- 3. 配置模板JdbcTemplate对象   -->
       <bean id="jdbcTemplate" class="org.springframework.jdbc.core.JdbcTemplate">
           <property name="dataSource" ref="dataSource" />
       </bean>
   </beans>
   ```

2. spring-mvc.xm   初步基础款配置

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns="http://www.springframework.org/schema/beans"
          xmlns:mvc="http://www.springframework.org/schema/mvc"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xsi:schemaLocation="
   http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/mvc http://www.springframework.org/schema/mvc/spring-mvc.xsd">
   <!-- 1. mvc注解驱动   -->
       <mvc:annotation-driven />
   <!-- 2. 配置视图解析器   -->
       <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver" >
           <!--   前缀     -->
           <property name="prefix" value="/pages" />
           <!--   后缀     -->
           <property name="suffix" value=".jsp" />
       </bean>
   <!-- 3. 静态资源权限开启   -->
       <mvc:default-servlet-handler />
   </beans>
   ```

3. jdbc.properties

   ```properties
   jdbc.Driver = com.mysql.cj.jdbc.Driver
   jdbc.Url = jdbc:mysql://127.0.0.1:3306/test
   jdbc.Username = root
   jdbc.Password = 321321
   ```

4. log4j.properties

   ```properties
   ### direct log messages to stdout ###
   log4j.appender.stdout=org.apache.log4j.ConsoleAppender
   log4j.appender.stdout.Target=System.out
   log4j.appender.stdout.layout=org.apache.log4j.PatternLayout
   log4j.appender.stdout.layout.ConversionPattern=%d{ABSOLUTE} %5p %c{1}:%L - %m%n
   ### direct messages to file mylog.log ###
   log4j.appender.file=org.apache.log4j.FileAppender
   log4j.appender.file.File=E:/temporary/mylog.log
   log4j.appender.file.layout=org.apache.log4j.PatternLayout
   log4j.appender.file.layout.ConversionPattern=%d{ABSOLUTE} %5p %c{1}:%L - %m%n
   ### set log levels - for more verbose logging change 'info' to 'debug' ###
   log4j.rootLogger=info, stdout
   ```

5. web.xml   初步基础款配置

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <web-app version="2.4"
            xmlns="http://java.sun.com/xml/ns/j2ee"
            xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
            xsi:schemaLocation="http://java.sun.com/xml/ns/j2ee http://java.sun.com/xml/ns/j2ee/web-app_2_4.xsd">
   <!-- 全局初始化参数   -->
       <context-param>
           <param-name>contextConfigLocation</param-name>
           <param-value>classpath:applicationContext.xml</param-value>
       </context-param>
   <!-- Spring监听器   -->
   <listener>
       <listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
   </listener>
   <!-- SpringMVC前端控制器  -->
       <servlet>
           <servlet-name>DispatcherServlet</servlet-name>
           <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
           <init-param>
               <param-name>contextConfigLocation</param-name>
               <param-value>classpath:spring-mvc.xml</param-value>
           </init-param>
           <load-on-startup>2</load-on-startup>
       </servlet>
       <servlet-mapping>
           <servlet-name>DispatcherServlet</servlet-name>
           <url-pattern>/</url-pattern>
       </servlet-mapping>
   </web-app>
   ```





## 角色列表展示和添加操作



### 角色列表展示

#### 步骤分析

1. 点击角色管理菜单发送请求到服务器端对应的controller  (修改角色管理菜单的url)
2. 创建RoleController和RoleList()方法
3. 创建RoleService和showList()方法
4. 创建RoleDao和findAll()方法
5. 使用JdbcTemplate完成查询操作
6. 将查询数据存储到Model中
7. 转发到rele-list.jsp页面进行展示

#### 实现步骤

> Web层注解,Service,Dao层配置文件

1. 前端看到,

   ``` jsp
   <a
      href="${pageContext.request.contextPath}/pages/role-list.jsp"> <i class="fa fa-circle-o"></i> 角色管理
   </a>
   ```

2. 新建ReloController,并将类标记为`@RequestMapping("/role")`,类下的方法标记为`@RequestMapping("/list")`

   ``` java
   @RequestMapping("/role")
   public class RoleController {
       @RequestMapping("/list")
       public ModelAndView list(){
           ModelAndView modelAndView = new ModelAndView();
           return modelAndView;
       }
   }
   ```

3. 编写list方法(Web层)

   ```java
   @RequestMapping("/role")
   public class RoleController {
       // 1. 配置文件方法
       private RoleService roleService;
       public void setRoleService(RoleService roleService) {this.roleService = roleService;}
       @RequestMapping("/list")
       public ModelAndView list() {
           ModelAndView modelAndView = new ModelAndView();
           List<Role> roleList = roleService.list();
           // 指定模型
           modelAndView.addObject("roleList", roleList);
           // 指定视图  前后缀已经在xml中写了
           modelAndView.setViewName("role-list");
           return modelAndView;
       }
   }
   ```

4. 编写Service层

   + 编写接口

     ``` java
     public interface RoleService {public List<Role> list();}
     ```

   + 实现接口

     ```java
     public class RoleServiceImpl implements RoleService {
         // 注解方式(可以省略set方法)
         @Autowired
         private RoleDao roleDao;
         public List<Role> list() {
             List<Role> roleList = roleDao.list();
             return roleList;
         }
     }
     ```

5. 编写Dao

   + 编写接口

     ```java
     public interface RoleDao {List<Role> list();}
     ```

   + 实现接口

     ```java
     public class RoleDaoImpl implements RoleDao {
         // 注入模板对象
         private JdbcTemplate jdbcTemplate;
         public void setJdbcTemplate(JdbcTemplate jdbcTemplate) {
             this.jdbcTemplate = jdbcTemplate;
         }
         @Override
         public List<Role> list() {
             List<Role> roleList = jdbcTemplate.query("select * from sys_role", new BeanPropertyRowMapper<Role>(Role.class));
             return roleList;
         }
     }
     ```

6. 编写Spring-mvc.xml配置文件,配置组件扫描

   + 在ReloController类前注解@Controller

   + 编写Spring-mvc.xml文件,配置组件扫描(因为Web层没有使用Autowired)

   + ```xml
     <!-- 4. 组件扫描 扫描Controller   -->
     <context:component-scan base-package="com.controller"/>
     ```

7. Service,Dao层使用注入方法需要编写applicationContext.xml文件,注入

   ```java
   <!--  4. 配置ReloService   -->
   <bean id="roleService" class="com.service.impl.RoleServiceImpl">
       <property name="roleDao" ref="roleDao"/>
   </bean>
   <!--  5. 配置ReloDao   -->
   <bean id="roleDao" class="com.dao.impl.RoleDaoImpl">
       <property name="jdbcTemplate" ref="jdbcTemplate"/>
   </bean>
   ```

8. 编写前端展示数据

   ``` jsp
   <!-- 添加jstl头  -->
   <%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
   
   <!-- 在table的tr行中使用forEach  -->
   <c:forEach items="${roleList}" var="role">
       <tr>
           <td><input name="ids" type="checkbox"></td>
           <td>${role.id}</td>
           <td>${role.roleName}</td>
           <td>${role.roleDesc}</td>
           <td class="text-center">
               <a href="#" class="btn bg-olive btn-xs">删除</a>
           </td>
       </tr>
   </c:forEach>
   ```



### 角色列表添加

#### 步骤分析

1. 点击角色管理添加按钮发送请求到服务器端对应的controller  (修改角色添加按钮的url)
2. 创建RoleController和save()方法
3. 创建RoleService和save()方法
4. 创建RoleDao和save()方法
5. 使用JdbcTemplate保存到sys_role中
6. 跳转回角色列表页面

#### 实现步骤

``` java
// 1. Web层
@RequestMapping("/save")
public String save(Role role) {
    roleService.save(role);
    return "redirect:/role/list";
}
// 2. Service层
public void save(Role role) {
    roleDao.save(role);
}
// 3. dao层
public void save(Role role) {
    jdbcTemplate.update("insert into sys_role value(?,?,?)", null, role.getRoleName(), role.getRoleDesc());
}

// 4. UTF-8解决乱码过滤器 在xml中配置filter
<filter>
    <filter-name>CharacterEncodingFilter</filter-name>
    <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
        <init-param>
        <param-name>encoding</param-name>
        <param-value>UTF-8</param-value>
        </init-param>
        </filter>
        <filter-mapping>
        <filter-name>CharacterEncodingFilter</filter-name>
        <url-pattern>/*</url-pattern>
    </filter-mapping>
```



## 用户列表展示和添加操作



### 用户列表展示

#### 步骤分析

<img src="https://gitee.com/thisisafigurebed/figure-bed/raw/master/servletNode/practice1.jpg" >

1. 用户domain类需要改动
2. 查询语句需要多表查询
3. 其他同上



#### 实现步骤

1. 修改User.java

   ``` java
   public class User {
       private Long id;
       private String username;
       private String email;
       private String password;
       private String phoneNum;
   
       // 当前用户属于什么角色
       private List<Role> roles;
   
   	// set get toString
   }
   ```

2. 编写Controller

    ```java
    @Controller
    @RequestMapping("/user")
    public class UserController {
        @Resource(name = "userService")
        private UserService userService;
        @RequestMapping("/list")
        public ModelAndView list() {
            ModelAndView modelAndView = new ModelAndView();
            List<User> userList = userService.list();
            modelAndView.addObject("userList", userList);
            modelAndView.setViewName("user-list");
            return modelAndView;
        }
    }
    ```

3. 编写Service

    ```java
    @Service("userService")
    public class UserServiceImpl implements UserService {
        @Resource(name = "userDao")
        private UserDao userDao;
        @Resource(name = "roleDao")
        private RoleDao roleDao;
        public List<User> list() {
            List<User> userList = userDao.findAll();
            // 封装每一个user的role
            for (User user : userList) {
                // 获得主键id
                Long id = user.getId();
                // 将id作为参数继续查询
                List<Role> roles = roleDao.findRoleByUserId(id);
                user.setRoles(roles);
            }
            return userList;
        }
    }
    ```

4. 编写Dao

    ```java
    //UserDao
    @Repository("userDao")
    public class UserDaoImpl implements UserDao {
        @Autowired
        private JdbcTemplate jdbcTemplate;
        public List<User> findAll() {
            List<User> userList = jdbcTemplate.query("select * from sys_user", new BeanPropertyRowMapper<User>(User.class));
            return userList;
        }
    }
    
    
    //RoleDao
    @Override
    public List<Role> findRoleByUserId(Long id) {
        List<Role> roles = jdbcTemplate.query("select * from sys_user_role ur,sys_role r where ur.roleId = r.id and ur.userId = ?", new BeanPropertyRowMapper<Role>(Role.class), id);
        System.out.println(roles);
        return roles;
    }
    ```

5. 编写前端jsp

   ``` jsp
<%-- 1. jstl的c标签  --%>   
   <%-- 2. 表格的行 --%>
<c:forEach items="${userList}" var="user">
       <tr>
           <td><input name="ids" type="checkbox"></td>
           <td>${user.id}</td>
           <td>${user.username}</td>
           <td>${user.email}</td>
           <td>${user.phoneNum}</td>
           <td class="text-center">
               <c:forEach items="${user.roles}" var="role">
                   ${role.roleName}&nbsp;
               </c:forEach>
           </td>
           <td class="text-center">
               <a href="javascript:void(0);" class="btn bg-olive btn-xs">删除</a>
           </td>
       </tr>
   </c:forEach>
   ```
   



### 用户添加

#### 步骤分析

<img src="https://gitee.com/thisisafigurebed/figure-bed/raw/master/servletNode/practice2.jpg">

1. 因为有"用户角色"栏目需要查询数据库,所以得先查Role表获得所有的角色
2. 调用之前的roleService.list()方法
3. 前端展示
4. 编写保存方法的Web层,Servicve,Dao层
5. 值得一提,用户名密码邮箱联系电话是一张表角色是另一张表,所以需要往数据库中写两次



#### 实现步骤

 ``` java
//1. web层
@RequestMapping("/saveUI")
public ModelAndView saveUI() {
    ModelAndView modelAndView = new ModelAndView();
    List<Role> roleList = roleService.list();
    modelAndView.addObject("roleList", roleList);
    modelAndView.setViewName("user-add");
    return modelAndView;
}

@RequestMapping("/save")
public String save(User user,Long[] roleIds) {
    userService.save(user, roleIds);
    return "redirect:/user/list";
}

//2.前端  记得还有两个url没有标出
//<div class="col-md-10 data">
//    <c:forEach items="${roleList}" var="role">
//        <input class="" type="checkbox" name="roleIds" value="${role.id}">${role.roleName}
//</c:forEach>
//</div>

    
//3. service层
@Override
public void save(User user, Long[] roleIds) {
    //1. 向sys_user表中存数据
    Long userId = userDao.save(user);
    //2. 向sys_user_role关系表中存数据
    userDao.saveUserRoleRel(userId, roleIds);
}

//4. Dao层
@Override
public Long save(User user) {
    jdbcTemplate.update("insert into sys_user value(?,?,?,?,?)", null, user.getUsername(), user.getEmail(), user.getPassword(), user.getPhoneNum());
    List<User> userList = jdbcTemplate.query("select id from sys_user where username = ?", new BeanPropertyRowMapper<User>(User.class), user.getUsername());
    return userList.get(0).getId();  //返回当前保存用户的ID
}

@Override
public void saveUserRoleRel(Long userId, Long[] roleIds) {
    for (Long roleId : roleIds) {
        jdbcTemplate.update("insert into sys_user_role value(?,?)", userId, roleId);
    }
}


 ```



## 用户删除



### 步骤分析

1. 前端点击删除按钮提示是否删除
2. 是->则跳转对应controller
3. service,dao编写

#### 实现步骤

``` jsp
<a href="javascript:void(0);" onclick="delUser('${user.id}')" class="btn bg-olive btn-xs">删除</a>
<script >
    function delUser(userId) {
        if (confirm("您确认要删除吗?")) {
            location.href = "${pageContext.request.contextPath}/user/del/" + userId;
        }
    }
</script>
```

``` java
@RequestMapping("/del/{userId}")
public String del(@PathVariable("userId") Long userId) {
    userService.del(userId);
    return "redirect:/user/list";
}


@Override
public void del(Long userId) {
    // 1. 删除sys_user-role关系表
    userDao.delUserRoleRel(userId);
    // 2. 删除sys_user表
    userDao.del(userId);
}


@Override
public void delUserRoleRel(Long userId) {
    jdbcTemplate.update("delete from sys_user_role where userId = ?", userId);
}

@Override
public void del(Long userId) {
    jdbcTemplate.update("delete from sys_user where id = ?", userId);
}
```



# Springmvc拦截器练习

案例: 用户登录权限控制

需求: 用户没有登陆的时候,不能对后台进行操作->点击任何后跳转到登陆页面。只有用户登陆时才可以对后台进行操作



## 判断是否登陆

1. 编写intercptor.java

   ``` java
   public class PrivilegeInterceptor implements HandlerInterceptor {
       @Override
       public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
           // 逻辑: 判断用户是否登陆   本质: 判断session中有没有user
           HttpSession session = request.getSession();
           User user = (User)session.getAttribute("user");
           if (user == null) {
               response.sendRedirect(request.getContextPath() + "login/jsp");
               return false;
           }
           // 放行 访问目标资源
           return true;
       }
   }
   ```

2. 配置对所有资源进行拦截判断

   ``` xml
   <!-- spring-mvc   -->
   ......
   <!-- 配置权限连接器(拦截器)   -->
   <mvc:interceptors>
       <mvc:interceptor>
           <mvc:mapping path="/**"/>
           <bean class="com.interceptor.PrivilegeInterceptor" />
       </mvc:interceptor>
   </mvc:interceptors>
   ......
   ```



## 判断用户名密码

1. web层

   ```java
   @RequestMapping("/login")
   public String login(String username, String password, HttpSession session) {
       User user = userService.login(username, password);
       if (user != null) {
           session.setAttribute("user", user);
           return "redirect:/index.jsp";
       } else {
           session.setAttribute("msg", "用户名密码错误");
           return "redirect:/login.jsp";
       }
   }
   ```

2. service层

   ``` java
       @Override
       public User login(String username, String password) {
           User user =  userDao.findUsernameAndPassword(username,password);
           return user;
       }
   ```

3. dao层

   ``` java
       @Override
       public User findUsernameAndPassword(String username, String password) {
           try {
               User user = jdbcTemplate.queryForObject("select username, password from sys_user where username = ? and password = ?", new BeanPropertyRowMapper<User>(User.class), username, password);
               return user;
           } catch (Exception e) {
               e.printStackTrace();
               return null;
           }
       }
   ```

4. interceptor.java

   ```java
   public class PrivilegeInterceptor implements HandlerInterceptor {
       @Override
       public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
           // 逻辑: 判断用户是否登陆   本质: 判断session中有没有user
           HttpSession session = request.getSession();
           User user = (User) session.getAttribute("user");
           if (user == null) {
               response.sendRedirect(request.getContextPath() + "/login.jsp");
               return false;
           }
           // 放行 访问目标资源
           return true;
       }
   }
   ```

5. spring-mvc.xml

   ```xml
   <!-- 5. 配置权限连接器(拦截器)   -->
   <mvc:interceptors>
       <mvc:interceptor>
           <!--   对哪些资源拦截     -->
           <mvc:mapping path="/**"/>
           <!--  对哪些资源放行      -->
           <mvc:exclude-mapping path="/user/login"/>
           <mvc:exclude-mapping path="/login.jsp"/>
           <bean class="com.interceptor.PrivilegeInterceptor" />
       </mvc:interceptor>
   </mvc:interceptors>
   ```

   

