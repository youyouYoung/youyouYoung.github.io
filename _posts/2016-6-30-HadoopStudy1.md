---
layout: post
title:  "hadoop的安装以及伪分布配置"
date:   2016-6-30 12:06:44
categories: Server Hadoop
excerpt: hadoop的学习和使用
---

* content
{: toc}

## Hadoop介绍

`Hadoop`是一个分布式系统的基础架构. 从这句话中我们可以得到:

1. **分布式**. 分布式的作用在于它可以以更高的效率来处理更大的数据量.

2. **系统**. 能作为一个系统必须具备两点要求: **数据存储能力**, **运算能力**.

3. **基础架构**. 要做成一个适用于大部分人群的系统, 必定需要添加很多功能同时也导致系统过于庞大. 那么基础架构的特点就在于该系统只提供了数据操作有关的功能, 更上层的建筑就需要使用者自己来完成了.

因此, `Hadoop`提供了两点功能: `分布式文件系统`, `文件操作功能`. Hadoop Distributed File System(HDFS)是Hadoop提供的分布式文件系统, MapReduce则是基于文件系统上的数据计算功能.

## 预配置

* `Java`安装和`JAVA_HOME`环境变量的配置. `Hadoop`是使用Java语言编写的系统, 所以需要JDK的支持在使用之前我们需要配置好Java环境, 最主要的是JAVA_HOME的配置.

* 安装软件`SSH Server`. 集群式分布需要`SSH`来建立各节点间的连接. 使用`sudo apt-get install openssh-server`即可.

### 实现:

* Java环境的配置

在这里我建议最好自己手动配置环境变量, 即我们手动从Oracle官网上下载JDK的压缩包. 这里我选择下载`jdk-8u91-linux-x64.tar.gz`完成后使用`tar -xzf jdk-8u91-linux-x64.tar.gz -C ~/bin`将压缩包中的内容解压到home下的bin目录中.

接下来配置Java开发环境:

``` shell

vim ~/.bashrc
#或者 sudo vim /etc/profile

JAVA_HOME=/home/user/bin/jdk1.8.0_91
CLASSPATH=.:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar
PATH=$PATH:$JAVA_HOME/bin
export JAVA_HOME
export CLASSPATH
export PATH

# 保存文件
. ~/.bashrc

```

* SSH Server安装

``` shell

sudo apt-get install openssh-server

ssh-keygen -t rsa

cp ~/.ssh/id_rsa.pub ~/.ssh/authorized_keys

```

### 检测正确性:

1. 命令行运行`echo $JAVA_HOME`, 如果得到JDK的安装路径(不包括bin目录)则成功.

2. 命令行运行`ssh localhost`, 如果正常连接则配置成功.

## Hadoop安装和配置

### Hadoop下载安装

可以使用[北理工源](http://mirror.bit.edu.cn/apache/hadoop/common/)下载Hadoop. 我下载了`hadoop-2.7.1.tar.gz`版本. 解压到任意目录下这里我使用`/usr/local/`路径.

``` shell
sudo tar -xzf hadoop-2.7.1.tar.gz -C /usr/local/
cd /usr/local
sudo mv hadoop-2.7.1 hadoop
cd hadoop
```

### Hadoop单机(非分布式)配置验证

Hadoop安装完成之后默采用非分布式模式, 无需再进行其他配置. 我们可以直接使用其中的例子进行测试.

``` shell
cd /usr/local/hadoop
mkdir ./input
cp ./etc/hadoop/*.xml ./input   # 将配置文件作为输入文件
./bin/hadoop jar ./share/hadoop/mapreduce/hadoop-mapreduce-examples-2.7.1.jar grep ./input ./output 'dfs[a-z.]+'
ls ./output
```

### hadoop伪分布的配置

Hadoop的所有配置文件位于`/usr/local/hadoop/etc/hadoop`下, 在配置伪分布时我们需要配置`core-site.xml`,`hdfs-site.xml`,`hadoop-env.sh`文件.

> core-site.xml的配置
> 
> #将其中的<configuration></configuration>用一下内容代替:  
> <configuration>  
>         <property>  
>              <name>hadoop.tmp.dir</name>  
>              <value>file:/usr/local/hadoop/tmp</value>  
>              <description>Abase for other temporary directories.</description>  
>         </property>  
>         <property>  
>              <name>fs.defaultFS</name>  
>              <value>hdfs://localhost:9000</value>  
>         </property>  
> </configuration>  

> hdfs-site.xml的配置  
> 
> #将其中的<configuration></configuration>用一下内容代替:   
> <configuration>  
>         <property>  
>              <name>dfs.replication</name>  
>              <value>1</value>  
>         </property>  
>         <property>  
>              <name>dfs.namenode.name.dir</name>  
>              <value>file:/usr/local/hadoop/tmp/dfs/name</value>  
>         </property>  
>         <property>  
>              <name>dfs.datanode.data.dir</name>  
>              <value>file:/usr/local/hadoop/tmp/dfs/data</value>  
>         </property>    
> </configuration>  

> hadoop-env.sh的配置  
> 
> #将JAVA_HOME=*改为  
>
> JAVA_HOME=/home/user/bin/jdk1.8.91  

接下来执行`NameNode`的格式化:

``` shell
/usr/local/hadoop/bin/hdfs namenode -format
```

开启`NameNode`和`DataNode`守护进程

``` shell
/usr/local/hadoop/sbin/start-dfs.sh
```