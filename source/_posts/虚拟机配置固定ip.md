---
title: 虚拟机配置固定ip
tags: [虚拟机,CentOS7]
copyright: true
typora-root-url: 虚拟机配置固定ip
date: 2023-12-22 15:06:11
categories: 技术连连看
description: 虚拟机配置固定ip
cover: conver.png
---

 

​    使用的虚拟机为vm17，装入linux系统CentOS7，创建好虚拟机后IP总是自动改变，于是想要配置固定ip。

1. 首先cd到`/etc/sysconfig/network-scripts`

   ~~~
   [root@localhost /]# cd /etc/sysconfig/network-scripts
   [root@localhost network-scripts]# 
   ~~~

2. 修改`ifcdf-ens33`文件

   ```
   [root@localhost network-scripts]# vim ifcif-ens33
   
   //文件内容
   PROXY_METHOD=none
   BROWSER_ONLY=no
   BOTTPROTO=static//将ip改为静态获取
   #BOOTPROTO=dhcp
   DEFROUTE=yes
   IPV4_FAILURE_FATAL=no
   IPV6INIT=yes
   IPV6_AUTOCONF=yes
   IPV6_DEFROUTE=yes
   IPV6_FAILURE_FATAL=no
   IPV6_ADDR_GEN_MODE=stable-privacy
   NAME=ens33
   UUID=661d2508-8d73-4d5c-b1b4-4f2f6d7100c4
   DEVICE=ens33
   ONBOOT=yes//开启自启
   //以下内容新增
   ZONE=public
   IPADDR=192.168.33.131//需要配置的静态ip
   NETMASK=255.255.255.0
   GETEWAY=192.168.33.2//网关
   DNS1=114.114.114.114
   DNS2=8.8.8.8
   NM_CONTROLLED=no//必须配置，将网络设备脱离NetworkManager服务的管理
   
   //点击ESC，输入“:wq”保存退出vim编辑器
   ```

   

3. 配置永久路由，修改（新建）route-ens33文件

   ~~~
   [root@localhost network-scripts]# vim route-ens33
   
   //修改为以下内容
   192.168.33.0/24 via 192.168.33.2
   0.0.0.0/0 via 192.168.33.2
   
   //点击ESC，输入“:wq”保存退出vim编辑器
   ~~~

4. 重启网络服务`service network restart`

   ~~~
   [root@localhost network-scripts]# vim route-ens33
   [root@localhost network-scripts]# service network restart
   Restarting network (via systemctl):                        [  确定  ]
   [root@localhost network-scripts]# 
   ~~~

   

5. 点击虚拟机 工具栏-编辑-虚拟网络编辑器，点击更改设置，赋予管理员权限，根据图片配置信息

   ![第一步](第一步.png)

   ![第二步](第二步.png)

   ![第三步](第三步.png)

6. 测试是否可以联网，联网成功，`Ctrl + C `取消`ping`命令

   ~~~
   [root@localhost network-scripts]# ping www.baidu.com
   PING www.a.shifen.com (110.242.68.4) 56(84) bytes of data.
   64 bytes from 110.242.68.4 (110.242.68.4): icmp_seq=1 ttl=128 time=17.2 ms
   64 bytes from 110.242.68.4 (110.242.68.4): icmp_seq=2 ttl=128 time=17.6 ms
   64 bytes from 110.242.68.4 (110.242.68.4): icmp_seq=3 ttl=128 time=17.7 ms
   64 bytes from 110.242.68.4 (110.242.68.4): icmp_seq=4 ttl=128 time=17.8 ms
   ^C
   --- www.a.shifen.com ping statistics ---
   4 packets transmitted, 4 received, 0% packet loss, time 7075ms
   rtt min/avg/max/mdev = 17.243/17.620/17.897/0.239 ms
   [root@localhost network-scripts]# 
   ~~~

   

   
