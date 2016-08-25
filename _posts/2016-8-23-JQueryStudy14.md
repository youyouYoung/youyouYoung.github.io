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
| ``


### `andSelf()`示例

该示例的目的是为了给item3以后的`<li>`元素(包括item3)添加背景颜色.

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

由上面的例子可以看出, `andSelf()`会将当前选中元素并入操作结果的堆栈中.

### `closest()`示例

该示例的目的是为了通过点击任意一个元素并改变距离该元素最近的`<li>`的背景.

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

---

## 参考文献

---

* **[w3school](http://www.w3school.com.cn/jquery/jquery_ref_traversing.asp)**