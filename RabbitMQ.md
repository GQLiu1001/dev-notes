# RabbitMQ
## 架构
![image.png](https://raw.githubusercontent.com/GQLiu1001/mytc/master/img/20250312162645481.png)
- `publisher`：生产者，也就是发送消息的一方
- `consumer`：消费者，也就是消费消息的一方
- `queue`：**队列**，存储消息。生产者投递的消息会暂存在消息队列中，等待消费者处理
- `exchange`：**交换机**，负责消息路由。生产者发送的消息由交换机决定投递到哪个队列。
- `virtual host`：虚拟主机，起到数据隔离的作用。每个虚拟主机相互独立，有各自的exchange、queue
## 用户管理:
利用`virtual host`的隔离特性，将不同项目隔离
- 给每个项目创建独立的运维账号，将管理权限分离。
- 给每个项目创建不同的`virtual host`，将每个项目的数据隔离。
## SpringAMQP
SpringAmqp的官方地址：
[Spring AMQP](https://spring.io/projects/spring-amqp)
