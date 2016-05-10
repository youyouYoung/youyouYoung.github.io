---
layout: post
title:  "Java多线程"
date:   2016-04-07 10:52:37
categories: Java Thread
excerpt: 对Java多线程的学习和使用
---

* content
{:toc}

## 线程状态

线程一共有四种状态:   

1. New (新生)   

2. Runnable (可运行)   

3. Blocked (被阻塞)   

4. Dead (死亡)   

**注意**

1. 可运行表示具有执行条件, 不等同于正在执行.   

## 中断线程  

中断线程表示在线程执行完`run()`之前结束线程的运行.可以通过`stop()`, `interrupt()`实现中断线程.但是`stop()`已经被弃用.   

`interrupt()`不会强制终端线程的执行,他的作用是请求终止该线程.每个线程都会有一个终端状态,是一个`boolean`类型的标志位.每个线程都应不断的检查该标志,以判断是否中断.   

检查该标志的方法为:   

        while (!Thread.currentThread().isInterrupted() && more work to do)
        {
		do more work.
        }

**注意**     
 
1. 如果对一个处于阻塞状态的线程调用`interrupt()`, 阻塞线程无法检查中断状态, 这时就会产生一个`InterruptedException`异常. 所以当对一个正在处于阻塞状态(使用阻塞方法,如`wait()`, `sleep()`)的线程调用`interrupt()`时, 阻塞方法会抛出`InterruptedException`异常.   

2. 没有任何语言方面的需求要求一个线程在接收到中断标志时应该终止. 中断一个线程只是为了引起该线程的注意, 而且由该线程决断如何处理中断.    

如果把线程的中断看作是终止线程, 一般的写法是:   

        public void run()
        {
		try
		{
			while (!Thread.currentThread().isInterrupted() && more work to do)
			{
				do more work.
			}
		}
		catch (InterruptedException e)
		{
			//thread was interrupted during wait or sleep.
		}
		finally
		{
			cleanup, if required.
		}
		//exiting the run method terminates the thread.
        }

**注意**

1. 阻塞调用方法(`sleep()`, `wait()`)在抛出`InterruptedException`后, 中断标志会被清除.   

2. 检查线程中断状态的方法有两个:`interrupted()`, `isInterrupted()`. 前者是一个静态方法. 调用此方法后该线程的中断标志会被清除. 后者是个实例方法, 调用此方法不会影响线程的中断标志.   

## 线程属性   

### 线程优先级

每个线程都会有一个优先级. 默认情况下线程的优先级和它的父线程一致. 一个线程的父线程为启动它的线程. 也可以通过`setPriority(int newPriority)` 方法改变线程优先级. 优先级对应的`int`值可以在`Thread`类的成员变量中找到.   

当执行程序在选择运行线程时, 它都会优先考虑优先级高的线程. 但是优先级高度依赖系统.   

Windows系统有7个优先级, 它会将Java的优先级映射到操作系统的优先级上. 而在Linux系统中线程的优先级会被忽略.   

### 守护线程

守护线程唯一的作用就是为其他线程提供服务. 计时器线程就是一个守护线程的例子, 可以定时为其他线程发送时间信息. 如果只剩下守护线程, JVM就会退出.   

可以通过`setDaemon(boolean on)` 方法将线程设置为守护线程.   

### 线程组

线程组实现了对功能类似线程的统一调度. 如停止一个浏览器程序所有关于获取图片的线程. 线程池也可以完成相同的功能.   

可以通过一下方式创建线程组:   

        String groupName = .....;
        ThreadGroup g = new ThreadGroup(groupName);

字符串`groupName`是用来唯一标识该线程组的, 必须时唯一的. 然后可以通过下面的方法将线程添加到线程组中.   

        Thread t = new Thread(g, "threadName");   

可以通过下面的方法查明某个线程组中仍处于可运行状态的线程数量:   

        if (g.activeCount() == 0)
        {
		//all thread in the group g have stopped.
        }

中断线程组中的所有线程:   

        g.interrupt();

>`java.lang.ThreadGroup`   
>
> * `ThreadGroup(ThreadGroup parent, String name)`    
>
>创建一个具有父线程组的线程组.   
>
> * `int enumerate(Thread[] list)`   
>
>得到线程组中所有的活线程的引用, 返回放入数组中线程的个数. 可以先通过`activeCount()`获取线程数, 在创建`Thread[]`大小.   
>如果活线程数大于`list`的长度, 那么将返回适量的线程.   
>

### 未捕获异常处理器

线程的`run()`不能抛出异常. 但是不被检查的异常可以导致线程的终止. 在线程死亡前, 异常将被传递给`未捕获异常处理器`来处理.   

未捕获异常处理器必须实现了`Thread.UncaughtExceptionHandler`接口. 此接口中只有一个方法:   
  
        uncaughtException(Thread t, Throwable e)

我们可以通过`setUncaughtExceptionHandler(Thread.UncaughtExceptionHandler eh) `来为一个线程设置未捕获异常处理器. 或者使用静态方法`Thread.setDefaultUncaughtExceptionHandler(Thread.UncaughtExceptionHandler eh)`为所有的线程设置一个通有的未捕获异常处理器.    

*注意*
如果没有为单独的线程安装未捕获异常处理器,此时的处理器就是线程的`ThreadGroup`对象. `ThreadGroup`实现了`Thread.UncaughtExceptionHandler`接口. 它的`uncaughtException()`方法操作如下:   

1. 如果有父线程组的话,调用父线程组的这个方法.   

2. 否则, 如果有`Thread.getDefaultUncaughtExceptionHandler() != null`调用该处理器.   

3. 否则, 判断该异常是否属于`ThreadDeath`对象, 如果是不做处理.   

4. 否则, 将堆栈踪迹输出到`System.err`.

## 同步

### 竞争条件

多线程应用中通常会有多个线程需要对同一个对象进行共享访问. 如果两个线程访问同一个对象, 而且每个线程都调用了可能改变对象状态的方法. 那么根据线程访问对象的不同顺序很可能会产生腐蚀的对象. 这种现象就叫做竞争条件.   

### 锁对象

旧版本的Java使用`Synchronized`关键字来实现锁. JDK 5.0引入了`ReentrantLock`类. 使用`ReentrantLock`类保护代码块的操作如下:    

        ReentrantLock myLock = new ReentrantLock();
        
        myLock.lock();
        try
        {
		critical section. //临界区, 需要同步的代码块
        }
        finally
        {
		myLock.unlock(); //make sure the lock is unlocked even if an exception is thrown.
        }   

*注意:*   

* 可以为每个需要被多线程共享的对象创建一个`ReentrantLock`对象, 这样在多个线程访问同一个对象时, 锁就会串行的服务于访问. 当访问不同的对象时, 每个线程都会得到不同的锁, 两者不会发生阻塞.    

* 锁是可重入的, 即线程可以重复获得它以拥有的锁. 如同一个对象中具有两个使用`myLock`锁的代码块, 如果一个线程已经进入了其中一个代码块, 那么他可以进入另外一个, 同时`myLock`对象持有锁的数量就增加变为2.   

>`java.util.concurrent.locks`   
>
> * `void lock()`   
>
> * `void unlock()`   
>
>`java.util.concurrent.locks.ReentrantLock`   
>
> * `ReentrantLock()`   
>
>创建一个可重入的锁.

### 条件对象(条件变量)

通常, 一个线程在进入临界区之后发现需要等待某个条件满足才能继续运行. 这是我们就需要使用*条件对象*来管理那些已经拥有锁却没有能力执行工作的线程. 有时条件对象也被称为条件变量.   

例如生产者和消费者的例子中, 当消费者获得锁时却发现产品对象为null, 这是就需要使用条件对象.   

一个锁对象可以有多个条件对象. 可以通过`LockObject.newCondition()`获得一个条件对象. 例如:   

        public class Product
        {
		private Lock lock;
		private Condition hasProduct;

		public Product()
		{
			lock = new ReentrantLock();
			hasProduct = lock.newCondition();
		}
        }   

如果当`product`对象没有被创建时, 他调用:    

        hasProduct.await();   

等待获得锁的线程和调用了`await()`方法的线程之间有一个本质的区别: 一旦一个线程调用了`await()`, 它就进入了等待该条件集中. 当锁可获得时线程不能立即解除阻塞. 它会维持责塞状态知道其他线程调用了*同一个条件*上的`signalAll()`.   

当生产者使`product != null`时, 它调用:   
 
        hasProduct.signalAll();

一般来说, `await()`的调用应该总是以下面的形式循环:   

        while (! (ok to proceed))
		condition.await();  

### synchronized关键字

使用synchronized相当于使用隐式锁, 并且每个锁都有一个隐式条件(可以调用`wait()`, `notifyAll()`, `notify()`).   

以下两种代码是等价的:   

        public synchronized void method()
        {
		method body
        }

等价于:   

        public void method()
        {
		lockObject.lock();
		try
		{
			method body
		}
		finally
		{
			lockObject.unlock();
		}
        }


### Volatile域

volatile关键字为一个实例的域提供了免锁机制. 在以下三个条件下对一个域的访问是安全的:  

* 域被volatile修饰.   

* 域被final修饰, 且已经被赋值.   

* 对域的访问有锁保护.   

### 读写锁

`java.util.concurrent.locks`包定义了两个锁类, 分别是`ReentrantLock`和`ReentrantReadWriteLock`. 当有很多线程都从某个数据结构中读取数据而很少有线程对其进行修改时, `ReentrantReadWriteLock`就有用了.   

* 创建一个`ReentrantReadWriteLock`对象:   

        private ReentrantReadWriteLock rwl = new ReentrantReadWriteLock();

* 获取读锁和写锁:   

        private Lock readLock = rwl.readLock();
        private Lock writeLock = rwl.writeLock();

## 参考

* **Java 核心技术卷II**
