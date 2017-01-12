---
layout: post
title:  Java 实现无重复随机过程
date:   2017-1-9 17:34:04
categories: Java
excerpt: 一次工作中, 为样本文件随机取样. 未经处理的随机方法会产生重复随机值, 但是对于当前这种应用环境是不允许的. 所以尝试一个无重复随机值制作
---

* content
{: toc}

## 功能介绍
   
---
   
在统计学中涉及到样本的概念, 是为某种情况下的特定采样. 在获取到这些样本之后需要对样本进行分析, 一般我们会从总样本中抽取部分作为子样本并用这部分去做分析.   
   
为了结果的准确性, 一般我们会选择随机从样本文件中抽取部分样本.   
   
这里就需要保证每个样本被抽到的概率是相同的, 同时每次抽取不能重复.
   
---

## 实现方法

---
   
为了显示上面的目标, 需要解决两件事情:   
1. 不重复随机  
2. 等概率抽取  
   
### 不重复随机数
   
实现不重复随机的做法很简单, 把每次随机的结果保存在一个集合中. 在下一次随机之后将结果与结合中的数进行比较, 如果不存在于集合中将这个结果添加到集合里面.
   
直到集合中元素的个数达到了子样本要求的样本数量就停止. 因为**哈希算法**使得一些使用了该算法的集合在查找对象时的时间复杂度等于O(1). 所以在这里把集合定义为 `Hash*` 类的集合是很明智的.
   
下面是示例:   
   
```Java
import java.util.Arrays;
import java.util.HashSet;
import java.util.Random;

/**
 * HashSet的内部其实使用HashMap实现的
 * */
public class RandomWithoutRepeat
{
	private int randomSize;
	private int topNum;
	public RandomWithoutRepeat(int randomSize, int topNum)
	{
		this.randomSize = randomSize;
		this.topNum = topNum;
	}
	
	
	
	//哈希表构建非重复随机数
	public int[] useHash()
	{
		Random random = new Random();
		
		HashSet<Integer> set = new HashSet<Integer>();
		while (set.size() < topNum)
		{
//			random.nextInt(randomSize);
			set.add(random.nextInt(randomSize));
		}
		
		int[] results = new int[topNum];
		int index = 0;
		for (Integer integer : set)
		{
			results[index++] = integer;
		}
		
		return results;
	}
}
```
   
另外, 实现不重复随机的目的可以采用下面这种思想:   
1. 创建一个总样本大小的数组, 数组中每个值都不重复. **先构建一个不重复的数组**
2. 随机选择两个下标, 如果这两个值不相等就把下标对应的数组中的值呼唤位置.  **随机互换数组中两个元素的位置**
3. 从数组中取出子样本数量长度的元素作为子数组, 子数组的值就是样本了.

```java
import java.util.Arrays;
import java.util.HashSet;
import java.util.Random;

public class RandomWithoutRepeat
{
	private int randomSize;
	private int topNum;
	public RandomWithoutRepeat(int randomSize, int topNum)
	{
		this.randomSize = randomSize;
		this.topNum = topNum;
	}
	
		
	//调换数组内容实现随机
	public int[] useArray()
	{
		Random random = new Random();
		int[] results = new int[randomSize];
		
		for (int i = 0; i < randomSize; i++)
		{
			results[i] = i + 1;
		}
		
		for (int i = 0; i < randomSize; i++)
		{
			int targetIndex = random.nextInt(randomSize);
			int temp = results[targetIndex];
			results[targetIndex] = results[i];
			results[i] = temp;
		}
		
		results = Arrays.copyOfRange(results, 0, topNum);
		return results;
	}
}
```

### 等概率抽取
   
蓄水池采样算法科室实现等概率随机抽样, 该算法的具体内容为:   
   
> 问题：要求从N个元素中随机的抽取k个元素，N值无法确定（N≥k）   
>  
> 解法：首先从N个元素中选取前k个元素放入“蓄水池”中，然后从第k + 1个元素开始，以k/(k+i) (i = 1,2,3 ... )的概率选择这个数，然后在“蓄水池”中随机选择一个数，并将其替换。
>	N个元素遍历完后，蓄水池中的k个数就是随机选择的。
>   
> 解释: 第 k+1 个元素被放进蓄水池的概率为: k/(k+1), 在第 k+1 个元素被处理过后(不论是否放入蓄水池)前 k 个元素每个元素存在在蓄水池中的概率是: 1-(1/k*k/(k+1)) = k/(k+1)   
>	即等于第 k+1 个元素被放进蓄水池的概率.   
>	
>	第 k+2 个元素被放进蓄水池的概率为: k/(k+2), 在第 k+2 个元素被处理过后(不论是否放入蓄水池)前 k+1 个元素每个元素存在在蓄水池中的概率是: k/(k+1)*(1-1/k*k/(k+2)) = k/(k+2)
>	即等于第 k+2 个元素被放进蓄水池的概率. 因为在处理 k+2 个元素时当前情景已经处于 k+1 的结果之上, 即条件概率所以要乘 k+1 后每个元素在蓄水池中的概率.   
>  
>	以此类推: 第 k+i 个元素被放入蓄水池的概率 = 前 k+i-1 个元素的概率 = k/(k+i)  
   
以 `k/(k+i)` 的概率抽取一个值的算法很简单, 可以认为是在一个箱子中放i个黑球放k个白球, 抽到白球的概率就是 k/(k+i).   
   
以下算法实现蓄水池理论:   
   
```Java
/**
 * 采用蓄水池采样算法实现的等概率随机抽样
 * 
 * 算法内容:
 * 问题：要求从N个元素中随机的抽取k个元素，N值无法确定（N≥k）. 
 * 解法：首先从N个元素中选取前k个元素放入“蓄水池”中，然后从第k + 1个元素开始，以k/(k+i) (i = 1,2,3 ⋯ )的概率选择这个数，然后在“蓄水池”中随机选择一个数，并将其替换。
 * 			N个元素遍历完后，蓄水池中的k个数就是随机选择的。
 * */
public class RandomWithoutRepeat
{
	private static Random random = new Random();
	/**
	 * 从该方法返回的数组中随机抽取一个元素, 得到 true 的概率为: (total-falseNumber)/total 得到 false 的概率为 falseNumber/total.
	 * 
	 * @param total 结果数组的长度
	 * @param falseNumber 结果数组中具有 false 的个数
	 * 
	 * 原理是把 falseNumber 个 false 值插入长度为 total 的全 true 数组中. 
	 * */
	private static boolean[] randomArray(int total, int falseNumber)
	{
		boolean[] resultArray = new boolean[total];
		Arrays.fill(resultArray, true);
		Random random = new Random();
		int count = 0;
		
		while (count < falseNumber)
		{
			int index = random.nextInt(total);
			if (resultArray[index])
			{
				resultArray[index] = false;
				count++;
			}
		}
		
		return resultArray;
	}
	
	/**
	 * 从整型数组中随机抽取一定量的值, 保证每个值被抽到的概率相等
	 * 
	 * @param allIndex 输入的整型数组, 返回结果中的值将从这里获取
	 * @param selectedIndexNumber 返回值数组的长度
	 * */
	public static int[] reservoirSampling(int[] allIndex, int selectedIndexNumber)
	{
		int[] resultArray = new int[selectedIndexNumber];
		for (int i = 0; i < selectedIndexNumber; i++)
			resultArray[i] = allIndex[i];
		
		for (int i = 0; i < allIndex.length-selectedIndexNumber; i++)
		{
			int total = selectedIndexNumber+i+1;
			int selectedIndex = random.nextInt(total);
			//构建一个选择骰子, 使得 allIndex[selectedIndexNumber+i] 的值被放入 resultArray 中的概率为 selectedIndexNumber/selectedIndexNumber+i+1
			boolean[] dice = randomArray(total, i+1);
			
			if (dice[selectedIndex])
			{
				//随机在 resultArray 中选择一个位置并将 allIndex[selectedIndexNumber+i] 的值放在当前位置处.
				int replacedIndex = random.nextInt(selectedIndexNumber);
				resultArray[replacedIndex] = allIndex[selectedIndexNumber+i];
			}
		}
		
		return resultArray;
	}
}
```

以上实现了等概率随机抽样的全部必要内容.
  
```
