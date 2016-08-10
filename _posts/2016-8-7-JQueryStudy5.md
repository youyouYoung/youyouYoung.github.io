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

---

## 参考文献

---

* **[w3school](http://www.w3school.com.cn/jquery/jquery_fade.asp)**