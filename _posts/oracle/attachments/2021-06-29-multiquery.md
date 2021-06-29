---
layout: post
title: 多表查询
categories: Oracle
description: 多表查询
keywords: Oracle
---

多表查询

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
- 语法规则：
```
SELECT table1.column, table2.column
FROM table1, table2
WHERE table1.column1 = table2.column2; 
```
- 在 WHERE 子句中写入连接条件
- 当多个表中有重名列时，必须在列的名字前加上表名作为前缀
- 连接的类型：
	- 等值连接<br>
		Equijoin
	- 非等值连接<br>
		Non-equijoin
	- 外连接 <br>
		Outer join
	- 自连接 <br>
		Self join
		
		
数据来自亍多张表 ,92表连接
- 注意: 明确引用同名的列，必须使用表名 戒者别名区分
- 一、迪卡尔积
- select 字段列表 from 表1,表2,表3....
- 二、等值连接: 取关系列相同的记录
- select 字段列表 from 表1,表2,表3....
- where 表1.列=表2.列 and 表1.列=表3.列 - 三、非等值连接：取关系列丌同的记录 != > < >= <= between and 
- select 字段列表 from 表1,表2,表3....
- where 表1.列!=表2.列 and 表1.列!=表3.列 - 四、自连接:(特殊的等值连接) 列来自亍同一张表,丌同角度看待表
- select 字段列表 from 表1 e,表1 m
- where e.列1=m.列2 
- 五、外连接: 在等值基础上，确保 一张表(主表)的记录都存在 从表满足则匹配，丌满足补充null
- 1、左外: 主表在左边
- 2、右外: 主表在右边

### 等值连接
两个表中包含相同的列名

- 语法规则：
	```
	SELECT table1.column, table2.column
	FROMtable1, table2
	WHERE table1.column1 = table2.column2;
	```
- 笛卡尔积：表\*表 
- 主外键
	- 在外键表中的映射字段称为 外键 Foreign key
	- 在主键表中的唯一字段称为 主键 Primary key

### 非等值连接
两个表中没有相同的列名，但是某一个列在另一张表的列的范围之中

< , > , <= , >= , != 连接时称为非等值连接
例：`select * from emp,salgrade where sal between losal and hisal;`

### 外连接
在前两种连接中，如果表A中的某条数据与表B中的某条数据有某种对应关系。
在连接后这两条数据会根据连接方式进行合并。

在等值连接中，如果表中存在无法连接的记录，该记录会被舍弃。

如：

表 A
| X   | Y   | S   |  
| --- | --- | --- |
| 1   | 2   | 2   |
| 1   | 2   | 3   |
| 1   | 2   | 4   |

表 B
| S   | D   | 
| --- | --- | 
| 3   | 3   | 
| 4   | 4   | 
| 5   | 5   | 

`select * from A,B where A.S = B.S;`

| A.X | A.Y | A.S | B.S | B.D |
| --- | --- | --- | --- | --- |
| 1   | 2   | 3   | 3   | 3   |
| 1   | 2   | 4   | 4   | 4   |

可以看到表A中的(1,2,2)记录和表B中的(5,5)记录没有被显示。<br>
因为这两条记录与另一张表的记录不存在连接关系。

为了在操作时能保持这些将被舍弃的元组，提出了外连接的概念，使用外连接可以看到丌满足连接条件的记录。
- 外连接运算符是 (+)
- 有左外连接和右外连接

#### 左外连接
显示左表的全部

`select * from A,B where A.S = B.S(+);`

| A.X | A.Y | A.S | B.S | B.D |
| --- | --- | --- | --- | --- |
| 1   | 2   | 2   |      |      |
| 1   | 2   | 3   | 3   | 3   |
| 1   | 2   | 4   | 4   | 4   |

#### 右外连接
显示右表的全部数据

`select * from A,B where A.S(+) = B.S;`

| A.X | A.Y | A.S | B.S | B.D |
| --- | --- | --- | --- | --- |
| 1   | 2   | 3   | 3   | 3   |
| 1   | 2   | 4   | 4   | 4   |
|      |      |      | 5   | 5   |


### 笛卡尔积
当连接时不指定连接方式时，将默认进行笛卡尔积计算。

将左表的每一条记录和右表的每一条记录进行连接。

生成的表的总条数为 m\*n

`select * from A,B;`
| A.X | A.Y | A.S | B.S | B.D |
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


## 源码链接
该文章源码链接 [](url)