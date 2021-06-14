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
		Method method = clz.getDeclaredMethod(methodName);
		method.invoke(object);

		method = clz.getDeclaredMethod(methodName, String.class);
		method.invoke(object, "helloworld");

		method = clz.getDeclaredMethod(methodName, String.class, boolean.class);
		method.invoke(object, "helloworld", true);
		```
		- 结果
		```
		I'm Test1
		I'm Test2
		I'm Test3
		```
		- 需要注意，在使用Class.getDeclaredMethod()方法获取Method对象时，传参除了需要方法名，还需要该方法的参数列表，顺序不能颠倒。使用Method.invoke()调用方法时，也必须传入对应的参数。
	
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
