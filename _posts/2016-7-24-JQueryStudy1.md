---
layout: post
title:  JQuery学习笔记(一)---概述
date:   2016-7-24 20:27:53
categories: JQuery Web
excerpt: 对JQuery的学习
---

* content
{: toc}

## JQuery的语法
---

`JQuery`的基础语法是**$(selector).action()**,其中:

* $代表`JQuery`

* `selector`是选择器, 用于查找HTML元素

* `action()`是对查找到的元素进行的操作

---
## 文档就绪函数
---

为了防止文档在没有完全加载完成之前就运行`JQuery`代码, 我们将所有要执行的`JQuery`代码都放在文档就绪函数中. 如果在文档没加载完成之前可能会出现错误, 例如: 

* 试图隐藏一个不存在的元素

* 获取一个没有完全加载完成的图片大小

文档就绪行数的写法如下:  

``` jQuery
$(document).(function(){
	//JQuery代码存放的位置
});
```

---
## 参考文献
---

* **[w3school](http://www.w3school.com.cn/jquery/jquery_reference.asp)**