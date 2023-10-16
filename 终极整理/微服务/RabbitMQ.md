#### 概念

```
“消息队列”是在消息的传输过程中保存消息的容器。它是典型的：生产者、消费者模型。生产者不断向消息队列中生产消息，消费者不断的从队列中获取消息。因为消息的生产和消费都是异步的，而且只关心消息的发送和接收，没有业务逻辑的侵入，这样就实现了生产者和消费者的解耦。
是一个开源的消息代理和队列服务器，用来通过普通协议在完全不同的应用之间共享数据，RabbitMQ是使用Erlang(高并发语言)语言来编写的，并且RabbitMQ是基于AMQP协议的。 
AMQP 高级消息队列协议
```

##### 关键词

```
Service(broker) 服务 接受客户端的链接
Connection 连接 应用程序与borker的网络连接
Channel 信道
Message 消息
Virtualhost 虚拟主机
Exchange 交换机 接收消息 根据路由转单消息分配给队列
Binding 交换机与队列间的虚拟链接
Routing key 路由规则

生产Producing
队列Queue
消费Consuming
```

##### 场景

```
异步处理
应用解耦
流量消锋
日志处理
消息通讯
```

#### 环境配置

##### 安装

```
erlang https://www.erlang.org/downloads
rabbitmq  https://www.rabbitmq.com/install-windows.html
注意两者版本匹配
```

##### rabbit配置

```
管理员运行cmd到rabbitmq sbin目录下
rabbitmq-plugins enable rabbitmq_management  
net stop RabbitMQ
net start RabbitMQ
rabbitmqctl status  查看状态
```

```
rabbitmqctl  add_user  JC JayChou   //创建用户JC密码为JayChou
rabbitmqctl  set_permissions  JC ".*"  ".*"  ".*"    //赋予JC读写所有消息队列的权限
rabbitmqctl  set_user_tags JC administrator    //分配用户组
rabbitmqctl change_password JC  123//修改JC密码为123：
rabbitmqctl delete_user  JC//删除用户JC：

可通过http://127.0.0.1:15672/访问
```

##### 角色权限

```
1、超级管理员(administrator)
可登陆管理控制台，可查看所有的信息，并且可以对用户，策略(policy)进行操作。
2、监控者(monitoring)
可登陆管理控制台，同时可以查看rabbitmq节点的相关信息(进程数，内存使用情况，磁盘使用情况等)
3、策略制定者(policymaker)
可登陆管理控制台, 同时可以对policy进行管理。但无法查看节点的相关信息(上图红框标识的部分)。
4、普通管理者(management)
仅可登陆管理控制台，无法看到节点信息，也无法对策略进行管理。
5、其他
无法登陆管理控制台，通常就是普通的生产者和消费者。
```

##### 添加虚拟主机Virtual Host

#### 工作模式

##### 简单模式

```
一个生产者 一个消息队列 一个消费者
聊天
```

##### 资源竞争模式

```
多个消费者 按顺序分发 平均分配
红包,资源调度
```

##### 共享资源模式

```
生产者通过交换机将资源同时分给不同消息队列
邮件群发，群广告
```

##### 路由模式direct

```
交换机根据消息携带的路由信息将信息分配给不同消息队列
```

##### 主题模式(路由模式的一种)topic

```
根据模糊匹配进行消息分发
```

##### RPC

```
一个消息队列 一个回调队列 
消息处理完成通过回调队列发送给生产者
```

#### 代码实现

```
https://www.cnblogs.com/wei325/p/15174212.html
```

