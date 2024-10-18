---
title: linux语法
tags: [linux]
copyright: true
typora-root-url: linux语法
date: 2024-01-10 16:31:43
categories: 技术连连看
description: linux基本语法
cover: cover.png
---

 # linux基本语法

## 文件目录指令

### pwd指令

用于显示当前目录的路径

~~~
[root@localhost ~]# pwd
/root
[root@localhost ~]#

~~~

### ls指令

列出当前目录下所有的文件和目录

- ls `[选项][目录或文件]`
- -a ：显示当前目录所有的文件和目录，包括隐藏的
- -l ：以列表的方式显示信息，相当于ll

~~~
[root@localhost /]# ls
bin  boot  dev  etc  home  lib  lib64  media  mnt  opt  proc  root  run  sbin  srv  sys  tmp  usr  var
[root@localhost /]#


~~~

### cd命令

切换到指定目录

- cd [参数]
- cd~或cd ：回到自己的主目录
- cd.. ：回到当前目录的上一级目录

~~~
[root@localhost /]# cd
[root@localhost ~]# ls
anaconda-ks.cfg  initial-setup-ks.cfg  公共  模板  视频  图片  文档  下载  音乐  桌面
[root@localhost ~]# cd ..
[root@localhost /]# ls
bin  boot  dev  etc  home  lib  lib64  media  mnt  opt  proc  root  run  sbin  srv  sys  tmp  usr  var
[root@localhost /]#

~~~

### mkdir命令

创建目录

- mkdir [选项] 要创建的目录
- -p ：创建多级目录

~~~
[root@localhost opt]# mkdir temp
[root@localhost opt]# ll
总用量 0
drwxr-xr-x. 2 root root 6 10月 31 2018 rh
drwxr-xr-x. 2 root root 6 1月  11 12:35 temp
[root@localhost opt]#

~~~

### rmdir指令

删除一个空目录

- rmdir 目录

~~~
[root@localhost opt]# cd tem
[root@localhost tem]# ls
tem
[root@localhost tem]# rmdir tem
[root@localhost tem]# ll
总用量 0

~~~

### touch指令

创建一个或多个空文件

- touch 文件名称或列表

~~~
[root@localhost temp]# touch temp.txt
[root@localhost temp]# ll
总用量 0
-rw-r--r--. 1 root root 0 1月  11 12:57 temp.txt
[root@localhost temp]#

~~~

### cp命令

复制文件或目录

- cp [选项] 源文件 目标文件
- -r  递归复制整个文件夹

~~~
[root@localhost opt]# cd temp
[root@localhost temp]# ll
总用量 0
-rw-r--r--. 1 root root 0 1月  11 12:57 temp.txt
[root@localhost temp]# cp temp.txt ../
[root@localhost temp]# ll
总用量 0
-rw-r--r--. 1 root root 0 1月  11 12:57 temp.txt
[root@localhost temp]# cd ../
[root@localhost opt]# ll
总用量 0
drwxr-xr-x. 2 root root  6 10月 31 2018 rh
drwxr-xr-x. 2 root root 22 1月  11 12:57 temp
-rw-r--r--. 1 root root  0 1月  11 13:45 temp.txt
[root@localhost opt]#

~~~

### rm指令

删除文件或目录

- rm [选项] 要删除的文件或目录
- -r ：递归删除整个文件夹
- -f ：强制删除不提示

~~~
[root@localhost opt]# rm temp
rm: 无法删除"temp": 是一个目录
[root@localhost opt]# rm temp -r
rm：是否进入目录"temp"? y
rm：是否删除普通空文件 "temp/temp.txt"？y
rm：是否删除目录 "temp"？y
[root@localhost opt]# ll
总用量 0
drwxr-xr-x. 2 root root 6 10月 31 2018 rh
-rw-r--r--. 1 root root 0 1月  11 13:45 temp.txt
[root@localhost opt]#

~~~

### mv指令

为文件或目录改名、或将文件或目录移入其他位置

- mv 源文件 目标文件  将源文件名改为目标文件名
- mv 源文件 目标目录         将源文件移动到目标目录

~~~
[root@localhost opt]# ll
总用量 0
drwxr-xr-x. 2 root root 6 10月 31 2018 rh
-rw-r--r--. 1 root root 0 1月  11 13:45 temp.txt
[root@localhost opt]# mv temp.txt temp_new.txt
[root@localhost opt]# ll
总用量 0
drwxr-xr-x. 2 root root 6 10月 31 2018 rh
-rw-r--r--. 1 root root 0 1月  11 13:45 temp_new.txt
[root@localhost opt]#

~~~

### cat指令

查看文件内容

- cat [选项] 目标文件
- -n ：显示行号

~~~
[root@localhost opt]# cat -n  temp_new.txt
     1  wudiyang
     2  yangwudi
     3  yangdiwu
     4  diwuyang
     5  yangxiaosa
     6
[root@localhost opt]#

~~~

### more指令

查看文件内容（分页）

- more  目标文件

| 操作   | 功能说明               |
| ------ | ---------------------- |
| 空格   | 下一页                 |
| enter  | 下一行                 |
| q      | 退出more               |
| ctrl+f | 向下滚动一屏           |
| ctrl+b | 返回上一屏             |
| =      | 输出当前行的行号       |
| :f     | 输出文件名和当前行行号 |

~~~
[root@localhost opt]# more 1.txt temp_new.txt
::::::::::::::
1.txt
::::::::::::::
wudiyang
yangwudi
yangdiwu
diwuyang
yangxiaosa

::::::::::::::
temp_new.txt
::::::::::::::
wudiyang
yangwudi
yangdiwu
diwuyang
yangxiaosa

[root@localhost opt]#

~~~

### less指令

分页查看文件内容（比more更强大）

-  less  目标文件

与more类似，可以随意浏览文件

| 操作       | 功能说明                                           |
| ---------- | -------------------------------------------------- |
| 空格键     | 向下翻动一页                                       |
| [pagedown] | 向下翻动一页                                       |
| [pageup]   | 向上翻动一页                                       |
| /字串      | 向下搜寻【字串】的功能；n：向下查找；N：向上查找； |
| ？字串     | 向上搜寻【字串】的功能；n：向上查找；N：向下查找； |
| q          | 离开这个less程序                                   |

### head指令

head 命令可用于查看文件的开头部分的内容

- head 文件（默认查看文件头10行内容）
- head -n 5 文件（查看文件头5行内容，5可以是任意数）

~~~
[root@localhost /]# head -n 2 /opt/temp_new.txt
wudiyang
yangwudi
[root@localhost /]#

~~~

### tail指令

tail 命令可用于查看文件的内容，有一个常用的参数 -f 常用于查阅正在改变的日志文件。

- tail [参数] 文件
- -f ：把文件最尾部的内容显示在屏幕上，并不断刷新

~~~
[root@localhost /]# tail -f /opt/temp_new.txt
wudiyang
yangwudi
yangdiwu
diwuyang
yangxiaosa

^C
[root@localhost /]# 

~~~

### echo指令

echo命令用于输出变量或常量内容到控制台

- echo 选项  输出内容

~~~
[root@localhost /]# echo "woxiangshangan"
woxiangshangan
[root@localhost /]#

~~~

### >指令

将前一个查看指令中的结果覆盖写入到目标文件中，如果目标文件不存在，则新建。

- 查看指令 > 目标文件

~~~
[root@localhost opt]# cat 1.txt
wudiyang
yangwudi
yangdiwu
diwuyang
yangxiaosa

[root@localhost opt]# cat 1.txt > 2.txt
[root@localhost opt]# cat 2.txt
wudiyang
yangwudi
yangdiwu
diwuyang
yangxiaosa

~~~

### >>指令

  将前一个查看指令中的结果追加写入到目标文件中，如果目标文件不存在，则新建。

- 查看指令 >> 目标文件

~~~
[root@localhost opt]# cat 1.txt
wudiyang
yangwudi
yangdiwu
diwuyang
yangxiaosa

[root@localhost opt]# cat 1.txt >> 2.txt
[root@localhost opt]# cat 2.txt
wudiyang
yangwudi
yangdiwu
diwuyang
yangxiaosa

wudiyang
yangwudi
yangdiwu
diwuyang
yangxiaosa

[root@localhost opt]#

~~~

## 时间日期指令

### date指令

Linux **date** 命令可以用来显示或设定系统的日期与时间。

- date（显示当前时间）
- date + %Y（显示当前年份）
- date + %m（显示当前月份）
- date + %d（显示当前是哪一天）
- date + "%Y-%m-%d %H:%M:%S"（显示年月日时分秒）
- date -s 字符串时间（设置当前时间）

~~~
[root@localhost opt]# date "+%Y-%m-%d %H:%M:%S"
2024-01-22 11:23:55
[root@localhost opt]#

~~~

### cal指令

显示日历

- cal  参数  月份  年份
- -1 显示一个月的月历
- -3 显示系统前一个月、当前月、下一个月的月历
- -y 显示当前年的日历

~~~
[root@localhost opt]# cal -1 9 2022
      九月 2022
日 一 二 三 四 五 六
             1  2  3
 4  5  6  7  8  9 10
11 12 13 14 15 16 17
18 19 20 21 22 23 24
25 26 27 28 29 30

[root@localhost opt]#

~~~

## 搜索查找指令

### find指令

find指令将从指定目录向下递归地遍历其各个子目录，将满足条件的文件或者目录显示在终端。

- find  搜索范围  选项
- -name ：按名称查找，支持通配符。
- -user：按用户名查找。
- -size ：按文件大小查找。

~~~
[root@localhost opt]# find -name 1.txt
./1.txt
[root@localhost opt]#

~~~

### grep指令

Linux grep (global regular expression) 命令用于查找文件里符合条件的字符串或正则表达式。

- grep  options  pattern  files
- pattern :表示要查找的字符串或正则表达式
- files ：表示要查找的文件名，可以同时查找多个文件，如果忽略files参数，则默认从标准输入中读取数据
- -i：忽略大小写进行匹配
- -v：反向查找，只打印不匹配的行
- -n：显示匹配行的行号
- -r：递归查找子目录中的文件
- -l：只打印匹配的文件名
- -c：只打印匹配的行数

~~~
[root@localhost opt]# cat 1.txt | grep -i yang
wudiyang
yangwudi
yangdiwu
diwuyang
yangxiaosa
[root@localhost opt]#


~~~

  说明：grep 过滤查找，管道符，“|”，表示将前一个命令的处理结果输出传递给后面的命令处理。

## 压缩和解压指令

### gzip指令

压缩文件，将文件压缩为*.gz文件存放在原文件所在目录，压缩成功后会把原文件删除。用于压缩单个文件。

- gzip 文件名

~~~
[root@localhost opt]# gzip 1.txt
[root@localhost opt]# ll
总用量 12
-rw-r--r--. 1 root root 57 1月  11 14:51 1.txt.gz
-rw-r--r--. 1 root root 96 1月  22 10:39 2.txt
drwxr-xr-x. 2 root root  6 10月 31 2018 rh
-rw-r--r--. 1 root root 48 1月  11 14:28 temp_new.txt
[root@localhost opt]#

~~~

### gunzip指令

解压缩文件命令，解压成功后存放在原压缩文件所在目录，并且把原压缩文件删除。

- gunzip  压缩文件名

~~~
[root@localhost opt]# ll
总用量 12
-rw-r--r--. 1 root root 57 1月  11 14:51 1.txt.gz
-rw-r--r--. 1 root root 96 1月  22 10:39 2.txt
drwxr-xr-x. 2 root root  6 10月 31 2018 rh
-rw-r--r--. 1 root root 48 1月  11 14:28 temp_new.txt
[root@localhost opt]# gunzip 1.txt.gz
[root@localhost opt]# ll
总用量 12
-rw-r--r--. 1 root root 48 1月  11 14:51 1.txt
-rw-r--r--. 1 root root 96 1月  22 10:39 2.txt
drwxr-xr-x. 2 root root  6 10月 31 2018 rh
-rw-r--r--. 1 root root 48 1月  11 14:28 temp_new.txt
[root@localhost opt]#

~~~

### zip指令

将指定文件或目录压缩成XXX.zip文件，用于压缩所有文件结构

- zip  [选项] XXX.zip 将要压缩的内容
- -r：递归压缩，即压缩目录

~~~
[root@localhost opt]# ll
总用量 12
-rw-r--r--. 1 root root 48 1月  11 14:51 1.txt
-rw-r--r--. 1 root root 96 1月  22 10:39 2.txt
drwxr-xr-x. 2 root root  6 10月 31 2018 rh
-rw-r--r--. 1 root root 48 1月  11 14:28 temp_new.txt
[root@localhost opt]# zip -r 666.zip rh
  adding: rh/ (stored 0%)
[root@localhost opt]# ll
总用量 16
-rw-r--r--. 1 root root  48 1月  11 14:51 1.txt
-rw-r--r--. 1 root root  96 1月  22 10:39 2.txt
-rw-r--r--. 1 root root 156 1月  22 15:01 666.zip
drwxr-xr-x. 2 root root   6 10月 31 2018 rh
-rw-r--r--. 1 root root  48 1月  11 14:28 temp_new.txt
[root@localhost opt]#

~~~

### unzip指令

Linux unzip命令用于解压缩zip文件

- unzip zip文件
- -d：指定解压后文件的存放目录

~~~
[root@localhost opt]# ll
总用量 16
-rw-r--r--. 1 root root  48 1月  11 14:51 1.txt
-rw-r--r--. 1 root root  96 1月  22 10:39 2.txt
-rw-r--r--. 1 root root 156 1月  22 15:01 666.zip
drwxr-xr-x. 2 root root   6 10月 31 2018 rh
-rw-r--r--. 1 root root  48 1月  11 14:28 temp_new.txt
[root@localhost opt]# unzip -d ./rh 666.zip
Archive:  666.zip
   creating: ./rh/rh/
[root@localhost opt]# ll
总用量 16
-rw-r--r--. 1 root root  48 1月  11 14:51 1.txt
-rw-r--r--. 1 root root  96 1月  22 10:39 2.txt
-rw-r--r--. 1 root root 156 1月  22 15:01 666.zip
drwxr-xr-x. 3 root root  16 1月  22 15:11 rh
-rw-r--r--. 1 root root  48 1月  11 14:28 temp_new.txt
[root@localhost opt]# cd rh
[root@localhost rh]# ll
总用量 0
drwxr-xr-x. 2 root root 6 10月 31 2018 rh
[root@localhost rh]#

~~~

### tar指令

打包或者解压文件

- tar [选项] XXX.tar.gz [打包的内容]
- -c：产生.tar.gz打包文件
- -v：显示详情信息
- -f：指定压缩后的文件名
- -z：通过gzip指令处理备份文件
- -x：解压.tar.gz文件
- -C：指定解压到哪个目录
- -t：列出文件内容

~~~
//打包并压缩一个文件
[root@localhost opt]# ll
总用量 16
-rw-r--r--. 1 root root  48 1月  11 14:51 1.txt
-rw-r--r--. 1 root root  96 1月  22 10:39 2.txt
-rw-r--r--. 1 root root 156 1月  22 15:01 666.zip
drwxr-xr-x. 3 root root  16 1月  22 15:11 rh
-rw-r--r--. 1 root root  48 1月  11 14:28 temp_new.txt
[root@localhost opt]# tar -zcvf yang.tar.gz 1.txt 2.txt
1.txt
2.txt
[root@localhost opt]# ll
总用量 20
-rw-r--r--. 1 root root  48 1月  11 14:51 1.txt
-rw-r--r--. 1 root root  96 1月  22 10:39 2.txt
-rw-r--r--. 1 root root 156 1月  22 15:01 666.zip
drwxr-xr-x. 3 root root  16 1月  22 15:11 rh
-rw-r--r--. 1 root root  48 1月  11 14:28 temp_new.txt
-rw-r--r--. 1 root root 172 1月  22 16:04 yang.tar.gz
[root@localhost opt]#

//查看一个压缩文件
[root@localhost opt]# ll
总用量 20
-rw-r--r--. 1 root root  48 1月  11 14:51 1.txt
-rw-r--r--. 1 root root  96 1月  22 10:39 2.txt
-rw-r--r--. 1 root root 156 1月  22 15:01 666.zip
drwxr-xr-x. 3 root root  16 1月  22 15:11 rh
-rw-r--r--. 1 root root  48 1月  11 14:28 temp_new.txt
-rw-r--r--. 1 root root 172 1月  22 16:04 yang.tar.gz
[root@localhost opt]# tar -ztvf yang.tar.gz
-rw-r--r-- root/root        48 2024-01-11 14:51 1.txt
-rw-r--r-- root/root        96 2024-01-22 10:39 2.txt
[root@localhost opt]#


//解压一个压缩文件
[root@localhost opt]# tar -zxvf yang.tar.gz -C ./rh/
1.txt
2.txt
[root@localhost opt]# ll
总用量 20
-rw-r--r--. 1 root root  48 1月  11 14:51 1.txt
-rw-r--r--. 1 root root  96 1月  22 10:39 2.txt
-rw-r--r--. 1 root root 156 1月  22 15:01 666.zip
drwxr-xr-x. 3 root root  42 1月  22 16:10 rh
-rw-r--r--. 1 root root  48 1月  11 14:28 temp_new.txt
-rw-r--r--. 1 root root 172 1月  22 16:04 yang.tar.gz
[root@localhost opt]# cd rh
[root@localhost rh]# ll
总用量 8
-rw-r--r--. 1 root root 48 1月  11 14:51 1.txt
-rw-r--r--. 1 root root 96 1月  22 10:39 2.txt
drwxr-xr-x. 2 root root  6 10月 31 2018 rh
[root@localhost rh]#


~~~

## 组管理指令

### Linux组的基本介绍

在linux中每个用户必须属于一个组，不能独立与组外，可以改变用户所属组

在linux中每个文件有所有者、所在的组、其他组，也可以改变文件所在组

### 文件/目录的所有者

一般为文件的创建者，谁创建了该文件，就自然地成为该文件的所有者，默认情况下所有者所在的组也是该文件所在的组

### 查看文件所有者和所在组指令

- ls -ahl 文件名

~~~
[root@localhost rh]# ls -ahl
总用量 8.0K
drwxr-xr-x. 3 root root 42 1月  22 16:10 .
drwxr-xr-x. 3 root root 96 1月  22 16:04 ..
-rw-r--r--. 1 root root 48 1月  11 14:51 1.txt
-rw-r--r--. 1 root root 96 1月  22 10:39 2.txt
drwxr-xr-x. 2 root root  6 10月 31 2018 rh
[root@localhost rh]# ls -ahl 1.txt
-rw-r--r--. 1 root root 48 1月  11 14:51 1.txt
[root@localhost rh]#

~~~

### 修改文件所有者指令

- chown  新所有者 文件名
- chown  新所有者:新组  文件名

~~~
[root@localhost rh]# ls -ahl
总用量 8.0K
drwxr-xr-x. 3 root root 42 1月  22 16:10 .
drwxr-xr-x. 3 root root 96 1月  22 16:04 ..
-rw-r--r--. 1 root root 48 1月  11 14:51 1.txt
-rw-r--r--. 1 root root 96 1月  22 10:39 2.txt
drwxr-xr-x. 2 root root  6 10月 31 2018 rh
[root@localhost rh]# chown admin:admin 1.txt
[root@localhost rh]# ls -ahl
总用量 8.0K
drwxr-xr-x. 3 root  root  42 1月  22 16:10 .
drwxr-xr-x. 3 root  root  96 1月  22 16:04 ..
-rw-r--r--. 1 admin admin 48 1月  11 14:51 1.txt
-rw-r--r--. 1 root  root  96 1月  22 10:39 2.txt
drwxr-xr-x. 2 root  root   6 10月 31 2018 rh
[root@localhost rh]#

~~~

### 修改文件所在组指令

- chgrp 新组名  文件名
- -R：如果是目录则使其下所有子文件或目录递归生效

~~~
[root@localhost rh]# ls -ahl
总用量 8.0K
drwxr-xr-x. 3 root  root  42 1月  22 16:10 .
drwxr-xr-x. 3 root  root  96 1月  22 16:04 ..
-rw-r--r--. 1 admin admin 48 1月  11 14:51 1.txt
-rw-r--r--. 1 root  root  96 1月  22 10:39 2.txt
drwxr-xr-x. 2 root  root   6 10月 31 2018 rh
[root@localhost rh]# chgrp admin 2.txt
[root@localhost rh]# ls -ahl
总用量 8.0K
drwxr-xr-x. 3 root  root  42 1月  22 16:10 .
drwxr-xr-x. 3 root  root  96 1月  22 16:04 ..
-rw-r--r--. 1 admin admin 48 1月  11 14:51 1.txt
-rw-r--r--. 1 root  admin 96 1月  22 10:39 2.txt
drwxr-xr-x. 2 root  root   6 10月 31 2018 rh
[root@localhost rh]#

~~~

## 权限管理指令

### 权限管理介绍

这里所说的权限都是文件和目录的权限。在linux中，每一个文件和目录都有自己的访问权限，通过文件列表可以查看到。

### RWX权限详解

#### rwx作用到文件

1. [r]代表可读（read）：可以读取，查看
2. [w]代表可写（write）：可以修改，但是不代表可以删除文件，删除一个文件的前提条件是对该文件所在的目录有写权限，才能删除该文件。
3. [x]代表可是执行（execute）：可以被执行

#### rwx作用到目录

1. [r]代表可读（read）：可以读取，ls查看目录内容
2. [w]代表可写（write）：可以修改，目录内创建+删除+重命名目录
3. [x]代表可执行（execute）：可以进入该目录

#### rwx用数字表示

r=4(即 2^2^),w=2(即2^1^),x=1(即2^0^)

## 权限管理指令

### 修改文件/目录权限的指令chmod

#### 通过r、w、x变更权限

1. chmod u=rwx,g=rx,0=x 文件目录名
2. chmod o+w 文件目录名
3. chmod a-x 文件目录名

说明：u、g、o、a分别代表文件所有者、文件所在组用户、其他组用户、所有用户；=、+、- 分别代表设置权限、增加权限、去掉权限

#### 通过数字变更权限

chomd  一组三个数字  文件目录名

说明：r=4 w=2 x=1 rwx=4+2+1=7

~~~
[root@localhost rh]# ls -ahl
总用量 8.0K
drwxr-xr-x. 3 root  root  42 1月  22 16:10 .
drwxr-xr-x. 3 root  root  96 1月  22 16:04 ..
-rw-r--r--. 1 admin admin 48 1月  11 14:51 1.txt
-rw-r--r--. 1 root  admin 96 1月  22 10:39 2.txt
drwxr-xr-x. 2 root  root   6 10月 31 2018 rh
[root@localhost rh]# chmod 777 1.txt
[root@localhost rh]# ls -ahl
总用量 8.0K
drwxr-xr-x. 3 root  root  42 1月  22 16:10 .
drwxr-xr-x. 3 root  root  96 1月  22 16:04 ..
-rwxrwxrwx. 1 admin admin 48 1月  11 14:51 1.txt
-rw-r--r--. 1 root  admin 96 1月  22 10:39 2.txt
drwxr-xr-x. 2 root  root   6 10月 31 2018 rh

~~~





