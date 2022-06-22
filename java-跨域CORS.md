对于 CORS的跨域请求，主要有以下几种方式可供选择：

1. 返回新的CorsFilter
2. 重写 WebMvcConfigurer
3. 使用注解 @CrossOrigin
4. 手动设置响应头 (HttpServletResponse)
5. 自定web filter 实现跨域



## 返回新的CorsFilter

``` java
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.web.cors.CorsConfiguration;
import org.springframework.web.cors.UrlBasedCorsConfigurationSource;
import org.springframework.web.filter.CorsFilter;

@Configuration
public class GlobalCorsConfig {
    @Bean
    public CorsFilter corsFilter() {
        //1. 添加 CORS配置信息
        CorsConfiguration config = new CorsConfiguration();
        //放行哪些原始域
//        config.addAllowedOrigin("*");
        config.addAllowedOriginPattern("*");
        //是否发送 Cookie
        config.setAllowCredentials(true);
        //放行哪些请求方式
        config.addAllowedMethod("*");
        //放行哪些原始请求头部信息
        config.addAllowedHeader("*");
        //暴露哪些头部信息
        config.addExposedHeader("*");
        //2. 添加映射路径
        UrlBasedCorsConfigurationSource corsConfigurationSource = new UrlBasedCorsConfigurationSource();
        corsConfigurationSource.registerCorsConfiguration("/**",config);
        //3. 返回新的CorsFilter
        return new CorsFilter(corsConfigurationSource);
    }
}
```







## 重写 WebMvcConfigurer

``` java
import org.springframework.context.annotation.Configuration;
import org.springframework.web.servlet.config.annotation.CorsRegistry;
import org.springframework.web.servlet.config.annotation.WebMvcConfigurer;

@Configuration
public class CorsConfig implements WebMvcConfigurer {
    @Override
    public void addCorsMappings(CorsRegistry registry) {
        registry.addMapping("/**")
                //是否发送Cookie
                .allowCredentials(true)
                //放行哪些原始域
                .allowedOriginPatterns("*")
                .allowedMethods(new String[]{"GET", "POST", "PUT", "DELETE"})
                .allowedHeaders("*")
                .exposedHeaders("*");
    }
}
```







## 使用注解 @CrossOrigin

``` java
import org.springframework.web.bind.annotation.CrossOrigin;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestMethod;
import org.springframework.web.bind.annotation.RestController;

import java.util.HashMap;
@RestController
//@CrossOrigin(origins = "*")
@CrossOrigin(value = "http://127.0.0.1:8080") //指定具体ip允许跨域
public class testController {
    @RequestMapping(value = "/Admin" , method = {RequestMethod.GET,RequestMethod.POST})
//    //@CrossOrigin(origins = "*")
//    @CrossOrigin(value = "http://127.0.0.1:8080") //指定具体ip允许跨域
    public HashMap test(String username, String password) {
        System.out.println("username = " + username);
        System.out.println("password = " + password);
        HashMap<String, String> stringStringHashMap = new HashMap<>();
        stringStringHashMap.put("msg", "username");
        stringStringHashMap.put("code", "0");
        return stringStringHashMap;
    }
    @RequestMapping(value = "/test" , method = {RequestMethod.GET,RequestMethod.POST})
    public String test1() {
        return "test";
    }
}
```





## 手动设置响应头 (HttpServletResponse)

使用 HttpServletResponse 对象添加响应头(Access-Control-Allow-Origin)来授权原始域，这里 Origin的值也可以设置为 “*”,表示全部放行。推荐：150道常见的Java面试题分解汇总

``` java
@RequestMapping("/index")
public String index(HttpServletResponse response) {
    response.addHeader("Access-Allow-Control-Origin","*");
    return "index";
}
```



## **使用自定义filter实现跨域**
