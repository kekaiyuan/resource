---
layout: post
title: Java异常
categories: Java
description: some word here
keywords: Java，异常
---




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
3、finally代码块
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
				二、执行finally代码块
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

原因：try中的"return number;"存储的是number的引用，所以会被finally代码块修改其中的值

总结：
	 try语句在返回前，将其他所有的操作执行完，保留好要返回的值，而后转入执行finally中的语句，而后分为以下三种情况：

    情况一：如果finally中有return语句，则会将try中的return语句”覆盖“掉，直接执行finally中的return语句，得到返回值，这样便无法得到try之前保留好的返回值。

    情况二：如果finally中没有return语句，也没有改变要返回值，则执行完finally中的语句后，会接着执行try中的return语句，返回之前保留的值。

    情况三：如果finally中没有return语句，但是改变了要返回的值，这里有点类似与引用传递和值传递的区别，分以下两种情况：
        1）如果return的数据是基本数据类型或文本字符串，则在finally中对该基本数据的改变不起作用，try中的return语句依然会返回进入finally块之前保留的值。
        2）如果return的数据是引用数据类型，而在finally中对该引用数据类型的属性值的改变起作用，try中的return语句返回的就是在finally中改变后的该属性的值。
		
		
