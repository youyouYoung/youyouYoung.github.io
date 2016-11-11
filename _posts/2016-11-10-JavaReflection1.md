---
layout: post
title:  Java反射机制(一)
date:   2016-11-10 17:06:43
categories: Java
excerpt: 关于Java反射机制的学习, 本文是对反射机制的初步介绍.
---

* content
{: toc}

## 基础知识介绍  
  
---

为了实现运行期对**对象类型**进行识别, 可以使用 Java 的反射机制. 其实质是 Java 通过 `Class对象` 执行`RTTI`(运行时类型识别).  
  
 一个 Java 项目中所有并不是一开始就完全加载. 所有类都是在第一次使用时动态的加载到`JVM`中. 当程序创建第一个对类的静态成员的引用时, 就会加载这个类. 并且构造器也是静态方法.
  
在使用之前类加载器先判断当前类的`Class 对象`是否已经加载. 如果没有则加载相应的**.class 文件**.

一般情况下, JVM 在调用一个类之前都会经历一下三个过程:  
1. **加载**. 由 JVM 的类加载器查找字节码文件, 创建该类的 Class 对象.  
2. **链接**. 为静态域分配存储空间, 解析该类对其他类的引用.   
3. **初始化**. 首先初始化超类, 执行静态初始化器和静态代码块.   
  
---

## 代码实现

---

### 类的加载顺序

* 类中代码块和构造函数的执行顺序.  
  
下面的代码使用了 `Class.forName()` 加载类.   
``` java
//以下是关于类加载顺序的
//------------------------------------------
class Candy
{
	static {System.out.println("Loading Candy!");}
}

/** 对静态代码块/代码块的使用  */
class Gum
{
	/**
	 * 静态代码块在该类被初始化时执行.
	 * 用于初始化一些不变量, 且最先被执行.
	 * */
	static {System.out.println("Loading Gum!");}
	
	/**
	 * 普通代码块在创建该类的对象时被调用.
	 * 用于初始化与特定对象关联的变量.
	 * */
	{System.out.println("Using Gum!");}
	
	public Gum()
	{
		super();
	}
	
	public void eat()
	{}
}

class TestClass0
{
	static void driver()
	{
		System.out.println("Inside main");
		new Candy();
		System.out.println("After create Candy!");
		
		try
		{
			Class.forName("thinking_java.Gum");
		}
		catch (ClassNotFoundException e)
		{
			e.printStackTrace();
		}
		System.out.println("After Class.forName(\"Candy\")!");
		
		Gum gum = new Gum();
		System.out.println("After create Gum!");
		gum.eat();
	}
}
```

* 使用类字面常量研究静态常量的加载过程.
   
``` java
// 以下是对类的加载过程的研究 以及 类字面常量
// -------------------------------------------
class Initable
{
	static final int staticFinal = 47;			//普通静态常量(编译器常量), 在类没有加载前也可以调用
	static final int staticFinal2 = TestClass2.rand.nextInt(1000);	//引用其他类的静态常量
	static {System.out.println("Initializing Initable");}
}

class Initable2
{
	static int staticNonFinal = 472;
	static {System.out.println("Initializing Initable2");}
}

class Initable3
{
	static int staticNonFinal = 473;
	static {System.out.println("Initializing Initable3");}
}

class TestClass2
{
	public static Random rand = new Random(47);

	static void driver() throws Exception
	{
		Class<Initable> initable = Initable.class;				//使用类字面常量后, 类并没有被加载, 只有在类加载的时候才会调用静态代码块
		System.out.println("After creating Initable ref");
		System.out.println(Initable.staticFinal);				//使用静态常量后, 类并没有被加载.
		System.out.println("After using varible staticFinal");
		System.out.println(Initable.staticFinal2);				//使用引用了其他类的变量或方法, 类就会被加载
		
		System.out.println(Initable2.staticNonFinal);
		
		Class initable3 = Class.forName("thinking_java.Initable3");	//与类字面常量不同, Class.forName()方法会直接加载被调用类
		System.out.println("After creating Initable3 ref");
		System.out.println(Initable3.staticNonFinal);
	}
}

```

### `Class`类的方法使用
  
```Java
//以下是关于 Class 类方法的
//-------------------------------------------------
interface HasBatteries {}
interface Waterproof {}
interface Shoots {}

class Toy
{
	Toy() {}
	Toy (int i) {}
}

class FancyToy extends Toy implements HasBatteries, Waterproof, Shoots
{
	FancyToy() {super(1);}
}

class TestClass1
{
	static void printInfo(Class cc)
	{
		System.out.println("Name: " + cc.getName());	//获取类名称
		System.out.println("SimpleName: " + cc.getSimpleName());	//获取类名称
		System.out.println("CanonicalName: " + cc.getCanonicalName());	//获取类名称
		System.out.println("isInterface" + cc.isInterface());	//是否是接口
		System.out.println();
	}
	
	public static void driver()
	{
		Class c = null;
		try
		{
			c = Class.forName("thinking_java.FancyToy");	//通过类全称加载类
		}
		catch (ClassNotFoundException e)
		{
			e.printStackTrace();
		}
		printInfo(c);
		
		for (Class face : c.getInterfaces())	//遍历接口
		{
			printInfo(face);
		}
		
		Class up = c.getSuperclass();	//获取父类
		Object obj = null;
		try
		{
			obj = up.newInstance();
		}
		catch (InstantiationException | IllegalAccessException e)
		{
			e.printStackTrace();
		}
		printInfo(obj.getClass());
	}
}

```

### `Class`类泛型的使用
   
```java
//以下是对Class对象泛型的学习
//---------------------------------------------
class CountedInteger
{
	private static long counter;
	private final long id = counter++;
	public String toString() {return id+"";}
}

class FilledList<T>
{
	private Class<T> type;
	public FilledList(Class<T> type)
	{
		this.type = type;
	}
	public List<T> create(int nElements) throws Exception
	{
		List<T> result = new ArrayList<T>();
		for (int i = 0; i < nElements; i++)
		{
			result.add(type.newInstance());
		}
		
		return result;
	}
}

class TestClass3
{
	static void driver() throws Exception
	{
//		//没有泛型定义的引用可以接受任何对象.
//		Class intClass = int.class;
//		intClass = Object.class;
//		intClass = TestClass3.class;
//		
//		//Class 类的泛型通配符 ? 表示该引用可以指向任何对象
//		Class<?> intClass2 = int.class;
//		
//		//被泛型限定的引用只能接受泛型包含的对象
//		Class<Integer> IntClass = Integer.TYPE;		//每种基本类型包装类都有一个 TYPE 成员, 代表其基本类型的 Class 对象.
//		IntClass = Integer.class;
//		IntClass = int.class;
//		
//		//Integer 是 Number 的子类
//		//Class<Integer> 并不是 Class<Number> 的子类, 所以以下写法是错的
//		//Class<Number> numberClass = int.class;
//		Class<? extends Number> numberClass = int.class;	//如果需要将 Class 引用限定为某一特定类型或者其子类型可以使用该方法.
//		Number number = numberClass.newInstance();			//返回值可以用 Number 引用接收
		
//		Class<? super Integer> integerClass = Number.class;	//当前 class 可以接收Integer的类对象或者其父类的类对象.
//		Object object = integerClass.newInstance();			//返回值只能用 Object 引用接收
		
		FilledList<CountedInteger> fl = new FilledList<CountedInteger>(CountedInteger.class);
		System.out.println(fl.create(15));
	}
}

```
---

## 参考文献

---

* **Thinking Java Chapter14**