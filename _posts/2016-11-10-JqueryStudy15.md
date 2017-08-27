---
layout: post
title:  Jquery定期执行任务
date:   2016-11-10 18:01:32
categories: JQuery
excerpt: 关于Jquery定期执行任务
---

* content
{: toc}

## 实现方式

---

```jquery
//通过 setInterval 方法可以在间隔 timeout ms之后执行一次 targetFunction 方法
$(function(){
setInterval(targetFunction, timeout);
});

function targetFunction(){
	alert("hi!");
}
```
