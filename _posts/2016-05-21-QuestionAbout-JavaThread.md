---
layout: post
title:  关于Java多线程的问题的解决
date:   2016-05-21 10:15:15
categories: Java
expert: "Java多线程虽然常被用到,但是有一些问题还是需要考虑"
---

* content
{:toc}

## 父线程和子线程的生存周期

**Q: 如果父线程运行完成, 子线程一定运行完成了吗?**

**A: 不会, 子线程会在执行完代码后结束.**

**证明示例**

以下示例中, 父线程执行结束后会返回一个`Integer`对象, 以此来判断线程是否执行完成. 子线程使用`sleep()`方法确保运行时间较长.

``` java
public static void test5()
	{
		FutureTask<Integer> task = new FutureTask<Integer>(new Callable<Integer>()
		{
			//父线程的执行方法
			public Integer call()
			{
				System.out.println("这是父线程");
				new Thread(new Runnable()
				{
					//子线程的执行方法
					@Override
					public void run()
					{
						// TODO Auto-generated method stub
						System.out.println("这是子线程,开始");
						try
						{
							Thread.sleep(10000);
							System.out.println("这是子线程,结束");
						}
						catch (InterruptedException e)
						{
							// TODO Auto-generated catch block
							e.printStackTrace();
						}
					}
				}).start();
				
				return new Integer(1);
			}
		});
		
		new Thread(task).start();
		try
		{
			int i = task.get().intValue();
			System.out.println("父线程结束运行,子线程输出了吗?" + i);
		}
		catch (InterruptedException | ExecutionException e)
		{
			// TODO Auto-generated catch block
			e.printStackTrace();
		}
		
	}
```
