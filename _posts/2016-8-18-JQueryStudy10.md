---
layout: post
title:  JQuery学习笔记(十)---添加元素
date:   2016-8-18 18:26:57
categories: JQuery Web
excerpt: 关于jQuery添加元素的学习
---

* content
{: toc}

## 功能介绍

有时候我们需要在一个指定的元素后面添加一些内容, 就可以使用jQuery的添加元素的功能. 例如: 可能需要在`<table>`在末尾或者最前面添加一行`<tr>`元素. 或者在`<input type="text">`的末尾添加一个`<label>`元素用来向用户提示文本框内容是否有误.

---

## 实现方法

---

向指定元素添加内容的形式有两种. 一种是在该指定元素的末尾或者开始处添加一个元素, 如在`<input>`后添加一个`<label>`. 一种是在该指定元素内部的开始位置或者末尾位置添加新的元素, 如在`<tr>`末尾添加'<td>'元素.

在**元素内部添加内容**的方法有两个:

1. `append()` - 在末尾处添加新内容.

2. `prepend()` - 在开始位置处添加新内容.

在**元素外部添加内容**的方法有两个:

1. `after()` - 在末尾添加新内容.

2. `before()` - 在开始位置添加新内容. 

### 内部添加

**示例**

使用`append()`和`prepend()`方法向`<ul>`元素中添加`<li>`元素.

```jQuery
<script>
	$(document).ready(function(){
		$(".btn_append").click(function(){
			$("ul").append("<li>末尾添加</li>");
		});

		$(".btn_prepend").click(function(){
			$("ul").prepend("<li>起始位置添加</li>");
		});
	});
</script>
<body>
	<ul>
		<li>第0行</li>
		<li>第1行</li>
		<li>第2行</li>
		<li>第3行</li>
	</ul>
	<button class="btn_append">在尾部添加新的数据</button>
	<button class="btn_prepend">在头部添加新数据</button>
</body>
```

### 外部添加

使用`after()`在`<input>`元素之后添加一个`<lable>`元素.

```
<script>
	$(document).ready(function(){
		$("button").click(function(){
			var name = $("input[name='name']").val();
			if (name == "")
			{
				$("input[name='name']").after("<lable>名称不能为空</lable>");
				return false;
			}
			$("form").submit();
		});
	});
</script>
<body>
	<form action="XXXXXX" method="post">
		<input type="text" name="name">
		<button>提交</button>
	</form>
</body>
```

**注意:**添加元素的这四种方法都可以一次添加多个元素. 做法为: `append("<p>段落</p>", "<input type='text' name='name'>", "<b>粗体</b>")`

---

## 参考文献

---

* **[w3school](http://www.w3school.com.cn/jquery/jquery_dom_add.asp)**