---
layout: post
title: Java异常
categories: Java
description: some word here
keywords: Java，异常
---

什么是异常？
	异常是指在程序的运行过程中所发生的不正常的事件，它会中断正在运行的程序。
	除数为0，数组越界，空指针都属于异常。
	当异常发生时，程序会中断运行。
	
异常处理：
	Java编程语言使用异常处理机制为程序提供了错误处理的能力。
	加入异常处理后，程序遇到异常后不会中断，而是会转入异常处理程序，正确处理后继续运行。
	![enter description here](./images/posts/java/exception/java-exception-process.jpg)

关键字：
	try、catch、finally、throw、throws
	
捕获异常：
	![enter description here](./images/posts/java/exception/java-exception-catch.jpg)
```java
	try{
		//可能会发生异常的块
	}catch(ExceptionType e){
		//处理异常
	}finally{
		//无论是否发生异常都会执行的块
	}
```
try块中存放可能会发生异常的代码。
尽量放置少量的代码，做到精准地处理程序中的异常。
	
catch块将对异常进行处理。
参数为某具体的异常
![常见的异常类型](./images/posts/java/exception/java-exception-type.jpg)
可使用printStackTrace函数的堆栈跟踪功能显示出程序运行到当前类的执行流程

多重catch块
	▪ 引发多种类型的异常
		– 排列catch 语句的顺序：先子类后父类
```java
	try{
		...
	}catch (InputMismatchException e){
		...
	}catch (Exception e){
		...
	}
```
		– 发生异常时按顺序逐个匹配
		– 只执行第一个与异常类型匹配的catch语句

	
```java
	try{
		...
	}catch(ExceptionType1 e){
		//捕获到ExceptionType1的异常如何处理
		...
	}catch(ExceptionType2 e){
		//捕获到ExceptionType2的异常如何处理
		...
	}catch(ExceptionType3 e){
		//捕获到ExceptionType3的异常如何处理
		...
	}
	...
	
```

	


Exception是InputMismatchException的父类，所以处理Exception的catch块必须放在处理InputMismatchException的catch块的下面

finally块
	finally块中存放的代码是一定会被执行的，除了唯一一种情况：在try块或catch块中添加System.exit(1)

![常见的异常类型](./images/posts/java/exception/java-exception-class.jpg)
Throwable是所有异常的父类

声明异常
	有时可以将异常抛出，由外部的调用方法进行处理
```java
	public class ExceptionThrow {
		public static void main(String[] args) {
			try{
				test();
			}catch (Exception e){
				...
			}
    }

    public static void test() throws Exception{
        ...
    }
}
```

此时test方法会将异常抛出到main方法中进行处理

应用场景：
	多个方法之间的调用，每个方法中都可能出现异常。此时可选择将所有异常由最外层的方法进行统一处理。
	在程序的调用过程中，如果某方法的外层的调用方法有对异常的处理，那么可以直接使用throws，否则，需要使用try...catch...处理本方法中的异常
	
抛出异常
	

	


关于try、catch、finally与return的二三事

情况一：try中有return，finally中没有return
```java
	public class TryTest{
	
		public static void main(String[] args){
			System.out.println(test());
		}

		private static int test(){
			int num = 10;
			try{
				System.out.println("try");
				return num += 80;
			}catch(Exception e){
				System.out.println("error");
			}finally{
				if (num > 20){
					System.out.println("num : " + num);
				}
				System.out.println("finally");
			}
			System.out.println("method end");
			return num;
		}
	}
```
执行结果
```java
	try
	num : 90
	finally
	90
```
执行顺序：
1、“return num += 80;”被拆分成了“num = num+80;”和“return num;”两个语句
2、num = num+80;
3、finally块
4、return num;
5、外面的"System.out.println("method end");"则不会被执行

情况二：try和finally中均有return
```java
	public class TryTest{
			
		public static void main(String[] args){
			System.out.println(test());
		}
	
		private static int test(){
			try{
				return 80;
			}catch(Exception e){
			}finally{
				return 100;
			}
		}
	}
```
执行结果：
```java
	100
```
原因：try中的return语句会被finally中的return语句覆盖。

情况三：finally中改变返回值num
```java
	public class TryTest{
	
		public static void main(String[] args){
			System.out.println(test());
		}

		private static int test(){
			int num = 10;
			try{
				return num;
			}catch(Exception e){
			}finally{
				num = 100;
			}
			return num;
		}
	}
```

执行结果：
```java
	10
```

原因：在执行"return num;"语句时分为三步：
				一、存储num的值
				二、执行finally块
				三、返回第一步存储的num的值
				
情况四：将num的值包装在Num类中
```java
	public class TryTest{
		public static void main(String[] args){
			System.out.println(test().num);
		}

		private static Num test(){
			Num number = new Num();
			try{
				return number;
			}catch(Exception e){
			}finally{
				number.num = 100;
			}
			return number;
		}
	}

	class Num{
		public int num = 10;
	}
```

执行结果：
```java
	100
```

原因：try中的"return number;"存储的是number的引用，所以会被finally块修改其中的值

总结：
	 try语句在返回前，将其他所有的操作执行完，保留好要返回的值，而后转入执行finally中的语句，而后分为以下三种情况：
    情况一：如果finally中有return语句，则会将try中的return语句”覆盖“掉，直接执行finally中的return语句，得到返回值，这样便无法得到try之前保留好的返回值。
    情况二：如果finally中没有return语句，也没有改变要返回值，则执行完finally中的语句后，会接着执行try中的return语句，返回之前保留的值。
    情况三：如果finally中没有return语句，但是改变了要返回的值，这里有点类似与引用传递和值传递的区别，分以下两种情况：
        1）如果return的数据是基本数据类型或文本字符串，则在finally中对该基本数据的改变不起作用，try中的return语句依然会返回进入finally块之前保留的值。
        2）如果return的数据是引用数据类型，而在finally中对该引用数据类型的属性值的改变起作用，try中的return语句返回的就是在finally中改变后的该属性的值。
		
		
