---
layout: post
title:  JQuery学习笔记(五)---淡入淡出
date:   2016-8-7 11:53:05
categories: JQuery Web
excerpt: 关于jquery的学习
---

* content
{: toc}

## 功能介绍

---

淡入淡出可以用来显示或者隐藏元素.

---

## 实现方法

---

JQuery中有四种`fade`方法可以实现淡入淡出效果, 分别是:  

* `fadeIn()`, 用于淡入指定元素.

* `fadeOut()`, 用于淡出指定元素.

* `fadeToggle()`, 用于在淡入淡出之间切换.

* `fadeTo()`, 用于将指定元素渐变为一个指定的状态, 该状态一般不会是透明状态.

### `fadeIn()`

`fadeIn()`用于淡入已隐藏的功能.

**用法**: $(selector).fadeIn(speed, callback);

| 可选参数 | 说明 |   
| [speed] | 淡入的速度, 可选项有: "slow", "fast", 毫秒值. |   
| [callback] | 执行完淡入操作后需要运行的函数. |   

**示例**

使用`shadeIn()`方法, 让`div`元素淡入.

```jquery
<script>
	$("button").click(function(){
		$("#div1").fadeIn();
		$("#div2").fadeIn("slow");
		$("#div3").fadeIn(3000);
	});
</script>
<body>
	<div id="div1" style="display:none;"><p>第一段</p></div>
	<div id="div1" style="display:none;"><p>第二段</p></div>
	<div id="div1" style="display:none;"><p>第三段</p></div>
	<button>点击</button>
</body>
```

### `fadeOut()`

`fadeOut()`用于淡出正在显示的元素.

**用法**: $(selector).fadeOut(speed, callback);

| 可选参数 | 说明 |   
| [speed] | 淡入的速度, 可选项有: "slow", "fast", 毫秒值. |   
| [callback] | 执行完淡入操作后需要运行的函数. |   

**示例**

使用`fadeOut()`方法, 让`div`元素淡出.

```jquery
<script>
	$("button").click(function(){
		$("#div1").fadeOut();
		$("#div1").fadeOut("slow");
		$("#div1").fadeOut(3000);
	});
</script>
<body>
	<div id="div1">第一段<p></p></div>
	<div id="div2">第二段<p></p></div>
	<div id="div3">第三段<p></p></div>
</body>
```

### `fadeToggle()`

`fadeToggle()`用于在`fadeIn()`与`fadeOut()`之间切换.

**用法**: $(selector).fadeToggle(speed, callback);

| 可选参数 | 说明 |   
| [speed] | 淡入淡出速度, 可选项有: "slow", "fast", 毫秒值. |   
| [callback] | 淡入淡出效果执行完成之后运行的方法. |   

### `fadeTo()`

`fadeTo()`允许渐变为给定的不透明度.

**用法**: $(selector).fadeTo(speed, opacity, callback);

| 可选参数 | 说明 |   
| speed | 必须,规定效果的时常, 可选项有: "slow", "fast", 毫秒值. |   
| opacity | 必须, 将淡入淡出效果设置为给定的不透明(值介于0-1之间). |   
| [callback] | 执行完成效果之后运行的方法. |  

**示例**

使用`fadeTo()`改变`<div>`元素的背景色状态.

```jquery
<script>
	$(document).ready(function(){
		$("button").click(function(){
			$("#div1").dadeTo("slow", 0.1);
			$("#div2").dadeTo("slow", 0.4);
			$("#div3").dadeTo("slow", 0.9);
		});
	});
</script>
<html>
	<button>点击淡出div</button>
	<div id="div1" style="width:80px;height:80px;background-color:red;"></div>
	<div id="div2" style="width:80px;height:80px;background-color:green;"></div>
	<div id="div3" style="width:80px;height:80px;background-color:blue;"></div>
</html>
```

---

## 参考文献

---

* **[w3school](http://www.w3school.com.cn/jquery/jquery_fade.asp)**