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
		所以 turncate 直接**永久**删除，**无法回滚**，但是**速度快**。


## 源码链接
该文章源码链接 [Github](url)