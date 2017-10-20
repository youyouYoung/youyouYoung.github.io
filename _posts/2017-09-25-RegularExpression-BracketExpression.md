---
layout: post
title:  方括号表达式中可以使用的表达式
date:   2017-09-25 14:35:15
categories: Regex
expert: 正则表达式的元字符中有一个'[...]', 可以用来匹配任意出现在'['和之间个任何记录']'
---

* content
{:toc}

## 概述

---

`[...]`格式的正则表达式被称作bracket expression. 功能在于可以匹配任何在[,]括号之间的字符.

为了方便定义需要匹配的字符集, 正则表达式中专门多种可以在`[]`中使用的字符集.

---

## 详情

---

### 范围表达式

范围表达式包含两个字符并且之间被连字符连接. 范围表达式匹配任意的在这两个字符之间的字符, 所以要求范围表达中的两个字符必须时已排序的, 至于这个范围中到底有什么字符是根据运行系统的编码方式决定的.

例如: `[0-9]`等价于`[0123456789]`.

随着多字节编码方式的使用(一开始使用的ASCII是通过一个字节对字符进行编码的. 即他最多只能对2^8=256个字符进行编码, 远远不能达到我们一直的字符数量. 所以产生了多字节编码方式例如unicode系列), 我们更多的是使用八进制或者十六进制去表示字符范围, 这样的匹配范围就会大于256个字符.

### 字符集

字符集只能在括号表达式中使用, 并且字符集需要被`[:`和`:]`包含在中间. 常使用的字符集如下:

字符集		|	含义
[:alnum:]	|	所有的字母和数字
[:alpah:]	|	所有的字母
[:blank:]	|	空格符和tab
[:cntrl:]	|	控制符号
[:digit:]	|	所有的数字
[:graph:]	|	既可以被打印出来又可以显示的字符.(例如:空格符可以被打印但是不显示,字符a符合要求)
[:lower:]	|	所有的小写字符
[:print:]	|	所有可以被打印出来的字符.(即非控制字符)
[:punct:]	|	标点符号.(即那些不是字母,数字,控制字符,空白字符的字符)
[:space:]	|	空白字符.(例如:空格符,换行,换页,tab等等)
[:upper:]	|	所有的大写字符
[:xdigit:]	|	十六进制数字

字符集使用的编码格式, 源自当前运行系统提供的编码方式. 如果不使用字符集的话我们可以通过`[a-z0-9A-Z]`来匹配所有的字母和数字, 如果当前系统使用的编码方式用还有其他的字母的话上面的表达式就无法匹配到了. 但是如果使用字符集`[:alnum:]`就可以匹配到当前编码方式中所有的字母和数字.

因为ASCII使用一个字节进行编码, 所以如果要匹配所有在ASCII中进行编码的字符可以使用字符集`[\x00-\x7f]`, 因为在POSIX的字符集中将范围在0-127之间的字符指定为ASCII中的字符. 那么也可以使用`[^\x00-\x7f]`表示所有当前编码中不属于ASCII的部分.

---

## 小结

---

* 如果要在括号中使用正则表达式的特殊字符('\','.','*',']'等)需要转义.

* 另外还有两种特殊的字符序列可以放在括号表达式中, 这些会被用在没有ASCII编码的地方. 在这些序列中可以使用一个字符代表一个或者多个字符, 同时也可能有多个字符可以等价与当前的表达式字符. 例如'e'和'è'等价.

---

## 参考文献

---

* **[Using Bracket Expressions](http://www.gnu.org/software/gawk/manual/html_node/Bracket-Expressions.html#Bracket-Expressions)**