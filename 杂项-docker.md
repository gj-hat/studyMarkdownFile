# Docker



## 认识Docker



Docker如何解决大型项目依赖关系复杂, 不同组件依赖的兼容性问题

+ Docker允许开发中将应用, 依赖, 函数库, 配置一起打包, 形成新的可移植镜像
+ Docker应用运行在容器中, 使用沙箱机制, 相互隔离

Docker如何解决开发测试生产环境上的差异呢?

+ Docker镜像中包含完整运行环境, 包括系统函数库, 仅依赖系统的Linux内核, 因此Docker可以在任何Linux操作系统上运行



镜像和容器:

+ 镜像

  Docker将应用程序及其所需的依赖, 函数库, 环境, 配置等的文件一起打包为一个文件. 称之为镜像

+ 容器

  镜像中的应用程序运行后形成的进程就是容器, 只是Docker会给容器做隔离, 对外不可见. 且容器在运行时是只读的 

 

Docker和DockerHub

+ DockerHub: DockerHub是一个Docker镜像的托管平台. 这样的平台称之为Docker Registry 



Docker架构

Docker是一个CS架构的程序, 由两部分组成:

+ 服务端(server): Docker守护进程, 负责处理Docker指令. 管理镜像, 容器等
+ 客户端(client): 通过命令活RestApi向Docker服务端发送指令. 可以在本地或者远程向服务端发送指令





## Docker基本操作



### 镜像相关命令

+ 镜像名称一般由两部分组成: [repository]:[tag]

命令常用:

+ Docker images
+ docker rmi
+ Docker pull
+ docker push
+ docker save
+ docker load

![image-20211231160937264](https://tva1.sinaimg.cn/large/008i3skNly1gxx2yln8nij31j60u0gp9.jpg)





### 容器相关命令



常用命令:

+ docker run    从镜像->容器
+ Docker pause/unpause     暂停/启动
+ docker start/stop

+ Docker ps       查看所有运行的容器及状态
+ docker logs     查看容器运行日志
+ docker exec  进入容器执行命令
  + Docker exec    进入容器内部执行一些命令
  + Docker exex -it    给当前进入的容器创建一个标准输入输出终端 允许我们与容器交互
  + Docker exec -it name    name是进入的容器名
  + docker exec -it name bash   进入容器后执行的命令   

+ Docker rm    删除指定容器

![image-20211231165100144](https://tva1.sinaimg.cn/large/008i3skNly1gxx45la0frj31kd0u041n.jpg)



 





### 数据卷操作

将数据卷挂在在docker中(挂在你懂)   以实现容器和数据的解耦合

数据卷的操作命令: `docker volume [command]`

其中command如下:

+  create   新增一个数据卷
+ inspect    显示一个或者多个volume信息
+ ls     列出所有的volume
+ prune     删除未使用的volume
+ rm    删除一个或多个指定的volume



### 挂在数据卷  使用数据卷挂在 可以不用管配置问题

语法: `docker run -v  宿主机目录(数据卷):docker中目录`





### 镜像结构

+ 镜像是将应用程序极其需要的系统函数库, 环境, 配置, 依赖打包而成.

以mysql为例:

![image-20211231210904635](https://tva1.sinaimg.cn/large/008i3skNly1gxxbm7e25aj31ic0u00xq.jpg)





### DockerFile

DockerFile就是一个文本文件, 其中包含一个个的指令, 用指令来说明要执行什么操作来构建镜像, 每一个指令都会形成一层Loyer(上图)

![image-20211231211243696](https://tva1.sinaimg.cn/large/008i3skNly1gxxbpwzjmij31xq0tyjx6.jpg)

实例:

1. 将java项目制作成DockerFile (其实就是安装一个jdk 然后暴露一个端口  还有启动命令;)

![image-20211231212450959](https://tva1.sinaimg.cn/large/008i3skNly1gxxc4oo9klj30u012l42f.jpg)



2. 将jdk, java项目, Dockerfile拷贝到一个目录

    ![image-20211231212752533](https://tva1.sinaimg.cn/large/008i3skNly1gxxc5o14ckj313209ajsu.jpg)



3. 构建

   运行命令:

   ``` 
   docker build 构建的镜像名:版本 .      # .指的是dockerfile所在的目录 其实主要用的是DockerFile一个文件
   ```

   



 





## Docker-Compose

https://www.bilibili.com/video/BV1LQ4y127n4?p=59&spm_id_from=pageDriver

Docker部署服务器集群





## Docker镜像服务















































e