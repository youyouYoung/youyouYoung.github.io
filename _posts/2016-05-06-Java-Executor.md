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

`Executor`是一个用来执行被提交的`Runnable`对象的接口. 这个接口提供了一个将**对象的创建和线程运行机制(调度,执行等)**解耦的方法: `void executor(Runnable task)`, 通常我们会使用执行器而不是显式的创建线程执行.

``` java
//显式创建线程
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

很多的`Executor`实现类都会对提交来的任务的运行方式和运行时间做限制. 下面的这个示例中的`Executor`子类实现将任务提交和任务执行同时进行的效果. **注意: 任务被执行的执行器并不是任务被提交到的执行器.**

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
		while ((active = tasks.poll()) != null)
		{
			//真正执行提交来的任务, 此方法的执行不影响当前对象调用自己的executor方法来向集合中添加新的任务.
			executor.executor(active);
		}
	}
}

```

`java.util.concurrent`包中`Executor`的实现类同时也实现了`ExecutorService`, 这是一个扩展性更广的接口. 其中, `ThreadPoolExecutor`类提供了可扩展的线程池的实现. `Executors`类实现了创建执行器的工厂方法.   

## ExecutorService
---

> public interface ExecutorService   
> extends Executor  

`ExecutorService`为`Executor`提供了三类方法:   

1. 关闭`Executor`的方法.   

2. 创建`Future`对象, 跟踪提交来的task的执行情况(通常为异步执行)的方法.   

3. 批量执行任务.   

> 如果`ExecutorService`对象使用了关闭方法, 那么它将不在接收新的任务. 该接口提供了两种关闭`ExecutorService`的方法, `shutdown()`方法允许执行器执行完目前已经接收到的任务. `shutdownNow()`方法将阻止处于等待状态的任务的执行, 同时企图停止掉正在执行的任务. 被终止之后的`ExecutorService`对象将没有任务处于执行窗台也没有任务处于等待状态更不会去接收新的任务. 这时应该回收处理器资源.   

> `submit`方法实现了上述功能. 该方法继承了`Executor.executor(java.lang.Runnable)`方法而且可以创建一个`Future`对象, 用于取消任务执行或者查看执行状态.   
> 
> `java.util.concurrent`   
> 
> 1. `Future<?> submit(Runnable task)`   
> 该方法返回一个`odd-look Future<?>`. 可以使用这个对象的`isDone()`,`cancel()`,`isCancelled()`但`get()`会返回`null`.   
> 
> 2. `<T> Future<T> submit(Runnable task, T result)`   
> 该方法提交一个Runable类的任务, 并在调用`Future.get()`时返回给定的result对象.   
> 
> 3. `<T> Future<T> submit(Callable<T> task)`   
> 该方法提交一个Callable类的任务, 并返回一个`Future`对象查看计算结果.   

> `invokeAny()`和`invokeAll()`方法是批量执行任务最常用的方法, 该方法可以执行一个任务集合(`Collection<Runnable/Callable>`)并等待其中一个任务或者全部任务执行完毕.   
> 
> 在处理一个搜索问题时如果可以接受任何解决方案, 那么就可以使用`invokeAny()`方法. 如需要对一个大整数进行因式分解来解码RSA密码.   
> 
> `ExecutorCompletionService`类可以将`invokeAll()`操作的结果按照获得结果的顺序保存起来.   

**示例**   

该示例实现了一个简单的网络服务, 这里的每个线程相当于一个`Socket`请求. 我们使用`Executors`的工厂方法创建执行器.   

``` java

public NetworkService
{
	private final ServerSocket serverSocket;
	private final ExecutorService pool;

	public NetworkService(int port, int poolSize) throws IOException
	{
		serverSocket = new ServerSocket(port);
		pool = Executor.newFixedThreadPool(poolSize);
	}
	
	//run this NetworkService
	public void startServer()
	{
		try
		{
			while (true)
			{
				pool.submit(new Handler(serverSocket.accept()));
			}
		}
		finally
		{
			//如果出现异常, 需要关闭资源. 这里只对ExecutorService对象做了详细的处理.
			shutdownResource();
		}
	}

	private void shutdownResource()
	{
		try
		{
			pool.shutdown();
			serverSocket.close();
			
			//Wait a while for existing tasks to terminate.
			if (!pool.awaitTermintion(60, TimeUnit.SECONDS))
			{
				pool.shutdownNow();
				//Wait a while for tasks to respond to being cancelled			if (!pool.awaitTermintion(60, TimeUnit.SECONDS))
					System.err.println("Pool did not terminate");
			}

		}
		catch (Exception e)
		{
			// (Re-)Cancel if current thread also interrupted
			pool.shutdownNow();
			// Preserve interrupt status
			Thread.currentThread().interrupt();
		}
	}
}

class Handler implements Runnable
{
	private final Socket socket;

	public Handler(Socket socket)
	{
		this.socket = socket;
	}

	public void run()
	{
		//do something about this socket.
	}
}

```

## ScheduledExecutorService
---

> java.util.concurrent
> 
> public interface ScheduledExecutorService   
> extends ExecutorService   

该接口是继承了`ExecutorService`接口. 它可以让提交来的任务在一定的延时之后执行或者让任务定期执行.   

> `<V> ScheduledFuture<V> schedule(Callable<V> callable, long delay, TimeUnit unit)`   
> `ScheduledFuture<?> schedule(Runnable command, long delay, TimeUnit unit)`   
> 
> > `long delay`延时的时间  
> > `TimeUnit unit`时间单位  
> 
> `schedule()`方法会在一定延时后执行接收的任务. 同时返回一个`Future`对象用来检查执行状态.   

> `ScheduledFuture<?> scheduleAtFixedRate(Runnable command, long initialDelay, long period, TimeUnit unit)`   
> `ScheduledFuture<?> scheduleWithFixedDelay(Runnable command, long initialDelay, long delay, TimeUnit unit)`   
> 
> > `long initialDelay`初始执行任务的延时  
> > `long delay`再次执行任务的时间间隔  
> 
> 定期执行接收到的任务直到被取消执行为止.   

**示例**   

该示例完成了一个在一小时时间内每间隔10秒蜂鸣一次的程序.   

``` java

class BeeperControl
{
	private final ScheduledExecutorService scheduled = Executors.newScheduledThreadPool(1);

	public void beepForAnHour()
	{
		final Runnable beeper = new Runnable(
			public void run()
			{
				System.out.println("beep");
			}
		);

		//将每10秒蜂鸣一次的任务交给scheduled.
		final ScheduledFuture<?> beeperHandler = scheduled.scheduleAtFixedRate(beeper, 10, 10, TimeUnit.SECONDS);

		//在一小时后结束蜂鸣任务.
		scheduled.schedule(new Runnable{
			public void run()
			{
				beeperHandler.cancel(ture);
			}
		}, 60*60, TimeUnit.SECONDS);
	}
}

```

## Executors
---

`Executor`类实现了`Executor`和`ExecutorService`. 该类是一个工厂类用于创建不同种类的执行器, 即线程池.   

| 方法			| 描述		|   
| :-----:	| :------	|   
| `newCachedThreadPool`	| 在需要时创建新线程, 空闲线程会被保留69秒 |   
| `newFixedThreadPool`	| 包含固定数量的线程, 空闲线程会被一直保留 |   
| `newSingleThreadExecutor` | 只有一个线程的线程池, 它会顺序执行提交的任务 |   
| `newScheduledThreadPool`  | 为预定执行而构建的固定线程池	 |   
| `newSingleThreadScheduledExecutor`	| 为预定执行而构建的单线程池 |  

使用线程池的理由:   

1. 程序中需要创建大量的生存周期很短的线程.   

2. 减少并发线程的数量, 创建大量的线程会导致性能降低甚至jvm崩溃.   

## 参考文献

* **Java SE API**
