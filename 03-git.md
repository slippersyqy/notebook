# Git笔记

## 安装

> 测试是否有git

~~~bash
git
# 结果如下：
-bash: git: command not found
~~~

> 使用yum安装git

~~~bash
# 安装
yum install -y git

# 查看版本
git version
~~~

## 基本使用

> 配置用户名和邮箱

~~~bash
git config --global user.name "Your Name"
git config --global user.email "email@example.com"
~~~

> 创建版本库

~~~bash
mkdir git_study
cd git_study
git init
~~~

> 基本操作

~~~bash
# 查看状态
git status

# 添加文件
git add 文件名 # 可加空格添加多个  也可多次使用add

# 提交添加的文件
git commit -m "本次修改的备注"

# 查看修改的情况
git diff

# 查看日志
git log
# 可以加上--pretty=oneline参数

# 回退版本
git reset --hard HEAD^
git reset --hard 1094a # log中的版本号也可以，没必要写全
# 可以把暂存区的文件回退
git reset HEAD readme.txt # 然后使用git checkout -- 文件名  取消工作区的修改

# 查看历史命令，可以知道版本号
git reflog 

# 让工作区的文件回到最近一次add或commit的状态
git checkout -- readme.txt
# 两种情况：
# 一种是readme.txt自修改后还没有被放到暂存区，现在，撤销修改就回到和版本库一模一样的状态；
# 一种是readme.txt已经添加到暂存区后，又作了修改，现在，撤销修改就回到添加到暂存区后的状态。

~~~

> 工作区和暂存区和仓库



![image-20210115152836812](C:\Users\Administrator.MM-202008042005\AppData\Roaming\Typora\typora-user-images\image-20210115152836812.png)

> 查看历史提交

```bash
git log --pretty=oneline --abbrev-commit
```

## 远程仓库

> 查看远程仓库信息

~~~bash
git remote

# 更详细的信息
git remote -v
~~~

> 创建ssh key

```bash
# 在shell窗口,git bash窗口
ssh-keygen -t rsa -C "youremail@example.com"

```

> .gitignore介绍

~~~
# 此为注释 ,将被 Git 忽略
# 忽略所有 .a 结尾的文件
*.a
# 但 lib.a 除外
!lib.a
# 仅仅忽略项目根目录下的 TODO 文件，不包括 subdir/TODO
/TODO
# 忽略 build/ 目录下的所有文件
build/
# 会忽略 doc/notes.txt 但不包括 doc/server/arch.txt
doc/*.txt
# 会忽略掉 doc/ 里面所有的txt文件，包括子目录下的（**/ 从 Git 1.8.2 之后开始支持 **/ 匹配模式，表示递归匹配子目录下的文件）
doc/**/*.txt
local.properties #过滤具体文件
！local.properties#添加具体文件
*.[oa]#忽略所有以 .o 或 .a 结尾的文件
~~~

> 关联远程仓库

~~~bash
git remote add origin git@github.com:slippersyqy/git_study.git

# 如果已经关联了远程库无法关联其他远程库，可以移除关联的库，再关联其他远程库
git remote -v # 查看已关联的远程库
git remote rm origin
git remote add origin git@github.com:slippersyqy/git_study.git
~~~

> 拉取远程代码

~~~bash
# 直接拉取
git pull

# 若拉取失败可以先建立连接再使用pull 拉取
git branch --set-upstream-to=origin/dev dev

~~~



> 推送到远程

~~~bash
# 第一次推送到远程时，可以使用-u参数，将本地master分支与远程master分支关联起来，后续可以简化命令
git push -u origin master

# 后续的提交
git push origin master
~~~

> 先有远程库的时候如何克隆

~~~bash
git clone git@github.com:slippersyqy/git_study.git
~~~

## 分支管理

> 查看所有分支

~~~bash
git branch -a # 可按回车向下继续查看
~~~



> 创建分支

使用checkout切换

~~~bash
# 创建并进入dev分支
git checkout -b dev

# -b参数相当于下列两条命令
git branch dev
git checkout dev

# 查看当前分支
git branch

# 切换到master分支
git checkout master

# 在master中合并dev分支
git merge dev

# 删除分支
git branch -d dev

# 强行删除分支
git branch -D feature-vulcan
~~~

或者使用switch切换，注意：==2.23版本以后才能使用==

~~~bash
# 创建并切换分支
git switch -c dev

# 直接切换到已有的分支
git switch master

~~~

> 发生冲突无法合并。需要先将文件修改后保存，再提交

```bash
git merge feature1
# 结果如下
Auto-merging readme.txt
CONFLICT (content): Merge conflict in readme.txt
Automatic merge failed; fix conflicts and then commit the result.
```

> 删除分支

~~~bash
git branch -d dev
~~~

## stash使用

> stash 贮藏  相当于一个快照

~~~bash
# 使用
git stash

# 查看
git stash list

# 恢复快照状态
git stash pop

# 将修改的bug提交到某分支
$ git branch
* dev
  master
$ git cherry-pick 4c805e2
~~~

注意：先修复bug :` git cherry-pick <commit_id>`,再恢复工作`git stash pop`



## 标签

> 创建标签

~~~bash
# 切换到需要打标签的分支上，使用tag，默认标签是打在最新提交的commit上的
git tag v1.0

# 打在指定提交上，先使用log查看提交号，再使用tag
git log --pretty=oneline --abbrev-commit
git tag v0.9 f52c633

# 查看标签信息
git show v0.9

# 创建带有说明的标签，用-a指定标签名，-m指定说明文字
git tag -a v0.1 -m "version 0.1 released" 1094adb
~~~

> 操作标签

~~~bash
# 删除标签
git tag -d v0.1

# 推送标签
git push origin v1.0

# 一次性推送尚未推送到远程的本地标签
git push origin --tags

# 删除远程标签需先删除本地
git tag -d v0.1
git push origin :refs/tags/v0.1

~~~

## 配置

> 文件名显示颜色

```bash
git config --global color.ui true
```































