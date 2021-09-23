---
layout: post
title: Mybatis 01
categories: [cate1, cate2]
description: some word here
keywords: keyword1, keyword2
---

# 01Mybatis的介绍和基本使用

### 0、数据库操作框架的历程

##### 		(1)	JDBC

​		JDBC(Java Data Base Connection,java数据库连接)是一种用于执行SQL语句的Java API,可以为多种关系数据库提供统一访问,它由一组用Java语言编写的类和接口组成.JDBC提供了一种基准,据此可以构建更高级的工具和接口,使数据库开发人员能够编写数据库应用程序

- 优点：运行期：快捷、高效
- 缺点：编辑期：代码量大、繁琐异常处理、不支持数据库跨平台

![img](images\jdbc.jpg)

##### 		(2) DBUtils

​		DBUtils是Java编程中的数据库操作实用工具，小巧简单实用。

​		DBUtils封装了对JDBC的操作，简化了JDBC操作，可以少写代码。

​		DBUtils三个核心功能介绍

​			1、QueryRunner中提供对sql语句操作的API

​			2、ResultSetHandler接口，用于定义select操作后，怎样封装结果集

​			3、DBUtils类，它就是一个工具类，定义了关闭资源与事务处理的方法

##### 		(3)Hibernate

​		Hibernate 是由 Gavin King 于 2001 年创建的开放源代码的对象关系框架。它强大且高效的构建具有关系对象持久性和查询服务的 Java 应用程序。

​		Hibernate 将 Java 类映射到数据库表中，从 Java 数据类型中映射到 SQL 数据类型中，并把开发人员从 95% 的公共数据持续性编程工作中解放出来。

​		Hibernate 是传统 Java 对象和数据库服务器之间的桥梁，用来处理基于 O/R 映射机制和模式的那些对象。

![image](images\hibernate.jpg)

​		**Hibernate 优势**

- Hibernate 使用 XML 文件来处理映射 Java 类别到数据库表格中，并且不用编写任何代码。

- 为在数据库中直接储存和检索 Java 对象提供简单的 APIs。

- 如果在数据库中或任何其它表格中出现变化，那么仅需要改变 XML 文件属性。

- 抽象不熟悉的 SQL 类型，并为我们提供工作中所熟悉的 Java 对象。

- Hibernate 不需要应用程序服务器来操作。

- 操控你数据库中对象复杂的关联。

- 最小化与访问数据库的智能提取策略。

- 提供简单的数据询问。

  **Hibernate劣势**

- hibernate的完全封装导致无法使用数据的一些功能。

- Hibernate的缓存问题。

- Hibernate对于代码的耦合度太高。

- Hibernate寻找bug困难。

- Hibernate批量数据操作需要大量的内存空间而且执行过程中需要的对象太多

  ##### (4) JDBCTemplate

​       JdbcTemplate针对数据查询提供了多个重载的模板方法,你可以根据需要选用不同的模板方法.如果你的查询很简单，仅仅是传入相应SQL或者相关参数，然后取得一个单一的结果，那么你可以选择如下一组便利的模板方法。

​		优点：运行期：高效、内嵌Spring框架中、支持基于AOP的声明式事务
​		缺点：必须于Spring框架结合在一起使用、不支持数据库跨平台、默认没有缓存

### 1、什么是Mybatis？

​		MyBatis 是一款优秀的持久层框架，它支持自定义 SQL、存储过程以及高级映射。MyBatis 免除了几乎所有的 JDBC 代码以及设置参数和获取结果集的工作。MyBatis 可以通过简单的 XML 或注解来配置和映射原始类型、接口和 Java POJO（Plain Old Java Objects，普通老式 Java 对象）为数据库中的记录。

​		**优点：**

​		1、与JDBC相比，减少了50%的代码量

​		2、 最简单的持久化框架，简单易学

​		3、SQL代码从程序代码中彻底分离出来，可以重用

​		4、提供XML标签，支持编写动态SQL

​		5、提供映射标签，支持对象与数据库的ORM字段关系映射

​		**缺点：**

​		1、SQL语句编写工作量大，熟练度要高

​		2、数据库移植性比较差，如果需要切换数据库的话，SQL语句会有很大的差异

# Mybatis 使用教程
## 一个简单的 Mybatis 项目
### 相关依赖
pom.xml
```xml
<!-- https://mvnrepository.com/artifact/org.mybatis/mybatis -->
<dependency>
	<groupId>org.mybatis</groupId>
	<artifactId>mybatis</artifactId>
	<version>3.5.4</version>
</dependency>
<!-- https://mvnrepository.com/artifact/mysql/mysql-connector-java -->
<dependency>
	<groupId>mysql</groupId>
	<artifactId>mysql-connector-java</artifactId>
	<version>8.0.19</version>
</dependency>
<!-- https://mvnrepository.com/artifact/log4j/log4j -->
<dependency>
	<groupId>log4j</groupId>
	<artifactId>log4j</artifactId>
	<version>1.2.17</version>
</dependency>
```

### 创建数据库

### 创建 entity
User.java
```java
public class User {
    private Integer id;
    private String userName;
    private String password;

    //省略无参构造器，getter/setter 方法，toString() 方法

}
```

### 创建 dao
UserDao.Interface
```java
public interface UserDao {

    public User selectById(Integer id);

}
```
注意，此时的 dao 是个接口，具体的实现要在 xml 配置文件中实现。

### 编写配置文件

#### db.properties
将 JDBC 所需要的 `driver` `url` `username` `password` 写入配置文件方便管理
```profile
driver=com.mysql.cj.jdbc.Driver
url=jdbc:mysql://localhost:3306/test?serverTimezone=UTC
username=root
password=123456
```
在连接某些版本的 mysql 数据库时，会出现时区问题，所以要添加
```
?serverTimezone=UTC
```

#### UserDao.xml
```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<!--namespace:编写接口的全类名，就是告诉要实现该配置文件是哪个接口的具体实现-->
<mapper namespace="com.kky.dao.UserDao">
    <!--
    select:表示这个操作是一个查询操作
    id表示的是要匹配的方法的名称
    resultType:表示返回值的类型，查询操作必须要包含返回值的类型
    #{属性名}：表示要传递的参数的名称
    -->
    <select id="selectById" resultType="com.kky.bean.User">
        select * from user where id = #{id}
    </select>
</mapper>
```

### mybatis-config.xml
```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<!--在填写标签的时候一定要注意相关配置的顺序-->
<configuration>
    <!--引入配置文件-->
    <properties resource="db.properties"></properties>
    <environments default="development">
        <environment id="development">
            <transactionManager type="JDBC"/>
            <!--配置数据库连接-->
            <dataSource type="POOLED">
                <property name="driver" value="${driver}"/>
                <property name="url" value="${url}"/>
                <property name="username" value="${username}"/>
                <property name="password" value="${password}"/>
            </dataSource>
        </environment>
    </environments>
    <!--引入每一个接口对应点xml文件-->
    <mappers>
        <mapper resource="UserDao.xml"/>
    </mappers>
</configuration>
```

### 测试
```java
// 根据全局配置文件创建出SqlSessionFactory
// SqlSessionFactory:负责创建SqlSession对象的工厂
// SqlSession:表示跟数据库建议的一次会话
String resource = "mybatis-config.xml";
InputStream inputStream = null;
try {
	inputStream = Resources.getResourceAsStream(resource);
} catch (IOException e) {
	e.printStackTrace();
}
SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(inputStream);

//获取与数据库相关的会话
SqlSession sqlSession = sqlSessionFactory.openSession();
//获取对应的映射接口对象
UserDao mapper = sqlSession.getMapper(UserDao.class);
//执行具体的sql语句
User user = mapper.selectById(1);
System.out.println(user);
//关闭会话
sqlSession.close();
```

### 结果
```java
User{id=1, userName='aaa', password='bbb'}
```

### 3、增删改查的基本操作

EmpDao.java

```java
package com.mashibing.dao;

import com.mashibing.bean.Emp;

public interface EmpDao {

    public Emp findEmpByEmpno(Integer empno);

    public int updateEmp(Emp emp);

    public int deleteEmp(Integer empno);

    public int insertEmp(Emp emp);

}
```

EmpDao.xml

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<!--namespace:编写接口的全类名，就是告诉要实现该配置文件是哪个接口的具体实现-->
<mapper namespace="com.mashibing.dao.EmpDao">
    <!--
    select:表示这个操作是一个查询操作
    id表示的是要匹配的方法的名称
    resultType:表示返回值的类型，查询操作必须要包含返回值的类型
    #{属性名}：表示要传递的参数的名称
    -->
    <select id="findEmpByEmpno" resultType="com.mashibing.bean.Emp">
        select * from emp where empno = #{empno}
    </select>
    <!--增删改查操作不需要返回值，增删改返回的是影响的行数，mybatis会自动做判断-->
    <insert id="insertEmp">
        insert into emp(empno,ename) values(#{empno},#{ename})
    </insert>
    <update id="updateEmp">
        update emp set ename=#{ename} where empno = #{empno}
    </update>
    <delete id="deleteEmp">
        delete from emp where empno = #{empno}
    </delete>
</mapper>
```

MyTest.java

```java
package com.mashibing.test;import com.mashibing.bean.Emp;import com.mashibing.dao.EmpDao;import org.apache.ibatis.io.Resources;import org.apache.ibatis.session.SqlSession;import org.apache.ibatis.session.SqlSessionFactory;import org.apache.ibatis.session.SqlSessionFactoryBuilder;import org.junit.Before;import org.junit.Test;import java.io.IOException;import java.io.InputStream;public class MyTest {    SqlSessionFactory sqlSessionFactory = null;    @Before    public void init(){        // 根据全局配置文件创建出SqlSessionFactory        // SqlSessionFactory:负责创建SqlSession对象的工厂        // SqlSession:表示跟数据库建议的一次会话        String resource = "mybatis-config.xml";        InputStream inputStream = null;        try {            inputStream = Resources.getResourceAsStream(resource);            sqlSessionFactory= new SqlSessionFactoryBuilder().build(inputStream);        } catch (IOException e) {            e.printStackTrace();        }    }    @Test    public void test01() {        // 获取数据库的会话        SqlSession sqlSession = sqlSessionFactory.openSession();        Emp empByEmpno = null;        try {            // 获取要调用的接口类            EmpDao mapper = sqlSession.getMapper(EmpDao.class);            // 调用方法开始执行            empByEmpno = mapper.findEmpByEmpno(7369);        } catch (Exception e) {            e.printStackTrace();        } finally {            sqlSession.close();        }        System.out.println(empByEmpno);    }    @Test    public void test02(){        SqlSession sqlSession = sqlSessionFactory.openSession();        EmpDao mapper = sqlSession.getMapper(EmpDao.class);        int zhangsan = mapper.insertEmp(new Emp(1111, "zhangsan"));        System.out.println(zhangsan);        sqlSession.commit();        sqlSession.close();    }    @Test    public void test03(){        SqlSession sqlSession = sqlSessionFactory.openSession();        EmpDao mapper = sqlSession.getMapper(EmpDao.class);        int zhangsan = mapper.updateEmp(new Emp(1111, "lisi"));        System.out.println(zhangsan);        sqlSession.commit();        sqlSession.close();    }    @Test    public void test04(){        SqlSession sqlSession = sqlSessionFactory.openSession();        EmpDao mapper = sqlSession.getMapper(EmpDao.class);        int zhangsan = mapper.deleteEmp(1111);        System.out.println(zhangsan);        sqlSession.commit();        sqlSession.close();    }}
```

### 4、配置文件详解

​		在mybatis的项目中，我们发现了有一个mybatis-config.xml的配置文件，这个配置文件是mybatis的全局配置文件，用来进行相关的全局配置，在任何操作下都生效的配置。下面我们要针对其中的属性做详细的解释，方便大家在后续使用的时候更加熟练。

#### 环境配置
https://1258712167.vod2.myqcloud.com/fb8e6c92vodtranscq1258712167/5ce8d8d45285890802873855525/drm/voddrm.token.dWluPTE0NDExNTIxNzY1MDUyNzUxODtza2V5PTtwc2tleT07cGxza2V5PTtleHQ9YTY0NTI1NTNkOGQ1YTBlYzViZGQxNjRiNDdhYWMwMTFmMDI1OTBjMmY1ODhlYTVmYzEzZDM1OWFiODk3NDBkOTI4MjM1YmRjYjgxZmNkNmM2ZDg3NGM4MGZhNDljNzczMDNhYmVjYzFhODIzOWNiNjQxNDNhZTdhNWM2ZjRkYjIxMWYyZmFiOTE4ZjEzNzMzO3VpZF9hcHBpZD0xNDAwMDAwMDA4O3VpZF90eXBlPTI7dWlkX29yaWdpbl91aWRfdHlwZT0yO3VpZF9vcmlnaW5fYXV0aF90eXBlPTI7Y2lkPTQyMzkwMjt0ZXJtX2lkPTEwMDUwNTk2Nzt2b2RfdHlwZT0w.master_playlist.m3u8?t=6155a579&exper=0&us=5581734388525065350&sign=28bfd4c63256468fb4f0987c57fa1bde

https://1258712167.vod2.myqcloud.com/fb8e6c92vodtranscq1258712167/5ce8d8d45285890802873855525/drm/voddrm.token.dWluPTE0NDExNTIxNzY1MDUyNzUxODtza2V5PTtwc2tleT07cGxza2V5PTtleHQ9YTY0NTI1NTNkOGQ1YTBlYzViZGQxNjRiNDdhYWMwMTFmMDI1OTBjMmY1ODhlYTVmYzEzZDM1OWFiODk3NDBkOTI4MjM1YmRjYjgxZmNkNmM2ZDg3NGM4MGZhNDljNzczMDNhYmVjYzFhODIzOWNiNjQxNDNhZTdhNWM2ZjRkYjIxMWYyZmFiOTE4ZjEzNzMzO3VpZF9hcHBpZD0xNDAwMDAwMDA4O3VpZF90eXBlPTI7dWlkX29yaWdpbl91aWRfdHlwZT0yO3VpZF9vcmlnaW5fYXV0aF90eXBlPTI7Y2lkPTQyMzkwMjt0ZXJtX2lkPTEwMDUwNTk2Nzt2b2RfdHlwZT0w.master_playlist.m3u8?t=6155a579&exper=0&us=5581734388525065350&sign=28bfd4c63256468fb4f0987c57fa1bde

https://1258712167.vod2.myqcloud.com/fb8e6c92vodtranscq1258712167/5ce8d8d45285890802873855525/drm/voddrm.token.dWluPTE0NDExNTIxNzY1MDUyNzUxODtza2V5PTtwc2tleT07cGxza2V5PTtleHQ9YTY0NTI1NTNkOGQ1YTBlYzViZGQxNjRiNDdhYWMwMTFmMDI1OTBjMmY1ODhlYTVmYzEzZDM1OWFiODk3NDBkOTI4MjM1YmRjYjgxZmNkNmM2ZDg3NGM4MGZhNDljNzczMDNhYmVjYzFhODIzOWNiNjQxNDNhZTdhNWM2ZjRkYjIxMWYyZmFiOTE4ZjEzNzMzO3VpZF9hcHBpZD0xNDAwMDAwMDA4O3VpZF90eXBlPTI7dWlkX29yaWdpbl91aWRfdHlwZT0yO3VpZF9vcmlnaW5fYXV0aF90eXBlPTI7Y2lkPTQyMzkwMjt0ZXJtX2lkPTEwMDUwNTk2Nzt2b2RfdHlwZT0w.master_playlist.m3u8?t=6155a579&exper=0&us=5581734388525065350&sign=28bfd4c63256468fb4f0987c57fa1bde

https://1258712167.vod2.myqcloud.com/fb8e6c92vodtranscq1258712167/5ce8d8d45285890802873855525/drm/voddrm.token.dWluPTE0NDExNTIxNzY1MDUyNzUxODtza2V5PTtwc2tleT07cGxza2V5PTtleHQ9YTY0NTI1NTNkOGQ1YTBlYzViZGQxNjRiNDdhYWMwMTFmMDI1OTBjMmY1ODhlYTVmYzEzZDM1OWFiODk3NDBkOTI4MjM1YmRjYjgxZmNkNmM2ZDg3NGM4MGZhNDljNzczMDNhYmVjYzFhODIzOWNiNjQxNDNhZTdhNWM2ZjRkYjIxMWYyZmFiOTE4ZjEzNzMzO3VpZF9hcHBpZD0xNDAwMDAwMDA4O3VpZF90eXBlPTI7dWlkX29yaWdpbl91aWRfdHlwZT0yO3VpZF9vcmlnaW5fYXV0aF90eXBlPTI7Y2lkPTQyMzkwMjt0ZXJtX2lkPTEwMDUwNTk2Nzt2b2RfdHlwZT0w.master_playlist.m3u8?t=6155a579&exper=0&us=5581734388525065350&sign=28bfd4c63256468fb4f0987c57fa1bde

https://1258712167.vod2.myqcloud.com/fb8e6c92vodtranscq1258712167/5ce8d8d45285890802873855525/drm/voddrm.token.dWluPTE0NDExNTIxNzY1MDUyNzUxODtza2V5PTtwc2tleT07cGxza2V5PTtleHQ9YTY0NTI1NTNkOGQ1YTBlYzViZGQxNjRiNDdhYWMwMTFmMDI1OTBjMmY1ODhlYTVmYzEzZDM1OWFiODk3NDBkOTI4MjM1YmRjYjgxZmNkNmM2ZDg3NGM4MGZhNDljNzczMDNhYmVjYzFhODIzOWNiNjQxNDNhZTdhNWM2ZjRkYjIxMWYyZmFiOTE4ZjEzNzMzO3VpZF9hcHBpZD0xNDAwMDAwMDA4O3VpZF90eXBlPTI7dWlkX29yaWdpbl91aWRfdHlwZT0yO3VpZF9vcmlnaW5fYXV0aF90eXBlPTI7Y2lkPTQyMzkwMjt0ZXJtX2lkPTEwMDUwNTk2Nzt2b2RfdHlwZT0w.master_playlist.m3u8?t=6155a579&exper=0&us=5581734388525065350&sign=28bfd4c63256468fb4f0987c57fa1bde

https://1258712167.vod2.myqcloud.com/fb8e6c92vodtranscq1258712167/5ce8d8d45285890802873855525/drm/voddrm.token.dWluPTE0NDExNTIxNzY1MDUyNzUxODtza2V5PTtwc2tleT07cGxza2V5PTtleHQ9YTY0NTI1NTNkOGQ1YTBlYzViZGQxNjRiNDdhYWMwMTFmMDI1OTBjMmY1ODhlYTVmYzEzZDM1OWFiODk3NDBkOTI4MjM1YmRjYjgxZmNkNmM2ZDg3NGM4MGZhNDljNzczMDNhYmVjYzFhODIzOWNiNjQxNDNhZTdhNWM2ZjRkYjIxMWYyZmFiOTE4ZjEzNzMzO3VpZF9hcHBpZD0xNDAwMDAwMDA4O3VpZF90eXBlPTI7dWlkX29yaWdpbl91aWRfdHlwZT0yO3VpZF9vcmlnaW5fYXV0aF90eXBlPTI7Y2lkPTQyMzkwMjt0ZXJtX2lkPTEwMDUwNTk2Nzt2b2RfdHlwZT0w.master_playlist.m3u8?t=6155a579&exper=0&us=5581734388525065350&sign=28bfd4c63256468fb4f0987c57fa1bde

https://1258712167.vod2.myqcloud.com/fb8e6c92vodtranscq1258712167/5ce8d8d45285890802873855525/drm/voddrm.token.dWluPTE0NDExNTIxNzY1MDUyNzUxODtza2V5PTtwc2tleT07cGxza2V5PTtleHQ9YTY0NTI1NTNkOGQ1YTBlYzViZGQxNjRiNDdhYWMwMTFmMDI1OTBjMmY1ODhlYTVmYzEzZDM1OWFiODk3NDBkOTI4MjM1YmRjYjgxZmNkNmM2ZDg3NGM4MGZhNDljNzczMDNhYmVjYzFhODIzOWNiNjQxNDNhZTdhNWM2ZjRkYjIxMWYyZmFiOTE4ZjEzNzMzO3VpZF9hcHBpZD0xNDAwMDAwMDA4O3VpZF90eXBlPTI7dWlkX29yaWdpbl91aWRfdHlwZT0yO3VpZF9vcmlnaW5fYXV0aF90eXBlPTI7Y2lkPTQyMzkwMjt0ZXJtX2lkPTEwMDUwNTk2Nzt2b2RfdHlwZT0w.master_playlist.m3u8?t=6155a579&exper=0&us=5581734388525065350&sign=28bfd4c63256468fb4f0987c57fa1bde

https://1258712167.vod2.myqcloud.com/fb8e6c92vodtranscq1258712167/5ce8d8d45285890802873855525/drm/voddrm.token.dWluPTE0NDExNTIxNzY1MDUyNzUxODtza2V5PTtwc2tleT07cGxza2V5PTtleHQ9YTY0NTI1NTNkOGQ1YTBlYzViZGQxNjRiNDdhYWMwMTFmMDI1OTBjMmY1ODhlYTVmYzEzZDM1OWFiODk3NDBkOTI4MjM1YmRjYjgxZmNkNmM2ZDg3NGM4MGZhNDljNzczMDNhYmVjYzFhODIzOWNiNjQxNDNhZTdhNWM2ZjRkYjIxMWYyZmFiOTE4ZjEzNzMzO3VpZF9hcHBpZD0xNDAwMDAwMDA4O3VpZF90eXBlPTI7dWlkX29yaWdpbl91aWRfdHlwZT0yO3VpZF9vcmlnaW5fYXV0aF90eXBlPTI7Y2lkPTQyMzkwMjt0ZXJtX2lkPTEwMDUwNTk2Nzt2b2RfdHlwZT0w.master_playlist.m3u8?t=6155a579&exper=0&us=5581734388525065350&sign=28bfd4c63256468fb4f0987c57fa1bde
```xml

```


EmpDaoAnnotation.java

```java
package com.mashibing.dao;import com.mashibing.bean.Emp;import org.apache.ibatis.annotations.Delete;import org.apache.ibatis.annotations.Insert;import org.apache.ibatis.annotations.Select;import org.apache.ibatis.annotations.Update;public interface EmpDaoAnnotation {    @Select("select * from emp where empno = #{empno}")    public Emp findEmpByEmpno(Integer empno);    @Update("update emp set ename=#{ename} where empno = #{empno}")    public int updateEmp(Emp emp);    @Delete("delete from emp where empno = #{empno}")    public int deleteEmp(Integer empno);    @Insert("insert into emp(empno,ename) values(#{empno},#{ename})")    public int insertEmp(Emp emp);}
```

(url)[https://mybatis.org/mybatis-3/zh/configuration.html#settings]




# 源码链接
该文章源码链接 [Github](url)