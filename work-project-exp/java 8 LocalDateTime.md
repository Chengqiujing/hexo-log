---
title: LocalDateTime和Date区别
date: 2021-4-19
comments: true
categories: 网站
tags: hexo
---



最后来看下在简单日期和时间类中最重要的一个:LocalDataTime。它是LocalDate和LocalTime的组合体，表示的是不带时区的 日期及时间。

看上去，LocalDateTime和Instant很象，但记得的是“Instant中是不带时区的即时时间点。可能有人说，即时的时间点 不就是日期＋时间么？看上去是这样的，但还是有所区别，比如LocalDateTime对于用户来说，可能就只是一个简单的日期和时间的概念，考虑如下的 例子：两个人都在2013年7月2日11点出生，第一个人是在英国出生，而第二个是在加尼福利亚，如果我们问他们是在什么时候出生的话，则他们看上去都是 在同样的时间出生（就是LocalDateTime所表达的），但如果我们根据时间线（如格林威治时间线）去仔细考察，则会发现在出生的人会比在英国出生的人稍微晚几个小时（这就是Instant所表达的概念，并且要将其转换为UTC格式的时间）。除此之外，LocalDateTime 的用法和上述介绍的其他类都很相似