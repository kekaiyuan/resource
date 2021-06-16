---
layout: wiki
title: IntelliJ IDEA
categories: Tools
description: 最好用的 Java IDE
keywords: IDEA, Java
---

我的IntelliJ IDEA版本：**Community 2020.03**

# 快捷键
`Ctrl`+`P`：构造方法提示

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

# 关联中文API文档
- 下载中文API的chm文件<br>
	- 链接：[https://pan.baidu.com/s/1a3vvEx5089kbVx48ufTidQ](https://pan.baidu.com/s/1a3vvEx5089kbVx48ufTidQ) <br>
提取码：3lkw

- 反编译chm文件为html文件<br>
	- 在chm文件所在目录打开cmd窗口，输入<br>
`hh -decompile doc 文件名全称（包括后缀）`<br>
例如：`hh -decompile doc jdk api 1.8_google.CHM`

- 打开`File`/`Project Structure`/`SDKSs`/`Documentation Paths`<br>
点击下方`+`号添加刚才反编译出的html的文件夹

- 之后把鼠标移到代码上即可查看相关的中文说明

# 通过Maven导入spring的jar包
项目名右键`Add Framework Support`/勾选`Maven`/`OK`后会生成`pom.xml`文件
在`<project></project>`标签内加入`<dependencies></dependencies>`标签，然后在`<dependencies></dependencies>`标签内加入我们需要的依赖项的代码
[https://mvnrepository.com/](https://mvnrepository.com/)搜索spring context
![enter description here](/images/wiki/intellij-idea/mavenindex.png)
![enter description here](/images/wiki/intellij-idea/mavenspring01.png)
把代码粘贴进`<dependencies></dependencies>`标签中
![enter description here](/images/wiki/intellij-idea/mavenspring02.png)
![enter description here](/images/wiki/intellij-idea/mavenspring03.png)
![enter description here](/images/wiki/intellij-idea/mavenspring04.png)

然后等待自动下载就好了

# 和Github的通信太慢
当cmd里和Github的通信没问题，而Idea却太慢时。

找到Idea的安装目录下的bin文件夹。

找到runnerw.exe和runnerw(64).exe

修改这两个文件的文件名或者直接删除他们