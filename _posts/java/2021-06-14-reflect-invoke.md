---
layout: post
title: Java 反射之——通过字符串调用类和方法
categories: Java
description: Java 反射之——通过字符串调用类和方法
keywords: Java，反射
---

如何利用 Java 的反射机制通过字符串调用同名的类和方法？

# 过程详解

现在有一个 Test 类，如何通过反射调用其中的方法？

```java
public class Test {

	public Test(){}

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

1. 创建 Class 对象
	```java
	Class<Test> testClass = Test.class;
	```

2. 创建实例
	```java
	Object object = testClass.newInstance();
	```
	这里使用的 newInstance 并没有传参，也就是调用类的**无参构造器**创建实例。
	
	这一步非常关键。<br>
	现在很多的框架底层原理都是通过反射来生成实例。<br>
	所以在实际开发中，最好把每个类的无参构造器都写出来，而不是由 JVM 默认生成。<br>
	否则使用某些框架时会报错。
	
	
3. 查找方法并调用<br>
	无论是查找还是调用，都需要注意传入正确的参数列表。
	- 查找 Test() 并调用
		```java
		Method method = clz.getDeclaredMethod("Test");
		method.invoke(object);
		```
		结果
		```
		I'm Test1
		```		
	- 查找 Test(String str) 并调用
		```java
		method = clz.getDeclaredMethod("Test", String.class);
		method.invoke(object, "helloworld");
		```
		结果
		```
		I'm Test2
		```
	- 查找 Test(String str,boolean b) 并调用
		```java
		method = clz.getDeclaredMethod("Test", String.class, boolean.class);
		method.invoke(object, "helloworld", true);
		```
		结果
		```
		I'm Test3
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

# 源码链接
本文源码链接[https://github.com/kekaiyuan/javaquestion/tree/main/reflectinvoke](https://github.com/kekaiyuan/javaquestion/tree/main/reflectinvoke)

