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
		``` bash
		# redis-server
		```
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
		Redis没有整数类型，但提供了用于整数操作的命令。整数回复以（integer）开头，并在后面跟上整数数据：
			
			redis> INCR foo
			(integer) 1
	* 字符串回复
		字符串回复是最常见的一种回复类型，当请求一个字符串类型键的值或一个其他键中的某个元素时就会得到一个字符串。字符串回复以双引号包裹：
		
			redis> GET foo
			"1"		
	* 多行字符串回复
		多行字符串回复中的每行字符串都以一个序号开头
			
			redis > KEYS *
			1) "bar"
			2) "foo"
			
## 4）配置

通过`redis-server`的启动参数port设置了Redis的端口号，除此之外还有其他的配置选项，如开启持久化，日志级别等。由于配置选项多，redis支持通过配置文件来配置这些选		
	
	#redis-server /path/to/redis.conf
	
通过启动参数传递同名的配置选项会覆盖配置文件中相应的参数

	#redis-server /path/to/redis.conf --loglevel warning
	
Redis提供了一个配置文件的模板redis.conf位于源代码目录

还可以在运行时通过`CONFIG SET`	命令在不重启Redis的情况下动态修改部分Redis配置
	
	redis>CONFIG SET loglevel warning
	OK		
并不是所有的配置都可以使用CONFIG SET 命令修改		
	
## 4）多数据库
每个 数据库 对外 都是 以 一个 从 0 开始 的 递增 数字 命名， Redis 默认 支持 16 个 数据库， 可以 通过 配置 参数 databases 来 修改 这一 数字。 客户 端 与 Redis 建立 连接 后 会 自动 选择 0 号 数据库， 不过 可以 随时 使用SELECT命令更换数据库，如要选择1号数据库：
		
		redis>SELECT 1
		OK
		redis[1] GET foo
		(nil)
redis不支持自定义数据库的名字		
	
	
---

# 二、入门

## 1）字符串类型

一个字符串类型键允许存储的数据的最大容量是512M。

1. 赋值与取值
		
	`SET key value`
	
	`GET key  `
		
set和get是redis中最简单的两个命令

	当键不存在时返回空结果

2. 递增数字
	
		INCR key

当存储的字符串是整数形式时，Redis提供了一个实用的命令INCR，其作用是让当键值递增，并返回递增后的值

		redis>INCR num
		(integer) 1
		redis>INCR num
		(integer) 2
当要操作的键不存在时默认键值为0，所以第一次递增后的结果是1.

当键值不是整数是会提示错误
		
		redis> SET foo lorem
		OK 
		redis> INCR foo
		(error) ERR value is not an integer or out of range
		
3. 命令拾遗
	
	1).增加指定的整数
	
		INCRBY key increment
		INCRBY通过 increment 参数指定一次增加的数值
		redis> INCRBY bar 2
		(integer)2
		redis> INCRBY bar 3
		(integer)5
	
	2).减少指定的整数
	
		DECR key
		DECRBY key increment
		redis> DECR bar 
		(integer)4
		
	3).增加指定浮点数
	
	`INCRBYFLOAT key increment`
	
	递增一个双精度浮点数	
	
		redis>INCRBYFLOAT bar 2.7
		"6.7"
		redis>INCRBYFLOAT bar 5E+4
		"50006.69999999999999929"
		
	4).向尾部追加值
	
	`APPEND key value`
	
	APPEND 作用是向键值的末尾追加value.如果键不存在则将该值设置为value,即相当于SET key value.返回值是追加后字符串的总长度
		
		redis>SET key hello
		OK
		redis>APPEND key " world!"
		（integer）12
	此时key的值是"hello world!"。APPEND命令的第二个参数加了双引号，原因是该参数包含空格，在redis-cli中输入需要双引号以示区分
	
	5).获取字符串长度
	
	`STRLEN key`
	
	STRLEN 命令返回键值的长度，如果键不存在返回0 
	
		redis>STRLEN key
		(interger)12
		redis>SET key 你好
		OK
		redis>STRLEN key
		(interger)6
		
	前面提到字符串类型可以存储二进制数据，所以它可以存储任意编码的字符串。例子中接收到的是使用UTF-8编码的中文，由于你好的UTF-8编码的长度都是3，所以返回6。
	
	6).同时获得/设置多个键值
		
	`MGET key[key ...]`
	
	`MMSET key value [key value ...]`
		
		redis> MSET key1 v1 key2 v2 key3 v3
		redis> GET key2
		"v2"
		redis > MGET key1 key3
		1)"v1"
		2)"v3"
		 
	7). 位操作
	
	`GETBIT key offset`
	
	`SETBIT key offset value`
	
	`BITCOUNT key [start] [key ...]`
	
	`BITOP operation destkey key [key ...]`
	
---	
			

## 2）散列类型

1. 赋值与取值

	`HSET key field value`
	
	`HGET key field	`
	
	`HMSET key field value [field value ...]`
	
	`HMGET key field [field ...]`
	
	`HGETALL key`
	
		redis> HSET car price 500
		(interger)1
		redis> HSET car name BMW
		(interger)1
		redis>HEGT car name
		"BMW"
	HSET命令方便之处在于不区分插入和更新操作，这意味着修改数据时不用事先判断字段是否存在来决定要执行的是插入操作还是更新操作。当执行的是插入操作时HSET会返回1，当执行的是更新操作是 HSET返回0。`当键本身不存在是，HSET命令还会自动创建`。
		
	`REDIS中每个键都属于一个明确的数据类型，如通过HSET命令建立的键都是散列类型，通过SET命令建立的键是字符串类型。使用一种数据类型的命令操作另外一种数据类型的键会提示错误“ERR Operation against a key holding the wrong kind of value”`	
	
		redis> HMGET car price name
		1)"500"
		2)"BMW"
		
		redis> HGETALL car  
		1)"price"
		2)"500"
		3)"name"
		4)"BMW"	
 
2. 判断字段是否存在
   
   `HEXISTS key field`
   
   HEXISTS命令用来判断一个字段是否存在。如果存在则返回1，否则返回0（`如果键不存在也会返回0`）。
   	 
   	 	redis>HEXISTS car model
   	 	(integer)0
   	 	redis>HSET car model C200
   	 	(integer)1
   	 	redis>HEXISTS car model
   	 	(integer)1

3. 当字段不存在时赋值

	`HSETNX key field value`

	HSETNX 命令与HSET命令相似，区别在于如果字段已经存在，HSETNX命令不执行任何操作

	HSETNX 命令是原子操作，不用担心竞态条件
	
4. 增加数字

	`HINCRBY key field increment`
	
		redis> HINCRBY person score 60
		(integer)60
	之前person键不存在，HINCARBY命令会自动建立该键并默认score字段在执行前的值为0.命令的返回值是增值后的字段值
	
5. 删除字段

	`HDELkey field [field]`
	
	HDEL 命令可以删除一个或者多个字段，返回值是被删除的字段个数
		
		redis> HDEL car price
		(integer)1
		redis> HDEL car price		
		(integer)0



6. 存储文章数据

		$postID=INCR posts:count
		#判断用户输入的slug是否可用，如果可用则记录
		$isSlugAvailable=HSETNX slug.to.id,$slug ,$postIID
		if $isSlugAvailable is 0
		#slug已经用过了需要提示更换slug
		exit
		HMSET post:$postID,title,$title,content,$content,slug,$slug,...
		这段代码使用了HSETNX命令原子的实现了HEXISTS和HSET两个命令以避免竞态条件。当用户访问文章时，我们从网址中得到文章的缩略名，并查询slug.to.id键来获取文章ID：
		$postID=HGET slug.to.id,$slug
		if not $postID
		print 文章不存在
		exit
		$post=HGETALL post:$postID
		print文章标题:$post.title
		需要注意的是如果要修改文章的缩略名一定不能忘了修改slug.to.id键对应的字段。
		#判断新的slug是否可用，如果可用则记录
		$isSlugAvailable=HSETNX slug.to.id ,$newSlug,42
		if $isSlugAvailable is 0
		exit
		#获得旧的缩略名 
		$oldSlug=HGET post:42,slug
		#设置新的缩略名
		HSET post:42,slug,$newSlug
		#删除旧的缩略名
		HDEL slug.to.id ,$oldslug
		
7. 命令拾遗

1）只获取字段名或字段值

`HEKYS key`

`HVALS key`

有时仅仅需要获取键中所有字段的名字而不需要字段值，那么可以使用HEKYS命令
	
		redis> HKEYS car 
		1)"name"
		2)"model"
HVALS 命令与HEKYS命令相对应，HVALS命令用来获得键中所有字段值
		
		redis> HVALS car
		1)"BMW"
		2)"C200"

2)获取字段数量

`HLEN key`

	redis> HLEN car
	(integer) 2 				


## 3）列表类型

1. 向列表两端增加元素

	`LPUSH key value [value ...]`
	
	`RPUSH key value [value ...]`
	
	LPUSH命令用来向列表左边增加元素，返回值表示增加元素后列表的长度
	
		redis> LPUSH numbers 1
		(integer) 1
		
		[1]
		
		redis> LPUSH numbers 2 3 
		(integer) 3
		
		[ 3 2 1 ]
	
	LPUSH 会向列表左边先加入 2 再加入 3
		
		redis> RPUSH numbers 0 -1
		(integer) 5
		
		[3 2 1 0 -1]	

2. 从列表两端弹出元素

	`LPOP key`

	`RPOP key `

	有进有出，LPOP可以从列表左边弹出一个元素。LPOP命令执行两步操作：第一步是将列表左边的元素从列表移除，第二步是返回被移除的元素值

		redis> LPOP numbers
		"3"
	
		redis> RPOP numbers
		"-1"

	`结合上面提到的4个命令可以使用列表类型来模拟栈和队列的操作`

	`栈：LPUSH和LPOP   RPUSH和RPOP`

	`队列：LPUSH和RPOP  RPUSH和LPOP `	



3. 获取列表中元素的个数

	`LLEN key`

	当键不存在时LLEN会返回0

		redis>LLEN numbers
		(integer) 3
	
	LLEN功能类似SQL语句 SELECT COUNT(*) FROM table_name 但是LLEN的时间复杂度为O(1) ,使用时Redis会直接读取现成的值，而不需要统计
	
4. 获取列表片段

	`LRANGE key start stop`
	
	返回索引从start到stop之间的所有元素（包含两端的元素），起始索引为0
	
		redis> LRANGE numbers 0 2
		1)"2"
		2)"1"
		3)"0"
	LRANGE在取得列表片段的同时不会像LPOP一样删除该片段
	
	LRANGE也支持负索引 表示从右边开始计算序数
	-1 表示最右边第一个元素 -2便是最右边第二个元素
	
		redis> LRANGE nubmers -2 -1
		1)"1"
		2)"0"	
	
	显然，LRANGE numbers 0 -1 可以获取列表中所有元素
	
	`特殊情况`
	
	* 如果start的索引位置比stop的索引位置靠后，返回空列表
	* 如果stop大于实际的索引范围，则返回到列表最右边的元素
	
		redis> LRANGE nubmers 1 999
		1)"1"
		2)"0"
	
5. 删除列表中指定的值
		
	`LREM key count value`
	
	LREM 会删除列表前count个值为value的元素，返回值是实际删除的元素个数，根据count值的不同，LREM执行方式有差异
	
	* 当count>0时，LREM会从列表左边开始删除前count个值为value的元素
	* 当count<0时，LREM会从列表右边开始删除|count|个值为value的值
	* 当count=0时，LREM会删除所有值为value的元素

6. 命令拾遗

	1.获得/设置指定索引的元素值
	
	`LINDEX key index`
	
	`LSET key index value`
	
	`LINDEX 返回指定索引的元素`
		
		redis> LINDEX numbers 0
		"2"
		redis> LINDEX numbers -1
		"0"
		
	LSET是另一个通过索引操作列表的命令,它会将索引为index的元素赋值为value.例如
		
		redis> LSET nubmers 1 7 
		OK 
		redis> LINDEX nubmers 1
		"7"			
		
	2.只保留列表指定片段
	
	`LTRIM key start end`
	
	LTRIM命令可以删除指定索引范围之外的所有元素，其指定列表范围的方法和LRANGE命令
	
		redis> LRANGE numbers 0 -1
		1)"1"
		2)"2"
		3)"7"
		4)"3"
		"0"
		redis> LTRIM numbers 1 2
		OK
		redis> LRANGE numbers 0 1
		1)"2"
		2)"7"
		
	LTRIM命令常和LPUSH命令一起来使用用来限制列表中元素的数量，比如日志只保留100条
	
	LPUSHlogs $newlog
	
	LTROM logs 0 99
	
	3.向列表中插入元素
	
	`LINSERT key BEFORE | AFTER pivot value`
	
	LINSERT 命令首先会在列表从左到右查找值为pivot的元素，然后根据BEFORE还是AFTER来决定将value插入到该元素的前面还是后面
	
		redis> LRANGE numbers 0 -1
		1)"2"
		2)"7"
		3)"0"
		redis> LINSERT numbers AFTER 7 3
		(integer) 4
		redis> LRANGE numbers 0 -1
		1)"2"
		2)"7"
		3)"3"
		4)"0"
		
	4.将元素从一个列表转到另一个列表
	
	`RPOPLPUSH source destination`
	
	RPOPLPUSH功能：先执行RPOP命令再执行LPUSH命令。RPOPLPUSH命令会先从source列表类型键的右边弹出一个元素，然后将其加入到destination列表类型键的左边，并返回这个元素的值，整个过程是原子的。
	
	当把列表类型作为队列使用时，RPOPLPUSH命令可以很直观的在多个队列中传递数据。当source和destination相同时，RPOPLPUSH命令会不断地将对尾的元素移到队首，借助这个特性我们可以实现一个网站监控系统：使用一个队列存储需要监控的网址，然后监控程序不断地使用RPOPLPUSH命令循环取出一个网址来测试可用性。RPOPLPUSH的好处在于在程序执行过程中仍然可以不断的向网址列表中加入新网址，整个系统易扩展，允许多个客户端同时处理队列
		

## 4）集合类型


`在集合中的每个元素都是不同的，且没有顺序。一个集合类型（set）键可以存储之多232-1个字符串`

1. 增加/删除元素

	`SADD key member [member ...]`
	
	`SREM key member [member ...]`
	
	SADD命令用来向集合中增加一个或多个元素，如果键不存在则自动创建。因为一个集合中不能有相同的元素，相同键的元素会被忽略执行
	
		redis> SADD letters a 
		(integer) 1
		redis> SADD letters a b c
		(integer) 2
	
	第二条SADD命令返回值为2是因为元素a已经存在，所以实际只加入了两个元素

	SREM命令用来从集合正删除一个或者多个元素，并返回删除成功的个数
	
		redis> SREM letters c d 
		(integer) 1

2. 获得集合中的所有元素

	`SMEMBERS key`
	
	SMEMBERS命令会返回集合中的所有元素
		
		redis> SMEMBERS letters
		1)"b"
		2)"a"

3. 判断元素是否在集合中

	`SMEMBERS key member`
	
	判断一个元素是否在集合中是一个时间复杂度为O(1)的操作，无论集合元素多少，都可以很快速的返回结果。当值存在时SMEMBERS命令返回1，不存在返回0
	
		redis> SMEMBERS letters a 
		(integer) 1
		redis> SMEMBERS letters d
		(integer) 0

4. 集合间运算

	`SDIFF key [key ...]`
	
	`SINTER key [key ...]`
	
	`SUNION key [key ...]`	

	* SDIFF用来对多个集合执行差集运算.
	
	集合A与集合B的差集表示A-B，代表所有属于A且不属于B的元素构成的集合
	
	```
	redis > SADD setA 1 2 3
	(integer) 3
	redis > SADD setB 2 3 4
	(integer) 3
	redis > SDIFF setA set B 
	1）"1"
	redis > SDIFF setB setA 
	1) "4" 
	```
	
	SDIFF支持同时传入多个键
	`redis > SDIFF setA setB setC` 计算顺序是先计算setA-setB,在计算结果与setC差集。
	
	* SINTER命令用来对多个集合执行交集运算。
	
	集合A与集合B的交集表示为A∩B，代表所有属于A且属于B的元素构成的集合
	
	```
	redis > SADD setA 1 2 3
	(integer) 3
	redis > SADD setB 2 3 4
	(integer) 3
	redis > SINTER setA setB
	1)"2"
	2)"3"
	```

	* SUNION 命令用来对多个集合执行并集运算
	
	集合A与集合B的并集表示为A∪B，代表所有属于A或者属于B的元素构成的集合
	```
	redis > SADD setA 1 2 3
	(integer) 3
	redis > SADD setB 2 3 4
	(integer) 3
	redis > SINTER setA setB
	1)"1"
	2)"2"
	3)"3"
	4)"4"
	```

5. 命令拾遗

	* 获得集合中元素个数
	
		`SCARD key`
	
	SCARD命令用来获得集合中的元素个数
	
	```
	redis > SMEMBERS letters
	1)"b"
	2)"2"
	redis > SCARD letters
	(integer)2
	```

	* 进行集合运算并将结果存储
	
		`SDIFFSTORE destination key  [key ...]`
	
		`SINTERSTORE destination key [key ...]`
	
		`SUNINONSTORE destination key [key ...]`
	 
	结果存储在 destination的键中
	
	* 随机获得集合中的元素
	
		`SRANDMEMBER key	[count]`
	
	SRANDMEMBER 命令用来随机从集合中获取一个元素
	
	```
	redis > SRANDMEMBER letters
	"a"
	redis > SRANDMEMBER letters
	"b"
	```
	
	count来控制一次获取多个元素
	
		1）当count为正数时，随机获取count个数不重复的数，大于元素总个数，返回全部
	
		2）当count为负数时，随机获取|count|个元素，这些元素`有可能相同`
	
	* 从集合中弹出一个元素
	
		`SPOP key `
	
	由于集合是无序的，所以SPOP会从集合中随机选择一个元素弹出
	
	```
	redis >SOPO letters	
	"b"	
	redis > SMEMBERS letters
	1)"a"
	2)"c"
	3)"d"
	```	
	
	
## 5）有序集合类型

`在集合的基础上有序集合类型为集合中的每个元素都关联了一个分数，这使得我们不仅可以完成插入、删除和判断元素是否存在等集合类型支持的操作，还能够获得分数最高（或最低）的前N个元素`

有序集合类型在某些方面和列表类型有些相似

	* （1）二者都是有序的
	* （2）二者都可以获得某一范围的元素

但是二者有很大的区别，使得他们的应用场景也不相同

	* （1）列表类型通过链表实现，获取靠近两端的元素极快，当元素增多时，访问中间元素的速度会变慢，所以他更加适合实现如“新鲜事”“日志”这样很少访问中间元素的应用
	* （2）有序集合类型是使用散列表和跳跃表实现的，即使读取中间部分的 数据也很快（时间复杂度是O(log(N))）
	* （3）列表中不能简单的调整某个元素的位置，但是有序集合可以（通过更改元素的分数）
	* （4）`有序集合比列表类型更消耗内存`



1. 增加元素

	`ZADD key score member [score member...]`
	
	ZADD命令用来向有序集合中添加一个元素和该元素的分数，如果该元素已经存在则会用新的分数替换原有的分数。ZADD返回的是新加入到元素集合中的个数（不包含之前已经存在的元素）
	
	```
	redis> ZADD scoreboard 89 Tom 67 Peter 100 David
	(integer)3
	```
	
	```
	redis> ZADD scoreboard  76 Peter 
	(integer)0
	```
	
	分数不仅可以是整数，还支持双精度浮点数
	```
	redis> ZADD testboard 17E+307 a
	(integer)1
	redis> ZADD testboard 1.5 b
	(integer)1
	redis> ZADD testboard +inf c
	(integer)1
	redis> ZADD testboard -inf d
	(integer)1
	```
	
	其中+inf和-inf分别表示正无穷和负无穷
	
2. 获得元素的分数
	
	`ZSCORE key member`
	
	
	```
	ZSCORE scoreboard Tom 
	"89"
	```
		
3. 获得排名在某个范围的元素列表	




	
	
	
	
	
			
			