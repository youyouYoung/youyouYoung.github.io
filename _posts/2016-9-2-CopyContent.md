---
layout: post
title:  JQuery复制网页内容到系统剪切板
date:   2016-9-2 13:21:29
categories: JQuery
excerpt: 部门老大屁事很多. 有一天硬是要一个复制网页文本的功能, 硬生生的逼着我获得了这项技能, 感谢这位老大.
---

* content
{: toc}

## 功能介绍

---

我们有时需要复制文本中的一段内容, 一般我们会通过左键选中然后复制的. 但是一旦出现需要反复去复制时, 在需要复制的内容比较多时可能出现没有复制全或者过程繁琐等问题. 例如我们会在网页上选择一个文件同时制定一个计算参数点击计算后将一个图表复制到 Excel 中, 这种工作有时会少复制一到两行数据. 因此设计了下面的功能.

---

## 实现方法

---

在网上找了好多种方法, 试了好多种不知道为什么都用不了. 在快绝望的时候试了一下 `ZeroClipboard` , 结果实现了这个功能.   
   
**注意: 使用该方法前必须将网页部署在 web server 上!!!!**   
    
在使用之前我们需要引入两个文件:  
    
1. jquery.js (使用jQuery必须引用的js文件)
2. jquery.zclip.min.js (使用复制功能必须引用的文件)

另外在网页文件中我们还需要通过 ZeroClipboard.swf 文件产生一个隐藏的flash对象.
   
```jQuery
<head>
	<script src="/demo/common/js/jquery.zclip.min.js"></script>
	<script src="/demo/bootstrap/js/jquery.min.js"></script>
	<script>
		$(document).ready(function(){
			$("#copybtn").click(function(){
				copy();
			});
		});

		function copy()
		{
			$('#copycontent').zclip({
				path: '/demo/common/js/ZeroClipboard.swf', 
				copy: function(){//复制内容 
					return $("#copycontent").html();
				}, 
				afterCopy: function(){//复制成功 
					alert('复制成功'); 
				}
			});
		}
	</script>
</head>
<body>
	<div id='copycontent'>
		<table>
			<tr>
				<td>数据</td>
				<td>数据</td>
				<td>数据</td>
			</tr>
		</table>
	</div>
	<button id='copybtn'>点击复制表格内容</button>
</body>
```

`ZeroClipboard`所需要的包的下载路径: [ZeroClipboard]({{ site.baseurl }}/download/ZeroClipboard.tar.gz)

---

## 参考文献

---

* **[【原】js实现复制到剪贴板功能，兼容所有浏览器- 白树- 博客园](http://www.cnblogs.com/PeunZhang/p/3324727.html)**

* **[无需Flash 使用jQuery 复制文字到剪贴板- 技术翻译- 开源中国社区](http://www.oschina.net/translate/copying-to-clipboard-using-jquery-without-the-need)**

* **[20 行JS 代码，实现复制到剪贴板功能- WEB前端- 伯乐在线](http://web.jobbole.com/86054/)**

* **[Jquery 实现复制到剪贴板- HSYS博客- ITeye技术网站](http://hsys.iteye.com/blog/607443)**

* **[将内容复制到剪切板兼容主流浏览器的解决方案- 简书](http://www.jianshu.com/p/1a74c112f962)**
