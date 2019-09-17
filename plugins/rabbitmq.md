# Part1 RabbitMQ简介
## 什么是消息中间件
* 消息中间件（Message Queue Middleware，简称MQ）是指利用高效可靠的消息传递机制进行与平台无关的数据交流，并基于数据通信来进行分布式系统的集成。

## 消息中间件的作用
* 解耦
* 冗余（存储）
* 扩张性
* 销峰
* 可恢复性
* 顺序保证
* 缓冲
* 异步通信

## RabbitMQ的起源

## RabbitMQ的安装

# Part3客户端开发向导
## 连接RabbitMQ
* Channel或者Connection中有个isOpen的方法用来检测是否已经处于开启状态，这个方法的返回值依赖于shutdownCause，有可能

# Part4 RabbitMQ进阶
## mandatory/immediate参数，备份交换器
* mandatory和immediate是channel.basicPublish方法中的两个参数，它们都有当消息传递过程中不可达目的地时将消息返回给生产者的功能。
* RabbitMQ提供的备份交换器(Alternate Exchange)可以将未能被交换器路由的消息（没有绑定队列或者没有匹配的绑定）存储起来，而不用返回给客户端。

### mandatory
* 当mandatory参数设为true时，交换器无法根据自身的类型和路由键找到一个符合条件的队列，那么RabbitMQ会调用Basic.Return命令将消息返回给生产者；为false时，如果出现上述情况，则消息直接被丢弃。那么生产者如何获取到没有被正确路由到合适队列的消息呢?这时候可以通过调用
channel addReturnListener 来添加 ReturnListener 监昕器实现。

# RabbitMQ消息的持久化
https://www.cnblogs.com/bigberg/p/8195622.html
## 什么情况下会造成消息丢失？
* 突然断电

## 哪些地方需要持久化
```java
// 队列的持久化 durable=true
channel.queueDeclare(QUEUE_NAME, true, false, false, null);
String msg = "Hello World! 你好世界！";
// 消息的持久化 MessageProperties.PERSISTENT_TEXT_PLAIN
channel.basicPublish("", QUEUE_NAME, MessageProperties.PERSISTENT_TEXT_PLAIN, msg.getBytes());
```

## 消息过载
* Producer生产的message的速度和Consumer消费的message的速度不匹配

## 死信队列（DLX）
### 什么是死信队列
DLX（dead-letter-exchange） 当消息在一个队列中变成死信之后，它会被重新publish到另外一个exchange中，这个exchange就是DLX.
### 死信队列产生的场景
* 消息被拒绝(basic.reject / basic.nack),并且requeue = false
* 消息因TTL过期
* 队列达到最大长度

## 常用命令

* List Queue
``` shell
rabbitmqctl list_queues name messages messages_ready \ messages_unacknowledged
```

* Find Queue
```
rabbitmqctl list_queues| grep hello | awk '{print $1}'
```

* Delete Queue
```
rabbitmqctl  delete_queue queueName
```