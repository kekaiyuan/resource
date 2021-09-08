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
Note over A: 生成密钥对
A->B: 发送公钥 A
Note over B: 生成密钥对
B->A: 发送公钥 B
Note over A: 使用\n公钥 B\n加密数据
A->B: 发送密文
Note over B: 使用私钥 B 解密数据
Note over B: 使用公钥 A 加密数据
B->A: 发送密文
Note over A: 使用私钥 A 解密数据
```

Node over A: 生成密钥对

# 源码链接
该文章源码链接 [Github](url)