---
layout: post
title: 用户登录那些事儿—— session 和 token
categories: [cate1, cate2]
description: some word here
keywords: keyword1, keyword2
---

**session** 和 **token** 都是用于保持会话的。

很久很久之前，在 Web 刚刚诞生的时候，这时候的 Web 是**没有记忆**的。
- 此时的 Web 放置的都是文档。
- 谁来看文档，什么时候看的，看了几次…… Web 统统不知道。
- 只要有请求发来，Web 就响应请求。
- 所有的请求在 Web 看来都是一样的：看看这份文档。

后来随着时间的推进，Web 诞生了丰富的功能


token 延期策略
- 每天第一次请求，延期 token。<br>
	可以延期 24 小时，实现用户每天访问不需要重复登录。
- 每次接口请求，延期 token。
- 双 token
	- 	access_token
	- 	refresh_token
	- 	refresh_token 的有效期会比 access_token 长。<br>
		当 access_token 过期了而 refresh_token 没有过期时。<br>
		重新生成**两**个 token 返回
- 双 access_token
	-  当 access_token 快过期时，生成一个**新**的 access_token 。
	-  客户端将在下一次请求开始使用**新**的 access_token。
	-  比起直接延期更丝滑


使用 Redis 存储 token

用户登录后将 token 存于 Redis，设置**过期时间**。<br>
之后每次访问都会先去查询 Redis，如果存在，**延期**。<br>
不存在，重新登录。



# 源码链接
该文章源码链接 [Github](url)