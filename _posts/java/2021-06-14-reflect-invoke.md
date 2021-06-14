---
layout: post
title: Java反射之通过字符串调用类和方法
categories: Java
description: Java反射之通过字符串调用类和方法
keywords: Java，反射
---

如何利用Java的反射机制通过字符串调用同名的类和方法？

1. 通过类名创建对象
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
2. 找到要调用的方法
	- 现在Test类有三个test()方法，同名，但是参数不同，属于方法的重载
		```java
		public void Test() {
			System.out.println("I'm Test1");
		}

		public void Test(String str) {
			System.out.println("I'm Test2");
		}

		public void Test(String str, boolean b) {
			System.out.println("I'm Test3");
		}
		```
	- 通过字符串调用
	```java
	String methodName = "Test";
	Method method = object.getClass().getDeclaredMethod(methodName);
	method.invoke(object);

	method = object.getClass().getDeclaredMethod(methodName, String.class);
	method.invoke(object, "helloworld");

	method = object.getClass().getDeclaredMethod(methodName, String.class, boolean.class);
	method.invoke(object, "helloworld", true);
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
