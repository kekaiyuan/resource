---
layout: post
title: Java容器
categories: Java
description: some word here
keywords: Java, 容器
---

# 简介
- 使用场景
	- 不知道对象的具体数量，需要动态扩展
	- 对象的存储方式复杂
- Java集合框架包含的内容
	- Java集合框架提供了一套性能优良、使用方便的接口和类，它们位于java.util包中
	- ![enter description here](/images/posts/java/study/collection/collection.png)

# Collection接口
- 常用方法
	- 集合作为容器应该具有的功能（增，删，改，查），不一定全有。
	- 集合的基本操作：增加，删除，判断，取出
	- ![enter description here](/images/posts/java/study/collection/method.png)

# List与Set接口
- Collection 接口存储一组丌唯一，无序的对象
	- ![enter description here](/images/posts/java/study/collection/collection-sub.png)
	- List 接口存储一组丌唯一，有序（插入顺序）的对象
		- ![enter description here](/images/posts/java/study/collection/list.png)
	- Set 接口存储一组唯一，无序的对象
		- ![enter description here](/images/posts/java/study/collection/set.png)
- Map接口存储一组键值对象，提供key到value的映射
	  ![enter description here](/images/posts/java/study/collection/map.png)
	  
# List接口
- 特点：有序，丌唯一（可重复）
- 实现类
	- ![enter description here](/images/posts/java/study/collection/list-sub.png)
	- ArrayList
		- 实现了长度可变的数组，在内存中分配连续的空间。
			- ![enter description here](/images/posts/java/study/collection/arraylist.png)
		- 优点
			- 遍历元素和随机访问元素的效率比较高
		- 缺点
			- 添加和删除需要大量移动元素效率低
			- 按照内容查询效率低
	- LinkedList
		- 采用链表存储方式
			- ![enter description here](/images/posts/java/study/collection/linkedlist.png)
		- 优点
			- 插入、删除元素时效率比较高
		- 缺点
			- 遍历和随机访问元素效率低下
- 特有方法
	- 凡是可以操作索引的方法都是该体系特有方法
		- ![enter description here](/images/posts/java/study/collection/list-method.png)

# Iterator 接口
- 所有实现了Collection接口的容器类都有一个iterator方法用以返回一个实现了Iterator接口的对象。
- Iterator对象称作迭代器，用以方便的实现对容器内元素的遍历操作。
- Iterator接口定义了如下方法：
	- ```java
		boolean hasNext();   //判断是否有元素没有被遍历
		
		Object next();      //返回游标当前位置的元素并将游标移动到下一个位置
		
		void remove();      //删除游标左面的元素
												//在执行完next之后该操作只能执行一次
	  ```
	- 
			
			
			



k-v：session、json、Redis、HBase

treemap：红黑树