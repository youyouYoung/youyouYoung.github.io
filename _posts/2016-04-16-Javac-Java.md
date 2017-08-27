---
layout: post
title:  java,javac,jar的用法
date:   2016-04-16 10:13:45
categories: Java
excerpt: 拿到一个jar包,我该怎么用. 怎编译需要第三方jar的项目
---

* content
{:toc}

## 起因
---

Java项目中一般都需要导入`jar`包,有时候我们也需要使用可以执行的`jar`包完成一些任务,比如:`burp suite`.  

所以我们经常需要去查找如何使用`jar`包,比如**查看其中的类**,**运行jar文件**等.  

也有时候一个项目中引用了第三方`jar`包, 我们在运行时需要设置`classpath`.  

## 查看jar中的类
---

`*.jar`是一种压缩文件,使用`jar`方式压缩.我们可以在linux命令行下使用`jar`命令查看其中内容,如果你是用过`tar`, `zip`的话.这个不难理解:  

``` shell
jar -xvf *.jar
```

| 选项	|	含义			|  
| x	|	解压缩jar文件		|  
| v	|	显示其中内容		|  
| f	|	指定需要处理的文件名	|  

## 运行jar文件
---

`jar`时java类的打包格式,所以我们使用java命令就可以执行`jar`文件.   

``` shell
java -jar *.jar
```

## 引用第三方jar包
---

1. 运行项目  

``` shell
java -cp .:[jar位置] [class名称]
```

**示例**  

``` shell
java -cp .:../lib/collection/mysql-connector-java-5.0.5-bin.jar collection.crawler.CrawlerTest
```
