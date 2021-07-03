---
layout: post
title: Java 之——数据库类型对照表
categories: [cate1, cate2]
description: some word here
keywords: keyword1, keyword2
---

| SQL数据类型 | JDBC类型代码 | 标准的Java类型 | Oracle扩展的Java类型 |
| ---- | ---- | ---- | ---- |
|   | 1.0标准的JDBC类型: |   |   |
| CHAR | java.sql.Types.<br>CHAR | java.lang.String | oracle.sql.CHAR |
| VARCHAR2 | java.sql.Types.<br>VARCHAR | java.lang.String | oracle.sql.CHAR |
| LONG | java.sql.Types.<br>LONGVARCHAR | java.lang.String | oracle.sql.CHAR |
| NUMBER | java.sql.Types.<br>NUMERIC | java.math.BigDecimal | oracle.sql.NUMBER |
| NUMBER | java.sql.Types.<br>DECIMAL | java.math.BigDecimal | oracle.sql.NUMBER |
| NUMBER | java.sql.Types.<br>BIT | boolean | oracle.sql.NUMBER |
| NUMBER | java.sql.Types.<br>TINYINT | byte | oracle.sql.NUMBER |
| NUMBER | java.sql.Types.<br>SMALLINT | short | oracle.sql.NUMBER |
| NUMBER | java.sql.Types.<br>INTEGER | int | oracle.sql.NUMBER |
| NUMBER | java.sql.Types.<br>BIGINT | long | oracle.sql.NUMBER |
| NUMBER | java.sql.Types.<br>REAL | float | oracle.sql.NUMBER |
| NUMBER | java.sql.Types.<br>FLOAT | double | oracle.sql.NUMBER |
| NUMBER | java.sql.Types.<br>DOUBLE | double | oracle.sql.NUMBER |
| RAW | java.sql.Types.<br>BINARY | byte[] | oracle.sql.RAW |
| RAW | java.sql.Types.<br>VARBINARY | byte[] | oracle.sql.RAW |
| LONGRAW | java.sql.Types.<br>LONGVARBINARY | byte[] | oracle.sql.RAW |
| DATE | java.sql.Types.<br>DATE | java.sql.Date | oracle.sql.DATE |
| DATE | java.sql.Types.<br>TIME | java.sql.Time | oracle.sql.DATE |
| TIMESTAMP | java.sql.Types.<br>TIMESTAMP | javal.sql.Timestamp | oracle.sql.TIMESTAMP |
|   | 2.0标准的JDBC类型: |   |   |
| BLOB | java.sql.Types.<br>BLOB | java.sql.Blob | oracle.sql.BLOB |
| CLOB | java.sql.Types.<br>CLOB | java.sql.Clob | oracle.sql.CLOB |
| 用户定义的对象 | java.sql.Types.<br>STRUCT | java.sql.Struct | oracle.sql.STRUCT |
| 用户定义的参考 | java.sql.Types.<br>REF | java.sql.Ref | oracle.sql.REF |
| 用户定义的集合 | java.sql.Types.<br>ARRAY | java.sql.Array | oracle.sql.ARRAY |
|   | Oracle扩展: |   |   |
| BFILE | oracle.jdbc.OracleTypes.<br>BFILE | N/A | oracle.sql.BFILE |
| ROWID | oracle.jdbc.OracleTypes.<br>ROWID | N/A | oracle.sql.ROWID |
| REF CURSOR | oracle.jdbc.OracleTypes.<br>CURSOR | java.sql.ResultSet | oracle.jdbc.<br>OracleResultSet |
| TIMESTAMP | oracle.jdbc.OracleTypes.<br>TIMESTAMP | java.sql.Timestamp | oracle.sql.<br>TIMESTAMP |
| TIMESTAMP WITH TIME ZONE | oracle.jdbc.OracleTypes.<br>TIMESTAMPTZ | java.sql.Timestamp | oracle.sql.<br>TIMESTAMPTZ |
| TIMESTAMP WITH LOCAL TIME ZONE | oracle.jdbc.OracleTypes.<br>TIMESTAMPLTZ | java.sql.Timestamp | oracle.sql.<br>TIMESTAMPLTZ |


## 源码链接
该文章源码链接 [Github](url)