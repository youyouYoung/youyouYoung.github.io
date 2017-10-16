---
layout: post
title:  awk句法格式
date:   2017-10-16 17:28:45
categories: Shell
expert: awk使用学习
---

* content
{:toc}

## 介绍

`awk`是一种解释性语言, 它不同于编译型语言. 是由数据驱动运行的. 而编译型语言是由编译器编译为机器语言进而由机器执行.

使用`awk`可以完成对文本数据的检索, 统计, 格式化等功能.

以下内容是对*GNU*下的`gawk`文档学习过程中的一些知识的记录.

---

## 详情

---

### 作用介绍

`awk`可以根据处理程序(自定义的处理方式)对文本信息进行处理.

* 这个处理方式由一个个的规则`(rule)`组成. 每个规则具有两个部分:**条件(pattern)**和**行为(action)**组成. 每个规则可以省略其中任意一个部分, 但不能两个都省略. 

如果条件被省略, 表示行为部分将对每一条记录进行处理. 如果行为被省略, 表示输出符合条件的记录内容. 

* 文本信息既可以通过在`awk`命令后面添加参数的方式实现, 也可以通过标准输入的方式实现(在命令后面不跟任何文件).

### 运行方式

根据如何调用处理程序, 可以通过两种方式运行`awk`.

* 命令行一次性调用

```
awk 'program' input-data1 input-data2 ...

\# 通过使用单引号的方式告诉shell, program是awk命令的一个完整的参数, 不需要shell对其中的内容进行解释或者转化.
\# input-dataN 表示需要使用program处理的文本文件. 如果没有这个参数则表示文本文件来自于标准输入.
```

* 使用source-file的方式调用

```
awk -f source-file input-data1 input-data2 ...

\# source-file中写有awk的表达式, 内容和上面一种的program一致. 这不过这种方式常用于处理程序内容较多且会被重复使用的情况下.
```

---

## 参考文件

---

* **(The GNU Awk User’s Guide)[http://www.gnu.org/software/gawk/manual/html_node/index.html#SEC_Contents]**
