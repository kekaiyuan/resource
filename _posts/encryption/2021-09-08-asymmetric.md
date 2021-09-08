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
title: 中间人
participant C as 中间人
Note over A: 生成密钥对 A
A->C: 发送公钥 A
Note over C: 截取公钥 A
Note over C: 生成密钥对 C
C->B: 发送公钥 A
C->A: 发送公钥 C
Note over A: 使用\n公钥 C 加密\n数据
A->C: 发送密文
Note over C: 截取密文
Note over C: 使用\n私钥 C 解密\n数据
Note over C: 修改原始数据
Note over C: 使用\n公钥 C 加密\n数据

```
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