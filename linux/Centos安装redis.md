---
title: Linux命令
date: 2021-5-5
comments: true
categories: Linux
tags: linux安装redis
---



# Centos7安装Redis教程

原文出处 https://blog.csdn.net/weixin_43451430/article/details/115621335



版权
Centos7安装Redis
0.更新文件

```shell
yum update -y
```

这里会更新好多内容，需要等待一会。

## 1.下载redis

https://download.csdn.net/download/weixin_43451430/16796375
https://redis.io/

## 2.将压缩包放到Linux，我放在了export/intstall并解压

```shell
cd export/install
tar -zxvf redis-6.2.1.tar.gz
```

## 3.安装环境c++

```shell
//安装c++
yum install gcc-c++ -y

//查看版本
gcc -v
```

## 4.配置基本文件

```shell
cd redis-6.2.1
make
```



## 5.安装服务

```shell
这是默认安装
make install 
这是自定义安装
make install PREFIX=/usr/local/redis//后面的是你想要安装的路径
```

我是默认安装。



## 6.启动服务

```shell
cd /
cd usr/local/bin
./redis-server
```


启动成功如下图。


启动成功如下图。

按ctrl+c即可退出。

## 7.设置后台启动

先将配置文件copy到启动项下。

```shell
cd /
cd export/install/redis-6.2.1
cp redis.conf /usr/local/bin/
```



## 7.1.修改配置文件

```shell
cd /
cd usr/local/bin
vim redis.conf
```

这里是修改运行为后台运行。
打开文件后，如果你想查找，直接输入 / +你想要查找的字段 比如 / daemonized然后回车就可以查找了，按n表示下一个匹配字符：
查询到这个，将显示的no改成yes，然后按esc 输入：wq保存文件。

7.1.修改配置文件
cd /
cd usr/local/bin
vim redis.conf
1
2
3
这里是修改运行为后台运行。
打开文件后，如果你想查找，直接输入 / +你想要查找的字段 比如 / daemonized然后回车就可以查找了，按n表示下一个匹配字符：
查询到这个，将显示的no改成yes，然后按esc 输入：wq保存文件。

## 7.2.启动服务

```shell
 ./redis-server redis.conf
```



## 7.3.查看进程是否启动

```shell
 ps aux|grep redis 
```

 

## 8.关闭服务

```shell
./redis-cli shutdown
```



## 9.简单使用redis

开启redis服务：

```shell
 ./redis-server redis.conf
```

开启redis客户端：

```shell
 ./redis-cli
```

出现下面是开启成功：


测试：出现pong是对的。

退出

```shell
quit
```

## 10.启动远程连接

防火墙放行。

```shell
firewall-cmd --zone=public --add-port=6379/tcp --permanent
firewall-cmd --reload
```

修改redis.conf配置文件

关闭protected-mode模式，此时外部网络可以直接访问

修改redis.conf 文件，protected-mode 要设置成no。

如果需要设置密码requirepass 空格后面跟设置的密码

修改redis.conf 文件，将 bind 127.0.0.1 修改成bind * -::* 

或者直接将bind这一行注释掉。

记得修改完执行重启一下服务。

```shell
redis-cli -h 127.0.0.1 -p 6379 shutdown
```



# 干掉redis服务器（比较暴力，谨慎使用）
```shell
sudo kill -9 pid 进程号
```

链接其他ip地址的redis
这里的ip是根据你要链接的ip地址进行更换的

```shell
./redis-cli -h 192.168.10.20 -p 6379
```

如果设置了密码，那么就再输入auth空格+密码

```shell
auth 密码
```

## 2.在链接时直接加上你的密码

```shell
./redis-cli -h 你服务器的ip -p 6379 -a 你的密码
```







