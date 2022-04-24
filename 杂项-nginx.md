# nginx



## 01_nginx简介



### 1.1 nginx是什么

​		Nginx (engine x) 是一个高性能的HTTP和反向代理web服务器，同时也提供了IMAP/POP3/SMTP服务。Nginx是由伊戈尔·赛索耶夫为俄罗斯访问量第二的Rambler.ru站点（俄文：Рамблер）开发的，第一个公开版本0.1.0发布于2004年10月4日。
​		其将源代码以类BSD许可证的形式发布，因它的稳定性、丰富的功能集、简单的配置文件和低系统资源的消耗而闻名。2011年6月1日，nginx 1.0.4发布。
​		Nginx是一款轻量级的Web 服务器/反向代理服务器及电子邮件（IMAP/POP3）代理服务器，在BSD-like 协议下发行。其特点是占有内存少，并发能力强，事实上nginx的并发能力在同类型的网页服务器中表现较好，中国大陆使用nginx网站用户有：百度、京东、新浪、网易、腾讯、淘宝等。



### 1.2 nginx特点

1. 可以高并发连接

   官方测试Nginx能够支撑5万并发连接，实际生产环境中可以支撑2~4万并发连接数。

   原因，主要是Nginx使用了最新的epoll（linux2.6内核）和kqueue（freeBSD）网路I/O模型，而Apache使用的是传统的Select模型，其比较稳定的Prefork模式为多进程模式，需要经常派生子进程，所以消耗的CPU等服务器资源，要比Nginx高很多。

2. 内存消耗少

   Nginx+PHP（FastCGI）服务器，在3万并发连接下，开启10个Nginx进程消耗150MB内存，15MB*10=150MB，开启的64个PHP-CGI进程消耗1280内存，20MB*64=1280MB，加上系统自身消耗的内存，总共消耗不到2GB的内存。

   如果服务器的内存比较小，完全可以只开启25个PHP-CGI进程，这样PHP-CGI消耗的总内存数才500MB。

3. 成本低廉

   购买F5BIG-IP、NetScaler等硬件负载均衡交换机，需要十多万到几十万人民币，而Nginx为开源软件，采用的是2-clause BSD-like协议，可以免费试用，并且可用于商业用途。

   BSD开源协议是一个给使用者很大自由的协议，协议指出可以自由使用、修改源代码、也可以将修改后的代码作为开源或专用软件再发布,

4.  配置文件非常简单

   网络和程序一样通俗易懂，即使，非专用系统管理员也能看懂。

5. 支持Rewrite重写

   能够根据域名、URL的不同，将http请求分到不同的后端服务器群组。

6. 内置的健康检查功能

   如果NginxProxy后端的某台Web服务器宕机了，不会影响前端的访问。

7. 节省带宽

   支持GZIP压缩，可以添加浏览器本地缓存的Header头。

8. 稳定性高

   用于反向代理，宕机的概率微乎其微。

9. 支持热部署

   Nginx支持热部署，它的自动特别容易，并且，几乎可以7天*24小时不间断的运行，即使，运行数个月也不需要重新启动，还能够在不间断服务的情况下，对软件版本进行升级。



### 1.3 正向代理和反向代理

**总结:**

正向代理: 服务器不知道用户的真实情况

反向代理: 用户不知道服务器的真实情况

1. 正向代理

   （正向代理代理的是客户端）
   代理也被称为正向代理，是一个位于客户端和目标服务器之间的代理服务器，客户端将发送的请求和制定的目标服务器都提交给代理服务器，然后代理服务器向目标服务器发起请求，并将获得的结果返回给客户端的过程，流程如下图:

   ![image-20211228142339192](https://tva1.sinaimg.cn/large/008i3skNly1gxtj1cuqfkj31em0iogms.jpg)

2. 反向代理

   （反向代理代理的是服务端）
   相对于代理服务，反向代理的对象就是服务器，即代理服务代理的时服务器而不是客户端，它的作用现在是代替服务器接受请求，而不在像正向代理那样代理客户端。

![image-20211228142503307](https://tva1.sinaimg.cn/large/008i3skNly1gxtj2tbbl9j31f20l40u4.jpg)



  

### 1.4 负载均衡

负载均衡（Load Balance），它在网络现有结构之上可以提供一种廉价、有效、透明的方法来扩展网络设备和服务器的带宽，并可以在一定程度上增加吞吐量、加强网络数据处理能力、提高网络的灵活性和可用性等。用官网的话说，它充当着网络流中“交通指挥官”的角色，“站在”服务器前处理所有服务器端和客户端之间的请求，从而最大程度地提高响应速率和容量利用率，同时确保任何服务器都没有超负荷工作。如果单个服务器出现故障，负载均衡的方法会将流量重定向到其余的集群服务器，以保证服务的稳定性。当新的服务器添加到服务器组后，也可通过负载均衡的方法使其开始自动处理客户端发来的请求。

简言之，负载均衡实际上就是将大量请求进行分布式处理的策略。









### 1.5 动静分离



动静分离是指在web服务器架构中，将静态页面与动态页面或者静态内容接口和动态内容接口分开不同系统访问的架构设计方法，进而提升整个服务访问性能和可维护性。







## 02_配置文件



### 2.1 位置

```
 etc/nginx/nginx.conf
```

注意:  nginx.conf中可能会有include文件 指的是多配置最后合并处理 



### 2.2 组成

nginx配置文件由三部分组成

1. 全局块

   指的是从配置文件开始到events之间的内容

2. events块

   指的是events{}之间的内容

3. http块

   指的是http{}之间的内容, 其中http中还包含

   + server块：配置虚拟主机的相关参数，一个http中可以有多个server。

   + location块：配置请求的路由，以及各种页面的处理情况



### 2.3 全局块

配置影响nginx全局的指令。一般有运行nginx服务器的用户组，nginx进程pid存放路径，日志存放路径，配置文件引入，允许生成worker process数等。

例如:

```nginx
user  nginx;
worker_processes  auto;         # 表示nginx并发数 

error_log  /var/log/nginx/error.log notice;
pid        /var/run/nginx.pid;
```





### 2.4 event块

配置影响nginx服务器或与用户的网络连接。有每个进程的最大连接数，选取哪种事件驱动模型处理连接请求，是否允许同时接受多个网路连接，开启多个网络连接序列化等。

例如:

``` nginx
events {
    worker_connections  1024;      # 最大连接数
}
```





### 2.5 http块

可以嵌套多个server，配置代理，缓存，日志定义等绝大多数功能和第三方模块的配置。

hhtp中又包含两部分: 

1. http块(http的全局块)

   如文件引入，mime-type定义，日志自定义，是否使用sendfile传输文件，连接超时时间，单连接请求数等。

2. server块

   配置虚拟主机的相关参数，一个http中可以有多个server。 server中包含location

   location块: 配置请求的路由，以及各种页面的处理情况。



**实例:**

``` nginx
http {
    include       /etc/nginx/mime.types;        # 文件拓展类型引入 太多了 不拷贝了
    default_type  application/octet-stream;
    log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
                      '$status $body_bytes_sent "$http_referer" '
                      '"$http_user_agent" "$http_x_forwarded_for"';
    access_log  /var/log/nginx/access.log  main;
    sendfile        on;
    #tcp_nopush     on;
    keepalive_timeout  65;
    #gzip  on;
    server {
    listen       80;                # 监听的端口
    listen  [::]:80;       
    server_name  localhost;              # 主机名称
    #access_log  /var/log/nginx/host.access.log  main;
    location / {                        									# 路径跳转  当发现请求路径有/ 就做下面的跳转
            root   /usr/share/nginx/html;
            index  index.html index.htm;
        }
        #error_page  404              /404.html;

        # redirect server error pages to the static page /50x.html
        #
        error_page   500 502 503 504  /50x.html;
        location = /50x.html {
            root   /usr/share/nginx/html;
        }
        # proxy the PHP scripts to Apache listening on 127.0.0.1:80
        #
        #location ~ \.php$ {                                    # 路径的正则表达式
        #    proxy_pass   http://127.0.0.1;
        #}
        # pass the PHP scripts to FastCGI server listening on 127.0.0.1:9000
        #
        #location ~ \.php$ {                  									# 路径的正则表达式
        #    root           html;
        #    fastcgi_pass   127.0.0.1:9000;
        #    fastcgi_index  index.php; 
        #    fastcgi_param  SCRIPT_FILENAME  /scripts$fastcgi_script_name;
        #    include        fastcgi_params;
        #}
        # deny access to .htaccess files, if Apache's document root
        # concurs with nginx's one
        #
        #location ~ /\.ht {  											     					# 路径的正则表达式
        #    deny  all;
        #}
    }
}
```





## 03_反向代理

需求:

1. 输入127.0.0.180/bbb跳转到百度

2. 输入

   ``` 
   127.0.0.1:80  ---------     127.0.0.1:8080
   127.0.0.1:81/aaa  ---------     127.0.0.1:8081
   127.0.0.1:81/bbb  ---------     127.0.0.1:8082
   ```

   

实现:

1. 需求1

   ``` nginx
   location  /bbb {
     proxy_pass  http://www.baidu.com;           # 跳转
   }
   ```

   

2. 需求2       值得一提 找不到的话 会回到根目录

   ``` nginx
   server {
       listen       80;
       server_name  localhost;
       location / {
        proxy_pass   http://127.0.0.1:8080;
       }
   }
    server{
       listen       81;
       server_name  localhost;
       location  / {
           proxy_pass   http://baidu.com;
       }
       location  /bbb {
           proxy_pass   http://jd.com;
       }
     location ~ /aaa {															# ~ 代表的正则表达式 里面只要有aaa就执行
           proxy_pass   http://taobao.com;
       }
   }
   ```

   



<font color="red" size="6px">值得一提:</font>.

location指令说明:  该指令用于匹配url

语法:

``` 
location [= | ~ | ~* | ^~] url{}
```



| 模式                | 含义                                                         |
| ------------------- | ------------------------------------------------------------ |
| location = /uri     | = 表示精确匹配                                               |
| location ^~ /uri    | ^ 进行前缀匹配，~ 表示区分大小写                             |
| location ~ pattern  | ~ 区分大小写的匹配                                           |
| location ~* pattern | ~* 不区分大小写的匹配                                        |
| location /uri       | 不带任何修饰符，也表示前缀匹配，但是在正则匹配之后           |
| location /          | 通用匹配，任何未匹配到其它 location 的请求都会匹配到，相当于 switch 中的 default |
| location !~         | 区分大小写不匹配                                             |
| location !~*        | 不区分大小写不匹配                                           |

**匹配优先级**

- 首先精确匹配 =
- 其次前缀匹配 ^~
- 其次是按文件中顺序的正则匹配
- 然后匹配不带任何修饰的前缀匹配
- 最后是交给 / 通用匹配
- 当有匹配成功时候，停止匹配，按当前匹配规则处理请求





## 04_负载均衡



### 4.1 基础款

 ``` nginx
 http{ 
   #......
   upstream serverName{
     server x.x.x.x:port weight = 1;
     server x.x.x.x:port weight = 1;
 		#  ......
   }
   server{
     listen 80;
     server_name 127.0.0.1 
     localtion /{
 #      ......
         proxy_pass http://serverName;
     }
     # ......
   }
 }
 ```



### 4.2 负载规则

常用的nginx提供的负载均衡策略:  

1. 源地址哈希法：

   每个请求按照访问ip的hash进行分配, 则访客再次访问时,  还可以访问到之前的服务器,  可以固定服务器进行访问

   ``` nginx
   http{ 
     #......
     upstream serverName{
       ip_hash;
       server x.x.x.x:port;
       server x.x.x.x:port;
   		#  ......
     }
   #  ......
   }
   ```

   

2. 轮询法：(默认)

   每个请求按时间顺序逐一分配到不同的服务器中, 如果服务器down了  自动剔除

   ``` nginx
   http{ 
     #......
     upstream serverName{
       server x.x.x.x:port ;
       server x.x.x.x:port ;
   		#  ......
     }
   #  ......
   }
   ```

3. 随机法：

   通过系统的随机算法，根据后端服务器的列表大小值来随机选取其中的一台服务器进行访问。

4. 加权轮询法：

   指定轮询概率 weight和访问概率成正比. 多用于服务器性能不统一的情况

   weight代表权重, 默认是1   权重越高分配的概率越高

   ``` nginx
   http{ 
     #......
     upstream serverName{
       server x.x.x.x:port weight = 1;
       server x.x.x.x:port weight = 10;      # 比上述多10倍
   		#  ......
     }
   #  ......
   }
   ```

   

5. 加权随机法：

   与加权轮询法一样，加权随机法也根据后端机器的配置，系统的负载分配不同的权重。不同的是，它是按照权重随机请求后端服务器，而非顺序。

6. 最小连接数法：

   由于后端服务器的配置不尽相同，对于请求的处理有快有慢，最小连接数法根据后端服务器当前的连接情况，动态地选取其中当前积压连接数最少的一台服务器来处理当前的请求，尽可能地提高后端服务的利用效率，将负责合理地分流到每一台服务器。

7. fair(第三方)

   按照后端服务器的相应时间进行分配.  相应时间越短 越优先

   ``` nginx
   http{ 
     #......
     upstream serverName{
       server x.x.x.x:port;
       server x.x.x.x:port;
       fair;
   		#  ......
     }
   #  ......
   }
   ```

   



## 05_动静分离

 两种动静分离方案:

1. 静态和动态是不同的服务器

2. 动态和静态是一个服务器, 由nginx进行分离

   使用nginx的location指定不同的后缀名进行匹配.  

   还可以使用expires参数设置, 可以设置浏览器缓存过期时间, 减少与服务器之间的请求和流量

   具体做法, 给一个资源设置一个过期时间, 这个时间内访问,浏览器发送一个请求, 服务器比对资源的最后更新时间, 如果没有更新则返回304, 如果有更新则返回200需要浏览器重新下载

​	

### 5.1 方案一实现:

``` nginx
http{ 
  server {
    listen       80;
    server_name  localhost;
    #拦截静态资源
    location ~ .*\.(html|htm|gif|jpg|jpeg|bmp|png|ico|js|css)$ {
      root /Users/dalaoyang/Downloads/static;      # 静态资源目录
    }
  }
  server {
    listen       8080;
    server_name  localhost;
    #拦截后台请求
    location / {
      proxy_pass http://localhost:8888;      # 后台地址
      proxy_set_header X-Real-IP $remote_addr;
    }
  }
}
```









### 5.2 方案二实现

``` nginx
http {
   server {
       listen       10000;
       server_name  localhost;
      #拦截后台请求
      location / {
        proxy_pass http://localhost:8888;
        proxy_set_header X-Real-IP $remote_addr;
      }

      #拦截静态资源
      location ~ .*\.(html|htm|gif|jpg|jpeg|bmp|png|ico|js|css)$ {
        root /Users/dalaoyang/Downloads/static;      # 静态资源目录
       }
    }
}
```





## 06_高可用(Keepalived)



利用Keepalived工具

原理:

1. 一共至少两台服务器(当然每台服务器也可以部署多个nginx)  nginx1 nginx2 ,每台服务器都需要安装keepalived工具
2. 在keepalived中配置虚拟ip和网卡的绑定(网卡就代表了nginx的地址)
3. 两台服务的网卡ip不同 但是虚拟ip是相同的
4. 通过虚拟ip进入主服务器的keepliaved中  在keepalived的检测脚本中可以判断本机的nginx有没有down
5. 如果有则进入备份 



配置文件:

1. /etc/keepalived/keepalived.conf

   ``` 
   ! Configuration File for keepalived
   global_defs {                                     # 全局定义
       notification_email {                          
           acassen@firewall.loc                      
       }                        
       notification_email_from test0@163.com         
       smtp_server smtp.163.com                      
       smtp_connect_timeout 30                       
       router_id LVS_DEVEL                               # 默认不动或者ip值  
       vrrp_skip_check_adv_addr                   
       vrrp_strict                                   
       vrrp_garp_interval 0                       
       vrrp_gna_interval 0     
       script_user keepalived_script                 
       enable_script_security                        
   }       
   
   vrrp_script chk_nginx_service {                   
       script "/etc/keepalived/chk_nginx.sh"            # 检测脚本
       interval 3                                       # 执行检测脚本的间隔 
       weight -20                                       # 权重参数
       fall 3                                           
       rise 2                                        
       user keepalived_script                        
   }                                             
   
   vrrp_instance VI_1 {                                  # 虚拟ip的配置 
       state MASTER                                     # 服务器状态 如果是主服务器就是master  不是就改为backup
       interface ens33                                  # 网卡名   
       virtual_router_id 51                             # 路由值  主备的值必须相同
       priority 100                                     # 主备机不同的优先级 主机较大  备机较小
       advert_int 1                                  
       authentication {                              
           auth_type PASS                        
           auth_pass 1111                        
       }                                         
       virtual_ipaddress {                           # VRRP虚拟ip地址 
           127.0.0.1                       
       }
       # track_script {                                
           # chk_nginx_service       #可加权重，但会覆盖声明的脚本权重值。chk_nginx_service weight -20                
       # }
           # notify_master "/etc/keepalived/start_haproxy.sh start"  #当前节点成为master时，通知脚本执行任务
           # notify_backup "/etc/keepalived/start_haproxy.sh stop"   #当前节点成为backup时，通知脚本执行任务
           # notify_fault  "/etc/keepalived/start_haproxy.sh stop"   #当当前节点出现故障，执行的任务;
   }                                             
   
   #     real_server 192.168.119.121 80 {          
   #         weight 1                              #权重，最大越高，lvs就越优先访问
   #         TCP_CHECK {                           #keepalived的健康检查方式HTTP_GET | SSL_GET | TCP_CHECK | SMTP_CHECK | MISC
   #             connect_timeout 10                #10秒无响应超时
   #             retry 3                           #重连次数3次
   #             delay_before_retry 3              #重连间隔时间
   #             connect_port 80                   #健康检查realserver的端口
   #         }                                     
   #     }                                         
   # }                                             
   
   # vrrp_instance VI_2 {                          #vrrp 实例部分定义，VI_1自定义名称
   #     state   BACKUP                            #指定 keepalived 的角色，必须大写 可选值：MASTER|BACKUP 分别表示（主|备）
   #     interface ens33                           #网卡设置，绑定vip的子接口，lvs需要绑定在网卡上，realserver绑定在回环口。区别：lvs对访问为外，realserver为内不易暴露本机信息
   #     virtual_router_id 52                      #虚拟路由标识，是一个数字，同一个vrrp 实例使用唯一的标识，MASTER和BACKUP 的 同一个 vrrp_instance 下 这个标识必须保持一致
   #     priority 90                               #定义优先级，数字越大，优先级越高。
   #     advert_int 1                              #设定 MASTER 与 BACKUP 负载均衡之间同步检查的时间间隔，单位为秒，两个节点设置必须一样
   #     authentication {                          #设置验证类型和密码，两个节点必须一致
   #         auth_type PASS                        
   #         auth_pass 1111                        
   #     }                                         
   #     virtual_ipaddress {                       #设置虚拟IP地址，可以设置多个虚拟IP地址，每行一个
   #         192.168.119.131                       
   #     }                                         
   # }                                             
   
   # virtual_server 192.168.119.131 80 {           #定义RealServer对应的VIP及服务端口，IP和端口之间用空格隔开
   #     delay_loop 6                              #每隔6秒查询realserver状态
   #     lb_algo rr                                #后端调试算法（load balancing algorithm）
   #     lb_kind DR                                #LVS调度类型NAT/DR/TUN
   #     #persistence_timeout 60                   #同一IP的连接60秒内被分配到同一台realserver
   #     protocol TCP                              #用TCP协议检查realserver状态
   #     real_server 192.168.119.120 80 {          
   #         weight 1                              #权重，最大越高，lvs就越优先访问
   #         TCP_CHECK {                           #keepalived的健康检查方式HTTP_GET | SSL_GET | TCP_CHECK | SMTP_CHECK | MISC
   #             connect_timeout 10                #10秒无响应超时
   #             retry 3                           #重连次数3次
   #             delay_before_retry 3              #重连间隔时间
   #             connect_port 80                   #健康检查realserver的端口
   #         }                                     
   #     }                                         
   #     real_server 192.168.119.121 80 {          
   #         weight 1                              #权重，最大越高，lvs就越优先访问
   #         TCP_CHECK {                           #keepalived的健康检查方式HTTP_GET | SSL_GET | TCP_CHECK | SMTP_CHECK | MISC
   #             connect_timeout 10                #10秒无响应超时
   #             retry 3                           #重连次数3次
   #             delay_before_retry 3              #重连间隔时间
   #             connect_port 80                   #健康检查realserver的端口
   #         }
   #     }
   # }
   ```

2. 检测脚本/etc/keepalived/chk_nginx.sh

   ``` 
   #!/bin/bash    
   if [ "$(ps -ef | grep "nginx: master process"| grep -v grep )" == "" ]    
   then    
   /usr/local/nginx/sbin/nginx    
   sleep 5    
   if [ "$(ps -ef | grep "nginx: master process"| grep -v grep )" == "" ]    
   then    
   killall keepalived    
   fi    
   fi  
   ```

   

   



































































