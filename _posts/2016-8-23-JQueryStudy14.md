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

### `andSelf()`示例

该示例的目的是为了给item3以后的`<li>`元素(包括item3)添加背景颜色.

```jquery
<script>
	$(document).ready(function(){
		
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

---

## 参考文献

---

* **[w3school](http://www.w3school.com.cn/jquery/jquery_ref_traversing.asp)**