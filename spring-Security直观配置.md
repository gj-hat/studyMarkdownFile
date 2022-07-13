# SpringSecurity配置



# 导入依赖

``` xml
<dependency>
<groupId>org.springframework.boot</groupId>
<artifactId>spring-boot-starter-data-redis</artifactId>
</dependency>

<dependency>
<groupId>org.springframework.boot</groupId>
<artifactId>spring-boot-starter-security</artifactId>
</dependency>

<dependency>
<groupId>com.alibaba</groupId>
<artifactId>fastjson</artifactId>
<version>2.0.7</version>
</dependency>

<dependency>
<groupId>io.jsonwebtoken</groupId>
<artifactId>jjwt</artifactId>
<version>0.9.1</version>
</dependency>

<dependency>
<groupId>javax.xml.bind</groupId>
<artifactId>jaxb-api</artifactId>
<version>2.3.1</version>
</dependency>
```



# 一些配置



## Redis配置

当然你也可以放到数据库  下面的读写Redis改一下就行

**其实这里就配置两个地方 一个是连接 一个是JSON格式转化**

``` java
@Configuration
public class LettuceRedisConfig 
    @Bean
    public RedisTemplate<String, Serializable> redisTemplate(LettuceConnectionFactory connectionFactory) {
        RedisTemplate<String, Serializable> redisTemplate = new RedisTemplate<>();
        redisTemplate.setKeySerializer(new StringRedisSerializer());
        redisTemplate.setValueSerializer(new GenericJackson2JsonRedisSerializer());
        redisTemplate.setConnectionFactory(connectionFactory);
        return redisTemplate;
    }
}

////////////  yaml
spring:
  datasource:
    driver-class-name: com.mysql.cj.jdbc.Driver
    url: jdbc:mysql://127.0.0.1:3306/code_generate?useUnicode=true&characterEncoding=utf8&
    username: root
    password: 321321321

  redis:
    database: 0
    host: 127.0.0.1
    port: 6379
    timeout: 5000
    password:
    lettuce:
      pool:
        max-wait: 100
        max-active: 200   #连接池的最大连接数
        max-idle: 10  #最大空闲数
        min-idle: -1 #初始化连接数
```



## Secutity配置

这里有两个需要说明 

1. 一个是密码校验

   这里是因为security会读取PasswordEncoder类  里面写的东西 会加载到security的密码加密器中 后面也不用管了

2. 过滤器链 就是为了检测token  后面会写 不急

3. 其他没啥了 照抄就好

``` java
@Configuration
public class SecurityConfig extends WebSecurityConfigurerAdapter {
    @Autowired
    JwtAuthenticationTokenFilter jwtAuthenticationTokenFilter;
    @Bean
    public PasswordEncoder passwordEncoder(){
        return new BCryptPasswordEncoder();
    }
    @Override
    protected void configure(HttpSecurity http) throws Exception {
        http
                //关闭csrf
                .csrf().disable()
                //不通过Session获取SecurityContext
                .sessionManagement().sessionCreationPolicy(SessionCreationPolicy.STATELESS)
                .and()
                .authorizeRequests()
                // 对于登录接口 允许匿名访问
                .antMatchers("/admin/login").anonymous()
                // 除上面外的所有请求全部需要鉴权认证
                .anyRequest().authenticated();
        //把token校验过滤器添加到过滤器链中
        http.addFilterBefore(jwtAuthenticationTokenFilter, UsernamePasswordAuthenticationFilter.class);
        //允许跨域
        http.cors();
    }
    @Bean
    @Override
    public AuthenticationManager authenticationManagerBean() throws Exception {
        return super.authenticationManagerBean();
    }
}
```



# 过滤器链

这里就是定义一些规则  规则方面可以不用动

值得一提 这里定义了一个全局的RequestHolder封装了一些请求的东西 比如以后随时随地都可以拿到一些数据 比如id之类的 下面会写这个RequestHoler  (这个不是必须的 但是写了会很方便)

``` java
@Component
public class JwtAuthenticationTokenFilter extends OncePerRequestFilter {
    @Autowired
    private RedisCache redisCache;

    @Override
    protected void doFilterInternal(HttpServletRequest request, HttpServletResponse response, FilterChain filterChain) throws ServletException, IOException {
        //获取token
        String token = request.getHeader(Constants.REQUEST_HEAD);
        // 没有token
        if (!StringUtils.hasText(token) || token.equals("") || token.equals("undefined")) {
            //放行
            filterChain.doFilter(request, response);
            return;
        }
        //解析token
        int userid;
        try {
            Claims claims = JwtManager.parse(token);
            userid = (int) claims.get(Constants.JWT_KEY_USERID);
            RequestHolder.setAttribute(Constants.JWT_KEY_ACCOUNT, claims.get(Constants.JWT_KEY_ACCOUNT, String.class), RequestAttributes.SCOPE_REQUEST);
            RequestHolder.setAttribute(Constants.JWT_KEY_USERID, claims.get(Constants.JWT_KEY_USERID, Integer.class), RequestAttributes.SCOPE_REQUEST);
        } catch (Exception e) {
            e.printStackTrace();
            throw new RuntimeException("token非法");
        }
        //从redis中获取用户信息
        String redisKey = "login:" + userid;
        JwtUser loginUser = redisCache.getCacheObject(redisKey);
        if (Objects.isNull(loginUser)) {
            throw new RuntimeException("用户未登录");
        }
        //存入SecurityContextHolder
        UsernamePasswordAuthenticationToken authenticationToken =
                new UsernamePasswordAuthenticationToken(loginUser, null, null);
        SecurityContextHolder.getContext().setAuthentication(authenticationToken);
        //放行
        filterChain.doFilter(request, response);
    }
}
```



# RequestHolder

这个不是必须的 但是写了会很方便  这里封装的东西 自定义

``` java
public final class RequestHolder {
    private RequestHolder() {
    }
    private static ServletRequestAttributes getRequestContext() {
        return (ServletRequestAttributes) RequestContextHolder.getRequestAttributes();
    }
    /**
     * @return
     */
    public static HttpServletRequest getRequest() {
        return getRequestContext().getRequest();
    }
    /**
     * 获取 header 信息
     *
     * @param name
     * @return
     */
    public static String getHeader(String name) {
        return getRequest().getHeader(name);
    }
    /**
     *
     * @return
     */
    public static int getUserId() {
        final HttpServletRequest request = getRequest();
        return (int) request.getAttribute(Constants.JWT_KEY_USERID);
    }
    public static String getUseAccount() {
        final HttpServletRequest request = getRequest();
        return (String) request.getAttribute(Constants.JWT_KEY_ACCOUNT);
    }
    /**
     * @param key
     * @param value
     * @param scope
     */
    public static void setAttribute(String key, Object value, int scope) {
        RequestAttributes requestAttributes = getRequestContext();
        requestAttributes.setAttribute(key, value, scope);
    }
    public static String getAccountName() {
        final HttpServletRequest request = getRequest();
        return (String) request.getAttribute(Constants.JWT_KEY_ACCOUNT);
    }
}
```



# 一些工具类



## Jwt工具类

这里一些全局的东西可以抽取出来 自己写

``` java
public class JwtManager {


    /**
     * 生成jtw
     * @param claims token中要存放的数据（json格式）
     * @return
     */
    public static String builderToken(Map<String, Object> claims) {
        final Date now = new Date();
        return Jwts.builder()
                .setId(getUUID())
                .setIssuer("jwtProperties.getIssuer()")
                .setSubject("jwtProperties.getSubject()")
                .setAudience("jwtProperties.getAudience()")
                .setNotBefore(now)
                .setExpiration(expirdateDate())
                .setIssuedAt(now)
                .setClaims(claims)
                .signWith(SignatureAlgorithm.HS512, Constants.JWT_KEY)
                .compact();
    }
    public static Date expirdateDate() {
        return new Date(System.currentTimeMillis() + Constants.JWT_TTL * 1000);
    }
    /**
     * 解析 token 内容体
     *
     * @param token
     * @return
     */
    public static Claims parse(String token) {
        return Jwts.parser()
                .setSigningKey(Constants.JWT_KEY)
                .parseClaimsJws(token)
                .getBody();
    }
    /**
     * 生成claims
     * @param userDetails
     * @param type
     * @return
     */
    public static Map<String, Object> builderClaims(UserDetails userDetails, String type) {
        // 如果type为空，则默认为web端登录
        if (StringUtils.isBlank(type)) {
            type = "";
        }
        JwtUser user = (JwtUser) userDetails;
        Map<String, Object> claims = new HashMap<>();
        claims.put(Constants.JWT_KEY_ACCOUNT, userDetails.getUsername());
        claims.put(Constants.JWT_KEY_NICKNAME, user.getNickname());
        claims.put(Constants.JWT_KEY_USERID, user.getId());
        claims.put(Constants.JWT_KEY_TYPE, type);
        return claims;
    }
    public static String getUUID(){
        String token = UUID.randomUUID().toString().replaceAll("-", "");
        return token;
    }
}
```



## Redis工具类

``` java
@Component
public class RedisCache
{
    @Autowired
    public RedisTemplate redisTemplate;


    /**
     * 缓存基本的对象，Integer、String、实体类等
     *
     * @param key 缓存的键值
     * @param value 缓存的值
     */
    public <T> void setCacheObject(final String key, final T value)
    {
        redisTemplate.opsForValue().set(key, value);
    }

    /**
     * 缓存基本的对象，Integer、String、实体类等
     *
     * @param key 缓存的键值
     * @param value 缓存的值
     * @param timeout 时间
     * @param timeUnit 时间颗粒度
     */
    public <T> void setCacheObject(final String key, final T value, final Integer timeout, final TimeUnit timeUnit)
    {
        redisTemplate.opsForValue().set(key, value, timeout, timeUnit);
    }
    /**
     * 设置有效时间
     *
     * @param key Redis键
     * @param timeout 超时时间
     * @return true=设置成功；false=设置失败
     */
    public boolean expire(final String key, final long timeout)
    {
        return expire(key, timeout, TimeUnit.SECONDS);
    }
    /**
     * 设置有效时间
     *
     * @param key Redis键
     * @param timeout 超时时间
     * @param unit 时间单位
     * @return true=设置成功；false=设置失败
     */
    public boolean expire(final String key, final long timeout, final TimeUnit unit)
    {
        return redisTemplate.expire(key, timeout, unit);
    }
    /**
     * 获得缓存的基本对象。
     *
     * @param key 缓存键值
     * @return 缓存键值对应的数据
     */
    public <T> T getCacheObject(final String key)
    {
        ValueOperations<String, T> operation = redisTemplate.opsForValue();
        return operation.get(key);
    }
    /**
     * 删除单个对象
     *
     * @param key
     */
    public boolean deleteObject(final String key)
    {
        return redisTemplate.delete(key);
    }
    /**
     * 删除集合对象
     *
     * @param collection 多个对象
     * @return
     */
    public long deleteObject(final Collection collection)
    {
        return redisTemplate.delete(collection);
    }
    /**
     * 缓存List数据
     *
     * @param key 缓存的键值
     * @param dataList 待缓存的List数据
     * @return 缓存的对象
     */
    public <T> long setCacheList(final String key, final List<T> dataList)
    {
        Long count = redisTemplate.opsForList().rightPushAll(key, dataList);
        return count == null ? 0 : count;
    }
    /**
     * 获得缓存的list对象
     *
     * @param key 缓存的键值
     * @return 缓存键值对应的数据
     */
    public <T> List<T> getCacheList(final String key)
    {
        return redisTemplate.opsForList().range(key, 0, -1);
    }
    /**
     * 缓存Set
     *
     * @param key 缓存键值
     * @param dataSet 缓存的数据
     * @return 缓存数据的对象
     */
    public <T> BoundSetOperations<String, T> setCacheSet(final String key, final Set<T> dataSet)
    {
        BoundSetOperations<String, T> setOperation = redisTemplate.boundSetOps(key);
        Iterator<T> it = dataSet.iterator();
        while (it.hasNext())
        {
            setOperation.add(it.next());
        }
        return setOperation;
    }
    /**
     * 获得缓存的set
     *
     * @param key
     * @return
     */
    public <T> Set<T> getCacheSet(final String key)
    {
        return redisTemplate.opsForSet().members(key);
    }
    /**
     * 缓存Map
     *
     * @param key
     * @param dataMap
     */
    public <T> void setCacheMap(final String key, final Map<String, T> dataMap)
    {
        if (dataMap != null) {
            redisTemplate.opsForHash().putAll(key, dataMap);
        }
    }
    /**
     * 获得缓存的Map
     *
     * @param key
     * @return
     */
    public <T> Map<String, T> getCacheMap(final String key)
    {
        return redisTemplate.opsForHash().entries(key);
    }
    /**
     * 往Hash中存入数据
     *
     * @param key Redis键
     * @param hKey Hash键
     * @param value 值
     */
    public <T> void setCacheMapValue(final String key, final String hKey, final T value)
    {
        redisTemplate.opsForHash().put(key, hKey, value);
    }
    /**
     * 获取Hash中的数据
     *
     * @param key Redis键
     * @param hKey Hash键
     * @return Hash中的对象
     */
    public <T> T getCacheMapValue(final String key, final String hKey)
    {
        HashOperations<String, String, T> opsForHash = redisTemplate.opsForHash();
        return opsForHash.get(key, hKey);
    }
    /**
     * 删除Hash中的数据
     *
     * @param key
     * @param hkey
     */
    public void delCacheMapValue(final String key, final String hkey)
    {
        HashOperations hashOperations = redisTemplate.opsForHash();
        hashOperations.delete(key, hkey);
    }
    /**
     * 获取多个Hash中的数据
     *
     * @param key Redis键
     * @param hKeys Hash键集合
     * @return Hash对象集合
     */
    public <T> List<T> getMultiCacheMapValue(final String key, final Collection<Object> hKeys)
    {
        return redisTemplate.opsForHash().multiGet(key, hKeys);
    }
    /**
     * 获得缓存的基本对象列表
     *
     * @param pattern 字符串前缀
     * @return 对象列表
     */
    public Collection<String> keys(final String pattern)
    {
        return redisTemplate.keys(pattern);
    }
}
```



# 实体类

这里有两个实体类  一个是数据库真实的实体类 另一个是继承了UserDetails的实体类(这个会封装到JWT中的 这里的字段就可以写到RequestHolder)

## SysUser

``` java
@Data
@TableName("sys_user")
public class SysUser extends Model<SysUser> {

    @TableField("id")
    private Integer id;
    //用户名
    @TableField("username")
    private String username;
    //密码
    @TableField("password")
    private String password;
    //昵称
    @TableField("nickname")
    private String nickName;
    //创建时间
    @TableField("create_time")
    private Date createTime;
    //创建人
    @TableField("create_by")
    private String createBy;
    //更新时间
    @TableField("update_time")
    private Date updateTime;
    //更新人
    @TableField("update_by")
    private String updateBy;

    @TableField("del_flag")
    private Boolean delFlag;
}
```



## JwtUser

这里有一些需要说的:

1. 字段是回头new的时候添加进去
2. 下面的一些方法是控制这个用户权限的
3. @Json….这个注解不能省略 这个是Redis的一个bug  redis要求读取的时候必须有set和get方法

``` java
@Data
@NoArgsConstructor
@JsonIgnoreProperties({"enabled","accountNonExpired", "accountNonLocked", "credentialsNonExpired", "authorities"})
public class JwtUser implements UserDetails {
    private int id;
    private String username;
    private String nickname;
    private String password;
    public JwtUser(int id, String username, String nickname, String password) {
        this.id = id;
        this.username = username;
        this.nickname = nickname;
        this.password = password;
    }
    @Override
    public Collection<? extends GrantedAuthority> getAuthorities() {
        return null;
    }
    @Override
    public String getPassword() {
        return password;
    }
    @Override
    public String getUsername() {
        return username;
    }
    @Override
    public boolean isAccountNonExpired() {
        return true;
    }
    @Override
    public boolean isAccountNonLocked() {
        return true;
    }
    @Override
    public boolean isCredentialsNonExpired() {
        return true;
    }
    @Override
    public boolean isEnabled() {
        return true;
    }
}
```



# UserDetailsServiceImpl

这个方法写了几乎就不需要动了  上面的`JwtUser`就是继承了`UserDetails`的方法会用这个Service去查询用户 当然这里还可以 查一些权限在返回值的地方写进去

``` java
@Service
public class UserDetailsServiceImpl implements UserDetailsService {
    private static final Logger log = LoggerFactory.getLogger(SysUser.class);
    @Autowired
    private SysUserMapper sysUserMapper;
    @Override
    public UserDetails loadUserByUsername(String username) throws UsernameNotFoundException {
        //根据用户名查询用户信息
        LambdaQueryWrapper<SysUser> wrapper = new LambdaQueryWrapper<>();
        wrapper.eq(SysUser::getUsername,username);
        SysUser user = sysUserMapper.selectOne(wrapper);
        //如果查询不到数据就通过抛出异常来给出提示
        if(Objects.isNull(user)){
            log.error("用户名或密码错误");
            throw new RuntimeException("用户名或密码错误");
        }
        //TODO 根据用户查询权限信息 添加到LoginUser中
        
        //封装成UserDetails对象返回 
        return new JwtUser(user.getId(),user.getUsername(),user.getNickName(),user.getPassword());
    }
}
```



# 接下来就是通用方法了

## SysUserController

这个就是数据表具体表的  service mapper就不写了



## LoginController

security有默认的退出登录方法   所以咱们换一个路径

``` java
@RestController
@RequestMapping("/admin")
public class LoginController {
    @Autowired
    private LoginServiceImpl loginService;
    @RequestMapping(value = "/logout", method = {RequestMethod.POST})
    public Result logout() {
        return loginService.logout();
    }
    @RequestMapping(value = "/login", method = {RequestMethod.POST})
    public Result login(@RequestBody SysUser user) {
        return loginService.login(user);
    }
}
```



## LoginServiceImpl

接口自己写 这里就写实现类

实际上这里AuthenticationManager会调用刚刚的UserDetailsServiceImpl的方法 进行验证密码是否正确(这里区别一个概念  LoginServiceImpl是验证密码是不是正确  过滤器只是验证token字符串和数据库的是否匹配)

``` java
@Service
public class LoginServiceImpl implements LoginService {
    @Autowired
    private AuthenticationManager authenticationManager;
    @Autowired
    private RedisCache redisCache;
    @Override
    public Result login(SysUser user) {
        // AuthenticationManager进行认证   查询数据库找到用户名和密码
        UsernamePasswordAuthenticationToken usernamePasswordAuthenticationToken = new UsernamePasswordAuthenticationToken(user.getUsername(), user.getPassword());
        Authentication authenticate = authenticationManager.authenticate(usernamePasswordAuthenticationToken);
        // authenticate不通给出对应的提示 抛出异常登录失败
        if (authenticate == null) {
            throw new RuntimeException("登录失败");
        }
        // 认证成功，使用userid生成一个jwt
        JwtUser loginUser = (JwtUser) authenticate.getPrincipal();
        Map<String, Object> stringObjectMap = JwtManager.builderClaims(loginUser, "");
        String jwt = JwtManager.builderToken(stringObjectMap);
        Map<String, String> map = new HashMap<>();
        map.put("token", jwt);
        // 把完整的用户信息存入redis中 userid作为key
        redisCache.setCacheObject("login:" + loginUser.getId(), loginUser);
        return Result.success("登录成功", map);
    }
    @Override
    public Result logout() {
        // 删除redis   看吧 这里的RequestHolder好用吧
        redisCache.deleteObject("login:" + RequestHolder.getUserId());

        return Result.success("退出成功");
    }
}
```



# 没了

如果还要写 就是要写一些具体的权限校验  异常处理

