---
layout: post
title: Oracle SQL ——数据更新
categories: Oracle
description: Oracle SQL ——数据更新
keywords: Oracle，SQL
---

在实际项目中，使用最多的是读取操作，但是插入数据和删除数据同等重要，而修改操作相对较少。

## 插入
### 元组值的插入
最基本的插入方式
- `insert into tablename values (val1, val2, ...., valn);`<br>
	如果表名之后没有列，那么只能将所有的列都插入<br>
	例：`insert into emp values(2222,'haha','clerk',7902,to_date('2019-11-2','YYYY-MM-dd'),1000,500,10);`
- `insert into tablename (col1, col2, ...colm) values (val1, val2, .. ., valm);`<br>
	可以指定向哪些列中插入数据<br>
	例：`insert into emp(empno,ename) values(3333,'wangwu');`
	- 省略的列必须满足以下条件之一：
		- 该列定义为允许Null值。
		- 在表定义中给出默认值，这表示如果不给出值，将使用默认值。
- 一次插入操作只插入一行

### 查询结果的插入
可以用 insert 语句把一个 select 语句的查询结果插入到一个基本表中，语法如下：
```
Insert into tablename(column,..) 
select * from tablename2
```

表 A

| X | Y | S |
| -- | -- | -- |
| 1 | 1 | 1 |
| 2 | 2 | 2 |
| 3 | 3 | 3 |

表 B
| S | Z |
| -- | -- |
| 4 | 4 |
| 5 | 5 |

执行语句<br>
`insert into B select X,Y from A ;`

结果

表 B

| S | Z |
| -- | -- |
| 1 | 1 |
| 2 | 2 |
| 3 | 3 |
| 4 | 4 |
| 5 | 5 |

#### 复制表
通过查询结果的插入我们可以复制表
- 复制表结构和数据<br>
	`create table table2 as select * from table1;`
- 只复制表结构，不复制数据<br>
	`create table table2 as select * from table1 where 1=2;`<br>
	因为 `select * from table1 where 1=2` 是一张空表
- 无论是哪种复制，都无法复制表的约束（外键、主键等）<br>
	当表没有主键时，oracle 将默认使用 **rownum** 作为主键
	
### Q&A
Q：如果有一个集合的数据，把集合中的所有数据都挨条插入的话，效率如何？

A：一般在实际的操作中，很少一条条插入，更多的是**批量插入**

## 删除
SQL的删除操作是指从基本表中删除元组。

语法如下：
```
DELETE [FROM] tablename
[WHERE condition];
```
-  其语义是从基本表中删除满足条件表达式的元组
-  `Delete from tablename;` 表示从表中删除一切元组
	-  `truncate table` 语句也是删除表中一切元组。<br>
		delete 有事务，truncate 没有事务。<br>
		所以 turncate 直接**永久**删除，**无法回滚**，但是**速度快**。<br>
		效率高，但是容易发生**误操作**，**不建议**使用。

## 修改
```
update tablename 
set col = val1 , col2 = val2 , ...
where condition;
```
可以更新或者修改满足条件的一个列或者**多个**列

## 事务
事务（Transaction）是一个操作序列。<br>
这些操作要么都做，要么都不做，是一个不可分割的工作单位，是数据库环境中的逻辑工作单位。
-  事务是为了保证数据库的完整性
-  事务不能嵌套
-  在 oracle 中，没有事务开始的语句。<br>
	一个 Transaction **起始于**一条 DML ( Insert , Update , Delete ) 语句<br>
	**结束于**以下的几种情况：
	-  用户显式执行 Commit 语句提交操作或Rollback语句回退。
	-  当执行 DDL ( Create , Alter , Drop ) 语句事务自动提交。
	-  用户正常断开连接时， Transaction 自动提交。
	-  系统崩溃或断电时事务自动回退。

增删改是数据库的常用操作，在进行操作的时候都需要 **事务** 的保证。<br>
也就是说每次在 PL/SQL 中执行 sql 语句之后都需要完成 commit 的操作。

事务的关键性在于保持**数据一致性**。
如果同一份数据，在同一个时刻只能有一个人访问，就不会出现数据错乱的问题，但是在现在的项目中，更多的是并发访问
并发访问很容易造成数据的不安全，即不一致。
如果要保证数据的安全，最主要的方式就是**加锁**的方式，例如MVCC
    
如果不保证事务的话，会造成**脏读**，**不可重复读**，**幻读**。	
	
事务的延申：
- 最基本的数据库事务
- 声明式事务
- 分布式事务



## 源码链接
该文章源码链接 [Github](url)