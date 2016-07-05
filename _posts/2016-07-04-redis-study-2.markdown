---
layout:     post
title:      "Redis Study 之二 入门指南"
subtitle:   "Redis学习笔记"
date:       2016-07-04
author:     "Clover"
header-img: "img/post-bg-universe.jpg"
catalog: true
tags:
    - Clover
    - Redis
    - DB 
    
---

>参考书籍：Redis入门指南（第二版）李子骅 编著 人民邮电出版社

# 一.准备

## I 启动Redis 

1. 直接启动

		# redis-server
		
	Redis 服务器 默认 会 使用 6379 端口 ,通过-- port 参数 可以 自定义 端口

		# redis-server --port 6380		
		
2. 通过初始化脚本启动（略过）		
3. 指定配置文件启动

		# redis-server redis.conf
		
## II 停止Redis

* **Redis有可能正在将内存中的数据同步到硬盘中，强行终止Redis进程可能会导致数据 丢失。 正确停止Redis的方式应该是向Redis发送SHUTDOWN 命令**
		
		# redis-cli shutdown

* 当Redis收到shutdown命令后，会先断开所有的客户端连接，然后根据配置执行持久化，最后完成退出
* Redis会妥善处理`sigterm`信号	,所以使用kill Redis进程的PID也可以正常结束Redis,效果与发送shutdown命令一样。

## 二 Redis命令行客户端

redis-cli是Redis自带的基于命令行的Redis客户端

### I 发送命令

