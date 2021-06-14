---
layout: post
title: Java反射之通过字符串调用类和方法
categories: Java
description: Java反射之通过字符串调用类和方法
keywords: Java，反射
---

# 如何利用Java的反射机制通过字符串调用同名的类和方法？

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
	
	
2. 调用方法
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
		
	- 通过**字符串**调用
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
		- 需要注意，在使用`Class.getDeclaredMethod()方法`获取`Method`对象时，传参除了需要**方法名**，还需要该方法的**参数列表**，顺序不能颠倒。使用`Method.invoke()`调用方法时，也必须正确地**传参**。


	- 通过对象的**方法数组**调用方法
		```java
		//获取Method数组
		Method[] methods = object.getClass().getDeclaredMethods();
		for (Method m : methods) {
			System.out.println(m.toString());
		}
		```
		- 结果
		```java
		public static void com.kky.Test.main(java.lang.String[]) throws java.lang.Exception
		public void com.kky.Test.Test(java.lang.String)
		public void com.kky.Test.Test(java.lang.String,boolean)
		public void com.kky.Test.Test()
		```
		- 当`getDeclaredMethods()`方法无参时，返回的是一个`Method`数组，我们可以打印查看该对象具有的所有方法。
		- 调用方法**没有区别**，注意**传参**。
		```java
		//调用方法
		methods[3].invoke(object);
		methods[1].invoke(object, "helloworld");
		methods[2].invoke(object, "helloworld", true);
		```
		
# 全部代码
```java
package com.kky;

import java.lang.reflect.Method;

/**
* @author 柯凯元
* @create 2021/6/14 16:43
*/
public class Test {
	public static void main(String[] args) throws Exception {
		//要调用的类的包名+类名
		String className = "com.kky.Test";
		//找到对应的类
		Class clz = Class.forName(className);
		//创建对象
		Object object = clz.newInstance();

		//通过字符串找到同名方法并调用
		{
			String methodName = "Test";
			Method method = clz.getDeclaredMethod(methodName);
			method.invoke(object);

			method = clz.getDeclaredMethod(methodName, String.class);
			method.invoke(object, "helloworld");

			method = clz.getDeclaredMethod(methodName, String.class, boolean.class);
			method.invoke(object, "helloworld", true);
		}

		System.out.println("----------------");

		//通过getDeclaredMethods()查找对象所具有的所有方法并调用
		{
			//获取Method数组
			Method[] methods = object.getClass().getDeclaredMethods();
			for (Method m : methods) {
				System.out.println(m.toString());
			}

			//调用方法
			methods[3].invoke(object);
			methods[1].invoke(object, "helloworld");
			methods[2].invoke(object, "helloworld", true);

		}

	}

	public void Test() {
		System.out.println("I'm Test1");
	}

	public void Test(String str) {
		System.out.println("I'm Test2");
	}

	public void Test(String str, boolean b) {
		System.out.println("I'm Test3");
	}
}
```

# 代码链接
该文章源码链接[[https://github.com/kekaiyuan/javaquestion/tree/main/reflectinvoke](https://github.com/kekaiyuan/javaquestion/tree/main/reflectinvoke)](https://github.com/kekaiyuan/javaquestion/tree/main/reflectinvoke)
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
