---
layout: post
title: 设计模式之——责任链 Chain of Responsibility
categories: DesignPatterns
description: 设计模式之——责任链 Chain of Responsibility
keywords: Java,设计模式
---

设计模式之——责任链 Chain of Responsibility

# 什么是责任链？

现在有一款坦克大战的小游戏，里面有三种对象，坦克、子弹、墙

当坦克和坦克碰撞时，两辆坦克都会停下

当坦克和子弹碰撞时，会发生爆炸，坦克和子弹都被摧毁

当坦克和墙碰撞时，坦克会停下

当子弹和墙碰撞时，子弹会被摧毁

这时候应该用什么实现？

是用一个非常大的循环，把四种判断都加进去吗？

那如果现在有七八个对象呢？

所以这时候就需要一个链条。

对于任意的两个物体，把他们放入链条中：
- 第一环是坦克之间的碰撞逻辑
- 第二环是坦克和子弹的碰撞逻辑
- 第三环是坦克和墙的碰撞逻辑
- 第四环是子弹和墙的碰撞逻辑

![enter description here](/images/posts/designpatterns/chain-of-responsibility/process.png)
 
这就是责任链，把对某种对象的处理流程组成一个链条，这样只需要把对象放入链条中，它就会被正确的处理 

# 概念

> 以下内容引用自[https://www.runoob.com/design-pattern/chain-of-responsibility-pattern.html](https://www.runoob.com/design-pattern/chain-of-responsibility-pattern.html)
> 
> ### 责任链模式
> 顾名思义，责任链模式（Chain of Responsibility Pattern）为请求创建了一个接收者对象的链。这种模式给予请求的类型，对请求的发送者和接收者进行解耦。这种类型的设计模式属于行为型模式。
> 
> 在这种模式中，通常每个接收者都包含对另一个接收者的引用。如果一个对象不能处理该请求，那么它会把相同的请求传给下一个接收者，依此类推。
> 
> ### 介绍
> 意图
> - 避免请求发送者与接收者耦合在一起，让多个对象都有可能接收请求，将这些对象连接成一条链，并且沿着这条链传递请求，直到有对象处理它为止。
> 
> 主要解决
> - 职责链上的处理者负责处理请求，客户只需要将请求发送到职责链上即可，无须关心请求的处理细节和请求的传递，所以职责链将请求的发送者和请求的处理者解耦了。
> 
> 何时使用
> - 在处理消息的时候用以过滤很多道。
> 
> 如何解决
> - 拦截的类都实现统一接口。
> 
> 关键代码
> - Handler 里面聚合它自己，在 HandlerRequest 里判断是否合适，如果没达到条件则向下传递，向谁传递之前 set 进去。
> 
> 应用实例
> - 红楼梦中的"击鼓传花"。
> - JS 中的事件冒泡。 
> - JAVA WEB 中 Apache Tomcat 对 Encoding 的处理，Struts2 的拦截器，jsp servlet 的 Filter。
> 
> 优点
> - 降低耦合度。它将请求的发送者和接收者解耦。
> - 简化了对象。使得对象不需要知道链的结构。 
> - 增强给对象指派职责的灵活性。通过改变链内的成员或者调动它们的次序，允许动态地新增或者删除责任。 
> - 增加新的请求处理类很方便。
> 
> 缺点
> - 不能保证请求一定被接收。 
> - 系统性能将受到一定影响，而且在进行代码调试时不太方便，可能会造成循环调用。 
> - 可能不容易观察运行时的特征，有碍于除错。
> 
> 使用场景 
> - 有多个对象可以处理同一个请求，具体哪个对象处理该请求由运行时刻自动确定。 
> - 在不明确指定接收者的情况下，向多个对象中的一个提交一个请求。 
> - 可动态指定一组对象处理请求。
> 
> 注意事项
> - 在 JAVA WEB 中遇到很多应用。

# 碰撞链
实现坦克大战中，坦克与坦克、坦克与子弹、坦克与墙、子弹与墙的碰撞链<br>
坦克、子弹、墙都是 GameObject 的子类

- 接口
```java
/**
 * 负责游戏物体的碰撞
 */
public interface Collider {

    boolean collide(GameObject o1,GameObject o2);

}
```

- 子弹与坦克的碰撞
```java
public class BulletTankCollider implements Collider {

    @Override
    public boolean collide(GameObject o1, GameObject o2) {
        if (o1 instanceof Tank && o2 instanceof Bullet) {
            交换 o1 和 o2
        }
        if (o1 instanceof Bullet && o2 instanceof Tank) {
            	处理子弹与坦克的碰撞
            }
            return true;
        }
        return false;
    }

}
```
其他三个类雷同，分别是 TanksCollider , BulletWallCollier , TankWallCollider 

- 责任链
	```java
	public class ColliderChain implements Collider {

		private List<Collider> colliders = new LinkedList<>();

		public ColliderChain() {
			add(new BulletTankCollider());
			add(new TanksCollider());
			add(new BulletWallCollier());
			add(new TankWallCollider());
		}

		public void add(Collider collider) {
			colliders.add(collider);
		}

		@Override
		public boolean collide(GameObject o1, GameObject o2) {
			for (int i = 0; i < colliders.size(); i++) {
				if (colliders.get(i).collide(o1, o2)) {
					return false;
				}
			}
			return true;
		}

	}
	```
	- ColliderChain 继承 Collider 的好处
		- 可以把责任链合并
	- 为什么 collider 方法的返回值是 boolean
		- 实现责任链的断开。
		- BulletTankCollider 是碰撞链的第一环，当它检测到传入责任链的参数是坦克和子弹时，那么就没有必要把参数传入下一环。

- 如何调用
	```java
	//碰撞检测的责任链
    ColliderChain colliderChain = new ColliderChain();
	
	//碰撞检测，使用责任链
	for (int i = 0; i < objects.size(); i++) {
            for (int j = i + 1; j < objects.size(); j++) {
                GameObject o1 = objects.get(i);
                GameObject o2 = objects.get(j);
                colliderChain.collide(o1, o2);
            }
        }
	```
	objects 是存储坦克大战中所有游戏对象的数组。
	只需要用双重循环遍历游戏对象所有可能的两两组合，并把组合传入责任链中，即可实现碰撞检测。
	这样写可以实现程序的解耦，无论责任链怎么变，外部的调用都不需要修改。
```java

```
```java

```
```java

```
# 源码链接
该文章源码链接[url](url)