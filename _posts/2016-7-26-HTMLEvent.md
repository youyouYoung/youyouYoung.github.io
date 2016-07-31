---
layout: post
title:  HTML DOM Event 对象
date:   2016-7-26 13:11:47
categories: Web HTML
excerpt: 为学习JQuery的事件监听函数而了解这部分功能
---

* content
{: toc}

## Event 对象

---

**Event对象**代表事件的状态, 如键盘按键的状态, 鼠标位置状态等. 每一个对象在HTML标签中都可以个使用一个属性来表示, 当用户的操作符合属性的规定时可以出发浏览器执行一段JavaScript代码.

---

## Event Handlers

---

| 属性 | 此事件在何时发生.... |   
| `onabort` | 图像的加载被中断时 |   
| `onblur` | 元素失去焦点 |   
| `onchange` | 域中的内容被改变之后 |   
| `onclick` | 元素被点击之后调用的事件 |   
| `ondblclick` | 元素被双击之后调用的事件 |   
| `onerror` | 在文档或者图片加载出错时发生 |   
| `onfocus` | 元素获得焦点 | 
| `onkeydown` | 某个键盘按键被按下 |   
| `onkeypress` | 某个键盘按键被按下并松开 |   
| `onkeyup` | 某个键盘按键被松开 |   
| `onload` | 一张页面或一幅图像完成加载 |   
| `onmousedown` | 鼠标按钮被按下 |   
| `onmousemove` | 鼠标被移动 |   
| `onmouseout` | 鼠标从某元素移开 |   
| `onmouseover` | 鼠标移到某元素之上 |  
| `onmouseup` | 鼠标按键被松开 |   
| `onreset` | 重置按钮被点击 |  
| `onresize` | 窗口或框架被重新调整大小 |   
| `onselect` | 文本被选中 |   
| `onsubmit` | 确认按钮被点击 |   
| `onunload` | 用户退出页面 |  


**示例**

* 此例中, 使用了`onblur`属性. 当输入框失去焦点之后其中的字母将转化为大写字母.

```HTML
<html>
<head>
   <script>
      function change(){
	var name = document.getElementById("name").value
	document.getElementById("name").value = name.toUpperCase()
      }
   </script>
</head>
<body>
   Enter your name: <input type="text" id="name" onblur="change()" />
</body>
</html>
```

* 此例中, 使用了`onkeydown`属性. 当且仅当按下的字符为非数字时才能正常输入到文本框中.  

```HTML
<html>
<head>
   <script>
      function checkNum(e){
         var keynum	//键盘上的按键对应的数值
	 var keychar	//键盘上的按键对应的字符
	 var numcheck	//用来检查按键是否是数字的函数

	 //IE与其他浏览器在获取键盘输入字符时使用的方法不同, 所有要做特殊区分.
	 if(window.event){
	    //IE
	    keynum = e.keyCode
	 }
	 else if(e.which){
	    //other browser
	    keynum = e.which
	 }

	 keychar = String.fromCharCode(keynum)
	 numcheck = /\d/
	 return !numcheck.test(keychar)
      }
   </script>
</head>
<body>
   <input type="text" onkeydown="return checkNum(event)" />
</body>
</html>

```

---

## 参考文献

---

* **[w3school](http://www.w3school.com.cn/tags/html_ref_eventattributes.asp)**