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
使用 < , > , <= , >= , != 等关系符号连接时称为非等值连接

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
显示左表的全部数据

`select * from A,B where A.S = B.S(+);`

| A.X | A.Y | A.S | B.S | B.Z |
| --- | --- | --- | --- | --- |
| 1   | 2   | 2   |      |      |
| 1   | 2   | 3   | 3   | 3   |
| 1   | 2   | 4   | 4   | 4   |

#### 右外连接
显示右表的全部数据

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
		- 表中有同名列
- 可以连接两张以上的表，但是要注意设置连接条件。<br>
    多表连接几乎不使用笛卡尔积连接，因为生成的数据太多。

## 1999语法
在 92语法 中，多张表的连接条件放在 where子句 中，同时 where子句 需要对表进行条件过滤。<br>
where子句 需要完成两个功能，这就使得 92语法 中的 where子句 格外的臃肿。<br>
为了解决这个问题，提出了 1999语法 。

## 源码链接
该文章源码链接 [](url)