# Rabbit MQ



## 01_基础



### 1.1 概念

AMQP，即Advanced Message Queuing Protocol，高级消息队列协议，是应用层协议的一个开放标准，为面向消息的中间件设计。消息中间件主要用于组件之间的解耦，消息的发送者无需知道消息使用者的存在，反之亦然。
AMQP的主要特征是面向消息、队列、路由（包括点对点和发布/订阅）、可靠性、安全。
RabbitMQ是一个开源的AMQP实现，服务器端用Erlang语言编写，支持多种客户端，如：Python、Ruby、.NET、Java、JMS、C、PHP、ActionScript、XMPP、STOMP等，支持AJAX。用于在分布式系统中存储转发消息，在易用性、扩展性、高可用性等方面表现不俗。

AMQP原理图:

![image-20211217093759057](https://tva1.sinaimg.cn/large/008i3skNly1gxgkyq9jk8j31pc0lagn8.jpg)

RabbitMQ原理图:

![image-20211217094001356](https://tva1.sinaimg.cn/large/008i3skNly1gxgl0ukuorj31pw0kudjj.jpg)





### 1.2 MQ的优劣势

+ 优点:
  + 应用解耦
  + 异步提速    多个系统之间并发同时进行
  + 削峰填谷:   业务量突然增大 (做活动)
+ 劣势
  + 系统可用性降低    对MQ的依赖性大幅提高  
  + 系统复杂度提高    
  + 一致性问题         分布式系统系统的事务  





### 1.3 什么时候使用MQ

1. 生产者不需要从消费者处获得反馈.引入消息队列之前的直接调用, 其接口的返回值应该为空, 这才让明明下层的动作还没做, 上层却当成动作做完了继续往后走,即所谓异步成为可能
2. 容许短暂的不一致性
3. 确实是用了有效果, 即解耦提速削峰这些方面的收益. 超过加入MQ 管理MQ这些成本



### 1.4 其他的MQ产品

![image-20211217093114897](https://tva1.sinaimg.cn/large/008i3skNly1gxgkrr4ig6j31ue0tctgi.jpg)



### 1.5 RabbitMQ6中工作模式

+ 简单模式
+ work queues
+ Publish/Subscribe发布与订阅模式
+ Routing路由模式
+ Topices主题模式
+ RPC远程调用模式



### 1.6 JMS

+ JMS即java消息服务(javaMessage Service)应用程序接口, 是一个Java平台中关于面向消息中间件的API
+ JMS是JavaEE规范的一种 类比JDBC
+ ActiveMQ没有遵守了这个规范    RabbitMQ官方没有提供JSM的实现包,但是开源社区有

 

### 1.7 基础概念总结

1. RabbitMQ是基于AMQP协议使用Erlang语言开发的一款消息队列产品
2. RabbitMQ提供六种工作模式  但是只有五种比较重要
3. AMQP是协议    类比HTTP
4. JMS是API接口规范 类比JDBC





## 02_快速入门



**地址:**

```
http://127.0.0.1:15672
```



**图示:**

![image-20211217150405377](https://tva1.sinaimg.cn/large/008i3skNly1gxgue1ph6qj30jw094t8v.jpg)



1. 创建两个工厂  

   1. 生产者
   2. 消费者

2. pom依赖

   ``` xml
   <dependencies>
     <dependency>
       <groupId>com.rabbitmq</groupId>
       <artifactId>amqp-client</artifactId>
       <version>5.14.0</version>
     </dependency>
   </dependencies>
   <build>
     <plugins>
       <plugin>
         <groupId>org.apache.maven.plugins</groupId>
         <artifactId>maven-compiler-plugin</artifactId>
         <version>3.8.0</version>
         <configuration>
           <source>1.8</source>
           <target>1.8</target>
         </configuration>
       </plugin>
     </plugins>
   </build>
   ```

   

### 3.1 生产者(简单模式)

``` java
public class Producer_HelloWorld {
    public static void main(String[] args) throws IOException, TimeoutException {
        // 1. 创建连接工厂
        ConnectionFactory factory = new ConnectionFactory();
        // 2. 设置参数
        factory.setHost("127.0.0.1");      // 默认值localhost
        factory.setPort(5672);             // 默认值5672
        factory.setVirtualHost("/JiaGuo");    // 虚拟机  默认/
        factory.setUsername("Jia_Guo");     // 默认guest
        factory.setPassword("321321");       // 默认guest
        // 3. 创建连接  Connection
        Connection connection = factory.newConnection();
        // 4. 创建Channel
        Channel channel = connection.createChannel();
        // 5. 创建队列queue
        /**
         *    参数
         *    String queue,        队列名称
         *    boolean durable,       是否持久化  mq重启后就消失了
         *    boolean exclusive,          一般False    是否独占   只能由一个消费者监听队列,当Connection关闭时是否删除队列
         *    boolean autoDelete,             是否自动删除 没有Consumer时自动删除
         *    Map<String, Object> arguments
         */
        // 如果有一个hello_World队列,则会创建该队列,如果有则不创建
        channel.queueDeclare("hello_World", true, false, false, null);
        // 6. 发送消息
        /**
         *     参数
         *     String exchange,        交换机名   简单模式下交换机使用默认 即""
         *     String routingKey,       路由名称  如果是简单模式  路由名要和队列名一样 会自动绑定
         *     BasicProperties props,      配置信息
         *     byte[] body                 发送消息数据
         *
         */
        String st = "Hello_World";
        channel.basicPublish("", "hello_World", null, st.getBytes());
        // 7. 释放资源
        channel.close();
        connection.close();
    }
}
```



+ 执行后可以在浏览器控制面板看到有一条待消费的队列





### 3.2 消费者

``` java
public class consumer {
    public static void main(String[] args) throws IOException, TimeoutException {
        // 1. 创建连接工厂
        ConnectionFactory factory = new ConnectionFactory();
        // 2. 设置参数
        factory.setHost("127.0.0.1");      // 默认值localhost
        factory.setPort(5672);             // 默认值5672
        factory.setVirtualHost("/JiaGuo");    // 虚拟机  默认/
        factory.setUsername("Jia_Guo");     // 默认guest
        factory.setPassword("321321");       // 默认guest
        // 3. 创建连接  Connection
        Connection connection = factory.newConnection();
        // 4. 创建Channel
        Channel channel = connection.createChannel();
        // 6. 接收消息
        /**
         *   参数
         *       String queue,       队列名称
         *       boolean autoAck,      释放自动确认  回执
         *       Consumer callback       回调对象
         */
        Consumer consumer = new DefaultConsumer(channel){
            /**
             * 回调方法 当收到消息后 自动执行该方法
             * @param consumerTag       标识
             * @param envelope          获取一些信息, 比如交换机 路由信息
             * @param properties        获取配置信息
             * @param body              数据
             * @throws IOException
             */
            @Override
            public void handleDelivery(String consumerTag, Envelope envelope, AMQP.BasicProperties properties, byte[] body) throws IOException {
                System.out.println("consumerTag = " + consumerTag);
                System.out.println("envelope = " + envelope);
                System.out.println("envelope.getExchange() = " + envelope.getExchange());
                System.out.println("properties = " + properties);
                System.out.println("body = " + new String(body));
            }
        };
        channel.basicConsume("hello_World",true, consumer);
    }
}
```

+ 控制台打印:

  ```
  consumerTag = amq.ctag-km3xcv82oSKfjgmUbjV5FQ
  envelope = Envelope(deliveryTag=1, redeliver=false, exchange=, routingKey=hello_World)
  envelope.getExchange() = 
  properties = #contentHeader<basic>(content-type=null, content-encoding=null, headers=null, delivery-mode=null, priority=null, correlation-id=null, reply-to=null, expiration=null, message-id=null, timestamp=null, type=null, user-id=null, app-id=null, cluster-id=null)
  body = Hello_World
  ```

  



## 03_工作模式



### 3.1 Work queues 工作队列模式

<font color=#FF00; size=5>特点: 一条消费只能被一个消费者使用 两个消费者抢占使用  应用于缓解单个服务器压力</font>

 **示例图:**

![image-20211217150109389](https://tva1.sinaimg.cn/large/008i3skNly1gxguazbuhyj30rw0awjrw.jpg)



模式说明:

+ 与入门模式相比多了一些消费者, 多个消费者共同消费同一个队列中的消息(竞争关系 只有一个消费者可以取到)
+ 应用:   任务过重或任务过多,使用工作队列可以分摊一个设备的压力



**实现:**(两个消费者代码一样)

``` java
//  ----------------------生产者--------------------------------------------
public class Producer_WorkQueues {
    public static void main(String[] args) throws IOException, TimeoutException {
        // 1. 创建连接工厂
        ConnectionFactory factory = new ConnectionFactory();
        // 2. 设置参数
        factory.setHost("127.0.0.1");      // 默认值localhost
        factory.setPort(5672);             // 默认值5672
        factory.setVirtualHost("/JiaGuo");    // 虚拟机  默认/
        factory.setUsername("Jia_Guo");     // 默认guest
        factory.setPassword("321321");       // 默认guest
        // 3. 创建连接  Connection
        Connection connection = factory.newConnection();
        // 4. 创建Channel
        Channel channel = connection.createChannel();
        // 5. 创建队列queue
        // 如果有一个Work_Queues队列,则会创建该队列,如果有则不创建
        channel.queueDeclare("Work_Queues", true, false, false, null);
        // 6. 发送消息     队列里加入10条消息
        String st ;
        int i = 0;
        while (i < 10) {
            st = "Work_Queues_msg....." + i;
            channel.basicPublish("", "Work_Queues", null, st.getBytes());
            i++;
        }
        // 7. 释放资源
        channel.close();
        connection.close();
    }
}

//  ----------------------消费者--------------------------------------------
public class Consumer2 {
    public static void main(String[] args) throws IOException, TimeoutException {
        // 1. 创建连接工厂
        ConnectionFactory factory =  new ConnectionFactory();
        // 2. 设置参数
        factory.setHost("127.0.0.1");      // 默认值localhost
        factory.setPort(5672);             // 默认值5672
        factory.setVirtualHost("/JiaGuo");    // 虚拟机  默认/
        factory.setUsername("Jia_Guo");     // 默认guest
        factory.setPassword("321321");       // 默认guest
        // 3. 创建连接  Connection
        Connection connection = factory.newConnection();
        // 4. 创建Channel
        Channel channel = connection.createChannel();
        // 6. 接收消息
        Consumer consumer = new DefaultConsumer(channel){
            @Override
            public void handleDelivery(String consumerTag, Envelope envelope, AMQP.BasicProperties properties, byte[] body) throws IOException {
                System.out.println("consumerTag = " + consumerTag);
                System.out.println("envelope = " + envelope);
                System.out.println("envelope.getExchange() = " + envelope.getExchange());
                System.out.println("properties = " + properties);
                System.out.println("body = " + new String(body));
            }
        };
        channel.basicConsume("Work_Queues",true, consumer);
    }
}

//  ----------------------消费者--------------------------------------------
public class Consumer1 {
    public static void main(String[] args) throws IOException, TimeoutException {
        // 1. 创建连接工厂
        ConnectionFactory factory =  new ConnectionFactory();
        // 2. 设置参数
        factory.setHost("127.0.0.1");      // 默认值localhost
        factory.setPort(5672);             // 默认值5672
        factory.setVirtualHost("/JiaGuo");    // 虚拟机  默认/
        factory.setUsername("Jia_Guo");     // 默认guest
        factory.setPassword("321321");       // 默认guest
        // 3. 创建连接  Connection
        Connection connection = factory.newConnection();
        // 4. 创建Channel
        Channel channel = connection.createChannel();
        // 6. 接收消息
        Consumer consumer = new DefaultConsumer(channel){
            @Override
            public void handleDelivery(String consumerTag, Envelope envelope, AMQP.BasicProperties properties, byte[] body) throws IOException {
                System.out.println("consumerTag = " + consumerTag);
                System.out.println("envelope = " + envelope);
                System.out.println("envelope.getExchange() = " + envelope.getExchange());
                System.out.println("properties = " + properties);
                System.out.println("body = " + new String(body));
            }
        };
        channel.basicConsume("Work_Queues",true, consumer);
    }
}
```



### 3.2 Public/Subscribe(订阅模式)

<font color=#FF00; size=5>特点: 一条消息可以被两个消费者使用 两个消费者可以同时使用</font>

说明:

1. 引入交换机的概念, 生产者将消息不再发送给队列而是发送给交换机
2. 交换机: 一方面接收生产者发送的消息,另一方面知道如何处理消息,例如:1.将消息交给某个队列、2. 将消息交给所有队列、3. 将消息丢弃. 到底是哪一种取决于Exchange的类型. 常见的交换机类型有一下三种:
   1. Fanout: 广播,将消息交给所有绑定到交换机的队列
   2. Direct: 定向   将消息交给符合指定routing key的队列
   3. Topic: 通配符 把消息交给符合routing pattern(路由模式)的队列

3. Exchange(交换机) 只负责转发消息, 不具备任何存储消息的能力, 因此如果没有任何队列与交换机进行绑定,或者没有符合规则的队列,那么数据会丢失



#### 3.2.1 fanout广播模式  

Fanout: 广播,将消息交给所有绑定到交换机的队列

即  所有绑定的队列都可以收到消息

图示:



![image-20211217153822734](https://tva1.sinaimg.cn/large/008i3skNly1gxgvdpmv4uj30os0b2mxn.jpg)

**代码实现:**

``` java
//  ----------------------生产者--------------------------------------------
public class Producer_PubSub {
    public static void main(String[] args) throws IOException, TimeoutException {
        // 1. 创建连接工厂
        ConnectionFactory factory = new ConnectionFactory();
        // 2. 设置参数
        factory.setHost("127.0.0.1");      // 默认值localhost
        factory.setPort(5672);             // 默认值5672
        factory.setVirtualHost("/JiaGuo");    // 虚拟机  默认/
        factory.setUsername("Jia_Guo");     // 默认guest
        factory.setPassword("321321");       // 默认guest
        // 3. 创建连接  Connection
        Connection connection = factory.newConnection();
        // 4. 创建Channel
        Channel channel = connection.createChannel();
        // 5. 创建交换机
        /**
         *     参数
         *     String exchange,       交换机名
         *     BuiltinExchangeType type,           交换机类型    (枚举或者String)
         *              DIRECT("direct"),        定向
         *              FANOUT("fanout"),        扇形(广播)   发送给每一个与之绑定的队列
         *              TOPIC("topic"),          通配符
         *              HEADERS("headers");         参数匹配
         *     boolean durable,             是否持久化
         *     boolean autoDelete,          是否自动删除
         *     boolean internal,            内部使用  一般是False
         *     Map<String, Object> arguments             参数
         */
        String exchange = "test_fanout";
        channel.exchangeDeclare(exchange, BuiltinExchangeType.FANOUT, true, false, false, null);
        // 6. 创建队列
        String queue1Name = "test_fanout_queue1";
        String queue2Name = "test_fanout_queue2";
        channel.queueDeclare(queue1Name, true, false, false, null);
        channel.queueDeclare(queue2Name, true, false, false, null);
        // 7. 绑定队列和交换机
        /**
         *     参数
         *     String queue,     队列名称
         *     String exchange,      交换机名
         *     String routingKey            路由键,绑定规则   如果交换机的类型是fanout类型则绑定规则为""
         */
        channel.queueBind(queue1Name,exchange,"");
        channel.queueBind(queue2Name,exchange,"");
        // 8. 发送消息
        String st = "PubSub_tets";
        channel.basicPublish(exchange, "",null,st.getBytes() );
        // 9. 释放资源
        channel.close();
        connection.close();
    }
}

//  ----------------------消费者--------------------------------------------
public class Consumer2 {
    public static void main(String[] args) throws IOException, TimeoutException {
        // 1. 创建连接工厂
        ConnectionFactory factory =  new ConnectionFactory();
        // 2. 设置参数
        factory.setHost("127.0.0.1");      // 默认值localhost
        factory.setPort(5672);             // 默认值5672
        factory.setVirtualHost("/JiaGuo");    // 虚拟机  默认/
        factory.setUsername("Jia_Guo");     // 默认guest
        factory.setPassword("321321");       // 默认guest
        // 3. 创建连接  Connection
        Connection connection = factory.newConnection();
        // 4. 创建Channel
        Channel channel = connection.createChannel();
        // 6. 接收消息
        Consumer consumer = new DefaultConsumer(channel){
            @Override
            public void handleDelivery(String consumerTag, Envelope envelope, AMQP.BasicProperties properties, byte[] body) throws IOException {
                System.out.println("consumerTag = " + consumerTag);
                System.out.println("envelope = " + envelope);
                System.out.println("envelope.getExchange() = " + envelope.getExchange());
                System.out.println("properties = " + properties);
                System.out.println("body = " + new String(body));
            }
        };
        String queue1Name = "test_fanout_queue2";
        channel.basicConsume(queue1Name,true, consumer);
    }
}

//  ----------------------消费者--------------------------------------------
public class Consumer1 {
    public static void main(String[] args) throws IOException, TimeoutException {
        // 1. 创建连接工厂
        ConnectionFactory factory =  new ConnectionFactory();
        // 2. 设置参数
        factory.setHost("127.0.0.1");      // 默认值localhost
        factory.setPort(5672);             // 默认值5672
        factory.setVirtualHost("/JiaGuo");    // 虚拟机  默认/
        factory.setUsername("Jia_Guo");     // 默认guest
        factory.setPassword("321321");       // 默认guest
        // 3. 创建连接  Connection
        Connection connection = factory.newConnection();
        // 4. 创建Channel
        Channel channel = connection.createChannel();
        // 6. 接收消息
        Consumer consumer = new DefaultConsumer(channel){
            @Override
            public void handleDelivery(String consumerTag, Envelope envelope, AMQP.BasicProperties properties, byte[] body) throws IOException {
                System.out.println("consumerTag = " + consumerTag);
                System.out.println("envelope = " + envelope);
                System.out.println("envelope.getExchange() = " + envelope.getExchange());
                System.out.println("properties = " + properties);
                System.out.println("body = " + new String(body));
            }
        };
        String queue2Name = "test_fanout_queue1";
        channel.basicConsume(queue2Name,true, consumer);
    }
}
```



#### 3.2.2 Routing路由定向模式

Direct: 定向   将消息交给符合指定routing key的队列

说明:

+ error 两个系统都收到
+ iufo, warning只有C2系统能收到

图示:

![image-20211217163926364](https://tva1.sinaimg.cn/large/008i3skNly1gxgx59fg1cj31260be0tq.jpg)



**代码实现:**

``` java
//  ----------------------生产者--------------------------------------------
public class Producer_Routing {
    public static void main(String[] args) throws IOException, TimeoutException {
        // 1. 创建连接工厂
        ConnectionFactory factory = new ConnectionFactory();
        // 2. 设置参数
        factory.setHost("127.0.0.1");      // 默认值localhost
        factory.setPort(5672);             // 默认值5672
        factory.setVirtualHost("/JiaGuo");    // 虚拟机  默认/
        factory.setUsername("Jia_Guo");     // 默认guest
        factory.setPassword("321321");       // 默认guest
        // 3. 创建连接  Connection
        Connection connection = factory.newConnection();
        // 4. 创建Channel
        Channel channel = connection.createChannel();
        // 5. 创建交换机
        /**
         *     参数
         *     String exchange,       交换机名
         *     BuiltinExchangeType type,           交换机类型    (枚举或者String)
         *              DIRECT("direct"),        定向
         *              FANOUT("fanout"),        扇形(广播)   发送给每一个与之绑定的队列
         *              TOPIC("topic"),          通配符
         *              HEADERS("headers");         参数匹配
         *     boolean durable,             是否持久化
         *     boolean autoDelete,          是否自动删除
         *     boolean internal,            内部使用  一般是False
         *     Map<String, Object> arguments             参数
         */
        String exchange = "test_direct";
        channel.exchangeDeclare(exchange, BuiltinExchangeType.DIRECT, true, false, false, null);
        // 6. 创建队列
        String queue1Name = "test_direct_queue1";
        String queue2Name = "test_direct_queue2";
        /**
         *    参数
         *    String queue,        队列名称
         *    boolean durable,       是否持久化  mq重启后就消失了
         *    boolean exclusive,          一般False    是否独占   只能由一个消费者监听队列,当Connection关闭时是否删除队列
         *    boolean autoDelete,             是否自动删除 没有Consumer时自动删除
         *    Map<String, Object> arguments
         */
        // 如果有一个queue1Name队列,则会创建该队列,如果有则不创建
        channel.queueDeclare(queue1Name, true, false, false, null);
        channel.queueDeclare(queue2Name, true, false, false, null);
        // 7. 绑定队列和交换机
        /**
         *     参数
         *     String queue,     队列名称
         *     String exchange,      交换机名
         *     String routingKey            路由键/路由名,绑定规则   如果交换机的类型是fanout类型则绑定规则为""
         */
        channel.queueBind(queue1Name,exchange,"error");
        channel.queueBind(queue2Name,exchange,"error");
        channel.queueBind(queue2Name,exchange,"info");
        channel.queueBind(queue2Name,exchange,"warning");
        // 8. 发送消息
        String st = "PubSub_test";
        /**
         *     参数
         *     String exchange,        交换机名   简单模式下交换机使用默认 即""
         *     String routingKey,       路由名称  如果是简单模式  路由名要和队列名一样 会自动绑定
         *     BasicProperties props,      配置信息
         *     byte[] body                 发送消息数据
         *
         */
        channel.basicPublish(exchange, "info",null,st.getBytes() );
        // 9. 释放资源
        channel.close();
        connection.close();
    }
}

//  ----------------------消费者--------------------------------------------
public class Consumer2 {
    public static void main(String[] args) throws IOException, TimeoutException {
        // 1. 创建连接工厂
        ConnectionFactory factory = new ConnectionFactory();
        // 2. 设置参数
        factory.setHost("127.0.0.1");      // 默认值localhost
        factory.setPort(5672);             // 默认值5672
        factory.setVirtualHost("/JiaGuo");    // 虚拟机  默认/
        factory.setUsername("Jia_Guo");     // 默认guest
        factory.setPassword("321321");       // 默认guest
        // 3. 创建连接  Connection
        Connection connection = factory.newConnection();
        // 4. 创建Channel
        Channel channel = connection.createChannel();
        // 6. 接收消息
        /**
         *   参数
         *       String queue,       队列名称
         *       boolean autoAck,      释放自动确认  回执
         *       Consumer callback       回调对象
         */
        Consumer consumer = new DefaultConsumer(channel){
            /**
             * 回调方法 当收到消息后 自动执行该方法
             * @param consumerTag       标识
             * @param envelope          获取一些信息, 比如交换机 路由信息
             * @param properties        获取配置信息
             * @param body              数据
             * @throws IOException
             */
            @Override
            public void handleDelivery(String consumerTag, Envelope envelope, AMQP.BasicProperties properties, byte[] body) throws IOException {
                System.out.println("队列2");
            }
        };
        String queue1Name = "test_direct_queue2";
        channel.basicConsume(queue1Name,true, consumer);
    }
}
//  ----------------------消费者--------------------------------------------
public class Consumer1 {
    public static void main(String[] args) throws IOException, TimeoutException {
        // 1. 创建连接工厂
        ConnectionFactory factory = new ConnectionFactory();
        // 2. 设置参数
        factory.setHost("127.0.0.1");      // 默认值localhost
        factory.setPort(5672);             // 默认值5672
        factory.setVirtualHost("/JiaGuo");    // 虚拟机  默认/
        factory.setUsername("Jia_Guo");     // 默认guest
        factory.setPassword("321321");       // 默认guest
        // 3. 创建连接  Connection
        Connection connection = factory.newConnection();
        // 4. 创建Channel
        Channel channel = connection.createChannel();
        // 6. 接收消息
        /**
         *   参数
         *       String queue,       队列名称
         *       boolean autoAck,      释放自动确认  回执
         *       Consumer callback       回调对象
         */
        Consumer consumer = new DefaultConsumer(channel){
            /**
             * 回调方法 当收到消息后 自动执行该方法
             * @param consumerTag       标识
             * @param envelope          获取一些信息, 比如交换机 路由信息
             * @param properties        获取配置信息
             * @param body              数据
             * @throws IOException
             */
            @Override
            public void handleDelivery(String consumerTag, Envelope envelope, AMQP.BasicProperties properties, byte[] body) throws IOException {
                System.out.println("队列1");
            }
        };
        String queue1Name = "test_direct_queue1";
        channel.basicConsume(queue1Name,true, consumer);
    }
}
```



#### 3.2.2 Topics路由通配符模式

Topic: 通配符 把消息交给符合routing pattern(路由模式)的队列

说明:   Topics的通配符模式是按照词为单位

+ `#`可以匹配零个或多个词
+ `*`只能匹配一个单词

图示:

![image-20211218123859421](https://tva1.sinaimg.cn/large/008i3skNly1gxhvterlxpj30va0cggmh.jpg)

代码实现:

``` java
//  ----------------------生产者--------------------------------------------
public class _Routing {
    public static void main(String[] args) throws IOException, TimeoutException {
        // 1. 创建连接工厂
        ConnectionFactory factory = new ConnectionFactory();
        // 2. 设置参数
        factory.setHost("127.0.0.1");      // 默认值localhost
        factory.setPort(5672);             // 默认值5672
        factory.setVirtualHost("/JiaGuo");    // 虚拟机  默认/
        factory.setUsername("Jia_Guo");     // 默认guest
        factory.setPassword("321321");       // 默认guest
        // 3. 创建连接  Connection
        Connection connection = factory.newConnection();
        // 4. 创建Channel
        Channel channel = connection.createChannel();
        // 5. 创建交换机
        /**
         *     参数
         *     String exchange,       交换机名
         *     BuiltinExchangeType type,           交换机类型    (枚举或者String)
         *              DIRECT("direct"),        定向
         *              FANOUT("fanout"),        扇形(广播)   发送给每一个与之绑定的队列
         *              TOPIC("topic"),          通配符
         *              HEADERS("headers");         参数匹配
         *     boolean durable,             是否持久化
         *     boolean autoDelete,          是否自动删除
         *     boolean internal,            内部使用  一般是False
         *     Map<String, Object> arguments             参数
         */
        String exchange = "test_topics";
        channel.exchangeDeclare(exchange, BuiltinExchangeType.TOPIC, true, false, false, null);
        // 6. 创建队列
        String queue1Name = "test_topics_queue1";
        String queue2Name = "test_topics_queue2";
        /**
         *    参数
         *    String queue,        队列名称
         *    boolean durable,       是否持久化  mq重启后就消失了
         *    boolean exclusive,          一般False    是否独占   只能由一个消费者监听队列,当Connection关闭时是否删除队列
         *    boolean autoDelete,             是否自动删除 没有Consumer时自动删除
         *    Map<String, Object> arguments
         */
        // 如果有一个queue1Name队列,则会创建该队列,如果有则不创建
        channel.queueDeclare(queue1Name, true, false, false, null);
        channel.queueDeclare(queue2Name, true, false, false, null);
        // 7. 绑定队列和交换机
        /**
         *     参数
         *     String queue,     队列名称
         *     String exchange,      交换机名
         *     String routingKey            路由键/路由名,绑定规则   如果交换机的类型是fanout类型则绑定规则为""
         */
        channel.queueBind(queue1Name,exchange,"error.*");
        channel.queueBind(queue2Name,exchange,"error.#");
        channel.queueBind(queue2Name,exchange,"*.*info");
        channel.queueBind(queue2Name,exchange,"*.warning.*");
        // 8. 发送消息
        String st = "PubSub_test";
        /**
         *     参数
         *     String exchange,        交换机名   简单模式下交换机使用默认 即""
         *     String routingKey,       路由名称  如果是简单模式  路由名要和队列名一样 会自动绑定
         *     BasicProperties props,      配置信息
         *     byte[] body                 发送消息数据
         *
         */
      // 这里的info就是路有键/路由名     之后怎么匹配由上面的规则去匹配
        channel.basicPublish(exchange, "info",null,st.getBytes() );
        // 9. 释放资源
        channel.close();
        connection.close();
    }
}

//  ----------------------消费者--------------------------------------------
public class Consumer2 {
    public static void main(String[] args) throws IOException, TimeoutException {
        // 1. 创建连接工厂
        ConnectionFactory factory = new ConnectionFactory();
        // 2. 设置参数
        factory.setHost("127.0.0.1");      // 默认值localhost
        factory.setPort(5672);             // 默认值5672
        factory.setVirtualHost("/JiaGuo");    // 虚拟机  默认/
        factory.setUsername("Jia_Guo");     // 默认guest
        factory.setPassword("321321");       // 默认guest
        // 3. 创建连接  Connection
        Connection connection = factory.newConnection();
        // 4. 创建Channel
        Channel channel = connection.createChannel();
        // 6. 接收消息
        /**
         *   参数
         *       String queue,       队列名称
         *       boolean autoAck,      释放自动确认  回执
         *       Consumer callback       回调对象
         */
        Consumer consumer = new DefaultConsumer(channel){
            /**
             * 回调方法 当收到消息后 自动执行该方法
             * @param consumerTag       标识
             * @param envelope          获取一些信息, 比如交换机 路由信息
             * @param properties        获取配置信息
             * @param body              数据
             * @throws IOException
             */
            @Override
            public void handleDelivery(String consumerTag, Envelope envelope, AMQP.BasicProperties properties, byte[] body) throws IOException {
                System.out.println("队列2");
            }
        };
        String queue1Name = "test_topics_queue2";
        channel.basicConsume(queue1Name,true, consumer);
    }
}
//  ----------------------消费者--------------------------------------------
public class Consumer1 {
    public static void main(String[] args) throws IOException, TimeoutException {
        // 1. 创建连接工厂
        ConnectionFactory factory = new ConnectionFactory();
        // 2. 设置参数
        factory.setHost("127.0.0.1");      // 默认值localhost
        factory.setPort(5672);             // 默认值5672
        factory.setVirtualHost("/JiaGuo");    // 虚拟机  默认/
        factory.setUsername("Jia_Guo");     // 默认guest
        factory.setPassword("321321");       // 默认guest
        // 3. 创建连接  Connection
        Connection connection = factory.newConnection();
        // 4. 创建Channel
        Channel channel = connection.createChannel();
        // 6. 接收消息
        /**
         *   参数
         *       String queue,       队列名称
         *       boolean autoAck,      释放自动确认  回执
         *       Consumer callback       回调对象
         */
        Consumer consumer = new DefaultConsumer(channel){
            /**
             * 回调方法 当收到消息后 自动执行该方法
             * @param consumerTag       标识
             * @param envelope          获取一些信息, 比如交换机 路由信息
             * @param properties        获取配置信息
             * @param body              数据
             * @throws IOException
             */
            @Override
            public void handleDelivery(String consumerTag, Envelope envelope, AMQP.BasicProperties properties, byte[] body) throws IOException {
                System.out.println("队列1");
            }
        };
        String queue1Name = "test_topics_queue1";
        channel.basicConsume(queue1Name,true, consumer);
    }
}
```





## 04_Spring整合RabbitMQ

> sprinboot提供了RabbitMQ的模板对象  RabbitTemplate,使用的时候直接注入就可以了



### 4.1 简单概括

1. MQ主要分为三大部分

   1. 生产者
   2. 中间件  (消息队列  交换机 队列 绑定….)
   3. 消费者

2. 生产者

   发送消息 一般消息带有三部分

   1. 交换机名
   2. 路由键   (也可以成为消息想要去的地方 类似于网络模型的目的地IP地址)
   3. 消息内容

3. 中间件

   实时监听是否有消息传过来,如果有执行以下操作,   根据目的地址(发送方的路由键)进行分发处理到对应的队列,  队列的消费者监听到有数据产生,则进行处理

   注意: 

   + 中间件类似于OSI模型的二层和三层,只关注消息如何进行分发,而不关心消息内容
   + 队列不对生产者公开,由交换机内部进行分发,  等同于网络中的目的地址后的端口映射
   + 队列类似于下一跳转

   



### 4.2 快速入门

#### 实现步骤:

1. 创建SpringBoot工厂
2. 引入坐标和yml配置(同上)
3. 定义交换机,队列,绑定等等
4. 注入RabbitTemplate,调用方法,完成消息发送



代码实现:

1. maven依赖

   ``` xml
   <dependency>
     <groupId>org.springframework.boot</groupId>
     <artifactId>spring-boot-starter-amqp</artifactId>
   </dependency>
   ```

2. yml配置

   ``` yaml
   spring:
     rabbitmq:
       host: 127.0.0.1
       port: 5672
       username: Jia_Guo
       password: 321321
       virtual-host: /JiaGuo
   ```

   

3. 配置类(配置交换机 队列 绑定关系等等)

   ```java
   package com.rabbitmqspringboot.config;
   
   import org.springframework.amqp.core.*;
   import org.springframework.beans.factory.annotation.Qualifier;
   import org.springframework.context.annotation.Bean;
   import org.springframework.context.annotation.Configuration;
   
   /**
    * @author ：JiaGuo
    * @emil ：1520047927@qq.com
    * @date ：Created in 2021/12/20 09:23
    * @description：RabbitMQ的配置
    * @modified By：
    * @version: 1.0
    */
   @Configuration
   public class RabbitmqConfig {
       public static final String TOPIC_EXCHANGE_NAME = "topic_exchange";
       public static final String DIRECT_EXCHANGE_NAME = "direct_exchange";
       public static final String FANOUT_EXCHANGE_NAME = "fanout_exchange";
       public static final String HEADERS_EXCHANGE_NAME = "headers_exchange";
       public static final String QUEUE_NAME = "boot_queue";
       // 1. 交换机   四种交换机
       @Bean("topicExchange")
       public Exchange topic() {
           // 如要进行配置 在后面直接点就可以了   具体点进去源码看看
           return ExchangeBuilder.topicExchange(TOPIC_EXCHANGE_NAME).durable(true).build();
       }
       @Bean("directExchange")
       public Exchange direct() {
           // 如要进行配置 在后面直接点就可以了   具体点进去源码看看
           return ExchangeBuilder.topicExchange(DIRECT_EXCHANGE_NAME).durable(true).build();
       }
       @Bean("fanoutExchange")
       public Exchange fanout() {
           // 如要进行配置 在后面直接点就可以了   具体点进去源码看看
           return ExchangeBuilder.topicExchange(FANOUT_EXCHANGE_NAME).durable(true).build();
       }
   
       @Bean("headersExchange")
       public Exchange heads() {
           // 如要进行配置 在后面直接点就可以了   具体点进去源码看看
           return ExchangeBuilder.topicExchange(HEADERS_EXCHANGE_NAME).durable(true).build();
       }
       // 2. 队列
       @Bean("bootQueue")
       public Queue bootQueue(){
           // 同交换机  可以链式调用配置参数
           return QueueBuilder.durable(QUEUE_NAME).build();
       }
       // 3. 绑定关系
       /**
        * 1. 提前想好队列名
        * 2. 想好绑定的交换机
        * 3. 路由键想好
        */
       @Bean
       public Binding bingQueueExchange(@Qualifier("bootQueue") Queue queue, @Qualifier("topicExchange") Exchange exchange) {
           // 同理想配置更多 则加入and
           return BindingBuilder.bind(queue).to(exchange).with("boot.#").noargs();
   
       }
   }
   ```

   

4. 生产者

   ``` java
   @SpringBootTest
   @RunWith(SpringRunner.class)
   public class RabbitMQ_Test {
       // 注入
       @Autowired
       private RabbitTemplate rabbitTemplate;
       @Test
       public void test() {      rabbitTemplate.convertAndSend(RabbitmqConfig.TOPIC_EXCHANGE_NAME,"boot.routing.key","让我看看收到消息了没有");
       }
   }
   ```

5. 消费者(通常在不同的工程中   所以记得完成上述的1,2   依赖和yml)

   ``` java
   @Component
   public class Consumer {
       // 表示监听的队列boot_queue
       @RabbitListener(queues = "boot_queue")
       public void consumer(String msg) {
           // 回调方法
           System.out.println("这是1: " + msg);
       }
   }
   ```

   

















