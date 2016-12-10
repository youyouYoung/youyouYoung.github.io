---
layout: post
title:  一个比Thinking in Java中Stack示例更好的Stack
date:   2016-12-10 13:23:39
categories: Java
excerpt: 多谢这本书, 我用从里面学习到的知识修改了里面的一个示例. 并且认为我写的这个更好.
---

* content
{: toc}

## 功能介绍
   
---

最近在看 Thinking in Java 感觉受益匪浅. 偶然一个想法在我学习 15.2.2 节的泛型时冒出来, 并且我认为比我看到这本书中的这个案例要好一些.   
   
在此记录一下, 并分享给所有看到这些的人.   
   
---

## 详情
   
---

本书中的 15.2.2 节讲到了一个 `Stack` 类, 主要的作用是为了讲述泛型的使用: 书中原本的示例是:   
   
``` java
class LinkedStack<T>
{
	private static class Node<U>
	{
		U item;
		Node<U> next;
		
		Node() { item = null; next = null; }
		Node(U item, Node<U> next)
		{
			this.item = item;
			this.next = next;
		}
		
		boolean end() { return item == null && next == null; }
	}
	
	private Node<T> top = new Node<T>();	//end sentinel
	
	public void push(T item)
	{
		top = new Node<T>(item, top);
	}
	
	public T pop()
	{
		T result = top.item;
		if (!top.end())
		{
			top = top.next;
		}
		
		return result;
	}
}
```
   
个人认为这个示例的缺点在于:   
   
1. 构造函数 `Node()` 的唯一作用是用来**创建末端哨兵**. 那么这个构造方法很大程度上对我们理解该类造成困扰.   
2. 初始化 `Stack` 时的 `top` 变量作为一个哨兵我认为整个代码运行期间有一个就行(即作为单例存在.) 这样既节省了内存消耗同时让我们可以更准确的判断堆是否为空.  
   
当然这段代码中也有非常**显著的优点**, 比如:  
   
1. 内部类 `Node` 被定义为 `static`. 这样做的好处在于没创建一个对象不会产生一个新的 Class<Node> 对象.   
   
对于上述两个缺点, 我做了一下改进:   
   
``` java
class CustomStack<E>
{
	private static class Node<E>
	{
		public E element;
		public Node<E> next;
		private static final Node sentinel;
		
		static
		{
			/**
			 * 这里如果使用单例设计模式, 而不是静态代码块的话就不会出现没有泛型约束的 Warning. 
			 * 不过在了解了 Java 泛型擦除之后, 对这些 warning 已经不再强迫了.
			 * */
			sentinel = new Node<>();
		}
		
		private Node() {};
		Node(E element, Node<E> node)
		{
			this.element = element;
			this.next = node;
		}
		
		static Node getEndSentinel()
		{
			return sentinel;
		}
		
		boolean isEnd()
		{
			return this == sentinel;
		}
	}
	
	private Node<E> top = Node.getEndSentinel();
	
	public void push(E e)
	{
		top = new Node<E>(e, top);
	}
	
	public E pop()
	{
		E element = top.element;
		if (!top.isEnd())
		{
			top = top.next;
		}
		
		return element;
	}
	
	public boolean isEnd()
	{
		return top.isEnd();
	}
}
```