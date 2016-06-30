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

检测正确性:

1. 命令行运行`echo $JAVA_HOME`, 如果得到JDK的安装路径(不包括bin目录)则成功.

2. 命令行运行`ssh localhost`, 如果正常连接则配置成功.

## Hadoop安装和配置

### Hadoop下载安装

可以使用[北理工源](http://mirror.bit.edu.cn/apache/hadoop/common/)下载Hadoop. 