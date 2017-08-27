---
layout: post
title:  JQuery学习笔记(三)---事件函数
date:   2016-7-26 11:00:59
categories: JQuery
excerpt: 关于JQuery事件函数的学习
---

* content
{: toc}

## 事件函数介绍

---

JQuery事件函数是在HTML元素的事件被触发的情况下被执行的函数, 是JQuery中的核心函数.

通常我们会把JQuery代码放在`<head>`元素的事件处理方法中. 下例中演示了一个监听点击事件的函数: 

``` html
<html>
<head lang="zh-CN">
<meta charset="utf-8">
   <script src="jquery.min.js"></script>
   <script>
      $(document).ready(function(){
         $("button").click(function(){
	    $("p:last").toggle(); //显示和隐藏标签
	 });
      });
   </script>
</head>
<body>
   <p>第一段</p>
   <p>第二段</p>
   <button>点击</button>
</body>
</html>
```

**结论**

* 把所有的JQuery代码放入事件处理函数中.

* 把所有事件处理函数放入文档就绪事件处理函数中.

* 把JQuery代码放在单独的\*.js文件中, 在需要使用的页面进行调用即可.

---

## 事件函数

---

| 函数 | 参数 | 描述 |   
| `ready()` | `function` | 文档就绪后触发 |  
| `click()` | `function` | 点击事件发生时触发函数`function`. |   
| `dblclick()` | `function` | 双击事件发生时触发函数`function`, 如果将`click`和`dblclick`放在一个元素上可能会产生问题 |   
| `focus()` | null | 元素获得焦点时触发, 或将焦点设置到指定元素上 |   
| `mouseover()` | null | 鼠标悬停在元素上时触发 |   
| `bind()` | `event, [data,] function` | 向元素添加事件处理器. `event`:必填项, 事件名称. `data`:选填项, 传递给后面触发函数的参数. `function`:必填项, 事件处理函数(有示例) |   
| `blur()` | `function` | 当元素失去焦点时触发`function`函数内容 |   
| `change()` | `function` | 当元素的值`value`被改变时触发函数`function`. 该事件是适用于`text`, `textarea`, `select`元素 |   
| `delegate()` | `childSelector, event, [data,] function` | 为指定元素`childSelector`(被选元素的子元素)添加一个或者多个事件处理程序, 并规定当这些事件`event`发生时运行函数`function`(有示例). `delegate()`方法的事件处理程序适用于当前或者未来的元素(有js创建的新元素) |   
| `live()` | `event, [data,] function` | 同`bind()`方法 |   
| `die()` | `event[, function]` | 移除通过`live()`方法向指定元素添加的事件处理函数(有示例), `function`参数用来规定要移除的特定函数. |   
| `event.preventDefault()` | null | `event`必填项. 用来阻止一个元素默认的事件处理过程. (有示例) |   
| `event.which` | null | `event`必填项. 获取当前按下的那个键. (有示例) |  
| `keydown()` | `function` | 当按键被按下时, 触发此事件. |  
| `keypress()` | `function` | 类似于`keypress()`函数, 两者之间的区别暂时没有理解. |   
| `keyup()` | `function` | 当按键被放开时, 触发此事件. |   
| `mousedown()` | `function` | 当鼠标被按下时触发此事件. |   
| `mouseup()` | `function` | 当鼠标按键被释放时, 触发此事件. |   
| `mouseover()` | `function` | 当鼠标位于指定元素上方时, 触发此事件. |   
| `mouseout()` | `function` | 当鼠标从指定元素上移开时, 触发此事件. |   
| `one()` | `event, [data,] function` | 向选定元素添加一个只能执行一次的触发事件`event`, 假设加入`event=click, function=function(){}`, 那么指定元素在第一次被点击时会触发`function`函数中的内容. |   
| `resize()` | `function` | 在大小被调整之后触发此监听函数. 可以是浏览器窗口: `$(window)`, 或者是文档: `$(document)`. |   
| `load()` | `function` | 当指定元素加载完成之后, 触发监听函数`function`. |   
| `scroll()` | `function` | 当指定元素发生滚动事件之后, 触发函数`function`. |   
| `select()`  | `function` | 当指定元素被选中时, 触发此函数. |   
| `submit()` | `function` | 当指定元素发生提交事件时, 触发函数. 监听函数`submit()`只适用于表单元素. |   
| `toggle()` | `[speed, callback]` | 切换指定元素的显示状态, 如果为正在显示将其设为隐藏, 如隐藏则设为显示. 即在`show()`和`hide()`之前转换. `speed`用于规定hide/show的速度, 默认为0. `speed`可以选择的值有: 毫秒(如1000), slow, normal, fast. `callback`定义了当toggle()方法完成之后执行的函数. 这两个参数都是非必填项. (有示例) |   
| `trigger()` | `event[, param1, param2 ...]` | 触发指定元素的`event`事件. 其中event可以是自定义事件, param为可选项, 用于向event函数传递参数. (有示例) |   
| `unbind()` | `[event, function]` | 移除指定元素上的事件处理程序, 不规定`event`时移除所有事件处理函数. |   
| `undelegate()` | `[selector, event, function]` | 删除由`delegate()`添加的事件处理程序. `selector`: 规定需要删除的事件监听函数的元素. `event`: 规定需要删除的事件类型. `function`: 规定要删除的具体事件处理函数. |   
| `unload()` | `function` | 当离开当前链接时触发函数. 视为离开当前页面的操作有: 1. 点击某个离开页面的链接. 2. 在地址栏导向新的链接. 3. 使用前进或者后退按钮. 4. 关闭浏览器. 5. 重新加载页面. |   

---

## 示例

---

* `bind()`函数的使用方法

``` jQuery
<script>
	$(document).ready(function(){
		$("#demo").bind("click", function(){
			$(p).hide();
		});
	});
</script>
<body>
	<div id="demo">
		<p>第一段</p>
		<p>第二段</p>
		<h3>三号标题</h3>
	</div>
</body>
```

* `delegate()`函数给未来元素添加点击事件监听函数

```jQuery
<script>
	$(document).ready(function(){
		$("div").delegate(".test", "click", function(){
			$(this).slideToggle();
		});
		$("button").click(function(){
			$("<p class=\"test\">这是一个新段落</p>").insertAfter("button");
	`	});
	});
</script>
<body>
	<div>
		<p class="test">第一段</p>
		<p class="test">第二段</p>
		<p class="test">第三段</p>
		<button>点击添加一个p元素</button>
	</div>
</body>
```

* `die()`函数移除`live()`添加的事件处理函数

```jQuery
<script>
	$(document).ready(function(){
		$("p").live("click", function(){
			$(this).slideToggle();
		});
		$("button").click(function(){
			$("p").die(); //移除<p>元素上的所有`live()`添加的事件触发函数.
		});
	});
</script>
<body>
	<p>第一段</p>
	<p>第二段</p>
	<p>第二段</p>
	<button>点击取消p元素上的点击事件</button>
</body>
```

* `event.preventDefault()`事件来组织链接点击后的页面跳转操作.

```jQuery
<script>
	$(document).ready(function(){
		$("a").click(funtion(){
			event.preventDefault();
		});
	});
</script>
<body>
	<a href="www.github.com">github</a>
</body>
```

* `event.which`函数获取当前当前按下的按键的数值.

``` jQuery
<script>
	$(document).ready(function(){
		$("input").keyDown(function(event){
			$(".demo").val("Key: " + event.which);
		});
	});
</script>
<body>
	<input type="text" />
	<p class="demo"></p>
</body>
```

* `toggle(speed, callback)`函数在点击按钮后缓慢的显示或者隐藏<p>元素.

```jQuery
<script>
	$(document).ready(function(){
		$("button").click(function(){
			$("p").toggle("slow");
			$("#demo").toggle(1000);
		});
	});
</script>
<body>
	<p>一行文字</p>
	<div id="demo">div中的内容.</div>
	<button>点击</button>
</body>
```

* `trigger()`函数当点击按钮时触发文本框的`select`函数.

```jQuery
<script>
	$(document).ready(function(){
		$("input").select(function(){
			$(this).after("元素被选中!");
		});
		$("button").click(function(){
			$("input").triggle("select");
		});
	});
</script>
<body>
	文本框: <input type="text"><br/>
	<button>点击选中文本框</button>
</body>
```

---

## 参考文献

---

* **[w3school](http://www.w3school.com.cn/jquery/jquery_events.asp)**
