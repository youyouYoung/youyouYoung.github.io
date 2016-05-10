---
layout: post
title:  Java同步器
date:   2016-05-07 20:31:45
categories: Java Thread
expert: Java多线程中同步器的学习
---

* content
{:toc}

## 同步器
---

同步器是使一些线程可以等待另一个线程的对象, 允许他们协调动作. 多线程并发执行时线程之间会通过某种共享状态(如:匹配到关键字)来实现线程同步. 只有当状态满足XXX条件才能触发线程执行XXX.   

## CyclicBarrier
---

> public class CyclicBarrier   
> extends Object   

该类是一个同步器类, 它要求线程之间相互等待直到全部满足某个条件. `CyclicBarrier`对象适用于一系列县城对象需要不时地等待其他线程满足同一条件. 当所有线程都满足条件时它们会被释放而再次运行. 而且`CyclicBarrier`对象也可以被重用, 所以此对象是循环的.   

在`CyclicBarrier`对象初始化时可以传入一个`Runnable`对象, 在最后一个线程被阻塞之后而且所有线程释放之前, 该`Runnable`对象会执行一次. 这个线程一般用于更新线程的共享状态.   

**示例**   

这是一个使用*barrier*来完成矩阵分解设计的:

``` java

class Solver
{
	final int N; //矩阵行数
	final float[][] data;  //矩阵数据
	final CyclicBarrier barrier;

	//该类用于处理一行数据, 会在处理完成后调用barrier等待.
	class Worker implements Runnable
	{
		int myRow;

		public Worker(int rows)
		{
			this.myRow = rows;
		}

		public void run()
		{
			while (!isDone())
			{
				//计算该行数据
				processRow(myRow);

				try
				{
					barrier.await();
				}
				catch (InterruptedException ex)
				{
					return;
				}
				catch (BrokenBarrierException ex)
				{
					return;
				}
			}
		}
	}

	public Solver(float[][] matrix)
	{
		this.data = matrix;
		N = matrix.length;
		barrier = new CyclicBarrier(N, new Runnable{
			public void run()
			{
				//整合所有行计算线程的计算结果是否符合要求.
				//如果符合要求就将isDone()置为true.
				mergeRows(...);
			}
		});

		for (int i = 0; i < N; i++)
		{
			new Thread(new Worker(i)).start();
		}

		waitUntilDone();
	}
}

```

在这个示例中, `Worker`对象负责计算每一行的数据, 计算完成后等待在`barrier`位置直到所有线程计算完成. 当所有计算都完成后, 创建`CyclicBarrier`时传入的`Runnable`对象就会开始执行, 它会整合所有的行同时如果得到需要的结果后就会将`isDone()`方法置为`true`所有的线程将会终结.   

`CyclicBarrier`使用'0或全部(`all-or-none`)'损坏模型来处理同步失败的情况, 即如果一个线程由于其他原因(中断, 出错等)过早的离开了`barrier`位置的等待状态, 其他在`barrier`处等待的线程也会以抛出异常(`BrokenBarrierException`或`InterruptedException`)的方式停止在该点等待.   

## CountDownLatch
---

> public class CountDownLatch  
> extends Object

`CountDownLatch`是一种同步器, 它允许一个或多个线程等待其他线程完成操作后开始执行.   

`CountDownLatch`对象在创建时会设置一个计数器`count`. 调用了`await()`方法的线程会等待其他线程调用`CountDown()`来使`count--`, 当`count == 0`时调用了`await()`而处于阻塞状态的线程将会被释放, 而且之后再有线程调用`await()`时会立即释放. 计数器只能使用一次如果需要重复使用的话建议使用`CyclicBarrier`.  

`CountDownLatch`是一种多用途的同步器, 使用率较高. 如果在`CountDownLatch`创建时设置`count = 1`, 该对象就可以被看作一个为一个开关或门: 所有的线程都会在门前等待`await()`直到调用了`CountDown()`方法的线程打开这扇门. 如果`CountDownLatch`在初始化时设置`count = N`会让调用了`await()`的线程等待其它N个线程完成了某个任务或者一个线程N次完成某个任务.   

`CountDownLatch`一个非常好的特性是他不需要调用了`CountDown()`方法的线程等待直到`count == 0`, 该线程只是阻止了那些想在`count == 0`之前通过`await()`方法的线程.   

**示例**  

下面的示例中设置了两个`CountDownLatch`同步器.  

* 第一个是一个启动信号, 它在驱动类`<Driver`准备好之前阻止`Worker`对象的执行.  

* 第二个是一个完成信号, 它会在所有`Worker`对象完成之前阻止`Driver`的执行.  

``` java

class Driver
{
	//启动信号
	private CountDownLatch startSignal = new CountDownLatch(1);
	//完成信号
	private CountDownLatch doneSignal = new CountDownLatch(N);

	void main() throws InterruptedException
	{
		for (int i = 0; i < N; i++)
		{
			new Thread(new Worker(startSignal, doneSignal)).start();
		}

		//其他准备工作
		doSomethingElse();
		startSignal.countDown();
		doSomethingElse();
		doneSignal.await(); //等待Worker对象全部完成.
	}
}

class Worker implements Runnable
{
	private final CountDownLatch startSignal;
	private fianl  CountDownLatch doneSignal;

	public Worker(CountDownLatch start, CountDownLatch done)
	{
		this.startSignal = start;
		this.doneSignal = done;
	}

	public void run()
	{
		try
		{
			startSignal.await(); //等待driver准备好
			doWork();
			doneSignal.countDown();
		}
		catch (InterruptedException ex)
		{...}
	}

	private void doWork()
	{....}
}

```

使用`CountDownLatch`的另一类情况是: 一个问题可以被分为N部分, 每一个部分使用一个线程去执行在线程完成之后调用`CountDown()`方法, 可以使用线程池执行所有的线程. 当所有的线程都完成时(`count == 0`)整合的线程就可以通过`await()`方法的等待.(如果线程需要重复执行, 建议使用`CyclicBarrier`)

**示例**

``` java

class Driver2
{
	void main() throws InterruptedException
	{
		CountDownLatch doneSignal = new CountDownLatch(N);
		Executor executor = ...;
		
		for (int i = 0; i < N; i++)
		{
			//执行第i部分
			executor.executor(new WorkerRunnable(doneSignal, i));
		}

		doneSignal.await();
		doSomethingElse(); //所有部分完成后的整合
	}
}

class WorkerRunnable implements Runnable
{
	private final CountDownLatch doneSignal;
	private final int i;

	public WorkerRunnable(CountDownLatch done, int i)
	{
		this.doneSignal = done;
		this.i = i;
	}

	public void run()
	{
		try
		{
			doWork(i);
			doneSignal.countDown();
		}
		catch (InterruptedException ex)
		{...}
	}

	private void doWork(int i)
	{...}
}

```

## Exchanger
---

> public class Exchanger<v>   
> extends Object   

`Exchanger`是一个用于在两个线程之间交换数据的同步器. 一个线程将自身的一个对象传给`exchange()`方法同时等待另外的线程也调用`exchange()`, 然后这两个线程就可以互换自身调用`exchange`时上传的对象了. `Exchanger`类似一个双向通道用于两个线程之间传递信息.   

`Exchanger`常用于两个线程工作在统一数据缓冲区时, 一个线程向缓冲区填充数据另一个消费掉这些数据.   

**示例**   

该示例使用`Exchanger`在两个线程之间交换缓冲区, 其中一个向缓冲区填充数据另一个消费数据.   

``` java

class FillAndEmpty
{
	Exchanger<DataBuffer> exchanger = new Exchanger<DataBuffer>();
	DataBuffer initialEmptyBuffer = ...; //初始的空缓冲区,向其中填充数据
	DataBuffer initialFullBuffer = ...; //初始的满缓冲区,消费其中数据

	class FillingLoop implements Runnable()
	{
		public void run()
		{
			DataBuffer currentBuffer = initialEmptyBuffer;
			try
			{
				while (currentBuffer != null)
				{
					//向缓冲区填充数据
					addToBuffer(currentBuffer);

					if (currentBuffer.isFull())
					{
						currentBuffer = exchanger.exchange(currentBuffer);
					}
				}
			}
			catch (InterruptedException ex)
			{....}			
		}
	}

	class EmptyingLoop implements Runnable
	{
		public void run()
		{
			DataBuffer currentBuffer = initialFullBuffer;

			try
			{
				while (currentBuffer != null)
				{
					//从缓冲区中取数据
					takeFromBuffer(currentBuffer);

					if (currentBuffer.isEmpty())
					{
						currentBuffer = exchanger.exchange(currentBuffer);
					}
				}
			}
			catch (InterruptedException ex)
			{....}
		}
	}

	void start()
	{
		new Thread(new FillingLoop()).start();
		new Thread(new EmptyingLoop()).start();
	}
}

```

## Semaphore - 信号量
---

> public class Semaphore   
> extends Object   
> implements Serializable   

`Semaphore`是一种可计数的信号量, 它包含一定量的许可. 调用其中的`acquire()`方法会申请一个许可, 该方法是一个阻塞方法. 在当前没有许可可用时会阻塞调用该方法的线程. 每一个`release()`方法会释放一个许可. 许可其实不是一个实际的对象, `Semaphore`通过一个计数器来模拟许可的释放与回收.   

许可不需要有获取它的线程释放, 实际上任何线程都可以释放任意数量的许可. 如果被释放的许可大于创建时规定的最大值, 信号量只会被重新设置为最大值.   

`Semaphore`常用于限制线程通过某一种资源(物理资源或逻辑资源)的数量. 下面的示例使用`Semaphore`来限制通过a pool of items.   

**示例**   

``` java

class Pool
{
	private static final int MAX_AVAILABLE = 100;
	private final Semaphore available = new Semaphore(MAX_AVAILABLE, true);

	public Object getItem()
	{
		available.acquire();
		return getNextAvailableItem();
	}

	public void putItem(Object x)
	{
		//将object置为未使用, 如果该object之前被使用过那么需要释放许可
		if (markAsUnused(x))
		{
			available.release();
		}
	}

	protected Object[] items = ...; //whatever kind of items
	//使用中的item,其used=true. 否则为false.
	protected boolean[] used = new boolean[MAX_AVAILABLE];

	protected synchronized Object getNextAvailableItem()
	{
		for (int i = 0; i < MAX_AVAILABLE; i++)
		{
			if (!used[i])
			{
				return items[i];
			}
		}
		return null;
	}

	//被传入的对象之前被使用过返回true,否则返回false
	protected boolean markAsUnused(Object x)
	{
		for (int i = 0; i < MAX_AVAILABLE; i++)
		{
			if (items[i] == x)
			{
				if (used[i])
				{
					used[i] = false;
					return true;
				}
				else
					return false;
			}
		}
		reture false;
	}
}

```

示例中在获取一个`item`之前, 首先需要调用`acquire()`获取许可, 这样保证了有处于空闲状态的`item`可以被使用. 当线程使用完了`item`就将它返回到池中同时归还信号量, 这样其他需要使用`item`的线程就可以调度了. **注意**在调用`acquire()`方法时不能使用同步锁, 因为这会导致`item`无法回到池中.   

一个`Semaphore`对象如果只初始化了一个信号量, 会被用在那些只具有一个许可的条件下, 可以被当做是一种互斥访问锁使用. 也可以被称作`二进制信号量`因为他只具有两个状态许可可用和许可不可用. 二进制信号量有一个特殊性: 这个信号量可以被非信号量拥有线程(调用了`acquire()`方法的线程)释放, 在某些地方非常有用比如从死锁中恢复.   

`Semaphore`的构造器接收一个公平参数, 如果参数为`false`该对象无法保证线程获取到许可的顺序. 设置允许抢断许可, 即一个刚刚调用`acquire()`方法的线程可能比已经处于等待许可状态的线程更早的获得许可, 该线程会将自身放置在等待获取许可队列的最前端. 当公平参数被设置为`true`, `Semaphore`对象会保证调用了任何一种`acquire()`方法的线程, 会按照他们的`acquire()`方法被`Semaphore`对象接收的顺序来给他们许可(他们会被放在一个`FIFO`队列中). 因为`FIFO`队列需要依赖`Semaphore`对象内部的一些特性来执行这些`acquire()`方法, 所以有可能一些先调用了`acquire()`方法的线程会比后调用的线程晚到达`FIFO`队列中. 需要注意的是`tryAcquire()`方法不支持公平性参数, 它会使用任意可使用的许可.   

一般情况下, 如果使用`Semaphore`对象控制资源的访问需要将其初始化为公平的. 这样可以确保没有线程因为得不到资源而被饿死. 如果为其他种类的原因使用`Semaphore`实现同步控制时, 非公平性对象的吞吐量会高于公平性的.   

`Semaphore`类也提供了同时获取/释放多个许可的方法:`acquire(int permits)`, `release(int permits)`, 如果在公平参数设置为`false`的情况下使用这类方法很有可能处于无限期等待状态.

## 参考文献
---

* **[java并发编程之同步器](http://blog.csdn.net/huangbiao86/article/details/8089224)**   

* **[Java多线程（七）之同步器基础：AQS框架深入分析](http://blog.csdn.net/vernonzheng/article/details/8275624)**   

* **Java 核心技术卷II**   

* **Java SE API**
