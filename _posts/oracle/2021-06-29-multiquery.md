---
layout: post
title: 多表查询
categories: Oracle
description: 多表查询
keywords: Oracle
---

## 多表查询

有时我们需要读取多个表中的数据

现有表 employee ，其中有雇员的ID，名字，部门ID<br>
![enter description here](/images/posts/oracle/multiquery/employee.png)

有表 department ，其中有部门的ID，名字，地点
![enter description here](/images/posts/oracle/multiquery/department.png)

现需查询雇员的ID以及雇员的部门名字。<br>
这分别在两张表上。<br>
此时需要将两张表合并。<br>
![enter description here](/images/posts/oracle/multiquery/employee-department.png)

## 1992语法

92语法共有五种连接方式：
- 等值连接
- 非等值连接
- 外连接
- 自连接
- 笛卡尔积

下文将通过表A和表B说明五种连接的区别。

表 A

| X   | Y   | S   |  
| --- | --- | --- |
| 1   | 2   | 2   |
| 1   | 2   | 3   |
| 1   | 2   | 4   |

表 B

| S   | Z   | 
| --- | --- | 
| 3   | 3   | 
| 4   | 4   | 
| 5   | 5   | 


### 等值连接
根据 **where子句** 中的等式连接两张表

例：`select * from A,B where A.S = B.S;`

| A.X | A.Y | A.S | B.S | B.Z |
| --- | --- | --- | --- | --- |
| 1   | 2   | 3   | 3   | 3   |
| 1   | 2   | 4   | 4   | 4   |

- 语法规则：
	```
	SELECT table1.column, table2.column
	FROM table1, table2
	WHERE table1.column1 = table2.column2;
	```
	将 table1 中的 column1 与 table2 中的 column2 相等的记录连接到一起。
- 一般而言 column1 为 table1 的主键，column2 为 table2 的主键。

### 非等值连接
使用 < , > , <= , >= , != , between and 等关系符号连接时称为非等值连接

例：`select * from A,B where A.S > B.S;`

| A.X | A.Y | A.S | B.S | B.Z |
| --- | --- | --- | --- | --- |
| 1   | 2   | 4   | 3   | 3   |

表A只有记录 (1,2,4) 中的S大于表B中记录 (3,3) 中的S

### 外连接

在前两种连接中，如果表中存在无法连接的记录，该记录会被舍弃。

为了在操作时能保持这些将被舍弃的元组，提出了外连接的概念，使用外连接可以看到丌满足连接条件的记录。
- 外连接运算符是 (+)
- 有左外连接和右外连接

#### 左外连接
显示左表的全部数据<br>
在**右表**添加 (+)

`select * from A,B where A.S = B.S(+);`

| A.X | A.Y | A.S | B.S | B.Z |
| --- | --- | --- | --- | --- |
| 1   | 2   | 2   |      |      |
| 1   | 2   | 3   | 3   | 3   |
| 1   | 2   | 4   | 4   | 4   |

#### 右外连接
显示右表的全部数据<br>
在**左表**添加 (+)

`select * from A,B where A.S(+) = B.S;`

| A.X | A.Y | A.S | B.S | B.Z |
| --- | --- | --- | --- | --- |
| 1   | 2   | 3   | 3   | 3   |
| 1   | 2   | 4   | 4   | 4   |
|      |      |      | 5   | 5   |


### 自连接
和自己进行连接

`select * from A A1,A A2 where A1.Y = A2.S;`

| A1.X | A1.Y | A1.S | A2.X | A2.Y | A2.S |
| --- | --- | --- | --- | --- | --- |
| 1   | 2   | 2   | 1   | 2   | 2   |
| 1   | 2   | 3   | 1   | 2   | 2   |
| 1   | 2   | 4   | 1   | 2   | 2   |

### 笛卡尔积
当连接时不指定连接方式时，将默认进行笛卡尔积计算。

将左表的每一条记录和右表的每一条记录进行连接。

检索出的行的数目将是左表的行数**乘以**右表的行数<br>
检索出的列的数目将是左表的列数**加上**右表的列数

`select * from A,B;`

| A.X | A.Y | A.S | B.S | B.Z |
| --- | --- | --- | --- | --- |
| 1   | 2   | 2  | 3   | 3   |
| 1   | 2   | 2  | 4   | 4   |
| 1   | 2   | 2  | 5   | 5   |
| 1   | 2   | 3  | 3   | 3   |
| 1   | 2   | 3  | 4   | 4   |
| 1   | 2   | 3  | 5   | 5   |
| 1   | 2   | 4  | 3   | 3   |
| 1   | 2   | 4  | 4   | 4   |
| 1   | 2   | 4  | 5   | 5   |

### 注意
- 连接时可以给表起别名，方便操作。<br>
	有时必须使用别名
	- 自连接
	- 不同的表中有同名列
- 可以连接两张以上的表，但是要注意设置连接条件。<br>
    多表连接几乎不使用笛卡尔积连接，因为生成的数据太多。<br>
    当有 n 张表进行连接时，至少需要 n-1 个条件，否则一定会发生笛卡尔积。

## 1999语法
在 92语法 中，多张表的连接条件放在 where子句 中，同时 where子句 需要对表进行条件过滤。

where子句 需要完成两个功能，这就使得 92语法 中的 where子句 格外的臃肿。

1999语法 解决了这个问题，并且提出了一些新的连接方式。

### 语法规则
```
CROSS JOIN
NATURAL JOIN
USING子句
ON子句
RIGHT OUTER JOIN
FULL OUTER JOIN
LEFT OUTER JOIN
Inner outer join
```

99语法 中共有**八种**连接方式，其中有部分连接原理等同于 92语法 。<br>
只是为了方便书写，更改了书写方式。

### 笛卡尔积
等同于 92语法 的笛卡尔积<br>
`select * from table1 cross join table2;`

### 自然连接
`select * from table1 natural join table2;`
- NATURAL JOIN子句基亍两个表中列名完全相同的列产生连接
	- 两个表有相同名字的列
	- 数据类型相同
	- 从两个表中选出连接列的值相等的所有行
- 类似于 92语法 中的 **等值连接**<br>
	但是 自然连接 会去除**重复**的列，而 等值连接 不会。
- 当两张表中没有列名相同的列时，做**笛卡尔积**。


### using 子句


### on 子句<br>
    添加连接条件，包括等值和非等值<br>
    `select * from table1 join table2 on table1.column1 = table2.column2;`<br>
	`select * from table1 join table2 on table1.column1 > table2.column2;`
	
	
### 外连接
#### 左外连接
等同于 92语法 的左外连接<br>
`select * from table1 left outer join table2 on 连接条件;`
	
#### 右外连接<br>
等同于 92语法 的右外连接<br>
`select * from table1 right outer join table2 on 连接条件;`
	
#### 全外连接<br>
99语法 的新特性<br>
能显示左表和右表的全部数据，兼顾了左外连接和右外连接。<br>
`select * from table1 full outer join table2 on 连接条件;`

例：`select * from A full outer join B on A.S = B.S;`

| A.X | A.Y | A.S | B.S | B.Z |
| --- | --- | --- | --- | --- |
| 1   | 2   | 3   | 3   | 3   |
| 1   | 2   | 4   | 4   | 4   |
|      |      |      | 5   | 5   |
| 1   | 2   | 2   |      |      |
	
### 内连接
`select * from table1 inner join table2 on 连接条件;`<br>
等同于`select * from table1 join table2 on 连接条件;`

### using子句
using子句 和 on子句 一样，都可以表示连接条件<br>
`select * from table1 join table2 using(column1);`<br>
等同于`select * from table1 join table2 on table1.column1 = table2.column1;`<br>

注意
- using子句 的连接条件是同名列，而且不能使用表名进行修饰。<br>
	~~`select * from table1 join table2 using(table1.column1);`<br>~~<br>
	该语句是错误的。
- using子句 类似于自然连接，使用同名列进行连接，同样地会去除重复的同名列。<br>
	而 on子句 不会去除重复的同名列。
- 当两张表存在 n 个同名列时，自然连接会将所有的同名列进行连接。<br>
	而 using子句 则根据指定的同名列进行连接。
		
		




## 源码链接
该文章源码链接 [](url)