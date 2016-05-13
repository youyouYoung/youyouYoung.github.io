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

### 字符
---

| 构造		| 匹配	|  
| x		| 字符x	|  
| \\\\		| 反斜线	|  
| \\0n		| 八进制值0n (0<=n<=7)	|  
| \\0nn		| 八进制值0nn (0<=n<=7)	|  
| \\0mnn	| 八进制值0mnn (0<=m<=3, 0<=n<=7)	|  
| \\xhh		| 16进制值0xhh (0<=h<=f)	|  
| \\uhhhh	| 16进制值0xhhhh (0<=h<=f)	|  
| \\x{h...h}	| 16进制值0xh...h (0<=h<=f)	|  
| \\t		| 制表符	|  
| \\n		| 换行符	|  
| \\r		| 回车符	|  

### 字符类
---

| 构造		| 匹配	|  
| \[abc\]	| a,b或c	|  
| \[^abc\]	| 除了a,b和c的任何字符 |  
| \[a-zA-Z\]	| a到z ∪ A到Z	|  
| \[a-d\[m-p\]\]| a到d ∩ m到p	|  
| \[a-z&&\[def\]\]	| d,e或f(交集)	|  
| \[a-z&&\[^bc\]\]	| a-z,除了bc. 等同于\[ad-z\]	|   
| \[a-z\[^m-p\]\]	| a-z,而非m-p. 等同于\[a-lq-z\]	|   

### 预定义字符类
---

| 构造	| 匹配		|  
| .	| 任何字符	|  
| \\d	| 数字, 等同于\[0-9\]	|  
| \\D	| 非数字, 等同于\[^0-9\]	|  
| \\s	| 空白字符, 等同于\[\\t,\\n,\\f,\\r\]	|  
| \\S	| 非空白字符	|  
| \\w	| 单词字符	|  
| \\W	| 非单词字符	|  

### 边界匹配器
---

| 构造	| 匹配		|  
| ^	| 行的开头	|  
| $	| 行的结尾	|  
| \\b	| 单词的边界	|  
| \\B	| 非单词的边界	|  
| \\A	| 输入的开头	|  
| \\G	| 上一个匹配的结尾	|  
| \\Z	| 输入的结尾	|  
| \\z	| 输入的结尾	|  

### Greedy数量词
---

| 构造		| 匹配			|  
| X?		| X,零次或一次		|  
| X*		| X,零次或多次		|  
| X+		| X,一次或多次		|  
| X{n}		| X,恰好n次		|  
| X{n,}		| X,至少n次		|  
| X{n,m}	| X,至少n次,最多m次	|  

### Logical运算符
---

| 构造	| 匹配		|  
| XY	| XY		|  
| X|Y	| X或Y		|  
| (X)	| X作为捕获组	|  

### 非捕获组
---

| 构造	| 匹配		|  
| (?:X)	| 只匹配X,但不作为捕获组	|  

### 其他语法
---

| 构造		| 匹配		|  
| ?		| 当此字符紧随在其他限定符之后, 表示匹配模式是非贪心的. 非贪心模式搜索到的是尽可能短的匹配字符串. 如用"o+?"匹配"oooo"的结果是"o", "o+"结果是"oooo"	|  

## 参考文献
---

* **[Java 正则表达式](http://www.runoob.com/java/java-regular-expressions.html)**   

* **[JAVA 正则表达式 （超详细）](http://smallwoniu.blog.51cto.com/3911954/1324133)**   

