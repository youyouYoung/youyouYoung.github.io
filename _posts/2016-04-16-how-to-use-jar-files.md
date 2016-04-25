---
layout: post
title:  "jar包的使用方法"
date:   2016-04-16 10:13:45
categories: Java jar
excerpt: 拿到一个jar包,我该怎么用
---

* content
{:toc}

## 起因
Java项目中一般都需要导入jar包,有时候我们也需要使用可以执行的jar包完成一些任务,比如:burp suite.  

所以我们经常需要去查找如何使用jar包,比如**查看其中的类**,**运行jar文件**等.

## 查看jar中的类

*.jar是一种压缩文件,使用jar方式压缩.我们可以在linux命令行下使用jar命令查看其中内容,如果你是用过tar, zip的话.这个不难理解:  
        jar -xvf *.jar  

>选项	|	含义  
>x	|	解压缩jar文件  
>v	|	显示其中内容  
>f	|	指定需要处理的文件名  

## 运行jar文件

jar时java类的打包格式,所以我们使用java命令就可以执行jar文件.  
        java -jar *.jar  
