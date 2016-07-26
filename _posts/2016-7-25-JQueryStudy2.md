---
layout: post
title:  JQuery学习笔记(二)---选择器
date:  2016-7-25 17:21:26
categories: JQuery Web
excerpt: 关于的JQuery的学习
---

* content
{: toc}

## 选择器介绍

---

**选择器**在JQuery中以`$("selector")`样式呈现, 其作用是可以通过选择器选取一个或一类**元素(DOM)对象**. 选择器的种类有:

* 元素选择器: 表示通过HTML标签来获取元素.

* 属性选择器: 表示通过HTML标签的属性来获取元素.

* css选择器: 用于选取或者改变HTML元素的css属性.

同时选择器中可以添加一些模式用于限制查找的结果. 如`XXX:last`表示返回符合`XXX`要求的元素序列中最后一个元素.

---

### 元素选择器   

---

元素选择器是根据HTML元素的名称来选择的模式, 例如:

> `$("p")`选择`<p>`元素.  
> `$("p.intro")`选择`<p>`元素中`class="intro"`的元素.   
> `$("p#intro")`选择`<p>`元素中`id=intro`的元素.  
> `$("p.intro.demo")`选择`<p>`元素中`class="intro demo"`的元素.  
> `$("p, tr, td")`选择`<p>`或`<tr>`或`<td>`元素.

---

### 属性选择器   

---

属性选择器是使用`XPATH表达式`来作为匹配模式, 选择符合模式的元素内容, 例如:

> `$("[href]")`选择所有带有`href`属性的元素.   
> `$("[XPATH='#']")`选择所有属性`href='#'`的元素.  
> `$("[XPATH!='#']")`选择所有属性`href!='#'`的元素.
> `$("[XPATH$=".JPG"]")`选择所有包含属性`href`且其值以`JPG`结尾的元素.  

---

### CSS选择器   

---

CSS选择器用于获取或者设置HTML指定元素的指定CSS值. 例如:

> `$("p").css("background-color")`用于获取`<p>`元素的`background-color`值.  
> `$("p").css("background-color", "red")`用于将`<p>`元素的`background-color`值设置为`red`.  

---

### 其他常用示例   

---

| 语法 | 描述 |  
| `$(this)` | 当前调用此程序的HTML元素 |   
| `$(".intro")` | `class="intro"`的HTML元素 |   
| `$("ul li")` | `<ul>`元素中的所有`<li>`元素 |   
| `$("ul li:first")` | `<ul>`元素中的第一个`<li>`元素 |   

---

| 选择器 | 实例 | 描述 |  
| * | `$("*")` | 所有元素 |   
| #id | `$("#jquery")` | `id=jquery`的元素 |   
| .class | `$(".intro")` | `class='intro'`的元素 |   
| element | `$("p")` | 所有的`<p>`元素 |   
| .class.class | `$(".c1.c2")` | 所有的`class='c1 c2'`的元素 |   
| | |   
| `:first` | `$("p:first")` | 第一个`<p>`元素 |   
| `:last` | `$("p:last")` | 最后一个`<p>`元素 |   
| `:even` | `$("li:even")` | 序数为偶数的`<li>`元素 |   
| `:odd` | `$("li:odd")` | 序数为奇数的`<li>`元素 |   
| | |   
| `:contain(text)` | `$("table:contain("father")")` |  所有包含father字符串的`<table>`元素 |   
| `:empty` | `$(":empty")` | 所有不具有子元素的元素 |   
| `:hidden` | `$("p:hidden")` | 所有处于隐藏状态的`<p>`元素 |   
| `:visible` | `$("table:visible")` | 所有处于显示状态的`<table>`元素 |   


---

## 参考文献   

---

* **[w3school](http://www.w3school.com.cn/jquery/jquery_selectors.asp)**
