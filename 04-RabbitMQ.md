# RabbitMQ笔记

## 安装

### 1 安装gcc和socat

在线安装依赖环境：

```shell
yum install gcc

yum install socat
```

### 2 上传erlang和rabbitmq的rpm安装包

~~~bash
rpm -ivh erlang-22.0.7-1.el7.x86_64.rpm

rpm -ivh rabbitmq-server-3.7.17-1.el7.noarch.rpm
~~~

### 3 修改配置文件，开启管理界面

~~~bash
cp rabbitmq.config.example /etc/rabbitmq/rabbitmq.config

# 修改配置文件
vi /etc/rabbitmq/rabbitmq.config

# 开启管理界面
rabbitmq-plugins enable rabbitmq_management
~~~

![rabbitconfig配置文件](.\image\rabbitconfig配置文件.png)



### 4 启动rabbitmq

~~~bash
# 查看状态
systemctl status rabbitmq-server

# centos7用这个命令：
systemctl start rabbitmq-server

# 重启
systemctl restart rabbitmq-server

# 关闭
systemctl stop rabbitmq-server
~~~

## 基本知识

### 1 AMQP协议模型



![AMQP协议图解](.\image\AMQP协议图解.png)



### 2 RabbitMQ模型

![RabbitMQ模型](.\image\RabbitMQ模型.png)

### 3 常用交换机

RabbitMQ常用的交换器类型有==direct==、==topic==、==fanout==、headers四种。

==Direct Exchange==

该类型的交换器将所有发送到该交换器的消息被转发到RoutingKey指定的队列中，也就是说路由到BindingKey和RoutingKey完全匹配的队列中。

![DirectExchange](.\image\DirectExchange.png)

==Topic Exchange==

该类型的交换器将所有发送到Topic Exchange的消息被转发到所有RoutingKey中指定的Topic的队列上面。

Exchange将RoutingKey和某Topic进行模糊匹配，其中“\*” 用来匹配一个词，“#”用于匹配一个或者多个词。例如“com.#”能匹配到“com.rabbitmq.oa”和“com.rabbitmq”；而"login.*"只能匹配到“com.rabbitmq”。

![TopicExchange](.\image\TopicExchange.png)

==Fanout Exchange==

该类型不处理路由键，会把所有发送到交换器的消息路由到所有绑定的队列中。优点是转发消息最快，性能最好。

![FanoutExchange](.\image\FanoutExchange.png)





















