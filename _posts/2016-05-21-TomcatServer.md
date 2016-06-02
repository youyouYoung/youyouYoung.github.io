---
layout: post
title:  Tomcat服务器配置
date:   2016-05-21 14:23:48
categories: Server Tomcat
expert: Tomcat服务器的配置
---

* content
{:toc}

## Tomcat for Ubuntu

### 安装

Ubuntu上安装tomcat服务器非常简单, 只需要运行一条命令即可, 系统会帮你完全安装tomcat以及所需的依赖. 

**注意: 如果没有安装JDK, 请先安装JDK**

``` shell
sudo apt-get install tomcat7
```

如果你想完全安装tomcat, 包括docs,examples,admin等功能. 建议:

``` shell
sudo apt-get install tomcat7 tomcat7-*
```

### 配置

如果你使用系统命令安装的话, 一般不需要再进行其他配置. 当然如果你想通过页面管理tomcat(你需要访问`manager webapp`, `host-manager webapp`的话), 还需要配置`/etc/tomcat7/tomcat-users.xml`, 具体做法如下:

模型情况下, tomcat安装完成并不会创建一个具有以上两种权限的用户, 所以需要我们手动添加:

``` xml
<!--
首先需要打开/etc/tomcat7/tomcat-users.xml.
向其中添加:
-->
<role rolename="manager-gui"/>
<role rolename="admin-gui"/>
<user username="demo" password="demo" roles="manager-gui,admin-gui"/>
```

### 重要文件位置

* **CATALINA_HOME = /usr/share/tomcat7**

* **CATALINA_BASE = /var/lib/tomcat7**

## 部署Web项目

Web项目的部署方式有两种: 静态部署和动态部署.

### 静态部署

静态部署是指需要在服务器运行之前部署文件, 所有使用这种部署方式每次在部署完成后需要重启tomcat服务器.

#### Tomcat自动部署

将我们写好的Web项目放在tomcat的`$CATALINA_HOME/webapps`路径下. Ubuntu自动安装的Tomcat 中`$CATALINA_HOME/webapps`处于:`/var/lib/tomcat7/webapps/`位置.

``` shell
\# 假设web项目名称为demo/test.jsp
cp -r demo/ /var/lib/tomcat7/webapps/
```

访问localhost:8080/demo/test.jsp即可.

#### 修改server.xml文件

使用此方式没必要将Web项目移动到tomcat目录下.但是需要更改tomcat的配置文件: `server.xml`.

`server.xml`文件位置: `$CATALINA_HOME/conf/server.xml`

添加一下内容: 

``` xml
<Host>
  ...
  <Context path ="/demo" reloadable ="false" docBase ="/root/Public/demo" workDir ="/tmp/demo"  />
</Host>

<!--
path: 网络路径,访问时的根地址, 表示访问的路径. 如:localhost/demo/test.jsp

reloadable: 表示可以在运行时在classes与lib文件夹下自动加载类包。其中reloadable="false"表示当应用程序 中的内容发生更改之后服务器不会自动加载，这个属性在开发阶段通常都设为true，方便开发，在发布阶段应该设置为false，提高应用程序的访问速度。

docbase: Web文件的实际位置, 可以时相对位置或者绝对位置.

workDir: 放置缓存文件的位置
-->
```

访问localhost:8080/demo/test.jsp即可.

#### 自定义web部署文件(推荐)

在`$CATALINA_HOME/conf/Catalina /localhost`目录下创建一个任意的`xml`文件. 将配置信息写入该文件中, 配置信息类似于在`server.xml`中添加的那行, 或者可以复制这个路径下的一个`xml`文件的内容到你新创建的文件中.

``` shell
\# root
sudo su

\# 进入$CATALINA_HOME/conf/Catalina /localhost
cd /var/lib/tomcat7/conf/Catalina/localhost

\# 复制内容到新创建的demo.xml中
cat docs.xml > demo.xml

\# 修改demo.xml中的内容
path="/docs"  ->  path="/demo"
docBase="/usr/share/tomcat7-docs/docs"  ->  docBase="/root/Public/demo"
```

访问localhost:8080/demo/test.jsp即可.

### 动态部署

动态部署是指可以在服务器启动之后部署web应用程序, 而不用重新启动服务器. 如果想使用动态部署, 需要完全安装Tomcat(主要包括`tomcat7-admin`)同时配置`tomcat-users.xml`文件, 具体配置请看安装部分.

设置完用户之后, 打开`manager webapp`网页(localhost:8080/manager/html). 如下图:
![manager webapp]({{ site.baseurl }}/css/pics/2016-05-21-TomcatServer.png)

`Context Path (required)`对应<Context>中的`path`, 可填:/test

`XML Configuration file URL`添加一个xml文件, 类似于在`$CATALINA_HOME/conf/Catalina /localhost`中创建的xml文件, 可空或者创建一个xml传入其位置.

`WAR or Directory URL`对应<Context>中的`docBase`, 可填/root/Public/demo

点击确定即可

## 常见问题解决

* **文件存在却无法访问**

常常会出现一个文件明明存在, 但是tomcat一直报404错误. 这可能和你要访问的那个网页权限有关. 修改方法是为该网页文件添加其他用户可读属性.

``` shell
chmod o+r [文件名称]
```

## 参考文献

* **[Ubuntu 中文](https://wiki.ubuntu.org.cn/Tomcat)**

* **[Tomcat部署Web应用方法总结](http://blog.csdn.net/yangxueyong/article/details/6130065)**
