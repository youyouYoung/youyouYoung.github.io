---
layout: post
title:  ��SSH Key���Ӳ�ͬ������ - ssh
date:   2017-08-28 14:11:50
categories: Shell
expert: ����github�ͱ�������ʹ�õ�Key��һ��, ��Ҫͨ�����ò����ڲ�ͬ������ʹ�ò�ͬ��SSH Key.
---

* content
{:toc}

## ����

---

����Linux������Ϊ�˷������, ʹ�� `ssh-keygen -t rsa`������һ����**��ǰ�û���**��Ϊ�û���SSH Key. ������Ϊ��Ҫʹ��`git`���汾�����û������ò��涨Ϊgit, ͬʱ��ע��¼������Ϣ. Ϊ��������֮ǰ������Key, ʹ�����������:

```shell
gitkeygen -t rsa -C "username@email.com" -f ~/.ssh/id_rsa_email

mv .ssh/id_rsa .ssh/id_rsa_common	##������֮ǰ��SSH Key˽Կ�͹�Կ
mv .ssh/id_rsa.pub .ssh/id_rsa_common.pub
```

�������ڶ����Ѿ��ϴ���֮ǰSSH Key��Կ�ķ�����, �ٴε�¼ʱ�����˻���, ��Ҫԭ������������һ����������ԭ����SSH Key.

����뱣�������״̬����ʹ�õĻ�, ���ǿ���ͨ��**����config�ļ�**������`ssh`, ��ʹ���ĸ�˽Կ���ӷ�����.

```
vim ~/.ssh/config
```

---

## config��ʽ

---

config�ļ������������п���ͨ��`ssh`�������ӵ�����. ÿһ����������Ϣ��һ��`Host`�ؼ����Լ��������**���Թؼ���**���. ��ʽ����:

>> Host �Զ������� 	##ssh�����ͨ���������ȥ��ѯ�������
>>	HostName ��������ַ	##�Ǳ���, ����ָ���Զ������Ƶķ�����IP��ַ������
>>	User �����û���		##�Ǳ���, �ȼ�������������ķ�ʽ
>>	Post ������SSH�˿�	##������, Ĭ��22
>>	IdentifyFile /root/.ssh/id_rsa_common	##˽Կ��ϸλ��

������ֻ�����Լ��õ��ļ����ؼ���.

1. `Host`��Ӧ��ֵʹ�����Ժ���`ssh`�����ָ�������ӵ��ķ�������ʶ
2. `HostName`��ֵ��ʾ��Host��Ӧ�ĵ�ַ
3. `User`�����û���
4. `Post`Ŀ�ķ�������ssh�˿�
5. `IndentifyFile`˽Կ�ľ���·��

������һ��ʾ��:
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

��������������ļ�, ����ʹ��`ssh server100`��ʹ��`ssh root@server100`��һ����Ч��

---

## �ο�����

---

* **[ssh-config����](http://zlong.org/2015/06/08/ssh-config/)**
* **[ʹ��SSH CONFIG](https://www.lainme.com/doku.php/blog/2011/02/%E4%BD%BF%E7%94%A8ssh_config)**

