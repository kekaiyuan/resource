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

## 源码链接
该文章源码链接 [](url)