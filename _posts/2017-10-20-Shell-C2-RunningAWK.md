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

**-F fs**  
**\-\-field-separator fs**

用于这是field的分隔符.

**-f source-file**  
**\-\-file source-file**

从指定文件中获取`awk`的处理程序. 这个选项可以如果被使用多次, 表示将每个原文件的处理程序组合为一个大的组合程序.

**-v var=val**  
**\-\-assign var=val**

用于定义一个变量`var`并赋值为`val`. `-v`后面只能为一个变量赋值, 如果想给其他变量赋值可以在使用一个`-v`选项. 同时如果对一个变量多次赋值, 会以最后一次的值为变量的实际值.

这里定义的变量内容可以在`BEGIN`模式中被使用, 但是不要使用该选项对内置变量进行赋值.

**\-\-**

用于标识已经到了命令行选项的结尾, 后面在出现以`-`开头的字符不会被认为是选项.

这个选项会被用在待处理的数据文件名中包含`-`

*以下选项只适用于gawk*

**-d[file]**  
**\-\-dump-variables[=file]**

将全局变量的`类型`, `最终值`打印到文件中. 如果文件`file`没有被指定, 默认输出到当前路径下的`awkvars.out`文件中. 注意*file变量和-d参数之间不能有空格*.

**-D[file]**  
**\-\-debug[=file]**

打开调试模式. 如果不指定`file`文件, 调试器将会以交互形式从标准输入读取命令. 或者可以通过指定file文件让调试器从文件中获取执行命令.

**-i source-file**  
**\-\-include source-file**

类似于`-f`, 用于指定`awk`的执行程序所在文件.

**-l ext**  
**\-\-load ext**

获取一个动态扩展文件`ext`. 扩展文件是使用编译语言`C`或者`C++`实现程序文件. 这个文件一般用于对`awk`功能的扩展.

**-L**  
**\-\-lint**

对于程序中可能会出现错误的代码或者不可以移植到其他awk版本使用的代码发出警告.

**-p[file]**  
**\-\-profile[=file]**

启动对`awk`程序的分析. 该选项会产生一个文件, 文件中标有每段`代码`, `代码块`, `表达式`被执行的次数. 详细介绍请看[这里](http://www.gnu.org/software/gawk/manual/html_node/Profiling.html#Profiling).

如果没有指定`file`文件的话, 将会在当前路径下生一个名为`awkprof.out`的文件. 如果指定`file`那么`file`与`-p`之间不能有空格.

**-S**  
**\-\-sandbox**

用于禁止awk程序使用`system()`方法, 禁止输入/输出重定向. 这个选项一般用在运行安全性未知的awk程序上面.

---

## 命令行参数

非`awk`选项以及选项值的其他参数会被`awk`默认为待处理数据文件. 将会按照文件顺序一一被执行, 但是以`var=val`形式表达的参数会被认为是对`var`变量的赋值, 不会被认为是文件名称.

```
awk -f program.awk file1 count=1 file2
```

上面的示例中`awk`会将file1, file2视为文件, 同时设置变量`count`的值为`1`. 注意: 这种方式定义的变量不能被`BEGIN`模式所识别, 同时变量被赋值为指定值是在程序处理完赋值语句之前的待处理数据文件之后进行的. 

所有在`awk`命令行上出现的参数, 除了命令行选项和执行程序内容以外, 都会被保存在`ARGV`数组中. 

变量赋值是在赋值语句之前的待处理数据已经被执行完成之后进行的, 这一点非常有用. 我们可以根据这点去对不同的数据文件指定不同`FS`, `RS`, `OFS`等文件特定的属性值.

当然也可以控制同一个文件多次被不同的程序处理. 例如:

```
awk ' 	pass == 1 { action1 }
	pass == 2 { action2 }
' pass=1 datafile pass=2 datafile
```

---

## 标准输入和文件混合作为待处理数据

可能会遇到以下情况, 需要使用`awk`即处理文件中的内容又处理标准输入中的内容. 这种情况下我们需要标准输入和文件名共同作为`awk`的参数.

我们可以使用`-`代表标准输入. 例如:

```
some_program | awk -f awk_program.awk file1 - file2
```

上面示例中我们使用`awk_program.awk`程序对file1, 标准输入, file2进行处理.

---

## 环境变量

环境变量的多个值之间使用`:`分隔. 可以使用空字符串或者`.`表示当前目录. 例如:

```
# 以下的三种方式都是为环境变量PATH赋值为: 当前文件夹/folder1/folder2
PATH=.:folder1:folder2
PATH=:folder1:folder2
PATH=folder::folder2
```

**AWKPATH**

用于指定`awk`程序文件搜索路径. 我们使用`-f program-file`选项指定一个处理程序, 一般情况下`awk`程序会在当前目录下查找文件名称为`program-file`的文件, 如果没有找到则在`AWKPATH`中指定的路径下查找文件.

**AWKLIBPATH**

用于指定`awk`扩展程序文件的搜索路径. 扩展程序是由`C`, `C++`编译成的程序文件.

使用这些环境变量, 可以定义多个目录用于管理不同功能的`awk`程序. 然后将一个个的小程序组合为整个程序去处理特定的文件.

---

## 处理程序间的调用

将`awk`程序做成一个个简短且功能唯一的小程序, 通过相互引用来实现更重要的功能, 这样就可以提高代码的复用.

可以使用`@include`方式在一个程序中引用其他的程序. 注意**该功能时gawk的特性.**

file1用于获取文件中数字的个数:
```
BEGIN { FS="" }

{
	for (i=1; i<NF; i++) {
		if ($i >= '0' && $i <= '9') sum++;
	}
}
```

file2用于输出数字的总数:
```
@include "file1"

END { print sum }
```
使用`awk -f file2 data`执行可获得data文件中数字的个数.

`@include`可以实现内嵌, 比如file3引用了file2, 执行file3会同时执行file2和file1.

---

## 参看文献

* **[The GNU Awk User’s Guide](http://www.gnu.org/software/gawk/manual/html_node/Invoking-Gawk.html#Invoking-Gawk)**
