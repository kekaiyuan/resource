---
layout: post
title: Java 之——数据库类型对照表
categories: [cate1, cate2]
description: some word here
keywords: keyword1, keyword2
---


| SQL数据类型 | JDBC类型代码 | 标准的Java类型 | Oracle扩展的Java类型<br>省略 oracle.sql. 前缀 |
| ---- | ---- | ---- | ---- |
|   | 1.0标准的JDBC类型: |   |   |
| CHAR | java.sql.Types.CHAR | java.lang.String | CHAR |
| VARCHAR2 | java.sql.Types.VARCHAR | java.lang.String | CHAR |
| LONG | java.sql.Types.LONGVARCHAR | java.lang.String | CHAR |
| NUMBER | java.sql.Types.NUMERIC | java.math.BigDecimal | NUMBER |
| NUMBER | java.sql.Types.DECIMAL | java.math.BigDecimal | NUMBER |
| NUMBER | java.sql.Types.BIT | boolean | NUMBER |
| NUMBER | java.sql.Types.TINYINT | byte | NUMBER |
| NUMBER | java.sql.Types.SMALLINT | short | NUMBER |
| NUMBER | java.sql.Types.INTEGER | int | NUMBER |
| NUMBER | java.sql.Types.BIGINT | long | NUMBER |
| NUMBER | java.sql.Types.REAL | float | NUMBER |
| NUMBER | java.sql.Types.FLOAT | double | NUMBER |
| NUMBER | java.sql.Types.DOUBLE | double | NUMBER |
| RAW | java.sql.Types.BINARY | byte[] | RAW |
| RAW | java.sql.Types.VARBINARY | byte[] | RAW |
| LONGRAW | java.sql.Types.LONGVARBINARY | byte[] | RAW |
| DATE | java.sql.Types.DATE | java.sql.Date | DATE |
| DATE | java.sql.Types.TIME | java.sql.Time | DATE |
| TIMESTAMP | java.sql.Types.TIMESTAMP | java.sql.Timestamp | TIMESTAMP |
|   | 2.0标准的JDBC类型: |   |   |
| BLOB | java.sql.Types.BLOB | java.sql.Blob | BLOB |
| CLOB | java.sql.Types.CLOB | java.sql.Clob | CLOB |
| 用户定义的对象 | java.sql.Types.STRUCT | java.sql.Struct | STRUCT |
| 用户定义的参考 | java.sql.Types.REF | java.sql.Ref | REF |
| 用户定义的集合 | java.sql.Types.ARRAY | java.sql.Array | ARRAY |
|   | Oracle扩展: |   |   |
| BFILE | oracle.jdbc.OracleTypes.BFILE | N/A | BFILE |
| ROWID | oracle.jdbc.OracleTypes.ROWID | N/A | ROWID |
| REF CURSOR | oracle.jdbc.OracleTypes.CURSOR | java.sql.ResultSet | oracle.jdbc.OracleResultSet |
| TIMESTAMP | oracle.jdbc.OracleTypes.TIMESTAMP | java.sql.Timestamp | TIMESTAMP |
| TIMESTAMP WITH TIME ZONE | oracle.jdbc.OracleTypes.TIMESTAMPTZ | java.sql.Timestamp | TIMESTAMPTZ |
| TIMESTAMP WITH LOCAL TIME ZONE | oracle.jdbc.OracleTypes.TIMESTAMPLTZ | java.sql.Timestamp | TIMESTAMPLTZ |



## 源码链接
该文章源码链接 [Github](url)