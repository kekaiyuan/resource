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
	- ![enter description here](/images/posts/java/exception/java-collection.jpg)
# Collection接口
- 常用方法
	- 集合作为容器应该具有的功能（增，删，改，查），不一定全有。
	- 集合的基本操作：增加，删除，判断，取出
	- ![enter description here](/images/posts/java/exception/java-collection-method.jpg)

# List与Set接口
- Collection 接口存储一组丌唯一，无序的对象
- List 接口存储一组丌唯一，有序（插入顺序）的对象
- Set 接口存储一组唯一，无序的对象
- Map接口存储一组键值对象，提供key到value的映射
	  ![enter description here](/images/posts/java/exception/java-collection-method.jpg)