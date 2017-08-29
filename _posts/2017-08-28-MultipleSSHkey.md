---
layout: post
title:  多SSH Key连接不同服务器 - ssh
date:   2017-08-28 14:11:50
categories: Shell
expert: 连接github和本地主机使用的Key不一样, 需要通过配置才能在不同的域下使用不同的SSH Key.
---

* content
{:toc}

## 起因

---

连接Linux服务器为了方便起见, 使用 `ssh-keygen -t rsa`创建了一个已**当前用户名**作为用户的SSH Key. 但是因为又要使用`git`做版本控制用户名不得不规定为git, 同时备注登录邮箱信息. 为了区别于之前创建的Key, 使用以下命令创建:

```shell
gitkeygen -t rsa -C "username@email.com" -f ~/.ssh/id_rsa_email

mv .ssh/id_rsa .ssh/id_rsa_common	##重命名之前的SSH Key私钥和公钥
mv .ssh/id_rsa.pub .ssh/id_rsa_common.pub
```

问题在于对于已经上传了之前SSH Key公钥的服务器, 再次登录时产生了混乱, 主要原因在于我在上一步重命名了原来的SSH Key.

如果想保存上面的状态继续使用的话, 我们可以通过**创建config文件**来告诉`ssh`, 该使用哪个私钥连接服务器.

```
vim ~/.ssh/config
```

---

## config格式

---

config文件中配置有所有可以通过`ssh`命令连接的主机. 每一个主机的信息由一个`Host`关键字以及其下面的**属性关键字**组成. 格式如下:

>> Host 自定义名称 	##ssh命令会通过这个名称去查询相关属性
>>	HostName 服务器地址	##非必填, 用于指定自定义名称的服务器IP地址或域名
>>	User 连接用户名		##非必填, 等价于命令行输入的方式
>>	Post 服务器SSH端口	##给必填, 默认22
>>	IdentifyFile /root/.ssh/id_rsa_common	##私钥详细位置

这里我只介绍自己用到的几个关键字.

1. `Host`对应的值使我们以后在`ssh`命令后指定的连接到的服务器标识
2. `HostName`的值表示该Host对应的地址
3. `User`连接用户名
4. `Post`目的服务器的ssh端口
5. `IndentifyFile`私钥的绝对路径

下面有一个示例:
```
Host server100
	HostName 192.168.0.100
	User root
	Post 2258
	IndentifyFile ~/.ssh/id_rsa_common

Host Git
	HostName github.com
	User git
	IndentifyFile ~/.ssh/id_rsa_email

Host localhost
	User username
	IndentifyFile ~/.ssh/id_rsa_common
```

根据上面的配置文件, 我们使用`ssh server100`与使用`ssh root@server100`是一样的效果

---

## 参考文献

---

* **[ssh-config配置](http://zlong.org/2015/06/08/ssh-config/)**
* **[使用SSH CONFIG](https://www.lainme.com/doku.php/blog/2011/02/%E4%BD%BF%E7%94%A8ssh_config)**

