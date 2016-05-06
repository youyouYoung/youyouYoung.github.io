---
layout: post
title:  具有返回值的异步方法:Callable和Future
date:   2016-05-05 20:02:14
categories: Java Thread
expert: Java实现一个具有返回值的异步方法.
---

* content
{:toc}

## 来由
---

Java多线程实现了异步运行的任务. 我们通过实现`Runnable`接口的`run()`可以实现一个没有参数和没有返回值的异步方法:`public void run()`. 但是有时候也需要具有返回值的异步方法.   

## Callable
---

Callable与Runnable相似, 但它有返回值:   

``` java

public interface Callable<V>
{
	V call() throws Exception;
}

```

## Future
---

`Future`保存异步计算的结果. 在使用`Future`对象时传入一个Callable异步程序, 让该程序异步执行同时主程序干其他的事情, 在需要结果时调用`Future`得到它即可. `Future`具有下面的方法:   

``` java

public interface Future<V>
{
	//该方法的调用会被阻塞, 直到获得执行结果.
	V get() throw ...;

	//设置一个超时时间, 如果在规定时间内没有获得结果抛出异常.
	V get(long timeout, TimeUnit unit) throw ...;

	//取消计算, 如果计算还没开始那么停止执行. 如果正在执行且传入参数值为true, 线程中断.
	boolean cancel(boolean mayInterruptIfRunning);

	//如果在正常计算完之前被取消执行, 则返回true.
	boolean isCancelled();

	//执行结束返回true, 否则为false.
	boolean isDone()
}

```

## FutureTask
---

`FutureTask`是一种可以将`Callable`对象转化为`Runnable`或者`Future`的机制. 它同时实现了两种接口, 是`Runnable`和`Future`的子类. 一般的使用方法为:   

``` java

Callable<String> call = ....;
FutureTask<String> task = new FutureTask<String>(call);

//task is a Runnable.
new Thread(task).start();

//task is a Future.
String line = task.get();

```

* 示例:   

下面的代码实现了一个指定目录下计算包含关键字的文件数量的程序.   

``` java

import java.io.File;
import java.io.FileInputStream;
import java.io.IOException;
import java.util.ArrayList;
import java.util.Scanner;
import java.util.concurrent.Callable;
import java.util.concurrent.ExecutionException;
import java.util.concurrent.Future;
import java.util.concurrent.FutureTask;

/**
 * This task counts the files in a directory and its subdirectories 
 * that contain a given keyword.
 * */
public class MatchCounter implements Callable<Integer>
{
	private File directory;
	private String keyword;
	private int count;
	
	/**
	 * Constructs a MatchCounter.
	 * @param directory the directory in which to start the search
	 * @param keyword the keyword to look for
	 * */
	public MatchCounter(File directory, String keyword)
	{
		this.directory = directory;
		this.keyword = keyword;
	}
	
	public Integer call()
	{
		this.count = 0;
		try
		{
			File[] files = directory.listFiles();
			ArrayList<Future<Integer>> results = new ArrayList<Future<Integer>>();
			
			for (File file : files)
			{
				if (file.isDirectory())
				{
					MatchCounter counter = new MatchCounter(file, keyword);
					FutureTask<Integer> task = new FutureTask<Integer>(counter);
					results.add(task);
					new Thread(task).start();
				}
				else
				{
					if (search(file))
					{
						this.count++;
					}
				}
			}
			
			for (Future<Integer> result : results)
			{
				try
				{
					count += result.get();
				}
				catch (ExecutionException e)
				{
					// TODO: handle exception
					e.printStackTrace();
				}
			}
		}
		catch (InterruptedException e)
		{
			// TODO: handle exception
			Thread.currentThread().interrupt();
		}
		return count;
	}
	
	/**
	 * Searches a file for a given keyword.
	 * @param file the file to search
	 * @return true if the keyword is contained in the file.
	 * */
	public boolean search(File file)
	{
		try
		{
			Scanner in = new Scanner(new FileInputStream(file));
			boolean found = false;
			while (!found && in.hasNextLine())
			{
				String line = in.nextLine();
				if (line.contains(keyword))
				{
					found = true;
				}
			}
			in.close();
			return found;
		}
		catch (IOException e)
		{
			// TODO: handle exception
			return false;
		}
	}
}


import java.io.File;
import java.util.Scanner;
import java.util.concurrent.ExecutionException;
import java.util.concurrent.FutureTask;


public class FutureTest
{
	public static void main(String[] args)
	{
		Scanner in = new Scanner(System.in);
		System.out.print("Enter base directory (e.g. /home/youyou): ");
		String directory = in.nextLine();
		System.out.print("Enter keyword (e.g. lonely): ");
		String keyword = in.nextLine();
		
		MatchCounter counter = new MatchCounter(new File(directory), keyword);
		FutureTask<Integer> task = new FutureTask<Integer>(counter);
		new Thread(task).start();
		try
		{
			System.out.println(task.get() + "matching files.");
		}
		catch (ExecutionException e)
		{
			// TODO: handle exception
			e.printStackTrace();
		}
		catch (InterruptedException e) {
			// TODO: handle exception
			Thread.currentThread().interrupt();
		}
		finally
		{
			in.close();
		}
	}
}

```

## 参考文献

* **Java 核心技术卷II**
