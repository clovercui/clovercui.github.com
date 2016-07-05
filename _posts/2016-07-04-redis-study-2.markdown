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

# 一、准备

## 1）启动Redis 

1. 直接启动

		# redis-server
		
	Redis 服务器 默认 会 使用 6379 端口 ,通过-- port 参数 可以 自定义 端口

		# redis-server --port 6380		
		
2. 通过初始化脚本启动（略过）		
3. 指定配置文件启动

		# redis-server redis.conf
		
## 2）停止Redis

* **Redis有可能正在将内存中的数据同步到硬盘中，强行终止Redis进程可能会导致数据 丢失。 正确停止Redis的方式应该是向Redis发送SHUTDOWN 命令**
		
		# redis-cli shutdown

* 当Redis收到shutdown命令后，会先断开所有的客户端连接，然后根据配置执行持久化，最后完成退出
* Redis会妥善处理`sigterm`信号	,所以使用kill Redis进程的PID也可以正常结束Redis,效果与发送shutdown命令一样。

## 3）Redis命令行客户端

redis-cli是Redis自带的基于命令行的Redis客户端

1. 发送命令

	redis-cli向Redis发送命令有两种方式。
	
	一是将命令作为redis-cli的参数执行 ,`-h`自定义地址 `-p`自定义端口号
	
		# redis-cli -h 127.0.0.1 -p 6379
		
	redis提供`PING`命令来测试客户端语Redis连接是否正常
		
		# redis-cli PING
		PONG	
		
	二是不附带参数运行redis-cli，这样会进入交互模式可以自由输入命令
	
		# redis-cli
		redis 127.0.0.1:6379> PING
		PONG
		redis 127.0.0.1:6379> ECHO hi
		"hi"
2. 命令返回值
	* 状态回复
	
		状态是最简单的一种回复如SET 会回复OK	
	* 错误回复	
		当出现命令不存在或命令格式有错误时Redis会返回错误回复（error replay）.错误回复以（error）开头，并在后面跟上错误信息。如执行一个不存在的命令：
		
			redis>ERRORCOMMEND
			(error)ERR unknown command 'ERRORCOMMEND'
			
		在 2. 6 版本 时， 错误 信息 均 是以“ ERR” 开头， 而在 2. 8 版 以后， 部分 错误 信息 会 以 具体 的 错误 类型 开头， 如： 
					
			redis> LPUSH key 1 (integer) 1
			redis> GET key 
			(error) WRONGTYPE Operation against a key holding the wrong kind of value 
		
		这里 错误 信息 开头 的“ WRONGTYPE” 就 表示 类型 错误， 这个 改进 使得 在调 试	
		
	* 整数回复	
			
			