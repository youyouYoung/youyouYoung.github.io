---
layout: post
title:  ʹ��VirtualBox��װCentos7�����
date:   2017-08-27 13:58:56
categories: Centos
expert: ѧϰambari����Ӧ��Sparkʱ, ��Ҫģ��һ��Centos��Ⱥ. ÿ�δ��������Ƿ���, �����¼һ��.
---

* content
{:toc}

## ǰ������

---

* VirtualBox Version 5.1.26. ��ͼ:

![VirtualBox��Ϣͼ]({{ site.baseurl }}/file/VirtualBox_Info.png)

* Centos.iso ����ʹ��DVD�汾, ��С�����Ѱ�װ�������. ���Դ�[��������ѧ��Դ����������](http://mirror.bit.edu.cn/web/)����

* ���VirtualBox��֧�ְ�װ64λ����ϵͳ, ��Ҫ�ڼ��������ʱ����BIOS��**���⻯��������Ϊ����**. �ɲο�[����](http://www.askmaclean.com/archives/vbox-virtualbox-64-bit.html). ��ͬ��BIOS����λ�ò�ͬ.


---

## ���������

---

1. �½������, ָ��ΪCentosϵͳ. ����Type:Linux Version:Red Hat(64-bit)

![centos_step0]({{ site.baseurl }}/file/create_centos_step0.png)

2. �����ڴ��С. ������������.

![centos_step1]({{ site.baseurl }}/file/create_centos_step1.png)

3. �������̿ռ�, ѡ�����ڴ���.

![centos_step2]({{ site.baseurl }}/file/create_centos_step2.png)

4. ѡ���ļ�����, Ĭ�ϼ���.

![centos_step3]({{ site.baseurl }}/file/create_centos_step3.png)

5. �������������ʵ�����ϵ�ռ�÷�ʽ, ��ѡ��̶���С��Ϊ�����������Կ���.

![centos_step4]({{ site.baseurl }}/file/create_centos_step4.png)

6. ������̴�С, �������м���.

![centos_step5]({{ site.baseurl }}/file/create_centos_step5.png)

��������������ɴ���һ���µ������.

---

## �״ο���ǰ����

---

����Ĳ��������Ѿ���������������ڴ��Ӳ��, ��������ʼ��װϵͳ. ����մ�����������Ҽ��������ý���.

1. **System**����, ����˳��ȡ������ѡ��, ����������Ӳ������.

![setting_centos_step0]({{ site.baseurl }}/file/setting_centos_step0.png)

2. **Storage**����, ��������������غõ�`centos.iso`�����ļ�. 

![setting_centos_step1]({{ site.baseurl }}/file/setting_centos_step1.png)

3. **Network**����, **ֻ������һ������������, ͨ��Attached toѡ��NATģʽ, ����Ĭ�ϼ���**. ǧ��Ҫʹ��������ʽ. 

![setting_centos_step2]({{ site.baseurl }}/file/setting_centos_step2.png)

������水ť, �����������ú��˳�������ǰ�ı�Ҫ����. ׼��������װϵͳ.

---

## ��װϵͳ

---

���濪ʼ��װ `Centos 7`

1. ѡ��ֱ�Ӱ�װ, �û��س���ȴ�ϵͳ�����Լ�.

![install_centos_step0]({{ site.baseurl }}/file/install_centos_step0.png)

2. ѡ������, ����ʹ��Ӣ��. ִ����һ��. 

![install_centos_step1]({{ site.baseurl }}/file/install_centos_step1.png)

3. ����ѡ��ʱ��(DATE & TIME), ��ͼ�ϵ���й�����.

![install_centos_step2]({{ site.baseurl }}/file/install_centos_step2.png)

4. ��������(NETWORK & HOST NAME), **����һ��Ҫ�� Ethernet ѡ��Ϊ��������.**(10.0.2.2 10.0.2.3)

![install_centos_step3]({{ site.baseurl }}/file/install_centos_step3.png)
![install_centos_step4]({{ site.baseurl }}/file/install_centos_step4.png)

5. ѡ��װλ��(INSTALLATION DESTINATION). ���ﲻ��������ѡ��, ֻ��Ϊ��ȷ��һ�¶���. 

![install_centos_step5]({{ site.baseurl }}/file/install_centos_step5.png)

6. �����ʼ��װ, �����������Ϊroot�û���������. ���ȷ�ϼ���.

![install_centos_step6]({{ site.baseurl }}/file/install_centos_step6.png)

����Ϊ������װ����, �ȴ���װ������, ����ʹ��`root`�˻������һ�����õ������¼ϵͳ��.

---

## ��������

---

ͨ������İ�װ����, �������centos�İ�װ, ������Ȼ��������. �����޷������������Ϊ�����������ʱ����ʹ����NAT����ģʽ. 

������Ҫ��NATģʽתΪ�Ž�ģʽ, ��λCentos 7���þ�̬IP.

1. ��װ`net-tools`

```shell
yum -y install net-tools
```

![network_centos_step0]({{ site.baseurl }}/file/network_centos_step0.png)

2. `ifconfig`�鿴������Ϣ

![network_centos_step1]({{ site.baseurl }}/file/network_centos_step1.png)

��ͼ�п���ϵͳ����һ��`enp0s3`������.

3. �༭������Ϣ

``` shell
vi /etc/sysconfig/network-scripts/ifcfg-enp0s3
```

��`BOOTPROTO`��Ϊ`static`. �����IP��Ϣ, ȫ���ļ�����Ϊ:

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

4. �ر������, ����������Ϊ�Ž�ģʽ.

![network_centos_step3]({{ site.baseurl }}/file/network_centos_step3.png)

5. ���������, ʹ��`ping`���ɼ�鵽�����������֮��ķ�����. ��Ŀǰ���ܻ�û�취��������. 

��������ͨ��`ping www.baidu.com`, ��Ȼ�޷����ʰٶ�. �������һ��������Ҫ����DNS��������ַ.

* �޸��ļ�`/etc/NetworkManager/NetworkManager.conf`, ��`main`�������`dns=none`

![network_centos_step4]({{ site.baseurl }}/file/network_centos_step4.png)

* �༭�ļ�`/etc/resolv.conf`, ���

``` shell
nameserver 223.5.5.5
nameserver 8.8.8.8
search localdomain
```

* ������·���񼴿�.

---

## �ο�����

---

* **[VirtualBoxû��64λѡ��޷���װ64λ�Ľ������](http://www.askmaclean.com/archives/vbox-virtualbox-64-bit.html)**
* **[VirtualBox��װCentOS 7�����(��Ⱥ)](http://blog.csdn.net/napoay/article/details/69675732)**
* **[CentOS 7 �£��������DNS������](http://blog.csdn.net/cheng_fangang/article/details/42676783)**

