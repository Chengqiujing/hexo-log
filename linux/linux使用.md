---
title: Linux命令
date: 2021-5-5
comments: true
categories: Linux
tags: linux基础命令
---



查看端口占用：



查看吞吐率：





> 原文出处 [刀锋93，一只小菜鸟的逆袭之路!](https://www.cnblogs.com/hebiao/)

## [CentOS查看端口防火墙状态](https://www.cnblogs.com/hebiao/p/12966000.html)

1.查看防火墙状态

命令：firewall-cmd --state

2.关闭防火墙

命令：systemctl stop firewalld.service

3.启动防火墙

命令：systemctl start firewalld.service

4.设置开机自动启动：`systemctl enable firewalld.service`

关闭开机启动：`systemctl disable firewalld.service`

5.查看防火墙所有开放的端口

命令：firewall-cmd --zone=public --list-ports

这条命令我执行完后居然什么都没有，我理解为我安装的虚拟机上可能没启任何端口吧，但一点提示也没有太不友好了。

6.开启某个(某段)端口

```
firewall-cmd --zone=public --add-port=8080-8081/tcp --permanent //永久
```

\```firewall-cmd --zone=public --add-port=8080-8081/tcp //临时`

firewall-cmd --zone=public --add-port=5672/tcp --permanent  # 永久开放5672端口

firewall-cmd --zone=public --remove-port=5672/tcp --permanent #永久关闭5672端口

firewall-cmd --reload  # 在不改变状态的条件下重新加载防火墙--配置立即生效

7.查看具体某个端口

命令：netstat -tunlp | grep 8081 【模糊查询】

lsof -i:8081【精确查询】

命令格式：netstat -tunlp | grep 端口号

- -t (tcp) 仅显示tcp相关选项
- -u (udp)仅显示udp相关选项
- -n 拒绝显示别名，能显示数字的全部转化为数字
- -l 仅列出在Listen(监听)的服务状态
- -p 显示建立相关链接的程序名
- *PS:centos7默认没有 netstat 命令，需要安装 net-tools 工具，yum install -y net-tools*

*如果发现端口被占用，kill -9 进程号 杀死进程*

*8.查看开放的所有端口，命令：firewall-cmd --list-ports*

*下面记录一下服务的相关命令：*

获取所有支持的服务firewall-cmd --get-service

启动某个服务firewall-cmd --zone=public --add-service=https //临时 firewall-cmd --permanent --zone=public --add-service=https //永久

查看开启的服务firewall-cmd --permanent --zone=public --list-services

设置ip访问某个服务firewall-cmd --permanent --zone=public --add-rich-rule="rule family="ipv4" source address="192.168.0.4/24" service name="http" accept" //ip 192.168.0.4/24 访问 http

*删除上面的设置firewall-cmd --permanent --zone=public --remove-rich-rule="rule family="ipv4" source address="192.168.0.4/24" service name="http" accept"*

*检查设定是否生效 iptables -L -n | grep 21*



