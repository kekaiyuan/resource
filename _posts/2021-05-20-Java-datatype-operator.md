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
- 命名规范
	- 所有命名做到见名知意
	- 变量、方法名：首字母小写，驼峰原则
		- run(),runRun(),age,ageNew,monthSalary
		- 驼峰原则
			- 单词的首字母大写
	- 类名、接口名：首字母大写和驼峰原则 
		- Man、GoodMan
	- 常量名：大写+下划线
		- MAX_VALUE

# 字符集
- ASCII
	- 英文字符集 
	- 1个字节
- ISO8859-1
	- 西欧字符集 
	- 1个字节
- BIG5
	- 台湾的大五码，表示繁体汉字 
	- 2个字节
- GB2312
	- 大陆使用最早、最广的简体中文字符集 
	- 2个字节
- GBK
	- GB2312的扩展，可以表示繁体中文 
	- 2个字节
- GB18030
	- 最新GBK的扩展，可以表示汉字、维吾尔文、藏文等中华民族字符 
	- 2个字节
- Unicode
	- 国际通用字符集 
	- 2个字节
- ![enter description here](/images/posts/java/02datatype-operator/char-set.jpg)

# Java数据类型
- Java是一种强类型语言
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
- 字符数据类型（2个字节）
	- 单引号用来表示字符常量，双引号表示字符串
		- 'A'是一个字符
		- "A"是含有一个字符的字符串
	- char类型用来表示在Unicode编码表中的字符
		- Unicode编码被设计用来处理各种语言的所有文字，它占2个字节，可允许有65536个字符；ASCII码占1个字节，可允许有128个字符（最高位是0），是Unicode编码表中前128个字符。
	- 使用转义字符‘\’来将其后的字符转变为其它的含义
		- ![enter description here](/images/posts/java/02datatype-operator/char.png)
	- char类型在内存中存储的是该字符的Unicode编码值，所以char类型可以当做int类型来处理
- 布尔数据类型（一位，不是一个字节）
	- boolean类型只有两个值，true和false，用来判断逻辑条件，控制程序流程

# 常量和变量
- 常量
	- 运行中值不会发生改变的数据
- 变量
	-  运行中值会发生改变的数据
	-  使用final修饰的变量值不可更改，变成了常量

# 运算符
- java支持以下运算符
	- 算术运算符:+，-，*，/，%，++，--
	- 赋值运算符:=
	- 扩展赋值运算符:+=，-=，*=，/=
	- 关系运算符:<，>，>=，<=，==，!=  
	- 逻辑运算符:&&，\|\|，!
	- 位运算符:&，\|，^，~，>>，<<，>>>
	- 条件运算符:？：
- 算术运算符
	- ![enter description here](/images/posts/java/02datatype-operator/math-op.png)
	- 二元运算符的类型提升
		- 整数运算
			- 如果两个操作数有一个为Long,则结果也为long
			- 没有long时，结果为int。即使操作数全为shot,byte，结果也是int
		- 浮点运算
			- 如果两个操作数有一个为double,则结果为double.
			- 只有两个操作数都是float,则结果才为float.
	- 一元运算符
		- ++,--
		- ![enter description here](/images/posts/java/02datatype-operator/unitary.png)
- 赋值运算符
	- 基本赋值运算符
		- =
	- 扩展赋值运算符
		- 算术运算符和赋值运算符结合
		- ![enter description here](/images/posts/java/02datatype-operator/ex-op.png)
- 关系运算符
	- 用于比较运算
	- 结果为布尔值
- 逻辑运算符
	- 逻辑运算符与布尔操作数一起使用，组成逻辑表达式
	- 逻辑表达式的结果是布尔值
	- ![enter description here](/images/posts/java/02datatype-operator/logic-op.png)
	- &和&&的区别
		- &
			- 无论任何情况，“&”两边的表达式都会参与计算
		- &&
			- 当“&&”的左边为false，则将不会计算其右边的表达式。即左false则false
		- “|”和“||”的区别与“&”和“&&”的区别类似。
- 位运算符
	- 对操作数以二进制比特位为单位进行操作和运算，操作数和结果都是整型数
	- 如果操作的对象是char、byte、short，位移动作发生前其值会自动晋升为int，运算结果也为int
	- ![enter description here](/images/posts/java/02datatype-operator/an-op.png)
- 条件运算符
	- 语法格式
		- x ? y : z
		- 唯一的三目运算符
	- 执行过程
		- x 为 boolean 类型表达式
		- x为true，整个三目运算的结果为表达式 y 的值
		- x为false，整个三目运算的结果为表达式 z在的值
	- 经常用来代替简单的if-else判断
- 运算符的优先级
	- ![enter description here](/images/posts/java/02datatype-operator/op-priority.png)
	- 赋值<三目<逻辑<关系 <算术<单目

# 类型转换
- 在赋值运算或算术运算时，要求数据类型相同，否则要进行类型转换
- 转换方式
	- 自动转换
	- 强制转换
- 除boolean类型外，所有的基本数据类型因为各自的精度不同，赋值时都要考虑这个问题
	
	
	
	
	
	
