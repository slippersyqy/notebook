# 服务器

## IP

> 内网IP

192.168.0.11	预发服务器

192.168.0.6	正式服务器

192.168.30.183	离线部署

192.168.0.16	arm64服务器 root Sailfish020

> 外网IP

120.79.180.136	==一般情况下不改动==

## 端口

> 常用端口号

- 18080/swagger-ui.html swagger测试使用
- 8888 宝塔
- 8889 partainer.io，docker的容器管理
- 9000、9001、9002、9003 minios，文件服务器管理
- 15672 RabbitMQ管理
- 8380 授权端口：admin Ntko\$123\$890
- 19099 前端页面

> 其它

- 192.168.0.20上的5000端口，qiboshi-nas文件上传下载，可以下载tar包



# 线上部署

> 进入项目目录

~~~bash
cd /opt/meeting/
~~~

> 需要先关闭docker

~~~bash
docker-compose down
~~~

> 修改配置文件

~~~bash
vi docker-compose.yml
~~~

> 修改image，在codeup上的流水线上的镜像日志中查看最新的image

~~~yaml
image: regist....域名反+项目名+文件名+日期序号
~~~

> 启动docker

~~~bash
docker-compose up -d # -d后台运行
~~~

> 其它命令

~~~bash
docker images
docker ps
# 以上可查看image
~~~

> 一些其它命令：

~~~bash
# 保存tar
docker save -o 目标路径+名 regis..

# 加载tar
docker load -i ...tar
~~~

> docker 登录

~~~bash
docker login --username=haitukeiotserver registry.cn-shenzhen.aliyuncs.com
~~~



# 离线部署

1. 使用向日葵远程连接，Moba连接服务器

2. 进入原项目路径meeting-ws目录关闭docker

3. 若无法使用Root权限直接登录，则使用sailfish登录，将压缩包放在sailfish文件夹下，再切换到root用户
4. 将压缩包移至home目录下，解压到home目录下，压缩包内里面有各类安装包，实际运行文件依然在opt文件夹下
5. 进入解压后的目录，运行install.sh文件，输入服务器ip回车
6. 若原来有docker版本冲突，可使用`apt-get remove docker-compose`卸载原docker，注意==需要先将原来的dockered文件进行备份==
7. 若宝塔未启动成功，可进入meeting目录下，`koctl restart`重新启动，还可使用浏览器进入8889端口，进入docker容器管理可视化页，账号密码为sailfish
8. 其他问题：可进入8888端口的宝塔页，管理数据库和nginx和mysql。



# Docker安装

> CentOS7上

~~~bash
curl -fsSL https://get.docker.com | bash -s docker --mirror Aliyun
# 或者
curl -sSL https://get.daocloud.io/docker | sh
~~~









# Docker使用

## 基本使用

> 查看docker的版本

~~~bash
docker --version
~~~

> 查找镜像

~~~bash
docker search nginx # 查找nginx
~~~

> 拉取镜像

~~~bash
docker pull nginx
~~~

> 使用镜像，运行容器

~~~bash
# -p 容器端口 ： 宿主机端口 
docker run -p 80:80 --name mynginx -v $PWD/www:/www -v $PWD/conf/nginx.conf:/etc/nginx/nginx.conf -v $PWD/logs:/wwwlogs  -d nginx
~~~

命令说明：

- -p 80:80：将容器的80端口映射到主机的80端口
- --name mynginx：将容器命名为mynginx
- -v $PWD/www:/www：将主机中当前目录下的www挂载到容器的/www
- -v $PWD/conf/nginx.conf:/etc/nginx/nginx.conf：将主机中当前目录下的nginx.conf挂载到容器的/etc/nginx/nginx.conf
- -v $PWD/logs:/wwwlogs：将主机中当前目录下的logs挂载到容器的/wwwlogs



> 也可通过Dockerfile安装

先在当前目录下建立Dockerfile文件，再使用`docker build -t tomcat .`



> 进入容器

~~~bash
# 先使用docker ps  | grep 查看id
docker exec -it 容器ID /bin/bash
~~~

> 从容器复制文件至宿主机（在宿主机下）

~~~bash
docker cp  容器ID:容器文件名
~~~

> 构建镜像地址？

~~~bash
docker build /home/meeting-deploy/me..arm64 -t registry.cn...
~~~

> 查看所有docker进程，包括非运行状态的

~~~bash
docker ps -a
~~~

> 删除进程残余，删除容器

~~~bash
docker rm docker进程id
~~~

> 删除镜像

~~~bash
docker rmi 镜像id
~~~

> 查看报错日志

~~~bash
docker logs -f  镜像id
~~~

> 停止正在运行的容器

~~~bash
docker stop 容器ID
~~~





> 清理所有停止的容器

~~~bash
docker container prune
~~~









> 退出容器

~~~bash
exit
或者ctrl + d
~~~





## Dockerfile

**1.ARG指令**

定义创建镜像过程中使用的变量，相当于我们为docker build - -build-arg赋值。镜像编译结束后，这个变量将不会被保存

```dockerfile
ARG version=1.0
```

**2.FROM指令**
指定我们要在哪个image之上再进行构建，尽量使用官方image进行base image，为了安全。并且一个Dockerfile，必须要以From指令作为开头（ARG是唯一一个可以先于From命令的)

```dockerfile
FROM debian:latest
```

**3.LABLE指令**
像是代码里的注释一样，一些概括的维护者信息。

```dockerfile
LABLE author=harry
```

**4.ENV指令**
定义变量，可以在dockerfile下方进行使用，例如我们定义了 ENV USER harry，那么下面可以这样使用 "${USER}"

```dockerfile
ENV FILE_LOCATION /usr/local/file
```

**5.USER指令**
指定运行容器时的用户是谁

**6.WORKDIR指令**
进入到我们指定的目录中，==如果没有这个目录会自动进行创建==，用WORKDIR，代替 RUN cd。尽量使用绝对目录，不要使用相对目录。

```dockerfile
WORKDIR /usr/local
WORKDIR tomcat/config
# 可以连续指定路径，如果像上述一样，指定的路径为/usr/local/tomcat/configs
```

**7.RUN指令**
每执行一次RUN就是就会产生镜像的一层，使用 "&&" 将多个命令串联起来，如果需要换行在最后需要使用" " 反斜杠。环境的运行与搭建，大多数情况下需要这个命令

```dockerfile
RUN yum update \
        && yum install -y nginx
#上述操作先更新yum，然后下载nginx
```

**8.CMD指令**
设置启动后默认执行的命令和参数。如果docker run 进行了指定了命令，例如 docker run -it … /bin/bash。则不会运行CMD中的命令，而且CMD定义多个，后面会覆盖之前的。

```dockerfile
启动tomcat命令
CMD ['catalina.sh', 'run']
```

**9.ENTRYPOINT指令**
设置容器启动时默认执行的命令和参数，该命令会在启动容器后作为根命令执行，通过名称可以看出来是入口。让容器以应用程序或者服务去执行。并且ENTRYPOINT一定会执行

```dockerfile
将一个shell脚本作为docker启动的入口。
ENTRYPOINT ['/entry.sh']
# entrypoint 入口点
```

**10.COPY指令**
把本地文件拷贝到docker里去，==COPY指令优于ADD指令==，如果需要添加远程文件可以使用 curl或者wget

```dockerfile
COPY . /temp
```

**11.ADD指令**
是把本地的文件复制到docker里去，不过不光如此，还会对==压缩文件自动进行解压缩==

```dockerfile
ADD . /temp
```

**12.VOLUME指令**
启动容器时，可以在本地或者是其他容器创建数据卷挂载点，用于存放数据库和持久化数据

```dockerfile
#指定挂载点为 /temp/mount
VOLUME /temp/mount
```

**13.EXPOSE指令**
声明镜像内部服务监听的端口，一次可以暴露多个端口

```dockerfile
#暴露22端口，和8888端口
EXPOSE 22 8888
```

**14.ONBUILD指令**
指定自己的子镜像都会执行哪些命令

```dockerfile
#把当前目录下的所有东西拷贝到/app/src目录下
ONBUILD COPY . /app/src
```



​	



# docker-compose





















