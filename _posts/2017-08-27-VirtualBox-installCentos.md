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

## 创建虚拟机

---

1. 新建虚拟机, 指定为Centos系统. 这里Type:Linux Version:Red Hat(64-bit)

![centos_step0]({{ site.baseurl }}/file/create_centos_step0.png)

2. 设置内存大小. 这里量力而行.

![centos_step1]({{ site.baseurl }}/file/create_centos_step1.png)

3. 创建磁盘空间, 选择现在创建.

![centos_step2]({{ site.baseurl }}/file/create_centos_step2.png)

4. 选择文件类型, 默认即可.

![centos_step3]({{ site.baseurl }}/file/create_centos_step3.png)

5. 虚拟机磁盘在真实磁盘上的占用方式, 我选择固定大小是为了运行流畅性考虑.

![centos_step4]({{ site.baseurl }}/file/create_centos_step4.png)

6. 分配磁盘大小, 量力而行即可.

![centos_step5]({{ site.baseurl }}/file/create_centos_step5.png)

完成以上六步即可创建一个新的虚拟机.

---

## 首次开机前配置

---

上面的操作我们已经给虚拟机分配了内存和硬盘, 接下来开始安装系统. 点击刚创建的虚拟机右键进入设置界面.

1. **System**界面, 启动顺序处取消软盘选项, 并将光驱和硬盘上移.

![setting_centos_step0]({{ site.baseurl }}/file/setting_centos_step0.png)

2. **Storage**界面, 光驱添加我们下载好的`centos.iso`镜像文件. 

![setting_centos_step1]({{ site.baseurl }}/file/setting_centos_step1.png)

3. **Network**界面, **只启动第一个网络适配器, 通过Attached to选择NAT模式, 其余默认即可**. 千万不要使用其他方式. 

![setting_centos_step2]({{ site.baseurl }}/file/setting_centos_step2.png)

点击保存按钮, 以上我们设置好了初次启动前的必要配置. 准备开机安装系统.

---

## 安装系统

---

下面开始安装 `Centos 7`

1. 选择直接安装, 敲击回车后等待系统程序自检.

![install_centos_step0]({{ site.baseurl }}/file/install_centos_step0.png)

2. 选择语言, 这里使用英语. 执行下一步. 

![install_centos_step1]({{ site.baseurl }}/file/install_centos_step1.png)

3. 首先选择时区(DATE & TIME), 地图上点击中国即可.

![install_centos_step2]({{ site.baseurl }}/file/install_centos_step2.png)

4. 配置网络(NETWORK & HOST NAME), **这里一定要将 Ethernet 选择为开机启动.**(10.0.2.2 10.0.2.3)

![install_centos_step3]({{ site.baseurl }}/file/install_centos_step3.png)
![install_centos_step4]({{ site.baseurl }}/file/install_centos_step4.png)

5. 选择安装位置(INSTALLATION DESTINATION). 这里不用做其他选择, 只是为了确认一下而已. 

![install_centos_step5]({{ site.baseurl }}/file/install_centos_step5.png)

6. 点击开始安装, 在这个界面中为root用户创建密码. 点击确认即可.

![install_centos_step6]({{ site.baseurl }}/file/install_centos_step6.png)

以上为整个安装过程, 等待安装结束后, 可以使用`root`账户和最后一步设置的密码登录系统了.

---

## 配置网络

---

通过上面的安装过程, 可以完成centos的安装, 但是仍然存在问题. 主机无法访问虚拟机因为在配置虚拟机时我们使用了NAT网络模式. 

这里需要将NAT模式转为桥接模式, 并位Centos 7配置静态IP.

1. 安装`net-tools`

```shell
yum -y install net-tools
```

![network_centos_step0]({{ site.baseurl }}/file/network_centos_step0.png)

2. `ifconfig`查看网卡信息

![network_centos_step1]({{ site.baseurl }}/file/network_centos_step1.png)

上图中看出系统具有一个`enp0s3`的网卡.

3. 编辑网卡信息

``` shell
vi /etc/sysconfig/network-scripts/ifcfg-enp0s3
```

将`BOOTPROTO`改为`static`. 并添加IP信息, 全部文件内容为:

```
TYPE="Ethernet"
BOOTPROTO="static"
DEFROUTE="yes"
PEERDNS="yes"
PEERROUTES="yes"
IPV4_FAILURE_FATAL="no"
IPV6INIT="yes"
IPV6_AUTOCONF="yes"
IPV6_DEFROUTE="yes"
IPV6_PEERDNS="yes"
IPV6_PEERROUTES="yes"
IPV6_FAILURE_FATAL="no"
IPV6_ADDR_GEN_MODE="stable-privacy"
NAME="enp0s3"
UUID="bf754dd0-bcb1-4a94-ba65-fbd8bcd577d7"
DEVICE="enp0s3"
ONBOOT="yes"

IPADDR=192.168.99.203
GATEWAY=192.168.99.1
NM_CONTROLLED=no
NETMASK=255.255.255.0
```

4. 关闭虚拟机, 将网络设置为桥接模式.

![network_centos_step3]({{ site.baseurl }}/file/network_centos_step3.png)

5. 启动虚拟机, 使用`ping`即可检查到主机和虚拟机之间的访问了. 但目前可能还没办法解析域名. 

所以我们通过`ping www.baidu.com`, 仍然无法访问百度. 所以最后一部我们需要配置DNS服务器地址.

* 修改文件`/etc/NetworkManager/NetworkManager.conf`, 在`main`部分添加`dns=none`

![network_centos_step4]({{ site.baseurl }}/file/network_centos_step4.png)

* 编辑文件`/etc/resolv.conf`, 添加

``` shell
nameserver 223.5.5.5
nameserver 8.8.8.8
search localdomain
```

* 重启网路服务即可.

---

## 参考文献

---

* **[VirtualBox没有64位选项，无法安装64位的解决方法](http://www.askmaclean.com/archives/vbox-virtualbox-64-bit.html)**
* **[VirtualBox安装CentOS 7虚拟机(集群)](http://blog.csdn.net/napoay/article/details/69675732)**
* **[CentOS 7 下，如何设置DNS服务器](http://blog.csdn.net/cheng_fangang/article/details/42676783)**

