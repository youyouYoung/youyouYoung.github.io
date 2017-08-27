---
layout: post
title:  JQuery学习笔记(四)--隐藏和显示
date:   2016-8-5 13:18:02
categories: JQuery
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

* `hide()`: 隐藏指定的元素.

* `show()`: 显示指定的元素.

**示例**

使用`hide`&`show`方法显示或者隐藏一个`<div>`元素.

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

| 可用参数 | 说明 |   
| speed | 显示或隐藏元素的速度, 可以使用的取值有: `slow`, `fast`, 毫秒值. |   
| callback | 在显示或者隐藏操作完成后执行的函数名称. |   

**示例**

规定显示或者隐藏的速度.

```jQuery
<script>
	$(".hide").click(function(){
		$("div").hide("slow");
	});

	$(".show").click(function(){
		$("div").show("fast");
	});
</script>
```

### `toggle()`

* `toggle()`: 在隐藏和显示之间切换.

使用`toggle`函数让`<p>`元素在显示和隐藏状态之间切换.

```jQuery
<script>
	$("button").click(function(){
		&("p").toggle();
	});
</script>
<body>
	<p>这是一段文字</p>
	<button>显示或者隐藏一段文字</button>
</body>
```

| 可用参数 | 说明 |   
| speed | 规定显示或者隐藏的速度. 可取的值有: "slow", "fast", 毫秒值. |   
| callback | 当显示或者隐藏操作完成之后执行的函数. |   

---

## 参考文献

---

* **[w3school](http://www.w3school.com.cn/jquery/jquery_hide_show.asp)**
