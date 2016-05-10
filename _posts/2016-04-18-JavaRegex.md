---
layout: post
title:  Java正则表达式
date:   2016-04-18 10:21:56
categories: Java Regex
excerpt: Java正则表达式的学习
---

* content
{:toc}

## 简介
---

JDK 4.0之后Java开始支持正则表达式, 有关正则表达式的包是`java.util.regex`.  

包中主要有三个类:   

* `Pattern`: 用来表达和陈述搜索的模式. `Pattern`类没有公共的构造方法需要使用该类提供的静态方法`compile(String regex)`来创建一个`Pattern`对象.   

* `Matcher`: 真正影响搜索的对象. `Matcher`对象是对输入字符串进行解释和匹配的引擎. 该类也没有公共的构造方法需要使用`Pattern`类的`matcher()`方法获得对象.   

* `PatternSyntaxException`: 是一个非强制异常类, 它表示正则表达式语法出错.   

**示例**   

典型的正则表达式调用顺序   

``` java

Pattern p = Pattern.compile("a*b");
Matcher m = Pattern.matcher("aaaab");
boolean b = m.matches();

```

如果仅使用一次正则表达式时, 可以直接使用`Pattern`类中的匹配方法:   

``` java

boolean b = Pattern.matches("a*b", "aaaab");

```

由于下面的示例不能重复使用已编译好的模式(`Pattern b`)所以效率不高.   

## 正则表达式语法
---


## 参考文献
---

* **[Java 正则表达式](http://www.runoob.com/java/java-regular-expressions.html)**   

* **[JAVA 正则表达式 （超详细）](http://smallwoniu.blog.51cto.com/3911954/1324133)**   

