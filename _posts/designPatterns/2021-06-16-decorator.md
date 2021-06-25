---
layout: post
title: 设计模式之——装饰者 Decorator
categories: DesignPatterns
description: 设计模式之——装饰者 Decorator
keywords: Java，设计模式
---

设计模式之——装饰者 Decorator

## 前言

装饰者模式，是一种可以给对象**无限**加“**装饰**”的模式。

日常生活中，穿衣服是装饰行为。

我可以穿一件衣服、两件衣服、三件衣服……数量是无限的

我可以穿一件衬衫。
我可以穿一件衬衫 + 一件外套。
我可以穿一件衬衫 + 两件外套。
我可以穿两件衬衫。
我可以穿两件衬衫 + 一件外套。
搭配是无限的。

奶茶也一样。

我可以选择不加配料，一份配料，两份配料……只要杯子装得下，我可以想装多少装多少。

其配料的搭配也是无限的。

这就是装饰者模式。

## 概念

> 以下内容引用自 [菜鸟教程](https://www.runoob.com/design-pattern/decorator-pattern.html)
> 
> ### 装饰器模式
> 装饰器模式（Decorator Pattern）允许向一个现有的对象添加新的功能，同时又不改变其结构。这种类型的设计模式属于结构型模式，它是作为现有的类的一个包装。
> 
> 这种模式创建了一个装饰类，用来包装原有的类，并在保持类方法签名完整性的前提下，提供了额外的功能。
> 
> 我们通过下面的实例来演示装饰器模式的用法。其中，我们将把一个形状装饰上不同的颜色，同时又不改变形状类。
> 
> ### 介绍
> 意图
> - 动态地给一个对象添加一些额外的职责。就增加功能来说，装饰器模式相比生成子类更为灵活。
> 
> 主要解决
> - 一般的，我们为了扩展一个类经常使用继承方式实现，由于继承为类引入静态特征，并且随着扩展功能的增多，子类会很膨胀。
> 
> 何时使用
> - 在不想增加很多子类的情况下扩展类。
> 
> 如何解决
> - 将具体功能职责划分，同时继承装饰者模式。
> 
> 关键代码
> - Component 类充当抽象角色，不应该具体实现。
> - 修饰类引用和继承 Component 类，具体扩展类重写父类方法。
> 
> 应用实例： 
> - 孙悟空有 72 变，当他变成"庙宇"后，他的根本还是一只猴子，但是他又有了庙宇的功能。
> - 不论一幅画有没有画框都可以挂在墙上，但是通常都是有画框的，并且实际上是画框被挂在墙上。在挂在墙上之前，画可以被蒙上玻璃，装到框子里；这时画、玻璃和画框形成了一个物体。
> 
> 优点
> - 装饰类和被装饰类可以独立发展，不会相互耦合，装饰模式是继承的一个替代模式，装饰模式可以动态扩展一个实现类的功能。
> 
> 缺点
> - 多层装饰比较复杂。
> 
> 使用场景
> - 扩展一个类的功能。 
> - 动态增加功能，动态撤销。
> 
> 注意事项
> - 可代替继承。

## 代码示例
用装饰者模式实现想加多少配料就加多少配料的奶茶，并且搭配不限。

装饰者模式的关键在于：
**被装饰类和装饰类都必须继承于同一个类。**

- 抽象类

	Beverage 是被装饰类和装饰类共同的父类
	```java
	public abstract class Beverage {

		//饮料的描述
		protected String description = null;
		public String getDescription(){
			return description;
		}

		//饮料的价格
		public abstract double cost();
	}
	```
	
	配料装饰类
	```java
	//配料装饰类
	public abstract class CondimentDecorator extends Beverage{

		//被装饰的对象
		protected Beverage beverage = null;

		@Override
		public abstract String getDescription();
	}
	```
	
- 奶茶的茶底 绿茶


	```java
	public class GreenTea extends Beverage{

		public GreenTea(){
			this.description = "绿茶";
		}

		@Override
		public double cost() {
			return 8;
		}
	}
	```
	
- 奶茶的装饰 珍珠和椰果
	```java
	//珍珠
	public class Pearl extends CondimentDecorator{

		public Pearl(Beverage beverage){
			this.beverage = beverage;
		}

		//价格+1
		@Override
		public double cost() {
			return beverage.cost()+1;
		}

		//添加描述
		@Override
		public String getDescription() {
			return beverage.getDescription() + " + 珍珠";
		}
	}
	```
	```java
	//椰果
	public class Coco extends CondimentDecorator{

		public Coco(Beverage beverage){
			this.beverage = beverage;
		}

		//价格+2
		@Override
		public double cost() {
			return beverage.cost()+2;
		}

		//添加描述
		@Override
		public String getDescription() {
			return beverage.getDescription() + " + 椰果";
		}
	}
	```
	
- 测试
	```java
	public class Main {
		public static void main(String[] args) {
			Beverage beverage = new GreenTea();
			System.out.println(beverage.getDescription() + " : "+beverage.cost());

			beverage = new Pearl(beverage);
			System.out.println(beverage.getDescription() + " : "+beverage.cost());

			beverage = new Pearl(beverage);
			System.out.println(beverage.getDescription() + " : "+beverage.cost());

			beverage = new Coco(beverage);
			System.out.println(beverage.getDescription() + " : "+beverage.cost());

			beverage = new Coco(beverage);
			System.out.println(beverage.getDescription() + " : "+beverage.cost());

		}
	}
	```
	
- 输出
	```java
	绿茶 : 8.0
	绿茶 + 珍珠 : 9.0
	绿茶 + 珍珠 + 珍珠 : 10.0
	绿茶 + 珍珠 + 珍珠 + 椰果 : 12.0
	绿茶 + 珍珠 + 珍珠 + 椰果 + 椰果 : 14.0
	```
	
### 总结
通过测试代码可以看到，只要愿意，我们可以无限地给绿茶加装饰。
而且其描述和价格是动态改变的，不需要我们关心。

其关键在于：
1. 被装饰的类和装饰类继承于同一个类。<br>
	配料装饰类 CondimentDecorator 是可以省略的，直接让珍珠和椰果继承于 Beverage 类同样可以实现这个效果。<br>
	只不过把装饰分类设计上更科学。<br>
	也许我们给绿茶做的不只是配料的装饰，还可以在外包装上做装饰。<br>
	此时把装饰分类就是一件有意义的事情了。
2. 通过无限调用构造方式形成套娃，与递归函数很类似。


## 源码链接
该文章源码链接 [Github](https://github.com/kekaiyuan/designpatterns/tree/main/src/main/java/com/kky/dp/decorator)