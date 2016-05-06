---
layout: post
title:  Java线程池以及执行器的使用
date:   2016-05-06 09:28:59
categories: Java Thread
expert: 关于Java线程池,执行器使用方法的学习.
---

* content
{:toc}

## 执行器-Executor
---

`Executor`是一个用来执行被提交的`Runnable`对象的接口. 这个接口提供了一个将**对象的创建和线程运行机制(调度,执行等)**解耦的方法: `void executor(Runnable task)`, 通常我们会使用执行器而不是显示的创建线程执行.

``` java
//显示创建线程
new Thread(new RunnableTask()).start();

//使用Executor, 解耦创建对象和线程运行机制
Executor executor = new anExecutor();
executor.executor(new RunnableTask());

```

`void executor(Runnable task)`执行接收到的任务的方式有很多种. 有些可能并不是异步的执行任务, 比如下面的方法中executor直接在当前线程中执行被提交来的任务.   

**示例**   

``` java 

public class DirectExecutor implements Executor
{
	public void executor(Runnable task)
	{
		task.run();
	}
}

```

但是大多数情况下, `Executor`的实现类会单独创建一个线程用来执行接收到的任务.   

**示例**   

``` java

public class ThreadPerTaskExecutor implements Executor
{
	public void executor(Runnable task)
	{
		new Thread(task).start();
	}
}

```

很多的`Executor`实现类都会对提交来的任务的运行方式和运行时间做限制. 下面的这个示例中的`Executor`子类实现一个将任务提交和任务执行同时进行. **注意: 任务被执行的执行器并不是任务被提交到的执行器.**

``` java

public SerialExecutor implements Executor
{
	//保存提交上来的任务, 但在保存前需要对任务进行包装.
	final Queue<Runnable> tasks = new ArrayDeque<Runnable>();
	//最终执行任务的执行器.
	final Executor executor;
	//当前需要执行的任务.
	Runnable active;

	public SerialExecutor (Executor executor)
	{
		this.executor = executor;
	}

	//该执行程序只负责将提交的任务进行再包装, 同时如果是第一次调度此执行器的话, 使任务开始执行.
	public synchronized void executor(final Runnable task)
	{
		//将提交的任务进行包装.
		tasks.offer(new Runnable(){
			public void run()
			{
				try
				{
					task.run();
				}
				finally
				{
					//在当前任务调度完成后,执行下一个任务.
					scheduleNext();
				}
			}
		});

		//第一次调度此执行器.
		if (active == null)
		{
			scheduleNext();
		}
	}

	public synchronized void scheduleNext()
	{
		if ((active = tasks.poll()) != null)
		{
			//真正执行提交来的任务, 此方法的执行不影响当前对象调用自己的executor方法来向集合中添加新的任务.
			executor.executor(active);
		}
	}
}

```

`java.util.concurrent`包中`Executor`的实现类同时也实现了`ExecutorService`, 这是一个扩展性更广的接口. 其中, `ThreadPoolExecutor`类提供了可扩展的线程池的实现. `Executors`类实现了创建执行器的工厂方法.



## 参考文献

* **Java SE API**
