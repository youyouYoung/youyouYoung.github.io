---
layout: post
title:  JQuery学习笔记(八)---获取内容和属性
date:   2016-8-15 12:46:13
categories: JQuery
excerpt: 关于jquery获取DOM对象的内容和属性方法的学习
---

* content
{: toc}

## 功能介绍

---

**DOM**(Document Object Model): 文档对象模型.

我们可以使用JQuery获取选择器(selector)指定对象的"内容"或者"属性".

---

## 实现方法

---

### 获取内容

获取内容的方式有三种: 

1. `text()` - 获取选定元素的文本内容

2. `html()` - 获取选定元素的HTML内容.

3. `val()` - 获取选定元素的value属性的内容.

### 获取属性

我们通过`$(selector).attr(attribute)`方法来获取选定元素的`attribute`属性的值.

---

## 参考文献

---

* **[w3school](http://www.w3school.com.cn/jquery/jquery_dom_get.asp)**
