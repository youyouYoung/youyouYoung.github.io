---
layout: post
title:  Java反射机制(二) --- Class类方法的使用
date:   2016-11-11 09:54:23
categories: Java
excerpt: Java反射机制学习第二弹 - 关于 Class 类中方法的使用
---

* content
{: toc}

## 功能介绍

---

该部分主要是对 `Class` 类中 `forName()`, `isAssignableFrom()`, `newInstance()`的使用介绍.

* `forName(param)` 是通过传入参数 `param` 获取一个 Class 对象的.  
  
* ``newInstance()` 可以创建一个该类的对象, 这相当于类加载器的功能.   
   
* `ClassA.isAssignableFrom(ClassB)` 用来判断 ClassB 是否是 ClassA 或者 ClassA 的子类.

---

## 实现方法

---

* 下面的代码描述了类之间的继承关系   
   
```Java
class Character {}
class Letter {}

class A extends Letter {}
class B extends Letter {}
class C extends Letter {}

class A1 extends A {}
class A2 extends A {}
class B1 extends B {}
class C1 extends C {}

class A1_1 extends A1 {}
class B1_1 extends B1 {}
class C1_1 extends C1 {}
```
  
  
* 下面的抽象类用于随机创建上述类对象的方法
   
```java
abstract class ClassCreator
{
	private static Random rand = new Random(47);
	//获取类集合的方法需要子类自定义实现
	public abstract List<Class<? extends Letter>> types();
	
	/** 随机创建一个 Letter 对象*/
	public Letter randomLetter()
	{
		int index = rand.nextInt(types().size());
		
		try
		{
			return types().get(index).newInstance();	//使用规定泛型的 Class 对象.
		}
		catch (InstantiationException | IllegalAccessException e)
		{
			e.printStackTrace();
		}
		return null;
	}
	
	/** 返回一个元素是随机生成的 Letter 数组 */
	public Letter[] createArray(int size)
	{
		Letter[] result = new Letter[size];
		for (int i = 0; i < size; i++)
		{
			result[i] = randomLetter();
		}
		
		return result;
	}
	
	/** 返回一个元素是随机生成的 Letter 的 List 集合 */
	public List<Letter> createList(int size)
	{
		List<Letter> resultList = new ArrayList<Letter>();
		Collections.addAll(resultList, createArray(size));	//Collections 是集合的操作类.
		return resultList;
	}
}
```
   
* 下面的类是 `ClassCreator` 的子类. 通过 `Class.forName(String)` 方法实现抽象方法
   
```java
/**
 * 使用 Class.forName() 实现类的加载. 是 ClassCreator 的子类.
 * 该类继承了 ClassCreator 以 forName() 实现其 types() 方法.
 * */
class ForNameCreator extends ClassCreator
{
	private static final List<Class<? extends Letter>> types = new ArrayList<Class<? extends Letter>>();
	
	private static final String[] typeNames = {
		"thinking_java.Letter",
		"thinking_java.A",
		"thinking_java.B",
		"thinking_java.C",
		"thinking_java.A1",
		"thinking_java.A2",
		"thinking_java.B1",
		"thinking_java.C1",
		"thinking_java.A1_1",
		"thinking_java.B1_1",
		"thinking_java.C1_1",
	};
	
	@SuppressWarnings("unchecked")
	private static void loader()
	{
		try
		{
			for (String typeName : typeNames)
			{
				types.add((Class<? extends Letter>)Class.forName(typeName));	//Class.forName()只能得到一个 Class<Object> 对象.
			}
		}
		catch (ClassNotFoundException e)
		{
			e.printStackTrace();
		}	
	}
	
	static {loader();}	//静态代码块, 在当前类首次被加载时调用. 完成对 types 数组的初始化.
	
	public List<Class<? extends Letter>> types()
	{
		return types;
	}
}


```
   
* 下面的类是 `ClassCreator` 的子类. 通过 `类字面常量(Letter.class)` 方法实现抽象方法
   
```java
/**
 * 使用 类字面常量 实现类的加载. 是 ClassCreator 的子类.
 * 该类继承了 ClassCreator 以 XX.class 实现其 types() 方法.
 * 
 * 该类是 ForNameCreator 类的升级版. 不在出现 Warning.
 * */
class LiteralCreator extends ClassCreator
{
	public static final List<Class<? extends Letter>> allTypes = Collections.unmodifiableList(Arrays.asList(
				Letter.class,
				A.class, B.class, C.class,
				A1.class, A2.class, B1.class, C1.class,
				A1_1.class, B1_1.class, C1_1.class
			));
	private static final List<Class<? extends Letter>> types = allTypes.subList(allTypes.indexOf(A.class), allTypes.size());
	
	public List<Class<? extends Letter>> types()
	{
		return types;
	}
}

```

* 下面的两个类是驱动类, 分别是对 `ForNameCreator` , `LiteralCreator` 的调用. 同时这两个类也分别实现了一个记录每个类产生的对象数量的功能.
此功能使用内部类实现. 通过对 `HashMap` 的装饰来完成.
   
```java
/**
 * 实现对 Letter 以及其 ForNameCreator 子类产生的对象数量进行计数.
 * Map 的键值对适合用于计数, key-代表类名称, value-对象数量
 * */
class LetterCount
{
	/**
	 * 该内部类实现对类对象数量的计数和跟踪.
	 * 
	 * 该类继承了 HashMap, 重新包装了 put() 方法.
	 * */
	private static class LetterCounter extends HashMap<String, Integer>
	{
		private static final long serialVersionUID = 1L;

		public void count(String typeName)
		{
			if (this.containsKey(typeName))
			{
				this.put(typeName, this.get(typeName)+1);
			}
			else
			{
				this.put(typeName, 1);
			}
		}
	}
	
	public static void countLetters(ClassCreator creator)
	{
		LetterCounter counter = new LetterCounter();
//		//用于记录随机生成的 Letter 对象都是那个类的实例, 被下面的更简便的代码替换了
//		for (Letter letter : creator.createArray(40))
//		{
//			System.out.print(letter.getClass().getSimpleName()+"; ");
//			if (letter instanceof A)
//			{
//				counter.add("A");
//				if (letter instanceof A1)
//				{
//					counter.add("A1");
//					if (letter instanceof A1_1)
//					{
//						counter.add("A1_1");
//					}
//				}
//				else if (letter instanceof A2)
//				{
//					counter.add("A2");
//				}
//			}//end if A
//			else if (letter instanceof B)
//			{
//				counter.add("B");
//				if (letter instanceof B1)
//				{
//					counter.add("B1");
//					if (letter instanceof B1_1)
//					{
//						counter.add("B1_1");
//					}
//				}
//			}//end if B
//			else if (letter instanceof C)
//			{
//				counter.add("C");
//				if (letter instanceof C1)
//				{
//					counter.add("C1");
//					if (letter instanceof C1_1)
//					{
//						counter.add("C1_1");
//					}
//				}
//			}//end if C
//		}//end for Letter Object
		
		//这种方法用于替换上一种 for 循环
		for (Letter letter : creator.createArray(50))
		{
			System.out.print(letter.getClass().getSimpleName()+"; ");
			for (Class<? extends Letter> classType : creator.types())
			{
				if (classType.isInstance(letter))
				{
					counter.count(classType.getSimpleName());
				}
			}
		}
		
		System.out.println();
		System.out.println(counter);
	}
	
	public static void main(String ... args)
	{
		countLetters(new ForNameCreator());
		
	}
}


/**
 * 实现对 Letter 以及其 LetterCount 子类产生的对象数量进行计数.
 * Map 的键值对适合用于计数, key-代表类, value-对象数量
 * */
class LetterCount2
{
	/** 用于记录类具有的对象的个数  */
	private static class LetterCounter extends LinkedHashMap<Class<? extends Letter>, Integer>
	{
		private static final long serialVersionUID = 1L;

		{
			for (Class<? extends Letter> type : LiteralCreator.allTypes)
			{
				put(type, 0);
			}
		}
		
		/** 用于计数 */
		public void count(Letter letter)
		{
			for (Class<? extends Letter> type : keySet())
			{
				if (type.isInstance(letter))
				{
					put(type, get(type)+1);
				}
			}
		}
		
		public String toString()
		{
			StringBuffer resultBuffer = new StringBuffer("{");
			
			for (Map.Entry<Class<? extends Letter>, Integer> entry : entrySet())
			{
				resultBuffer.append(entry.getKey().getSimpleName()).append("=").append(entry.getValue()).append(", ");
			}
			return isEmpty() ? "{}" : resultBuffer.delete(resultBuffer.length()-2, resultBuffer.length()).append("}").toString();
		}
	}
	
	public static void countLetters(ClassCreator creator)
	{
		LetterCounter letterCounter = new LetterCounter();
		
		for (Letter letter : creator.createArray(101))
		{
			System.out.print(letter.getClass().getSimpleName()+"; ");
			letterCounter.count(letter);
		}
		System.out.println();
		System.out.println(letterCounter);
	}
	
	public static void main(String ... args)
	{
		countLetters(new LiteralCreator());
	}
}

```

* 对于 `LetterCount`, `LetterCount2`类中都有一个内部类实现对对象数量的计数, 但是这些类都有一些局限性,为了实现更加广泛的使用下面的类对这些内部类进行了扩展.
   
```java
/**
 * 当前类是对 LetterCount* 类中的 LetterCounter 成员类进行的扩展.
 * LetterCounter 类只适用于对 Letter 类以及其子类产生的对象进行计数.
 * 
 * 当前类会在初始化时传入一个基类, 并在以后为基类以及基类的子类产生的对象计数.
 * */
@SuppressWarnings("serial")
class TypeCounter extends HashMap<Class<?>, Integer>
{
	private final Class<?> baseType;
	public TypeCounter(Class<?> baseType)
	{
		this.baseType = baseType;
	}
	
	public void count(Object object)
	{
		Class<?> currentType = object.getClass();
		
		if (this.baseType.isAssignableFrom(currentType))
		{
			countClass(currentType);
		}
	}
	
	private void countClass(Class<?> type)
	{
		if (containsKey(type))
		{
			put(type, get(type)+1);
		}
		else
		{
			put(type, 1);
		}
		
		if (this.baseType.isAssignableFrom(type.getSuperclass()))
		{
			countClass(type.getSuperclass());
		}
//		//不能使用下面这段代码的原因是因为 当前类和其父类有可能都实现了一个相同的接口
//		//这样对于一个从 count(Object object) 方法传过来的 class 可能会多次对一个接口进行对象数量加一
//		for (Class<?> interfaceType : type.getInterfaces())
//		{
//			if (this.baseType.isAssignableFrom(interfaceType))
//			{
//				countClass(interfaceType);
//			}
//		}
	}
	
	@Override
	public String toString()
	{
		StringBuffer resultBuffer = new StringBuffer("{");
		
		for (Map.Entry<Class<?>, Integer> entry : entrySet())
		{
			resultBuffer.append(entry.getKey().getSimpleName());
			resultBuffer.append("=");
			resultBuffer.append(entry.getValue());
			resultBuffer.append(", ");
		}
		
		return isEmpty() ? "{}" : resultBuffer.delete(resultBuffer.length()-2, resultBuffer.length()).append("}").toString();
	}
}
```

---

## 参考文献

---

* **Thinking Java Chapter14**