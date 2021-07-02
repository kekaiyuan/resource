---
layout: post
title: 设计模式之——单例 Singleton
categories: DesignPatterns
description: 设计模式之——单例 Singleton
keywords: Java，设计模式
---

设计模式之——单例 Singleton

## 概念

> 以下内容引用自 [菜鸟教程](https://www.runoob.com/design-pattern/singleton-pattern.html)
> 
> ### 单例模式
> 单例模式（Singleton Pattern）是 Java 中最简单的设计模式之一。这种类型的设计模式属于创建型模式，它提供了一种创建对象的最佳方式。
> 
> 这种模式涉及到一个单一的类，该类负责创建自己的对象，同时确保只有单个对象被创建。这个类提供了一种访问其唯一的对象的方式，可以直接访问，不需要实例化该类的对象。
> 
> ### 注意
> 
> - 单例类只能有一个实例。
> - 单例类必须自己创建自己的唯一实例。
> - 单例类必须给所有其他对象提供这一实例。
> 
> ### 介绍
> 意图
> - 保证一个类仅有一个实例，并提供一个访问它的全局访问点。
> 
> 主要解决
> - 一个全局使用的类频繁地创建与销毁。
> 
> 何时使用
> - 当您想控制实例数目，节省系统资源的时候。
> 
> 如何解决
> - 判断系统是否已经有这个单例，如果有则返回，如果没有则创建。
> 
> **关键代码**
> - 构造函数是**私有**的。
> 
> 应用实例
> - 一个班级只有一个班主任。
> - Windows 是多进程多线程的，在操作一个文件的时候，就不可避免地出现多个进程或线程同时操作一个文件的现象，所以所有文件的处理必须通过唯一的实例来进行。
> - 一些设备管理器常常设计为单例模式，比如一个电脑有两台打印机，在输出的时候就要处理不能两台打印机打印同一个文件。
> 
> 优点
> - 在内存里只有一个实例，减少了内存的开销，尤其是频繁的创建和销毁实例（比如管理学院首页页面缓存）。
> - 避免对资源的多重占用（比如写文件操作）。
> 
> 缺点
> - 没有接口，不能继承，与单一职责原则冲突，一个类应该只关心内部逻辑，而不关心外面怎么样来实例化。
> 
> 使用场景：
> - 要求生产唯一序列号。
> - WEB 中的计数器，不用每次刷新都在数据库里加一次，用单例先缓存起来。
> - 创建的一个对象需要消耗的资源过多，比如 I/O 与数据库的连接等。
> 
> 注意事项
> - getInstance() 方法中需要使用同步锁 synchronized (Singleton.class) 防止多线程同时进入造成 instance 被多次实例化。

## 类型
### 饿汉式
```java
/*
* 饿汉式
* 类加载到内存后，就实例化一个单例，JVM保证线程安全
* 简单使用，推荐！
* 唯一缺点：不管用到与否，类装载时就完成实例化
* 经典中的经典
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

### 懒汉式
```java
/*
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
### 双检锁
```java
/*
 * Mgr05的改进，双重检查
 * 完美版本1
 * */
public class Mgr06 {

	//使用volatile关键字，防止JVM内部语句重排后，没有初始化就放回INSTANCE
    private static volatile Mgr06 INSTANCE;

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
- 指令重排<br>
  在编译阶段，Java会被编译为汇编语言。<br>
  一句Java会被编译多句汇编语言。<br>
  此时可能会发生只创建了 INSTANCE 对象，但没有正确地设置初值的情况。<br>
  此时得到的单例是半初始化的实例。<br>
  该情况几率很低，极难复现，百万级别的并发下可能会出现。<br>
- volatile 关键字
	- 线程间可见
	- 防止指令重排


### 静态内部类
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

java的反射可以通过class文件new实例，以上七种办法都无法抵挡。
想要防止反序列化，需要设置一些内部变量，非常复杂。
最简单的办法就是枚举单例，因为枚举类没有构造方法。

### 枚举

```java
/*
* 不仅可以解决线程同步，还可以防止反序列化
* 完美版本3
* */
public enum Mgr08 {

    INSTANCE;

}
```
## 如何检查是否是单例模式？
```java
//多线程访问，通过hashcode检验是否是单例模式
for(int i=0;i<100;i++){
	new Thread(()-> System.out.println(MgrXX.getInstance().hashCode())).start();
}
```

## 源码链接
该文章源码链接 [Github](https://github.com/kekaiyuan/designpatterns/tree/main/src/main/java/com/kky/dp/singleton)
