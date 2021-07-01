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

## 源码链接
该文章源码链接 [Github](url)