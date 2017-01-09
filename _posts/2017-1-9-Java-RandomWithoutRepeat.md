---
layout: post
title:  Java 实现无重复随机过程
date:   2017-1-9 17:34:04
categories: Java
excerpt: 一次工作中, 为样本文件随机取样. 未经处理的随机方法会产生重复随机值, 但是对于当前这种应用环境是不允许的. 所以尝试一个无重复随机值制作
---

* content
{: toc}


``` java
package datastructures.random;

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
	
	public static void main(String[] args)
	{
		RandomWithoutRepeat random = new RandomWithoutRepeat(20000, 10000);
		int[] result;
		long before;
		long after;
		
		before = System.currentTimeMillis();
		result = random.useHash();
		after = System.currentTimeMillis();
		System.out.println("耗时: "+(after-before));
		System.out.println("长度: "+(result.length));
		for (int i : result)
		{
			System.out.print(i + ", ");
		}
		System.out.println();
		
		before = System.currentTimeMillis();
		result = random.useArray();
		after = System.currentTimeMillis();
		System.out.println("耗时: "+(after-before));
		System.out.println("长度: "+(result.length));
		for (int i : result)
		{
			System.out.print(i + ", ");
		}
	}
}


```