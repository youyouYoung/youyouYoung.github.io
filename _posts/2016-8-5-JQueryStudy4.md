---
layout: post
title:  JQuery学习笔记(四)--隐藏和显示
date:   2016-8-5 13:18:02
categories: JQuery Web
excerpt: 关于jQuery的学习
---

* content
{: toc}

## 功能介绍

---

页面中有时需要使一些元素处于显示或者隐藏状态, 如点击隐藏一个`div`等. 这种情况下我们就需要使用到jQuery的显示和隐藏功能.

---

## 实现方法

---

### `hide()`&`show()`

```jquery
<script>
	$(".hide").click(function(){
		$("div").hide();
	});

	$(".show").click(function(){
		$("div").show();
	});
</script>
<body>
	<div>
		<p>这是一段文字</p>
	</div>
	<button class="hide">隐藏上面一段话</button>
	<button class="show">显示上面一段话</button>
</body>
```

---

## 参考文献

---

* **[w3school](http://www.w3school.com.cn/jquery/jquery_hide_show.asp)**