---
layout: post
title:  awk的printf函数中格式化控制字符的含义
date:   2017-10-06 16:59:52
categories: Shell
expert: 学习awk的输出时, 记录关于printf中控制字符的含义
---

* content
{:toc}

## 概述

---

`awk`中有两种常见的打印方式:`print`,`printf`. 通过这两种方式我们可以将目标内容输出, 但是`print`函数在输出形式上的控制要比`printf`函数复杂很多甚至有时候实现不了我们想要的显示方式. 为此我们可能需要使用`printf`函数去输出目标内容. 

`printf`函数的格式为: `printf 格式化字符串, 输出项1, 输出项2, ...`, 其中格式化字符串用来规定怎么去输出后面的这些输出项的.

`printf`函数通过格式化字符去指定输出项的类型. 对于每一个**格式化说明符**都是开始于`%`截止于一个格式化控制字符. 这些用于告诉`printf`函数输出什么怎么输出. 其中格式化控制字符用于告诉函数输出内容的值类型(例如: 数字, 字符串等), 说明符中剩余的内容告诉函数怎么输出这个值(例如: 规定宽度等).

---

## 详情

---

### %c

用于将一个数字作为字符输出, 例如`printf "%c", 65`将会输出字符'A'. 如果输出项是一个字符串的话将会输出字符串的第一个字符.

### %d, %i

用于输出一个十进制数字, 这两个控制字符是等价的. (`%i`专门用于兼容ISO C标准的).

### %e, %E

使用科学计数法打印一个数字. 例如`printf "%4.3e", 1950`将会打印出结果'1.950e+03', 其中4代表4位有效数字, 3表示结果需要有三个小数位.

如果使用`%E`的话会将结果中中的'e'改为'E'.

### %f

使用浮点型格式打印一个数字. 例如`printf "%4.3f", 1950`将会打印出结果'1950.000', 其中4代表4位有效数字, 3表示小数点后3位小数.

在支持IEEE 754标准的系统中, 可以使用`-inf`, `-infinity`表示负无穷大. 或者使用`inf`, `infinity`表示正无穷大.

### %g, %G

使用浮点格式或者科学计数法格式打印一个数值, 取决于哪种占用的字符数量最少. `%G`会使用`%E`而不是`%e`.

### %o

打印一个八进制数值.

### %s

打印一个字符串.

### %u

打印一个无符号的十进制数值.

### %x, %X

打印十六进制的数值, 其中`%x`使用`a-f`而`%X`使用`A-F`.

### %%

打印一个'%'.

---

## 参考文献

---

* **[5.5.2 Format-Control Letters](http://www.gnu.org/software/gawk/manual/html_node/Control-Letters.html#Control-Letters)**
