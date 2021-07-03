---
layout: post
title: Java 之——数据库类型对照表
categories: [cate1, cate2]
description: some word here
keywords: keyword1, keyword2
---


| SQL数据类型 | JDBC类型代码 | 标准的Java类型 | Oracle扩展的Java类型<br>省略 **oracle.sql.** 前缀 |
| ---- | ---- | ---- | ---- |
|   | 1.0标准的JDBC类型: <br>省略 java.sql.Types. 前缀|   |   |
| CHAR | CHAR | java.lang.String | CHAR |
| VARCHAR2 | VARCHAR | java.lang.String | CHAR |
| LONG | LONGVARCHAR | java.lang.String | CHAR |
| NUMBER | NUMERIC | java.math.BigDecimal | NUMBER |
| NUMBER | DECIMAL | java.math.BigDecimal | NUMBER |
| NUMBER | BIT | boolean | NUMBER |
| NUMBER | TINYINT | byte | NUMBER |
| NUMBER | SMALLINT | short | NUMBER |
| NUMBER | INTEGER | int | NUMBER |
| NUMBER | BIGINT | long | NUMBER |
| NUMBER | REAL | float | NUMBER |
| NUMBER | FLOAT | double | NUMBER |
| NUMBER | DOUBLE | double | NUMBER |
| RAW | BINARY | byte[] | RAW |
| RAW | VARBINARY | byte[] | RAW |
| LONGRAW | LONGVARBINARY | byte[] | RAW |
| DATE | DATE | java.sql.Date | DATE |
| DATE | TIME | java.sql.Time | DATE |
| TIMESTAMP | TIMESTAMP | java.sql.Timestamp | TIMESTAMP |
|   | 2.0标准的JDBC类型:<br>省略 java.sql.Types. 前缀 |   |   |
| BLOB | BLOB | java.sql.Blob | BLOB |
| CLOB | CLOB | java.sql.Clob | CLOB |
| 用户定义的对象 | STRUCT | java.sql.Struct | STRUCT |
| 用户定义的参考 | REF | java.sql.Ref | REF |
| 用户定义的集合 | ARRAY | java.sql.Array | ARRAY |
|   | Oracle扩展:<br>省略 oracle.jdbc.OracleTypes. 前缀 |   |   |
| BFILE | BFILE | N/A | BFILE |
| ROWID | ROWID | N/A | ROWID |
| REF CURSOR | CURSOR | java.sql.ResultSet | oracle.jdbc.OracleResultSet |
| TIMESTAMP | TIMESTAMP | java.sql.Timestamp | TIMESTAMP |
| TIMESTAMP WITH TIME ZONE | TIMESTAMPTZ | java.sql.Timestamp | TIMESTAMPTZ |
| TIMESTAMP WITH LOCAL TIME ZONE | TIMESTAMPLTZ | java.sql.Timestamp | TIMESTAMPLTZ |



## 源码链接
该文章源码链接 [Github](url)