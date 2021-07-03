---
layout: post
title: Java 之——数据库类型对照表
categories: [cate1, cate2]
description: some word here
keywords: keyword1, keyword2
---


| SQL数据类型 | JDBC类型代码 | 标准的Java类型 | Oracle扩展的Java类型<br>省略 **oracle.sql.<br>** 前缀 |
| ---- | ---- | ---- | ---- |
|   | 1.0标准的JDBC类型: <br>省略 java.sql.Types.<br> 前缀|   |   |
| CHAR | CHAR | java.lang.<br>String | CHAR |
| VARCHAR2 | VARCHAR | java.lang.<br>String | CHAR |
| LONG | LONGVARCHAR | java.lang.<br>String | CHAR |
| NUMBER | NUMERIC | java.math.<br>BigDecimal | NUMBER |
| NUMBER | DECIMAL | java.math.<br>BigDecimal | NUMBER |
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
|   | 2.0标准的JDBC类型:<br>省略 java.sql.Types.<br> 前缀 |   |   |
| BLOB | BLOB | java.sql.Blob | BLOB |
| CLOB | CLOB | java.sql.Clob | CLOB |
| 用户定义的对象 | STRUCT | java.sql.Struct | STRUCT |
| 用户定义的参考 | REF | java.sql.Ref | REF |
| 用户定义的集合 | ARRAY | java.sql.Array | ARRAY |
|   | Oracle扩展:<br>省略 oracle.jdbc.OracleTypes.<br> 前缀 |   |   |
| BFILE | BFILE | N/A | BFILE |
| ROWID | ROWID | N/A | ROWID |
| REF CURSOR | CURSOR | java.sql.ResultSet | oracle.jdbc.OracleResultSet |
| TIMESTAMP | TIMESTAMP | java.sql.Timestamp | TIMESTAMP |
| TIMESTAMP WITH TIME ZONE | TIMESTAMPTZ | java.sql.Timestamp | TIMESTAMPTZ |
| TIMESTAMP WITH LOCAL TIME ZONE | TIMESTAMPLTZ | java.sql.Timestamp | TIMESTAMPLTZ |

| SQL数据类型 | JDBC类型代码 | 标准的Java类型 | Oracle扩展的Java类型 |
| ---- | ---- | ---- | ---- |
|   | 1.0标准的JDBC类型: |   |   |
| CHAR | java.sql.Types.<br>CHAR | java.lang.<br>String | oracle.sql.<br>CHAR |
| VARCHAR2 | java.sql.Types.<br>VARCHAR | java.lang.<br>String | oracle.sql.<br>CHAR |
| LONG | java.sql.Types.<br>LONGVARCHAR | java.lang.<br>String | oracle.sql.<br>CHAR |
| NUMBER | java.sql.Types.<br>NUMERIC | java.math.<br>BigDecimal | oracle.sql.<br>NUMBER |
| NUMBER | java.sql.Types.<br>DECIMAL | java.math.<br>BigDecimal | oracle.sql.<br>NUMBER |
| NUMBER | java.sql.Types.<br>BIT | boolean | oracle.sql.<br>NUMBER |
| NUMBER | java.sql.Types.<br>TINYINT | byte | oracle.sql.<br>NUMBER |
| NUMBER | java.sql.Types.<br>SMALLINT | short | oracle.sql.<br>NUMBER |
| NUMBER | java.sql.Types.<br>INTEGER | int | oracle.sql.<br>NUMBER |
| NUMBER | java.sql.Types.<br>BIGINT | long | oracle.sql.<br>NUMBER |
| NUMBER | java.sql.Types.<br>REAL | float | oracle.sql.<br>NUMBER |
| NUMBER | java.sql.Types.<br>FLOAT | double | oracle.sql.<br>NUMBER |
| NUMBER | java.sql.Types.<br>DOUBLE | double | oracle.sql.<br>NUMBER |
| RAW | java.sql.Types.<br>BINARY | byte[] | oracle.sql.<br>RAW |
| RAW | java.sql.Types.<br>VARBINARY | byte[] | oracle.sql.<br>RAW |
| LONGRAW | java.sql.Types.<br>LONGVARBINARY | byte[] | oracle.sql.<br>RAW |
| DATE | java.sql.Types.<br>DATE | java.sql.Date | oracle.sql.<br>DATE |
| DATE | java.sql.Types.<br>TIME | java.sql.Time | oracle.sql.<br>DATE |
| TIMESTAMP | java.sql.Types.<br>TIMESTAMP | javal.sql.Timestamp | oracle.sql.<br>TIMESTAMP |
|   | 2.0标准的JDBC类型: |   |   |
| BLOB | java.sql.Types.<br>BLOB | java.sql.Blob | oracle.sql.<br>BLOB |
| CLOB | java.sql.Types.<br>CLOB | java.sql.Clob | oracle.sql.<br>CLOB |
| 用户定义的对象 | java.sql.Types.<br>STRUCT | java.sql.Struct | oracle.sql.<br>STRUCT |
| 用户定义的参考 | java.sql.Types.<br>REF | java.sql.Ref | oracle.sql.<br>REF |
| 用户定义的集合 | java.sql.Types.<br>ARRAY | java.sql.Array | oracle.sql.<br>ARRAY |
|   | Oracle扩展: |   |   |
| BFILE | oracle.jdbc.OracleTypes.<br>BFILE | N/A | oracle.sql.<br>BFILE |
| ROWID | oracle.jdbc.OracleTypes.<br>ROWID | N/A | oracle.sql.<br>ROWID |
| REF CURSOR | oracle.jdbc.OracleTypes.<br>CURSOR | java.sql.ResultSet | oracle.jdbc.OracleResultSet |
| TIMESTAMP | oracle.jdbc.OracleTypes.<br>TIMESTAMP | java.sql.Timestamp | oracle.sql.<br>TIMESTAMP |
| TIMESTAMP WITH TIME ZONE | oracle.jdbc.OracleTypes.<br>TIMESTAMPTZ | java.sql.Timestamp | oracle.sql.<br>TIMESTAMPTZ |
| TIMESTAMP WITH LOCAL TIME ZONE | oracle.jdbc.OracleTypes.<br>TIMESTAMPLTZ | java.sql.Timestamp | oracle.sql.<br>TIMESTAMPLTZ |


## 源码链接
该文章源码链接 [Github](url)