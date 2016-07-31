---
layout: post
title:  Bash Shell命令学习---alias
date:   2016-7-30 13:06:13
categories: Linux Bash
excerpt: 复习之前学习的Linux命令
---

* content
{: toc}

## 功能用法介绍   

---

### 功能:

用于给一个命令序列起别名.

### 用法: 

alias [option] [name[='value'] ...]

### 示例:

``` shell
# 显示当前已存在的别名命令及原始命令序列
alias

# 同alias
alias -p

# 为一个命令序列起别名
alias lly='ll /home/youyou'
```

---  

## 常用参数   

---

| option | detail |   
| -p | 列出当前用户已存在的别名命令以及含义 |   

---   

## 参考文献  

---

