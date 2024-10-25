---
title: git同时上传gitee和github
tags: [git]
copyright: true
typora-root-url: git同时上传gitee和github
date: 2024-10-25 15:01:05
categories: 技术连连看
description: git上传gitee和github
cover:
top:
hide:
---

#   <center>git同时上传gitee和github平台</center>

转自：[前端 - 将本地仓库项目上传github和gitee仓库中 - 个人文章 - SegmentFault 思否](https://segmentfault.com/a/1190000044950335)

# 一、Gitee创建代码仓库

## 1、创建仓库

点击右上角 + 处，选择新建仓库

![Gitee创建仓库](Gitee创建仓库.png)

## 2、填写仓库信息

根据提示，填写仓库的名称、描述信息、是否公开等，来完成下图所示的创建仓库过程。

![Gitee填写仓库信息](Gitee填写仓库信息.png)

## 3、查看创建的代码仓库

直接进入代码查库查看是否创建成功：

![Gitee查看创建的代码仓库](Gitee查看创建的代码仓库.png)

# 二、Github创建代码仓库

## 1、创建仓库（Repository）

点击右上角+，选择 New repository，来创建仓库：

![Github创建仓库](Github创建仓库.png)

## 2、填写仓库信息

根据提示，填写仓库的名称、描述信息、是否公开等，来完成下图所示的创建仓库过程。

![Github填写仓库信息](Github填写仓库信息.png)

### 3、查看创建的代码仓库

直接进入代码查库查看是否创建成功：

![Github查看创建的代码库](Github查看创建的代码库.png)

# 三、生成SSH公钥

## 1、清除Git全局设置

通过git config --global --list指令来查看git是否全局设置（我前期使用过Gitee所以有设置，git新用户一般没有）：

~~~javascript
git config --global --list
safe.directory=/opt/homebrew
user.name=China-quanda
user.email=877880098@qq.com
~~~

## 2、通过下列指令来清除git的全局设置，将youName与youEmail替换成自己的用户名及邮箱：

```javascript
git config --global --unset user.name "youName"
git config --global --unset user.email "youEmail"
```

## 3、SSH key的生成

### 3.1、生成Github的SSH Key：

```javascript
ssh-keygen -t rsa -f ~/.ssh/id_rsa.github -C "877880098@qq.com"
```

### 3.2、生成Gitee的SSH Key：

```javascript
ssh-keygen -t rsa -f ~/.ssh/id_rsa.gitee -C "877880098@qq.com"
```

## 4、查看生成的SSH Key

windows在c盘的~/.ssh / 路径下

mac系统是在 用户下的 ：

![查看生成的SSH Key](查看生成的SSH Key.png)

## 5、配置SSH识别新的私钥

由于默认只读取 id_rsa，为了让 SSH 识别新的私钥，需要将新的私钥加入到 SSH agent 中

需要在终端依次执行：

```javascript
ssh-agent bash
ssh-add ~/.ssh/id_rsa.github
ssh-add ~/.ssh/id_rsa.gitee
```

![新的私钥加入到 SSH agent](新的私钥加入到 SSH agent.png)

## 6、多账号配置

为了便于Github与Gitee都能使用git，需要进行多账号配置：

查看您的.ssh文件中是否存在 config 文件，如果不存在则创建生成config文件： touch ~/.ssh/config

作者这边目录下是有这个config配置文件的 所以不需要创建了，直接打开文件进行修改

在该文件（config）中填写下列内容：

```javascript
#Default gitHub user Self
Host github.com
    HostName github.com
    User git
    IdentityFile ~/.ssh/id_rsa.github

# gitee
Host gitee.com
    Port 22
    HostName gitee.com
    User git
    IdentityFile ~/.ssh/id_rsa.gitee
```

# 四、添加SSH

## 1、Gitee添加SSH

将文件id_rsa.gitee.pub内容复制到SSH Key中，成功后如下图所示：

![Gitee添加SSH](Gitee添加SSH.png)

## 2、Github添加SSH

将文件id_rsa.github.pub内容复制到SSH Key中，成功后如下图所示：

![Github添加SSH](Github添加SSH.png)

## 3、测试

分别使用下列指令进行链接测试，直接yes，红框处出现Hi表示公钥添加成功：

```javascript
ssh -T git@gitee.com
ssh -T git@github.com
```

![测试](测试.png)

# 五、代码上传仓库

## 1、创建本地仓库 打开项目文件夹作为Git的本地仓库

![本地仓库](本地仓库.png)

## 2、在终端打开项目文件夹执行git init 指令，把文件qd-batteryOptimize项目变成Git可以管理的仓库，生成.git文件表示创建成功：

![初始化本地仓库](初始化本地仓库.png)

## 3、关联远程仓库

### 3.1、关联github仓库

```javascript
git remote add github git@github.com:China-quanda/qd-batteryOptimize.git    
```

### 3.2、关联gitee仓库

```javascript
git remote add gitee https://gitee.com/china-quanda/qd-battery-optimize.git
```

### 3.3、查看本地仓库关联的远程仓库

使用 `git remote -v` 查看本地仓库关联了哪些远程仓库

可以看到关联了 github 和 gitee 仓库

![查看关联](查看关联.png)

## 4、将项目所有文件添加为暂存区

使用` git add .` 将本地仓库项目添加为暂存区中

## 5、提交本地暂存区文件到本地仓库

使用 `git commit -m` "忽略电池优化功能" 命令提交到本地仓库中

## 6、将本地仓库提交到远程Gitee仓库中

使用 `git push -u gitee master` 命令将本地仓库提交到远程仓库

```javascript
quanda@192 qd-batteryOptimize % git push -u gitee master                   
枚举对象中: 15, 完成.
对象计数中: 100% (15/15), 完成.
使用 8 个线程进行压缩
压缩对象中: 100% (13/13), 完成.
写入对象中: 100% (15/15), 5.66 KiB | 5.66 MiB/s, 完成.
总共 15（差异 0），复用 0（差异 0），包复用 0（来自  0 个包）
remote: Powered by GITEE.COM [GNK-6.4]
To https://gitee.com/china-quanda/qd-battery-optimize.git
 * [new branch]      master -> master
分支 'master' 设置为跟踪 'gitee/master'。
```

刷新仓库可以看到本地仓库提交到远程仓库成功了：

![提交Gitee仓库](提交Gitee仓库.png)

## 7、将本地仓库提交到远程Github仓库中

使用 `git push -u github master` 命令将本地仓库提交到远程仓库

```javascript
quanda@192 qd-batteryOptimize % git push -u github master                                                  
枚举对象中: 15, 完成.
对象计数中: 100% (15/15), 完成.
使用 8 个线程进行压缩
压缩对象中: 100% (13/13), 完成.
写入对象中: 100% (15/15), 5.66 KiB | 5.66 MiB/s, 完成.
总共 15（差异 0），复用 0（差异 0），包复用 0（来自  0 个包）
remote: 
remote: Create a pull request for 'master' on GitHub by visiting:
remote:      https://github.com/China-quanda/qd-batteryOptimize/pull/new/master
remote: 
To https://github.com/China-quanda/qd-batteryOptimize.git
 * [new branch]      master -> master
分支 'master' 设置为跟踪 'github/master'。
```

刷新仓库可以看到本地仓库提交到远程仓库成功了：

![提交Github仓库](提交Github仓库.png)



THE   END
