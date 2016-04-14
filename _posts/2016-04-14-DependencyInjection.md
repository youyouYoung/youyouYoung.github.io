---
layout: default
title:  "依赖注入的学习"
date:   2016-04-14 10:29:24
categories: DependencyInjection
excerpt: 在做毕设的网络爬虫时看到的有关依赖注入的代码,顺便学习一下
---

* content
{:toc}

## 说明
16年4月12日下午,我试着给自己的毕设写一个爬虫程序.由于一贯的眼高手低导致我一开始不知道从哪里下手,于是我在GitHub上clone了一个简单的网络爬虫程序给自己来点思路.
然而可悲的是已经是最简单的爬虫程序了我仍然没有看懂,一开始我甚至连main方法都没有找到(心里在嘲笑着自己的无能).在我找到入口之后再一次的懵逼又来袭了!
*代码的开始部分使用了两个第三方jar包*,google了一篇API硬是没看懂里面的英文单词更没懂这玩意究竟是做什么的.他们分别是:
>1. org.apache.commons.cli
>2. com.google.inject
在半颓废状态下,我当天下午以及晚上睡觉前对这两个引用稍微了解了一下(感谢我之前学习的基础帮助了我).别问我整个晚上在干嘛,由于不爽我晚上通过打游戏和看电影在发泄,现在比较后悔这样做了.在看到别人写的代码我压根看不懂时心里真的有种说不出来的难受,甚至都开始怀疑自己是否能成为一个优秀的程序员.不过我最终还是扛过来了(这段心里路程有时间我会写下来,作为以后的经验)!
好了费话不多说,现在开始介绍这两个包的功能:
*1. org.apache.commons.cli是一个从命令行获取选项和参数的包*.不,准确来说应该是获取结构化选项和参数,因为被他处理的命令行参数可以更加清晰的区分和获取.如果你有Bash Shell的编程经验,可以把它理解成options命令.
*2. com.google.inject是一个实现依赖注入的包*.具体的内容会作为我本篇文章的重点在下面介绍.

## 名词解释
依赖注入是指程序运行时,如果需要调用一个对象时不需要自己去创建类的实例(对象),而是依赖外部注入一个对象.
下面的这段代码时我使用的爬虫的有关与依赖注入的部分,多余的部分我省略没有写:
>import com.google.inject.Guice;
>import com.google.inject.Injection;
>public className
>{
>	public void functionName()
>	{
>		final Injector injector = Guice.createInjector(new CrawlModule());
>		final Crawel crawel = injector.getInstance(Crawel.class);
>
>		//
>		final CrawelResult result = crawel.getUrls(getConfigration())
>	}
>}

## 依赖注入的原理

## 依赖注入的使用
