---
layout: wiki
title: IntelliJ IDEA
categories: Tools
description: 最好用的 Java IDE
keywords: IDEA, Java
---

## 版本：Community 2020.03

### 快捷键
`Ctrl`+`P`：构造方法提示

### 使用 Jdk 中的方法时报错
修改`File`/`Project Structure`/`Project Settings`/`Modules`的`Language level`与使用的jdk版本一致

### 自动声明变量
=号左边的东西不用写，将=号右边的东西写完后+`.var`+回车
```java
//=号左边东西不写，右边写好
new File("test.txt")

//+.var
new File("test.txt").var

//按下回车
File file = new File("abc.txt");
```

### 自动生成 SerialVersionUID
勾选`File`/`Settings`/`Editor`/`Inspections`/`Java`/`Serializable issues`/`Serializable class without 'serialVersionUID'`，右边窗口里`Severity`可选择警告的等级和作用范围，上边窗口里`Profile`可选择该设置是对当前项目生效还是对IDEA生效

这样当实体类继承了Serializable却没有定义serialVersionUID时类名会高亮警告，把鼠标移动到类名上即可自动生成serialVersionUID

### 关联中文 API 文档
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

### 通过 Maven 导入 Jar 包
举例如何导入 Spring 的 Jar 包，其他能在 Maven 上搜到的 Jar 包都是同样的流程。

项目名右键`Add Framework Support`/勾选`Maven`/`OK`后会生成`pom.xml`文件

在`<project></project>`标签内加入`<dependencies></dependencies>`标签，然后在`<dependencies></dependencies>`标签内加入我们需要的依赖项的代码

maven 网址[https://mvnrepository.com/](https://mvnrepository.com/)

搜索spring context<br>
![enter description here](/images/wiki/intellij-idea/mavenindex.png)<br><br>

---------- 

![enter description here](/images/wiki/intellij-idea/mavenspring01.png)<br><br>
选择需要的版本<br>
![enter description here](/images/wiki/intellij-idea/mavenspring02.png)<br><br>
复制代码<br>
![enter description here](/images/wiki/intellij-idea/mavenspring03.png)<br><br>
把代码粘贴进`<dependencies></dependencies>`标签中<br>
![enter description here](/images/wiki/intellij-idea/mavenspring04.png)<br>

打开 Maven 窗口，右键 `Reload project` 项目，等待下载完成

### 和 Github 的通信太慢
当cmd里和Github的通信没问题，而Idea却太慢时。

找到Idea的安装目录下的bin文件夹。

找到`runnerw.exe`和`runnerw(64).exe`

修改这两个文件的文件名或者直接删除他们

## 版本：Community 2021.06
### 并发启动代码
在网络开发中，经常需要启动多个客户端测试代码，如何将同一个 class 同时运行多个

在左侧 `Project` 窗口找到要并发启动的文件，右键 `Modify Run Configuration`

在 `Build and run` 窗口，点击 `Modify options` ， 勾选 `Allow multify instances`

![enter description here](/images/wiki/intellij-idea/multify-instances.png)

完成

### Maven 找不到包
打开 `Setting` ， 输入 `maven` ，打开 `local repository`，把相关的文件删除，重新 `reload`

### Gradle 项目控制台乱码
`help` / `edit custom vm options` 

加上 `-Dfile.encoding=utf-8` 

重启 IDEA

### 修改 File Header
打开 `File` / `Settings` / `Editor` / `File and Code Templates/Includes`

添加

```
/**
 * @author 柯凯元
 * @date ${YEAR}/${MONTH}/${DAY} ${TIME}
 */
 ```
 
 ![enter description here](/images/wiki/intellij-idea/fileheader.jpg)
 
 ### 自动换行
 
 打开 `File` / `Settings` / `Editor` / `Code Style`
 
勾选 `Wrap on typing` ，左侧 `Hard wrap at` 设置每行的长度。

 ![enter description here](/images/wiki/intellij-idea/autowrap01.png)

### Ctrl + L 代码排版时自动换行
 打开 `File` / `Settings` / `Editor` / `Code Style` / `Java` / `Wrapping and Braces`
 
 勾选 `Ensure right margin is not exceeded`
 
 ![enter description here](/images/wiki/intellij-idea/autowrap02.png)


 

 
 