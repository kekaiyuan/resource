---
layout: post
title: 加密
categories: [cate1, cate2]
description: some word here
keywords: keyword1, keyword2
mermaid: true
sequence: true
---

# 非对称加密

<div class="mermaid">
sequenceDiagram
    Alice-->>John: Hello John, how are you?
    John-->>Alice: Great!
</div>

```sequence
title: 非对称加密流程
Note over A: 生成密钥对 A
A->B: 发送公钥 A
Note over B: 生成密钥对 B
B->A: 发送公钥 B
Note over A: 使用\n公钥 B 加密\n数据
A->B: 发送密文
Note over B: 使用\n私钥 B 解密\n数据
Note over B: 使用\n公钥 A 加密\n数据
B->A: 发送密文
Note over A: 使用\n私钥 A 解密\n数据
```
非对称加密的安全性极高。
- 在网络中传递的只有公钥，并没有传递私钥。
- 第三方即使获取了公钥，也是无法解开密文的，密文必须使用私钥解密。

但是非对称加密的效率极低，比对称加密慢**数千倍**。

所以往往**混合**使用
- 使用非对称加密传递对称加密的密钥
- 使用对称加密传递一般数据

## 中间人攻击
```sequence
title: 中间人攻击
participant A
participant 中间人 as C
participant B
Note over A: 生成密钥对 A
A->C: 发送公钥 A
Note over C: 截取公钥 A/n得到公钥 A
C->B: 发送公钥 A
Note over B: 生成密钥对 B
B->C: 发送公钥 B
Note over C: 截取公钥 B
Note over C: 生成密钥对 C
C->A: 发送公钥 C
Note over A: 使用\n公钥 C 加密\n数据
A->C: 发送密文
Note over C: 截取密文
Note over C: 使用\n私钥 C 解密\n数据
Note over C: 修改原始数据
Note over C: 使用\n公钥 A 加密\n数据
C->B: 发送修改后的密文
Note over B: 使用\n私钥 B 解密\n修改后的数据
Note over B: 执行错误操作
```

真实案例

> 南京90后小伙从网上学来了一种小技巧，只花1分钱就能成功充值上百元话费，结果因涉嫌盗窃罪被警方批捕。
> 
> 据央视新闻报道，小伙无业，在一个“薅羊毛”的聊天群里，于是“先是用1分钱充了200元成功到账，然后就开始反复充值，一共到账58笔、26000余元。”
> 
> 除了这名90后小伙，聊天群的其他数十位网友，仅仅两天就通过这个攻略，“薅”走了160多万元话费。
> 
> 经调查，江苏某电子商务公司的网上话费充值平台由于没有设置校验环节，导致支付环节存在技术漏洞，从而被黑客利用。
> 
> 日前，小刘等人因涉嫌盗窃罪，被南京市检雨花台区检察院正式批捕。

这就是中间人攻击。
- 原先
	- 甲通过平台充值 200 元话费，平台会根据甲提供的支付手段（银行卡/支付宝/微信钱包）扣除 200 元。
- 中间人攻击
	- 乙通过平台充值 200 元话费，截取平台发送给支付方的消息，修改金额为 0.01 元。
- 为什么？
	- 话费充值平台没有设置校验环节。
		- 到底扣了用户多少钱？<br>
			金额对不对？

Note over B: 生成密钥对
B->A: 发送公钥 B
Note over A: 使用\n公钥 B 加密\n数据
A->B: 发送密文
Note over B: 使用\n私钥 B 解密\n数据
Note over B: 使用\n公钥 A 加密\n数据
B->A: 发送密文
Note over A: 使用\n私钥 A 解密\n数据


# 源码链接
该文章源码链接 [Github](url)