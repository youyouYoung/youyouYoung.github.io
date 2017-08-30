---
layout: post
title:  查看MySQL表字段详情
date:   2017-08-30 17:51:54
categories: MySQL
expert: 无法使用图形化界面时需要用到
---

* content
{:toc}

## 概述

---

在没有图形化界面的情况下, 使用sql查看MySQL数据库中表的字段详情.

**注意以下命令的使用仅限于MySQL, 其他数据库中使用可能会出错.**

---

## 方法

---

为了演示方便, 在此创建一张表:
```
create table test (
    id int auto_increment primary key,
    name varchar(100) not null,
    gender varchar(10) not null,
    age int,
    link varchar(255)
);

```

**以下操作全部针对于上面创建的表test.**

### 方式一

```
show fields from test;
```

### 方式二

```
desc test;
```

通过上面两种方法都可以得到`test`表中各字段的详情.

---

## 参考文献

---

* **[mysql 查看表结构的命令](http://www.itpub.net/thread-510556-1-1.html)**
