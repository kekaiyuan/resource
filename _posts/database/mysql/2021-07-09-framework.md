---
layout: post
title: Mysql 之——底层架构
categories: [cate1, cate2]
description: some word here
keywords: keyword1, keyword2
---


## 序言

![enter description here](/images/posts/database/mysql/framework/framework.png)

## 连接器
连接器负责跟客户端建立连接，获取权限、维持和管理连接
- 用户名密码验证
- 查询权限信息，分配对应的权限<br>
	mysql 的权限控制比 oracle 的要宽松。
- 可以使用 `show processlist` 查看现在的连接
- 如果太长时间没有动静，就会自动断开，通过 `wait_timeout` 控制，默认 8 小时

连接可以分为两类：
- 长连接<br>
	推荐使用，但是要周期性的断开长连接<br>
	连接池用的就是长连接。
- 短连接

## 查询缓存
当执行查询语句的时候，会先去查询缓存中查看结果。<br>
之前执行过的 sql 语句及其结果可能以 key-value 的形式存储在缓存中。<br>
如果能找到则直接返回，如果找不到，就继续执行后续的阶段。

但是，**不推荐**使用查询缓存：
1. 查询缓存的失效比较频繁，只要表更新，缓存就会**清空**
2. 缓存对应新更新的数据**命中率**比较低

Mysql 8.0 之后没有缓存了，即使是 8.0 以前的版本，缓存也是默认关闭的，需要手动开启。

## 分析器
词法分析：Mysql 需要把输入的字符串进行识别每个部分代表什么意思
- 把字符串 T 识别成 表名 T
- 把字符串 ID 识别成 列 ID

语法分析：<br>
根据语法规则判断这个 sql 语句是否满足 mysql 的语法。<br>
如果不符合就会报错 
```
You have an error in your SQL syntax;
```

## 优化器
在具体执行SQL语句之前，要先经过优化器的处理
- 当表中有多个索引的时候，决定用哪个索引
- 当sql语句需要做多表关联的时候，决定表的连接顺序
- 等等

不同的执行方式对SQL语句的执行效率影响很大
- RBO<br>
	基于规则的优化
- CBO<br>
	基于成本的优化<br>
	现在**更多**的使用基于成本的优化。
