---
title: groovy语言
date: 2021-9-3
comments: true
categories: 编程语言
tags: groovy
---


# Groovy初探

学习groovy的笔记

## groovy语法

GroovyYuFa.groovy

```groovy
package btbullet.yufa



/**
 * <p>
 *
 * </p>
 *
 * @description:
 * @author: chengqj
 * @date: Created in 2021/7/30 0:02
 * @copyright: Copyright (c) 2021
 * @version: V1.0* @modified:
 */
/**
     与Java相比,Groovy的不同或优势
     1. Groovy完全兼容Java语法，也可以作为脚本执行
     2. 分号是可选的，一般不加分号，可换行作为结束
     3. 类，方法，字段，都是公共的，没有访问权限限制
     4. 默认生成具名（名值对）参数构造器 key:value
     5. 字段不定义访问权限的时候，编译器自动给字段添加getter/setter方法
     6. 字段可以使用点来获取,无访问权限的也可以使用getter/setter来操作
     7. 方法可以省略return关键字,自动检索最后一行的结果作为返回值
     8. 空值比较不会有NullPointerException
<!-- more -->
     Groovy的高级特性
     1. assert断言:可以使用assert代替之前java的断言语句
     2. 可选类型: 可使用类JavaScript的弱类型,可使用 def 来表示任意类型
     3. 方法调用: 调用带参方法时可以省略括号
     4. 字符串定义: 字符串定义有三种方式,单引号,双引号,三个三引号
     5. 几何API: 集合的定义和使用更加简单,API和Java有所不同,但是兼容Java API
     6. 闭包: Groovy的一大特性,跟方法类似的代码块,可以赋给一个变量,也可以作为一个参数传递给一个方法,像普通
     方法一样调用

 */
println "============ 基本语法 ============="
// 变量声明
def name = "小明"
age = 10 // def 也可以省略

// 调用带参数的方法时可以省略括号
println name + ":" + age

// 无空指针
age = null
println age.equals(null)

// 断言
//assert age == 19

println "============ 字符串定义 ============="
str1 = '普通字符串'   // 普通字符串
str2 = "双引号可以应用变量${str1}" // 可以引用变量
str3 = '''可以按格式
          定义字符串''';  // 按格式定义字符串
println str1
println str2
println str3

println "============ 集合定义 ============="
// List集合,使用[] 来声明集合
def list = ['sdf','sdf','123']
// 添加
list.add('小明明')
list << "test"
println list;
println list.getClass()

// map 定义 映射,使用[key:value] 方式定义
map = ['one':'小明','two':'小红']
// 给map赋值
map.put('email','xiaoming@tete.com')
// 使用点key的方式来赋值
map.gender = "key"
println map
println map.getClass()

println "============ 闭包定义 ============="
def c1 = {
    println "不带参数的闭包"
}
def c2 = {
    val ->
        println "带参数的闭包:${val}"
}

// 定义方法形参是指定类型,接受不能带参数的闭包方法
def method1(Closure closure){
    closure()
//    closure.call()
}

// 定义方法形参无指定类型,接受带参数闭包的方法
def method2(c){
    c'willie'
//    c.call('willie')
}

method1 c1
method2 c2

// 调用方法时直接定义新的闭包作为实际参数
method1 {
    println '小梦'
}
method2 {
    val -> println "闭包:${val}"
}

println "========= test =========="

method1 method2 {
    v -> println "cc"
}
```

## groovy与java bean

创建的时候new的是Groovy Class

Student

```groovy
package btbullet.javabean

/**
 * <p>
 *
 * </p>
 *
 * @description:
 * @author: chengqj
 * @date: Created in 2021/7/31 15:25
 * @copyright: Copyright (c) 2021
 * @version: V1.0* @modified:
 */

/*
与java bean不同的地方
 1) 可省略分号
 2) 可省略 getter/setter
 3) 可省略 return
 4) 无权限修饰符自动生成 getter/setter
 5) 默认带有具名构造器
 */


// 不用权限修饰符,默认使用public
class Student {
    private String username // 可省略分号
    private String email;  // 省略getter setter
    int age; // 无权限修饰的时候,默认生成getter setter

    String getUsername(){
        username;  // 可省略return
    }

    void setUsername(String username){
        this.username = username;
    }
}
```

Student2

```groovy
package btbullet.javabean

/**
 * <p>
 *
 * </p>
 *
 * @description:
 * @author: chengqj
 * @date: Created in 2021/7/31 15:45
 * @version: V1.0
 */

/**
 * 定义可以不指定变量类型
 */

 class Student2 {
     def username
     def email
     def age

 }
```

App.groovy

```groovy
package btbullet.javabean
/**
 * <p>
 *
 * </p>
 *
 * @description:
 * @author: chengqj
 * @date: Created in 2021/7/31 15:33
 * @version: V1.0
 */

// 使用脚本的方式书写代码

Student student = new Student()

// 调用getter/setter
student.setUsername("xiaoming")
println student.getUsername()

// 使用点的方式来赋值和获取字段值
student.email = "xiaoming@test.com"
println student.email

// 调用无权限修饰符的字段的getter/setter
student.setAge(10)
println student.getAge()

// 调用默认带有具名构造器 key:value
Student student1 = new Student(username: "小东", email: "xiaodong@test.com")
println student1.username
```

