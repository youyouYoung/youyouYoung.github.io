---
layout: post
title:  JQuery学习笔记(六)---滑动效果
date:   2016-8-11 12:44:23
categories: JQuery Web
excerpt: 关于jquery滑动效果的学习
---

* content
{: toc}

## 功能介绍

---

在网页上创建具有滑动效果的特效. 滑动效果不同于淡入淡出, 所占空间是渐变的过程而淡入淡出的颜色效果是渐变的.

---

## 实现方法

---

JQuery中有三种方法实现滑动效果, 分别是:  

* `slideDown()`用于向下滑动选定元素. (是显示效果)

* `slideUp()`用于向上滑动选定元素. (是隐藏效果)

* `slideToggle()`用于在向上滑动和向下滑动之间转换.

### `slideDown()`

`slideDown()`用于向下滑动, 具有显示效果.

**用法**: $(selector).slideDown(speed, callback);

| 可选参数 | 说明 |   
| [speed] | 规定速度 |   
| [callback] | 滑动效果执行完成之后执行的函数. |   

### `slideUp()`

`slideUp()`用于向上滑动, 具有隐藏效果.

**用法**: $(selector).slideUp(speed, callback);

| 可选参数 | 说明 |   
| [speed] | 规定速度 |   
| [callback] | 滑动效果执行完成之后执行的函数. |   

### `slideToggle()`

`slideToggle()`用于在`slideDown()`和`slideUp()`之间转换.

**用法**: $(selector).slideToggle(speed, callback);

| 可选参数 | 说明 |   
| [speed] | 规定速度 |   
| [callback] | 滑动效果执行完成之后执行的函数. |   

---

## 参考文献

---

* **[w3school](http://www.w3school.com.cn/jquery/jquery_slide.asp)**