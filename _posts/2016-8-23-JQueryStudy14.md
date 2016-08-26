---
layout: post
title:  JQuery学习笔记(十四)---遍历函数
date:   2016-8-23 13:04:22
categories: JQuery Web
excerpt: 关于JQuery提供的遍历函数的学习
---

* content
{: toc}

## 功能介绍

---

我们常常遇到这样的需求, 在点击一个按钮之后要显示/隐藏一个元素. 这种情况下就需要定位到需要操作的元素, 接着调用显示/隐藏方法. 查找该元素的过程就是一种遍历.

---

## 实现方法

---

| 函数 | 参数 | 描述 |   
| `andSelf()` | 无 | 将堆栈中之前的元素集添加到当前的集合中. |   
| `children()` | [selector] | 查找当前选定元素的所有直接子元素, 如果有参数则选择所有符合参数选择器要求的子元素. |   
| `closest()` | selector | 从当前选定元素开始向上查找(找祖先元素), 获取第一个符合选择器要求的元素. |   
| `contents()` | 无 | 获得选定元素包含的所有元素, 包括子元素/文本/注释节点等. |   
| `each()` | function([index], [element]) | 遍历当前堆栈中选中的所有元素. index 表示当前执行元素的序号, element 当前执行元素. |   
| `end()`| 无 | 结束当前链条中的筛选结果, 将匹配元素还原为之前的状态. |   


### `andSelf()`

该示例的目的是为了给 item3 以后的 `<li>` 元素(包括 item3 )添加背景颜色.

```jquery
<script>
	$(document).ready(function(){
		//方法一
		$(".third-item").css("background-color", "red");
		$(".third-item").nextAll().css("background-color", "red");

		//方法二
		$(".third-item").nextAll().andSelf().css("background-color", "red");
	});
</script>
<html>
	<ul>
		<li>list item 1</li>
		<li>list item 2</li>
		<li class="third-item">list item 3</li>
		<li>list item 4</li>
		<li>list item 5</li>
	</ul>
</html>
```

由上面的例子可以看出, `andSelf()` 会将当前选中元素并入操作结果的堆栈中.

### `closest()`

该示例的目的是为了通过点击任意一个元素并改变距离该元素最近的 `<li>` 的背景.

```jQuery
<style>
	li { margin: 3px; padding: 3px; background: #EEEEEE; }
	li.hilight { background: yellow; }
</style>
<script>
	$(document).bind("click", function(e){
		$(e.target).closest("li").toggleClass("hilight");
	});
</script>
<body>
	<ul>
		<li><b>Click me!</b></li>
		<li>You can also <b>Click me!</b></li>
	</ul>
</body>
```

### `contents()`

`children()` 方法类似于 `contents()` , 但前者指获取选定元素中的子元素, 不提取其他内容. **`contents()` 也可以获取 `<iframe>` 的内容, 前提是 iframe 和主页面在同一个域中.**

下面的示例中, 使用 `contents()` 方法对文本段落进行加粗.

```jQuery
<script>
	$(document).ready(function(){
		$("p").contents().filter(function(){
			//nodeType 属性存有指示节点类型的数字代码, 文本节点=3, 标签 = 1.
			this.nodeType != 1;
		}).wrap("<b/>");
	});
</script>
<body>
	<p>Hello <a href="http://myapple.com.cn/">Apple</a>, how are you doing?</p>
</body>
```

### `end()`

大多数 jQuery 的遍历方法会操作一个 jQuery 对象实例，并生成一个匹配不同 DOM 元素集的新对象。当发生这种情况时，应该会把新的元素集推入维持在对象中的堆栈内。每次成功的筛选方法调用都会把新元素推入堆栈中。如果我们需要老的元素集，可以使用 `end()` 从堆栈中弹出新集合。

该方法主要适用于 jQuery 的命令链中.

下面的示例中 `end()` 会将匹配对象还原到 `find('.foo')` 方法之前.

``` jQuery
<script>
	$(document).ready(function(){
		$('ul.first').find('.foo').css('background-color', 'red')
			.end().find('.bar').css('background-color', 'green');
	});
</script>
<body>
	<ul class="first">
		<li class="foo">list item 1</li>
		<li>list item 2</li>
		<li class="bar">list item 3</li>
	</ul>
	<ul class="second">
		<li class="foo">list item 1</li>
		<li>list item 2</li>
		<li class="bar">list item 3</li>
	</ul>
</body>
```

---

## 参考文献

---

* **[w3school](http://www.w3school.com.cn/jquery/jquery_ref_traversing.asp)**