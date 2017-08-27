---
layout: post
title:  使用VirtualBox安装Centos7虚拟机
date:   2017-08-27 13:58:56
categories: Centos
expert: 学习ambari部署应用Spark时, 需要模拟一个Centos集群. 每次创建都忘记方法, 这里记录一下.
---

* content
{:toc}

## 前置条件

---

* VirtualBox Version 5.1.26. 如图:

![VirtualBox信息图]({{ site.baseurl }}/file/VirtualBox_Info.png)

* Centos.iso 建议使用DVD版本, 大小适中已安装软件够用. 可以从[北京理工大学开源软件镜像服务](http://mirror.bit.edu.cn/web/)下载

* 如果VirtualBox不支持安装64位操作系统, 需要在计算机启动时进入BIOS将**虚拟化技术设置为可用**. 可参考[链接](http://www.askmaclean.com/archives/vbox-virtualbox-64-bit.html). 不同的BIOS可能位置不同.


---

## 参考文献

---

* **[VirtualBox没有64位选项，无法安装64位的解决方法](http://www.askmaclean.com/archives/vbox-virtualbox-64-bit.html)**
* **[VirtualBox安装CentOS 7虚拟机(集群)](http://blog.csdn.net/napoay/article/details/69675732)**
* **[CentOS 7 下，如何设置DNS服务器](http://blog.csdn.net/cheng_fangang/article/details/42676783)**

