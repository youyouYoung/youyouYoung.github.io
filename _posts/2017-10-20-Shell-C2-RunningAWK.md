---
layout: post
title:  awk学习-第二章-awk和gawk的运行
date:   2017-10-20 14:47:08
categories: Shell
expert: 在GNU上学习awk的使用第二章记录
---

* content
{:toc}

## 概述

这篇文章是对GNU中[gawk帮助文档](http://www.gnu.org/software/gawk/manual/html_node/index.html)第二章的学习总结.

主要有一下几点:

* 常见命令行选项的含义及使用.
* 非选项的命令行参数的含义和使用
* 从标准输入获取待处理数据
* 重要的环境变量的意义
* 多处理程序间的调用

---

## 命令行选项

下面介绍的这些选项中, 不全适用于`awk`. 其中有部分内容是`gawk`专有的, 我会刻意注明是否专属于`gawk`.

其中每个单字符选项都会有一个对应的GNU类型的长选项.

---

**-F fs**  
**\-\-field-separator fs**

用于这是field的分隔符.

**-f source-file**  
**--file source-file**

从指定文件中获取`awk`的处理程序. 这个选项可以如果被使用多次, 表示将每个原文件的处理程序组合为一个大的组合程序.

**-v var=val**  
**assign var=val**

用于定义一个变量`var`并赋值为`val`. `-v`后面只能为一个变量赋值, 如果想给其他变量赋值可以在使用一个`-v`选项. 同时如果对一个变量多次赋值, 会以最后一次的值为变量的实际值.

这里定义的变量内容可以在`BEGIN`模式中被使用, 但是不要使用该选项对内置变量进行赋值.

**\-\-**

用于标识已经到了命令行选项的结尾, 后面在出现以`-`开头的字符不会被认为是选项.

这个选项会被用在待处理的数据文件名中包含`-`

*以下选项只适用于gawk*

**-d[file]**  
**--dump-variables[=file]**

将全局变量的`类型`, `最终值`打印到文件中. 如果文件`file`没有被指定, 默认输出到当前路径下的`awkvars.out`文件中. 注意*file变量和-d参数之间不能有空格*.

**-D[file]**  
**--debug[=file]**

打开调试模式. 如果不指定`file`文件, 调试器将会以交互形式从标准输入读取命令. 或者可以通过指定file文件让调试器从文件中获取执行命令.

**-i source-file**  
**--include source-file**

类似于`-f`, 用于指定`awk`的执行程序所在文件.

**-l ext**  
**--load ext**

获取一个动态扩展文件`ext`. 扩展文件是使用编译语言`C`或者`C++`实现程序文件. 这个文件一般用于对`awk`功能的扩展.

**-L**  
**--lint**

对于程序中可能会出现错误的代码或者不可以移植到其他awk版本使用的代码发出警告.

**-p[file]**  
**-profile[=file]**

启动对`awk`程序的分析. 该选项会产生一个文件, 文件中标有每段`代码`, `代码块`, `表达式`被执行的次数. 详细介绍请看[这里](http://www.gnu.org/software/gawk/manual/html_node/Profiling.html#Profiling).

如果没有指定`file`文件的话, 将会在当前路径下生一个名为`awkprof.out`的文件. 如果指定`file`那么`file`与`-p`之间不能有空格.

**-S**  
**--sandbox**

用于禁止awk程序使用`system()`方法, 禁止输入/输出重定向. 这个选项一般用在运行安全性未知的awk程序上面.

