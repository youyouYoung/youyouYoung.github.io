---
layout: post
title:  JQuery学习笔记(七)---Callback
date:   2016-8-12 17:42:48
categories: JQuery
excerpt: 关于JQuery的学习
---

* content
{: toc}

## 功能介绍 

---

Callback函数用于在当前动画完全执行完成之后再执行. 可以这样理解如果不是Callback函数的话, 动画的执行和函数的执行是同步的(可能是一种多线程的机制). 但有些情况下我们可能会需要在一个动画执行完成之后再去执行一个函数中的操作.

---

## 实现方法

---

**假设我们需要在`<p>`元素被隐藏之后执行一个`alert`方法.**在这种情况下普通的做法一般可能如下:

```jquery
<script>
	$(document).ready(function(){
		$("button").click(function(){
			$("p").hide("slow");
			alert("<p>元素已被隐藏");
		});
	});
</script>
<body>
	<p>这是一段文字</p>
	<button>点击隐藏文字</button>
</body>
```

**注意:**这种做法无法满足我们的需求, 元素的隐藏和`alert`的执行是同步进行的.

为了实现上述需要, 我们可以通过以下Callback方法.

```jquery
<script>
	$(document).ready(function(){
		$("button").click(function(){
			$("p").hide("slow", function(){
				alert("<p>元素已被隐藏");
			});
		});
	});
</script>
```

---

## 参看文献

---

* **[w3school](http://www.w3school.com.cn/jquery/jquery_callback.asp)**
