---
layout: wiki
title: IntelliJ IDEA
categories: Tools
description: 最好用的 Java IDE
keywords: IDEA, Java
---

我的IntelliJ IDEA版本：**Community 2020.03**

# 使用jdk中的方法时报错
修改`File`/`Project Structure`/`Project Settings`/`Modules`的`Language level`与使用的jdk版本一致

# 自动声明变量
=号左边的东西不用写，将=号右边的东西写完后+`.var`+回车
```java
//=号左边东西不写，右边写好
new File("test.txt")

//+.var
new File("test.txt").var

//按下回车
File file = new File("abc.txt");
```

# 自动生成serialVersionUID
勾选`File`/`Settings`/`Editor`/`Inspections`/`Java`/`Serializable issues`/`Serializable class without 'serialVersionUID'`，右边窗口里`Severity`可选择警告的等级和作用范围，上边窗口里`Profile`可选择该设置是对当前项目生效还是对IDEA生效

这样当实体类继承了Serializable却没有定义serialVersionUID时类名会高亮警告，把鼠标移动到类名上即可自动生成serialVersionUID
