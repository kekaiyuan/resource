---
layout: post
title: 02-Java数据类型和运算符
categories: Java
description: some word here
keywords: Java
---


# 标识符
- 作用
	- 常量、变量、方法、类和包等的名称，类似于人的名称，用于标识某个特定的个体。
- 命名规则
	- 必须以字母、下划线“_”、美元符号“$”开头。
	- 其它部分可以是字母、下划线“_”、美元符“$”和数字的任意组合。
	- 大小写敏感，长度无限制。
	- 不可以是Java的关键字。
		- Java关键字是Java语言保留供内部使用的，是预先设置好的，有特殊意义的标识符。
		- ![enter description here](/images/posts/java/02datatype-operator/keyword.png)  
	- 注意
		- Java不采用通常语言使用的ASCII字符集，而是采用unicode这样的标准的国际字符集。
		- 因此Java命名所使用的字母除了英文还包括汉字，但是不推荐使用汉字。

# 字符集
- ASCII
	- 英文字符集 1个字节
- ISO8859-1
	- 西欧字符集 1个字节
- BIG5
	- 台湾的大五码，表示繁体汉字 2个字节
- GB2312
	- 大陆使用最早、最广的简体中文字符集 2个字节
- GBK
	- GB2312的扩展，可以表示繁体中文 2个字节
- GB18030
	- 最新GBK的扩展，可以表示汉字、维吾尔文、藏文等中华民族字符 2个字节
- Unicode
	- 国际通用字符集 2个字节
- ![enter description here](/images/posts/java/02datatype-operator/char-set.jpg)

# Java数据类型
Java是一种强类型语言
- 常量是有数据类型的
- 变量都必须声明其数据类型。
	- ![enter description here](/images/posts/java/02datatype-operator/datatype.png)
- 整型常量
	- ![enter description here](/images/posts/java/02datatype-operator/integer.png)
	- 十进制整数
		- 99,-500,0
	- 八进制整数
		- 以0开头，如：015
	- 十六进制整数
		- 0x或0X开头，如：0x15
	- 整型常量默认为int型，声明long型常量可以后加‘l’或‘L‘，推荐使用大写，便于与数字1区分
- 浮点型
	- ![enter description here](/images/posts/java/02datatype-operator/float.png)
	- float类型
		- 单精度，尾数到小数点后7位数，大多数情况下精度不够
	- double类型
		- 双精度，精度是float的双倍，推荐使用
	- 浮点型默认为double类型，要变为float，需在后面添加f/F，如3.14f/3.14F
	- 浮点数存在舍入误差，很多数字不能精确表示。如果需要进行不产生舍入误差的精确数字计算，需要使用BigDecimal类。
- 字符数据类型
	- 单引号用来表示字符常量，双引号表示字符串
		- 'A'是一个字符
		- "A"是含有一个字符的字符串
	- char类型用来表示在Unicode编码表中的字符
		- Unicode编码被设计用来处理各种语言的所有文字，它占2个字节，可允许有65536个字符；ASCII码占1个字节，可允许有128个字符（最高位是0），是Unicode编码表中前128个字符。
	- 使用转义字符‘\’来将其后的字符转变为其它的含义
		- charc2='\n';//代表换行符
		- ![enter description here](/images/posts/java/02datatype-operator/char.png)
	- char类型在内存中存储的是该字符的Unicode编码值，所以char类型可以当做int类型来处理
- 布尔数据类型
	- 

# 常量和变量
- 常量
	- 运行中值不会发生改变的数据
- 变量
	-  运行中值会发生改变的数据
