---
title: redis基础
date: 2021-5-5
comments: true
categories: 中间件
tags: redis基础
---

redis基础
===


### redis可以用来做什么

1.缓存

2.分布式锁

3.分布式锁

4.分布式id

5.消息队列

### redis的基础数据结构

#### String

唯一key字符串 对应 唯一value字符串

key，value需要序列化

redis字符串是 动态字符串 ，分配采用 冗余空间 以减少内存频繁分配（字符串长度小于 1m，每次加倍分配，超过则每次增加 1m）

字符串最大长度512m

<!--more-->

**命令**

get key

set key value

exists 返回 1存在  0不存在

del key

**批量操作**

mget key1 key2 key3

mset key1 123 key2 321 key3 134

**过期和扩展**

set key value
expire key 5

setnx key value

setex key expireTime value

**计数**






延时队列

功能 
    添加延时
    
    循环


