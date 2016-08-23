---
layout: post
title:  JQuery学习笔记(十二)---获取并设置CSS
date:   2016-8-21 16:00:47
categories: JQuery Web
excerpt: 关于jQuery对css操作的学习
---

* content
{: toc}

## 功能介绍

---

我们可以通过该功能获取或者设置选定元素的css样式.

---

## 实现方法

---

`jQuery`提供了四种方法操作CSS样式, 分别是:

1. `addClass()` - 向被选元素添加一个或者多个类.

2. `removeClass()` - 从被选元素中移除一个或者多个类.

3. `toggleClass()` - 对被选元素进行添加或者移除类的操作.

4. `css()` - 设置或者返回样式属性.

下面是为本篇文档所有例子设置一个css样式.

```css
<style>
	.important
	{
		font-weight:bold;
		font-size:xx-large;
	}

	.blue
	{
		color:blue;
	}
</style>
```

### `addClass()`

```jQuery
//使用addClass()为选定元素添加一个class属性.
$("h1, h2, p").addClass("blue");
$("div").addClass("important");

//使用addClass()为选定元素添加多个class属性.
$("#div2").addClass("important blue");
```

### `removeClass()`

```jQuery
//使用removeClass从选定元素上移除指定的class属性.
$("#div2").removeClass("blue");
```

### `toggleClass()`

```jQuery
//使用toggleClass()方法在添加和删除class属性之间切换.
$("button").click(function(){
	$("div2").toggleClass("blue");
});
```

### `css()`

#### 获取属性

通过使用`css("属性名称")`来获取指定属性的值.

```jQuery
var value = $("#div2").css("background-color");
```

#### 设置属性

通过使用`css("属性名称", "属性值")`来为选定元素的class中的特定属性设置属性值.

```jQuery
$("p").css("background-color","yellow");
```

我们也可以使用该方法为选定元素一次设置多个属性.

```jQuery
$("p").css({"background-color":"yellow","font-size":"200%"});
```

---

## 参考文献

---

* **[w3school](http://www.w3school.com.cn/jquery/jquery_css_classes.asp)**