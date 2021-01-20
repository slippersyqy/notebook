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

![image-20210114142902350](C:\Users\Administrator.MM-202008042005\AppData\Roaming\Typora\typora-user-images\image-20210114142902350.png)



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













