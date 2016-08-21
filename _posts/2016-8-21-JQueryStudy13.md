---
layout: post
title:  jQuery学习笔记(十三)---遍历
date:   2016-8-21 17:17:53
categories: JQuery Web
excerpt: 关于jQuery遍历DOM对象中元素的学习
---

* content
{: toc}

## 功能介绍

---

我们有时可能需要通过当前元素来找寻其他元素, 在知道其他元素相对于当前元素的相对位置之后, 我们可以通过遍历实现这一需求. 

整个HTML文本可以视为一个由HTML标签组成的标签树. 如下图:

![DOM树结构图]({{ site.baseurl }}/css/pics/dom_tree.gif)
   
如图所示, 我们可以对祖先, 后代, 同胞进行遍历.

---

## 实现方法

---

### 遍历祖先
   
遍历祖先的方法三个, 分别是:  
  
1. `parent()` - 该方法返回选定元素的直接父元素, 只会向上一级遍历.

2. `parents()` - 该方法返回选定元素的所有祖先元素, 一路向上直到文档的根元素`<html>`为止.

3. `parentsUntil()` - 该方法返回选定元素和参数元素之间的所有元素. 

```jQuery
<script>
	$(document).ready(function(){
		//查找class='demo'的直接父级元素.
		$(".demo").parent().css({"color":"red","border":"2px solid red"});

		//查找class='demo'元素的所有父级元素.
		$(".demo").parents().css({"color":"red","border":"2px solid red"});

		//查找id='div2'元素的所有父级元素中class="demo"的父级元素.
		$("#div2").parents(".demo").css({"color":"red","border":"2px solid red"});

		//查找<span>元素与<div>元素之间的所有元素.
		$("span").parentsUntil("div").css({"color":"red","border":"2px solid red"});

		//查找<span>元素与class="demo"的<div>元素之间的所有元素.
		$("span").parentsUntil("div.demo").css({"color":"red","border":"2px solid red"});
	});
</script>
<body class="ancestors"> body (曾曾祖父)
	<div style="width:500px;" class="demo">div (曾祖父)
		<div style="width:500px;">div (曾祖父)
			<ul>ul (祖父)  
				<li>li (直接父)
					<span>span</span>
				</li>
			</ul>   
		</div>
	</div>
</body>
```

### 遍历后代

### 遍历同胞

---

## 参考文献

---

* **[w3school](http://www.w3school.com.cn/jquery/jquery_traversing.asp)**