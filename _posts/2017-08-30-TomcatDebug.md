---
layout: post
title:  Tomcat开启远程调试
date:   2017-08-30 15:54:47
categories: Tomcat
expert: 调试对于编码而言, 具有极大地作用
---

* content
{:toc}

## 起因 

---

调试是编码过程中不可缺少的一部分. 对于部署在`Tomcat`上的Web应用, 使用调试功能往往能事半功倍.

---

## 方法

---

在Tomcat安装路径下的bin文件夹中查找以`catalina`开头的可执行文件, 根据系统的不同选择不同的文件打开.

在文件的首个有效行添加允许单步调试的命令. 不同的操作系统使用的命令不同.

**其中`address`参数用来规定远程调试端口**

### Centos

在`catalina.sh`中添加: 

```
declare -x CATALINA_OPTS="-Xdebug -Xrunjdwp:transport=dt_socket,server=y,suspend=n,address=8010"
```

### Ubuntu

在`catalina.sh`中添加: 

```
CATALINA_OPTS="-Xdebug  -Xrunjdwp:transport=dt_socket,address=8100,server=y,suspend=n"
```

### 

在`catalina.bat`中添加: 

```
Set  “CATALINA_OPTS=-Xdebug  -Xrunjdwp:transport=dt_socket,address=8100,server=y,suspend=n"
```

在以上的三个配置中, debug端口都设置为8010.

---

## 参考文件

---

* **[linux下tomcat开启远程调试](http://blog.csdn.net/dfdsggdgg/article/details/50730311)**
