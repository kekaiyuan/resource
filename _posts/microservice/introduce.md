---
layout: post
title: 微服务架构——微服务简介
categories: 微服务
description: 微服务架构——微服务简介
keywords: 微服务简介
---



# 微服务技术对比

| | Dubbo | Spring Cloud | Spring Cloud Alibaba |
| :--: | :--: | :--: | :--: |
| 年份 |  |  |  |
| 注册中心 | Zookeeper, Redis | Euraka, Consul| 
| 服务远程调用 | Dubbo 协议 | Feign（http协议）|
| 配置中心 | 无 | Spring Cloud Config|
| 服务网关 | 无 | Spring Cloud Gateway, Zuul|
| 服务监控和保护 | dubbo-admin（功能弱） | Hystrix|

## Dubbo
Dubbo 的诞生时间很早，所以比起目前主流的微服务架构少了很多组件。
- 没有配置中心和服务网关
- Zookeeper 是集群管理工具，Redis 是缓存<br>
	这两个都不是专业的注册中心，Dubbo 只是用这两个工具实现了简单的注册中心
- 在服务监控和保护方面，自带了 dubbo-admin，但是功能非常弱，只有基础功能

它的重点是提出了服务远程调用的概念，并且定义了严格的 dubbo 协议。

在项目量级不够时，可以使用 Dubbo 实现简单的分布式服务。


# 源码链接
该文章源码链接 [Github](url)