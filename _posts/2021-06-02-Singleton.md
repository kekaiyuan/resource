---
layout: post
title: 单例模式
categories: [cate1, cate2]
description: some word here
keywords: keyword1, keyword2
---

应用场景：
只需要一个实例，比如各种Mgr和Factory

单例模式的演化之路

```java
/*
* 饿汉式
* 类加载到内存后，就实例化一个单例，JVM保证线程安全
* 简单使用，推荐！
* 唯一缺点：不管用到与否，类装载时就完成实例化
* */
public class Mgr01 {

    private static final Mgr01 INSTANCE = new Mgr01();
	//将构造函数设置为private，这样外部类就无法创建实例，单例模式的关键点
    private Mgr01(){}
	
    public static Mgr01 getInstance(){
        return INSTANCE;
    }
	
}
```
```java
/*
* 与Mgr01一样，只是写法略有不同
* */
public class Mgr02 {

    private static final Mgr02 INSTANCE;
	
    static {
        INSTANCE = new Mgr02();
    }
	
    private Mgr02(){}
	
    public static Mgr02 getInstance(){
        return INSTANCE;
    }

}
```


```java
/*
* 懒汉式
* 用的时候才初始化
* 多线程并发访问时容易创建出多个实例，无法达成单例的目的
* */
public class Mgr03 {

    private static Mgr03 INSTANCE;

    private Mgr03(){}

    public static Mgr03 getInstance() {
        if(INSTANCE == null){
            INSTANCE = new Mgr03();
        }
        return INSTANCE;
    }

}

```
```java
/*
* Mgr03的改进，实现线程同步，但是效率会下降
* */
public class Mgr04 {

    private static Mgr04 INSTANCE;

    private Mgr04(){}

    public static synchronized Mgr04 getInstance() {
        if(INSTANCE == null){
            INSTANCE = new Mgr04();
        }
        return INSTANCE;
    }

}

```
```java
/*
* Mgr04的改进，尝试减小同步代码块，结果不可行
* */
public class Mgr05 {

    private static Mgr05 INSTANCE;

    private Mgr05(){}

    public static Mgr05 getInstance() {
        if(INSTANCE == null){
            //该方法不可行！！!
            synchronized (Mgr05.class){
				INSTANCE = new Mgr05();
            }        
        }
        return INSTANCE;
    }
}


```
```java
/*
 * Mgr05的改进，双重检查
 * 完美版本1
 * */
public class Mgr06 {

    private static Mgr06 INSTANCE;

    private Mgr06(){}

    public static Mgr06 getInstance() {
        if(INSTANCE == null){
            //双重检查
            synchronized (Mgr06.class){
                if(INSTANCE == null){
                   INSTANCE = new Mgr06();
                }
            }
        }
        return INSTANCE;
    }

}
```
```java
/*
* 静态内部类方式
* 由JVM保证单例
* 加载外部类时不会加载内部类，实现懒加载
* 完美版本2
* */
public class Mgr07 {

    private Mgr07(){}

    private static class Mgr07Holder{
        private final static Mgr07 INSTANCE = new Mgr07();
    }

    public static Mgr07 getInstance(){
        return Mgr07Holder.INSTANCE;
    }
	
}

```
```java
/*
* 不仅可以解决线程同步，还可以实现反序列化
* 完美版本3
* */
public enum Mgr08 {

    INSTANCE;

}
```
如何检查是否是单例模式？
```java
//多线程访问，通过hashcode检验是否是单例模式
for(int i=0;i<100;i++){
	new Thread(()-> System.out.println(XXX.getInstance().hashCode())).start();
}
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