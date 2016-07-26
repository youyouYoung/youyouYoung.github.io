---
layout: post
title:  JQuery学习笔记(三)---事件函数
date:   2016-7-26 11:00:59
categories: JQuery Web
excerpt: 关于JQuery事件函数的学习
---

* content
{: toc}

## 事件函数介绍

---

JQuery事件函数是在HTML元素的事件被触发的情况下被执行的函数, 是JQuery中的核心函数.

通常我们会把JQuery代码放在`<head>`元素的事件处理方法中. 下例中演示了一个监听点击事件的函数: 

``` html
<html>
<head lang="zh-CN">
<meta charset="utf-8">
   <script src="jquery.min.js"></script>
   <script>
      $(document).ready(function(){
         $("button").click(function(){
	    $("p:last").toggle();
	 });
      });
   </script>
</head>
<body>
   <p>第一段</p>
   <p>第二段</p>
   <button>点击</button>
</body>
</html>
```

---

## 参考文献

---

* **[w3school](http://www.w3school.com.cn/jquery/jquery_events.asp)**