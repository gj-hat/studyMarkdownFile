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

1. work queues   

   work queue是一个较为基础的工作模式，其工作模式是多个消费者共同消费同一个队列中的信息。

   启动多个消费者，当生产者将消息发送给队列时，一条信息只会被一个消费者接收，rabbit采用轮询的方式将消息平均发送给消费者，消费者在处理完某个信息后才会接收到下一条信息。
   ![](https://tva1.sinaimg.cn/large/e6c9d24ely1h211egqlg0j20ak07hmxi.jpg)

2. Publish/Subscribe发布与订阅模式

   Publish/Subscribe模式下exchange的类型是fanout

   Publish/Subscribe模式是work queue的升级版，生产者通过交换机将信息转发给绑定到交换机的相应队列，每个绑定到交换机的队列都将收到信息；消费者会监听自己的队列，然后队列会将信息发送给监听它的消费者。
   ![](https://tva1.sinaimg.cn/large/e6c9d24ely1h211fc290bj20bw074t8z.jpg)

3. Routing路由模式

   Routing模式下exchange的类型是direct

   相较Publish/Subscribe模式该模式多了routingkey，即如上图所示，当routingkey设置为error时上下两个队列都会接收到来自交换机转发的信息；routingkey为info时，只有下面的队列能收到交换机转发来的信息。
   ![](https://tva1.sinaimg.cn/large/e6c9d24ely1h211fwts1cj20f0069aa5.jpg)

4. Topices通配符模式

   Topic模式下交换机类型为topic，该模式和Routing模式一样，只不过是这个模式下的routingkey可以用通配符

   统配符规则：
   中间以“.”分隔。
   符号#可以匹配多个词，符号*可以匹配一个词语。
   ![](https://tva1.sinaimg.cn/large/e6c9d24ely1h211gj3bfkj20fq067mx8.jpg)

5. Hreader模式 (简单模式)

   header模式与routing不同的地方在于，header模式取消routingkey，使用header中的 key/value（键值对）匹配 队列

   

6. RPC远程调用模式

   RPC即客户端远程调用服务端的方法 ，使用MQ可以实现RPC的异步调用，基于Direct交换机实现，流程如下：

   客户端即是生产者就是消费者，向RPC请求队列发送RPC调用消息，同时监听RPC响应队列。
   服务端监听RPC请求队列的消息，收到消息后执行服务端的方法，得到方法返回的结果
   服务端将RPC方法 的结果发送到RPC响应队列
   客户端（RPC调用方）监听RPC响应队列，接收到RPC调用结果。
   ![](https://tva1.sinaimg.cn/large/e6c9d24ely1h211h9elqrj20ik06h0t1.jpg)



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



### 3.1 work queues

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



### 3.2 fanout广播模式

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



### 3.3 Routing路由定向模式

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



### 3.4 Topics路由通配符模式

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
   //    public Exchange direct() {
   //        // 如要进行配置 在后面直接点就可以了   具体点进去源码看看
   //        return ExchangeBuilder.directExchange(DIRECT_EXCHANGE_NAME).durable(true).build();
   //    }
   //    @Bean("fanoutExchange")
   //    public Exchange fanout() {
   //        // 如要进行配置 在后面直接点就可以了   具体点进去源码看看
   //        return ExchangeBuilder.fanoutExchange(FANOUT_EXCHANGE_NAME).durable(true).build();
   //    }
   //
   //    @Bean("headersExchange")
   //    public Exchange heads() {
   //        // 如要进行配置 在后面直接点就可以了   具体点进去源码看看
   //        return ExchangeBuilder.headersExchange(HEADERS_EXCHANGE_NAME).durable(true).build();
   //    }
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

   



## 05_术语基础概念


### connection是什么，与TCP连接有什么关系？
  1. connection代表着一条真实的TCP连接，即物理连接，具有ip和port
  2. 该连接与RabbitMq服务器相连，大部分场景下设计时采用长连接
  3. 可以配置心跳来检测连接状态
  4. connection是线程安全的
  5. 创建connection的代价相对较高（其实也不高），且当connection较多时，RabbitMq服务的负荷会变高
  6. 使用完毕需要关闭

### channel是什么，为什么需要使用它？
  1. 可以把它看作是虚拟的连接，这里的虚拟是相对于connection的真实连接而言
  2. 该虚拟连接处于一个connection的内部，可以把它看作是connection上的session
  3. 在channel内进行真实的数据传输，所有的RabbitMq指令都是在channel内进行的
  4. 之所以使用channel，是因为创建真实的TCP连接开销较大，且真实连接太多时会增大RabbitMq服务的负荷
  5. channel不是线程安全的，一般每个线程维护自己的channel
  6. channel设计时就是短暂的，一旦该channel上出现错误，就会关闭，后续操作也不会成功
  7. 使用完毕需要关闭

### channel和connection之间到底是什么关系？
  1. 在应用中，先创建connection，然后在该connection上创建channel，在channel上传输数据
  2. 可以在一个connection内创建多个channel，协议支持最大值为65535，推荐默认值为2047
  3. 其它内容可以参考对channel的解释

### Exchange是什么？它是如何工作的？
  1. 生产者发送的消息全部都发到Exchange，而非直接到queue
  2. 负责从生产者接收消息，然后通过路由规则把消息路由到相应的queue
  3. 可以使用默认的Exchange，这里生产者和消费者共享一个同名的queue来传递消息
  4. 创建Exchange时指定名称和类型，类型通常是：dircet, topic, header, fanout，类型决定了路由规则
  5. 生产者把消息publish到指定的Exchange，并指定routing_key，路由规则根据routing_key进行路由
  6. 消费者创建一个queue，并使用routing_key把queue绑定到Exchange，以接收从Exchange路由到该queue的消息
  7. 各种类型的Exchange的具体说明请参考 RabbitMq入门简介

### queue是什么？
  1. 存放消息的队列，消息的传递必须进入到某个queue，且消息在queue中被消费
  2. 消费者创建queue，并把queue绑定到Exchange，依据绑定的routing_key接收消息
  3. 可以创建匿名的queue，由RabbitMq随机命名，使用完毕自动删除，queue命名时最长255字节（utf-8字符）
  4. 可以在创建queue时，设置消息持久化，这样RabbitMq重启后，queue依然存在
  5. Exclusive 属性决定了该queue只能被一个connection使用，且该connection关闭后自动删除queue

### bind是什么？为什么要bind？
  1. 生产者发布的消息都通过Exchange路由，不会直接发送给消费者
  2. 消费者要想取得消息，必须把queue绑定到Exchange，这样Exchange才知道需要把消息路由到这个queue
  3. 绑定时需要指定Exchange、queue和routing_key
  4. bind是一种把queue和Exchange连接起来的方式

### 总体数据流是怎样的？
   如图所示：
![](https://tva1.sinaimg.cn/large/e6c9d24ely1h20zdn8n84j20ca0dj0tc.jpg)



  1. 生产者发布消息到Exchange
  2. Exchange接收消息并负责路由消息
  3. 必须有queue已经binding到了Exchange，示例中Exchange根据路由规则把消息路由到两个queue（由Exchange的类型和routing_key决定）
  4. 消息会驻留在queue中，直到被消费者处理
  5. 最终消息被消费者



## 06_死信队列



### 6.1 概念

当消息发送到队列后,由于以下三种原因导致消息未被消费 则称为死信(死亡消息)

+ 消息被拒绝
+ 消息TTL过期
+ 队列达到最大长度

死信也需要一个交换机和队列,  原队列消息出现了上述三个原因则自动转发到死信交换机再到死信队列,最后由消费者消费

### 6.2 死信架构图

![](https://tva1.sinaimg.cn/large/e6c9d24ely1h21w64bzjqj21gk0nsdiq.jpg)

解释:

1. 生产者发送消息到普通交换机(类型任意, 此处以直连为例)
2. 普通交换机发送消息到普通队列
3. 消息出现了上述的三个原因导致该消息称为死信
4. 由普通队列发送消息到死信交换机
5. 死信交换机发送消息到死信队列
6. 消费者从死信队列中取出消息



### 6.3 实现

1. 配置类

``` java
@Configuration
public class RabbitmqConfig {
    // 声明交换机模式为 定向模式    按照Routing Key定向发送到队列
    public static final String NORMAL_EXCHANGE_NAME = "normal_exchange";
    public static final String DEAD_EXCHANGE_NAME = "dead_exchange";
    public static final String NORMAL_QUEUE_NAME = "normal_queue";
    public static final String DEAD_QUEUE_NAME = "dead_queue";
    // 路由key
    public static final String DEAD_ROUTING_KEY = "dead_routing_key";
    public static final String NORMAL_ROUTING_KEY = "normal_routing_key";
    // 普通交换机  路由定向模式
    @Bean("normalExchange")
    public Exchange driect() {
        // 如要进行配置 在后面直接点就可以了   具体点进去源码看看
        return ExchangeBuilder
                .directExchange(NORMAL_EXCHANGE_NAME)
                .durable(false)
                .build();
    }
    // 死信交换机  路由定向模式
    @Bean("deadExchange")
    public Exchange dead() {
        // 如要进行配置 在后面直接点就可以了   具体点进去源码看看
        return ExchangeBuilder
                .directExchange(DEAD_EXCHANGE_NAME)
                .durable(false)
                .build();
    }
    // 普通消息队列
    @Bean("normal_queue")
    public Queue normalQueue() {
        /**
         * 有三种情况成为死信队列
         * 1. 消息被拒绝
         * 2. 消息超时
         * 3. 队列满了
         */
        HashMap<String, Object> map = new HashMap<>();
        // 死信交换机
        map.put("x-dead-letter-exchange", DEAD_EXCHANGE_NAME);
        // 死信队列的路由key
        map.put("x-dead-letter-routing-key", DEAD_ROUTING_KEY);
        // 设置过期时间   单位是毫秒   当然也可以在生产者发送消息的时候设置
//        map.put("x-message-ttl", 10000);
        return QueueBuilder
                .durable(NORMAL_QUEUE_NAME)    // 默认是持久化的
//                .exclusive()    // 排他的  不共享
//                .autoDelete()  // 不自动删除
                .withArguments(map)   // 参数r
                .build();
    }
    // 死信队列
    @Bean("dead_queue")
    public Queue deadQueue() {
        return QueueBuilder
                .durable(DEAD_QUEUE_NAME)    // 默认是持久化的
//                .exclusive()    // 排他的  不共享
//                .autoDelete()  // 不自动删除
                .build();
    }
    // 绑定死信消息队列和交换机
    @Bean
    public Binding bingDeadQueueExchange(@Qualifier("dead_queue") Queue queue, @Qualifier("deadExchange") Exchange exchange) {

        return BindingBuilder.bind(queue)
                .to(exchange)
                .with(DEAD_ROUTING_KEY)
                .noargs();
    }
    // 绑定普通消息队列和交换机
    @Bean
    // 此处的队列不一定一开始创建 要么是消费者创建  如果没有消费者则不会触发创建(当然如果有生产者 也出触发这个方法)
    public Binding bingNormalQueueExchange(@Qualifier("normal_queue") Queue queue, @Qualifier("normalExchange") Exchange exchange) {
        // 同理想配置更多 则加入and
        return BindingBuilder.bind(queue)
                .to(exchange)
                .with(NORMAL_ROUTING_KEY)
                .noargs();
    }
}
```



2. 消费者

   ``` java
   @Component
   public class Consumer {
       // 表示监听的队列boot_queue  如果队列不存在则自动创建
       @RabbitListener(queues = "dead_queue")
       public void consumer(String msg, Channel channel) throws IOException {
           // 回调方法
           System.out.println("这是1: " + msg);
           // 手动确认消息
   //        channel.basicAck(1L, false);
           /**
            *String basicConsume(String queue, boolean autoAck, String consumerTag, boolean noLocal, boolean exclusive, Map<String, Object> arguments, Consumer callback) throws IOException;
            * 有多个同名不同参数的方法，此处用最全的方法举例
            * 参数1：队列名
            * 参数2：是否自动确认答复
            * 参数3：消费者标签
            * 参数4：是否禁止本地消费 不能将同一个Conenction中生产者发送的消息传递给这个Connection中 的消费者
            * 参数5：是否独占队列 排他
            * 参数6：消费者参数
            * 参数7：回调函数(如果使用这个需要是写一个回调函数,如果不使用这个可以不写)
            */
   //        channel.basicConsume("boot_queue", false, null);
       }
   }
   ```

3. 生产者(设置了ttl值)

   ``` java
   @Autowired
   private RabbitTemplate rabbitTemplate;
   public void test() {
           MessageProperties messageProperties = new MessageProperties();
           messageProperties.setExpiration("5000");
           Message message = new Message("hello".getBytes(), messageProperties);
           rabbitTemplate.convertAndSend("normal_exchange","normal_routing_key", message);
   
       }
   ```



## 07_消息应答机制

![](https://tva1.sinaimg.cn/large/e6c9d24ely1h21xt0q56zj21zm0hk0vu.jpg)

解释:

消息确认机制分为生产者发送消息确认和消费者接收消息确认

1. 发送消息确认机制: 由mqServer进行配置 确认后发送给生产者, 由生产者的回调进行处理
2. 接收消息机制: 由消费者发送给mqServer的对应的queue 由queue的回调进行处理

### 7.1发送消息

生产者给mq push消息的时候，我们怎么知道这消息放进了mq里面了呢？所以这引发了mq确认消息的相关问题。

**问题1:** 发布者确认消息机制是怎样的？

答：首先消息的传递机制是这样，发布者将消息发送到消息队列的exchange中，然后根据exchange的分发规则，分发到制定具体队列，如果开启了持久化，消息会复制一份持久化队列中， 持久化队列在收到消息后会给队列返回ack确认信息， 然后队列给exchange返回确认信息， exchange根据回调函数给发布者返回确认信息，这样发布者确认就算完成了。

　　当时你知道发布者的确认规则，你是不是立马想到确认要经过这么多个中间人，省略中间的环节行不行勒，哈哈答案是可以的，不过得自己去改造啦，这是性能优化的一项。

 

**问题2:**发布者确认的方式有哪几种？

答：有三种确认方式

1）同步确认，消息发出后一直处于阻塞状态等待确认消息，可以设置超时时间，如果超过超时时间则认为消息丢失， 如果超时或者消息确认失败则会抛出异常，我们捕获异常然后选择是重发还是等其他处理方式。

2）批量确认，一次性发出一批消息然后阻塞等待，形式和同步确认相似，有点则是一批确认所以性能上会有很大的提升，缺点是我们不能确认具体发生了什么错误，并且我们得在内存中存储这批消息以确认发送成功的消息和重发失败的消息。

3）异步确认，发送消息后注册一个回调函数，不阻塞线程，在消息确认后会调用回调函数







### 7.2 接收消息确认

mq推送了消息给消费者后，我们怎么知道消息被消费者消费了呢？万一消费者没有消费该消息，或者消费者挂了，这消息是不是就永久丢失了，所以根据此有下列几个关于消费者确认的相关问题。同时在这我们也纠正一个常见的误区，mq是推消息到消费者处的而不是消费者去mq中取消息的，然后在mq中消息充足的情况下，mq推消息给消费者不是等消费者消费完一个再推一个，而是根据 prefetch_count参数来决定可以推多个消息到消费者的缓存里面。

 

**问题1:** 如果其中一个消费者开始一项漫长的任务，而仅部分完成而死掉，会发生什么情况。使用我们当前的代码，RabbitMQ一旦将消息传递给消费者，便立即将其标记为删除。在这种情况下，如果您杀死一个work，我们将丢失正在处理的消息。我们还将丢失所有发送给该特定工作人员但尚未处理的消息。

答：为了确保消息永不丢失，RabbitMQ支持 [消息](https://www.rabbitmq.com/confirms.html)[确认](https://www.rabbitmq.com/confirms.html)。消费者发送回一个确认（acknowledgement）以告知RabbitMQ已经接收，处理了特定的消息，然后RabbitMQ会去删除这条消息，如果消费者在不发送确认的情况下死亡（其通道已关闭，连接已关闭或TCP连接丢失），RabbitMQ将了解消息未得到充分处理，并将重新排队。如果同时有其他消费者在线，它将很快将其重新分发给另一个消费者。这样，您可以确保即使工人偶尔死亡也不会丢失任何消息

 

**问题2:**开启了消息持久化，消息就一定不会丢失吗？

答： 将消息标记为持久性并不能完全保证不会丢失消息。尽管它告诉RabbitMQ将消息保存到磁盘，但是仍有很短的时间RabbitMQ接受了消息但尚未将其保存。另外，RabbitMQ不会对每条消息都执行fsync（2）－它可能只是保存到缓存中，而没有真正写入磁盘。持久性保证并不强，但是对于我们的简单任务队列而言，这已经绰绰有余了。如果您需要更强有力的保证，则可以使用 [发布者确认](https://www.rabbitmq.com/confirms.html)。

　　在这里补充一个知识：如果mq开启了持久化以后， 生产者把消息推给消息队列， 消息队列会复制一份消息到持久化队列，然后有新线程从持久化队列中把消息持久化到磁盘中。

 

**问题3:** 两个消费者同时消费一个队列，是队列分发消息还是消费者去取消息？

答：您可能已经注意到，调度仍然无法完全按照我们的要求进行。例如，在有两名工人的情况下，当有的消息都很重，有的消息消息很轻时，一位工人将一直忙碌而另一位工人将几乎不做任何工作。好吧，RabbitMQ对此一无所知，并且仍将平均分配消息。

发生这种情况是因为RabbitMQ在消息进入队列时才调度消息。它不会查看使用者的未确认消息数。它只是盲目地将每第n条消息发送给第n个使用者。

为了解决这个问题，我们可以将Channel＃basic_qos通道方法与 prefetch_count = 1设置一起使用。这使用basic.qos协议方法来告诉RabbitMQ一次不向工作人员发送多条消息。换句话说，在处理并确认上一条消息之前，不要将新消息发送给工作人员。而是将其分派给不忙的下一个工作程序。

　　这里补充一个知识：当两个消费者同时订阅了同一个队列时，队列会把消息循环平均分配给两个消费者。

**问题4:** 关于队列大小的注意事项，如果队列满了怎么处理？

答：如果队列满了以后，我们一方面可以增加消费者的数量，很浅显消费者越多消费消息就越快，还有一个是设置消息的过期时间来控制。

　　这里补充个知识： 当消息过期或者被消费者拒绝并且设置不返回队列中，这消息将加入死信队列。



### 7.3 接收消息应答详解

> mqServer->消费者 的应答有三种,   None, MANUAL模式 (手动模式) , Auto模式
>
> yaml中配置对所有队列都生效
>
> acknowledge-mode=none
> acknowledge-mode=auto
> acknowledge-mode=manual

手动应答方法基础方法
相应的，使用手动应答时，需要把autoAck属性设置为false，然后进行手动应答。
消息手动应答 有如下几个方法

+ Channel.basicAck(用于肯定确认) RabbitMQ已知道该消息并且成功的处理消息， 可以将其丢弃了
+ Channel.basicNack(用于否定确认)
+ Channel.basicReject(用于否定确认) 与Channel.basicNack相比少一个参数 不处理该消息了直接拒绝，可以将其丢弃了  手动应答时还有一个参数：Multiple 是否批量处理，一般选择false ，

#### NONE模式

默认推送的所有消息都已经消费成功，会不断地向消费端推送消息。消息丢失的风险 (其实我觉得,这个才是最像 "rabbitmq原生自动应答模式"的配置)

注意:NONE模式下 "spring.rabbitmq.listener.simple.default-requeue-rejected=true //重新入队" 这个配置不会生效 , 因为这种模式下 默认发完就算成功,不存在所谓"rejected"



#### MANUAL模式 (手动模式)

**注意 : channel.basicReject(message.getMessageProperties().getDeliveryTag(),true); 的第二个参数是requeue , 这个参数跟 "spring.rabbitmq.listener.simple.default-requeue-rejected=true " 有重义 , 而却是方法的优先级高于配置的 . 有没有大佬能系统解释下这块的 .** 

``` java
@Component
@RabbitListener(queues = "hello")
public class HelloReceiver {
    @RabbitHandler
    public void process(String hello,Channel channel, Message message) throws IOException {
        try {
            //告诉服务器收到这条消息 已经被我消费了 可以在队列删掉 这样以后就不会再发了 否则消息服务器以为这条消息没处理掉 后续还会在发
            channel.basicAck(message.getMessageProperties().getDeliveryTag(),false);
        } catch (IOException e) {
            e.printStackTrace();
            //丢弃这条消息 或者 进入死信队列
　　　　　　channel.basicReject(message.getMessageProperties().getDeliveryTag(),true);
　　　　　　　//channel.basicNack(message.getMessageProperties().getDeliveryTag(), false,false);
　　　　} 
　　} 
}
```


#### AUTO模式

看上去应该是自动应答, 其实跟rabbitmq本身的自动应答 不太一样,下面看分析

该方式是通过抛出异常的类型，来做响应的处理

spring boot 配置 

```properties
spring.rabbitmq.listener.simple.acknowledge-mode=auto
spring.rabbitmq.listener.simple.default-requeue-rejected=true # 重新入队
#  或者 
spring.rabbitmq.listener.direct.acknowledge-mode=auto
spring.rabbitmq.listener.direct.default-requeue-rejected=true # 重新入队
```

这样配置后 是不是就是自动应答了呢? 跟rabbitmq自己的自动应答有什么区别吗?
上述配置后, 有以下效果

1. 消息成功被消费，没有抛出异常，则自动确认。
2. 当抛出 `ImmediateAcknowledgeAmqpException` 异常，则视为成功消费，确认该消息。
3. 抛出 `AmqpRejectAndDontRequeueException` 异常的时候，则消息会被拒绝，且 `requeue = false`(该异常会在重试超过限制后抛出)
4. 其他的异常，则消息会被拒绝，且 `requeue = true`

根据以上几种效果看, 我觉得跟rabbitmq原生的自动应答还是有区别的, 　　　　　　　　
rabbitmq原生的自动应答 ,只要消息发送给消费者(即便消费者内部有问题), 就会收到应答,删除消息, 并没有重新入队这个操作(我个人没发现原生rabbitmq的自动应答有).　　　　 
但是spring boot集成rabbitmq之后,只要配置"default-requeue-rejected=true"　　　　　　　　 
遇到异常后(除了AmqpRejectAndDontRequeueException),就可以重新入队.



### 7.4 发送消息应答详解

- 通过实现 ConfirmCallback 接口，消息发送到 交换机 后触发回调，确认消息是否到达 mqServer 服务器，也就是只确认是否正确到达 Exchange 中

- 通过实现 ReturnCallback 接口，启动消息失败返回，比如路由不到队列时触发回调



#### 前期配置

在rabbitmq-provider项目的application.yml文件上加上一些配置

```yaml
server:
  port: 8021
spring:
  #给项目来个名字
  application:
    name: rabbitmq-provider
  #配置rabbitMq 服务器
  rabbitmq:
    host: 127.0.0.1
    port: 5672
    username: guest
    password: guest
#    connection-timeout: 60s
    #虚拟host 可以不设置,使用server默认host
    #virtual-host: admin
    # 确认消息已发送到交换机（Exchange）
    publisher-confirm-type: correlated
    # 确认消息已发送到队列
    publisher-returns: true
```

创建RabbitConfig配置类，配置消息的相关回调函数

```java
@Configuration
public class RabbitConfig {
    @Bean
    DirectExchange lonelyDirectExchange() {
        return new DirectExchange("lonelyDirectExchange");
    }
    @Bean
    public RabbitTemplate createRabbitTemplate(ConnectionFactory connectionFactory){
        RabbitTemplate rabbitTemplate = new RabbitTemplate();
        rabbitTemplate.setConnectionFactory(connectionFactory);
        // 设置开启Mandatory，才能触发回调函数，无论消息推送结果怎么样都强制调用回调函数
        rabbitTemplate.setMandatory(true);
        rabbitTemplate.setConfirmCallback(new RabbitTemplate.ConfirmCallback() {
            @Override
            public void confirm(CorrelationData correlationData, boolean b, String s) {
                System.out.println("ConfirmCallback    ：相关数据：" + correlationData);
                System.out.println("ConfirmCallback    ：确认情况：" + b);
                System.out.println("ConfirmCallback    ：原因：" + s);
            }
        });
        rabbitTemplate.setReturnCallback(new RabbitTemplate.ReturnCallback() {
            @Override
            public void returnedMessage(Message message, int i, String s, String s1, String s2) {
                System.out.println("ReturnCallback    ：消息：" + message);
                System.out.println("ReturnCallback    ：回应码：" + i);
                System.out.println("ReturnCallback    ：回应消息：" + s);
                System.out.println("ReturnCallback    ：交换机：" + s1);
                System.out.println("ReturnCallback    ：路由键：" + s2);
            }
        });
        return rabbitTemplate;
    }
}
```



#### 测试两种应答

生产者回调函数已经配置完毕，上面我们配置了两个回调函数setConfirmCallback和setReturnCallback

一般会产生四种情况：

①消息推送到server，但是在server里找不到交换机
②消息推送到server，找到交换机了，但是没找到队列
③消息推送到sever，交换机和队列啥都没找到
④消息推送成功

接下来分别测试和认证下以上4种情况，消息确认触发回调函数的情况：

##### 消息推送到server，但是在server里找不到交换机

写个测试接口，把消息推送到名为‘non-existent-exchange'的交换机上（这个交换机是没有创建没有配置的）：

```java
@GetMapping("/testMessageAck")
    public String testMessageAck() {
        String messageId = String.valueOf(UUID.randomUUID());
        String messageData = "message: non-existent-exchange test message ";
        String createTime = LocalDateTime.now().format(DateTimeFormatter.ofPattern("yyyy-MM-dd HH:mm:ss"));
        Map<String, Object> map = new HashMap<>();
        map.put("messageId", messageId);
        map.put("messageData", messageData);
        map.put("createTime", createTime);
        rabbitTemplate.convertAndSend("non-existent-exchange", "TestDirectRouting", map);
        return "ok";
    }
```

调用接口，查看rabbitmq-provuder项目的控制台输出情况（原因里面有说，没有找到交换机'non-existent-exchange'）：

![img](https://img.jbzj.com/file_images/article/202108/2021083114061951.png)

所以证明在交换器没有配置，推送消息找不到对应的交换器，会执行ConfirmCallback方法。

##### 消息推送到server，找到交换机了，但是没找到队列

我们在RabbitConfig里面配置了一个名为lonelyDirectExchange的交换器，再新增一个接口testMessageAck2，使用lonelyDirectExchange交换器

```java
@GetMapping("/testMessageAck2")
    public String testMessageAck2() {
        String messageId = String.valueOf(UUID.randomUUID());
        String messageData = "message: non-existent-exchange test message ";
        String createTime = LocalDateTime.now().format(DateTimeFormatter.ofPattern("yyyy-MM-dd HH:mm:ss"));
        Map<String, Object> map = new HashMap<>();
        map.put("messageId", messageId);
        map.put("messageData", messageData);
        map.put("createTime", createTime);
        rabbitTemplate.convertAndSend("lonelyDirectExchange", "TestDirectRouting", map);
        return "ok";
    }
```

重启服务，使用postman调用这个接口

![img](https://img.jbzj.com/file_images/article/202108/2021083114062052.png)

可以看到ReturnCallback和ConfirmCallback这两个都被调用了。

CallfirmCallback的确认情况为true，因为消息是推送成功到服务器了的，所以ConfirmCallback对消息确认情况是true

ReturnCallback的回应消息为NO_ROUTE找不到队列

##### 消息推送到sever，交换机和队列啥都没找到

这种情况和1、2很相似，会调用ReturnCallback和CallfirmCallback这两个回调函数

##### 消息推送成功

我们调用之前的sendFanoutMessage接口来看一下会是什么情况

![img](https://img.jbzj.com/file_images/article/202108/2021083114062053.png)

结论：消息推送成功调用ConfirmCallback回调函数



## 08_交换机队列配置参数



### 8.1 交换机

1. 交换机类型: fanout、direct、topic、headers

2. **Durability**： 持久化特性 （transient 临时的， durable 持久化的）， 默认为持久化

3. **Auto Delete** : 是否自动删除， 默认为false
       true：当没有消费者连接时，队列会被删除，期间生产者发送到队列的消息会丢失
       false：没有消费者连接时，队列不会被删除，期间生产者发送到队列的消息不会丢失

4. **internal**：设置是否是RabbitMQ内部使用，默认false。如果设置为 true ，　　则表示是内置的交换器，客户端程序无法直接发送消息到这个交换器中，只能通过交换器路由到交换器这种方式。

5. **Arguments** ：表示其他自定义参数。

   1. **alternate-exchange**

      如果无法通过其他方式路由到此交换的消息，请将它们发送到此处指定的备用交换。（设置“替代交换”参数。）

      



### 8.2 队列

1. **Durability**

   持久化特性 （transient 临时的， durable 持久化的）， 默认为持久化

2. **Auto Delete**

    是否自动删除， 默认为false
    true：当没有消费者连接时，队列会被删除，期间生产者发送到队列的消息会丢失
     false：没有消费者连接时，队列不会被删除，期间生产者发送到队列的消息不会丢失

3. **Arguments** 

   表示其他自定义参数。

   1. **alternate-exchange**

      如果无法通过其他方式路由到此交换的消息，请将它们发送到此处指定的备用交换。（设置“替代交换”参数。）

   2. **x-message-ttl**
      消息的最大存活时间，单位毫秒， 当超过时间后消息会被丢弃   默认消息存活时间为永久存在

   3. **x-expires**
      队列过期时间，当auto delete设置为true时，才会生效

   4. **x-max-length**
      队列存放最大就绪消息数量，超过限制时，从头部丢弃消息   默认最大数量限制与操作系统有关。

   5. **x-max-length-bytes**
      队列存放的所有消息总大小，超过限制时，从头部丢弃消息

   6. **x-overflow**
      消息超出最大数量时，溢出行为： drop-head 或 reject-publish
      （drop-head:头部丢弃， reject-publish拒绝生产者发布消息）

   7. **x-dead-letter-exchange**
      当队列满时，被拒绝的消息，或者消息过期时，将被重新发布到死信交换机上。

   8. **x-dead-letter-routing-key**
          消息被发布到死信交换机时，如果没设置这个路由键，则将使用消息的原始路由键，
          比如，消息发送到
          exchange=contract.exchange
          路由键 routeKey = contract.info
          当该队列满时，如果 x-dead-letter-exchange=contract.dead.exchange
          没有指定x-dead-letter-routing-key时，会将消息发送到队列为
          exchange=contract.dead.exchange
          routeKey = contract.info
          消息的原始路由键即时，消息原本要发送到的队列绑定路由键名

   9. **x-max-priority**
      队列支持的消息最大优先级数，没设置时，队列不支持消息优先级

   10. **x-queue-mode**：lazy
          将队列设置为惰性模式，将消息保存在磁盘上，如果没设置，将保存内存缓存上以尽快传递消息

   11. **x-queue-master-locator**
       将队列设置为 master 位置模式，确定队列 master 在节点集群上声明时的定位规则。

4. **exclusive** 排他队列
   当exclusive = true 时，
   只针对首次申明它的 connection 连接可见。并且在连接断开时自动删除。
   当前 connection下的 channel 信道都可以访问。
   其他连接无法再申明相同名称的队列。



