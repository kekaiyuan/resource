---
layout: post
title: Java反射之通过字符串调用类和方法
categories: Java
description: Java反射之通过字符串调用类和方法
keywords: Java，反射
---

如何利用Java的反射机制通过字符串调用同名的类和方法？

- 通过类名创建对象
	- 找到同名类并创建一个该类的实例
	```java
	//要调用的类的包名+类名
	String className = "com.kky.Test";
	//找到对应的类
	Class clz = Class.forName(className);
	//创建对象
	Object object = clz.newInstance();
	```
	- 这三步等于实现了
	```java
	Test object = new Test();
	```
```java
	
```
```java
	
```
```java
	
```
```java
	
```
```java
	
```
```java
	
```
```java
	
```
```java
	
```
```java
	
```
