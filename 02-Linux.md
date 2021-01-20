# Linux

## 基本操作

> 复制文件

~~~bash
cp -rf 原路径 复制后路径
~~~

- -r：若给出的代源文件是一个目录文件，此时将复制该目录下所有的子目录和文件。

- -f：覆盖已经存在的目标文件而不给出提示。



## 打包与解压

> 打包

~~~bash
# 压缩
tar -czvf ***.tar.gz
tar -cjvf ***.tar.bz2

tar -czvf meeting.tar.gz meeting

tar --exclude /home/test -zcvf myfile.tar.gz /home/* /etc * 忽略
~~~

> 解压

~~~bash
# 解压缩
tar -xzvf ***.tar.gz
tar -xjvf ***.tar.bz2

# 将test.tar.gz压缩包解压到tmp目录下
tar -xzvf test.tar.gz -C /tmp
~~~

> 参数介绍

**tar  [主选项+辅选项] 文件或目录**
主选项是必须要有的，它告诉tar要做什么事情。
辅选项是辅助使用的，可以选用。

tar常用命令：

主选项：
**-x**  从档案文件中**释放**文件。
**-c**  **创建**新的档案文件。如果用户想备份一个目录或是一些文件，就要选择这个选项。
**-r**   把要存档的文件追加到档案文件的末尾。例如用户已经做好备份文件，又发现还有一个目录或
     是一些文件忘 记备份了，这时可以使用该选项，将忘记的目录或文件追加到备份文件中。
**-t**    列出档案文件的内容，查看已经备份了哪些文件。
**-u**   更新文件。就是说，用新增的文件取代原备份文件，如果在备份文件中找不到要更新的文件，
     则把它追加到备份文件的最后。

-C --directory DIR   转到指定的目录

辅助选项：
**-j**     代表使用‘bzip2’程序进行文件的压缩   tar.bz2
**-z**    用gzip来压缩/解压缩文件，加上该选项后可以将档案文件进行压缩，但还原时也一定要使用该
      选项进行解压缩。  tar.gz
**-v**    详细报告tar处理的文件信息。如无此选项，tar不报告文件信息。
-b   该选项是为磁带机设定的，其后跟一数字，用来说明区块的大小，系统预设值为20（20×512 bytes）。
-f    使用档案文件或设备，这个选项通常是必选的。
-k    保存已经存在的文件。例如把某个文件还原，在还原的过程中遇到相同的文件，不会进行覆盖。
-m    在还原文件时，把所有文件的修改时间设定为。
-M   创建多卷的档案文件，以便在几个磁盘中存放。
-w      每一步都要求确认。

## 查看存储状态

> 查看全部

~~~bash
df -hl
~~~

> 查看当前 

~~~bash
# 必须设置深度
du -hl --max-depth=1
~~~







## 分配文件权限

> 添加运行权限

~~~bash
chmod +x 文件名
~~~

> 添加所有权限

```bash
chmod 777 文件名
```

使用-R则为文件夹下全部子文件

- r 读 4
- w 写 2
- x 执行 1

## 创建文件

> 创建文件夹

~~~bash
mkdir 文件名 # -p 则可创建多子文件夹
~~~

> 创建新文件

- `vim 文件名`
- `touch 文件名`
- `echo "文字" > 新文件`或`echo "新文字" >> 老文件`





## Vim编辑器使用

> 显示行号

使用`:set nu`显示行号，`set nonu`关闭行号

> 删除行

- 单行：在命令模式下使用`dd`删除光标所在行
- 多行：使用`:1,3d`删除第1行下的三行

> 复制一行

命令模式下`yy`然后在新行`p`

> 撤回

- 命令模式u 撤回所有本次操作
- ctrl r 恢复撤回的内容

> 查找

命令模式下`/查找内容` 使用`n`下一个  `N`上一个

> 查看，翻页相关

整页翻页 ctrl-f ctrl-b
	f就是forword b就是backward

翻半页
	ctrl-d ctlr-u
	d=down u=up

滚一行
	ctrl-e ctrl-y

zz 让光标所在的行居屏幕中央
zt 让光标所在的行居屏幕最上一行 t=top
zb 让光标所在的行居屏幕最下一行 b=bottom

## 终止程序

- Ctrl+c	在命令行下起着终止当前执行程序的作用，
- Ctrl+d	相当于exit命令，退出当前shell
- Ctrl+s	挂起当前shell（保护作用很明显哦）
- Ctrl+q	解冻挂起的shell再不行就重新连接打开一个终端，reboot linux 或 kill 相关进程

## 服务和防火墙

> 查看服务状态，启动服务，重启服务，和关闭服务

~~~bash
# 查看所有服务
systemctl list-units

# 查看状态
systemctl status rabbitmq-server

# centos7用这个命令：
systemctl start rabbitmq-server

# 重启
systemctl restart rabbitmq-server

# 关闭
systemctl stop rabbitmq-server

# sshd为服务名，不包含.service
systemctl enable sshd           ##设定指定服务开机开启

systemctl disable sshd          ##设定指定服务开机关闭
~~~

> 查看防火墙状态

~~~bash
systemctl status firewalld

# 从开机自启关闭
systemctl disable firewalld

# 关闭防火墙
systemctl stop firewalld
~~~









## 宝塔相关

> 修改宝塔密码

进入/www/server/panel/下，使用`python tools.py panel 密码`命令重置密码

> 关闭宝塔

~~~bash
/etc/int.d/bt stop
~~~

> 启动超时

```
export DOCKER_CLIENT_TIMEOUT=600
export COMPOSE_HTTP_TIMEOUT=600
```

## RPM软件管理

> rpm是什么

rpm命令是RPM软件包的管理工具。rpm原本是Red Hat Linux发行版专门用来管理Linux各项套件的程序，由于它遵循GPL规则且功能强大方便，因而广受欢迎。逐渐受到其他发行版的采用。RPM套件管理方式的出现，让Linux易于安装，升级，间接提升了Linux的适用度。

> 常用参数

- -a 　查询所有套件。

- -e<套件档>或--erase<套件档> 　删除指定的套件。
- -h或--hash 　套件安装时列出标记。

- -i 　显示套件的相关信息。

- -l 　显示套件的文件列表。
- -q 　使用询问模式，当遇到任何问题时，rpm指令会先询问用户。
- -s 　显示文件状态，本参数需配合"-l"参数使用。
- -v 　显示指令执行过程。
- --version 　显示版本信息。

> 常用命令

~~~bash
# rpm查找指定软件是否存在
rpm -qa | grep 软件名.rpm

# 查询软件信息
rpm -qi 软件名.rpm
~~~

## yum相关





## Shell相关

> 字符串拼接

~~~bash
echo "hello"`date`
~~~

> 格式化字符串

```bash
echo `date +'%Y-%m-%d'`
echo `date +'%Y-%m-%d-%H-%M-%S'`
```

- % Y 年（例如：1970，2018等） 
- % m 月（01..12）
- % d 一个月的第几天（01..31）
- % H 小时（00..23）
- % M 分（00..59）
- % S 秒（00..59）

> /bin/bash^M: 解释器错误: 没有那个文件或目录

报错原因：这个文件在Windows 下编辑过，在Windows下每一行结尾是\n\r，而Linux下则是\n，所以才会有 多出来的\r。

解决方法：使用指令`sed -i 's/\r$//' xxxxxxx.sh`，上面的指令会把在Windows下修改过的 ==xxxxxxx.sh== 中的\r 替换成空白！

`sed -i 's/被替换的内容/要替换成的内容/' file`

> seb对文本处理

Linux sed 命令是利用脚本来处理文本文件。

sed 可依照脚本的指令来处理、编辑文本文件。

Sed 主要用来自动编辑一个或多个文件、简化对文件的反复操作、编写转换程序等。

**参数说明**：

- -e<script>或--expression=<script> 以选项中指定的script来处理输入的文本文件。
- -f<script文件>或--file=<script文件> 以选项中指定的script文件来处理输入的文本文件。
- -h或--help 显示帮助。
- -n或--quiet或--silent 仅显示script处理后的结果。
- -V或--version 显示版本信息。

**动作说明**：

- a ：新增， a 的后面可以接字串，而这些字串会在新的一行出现(目前的下一行)～
- c ：取代， c 的后面可以接字串，这些字串可以取代 n1,n2 之间的行！
- d ：删除，因为是删除啊，所以 d 后面通常不接任何咚咚；
- i ：插入， i 的后面可以接字串，而这些字串会在新的一行出现(目前的上一行)；
- p ：打印，亦即将某个选择的数据印出。通常 p 会与参数 sed -n 一起运行～
- s ：取代，可以直接进行取代的工作哩！通常这个 s 的动作可以搭配正规表示法！例如 1,20s/old/new/g 就是啦！

```bash
sed -e 4a\newline testfile #使用sed 在第四行后添加新字符串

# 在第二行后(亦即是加在第三行)加上『drink tea?』字样
nl /etc/passwd | sed '2a drink tea'

# 删除第2行
nl /etc/passwd | sed '2d' 
# 要删除第 3 到最后一行
nl /etc/passwd | sed '3,$d' 

sed 's/要被取代的字串/新的字串/g'

# sed 的 -i 选项可以直接修改文件内容
# 利用 sed 直接在 regular_express.txt 最后一行加入 # This is a test:
sed -i '$a # This is a test' regular_express.txt

sed -i '24a This is testLine' Dockerfile
sed -i "24a This is testLine2" Dockerfile
```



























