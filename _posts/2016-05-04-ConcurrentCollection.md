---
layout: post
title:  线程安全的集合
date:   2016-05-04 16:52:56
categories: Java.Collection
expert: 关于Java多线程中集合的学习
---

* content
{:toc}

## 集合

按照数据结构的不同可以将集合分为多种, 同时按照线程安全性可以将集合分为两种.   

* 线程安全的集合, 采用加锁技术使得多线程访问不会破坏集合的属性, 以及造成数据污染.   

* 非线程安全的集合, 不提供数据访问的保护. 在多线程访问时可能出现集合属性被破坏(集合大小)或者数据污染问题.   

所以在使用多线程访问集合时, 我们会使用具有线程安全的集合或者使用具有加锁机制的非线程安全集合. 这篇文章讲述一些线程安全集合的应用.   

---

## 阻塞队列

*队列*采用先进先出的方式管理数据. 使用费阻塞队列时有个非常大的问题: 不会对当前访问线程产生阻塞, 如生产者-消费者模型中使用这类队列时就必须额外实现同步机制和唤醒机制. 但是在多线程进行合作时如果某个线程向一个已经满了的阻塞队列添加数据或者从一个已经为空的阻塞队列获取数据时, 将导致线程阻塞. 当阻塞队列当前状态满足被阻塞线程需求时, 线程会被自动唤醒不需要我们实现这部分代码.   

* 阻塞队列的方法:   

| 处理方式	| 抛出异常	| 返回特殊值	| 一直阻塞	| 超时退出	|    
| :------ | :----: | :--------: | :-----: | :------: |    
| 插入方法	| `add(e)`	| `offer(e)`	| `put(e)`	| `offer(e, time, unit)`	|   
| 移除方法	| `remove()`	| `poll()`	| `take()`	| `poll(time, unit)`	|   
| 检查方法	| `element()`	| `peek()`	| 不可用	| 不可用	|   

> * 抛出异常一列会在无法操作阻塞队列时抛出异常   

> * 返回特殊值一列, `offer()`向满队列插入值时返回`false`, 在`poll()`从空队列取值时返回`null`, 在`peek()`从空队列取头部元素时返回`null`   

> * 一直阻塞一列, 当线程无法对队列进行操作时, 线程被阻塞.   

> * 超时退出一列, 当到达超时时仍无法操作, 会返回特殊值, 与返回特殊值一列相同.   

* `java.util.concurrent`包提供了阻塞队列的四个变种:   

1. `LinkedBlockingQueue`的容量没有上限, 但是也可以指定最大容量.   

2. `ArrayBlockingQueue`构造时需要指定最大容量, 并可以选择是否需要公平性. 如果公平性被设置, 那么每次在调用线程时会优先考虑等待时间长的线程. 但同时也会在性能上付出代价.   

3. `PriorityBlockingQueue`是一个带优先级的队列, 而不遵循先进先出, 该队列没有上限.    

4. `DelayQueue`实现了`Delayed`接口的对象. 每一个元素具有一个延迟只有当延迟用完的情况下才能从`DelayQueue`中移除.

**示例:**   

实现一个生产者-消费者模型   

```java

/** 产品类 */
class product
{}

/** 生产者 */
class process implements Runnable
{
	private BlockingQueue<product> queue;

	public process(BlockingQueue<product> queue)
	{
		this.queue = queue;
	}

	public void run()
	{
		queue.put(new product());
	}
}

/** 消费者 */
class consumer implements Runnable
{
	private BlockingQueue<product> queue;

	public consumer(BlockingQueue<product> queue)
	{
		this.queue = queue;
	}

	public void run()
	{
		product goods = queue.take();
	}
}

```

## 参考文献

* **[Java多线程理解：线程安全的集合对象](http://www.jianshu.com/p/eccb5f350c12)**   

* **[Java并发编程：阻塞队列](http://www.cnblogs.com/dolphin0520/p/3932906.html)**    

* **Java 核心技术卷II**   

* **[聊聊并发（七）——Java中的阻塞队列](http://www.infoq.com/cn/articles/java-blocking-queue)**
