# 简单入门Spring AMQP

<font size = 5 color = gray>参考：https://docs.spring.io/spring-amqp/docs/2.4.10/reference/html/</font>

>**Features**<br>**Listener**: container for asynchronous processing of inbound messages<br>**RabbitTemplate**: for sending and receiving messages<br>**RabbitAdmin**: for automatically declaring queues, exchanges and bindings

<h2>1、快速构建应用</h2>

<h3>1.1、依赖</h3>

```xml
<dependency>
  <groupId>org.springframework.amqp</groupId>
  <artifactId>spring-rabbit</artifactId>
  <version>2.4.10</version>
</dependency>
```

<h3>1.2、Send and receive a message</h3>

```java
package com.ifi.vote.amqp;
import org.springframework.amqp.rabbit.connection.ConnectionFactory;
import org.springframework.amqp.core.AmqpAdmin;
import org.springframework.amqp.core.AmqpTemplate;
import org.springframework.amqp.core.Queue;
import org.springframework.amqp.rabbit.connection.CachingConnectionFactory;
import org.springframework.amqp.rabbit.core.RabbitAdmin;
import org.springframework.amqp.rabbit.core.RabbitTemplate;
public class AmqpTest {
    public static void main(String[] args) {
        ConnectionFactory connectionFactory = new CachingConnectionFactory();
        AmqpAdmin admin = new RabbitAdmin(connectionFactory);
        admin.declareQueue(new Queue("myqueue"));
        AmqpTemplate template = new RabbitTemplate(connectionFactory);
        template.convertAndSend("myqueue", "foo");
        String foo = (String) template.receiveAndConvert("myqueue");
        System.out.println(foo);
    }
}
```

```console
11:38:45.581 [main] DEBUG org.springframework.amqp.rabbit.connection.CachingConnectionFactory - Using hostname [Arvinw] for hostname.
11:38:46.031 [main] INFO org.springframework.amqp.rabbit.connection.CachingConnectionFactory - Attempting to connect to: Arvinw:5672
11:38:46.069 [main] INFO org.springframework.amqp.rabbit.connection.CachingConnectionFactory - Created new connection: SpringAMQP#14acaea5:0/SimpleConnection@3159c4b8 [delegate=amqp://guest@192.168.1.168:5672/, localPort= 58322]
11:38:46.074 [main] DEBUG org.springframework.amqp.rabbit.connection.CachingConnectionFactory - Creating cached Rabbit Channel from AMQChannel(amqp://guest@192.168.1.168:5672/,1)
11:38:46.089 [main] DEBUG org.springframework.amqp.rabbit.core.RabbitTemplate - Executing callback RabbitAdmin$$Lambda$13/1551870003 on RabbitMQ Channel: Cached Rabbit Channel: AMQChannel(amqp://guest@192.168.1.168:5672/,1), conn: Proxy@73846619 Shared Rabbit Connection: SimpleConnection@3159c4b8 [delegate=amqp://guest@192.168.1.168:5672/, localPort= 58322]
11:38:46.089 [main] DEBUG org.springframework.amqp.rabbit.core.RabbitAdmin - declaring Queue 'myqueue'
11:38:46.094 [main] DEBUG org.springframework.amqp.rabbit.core.RabbitTemplate - Executing callback RabbitTemplate$$Lambda$18/1735934726 on RabbitMQ Channel: Cached Rabbit Channel: AMQChannel(amqp://guest@192.168.1.168:5672/,1), conn: Proxy@73846619 Shared Rabbit Connection: SimpleConnection@3159c4b8 [delegate=amqp://guest@192.168.1.168:5672/, localPort= 58322]
11:38:46.095 [main] DEBUG org.springframework.amqp.rabbit.core.RabbitTemplate - Publishing message [(Body:'foo' MessageProperties [headers={}, contentType=text/plain, contentEncoding=UTF-8, contentLength=3, deliveryMode=PERSISTENT, priority=0, deliveryTag=0])] on exchange [], routingKey = [myqueue]
11:38:46.097 [main] DEBUG org.springframework.amqp.rabbit.core.RabbitTemplate - Executing callback RabbitTemplate$$Lambda$19/2006034581 on RabbitMQ Channel: Cached Rabbit Channel: AMQChannel(amqp://guest@192.168.1.168:5672/,1), conn: Proxy@73846619 Shared Rabbit Connection: SimpleConnection@3159c4b8 [delegate=amqp://guest@192.168.1.168:5672/, localPort= 58322]
11:38:46.099 [main] DEBUG org.springframework.amqp.rabbit.core.RabbitTemplate - Received: (Body:'foo' MessageProperties [headers={}, contentType=text/plain, contentEncoding=UTF-8, contentLength=0, receivedDeliveryMode=PERSISTENT, priority=0, redelivered=false, receivedExchange=, receivedRoutingKey=myqueue, deliveryTag=1, messageCount=0])
foo
```

<h3>1.3、With XML Configuration</h3>

>一个简单例子展示xml中配置

```java
ApplicationContext context =
    new GenericXmlApplicationContext("classpath:/rabbit-context.xml");
AmqpTemplate template = context.getBean(AmqpTemplate.class);
template.convertAndSend("myqueue", "foo");
String foo = (String) template.receiveAndConvert("myqueue");
```

```xml
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:rabbit="http://www.springframework.org/schema/rabbit"
       xsi:schemaLocation="http://www.springframework.org/schema/rabbit
           https://www.springframework.org/schema/rabbit/spring-rabbit.xsd
           http://www.springframework.org/schema/beans
           https://www.springframework.org/schema/beans/spring-beans.xsd">

    <rabbit:connection-factory id="connectionFactory"/>

    <rabbit:template id="amqpTemplate" connection-factory="connectionFactory"/>

    <rabbit:admin connection-factory="connectionFactory"/>

    <rabbit:queue name="myqueue"/>

</beans>
```

>**note:**<br>By default, the <rabbit:admin/> declaration automatically looks for beans of type Queue, Exchange, and Binding and declares them to the broker on behalf of the user. 

<h3>1.4、With Java Configuration</h3>

```java
ApplicationContext context =
    new AnnotationConfigApplicationContext(RabbitConfiguration.class);
AmqpTemplate template = context.getBean(AmqpTemplate.class);
template.convertAndSend("myqueue", "foo");
String foo = (String) template.receiveAndConvert("myqueue");

........

@Configuration
public class RabbitConfiguration {

    @Bean
    public CachingConnectionFactory connectionFactory() {
        return new CachingConnectionFactory("localhost");
    }

    @Bean
    public RabbitAdmin amqpAdmin() {
        return new RabbitAdmin(connectionFactory());
    }

    @Bean
    public RabbitTemplate rabbitTemplate() {
        return new RabbitTemplate(connectionFactory());
    }

    @Bean
    public Queue myQueue() {
       return new Queue("myqueue");
    }
}
```

<h3>1.5、With Spring Boot Auto Configuration and an Async POJO Listener</h3>

```java
@SpringBootApplication
public class Application {

    public static void main(String[] args) {
        SpringApplication.run(Application.class, args);
    }

    @Bean
    public ApplicationRunner runner(AmqpTemplate template) {
        return args -> template.convertAndSend("myqueue", "foo");
    }

    @Bean
    public Queue myQueue() {
        return new Queue("myqueue");
    }

    @RabbitListener(queues = "myqueue")
    public void listen(String in) {
        System.out.println(in);
    }

}
```