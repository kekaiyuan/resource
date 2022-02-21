---
layout: wiki
title: Windows
categories: Windows
description: Windows Wiki
keywords: Windows
---

# 常用命令
查看端口
```
netstat -ano | findstr "端口号"
```
查看进程
```
tasklist | findstr "进程号"
```
杀死进程
```
taskkill /f /t /im 进程名
```
