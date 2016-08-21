---
layout: post
title:  JQuery学习笔记(十一)---删除元素
date:   2016-8-21 12:07:21
categories: JQuery Web
excerpt: 关于jQuery删除元素的学习
---

* content
{: toc}

## 功能介绍

---

页面中我们有时需要一个可以添加或者删除元素的功能, 例如需要删除`<table>`的一行数据(`<tr>`). 或者删除一个`<div>`.

---

## 实现方法

---

jQuery提供了两种方法可以实现删除元素的功能, 包括: 

1. `remove()` - 删除被选元素, 以及其子元素.

2. `empty()` - 清空被选元素的内容, 包括清空被选元素中的文本内容和子元素.

### 删除元素

**示例**

下面示例将展示一个通过点击按钮, 删除`<div>`的效果.

```jQuery
<script>
	$(document).ready(function(){
		$("button").click(function(){
			$("#div1").remove();
		});
	});
</script>
<body>
	<div id="div1">
		这是一个div元素的内容.
	</div>
	<button>点击删除div元素</button>
</body>
```

### 具有过滤条件的`remove()`

`remove()`方法可以指定一个参数作为过滤条件, 用于指定需要删除的元素. 该参数可以是任何jQuery选择器的语法.

**示例**

下面的示例使用`remove()`方法删除所有`class`属性包含`demo`的`<p>`元素.

```jQuery
<script>
	$(document).ready(function(){
		$("button").click(function(){
			$("p").remove(".demo");
		});
	});
</script>
<body>
	<p>段落1</p>
	<p>段落2</p>
	<p>段落3</p>
	<p class="demo">段落1</p>
	<p class="demo">段落2</p>
	<p class="demo">段落3</p>
</body>
```

### 清空元素

**示例**

下面的示例将展示一个通过点击按钮, 完成清空`<div>`中所有内容的功能.

```jQuery
<script>
	$(document).ready(function(){
		$("button").click(function(){
			$("#div1").empty();
		});
	});
</script>
<body>
	<div id="div1">
		这是一个div元素的内容.
	</div>
	<button>点击清空div元素</button>
</body>
```

---

## 参考文献

---

* **[w3school](http://www.w3school.com.cn/jquery/jquery_dom_remove.asp)**