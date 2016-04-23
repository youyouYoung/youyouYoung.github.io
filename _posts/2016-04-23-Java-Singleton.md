---
layout: post
title:  Java单例模式与线程安全
date:	2016-04-23 15:12:26
categories: Java Pattern Thread
excerpt: 单例模式如何保证线程安全
---

* content
{:toc}

## 单例模式的种类
*懒汉式*
因为此类型的对象在调用时才会被创建,所以称为懒汉式.
>public Singleton
>{
>	public static Singleton instance;
>
>	private Singleton()
>	{}
>
>	public static Singleton getInstance()
>	{
>		if(instance == null)
>		{
>			instance = new Singleton();
>		}
>		return instance;
>	}
>}

*饿汉式*
此类型的对象在类加载时就会被创建,称为饿汉式.
>public Singleton
>{
>	public static Singleton instance = new Singleton();
>
>	private Singleton()
>	{}
>
>	public static Singleton getInstance()
>	{
>		return instance;
>	}
>}


## 存在的问题
*懒汉式*
多线程时不安全,可能创建多个对象.

*饿汉式*
在未调用前就以创建了对象,浪费资源.尤其是在资源少或者此对象消耗资源多的情况下.

## 线程安全的单例模式
由于饿汉式不存在线程安全的问题,所以这里对懒汉式进行改进.

*1.使用同步方法,获取单例对象.*
>public Singleton
>{
>	public static Singleton instance;
>
>	private Singleton()
>	{}
>
>	public static synchronized Singleton getInstance()
>	{
>		if(instance == null)
>		{
>			instance = new Singleton();
>		}
>		return instance;
>	}
>}
*缺点*
同步方法会使该方法只能单个线程访问,效率降低.而可能出现线程安全的情况发生在对象还未创建时.

*2.使用双重验证方法*
>public Singleton
>{
>	public static Singleton instance;
>
>	private Singleton()
>	{}
>
>	public static Singleton getInstance()
>	{
>1		if(instance == null)
>		{
>2			synchronized (Singleton.class)
>			{
>				if (instance == null)
>3					instance = new Singleton();
>			}
>		}
>		return instance;
>	}
>}
*缺点*
由于位置3处的代码并非时原子操作,即jvm在运行时不会一步完成.他需要的步骤为:
1.为instance引用分配空间.
2.创建Singleton对象.
3.将instance指向对象所在地址.(这步之后instance != null)
但是jvm具有指令重排序优化.可能通过1-3-2的顺序执行.
假设线程A已运行到位置3处,并执行完1-3(注意:此时instance != null).这时另外一个线程B运行到位置1处,判断instance不为空返回并使用时就会报错.
*解决方案*
使用volatile关键字修饰instance变量.volatile会标记在jvm执行位置3处代码时不能对指令重新排序.
>public Singleton
>{
>	public static volatile Singleton instance;
>
>	private Singleton()
>	{}
>
>	public static Singleton getInstance()
>	{
>1		if(instance == null)
>		{
>2			synchronized (Singleton.class)
>			{
>				if (instance == null)
>3					instance = new Singleton();
>			}
>		}
>		return instance;
>	}
>}

