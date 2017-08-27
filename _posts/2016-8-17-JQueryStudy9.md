---
layout: post
title:  JQuery学习笔记(九)---设置内容和属性
date:   2016-8-17 12:34:06
categories: JQuery
excerpt: 关于jQuery设置内容和属性的学习
---

* content
{: toc}

## 功能介绍

---

设置内容和属性的方法用来设置指定元素的属性和内容, 比如设置指定元素中的HTML内容, 文本内容或者style属性或者class属性等.

---

## 实现方法

---

### 设置内容

设置内容的方法有三种:

1. `text()` - 设置元素的文本内容

2. `html()` - 设置元素的内容(包括HTML标签)

3. `val()` - 设置元素的value值

### 设置属性

通过`attr()`方法来设置选定元素的属性, `attr()`方法允许同时设置多个属性.

**示例**

该示例使用`attr()`方法设置多个属性.

``` jQuery
<script>
	$(document).ready(function(){
		$("button").click(funtion(){
			$("#demo").attr({
				"href" : "www.baidu.com",
				"title" : "百度"
			});
		});
	});
</script>
<body>
	<a id="demo" href="www.google.com.hk">google</a>
	<button>改变href</button>
</body>
```

---

## 具有回调函数的函数

---

### 设置内容的回调函数

`text()`, `html()`, `val()`这三个方法都拥有回调函数功能. 回调函数有两个参数: 当前元素在被选元素列表中的下标(从0开始), 以及当前元素的原始(旧的)值. 然后将函数的返回值作为当前元素的内容.

**示例**

下面的示例中使用回调函数改变`<li>`元素的文本内容.

```jQuery
<script>
	$(document).ready(function(){
		$("button").click(function(){
			$("li").text(function(i, orignText){
				return "The new value is "+orignText+"(index: "+i+")";
			});
		});
	});
</script>
<body>
	<ul>
		<li>第0列</li>
		<li>第1列</li>
		<li>第2列</li>
		<li>第3列</li>
	</ul>
	<button>点击改变内容</button>
</body>
```

### 设置属性的回调函数

`attr()`方法提供的回调函数具有两个参数: 当前元素在被选元素列表中的下标(从0开始), 以及当前元素的原始(旧的)值.

**示例**

下面的示例中使用`attr()`的回调函数设置每个`<li>`标签的`demo`属性.

```jQuery
<script>
	$(document).ready(function(){
		$("button").click(function(){
			$("li").attr("demo", function(i, orignText){
				return orignText+": "+i;
			});
		});
	});
<script>
<body>
	<ul>
		<li demo="test1">第0列</li>
		<li demo="test2">第1列</li>
		<li demo="test3">第2列</li>
		<li demo="test4">第3列</li>
	</ul>
	<button>编辑改变li的demo属性</button>
</body>
```

---

## 参考文献

---

* **[w3school](http://www.w3school.com.cn/jquery/jquery_dom_set.asp)**
