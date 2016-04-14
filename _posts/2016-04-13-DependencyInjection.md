---
layout: default
title:  "依赖注入的学习"
date:   2016-4-13 11:06:13
categories: CreateObject
excerpt: 在做毕设的网络爬虫时看到了有关依赖注入的代码，顺便学习一下
---

* content
{:toc}

## 说明
16年4月12日下午，我试着给自己的毕设写一个爬虫程序。由于一贯的眼高手低我开始时无从下手，所以先从GitHub上clone了一个Java实现的简单爬虫，看看人家怎么写然后不上自己思路中空缺的那一块。
然而可悲的是下载的爬虫代码自己却看不懂，起初连main方法都没有找到（心里在嘲笑自己的无能）。在我找到入口之后再一次的懵逼又来袭了！
*代码的开始部分就引用了两个第三方jar包*，Google了一篇API硬是没看懂jar包的功能。在这里写出来因为他和下面我要介绍的有关，他们分别是：
>1. org.apache.commons.cli
>2. com.google.inject
在半颓废状态中，当天下午以及晚上睡觉前我对这两个引用稍微了解了一些（因为不爽我晚上通过玩游戏和看电影放松了一下，在这里做一下检讨）。在看到别人写的代码我压根就看不懂时心里真的很难受甚至都开始怀疑自己的是否能成为自己心中期望的程序员。这段心里路程（链接）真的很艰难，有时间我会把它写出来放在我的博客中，反思必须时刻进行！
好了废话不多说，现在开始介绍这两个包的功能：
*1. org.apache.commons.cli 是一个从命令行获得选项和参数的包。*不，准确说应该是结构化选项和参数，因为被它处理的命令行参数可以更加清新的区分和获取。我的理解它就像options命令一下（Bash Shell中的命令）
*2. com.google.inject 是一个实现依赖注入的包。*具体的内容我会在下面介绍。

## 名词解释
依赖注入是指程序运行中，如果需要调用一个对象时不需要自己去创建对象，而是依赖于外部注入一个对象。
下面的这段代码是我使用的爬虫工具里面的依赖注入部分：
>import com.google.inject.Guice;
>import com.google.inject.Injector;
>public className
>{
>	public void functionName()
>	{
>		final Injector injector = Guice.createInjector(new CrawlModule());
>		final Crawler crawler = injector.getInstance(Crawler.class);
>		//crawler在此刻并没有被创建，而且Crawler.class是一个接口类，并没有做实现
>		final CrawlerResult result = crawler.getUrls(getConfiguration());
>	}
>}

## 依赖注入的使用

