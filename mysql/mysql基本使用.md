---
title: MySQL基本使用
date: 2021-5-5
comments: true
categories: DB
tags: mysql
---

MySQL基本使用
===

### 数据库选择与创建

**数据库查看**

 `show databases`

 **数据库选择**

 `use dbname`

 **数据库创建与删除**

 创建     
 `create database dbname`

 删除     
 `drop database dbname`


### 数据库存储引擎和数据类型

**查看支持的存储引擎**

`show engines`

还可以通过`show variables like 'have%'`，来查看支持的引擎

**查看默认引擎**

<!--more-->


**修改默认引擎**

图形化修改

手动修改：修改配置文件 my.ini -> 重启mysql

### 数据类型

**整数类型**

`TINYINT`：1字节 有符号范围 -128~127 无符号：0~255

`SMALLINT`：2字节 有符号范围 -32768~32767 无符号：0~65535

`MEDIUMINT`：3字节 有符号范围 -8388608~8388607 无符号：0~1677215

`INT,INTEGER`：4字节 有符号范围 -2147483648~2147483647 无符号：0~4294967295

`BIGINT`：8字节 有符号范围：-9223372036854775808~9223372036854775807 无符号：0~18446744073709551615


**浮点类型，定点数据类型和位类型**

`FLOAT`: 4字节

`DOUBLE`: 8字节

**定点数类型**

`DEC(M,D)`或者`DECIMAL(M,D)`：和DOUBLE相同，但是小数精度可以更高，例如：DEC(38,30),相对于DOUBLE(38,30)，小数精度不会丢失。

**位类型**

`BIT(M)`: 最小值：BIT(1) 最大值：BIT(64)

**日期类型**

| 日期和时间类型  | 字节  | 最小值  | 最大值 |
|---|---|---|---|
|  DATE |  4 | 1000-01-01  |  9999-12-31 |
|  DATETIME | 8  |  1000-01-01 00:00:00 | 9999-12-31 23:59:59  |
|  TIMESTAMP | 4  | 1970010180001  | 2038年的某一个时刻  |
|  TIME |  3 |  -835:59:59 | 835:59:59  |
|  YEAR | 1  | 1901  | 2155  |

**CHAR字符串类型**

| CHAR系列字符串类型  |  字节 | 描述  |
|---|---|---|
|  CHAR(M) | M  | M 0~255  |
|  VARCHAR(M) | M  |  M 0~65536 |

**TEXT字符串类型**

|  TEXT系列字符串类型 | 字节  | 描述  |
|---|---|---|
| TINYTEXT | 0-255  |   |
|  TEXT |  0-65535 |   |
|  MEDIUMTEXT | 0-167772150  |   |
|  LONGTEXT |  0-4294967295 |   |

**BINARY系列字符串类型**

|  BINARY系列字符串类型 |  字节 | 描述  |
|---|---|---|
| BINARY(M)  | M  | 允许长度0-M  |
| VARBINARY  | M  |  允许长度0-M |

可储存音乐，图片，视频

BLOB字符串类型

| BLOB系列字符串类型  |   |
|---|---|
| TINYBLOB  |  0-255 |
| BLOB  |  0-2^16 |
| MEDIUMBLOB | 0-2^24   |
| LONGBLOB  |  0-2^32 |

# MySQL常用函数

### 字符串

--合并字符串

`select concat('my','s','ql') from dual;`

-- 带分隔符

 `select concat_ws('-','java','python','go') from dual;` -- 如果分隔符是NULL，则合并完位NULL

 -- 比较字符串大小

 `select STRCMP('ABC','ABD') FROM DUAL;` -- -1

 `SELECT STRCMP('ABC','ABC') FROM DUAL;` -- 0

 `SELECT STRCMP('ABC','ABB') FROM DUAL;` -- 1

 -- 长度

 `SELECT LENGTH('MYSLQ测试') FROM DUAL;` -- 字节长度

 `SELECT CHAR_LENGTH('MYSQL测试') FROM DUAL;` -- 字符长度

 -- 大小写转换

 `SELECT UPPER('mySQL'),UCASE('mySQL') FROM DUAL;`

 `SELECT LOWER('mySQL'),LCASE('mySQL') FROM DUAL;`

 -- 查找字符串

 `SELECT FIND_IN_SET('mysql','oracle,mysql, sql server') FROM DUAL;` -- 第二个参数以逗号隔开

 -- 返回指定字符串位置

 `SELECT FIELD('MYSQL','ORACLE','SQL SERVER','MYSQL') FROM DUAL;`

 -- 返回子串匹配的开始位置

 `SELECT LOCATE('SQL','MySQL'),POSITION('SQL' IN 'MySQL'),INSTR('MySQL','SQL') FROM DUAL;`

 -- 返回指定位置的字符串

`SELECT ELT(1,'QWE','ER','MNB') FROM DUAL;`

 -- 选择字符串的MAKE_SET()

 `SELECT BIN(5) 二进制数, MAKE_SET(5,'mysql','ORACLE','SQL SERVER','ASDF') 选取后的字符串 , BIN(7) 二进制数, MAKE_SET(7,'MYSQL','ORACLE','SQL SERVER','ASDF') 选取后的字符串 FROM DUAL;`

 -- 截取字符串

 `SELECT LEFT('QWERTYUI',3),RIGHT('QWERTYUI',3) FROM DUAL;`

 `SELECT SUBSTR('ORACLEMYSQL',3,6),MID('ORACLEMYSQL',3,6) FROM DUAL;`

 -- 去除空格

 `SELECT LTRIM('  ASDF  '), RTRIM('  asdf  '),TRIM('  ASDF  ') FROM DUAL;`

 -- 替换字符串

 `SELECT INSERT('这是要替换的mysql，测试测试',7,5,'ORACLE') FROM DUAL;` -- 起始位置，长度；任何一个参数位null，则返回null

 `SELECT REPLACE('这是要替换的mysql，测试测试','mysql','oracle') FROM DUAL;`

### 数值函数

-- 绝对值

SELECT ABS(-123) FROM DUAL;

SELECT CEIL(123.321)，CEILING(123.321) FROM DUAL; -- 124 两个函数的作用好像木有区别

SELECT FLOOR(123.321) FROM DUAL; -- 123 

-- 取余

SELECT MOD(123,4) FROM DUAL;

-- 随机数

SELECT RAND(),RAND(),RAND(3),RAND(3) FROM DUAL; -- RAND(x)返回的是固定的随机数，RAND()每次都不一样

-- 四舍五入

SELECT ROUND(145.1233567,5),ROUND(145.1233567,-1) FROM DUAL; -- 1.12336 -- 150

-- 截取小数位

SELECT TRUNCATE(123.12343123,3),TRUNCATE(123.12343123,-1) FROM DUAL;

### 日期和时间函数


-- 获取时间 日期

SELECT NOW() now方式, CURRENT_TIMESTAMP() timestamp方式,LOCALTIME() localtime方式,SYSDATE() systemdate方式 FROM DUAL;
-- 2020-01-31 05:04:46 		2020-01-31 05:04:46 	2020-01-31 05:04:46 	2020-01-31 05:04:46

-- 获取日期

SELECT CURDATE() curdate方式,CURRENT_DATE() current_date方式 FROM DUAL; -- 2020-01-31	2020-01-31

-- 获取时间

SELECT CURTIME() curtime方式, CURRENT_TIME() current_time方式; -- 05:10:25	05:10:25

-- unix方式显示时间

SELECT NOW() 当前时间, UNIX_TIMESTAMP(NOW()) unix格式, UNIX_TIMESTAMP() unix无参格式, FROM_UNIXTIME(UNIX_TIMESTAMP(NOW())) 普通格式;

-- UTC方式显示时间 日期

SELECT NOW() 当前日期时间, UTC_DATE() UTC日期, UTC_TIME() UTC时间;



SELECT 
NOW() 当前时间日期,
YEAR(NOW()) 年,
QUARTER(NOW()) 季度,
MONTH(NOW()) 月,
WEEK(NOW()) 星期,
DAYOFMONTH(NOW()) 天月,
HOUR(NOW()) 小时,
MINUTE(NOW()) 分,
SECOND(NOW()) 秒;


SELECT MONTHNAME(NOW()) 月份名 ;

-- 关于星期

SELECT NOW() 当前日期时间,
WEEK(NOW()) "年中的第几个星期1-53",
WEEKOFYEAR(NOW()) "年中的第几个星期1-53",
DAYNAME(NOW()) 星期的英文名,
DAYOFWEEK(NOW()) "1-7",
WEEKDAY(NOW()) "0-6";

-- 关于天的函数啊

SELECT NOW() 当前时间日期,
DAYOFYEAR(NOW()) 年中第几天,
DAYOFMONTH(NOW()) 月中第几天;

-- 获取指定EXTRACT()函数

SELECT NOW() 当前日期时间,
EXTRACT(YEAR FROM NOW()) 年,
EXTRACT(MONTH FROM NOW()) 月,
EXTRACT(DAY FROM NOW()) 天,
EXTRACT(HOUR FROM NOW()) 小时,
EXTRACT(MINUTE FROM NOW()) 分钟,
EXTRACT(SECOND FROM NOW()) 秒;

-- 与默认时间（0000年1月1日）计算

SELECT NOW() 当前日期时间,
TO_DAYS(NOW()) 与默认时间相隔天数,
FROM_DAYS(TO_DAYS(NOW())) 一段时间后日期和时间;

-- 两个时间点之间相隔天数

SELECT NOW() 当前日期时间,DATEDIFF(NOW(),'2000-12-01') 相隔天数;

-- 与指定时间日期操作

SELECT CURDATE() 当前日期, ADDDATE(CURDATE(),5) '5天后日期',SUBDATE(CURDATE(),5) '5天前日期';

另一种计算
| type  | 含义  | expr  |
|---|---|---|
| YEAR | 年  | YY  |
|  MONTH | 月  | MM  |
| DAY  | 天  |  DD |
|  HOUR |  小时 | hh  |
|  MINUTE |  分钟 | mm  |
|  SECOND |  秒 |  ss |
|  YEAR_MONTH | 年和月  | YY,MM  任意符号隔开 |
|  DAY_HOUR |  日和小时 | DD,hh 任意字符隔开  |
|  DAY_MINUTE |  日和分钟 |  DD,mm 任意字符隔开 |
|  DAY_SECOND |  日和秒 |  DD,ss 任意字符隔开 |
|  HOUR_MINUTE |  小时和分钟 | hh,mm 任意字符隔开  |
|  HOUR_SECOND |  小时和秒 | hh,ss 任意字符隔开  |
|  MINUTE_SECOND |  分钟和秒 | mm,ss 任意字符隔开  |

SELECT CURDATE() 当前日期,
ADDDATE(CURDATE(),INTERVAL '2,3' YEAR_MONTH) '2年3个月后的日期',
SUBDATE(CURDATE(),INTERVAL '2,3' YEAR_MONTH) '2年3个月前的日期';

-- 按秒计算日期

SELECT CURTIME() 当前时间,
ADDTIME(CURTIME(),10) '10秒后时间',
SUBTIME(CURTIME(),10) '10秒前时间';

### 系统参数

SELECT 
VERSION() 数据库版本,
DATABASE() 当前数据库的名字,
USER() 当前用户,
LAST_INSERT_ID() '最近生成AUTO_INCREMENT值';

-- 最近一个自增长的id是多少

SELECT LAST_INSERT_ID();

### 其他函数

**流程函数**

| 函数  | 作用  |
|---|---|
| IF(value,t f)  | 如果value为真，返回t，否则返回f  |
|  IFNULL(value1,value2) | 如果 value1不为空，则返回value1，如果为空，则返回value2 |
|   |   |
|   |   |

SELECT IF(id=3 , '123','321') from t_video where id = 4; 

SELECT IFNULL(class,'无') FROM t_video WHERE ID >2;



# Mysql查看帮助文档

**`HELP contents； --选择项--> HELP '<item>'`选项一定要加单引号。**

**如：`HELP contents --> HELP 'Data Types' --> HELP 'INT'`**



