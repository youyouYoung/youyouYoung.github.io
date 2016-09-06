---
layout: post
title:  JQuery操作iframe的内容
date:   2016-9-4 18:05:27
categories: Web JQuery
excerpt: 网页中经常会使用iframe来展现另一个网页文件的内容, 但是这种情况下我们不能直接通过jQuery的选择器获取iframe中的元素.
---

* content
{: toc}

## 功能介绍

---

网页中经常会用到 `<iframe>` 元素. 通过这个元素我们可以将另一个网页附加到当前网页中. 但是在这种情况下我们不能直接通过 jQuery 的选择器来获取 `<iframe>` 引入的网页中的任何元素.

---

## 实现方法

---

**示例**   
   
下面的示例中创建了两个网页文件, 网页A通过 `<iframe>` 引用了网页B. 同时可以通过点击网页A中的按钮获取网页B中的一个输入框中的内容.
   
网页A 'a.jsp'
```jQuery
<head>
	<script>
		$(document).ready(function(){
			$("#showValue").click(function(){
				$("#iframe")[0].contentWindow.copyInfo();
			});
		});
	</script>
</head>
<body>
	<button id="showValue">显示输入框内容</button>
	<iframe name='iframe' id='iframe' src='b.jsp'>
</body>
```
   
网页B `b.jsp`
```jQuery
<head>
	<script>
		function copyInfo()
		{
			return $("[name='demo']").val();
		}
	</script>
</head>
<body>
	<input type='text' name='demo' id='demo'>
</body>
```

---

## 参考文献

---

* **我的上级荣哥的代码**

* **[网站优化使用iframe嵌套另一页面](http://www.360doc.com/content/09/0922/15/323829_6293598.shtml)**

* **[Jquery取得iframe中元素的几种方法](http://www.cnblogs.com/vipstone/archive/2012/04/01/2428257.html)**