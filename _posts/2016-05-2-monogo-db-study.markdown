---
layout:     post
title:      "Monogo DB Study"
subtitle:   "MonogoDB学习笔记"
date:       2016-05-20
author:     "Clover"
header-img: "img/post-bg-os-metro.jpg"
catalog: true
tags:
    - Clover
    - MonogoDB 
---

# MonogoDB  学习笔记

---


#入门篇

---

## 学习目标

---

#### Mongo 概念 

* `mongo` `MongoDB` `索引` `集合` `复制集` `分片` `数据均衡`

#### 学会MongoDB的搭建

* 搭建简单的单击服务
* 搭建具有冗余容错功能的复制集
* 搭建大规模数据集群
* 完成集群的自动部署

#### MongoDB的使用

* 最基本的文档的读写和更新
* 各种不同类型的索引的创建与使用
* 复杂的聚合查询
* 对数据集合进行分片，在不同分片间维持数据均衡
* 数据备份与恢复
* 数据迁移

#### 简单运维

* 部署集群
* 处理多种常见的故障
 
 ```
 单节点失效，如何恢复工作
 数据库意外被杀死如何进行数据恢复
 数据库拒绝服务如何排除原因
 数据库磁盘快慢时如何处理
 
 ``` 

---

## 学习资源
---
* **MongoDB官网** `www.mongodb.org`
* **MongoDB中国官网** `www.mongoing.com`
* **github** `https://github.com/mongodb/mongo`
* **中文文档** `docs.mongoing.com`
* **视频资源** `http://www.imooc.com/video/5934`

---

#开始学习

---

## 数据库

---
### 概念
1. 有组织的存放数据
2. 按照不同的需求进行查询

### 数据库分类
1. sql数据库：支持sql语言的数据库 Oracle,Mysql
2. NoSql数据库：不支持SQL语言的数据库 Redis,MongoDB

Sql 数据库 | NoSql数据库 
----------|------------- 
实时一致性   	  | 简单便捷 
事务          |方便扩展
多表联合查询   | 更好的性能

### 为什么是MongoDB
1. 无数据结构的限制
	1. 没有表结构的概念，每条记录可以有完全不同的结构
	2. 业务开发方便快捷
	3. sql数据库需要先定义表结构再使用
	
	```
	{name:"小明",sex:"男"}
	{name:"小明",address:"上海"}
	{name:"小明",home:[{"山东"},{"江西"}]}
	```
2. 完全的索引支持
	1. redis的key-value
	2. hbase的单索引	，二级索引需要自己实现
	
	```
	单键索引，多键索引：{x:1,y:1}
	数组索引：["apple","lemon"]
	全文索引：“I am a little bird” (中文)
	地理位置索引：2D
	```
3. 方便的冗余和扩展
	1. 复制集保证数据安全
	2. 分片扩展数据规模

4. 良好的支持
	1. 完善的文档
	2. 齐全的驱动支持

---

>安装过程 略过

---	 		

```
# mkdir data
# mkdir log
# mkdir conf
# mkdir bin
# cd conf
# vim mongodb.conf
  port=27017
  dbpath=data
  logpath=log/mongod.log
  fork=true
启动 # mongod -f conf/mongodb.conf   
```
连接 mongo 121.41.31.214:27017
数据库的关闭 连接时   db.shutdownServer()

---

## mongoDB的基本操作

---



```

查看所有数据库
> show dbs

切换数据库
> use dbname

删除当前数据库,（需要先use）
> db.dropDatabase();

创建表 (不要特殊创建 只需要use 一个不存在的name mongoDb会在需要的时候自己创建，一个表是一个集合)
> use dbname

数据的写入  db.集合名.insert(json数据) 表集合不存在的话自动创建 _id自动创建 _id自定义时不能重复
> db.dbname_collection.insert({x:1})

数据的查询 db.集合名.find()  参数可以为空，返回所有文档
> db.dbname_collection.find()
> db.dbname_collection.find({x:1})   查询x=1的文档

循环插入
> for(i=3;i<100;i++)db.dbname_collection.insert({x:i})

计数
> db.dbname_collection.find().count()

跳过，限制条数，排序
> db.dbname_collection.find().skip(3).limit(2).sort({x:1})


数据更新 db.集合名.find(查询条件,更新条目)  默认只会更新第一条找到的数据
> db.dbname_collection.update({x:1},{x:999})  会更新x=1的文档为 {x:999}
注意：db.dbname_collection.insert({x:1,y:2,z:3}) 若要更新z=3的文档 把x更新为22， 使用 db.dbname_collection.update({z:3},{x:22})  会把{x:1,y:2,z:3}整体覆盖为{x:22} 

局部更新
>db.dbname_collection.update({z:3},{$set:{x:22}})  局部更新 不会被覆盖

更新不存在的数据，即自动创建 第三个参数 true 标示不存在的时候自动创建
> db.dbname_collection.update({x:3},{x:22},true)

批量更新  第四个参数 true 批量更新
> db.dbname_collection.update({x:1},{$set:{x:22}},false,true)

数据删除  必须传参，默认删除所有找的数据
> db.dbname_collection.remove({x:1})
> db.dbname_collection.drop()

```


---

## MongoDB索引
 
---

#### 内容简介

1. 索引的种类和使用
2. 索引的匹配规则
3. 如何建立合适的索引
4. 索引建立的情况评估

```
查看索引
> db.dbname_collection.getIndexes()

创建索引  参数也是文档  1代表正向排序 -1 代表逆向排序
> db.dbname_collection.ensureIndex({x:1})

```

#### 索引的种类

1. _id索引
2.  单键索引
3.  多键索引
4.  复合索引
5.  过期索引
6.  全文索引
7.  地理位置索引

##### _id索引
* _id索引是绝大多数集合默认建立的索引
* 对于每个插入的数据，MongoDB都会自动生成一条唯一的_id字段

##### 单键索引
* 单键索引是最普通的索引
* 与_id索引不同，单键索引不会自动创建

##### 多键索引
* 多键索引与单键索引创建形式相同，区别在于字段的值
   * 单键索引：值为单一的值，例如字符串，数字或者日期
   * 多键索引：值具有多个记录，例如数组
##### 复合索引
* 当我们的查询条件不只有一个时，就需要建立复合索引
  * 插入{x:1,y:2,z:3}->按照x与y的值查询->创建索引db.collection.ensureIndex({x:1,y:1})->使用{x:1,y:1}作为条件进行查询  

##### 过期索引
* 过期索引：是在一段时间后会过期的索引
* 在索引过期后，相应的数据会被删除
* 这适合存储一些在一些时间后会失效的数据 比如用户的登陆信息，存储的日志  
* 建立方法：db.collection.ensureIndex({time:1},{expireAfterSeconds:10}),第二个参数是过期时间的秒数
* 过期索引的限制
	1. 存储在过期索引字段的值必须是指定的时间类型，说明：必须是ISODate或者ISODate数组，不能使用时间戳，否则不能被自动删除
	2. 如果指定了ISODate数组，则按照最小的时间进行删除
	3. 过期索引不能是复合索引
	4. 删除时间是不精确的 说明：删除过程是由后台程序每60S跑一次，而且删除也需要一些时间，所以存在误差

##### 全文索引 
*  全文索引：对字符串与字符串数组创建全文可搜索的索引
适用情况{author:"",title:"",article:""}
* 建立方法
	db.articles.ensureIndex({key:"text"})
	db.articles.ensureIndex({key_1:"text",key_2:"text"})
	db.articles.ensureIndex({"$**":"text"})
* 如何创建全文索引	
	db.imooc_2.ensureIndex({"article":"text"})
* 如何使用全文索引进行查询
	1.	db.articles.find({$text:{$search:"coffee"}})
	2.	db.articles.find({$text:{$search:"aa bb cc"}}) 查询 aa 或bb 或cc
	3.	db.articles.find({$text:{$search:"aa bb -cc"}}) 查询aa 或bb 不包含cc
	4.	db.articles.find({$text:{$search:"\"aa\" \"bb\" \"cc\""}}) 且的关系 既包含aa也包含bb也包含cc
* 全文索引的相似度
 
  $meta操作符：{score:{$meta:"textScore"}}
  写在查询条件后面可以返回结果的相似度与sort一起使用，可以达到很好的实用效果	db.articles.find({$text:{$search:"aa bb"}},{score:{$meta:"tetxScore"}}).sort({score:{$meta:"tetxScore"}})
* 全文索引的使用限制
  1.每次查询，只能指定一个$text查询
  2. $test查询不能出现在$nor查询中	
  3. 查询中如果包含了$test,hint不再起作用
  很可惜，MongoDB全文索引还不支持中文

#### 索引属性
1. 创建索引的格式
	db,collection.ensureIndex({param},{param}) 
	其中第二个参数便是索引的属性
2. 比较重要的属性有 名字 唯一性 稀疏性 是否定时删除
	* 名字，name指定：db.collection.ensureIndex({},{name:""})
	* 唯一性，unique指定：db.collection.ensureIndex({},{unique:true/false})
	* 稀疏性，sparse指定：db.collection.ensureIndex({},{sparse:true/false}) 默认是不稀疏的
	db.collection.find({m:{$exists:true}})查询存在m的文档
	db.collection.ensureIndex({m:1},{sparse:true})
	
	db.collection.find({m:{$exists:false}})
	**在选取索引  在稀疏索引上 查找是否存在 将不使用稀疏索引**
	强制使用索引 db.collection.find({m:{$exists:false}}).hint("m_1")
	
	* 是否定时删除，expireAfterSeconds指定
	TTL，过期索引
	 
##### 地理位置索引 
* 将一些点的位置存储在MongoDB中，创建索引后，可以按照位置来查找其他点
  * 子分类:
  1. 2d索引，用于存储和查找平面上的点。
  2. 2dsphere索引，用于存储和查找球面上的点
* 查找方式
  * 查找距离某个点一定距离的点  
  * 查找包含在某区域内的点

```

2D索引：平面地理位置索引
		创建方式:db.collection.ensureIndex({w:"2d"})
		位置表示方式：经纬度[经度,维度]
		取值范围：经度[-180,180]维度[-90,90]
		db.collection.insert({w:[100,2]})
		db.collection.find({w:{$near:[1,1]}})
		near会返回100个离查询的点最近的点
		db.collection.find({w:{$near:[1,1]，$maxDistance:10}})  
		
		查询方式：
		（1）$near查询：查询距离某个点最近的点
		（2）$geoWithin查询：查询某个形状内的点
		
		形状的标示：
		1.$box:矩形，使用
		{$box[[<x1>,<y1>],[<x2>,<y2>]]}  1.左边界 2.右边界
		2.$center:圆形，使用
		{$center:[[<x1>,<y1>],r]} 1.圆心位置 2.半径
		3.$polygon:多边形，使用
		{$polygon：[[<x1>,<y1>],[<x2>,<y2>],[<x3>,<y3>]]}
		
		db.collection.find({w:{$geoWithin:{$box:[[0,0],[3,3]]}}})
		
	geoNear查询
	geoNear使用runCommand命令进行使用
	{
	geoNear:<collection>,
	near:[x,y],
	minDistance:(对2D索引无效)
	MaxDistance:
	num:
	}
	
	db.runCommand({geoNear:"location",near:[1,2],maxDistance:10,num:1})
	
		
2dsphere: 
创建方式:db.collection.ensureIndex({w:"2dsphere"})
位置表示方式
GeoJson:描述一个点，一条直线，多边形等
格式：{type:"",coordinates:[<coordinates>]}
查询方式和2D索引查询方式类似
支持$minDistance与$maxDistance

```  

---

## 索引构建情况分析

---

* 索引好处：加快索引相关的查询
* 索引不好处：增加磁盘空间消耗，降低写入性能

---
* 如何评判当前索引构建情况
	1. mongostat工具介绍
	2. profile集合介绍
	3. 日志介绍
	4. explain分析

##### mongostat工具
 * mongostat查看mongodb运行状态的程序
 * 使用说明	:mongostat -h 127.0.0.1:12345
 
 * 字段说明:
 	* inserts
 	* query
 	* update
 	* delete
 	* getmore
 	* command
 	* flushes
 	* mapped
 	* vsize
 	* res
 	* non-mapped
 	* faults
 	* locked
 	* idx miss 索引情况
 	* qr|qw
 	* ar|aw
 	* netIn
 	* netOut
 	* conn
 	* set
 	* repl
 	
##### profile集合
 * db.getProfilingStatus()
 * db.setProfilingLevel(2)

 * db.system.profile.find().sort({$natural:-1}).limit(1)  
 
##### 日志
 * mongodb.conf 中 verbose=vvvvv    v越多代表日志越详细
  
##### explain
* db.collection.find({x:1}).explain()


---

## MongoDB 安全

---

###安全概览
1. 最安全的是物理隔离：不现实
2. 网络隔离其次
3. 防火墙再其次
4. 用户名密码在最后

---

#### 开启权限验证
1. auth开启
mongodb.conf 设置  auth=true;

2. keyfile开启

##### MongoDB创建用户
1. 创建语法：createUser(2.6之前为addUser)
2. 
```
{
user:"<name>",
pwd:"<password>",
sustomData:{<说明>},
roles;[{role:"<role>",db:"database"}]
}
```
3. 角色类型：内建类型（read,readWrite,dbAdmin,dbOwner,userAdmin）

##### 用户角色详解

1. 数据库角色（read,readWrite,dbAdmin,dbOwner,userAdmin）
2. 集群角色（cluterAdmin,clusterManager）
3. 备份角色（backup,restore）
4. 其他特殊权限（DBAdminAnyDatabase） 




	
 



 


