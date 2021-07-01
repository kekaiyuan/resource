---
layout: post
title: Oracle 之——表设计
categories: Oracle
description: Oracle 之——表设计
keywords: Oracle
---

Oracle 之——表设计

## 视图
- 视图(view)，也称虚表。<br>
	视图生成的表不占用物理空间，本身的定义语句存储在数据字典里，占用少量空间。<br>
	视图只有逻辑定义。<br>
	每次使用的时候， 只是重新执行SQL。
- 视图是从一个或多个实际表中获得的，这些表的数据存放在数据库中。<br>
	那些用于产生视图的表叫做该视图的基表。<br>
	一个视图也可以从另一个视图中产生。
- 视图的定义存在数据库中，与此定义相关的数据并没有再存一份于数据库中。<br>
	通过视图看到的数据存放在基表中。
- 视图看上去非常象数据库的物理表，对它的操作同任何其它的表一样。<br>
	当通过视图修改数据时，实际上是在改变基表中的数据；<br>
	相反地，基表数据的改变也会自动反映在由基表产生的视图中。<br>
	由于逻辑上的原因，有些Oracle视图可以修改对应的基表，有些则不能（仅仅能查询）。

### 物化视图
物化视图是 Oracle 的特性，即生成一张物理表而非虚拟表。

虚拟表的数据都来自于基表，每次执行语句都是从基表中取数据，虚拟表和基表是同步的。

但是物化视图有自己的物理表，所以需要**刷新**操作来更新物化视图，保持和基表数据一致。<br>
其更新方式有两种：
- On Demand<br>
	仅在需要“被刷新”时才刷新。
- On Commit<br>
	一旦基表有 Commit 操作，立刻刷新物化视图。

## 创建视图
在 CREATE VIEW 语句后加入子查询
```
CREATE [OR REPLACE] VIEW view [(alias[, alias]...)] AS
	subquery
[WITH READ ONLY];
```
例
```
create or replace view v$_emp_dept as 
  select emp.deptno, ename, dname
    from emp
    join dept
      on emp.deptno = dept.deptno
with read only
```



## 源码链接
该文章源码链接 [](url)