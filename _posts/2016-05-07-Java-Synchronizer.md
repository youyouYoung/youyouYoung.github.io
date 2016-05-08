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

## 参考文献
---

* **[java并发编程之同步器](http://blog.csdn.net/huangbiao86/article/details/8089224)**   

* **[Java多线程（七）之同步器基础：AQS框架深入分析](http://blog.csdn.net/vernonzheng/article/details/8275624)**   

* **Java 核心技术卷II**
