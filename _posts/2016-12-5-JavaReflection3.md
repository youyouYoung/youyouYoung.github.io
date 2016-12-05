---
layout: post
title:  Java反射机制(三) --- 反射使用任意类中方法和代理
date:   2016-12-5 12:28:12
categories: Java
excerpt: Java反射机制学习第三弹 - 反射使用任意类中方法和代理
---

* content
{: toc}

## 功能介绍

---  

该部分是对 Java 反射的使用. 通过反射我们使用一个 Method 对象来执行该对象指定的方法.   
   
同时还对代理模式以及 Java 的动态代理思想的理解和简单的实现.   
   
---

## 实现方法

---

### 获取任意类中信息(RTTI)  
   
`RTTI` 指运行时类型识别. 用来在代码的运行期间识别某个类中具有的方法/成员信息. 以及对这些信息进行使用的思想.   
   
```java
/**
 * 通过反射获取类中的方法信息.
 * */
public class _Class_2
{
	/**
	 * 通过类名称 (className) 获取类中的方法介绍.
	 * 
	 * @param className 类名称. 必须是全称包含包名
	 * @param methodName 方法名称. 获取指定方法的介绍.
	 * @throws ClassNotFoundException 
	 * */
	public static void test0(String className, String... methodNames) throws ClassNotFoundException
	{
		if ("".equals(className))
		{
			System.err.println("没有指定类");
			System.exit(-1);
		}
		
		Pattern pattern = Pattern.compile("\\w+\\.");
		Class<?> c = Class.forName(className);
		Method[] methods = c.getMethods();
		Constructor<?>[] constructors = c.getConstructors();
		
		if (methodNames.length == 0)
		{
			for (Method method : methods)
			{
//				System.out.println(method.toString());
				System.out.println(pattern.matcher(method.toString()).replaceAll(""));
			}
			
			for (Constructor<?> constructor : constructors)
			{
//				System.out.println(constructor.toString());
				System.out.println(pattern.matcher(constructor.toString()).replaceAll(""));
			}
		}
		else
		{
			for (Method method : methods)
			{
				for (String methodName : methodNames)
				{
					if (method.toString().indexOf(methodName) != -1)
					{
						System.out.println(pattern.matcher(method.toString()).replaceAll(""));
					}
				}
				
			}
			
			for (Constructor<?> constructor : constructors)
			{
				for (String methodName : methodNames)
				{
					if (constructor.toString().indexOf(methodName) != -1)
					{
						System.out.println(pattern.matcher(constructor.toString()).replaceAll(""));
					}
				}
			}
		}
	}
	
	public static void main(String... args) throws ClassNotFoundException
	{
		test0("thinking_java._Class_2");
	}
	
	private void hiddenMethod() {}	//该方法不会被反射机制的 getMethods() 方法找到, 但是知道方法名称也可以被调用. 调用方法在下面的代理中使用了
}
```

### 代理
   
`proxy` 代理: 对实际对象的封装, 比如该示例中对一个 `Interface(下面代码中)` 的实际对象的封装. 代理持有这个对象并可以调用其中的方法甚至提供额外的或不同的操作. 相当于在实际对象上的另外一层装饰.
 * 持有代理对象的类不在需要直接对实际对象进行操作.

```Java
/**
 * 以下是关于代理模式的学习, 大多数代理会使用反射机制创建对象或调用类中的方法.
 * 
 * 所以学习代理的前提是对反射机制的了解
 * */

/**
 * 以下过程实现了一个简单的代理, 目的是为了理解代理的作用和功能.
 * */
interface Interface
{
	void doSomething();
	void somethingElse(String arg);
}

class RealObject implements Interface
{
	@Override
	public void doSomething() {System.out.println(getClass().getSimpleName() + " --- doSomething");}
	
	@Override
	public void somethingElse(String arg)
	{
		System.out.println(getClass().getSimpleName() + " --- somethingElse --- " + arg);
	}
}


/**
 * 下面的类是为了模拟代理的概念而实现的简单代理.
 * 
 * 代理: 对实际对象的封装, 比如该示例中对一个 Interface 的实际对象的封装. 代理持有这个对象并可以调用其中的方法甚至提供额外的或不同的操作. 相当于在实际对象上的另外一层装饰.
 * 持有代理对象的类不在需要直接对实际对象进行操作.
 * 
 * 使用的情况: 想要把额外的操作从实际对象中分离出来的时候
 * */
class SimpleProxy implements Interface
{
	private Interface proxied;
	public SimpleProxy(Interface proxied)
	{
		this.proxied = proxied;
	}
	
	@Override
	public void doSomething() 
	{
		System.out.println(getClass().getSimpleName() + " --- doSomething");
		proxied.doSomething();
	}
	
	@Override
	public void somethingElse(String arg)
	{
		System.out.println(getClass().getSimpleName() + " --- somethingElse --- " + arg);
		proxied.somethingElse(arg);
	}
}
```

### 动态代理

`动态代理类` 是 Java 反射中的一部分, 可以动态的生成一个代理, 并动态调用所代理的方法.   
   
`java.lang.reflect` 包提供了对动态代理的实现和接口.

```Java
/**
 * 动态代理类: 可以动态的生成一个代理, 并动态调用所代理的方法.
 * java.lang.reflect 包提供了对动态代理的实现和接口.
 * */
class DynamicProxyHandler implements InvocationHandler
{
	private Object proxied;
	public DynamicProxyHandler(Object proxied)
	{
		this.proxied = proxied;
	}
	
	/**
	 * @param proxy 是一个代理,和构造器传入的代理对象是等价的. 在invoke()方法中既可以使用参数 proxy 也可以使用成员 proxied 作为具体方法的处理者
	 * 					一般这个参数是为了区分请求的来源的
	 * */
	@Override
	public Object invoke(Object proxy, Method method, Object[] args) throws Throwable
	{
		//我们可以在这里加入自己的一些行为, 让代理在执行被代理的方法的同时可以执行自定义的其他行为.
		System.out.println(proxy.getClass().getSimpleName() + ": the proxy from invoke method.");
		System.out.println(getClass().getSimpleName() + " will invoke " + this.proxied.getClass().getSimpleName() + "'s method: " + method.getName() + " and their argument is ");
		if (args != null)
			for (Object object : args)
			{
				System.out.print(object);
			}
		else
			System.out.print("null");
		System.out.println();
		
		//下面是对方法真正的调用.
		return method.invoke(proxied, args);
	}
}

class simpleProxyTest
{
	public static void consumer(Interface iface)
	{
		iface.doSomething();
		iface.somethingElse("just kidding");
	}
	
	public static void main(String[] args)
	{
		RealObject realObject = new RealObject();
		consumer(realObject);
		
		SimpleProxy proxy = new SimpleProxy(new RealObject());
		consumer(proxy);

		//Proxy.newProxyInstance() 该静态方法可以创建一个动态代理, 第二个参数是希望得到的代理实现的接口(注意必须传入接口的类对象.)
		Interface dynamicProxy = (Interface)Proxy.newProxyInstance(realObject.getClass().getClassLoader(), new Class[]{Interface.class}, new DynamicProxyHandler(realObject));
		consumer(dynamicProxy);
	}
}
```

---

## 参考文献

---

* **Thinking Java Chapter14**