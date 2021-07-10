---
title: Linux命令
date: 2021-5-5
comments: true
categories: Linux
tags: linux安装mysql
---













# CentOS7 安装mysql7

> 原文地址： https://www.cnblogs.com/yy3b2007com/p/10497787.html



服务器环境：

centos7 x64

需要安装mysql5.7+

## 一、卸载CentOS7系统自带mariadb

```shell
# 查看系统自带的Mariadb
[root@CDH-141 ~]# rpm -qa|grep mariadb
mariadb-libs-5.5.44-2.el7.centos.x86_64
# 卸载系统自带的Mariadb
[root@CDH-141 ~]# rpm -e --nodeps mariadb-libs-5.5.44-2.el7.centos.x86_64
# 删除etc目录下的my.cnf
[root@CDH-141 ~]# rm /etc/my.cnf
```

## 二、检查mysql是否存在

```shell
# 检查mysql是否存在
[root@CDH-141 ~]# rpm -qa | grep mysql
[root@CDH-141 ~]# 
```

## 三、查看用户和组是否存在

1）检查mysql组合用户是否存在

```shell
# 检查mysql组和用户是否存在，如无则创建
[root@CDH-141 ~]# cat /etc/group | grep mysql
[root@CDH-141 ~]# cat /etc/passwd | grep mysql 
```

\# 查询全部用户（只是做记录，没必要执行）

```shell
[root@CDH-141 ~]# cat /etc/passwd|grep -v nologin|grep -v halt|grep -v shutdown|awk -F ":" '{print $1 "|" $3 "1" $4}' | more
root|010
sync|510
flume|9921989
hdfs|9911988
zookeeper|9891986
llama|9881985
httpfs|9871984
mapred|9861983
sqoop|9851982
yarn|9841981
kms|9831980
hive|9821979
oozie|9801977
hbase|9781975
impala|9761973
hue|9741971
wlaqzc2018|100111001
[root@CDH-141 mysql]# 
```

2）若不存在，则创建mysql组和用户

```shell
# 创建mysql用户组
[root@CDH-141 ~]# groupadd mysql
# 创建一个用户名为mysql的用户，并加入mysql用户组
[root@CDH-141 ~]# useradd -g mysql mysql
# 制定password 为111111
[root@CDH-141 ~]# passwd mysql
Changing password for user mysql.
New password:
BAD PASSWORD: The password is a palindrome
Retype new password:
passwd: all authentication tokens updated successfully.
```

## 四、下载mysql离线安装包tar文件

官网下载地址：https://dev.mysql.com/downloads/mysql/5.7.html#downloads

版本选择，可以选择一下两种方式：

1）使用Red Hat Enterprise Linux
Select Version:**5.7.25**
Select Operating **System:Red Hat Enterprise Linux / Oracle Linux**
Select OS Version:**Red Hat Enterprise Linux 7 / Oracle Linux 7 (x86, 64-bit)**
列表中下载：
Compressed TAR Archive：(mysql-5.7.25-el7-x86_64.tar.gz)
2）使用Linux - Generic
Select Version:**5.7.25**
Select Operating System:**Linux - Generic**
Select OS Version:**Linux - Generic (glibc 2.12) (x86, 64-bit)**
列表中下载：
Compressed TAR Archive：(mysql-5.7.25-linux-glibc2.12-x86_64.tar.gz)【本文中使用的是这个版本】
注意：上边两种方式找mysql离线安装包的方式都可以。

## 五、上传第四步下载的mysql TAR包

```sh
# 进入/usr/local/文件夹
[root@CDH-141 ~]# cd /usr/local/
# 上传mysql TAR包
[root@CDH-141 local]# rz
# 解压mysql-5.7.25-linux-glibc2.12-x86_64.tar.gz
[root@CDH-141 local]# ls
bin  full-path-to-mysql-VERSION-OS  include  lib64    mysql-5.7.25-linux-glibc2.12-x86_64.tar.gz  share
etc  games                          lib      libexec  sbin                                 src
[root@CDH-141 local]# tar -zxvf mysql-5.7.25-linux-glibc2.12-x86_64.tar.gz
mysql-5.7.25-lin
...
mysql-5.7.25-linux-glibc2.12-x86_64/share/install_rewriter.sql
mysql-5.7.25-linux-glibc2.12-x86_64/share/uninstall_rewriter.sql
mysql-5.7.25-linux-glibc2.12-x86_64/support-files/magic
mysql-5.7.25-linux-glibc2.12-x86_64/support-files/mysql.server
mysql-5.7.25-linux-glibc2.12-x86_64/docs/INFO_BIN
mysql-5.7.25-linux-glibc2.12-x86_64/docs/INFO_SRC
[root@CDH-141 local]# ls
bin  full-path-to-mysql-VERSION-OS  include  lib64    mysql-5.7.25-linux-glibc2.12-x86_64.tar.gz  share
etc  games                          lib      libexec  mysql-5.7.25-linux-glibc2.12-x86_64  sbin                                        src
# 进入/usr/local下，修改为mysql
[root@CDH-141 local]# mv mysql-5.7.25-linux-glibc2.12-x86_64 mysql
[root@CDH-141 local]# ls
bin  etc  full-path-to-mysql-VERSION-OS  games  include  lib  lib64  libexec  mysql  mysql-5.7.25-linux-glibc2.12-x86_64.tar.gz  sbin  share  src
```

## 六、更改所属的组和用户

```shell
# 更改所属的组和用户
[root@CDH-141 ~]# cd /usr/local/
[root@CDH-141 local]# chown -R mysql mysql/
[root@CDH-141 local]# chgrp -R mysql mysql/
[root@CDH-141 local]# cd mysql/
[root@CDH-141 mysql]# mkdir data
[root@CDH-141 mysql]# chown -R mysql:mysql data
```

## 七、在/etc下创建my.cnf文件

```shell
# 进入/usr/local/mysql文件夹下
[root@CDH-141 ~]# cd /usr/local/mysql
# 创建my.cnf文件
[root@CDH-141 mysql]# touch my.cnf #或者cd ''>my.conf
# 编辑my.cnf
[root@CDH-141 mysql]# vi my.conf
[mysql]
socket=/var/lib/mysql/mysql.sock
# set mysql client default chararter
default-character-set=utf8

[mysqld]
socket=/var/lib/mysql/mysql.sock
# set mysql server port  
port = 3323 #默认是3306，这里发现3306已经被占用，因此防止这种情况发生，可以避免使用3306mysql默认端口
# set mysql install base dir
basedir=/usr/local/mysql
# set the data store dir
datadir=/usr/local/mysql/data
# set the number of allow max connnection
max_connections=200
# set server charactre default encoding
character-set-server=utf8
# the storage engine
default-storage-engine=INNODB
lower_case_table_names=1
max_allowed_packet=16M
explicit_defaults_for_timestamp=true

[mysql.server]
user=mysql
basedir=/usr/local/mysql
[root@CDH-141 mysql]# 
```

## 八、进入mysql文件夹，并安装mysql

```shell
# 进入mysql
[root@CDH-141 local]# cd /usr/local/mysql
# 安装mysql
[root@CDH-141 mysql]# bin/mysql_install_db --user=mysql --basedir=/usr/local/mysql/ --datadir=/usr/local/mysql/data/
2019-03-08 18:11:07 [WARNING] mysql_install_db is deprecated. Please consider switching to mysqld --initialize
2019-03-08 18:11:24 [WARNING] The bootstrap log isn't empty:
2019-03-08 18:11:24 [WARNING] 2019-03-08T10:11:07.208602Z 0 [Warning] --bootstrap is deprecated. Please consider using --initialize instead
```

设置文件及目录权限：

```shell
[root@CDH-141 mysql]# cp ./support-files/mysql.server /etc/init.d/mysqld
[root@CDH-141 mysql]# chown 777 my.cnf
[root@CDH-141 mysql]# ls
bin  COPYING  data  docs  include  lib  man  my.cnf  README  share  support-files
[root@CDH-141 mysql]# ls -l
total 60
drwxr-xr-x  2 root  root   4096 Mar  8 15:56 bin
-rw-r--r--  1  7161 31415 17987 Dec 21 18:39 COPYING
drwxr-x---  5 mysql mysql  4096 Mar  8 16:21 data
drwxr-xr-x  2 root  root   4096 Mar  8 15:56 docs
drwxr-xr-x  3 root  root   4096 Mar  8 15:56 include
drwxr-xr-x  5 root  root   4096 Mar  8 15:56 lib
drwxr-xr-x  4 root  root   4096 Mar  8 15:56 man
-rw-r--r--  1   777 root    516 Mar  8 16:19 my.cnf
-rw-r--r--  1  7161 31415  2478 Dec 21 18:39 README
drwxr-xr-x 28 root  root   4096 Mar  8 15:56 share
drwxr-xr-x  2 root  root   4096 Mar  8 15:56 support-files
[root@CDH-141 mysql]# chmod +x /etc/init.d/mysqld
[root@CDH-141 mysql]#
[root@CDH-141 mysql]# mkdir data
[root@CDH-141 mysql]#
[root@CDH-141 mysql]# chown -R mysql:mysql data
[root@CDH-141 mysql]# 
```

## 九、启动mysql

```shell
# 启动mysql
[root@CDH-141 mysql]# /etc/init.d/mysqld restart
MySQL server PID file could not be found![FAILED]
Starting MySQL.Logging to '/usr/local/mysql/data/CDH-141.err'.
..The server quit without updating PID file (/usr/local/mysql/data/CDH-141.pid).[FAILED]
[root@CDH-141 mysql]# 
```

出现错误，解决方案如下：

```shell
#找到是否已经有进程占用
[root@CDH-141 mysql]# ps aux|grep mysql
root     32483  0.0  0.0 113252  1620 pts/0    S    18:04   0:00 /bin/sh /usr/local/mysql/bin/mysqld_safe --datadir=/usr/local/mysql/data --pid-file=/usr/local/mysql/data/CDH-141.pid
mysql    32684  0.1  0.1 1119892 178224 pts/0  Sl   18:04   0:00 /usr/local/mysql/bin/mysqld --basedir=/usr/local/mysql --datadir=/usr/local/mysql/data --plugin-dir=/usr/local/mysql/lib/plugin --user=mysql --log-error=CDH-141.err --pid-file=/usr/local/mysql/data/CDH-141.pid --port=3323
root     35137  0.0  0.0 112648   944 pts/0    S+   18:12   0:00 grep --color=auto mysql

#关闭进程
[root@CDH-141 mysql]# kill -9 32684
[root@CDH-141 mysql]# /usr/local/mysql/bin/mysqld_safe: line 198: 32684 Killed                  nohup /usr/local/mysql/bin/mysqld --basedir=/usr/local/mysql --datadir=/usr/local/mysql/data --plugin-dir=/usr/local/mysql/lib/plugin --user=mysql --log-error=CDH-141.err --pid-file=/usr/local/mysql/data/CDH-141.pid --port=3323 < /dev/null > /dev/null 2>&1
#确认是否还占用
[root@CDH-141 mysql]#  ps aux|grep mysql
root     35501  0.0  0.0 112644   948 pts/0    S+   18:13   0:00 grep --color=auto mysql
[root@CDH-141 mysql]# /etc/init.d/mysqld restart
MySQL server PID file could not be found![FAILED]
Starting MySQL..[  OK  ]
[root@CDH-141 mysql]#

# 重启mysql
[root@CDH-141 mysql]# /etc/init.d/mysqld restart
Shutting down MySQL..[  OK  ]
Starting MySQL..[  OK  ]
[root@CDH-141 mysql]# 
```

## 十、设置开机启动

```shell
#设置开机启动
[root@CDH-141 mysql]# chkconfig --level 35 mysqld on
[root@CDH-141 mysql]# chkconfig --list mysqld

Note: This output shows SysV services only and does not include native
      systemd services. SysV configuration data might be overridden by native
      systemd configuration.

      If you want to list systemd services use 'systemctl list-unit-files'.
      To see services enabled on particular target use
      'systemctl list-dependencies [target]'.

mysqld          0:off   1:off   2:on    3:on    4:on    5:on    6:off
[root@CDH-141 mysql]# chmod +x /etc/rc.d/init.d/mysqld
[root@CDH-141 mysql]# chkconfig --add mysqld
[root@CDH-141 mysql]# chkconfig --list mysqld

Note: This output shows SysV services only and does not include native
      systemd services. SysV configuration data might be overridden by native
      systemd configuration.

      If you want to list systemd services use 'systemctl list-unit-files'.
      To see services enabled on particular target use
      'systemctl list-dependencies [target]'.

mysqld          0:off   1:off   2:on    3:on    4:on    5:on    6:off
[root@CDH-141 mysql]# service mysqld status
MySQL running (26122)[  OK  ]
[root@CDH-141 mysql]# 
```

## 十一、修改配置文件

```shell
# 进入/etc/profile文件夹
[root@CDH-141 mysql]# vim /etc/profile
修改/etc/profile，在最后添加如下内容
# 修改/etc/profile文件
#set mysql environment
export PATH=$PATH:/usr/local/mysql/bin
# 使文件生效
[root@CDH-141 mysql]# source /etc/profile
```

## 十二、获得mysql初始密码

 1）获得mysql初始密码

```shell
[root@CDH-141 mysql]#  cat /root/.mysql_secret  
# Password set for user 'root@localhost' at 2019-03-08 17:40:42
poc3u0mO_luv
[root@CDH-141 mysql]# 
```

2）修改密码

```shell
[root@CDH-141 mysql]# mysql -uroot -p
Enter password: #此处填写上边获取到的初始密码‘poc3u0mO_luv’
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 2
Server version: 5.7.25

Copyright (c) 2000, 2015, Oracle and/or its affiliates. All rights reserved.
Oracle is a registered trademark of Oracle Corporation and/or its affiliates. Other names may be trademarks of their respective owners.
Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql>  set PASSWORD = PASSWORD('123456');
Query OK, 0 rows affected, 1 warning (0.00 sec)

mysql> flush privileges;
Query OK, 0 rows affected (0.00 sec)

mysql> exit
Bye
```

3）验证新密码是否登录成功：

```shell
[root@CDH-141 mysql]# mysql -uroot -p
Enter password: #此处输入新密码‘123456’
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 4
Server version: 5.7.25 MySQL Community Server (GPL)

Copyright (c) 2000, 2015, Oracle and/or its affiliates. All rights reserved.
Oracle is a registered trademark of Oracle Corporation and/or its affiliates. Other names may be trademarks of their respective owners.
Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql> show tables;
ERROR 1046 (3D000): No database selected
mysql> show databases;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| mysql              |
| performance_schema |
| sys                |
+--------------------+
4 rows in set (0.00 sec)

mysql> 
```

## 十三、添加远程访问权限

```shell
# 添加远程访问权限
mysql> use mysql
Reading table information for completion of table and column names
You can turn off this feature to get a quicker startup with -A

Database changed
mysql> update user set host='%' where user='root';
Query OK, 1 row affected (0.00 sec)
Rows matched: 1  Changed: 1  Warnings: 0

mysql> select host,user from user;
+-----------+---------------+
| host      | user          |
+-----------+---------------+
| %         | root          |
| localhost | mysql.session |
| localhost | mysql.sys     |
+-----------+---------------+
3 rows in set (0.00 sec)

mysql> 
```

## 十四、重启mysql生效

```shell
# 重启mysql
[root@CDH-141 mysql]# /etc/init.d/mysqld restart
Shutting down MySQL..[  OK  ]
Starting MySQL..[  OK  ]
[root@CDH-141 mysql]# 
```