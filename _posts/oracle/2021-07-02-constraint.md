---
layout: post
title: Oracle 之——约束
categories: Oracle
description: Oracle 之——约束
keywords: Oracle
---

Oracle 之——约束

## 约束 constraint
当我们创建表的时候，同时可以指定所插入数据的一些规则，比如说<br>
某个字段不能为空值，某个字段的值（比如年龄）不能小于零……<br>
这些规则称为约束。

约束是在表上强制执行的数据校验规则。

在添加约束之前，插入语句只需要**合法**：即满足表结构的**数据类型**。<br>
在添加约束之后，插入语句还需要**合理**：即满足建表人所定义的**规范**。

----------

约束用于确保数据库数据满足特定的商业逻辑或者企业规则。<br>
如果定义了约束，并且数据不符合约束，那么DML操作（INSERT、UPDATE、DELETE）将不能成功执行。

**Oracle 支持下面五类完整性约束**:
1. NOT NULL <br>
	非空约束<br>
	插入数据的时候某些列不允许为空。
2. UNIQUE Key <br>
	唯一键约束<br>
	可以限定某一个列的值是唯一的，唯一键的列一般被用作**索引**列。
3. PRIMARY KEY <br>
	主键<br>
	非空且唯一，任何一张表一般情况下最好有主键，用来唯一的**标识**一行记录。
4. FOREIGN KEY <br>
	外键<br>
	当多个表之间有**关联关系**（一个表的某个列的值**依赖**于另一张表的某个值）的时候，需要使用外键。
5. CHECK <br>
	自定义检查约束<br>
	可以根据用户自己的**需求**去限定某些列的值。


----------


**注意**：
- Oracle使用SYS_Cn格式命名约束，也可以由用户命名
	- 命名规则<br>
		约束_表名_字段 <br>
		可以保证唯一性。 如果太长，可用缩写。
- 同一字段可以有多个约束，但是约束之间不要冲突
- 创建约束的时机
	1. **在建表的同时创建**。 <br>
		**建议使用**。 <br>
		如果包含外键约束的话，最好先把**外键**关联表的数据**优先**插入。
	2. 建表后创建。
- 约束从作用上分类，可以分成两大类：
	- 列级约束<br>
		在定义列的同时声明约束。<br>
		`列名 [CONSTRAINT 约束名] 约束类型`<br>
		只能约束其所在的某一列。<br>
		可以定义任何约束。
	- 表级约束<br>
		先定义列，然后再定义约束。<br>
		```
		列1 , 列2 , ...
		[CONSTRAINT 约束名] 约束类型(列1 , 列2 , ...)
		```
		可以约束表中的任意一列或多列。<br>
		可以定义除了 **not null** 以外的任何约束。
	- 有些时候，列级约束无法实现某种约束的定义，比如联合主键的定义，就要用到**表级约束**：<br>
		```
		create table test(
			id1 number,
			id2 number, 
			primary key(id1, id2)
		);
		```

### 非空约束 not null
确保字段值不允许为空

只能在字段级定义
```
CREATE TABLE employees(
	name VARCHAR2(25) not null,
	hire_date DATE CONSTRAINT emp_hire_date_nn not null
)
```

### 唯一性约束 unique
- 唯一性约束条件确保所在的字段或者字段组合不出现重复值
- 唯一性约束条件的字段允许出现空值
- Oracle将为唯一性约束条件创建对应的唯一性索引
- 定义方式
	```
	CREATE TABLE employees(
		name VARCHAR2(25) not null unique,
		email VARCHAR2(25),
		CONSTRAINT emp_email_uk unique(email)
	);
	```
	
### 主键约束 primary key
主键约束是数据库中最重要的一种约束。<br>
在关系中，主键值不可为空，也不允许出现重复，即关系要满足实体完整性规则。
- 主键从功能上看相当于非空且唯一
- 一个表中只允许一个主键
- 主键是表中能够唯一确定一个行数据的字段
- 主键字段可以是单字段或者是多字段的组合
- Oracle 为主键创建对应的唯一性**索引**
- 定义方式
	```
	create table t3(
		id number(4) primary key
	)
	或
	create table t3(
		id number(4), 
		CONSTRAINT t3_pk primary key(id)
	)
	```

### 外键约束 foreign key
现在有学生表和成绩表。

学生表中有字段学生ID，成绩表中也有字段学生ID。

在给成绩表的学生ID设置外键约束后（参照为学生表的学生ID）。

成绩表想要插入成绩，其学生ID必须是学生表中存在的学生ID。<br>
也就是说，并不能插入**不存在**的学生的成绩。

然后当学生毕业了，或者退学了，当把该学生从学生表中删除时（删除了学生ID）。<br>
那么成绩表中对应的成绩也会被一起**清空**。

这就是外键约束。

成绩表依赖于学生表，成绩表是依赖表，学生表是主表。

成绩表中的学生ID是外键，学生表中的学生ID是主键。

- 外键是表中的一个列，其值必须在另一表的主键或者唯一键中列出
- 作为主键的表称为“主表”，作为外键的关系称为“依赖表”
- 外键参照的是主表的**主键**或者**唯一键**
- 声明外键时，外键的列和依赖的列**数据类型**必须一致。
- 对于主表的删除和修改主键值的操作，会对依赖关系产生影响。
	以删除为例：当要删除主表的某个记录为例，即删除一个主键值，有 3 种做法
	1. **restrict** 方式
		- 只有当依赖表中没有一个外键值与要删除的主表中主键值相对应时，才可执行删除操作。
		- 需要**手动**地修改依赖表，否则无法删除主表中的数据。
	2. **cascade** 方式
		- 将依赖表中所有外键值与主表中要删除的主键值相对应的记录一起**删除**。
		- 自动执行。
	3. **set null** 方式
		- 将依赖表中所有与主表中被删除的主键值相对应的外键值设为**空值**。
		- 自动执行。

外键的声明<br>
`foreign key (列名) references 主表(主键或唯一键) [on delete [cascade|set null]]` <br>
如果省略 on 短语，缺省将使用 **restrict** 方式

### check约束
- check 约束用于对一个属性的值加以限制
- 在 check 中定义检查的条件表达式，数据需要符合设置的条件
	```
	create table emp3( 
		age number(2) check(age > 0 and age < 100),
		salary number(7,2),
		constraint salary_check check(salary > 0)
	)
	```
- 在这种约束下，插入记录或修改记录时，系统要测试新的记录的值是否满足条件

## 约束的添加和撤销
可增加或删除约束，但不能直接修改。

增加约束<br>
`alter table 表名 add constraint 约束名 约束类型(列名);`

删除约束<br>
`alter table 表名 drop constraint 约束名 [cascade];`

## 查看约束
- 查看某表的约束
	```
	select constraint_name, constraint_type
	  from user_constraints
	 where table_name = upper(表名);
	```
- 查看某表的约束及列名	
	```
	select constraint_name, column_name
	  from user_cons_columns
	 where table_name = upper(表名);
	```
- 查看某数据库所有表的约束
	```
	select constraint_name, constraint_type
	  from user_constraints
	 where owner = 用户名;
	```


## 关系模型的三类完整性规则
为了维护数据库中的数据与现实世界的一致性，关系数据库的数据与更新操作必须遵循下列三类完整性规则：
1. 实体完整性规则<br>
	这条规则要求关系中在组成主键的属性上不能有空值。
2. 参照完整性规则<br>
	这条规则要求“不引用不存在的实体”。<br>
	例如：deptno是dept表的主键，而相应的属性也在表emp中出现，此时deptno是表emp的外键。<br>
	在emp表中，deptno的取值要么为空，要么等于dept中的某个主键值。
3. 用户定义的完整性规则<br>
	用户定义的完整性规则反应了某一具体的应用涉及的数据必须满足的语义要求。


## 源码链接
该文章源码链接 [Github](url)