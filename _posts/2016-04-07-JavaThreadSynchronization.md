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

        while (!Thread.currentThread().isInterrupted() && *more work to do*)
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
			while (!Thread.currentThread().isInterrupted() && *more work to do*)
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

1. 阻塞调用方法(`sleep`, `wait`)在抛出`InterruptedException`后, 中断标志会被清除.   

2. 检查线程中断状态的方法有两个:`interrupted()`, `isInterrupted()`. 前者是一个静态方法. 调用此方法后该线程的中断标志会被清除. 后者是个实例方法, 调用此方法不会影响线程的中断标志.   

## 线程属性   

**1. 线程优先级**   

每个线程都会有一个优先级. 默认情况下线程的优先级和它的父线程一致. 一个线程的父线程为启动它的线程. 也可以通过`setPriority(int newPriority)` 方法改变线程优先级. 优先级对应的`int`值可以在`Thread`类的成员变量中找到.   

当执行程序在选择运行线程时, 它都会优先考虑优先级高的线程. 但是优先级高度依赖系统.   

Windows系统有7个优先级, 它会将Java的优先级映射到操作系统的优先级上. 而在Linux系统中线程的优先级会被忽略.   

**2. 守护线程**

守护线程唯一的作用就是为其他线程提供服务. 计时器线程就是一个守护线程的例子, 可以定时为其他线程发送时间信息. 如果只剩下守护线程, JVM就会退出.   

可以通过`setDaemon(boolean on)` 方法将线程设置为守护线程.   

**3. 线程组**

线程组实现了对功能类似线程的统一调度. 如停止一个浏览器程序所有关于获取图片的线程. 线程池也可以完成相同的功能.   

可以通过一下方式创建线程组:   

        String groupName = .....;
        ThreadGroup g = new ThreadGroup(groupName);

字符串groupName是用来唯一标识该线程组的, 必须时唯一的. 然后可以通过下面的方法将线程添加到线程组中.   

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
>`int enumerate(Thread[] list)`   
>
>得到线程组中所有的活线程的引用, 返回放入数组中线程的个数. 可以先通过`activeCount()`获取线程数, 在创建`Thread[]`大小.   
>如果活线程数大于`list`的长度, 那么将返回适量的线程.   
>

**4. 未捕获异常处理器**

线程的`run()`不能抛出异常. 但是不被检查的异常可以导致线程的终止. 在线程死亡前, 异常将被传递给`未捕获异常处理器`来处理.   

未捕获异常处理器必须实现了`Thread.UncaughtExceptionHandler`接口. 此接口中只有一个方法:   
  
        uncaughtException(Thread t, Throwable e)

我们可以通过`setUncaughtExceptionHandler(Thread.UncaughtExceptionHandler eh) `来为一个线程设置未捕获异常处理器. 或者使用静态方法`Thread.setDefaultUncaughtExceptionHandler(Thread.UncaughtExceptionHandler eh)`为所有的线程设置一个通有的未捕获异常处理器.    

*注意*
如果没有为单独的线程安装未捕获异常处理器,此时的处理器就是线程的`ThreadGroup`对象. `ThreadGroup`实现了`Thread.UncaughtExceptionHandler`接口.

## 同步的必要性

## 竞争的条件

## 锁对象

## 条件对象

## Synchronized关键字

## 同步块

## Volatile域关键字

## 死锁

## 读/写锁

## 参考

* **Java 核心技术卷II**
