注：

1. redis运行文件再/usr/local/bin下面
2. redis原始文件在/www/server/redis下面



# Nosql概述

## 为什么要用Nosql？

> 1. 单机MYSQL的年代

![image-20220530230017091](C:\Users\啊坤\AppData\Roaming\Typora\typora-user-images\image-20220530230017091.png)

90年代，一个基本的网站访问量一般不会太大，单个数据库完全足够！

那个时候更多的去使用静态网页HTML~服务器根本就没有太大的压力！

思考以下，这种情况下：整个网站的瓶颈是什么？

1. 数据量如果太大，一个机器放不下！
2. 数据的索引（B+Tree），一个机器内存也放不下
3. 访问量（读写混合），一个服务器承受不了~

只要你开始出现以上的三种情况致一，那么你就必须要晋级！

> 2.Memcached（缓存）+MYSQL+垂直拆分（读写分离）

网站80%的情况都是在读，每次都去查询数据库的话，就十分麻烦！所以我们希望减轻数据压力，我们可以使用缓存来保证效率。

（并不是一出来就有缓存）

发展过程：优化Mysql底层的数据结构和索引-->文件缓存（IO操作，效率低）-->Memcached(当时最热门的技术！)

![image-20220530230757765](C:\Users\啊坤\AppData\Roaming\Typora\typora-user-images\image-20220530230757765.png)



> 3.分库分表+水平拆分+MYSQL集群

技术和业务在发展的同时，对人的要求也越来越高！

本质：数据库（读+写）

早些年MyISAM：表锁，十分影响效率！高并发下就会出现严重的锁问题

转战InnoDB:行锁

慢慢的就开始使用分库分表来解决写的压力！MYSQL在那个年代推出了表分区！这个并没有多少公司使用！

MYSQL的表集群，很好的满足的那个年代的所有需求！

![image-20220530231449336](C:\Users\啊坤\AppData\Roaming\Typora\typora-user-images\image-20220530231449336.png)

（上图是将写数据分表，比如一个集群写三分之一）

> 4.如今最近的年代

2010--2020十年之间，世界已经发生了翻天覆地的变化；（定位，也是一种数据，音乐，热榜！）

MYSQL等关系型数据库就不够用了！数据量很多，变化很快~！

MYSQL有的人使用它来存储一些比较大的文件，博客，图片！数据库表很大，效率就很低了！如果有一种数据库专门来处理这种数据，我们MySQL压力就变得十分小（研究如何胡里这些问题！）大数据的IO压力下，表几乎没法更改！



> 目前一个基本的互联网项目

![image-20220530232857125](C:\Users\啊坤\AppData\Roaming\Typora\typora-user-images\image-20220530232857125.png)

> 为什么要用NoSQL !

用户的个人信息，社交网络，地理位置，用户自己产生的数据，用户日志等等爆发式增长！

这时候我们就需要使用NoSQL数据库了，Nosql可以很好的处理以上的情况！





## 什么是NoSQL

NoSQL = Not Only SQL（不仅仅是SQL）

关系型数据库：表格，行，列

泛指非关系型数据库，随着web2.0互联网的诞生！传统的关系型数据库很难对付web2.0时代！尤其是超大规模的高并发的社区！当数据爆发性增长，关系型数据库就暴露楚很多难以克服的问题，NoSQL在当今大数据环境下发展十分迅速，Redis是发展最快的，而且是当下必须要掌握的技术。

很多的数据类型，比如：用户的个人信息，社交网络，地理位置，这些数据类型的存储不需要一个固定的格式（拓扑图）！不需要多余的操作就可以横向扩展的！Map<String,Object>使用键值对来控制！





> NoSQL特点

为什么java要面向接口编程----->解耦！

1. 方便扩展（数据之间没有关系，很好扩展）
2. 大数据量高性能（Redis  一秒写八万次，读取11万次，NoSQL的缓存记录级，是一种细粒度的缓存，性能比较高！）
3. 数据类型是多样型的！（不需要事先设计数据库！随取随用！如果是数据量十分大的表，很多人就无法设计了）
4. 传统RDBMS和NoSQL

```sql
传统的RDBMS
- 结构化组织
- SQL 
- 数据和关系都存在单独的表中 row  clo
- 操作，数据定义语言
- 严格的一致性
- 基础的事务
- 。。。。。
```

```sql
NoSQL
- 不仅仅是数据
- 没有固定的查询语言
- 键值对存储，列存储，图形数据库（社交关系）
- 最终一致性
- CAP定义 和 BASE  （异地多活）
- 高性能，高可用，高可扩展性
- 。。。。。。
```



> 了解：3V+3高

大数据时代的3V：主要是描述问题的

1. 海量Volume
2. 多样Variety
3. 实时Velocity

大数据时代的3高：主要是对程序的要求

1. 高并发
2. 高可扩（随时水平拆分，机器不够了，可以扩展机器来解决【最基本的一种】）
3. 高性能（保证用户的体验）



真正在公司中的使用：NoSQL+RDBMS一起使用才是最强的！



## 阿里巴巴架构演进分析

![image-20220531085142757](C:\Users\啊坤\AppData\Roaming\Typora\typora-user-images\image-20220531085142757.png)

如果你未来想当一个架构师：没有什么是加一层解决不了的！

```bash
参考淘宝网页
# 1、商品的基本信息
	名臣给、价格、商家信息
	关系型数据库就可以解决了！ MYSQL / Oracle（淘宝早年就去IOE了！ 王坚 推荐文章：阿里云的这群疯子）
	淘宝内部的MySQL不是大家用的MySQL
	
# 2、商品描述、评论（文字比较多）
	文档型数据库中，MongoDB
# 3、图片
	分布式文件系统  FastDFS
	- 淘宝自己的TFS
	- Gooale的 GFS
	- Hadoop  HDFS
	- 阿里云的  oss
	
# 4、商品的关键字（搜索）
	- 搜索引擎  solr elasticsearch
	- ISerach ： 多隆（多去了解以下这些技术大佬！）
	所有牛逼的人，都有一段苦逼的岁月！但是只要像SB一样去坚持，终将牛逼！
	
# 5、商品热门的波段信息
	- 内存数据库
	- Redis Tair、Memache....
	
# 6、商品的交易，外部的支付接口
	- 三方应用
```

要知道，一个简单的网页背后的技术一定不是大家所想的那么简单！

大型互联网应用问题：

- 数据类型太多了！
- 数据源繁多，经常重构
- 数据要改造，大面积改造？

解决问题：

![image-20220531090418186](C:\Users\啊坤\AppData\Roaming\Typora\typora-user-images\image-20220531090418186.png)



<img src="C:\Users\啊坤\AppData\Roaming\Typora\typora-user-images\image-20220531090617604.png" alt="image-20220531090617604" style="zoom:80%;" />

![image-20220531090711966](C:\Users\啊坤\AppData\Roaming\Typora\typora-user-images\image-20220531090711966.png)

这以上都是NoSQL的入门概述！



## NoSQL的四大分类

**KV键值对：**

- 新浪：Redis
- 美团：Redis+Tair
- 阿里、百度：Redis+memecache

**文档型数据库（bson格式和json一样）：**

- MongoDB（一般必须要掌握）
  - MongoDB是一个基于分布式文件存储的数据库，C++编写，主要用来处理大量的文档
  - MongoDB是一个介于关系型数据库和非关系型数据库的中间产品！MongoDB是非关系型数据库中功能最丰富，最像关系型数据库的！
- ConthDB

**列存储数据库**

- HBase
- 分布式文件系统

**图形关系数据库**

![myfriends](https://greatpowerlaw.files.wordpress.com/2012/10/myfriends_thumb.jpg?w=655&h=525)

- 他不是存图片的，放的是关系，比如：朋友圈社交网络，广告推荐！
- Neo4j， InfoGrid
- 

> 四者对比

![image-20220531092604955](C:\Users\啊坤\AppData\Roaming\Typora\typora-user-images\image-20220531092604955.png)





# Redis入门

## 概述

> Redis是什么

Redis（Remote Dictionary Server），即远程字典服务！

是一个开源的使用ANSI c语言编写、支持网络、可基于内存亦可持久化的日志型，Key-Value数据库，并提供多种语言的API。

Redis会周期性的把更新的数据ieru磁盘或者把修改操作写入追加的记录文件，并且在此基础上实现了master-slave（主从）同步。

免费和开源！是当下最热门的NoSql技术之一！也被人们称之为结构化的数据库！

> Redis能干什么？

1. 内存存储、持久化，内存中是断电即失的、所以说是持久化很重要（rdb，aof）
2. 效率高，可以用于高速缓存
3. 发布订阅系统
4. 地图信息分析
5. 计时器、计算器（浏览量！）
6. 、、、、

> 特性

1. 多样性的数据类型
2. 持久化
3. 集群
4. 事务

......

> 学习中需要用到的东西

1. 狂神说公共号
2. 官网 https://redis.io/
3. 中文网 ：https://www.redis.net.cn/
4. 下载地址：https://redis.io/download/

注意：Window在Github上下载（停更很久了！）

==Redis推荐都是在Linux服务器上搭建的，我们是基于Linux学习！



## 测试性能

redis-Benchmark是一个压力测试工具！

官方自带的性能测试工具！

redis-benchmark 命令参数！



![image-20220601161458315](C:\Users\啊坤\AppData\Roaming\Typora\typora-user-images\image-20220601161458315.png)

我们来简单测试一下：

```bash
# 测试：100个并发连接  100000请求
redis-benchmark -h localhost -p 6379 -c 100 -n 100000
```

![image-20220601163027146](C:\Users\啊坤\AppData\Roaming\Typora\typora-user-images\image-20220601163027146.png)



## 基础知识

redis默认有16个数据库

![image-20220601163911661](C:\Users\啊坤\AppData\Roaming\Typora\typora-user-images\image-20220601163911661.png)

默认使用的是第0个

可以使用select进行切换数据库！

```bash
127.0.0.1:6379[3]> select 3   #切换数据库
OK
127.0.0.1:6379[3]> dbsize    #查看数据库大小
(integer) 0

```

1. 查看数据库所有的key

```bash
127.0.0.1:6379[3]> keys *
1) "name"
```

2. 清除当前数据库**flushdb**
3. 清除所有的数据库 flushall

```bash
127.0.0.1:6379[3]> get name
"akun"
127.0.0.1:6379[3]> flushdb
OK
127.0.0.1:6379[3]> keys *
(empty array)


```



> Redis是单线程的！

明白Redis是很快的，官方表示，Redis是基于内存操作的，CPU不是Redis性能瓶颈，Redis的瓶颈是根据机器的内存和网络带宽秒既然可以使用单线程来实现，就使用了单线程。

Redis是C语言写的，官方提供的数据为100000+的QPS，完全不必同样是使用key-value的Memecache差！

**Redis为什么单线程还这么快？**

1. 误区1：高性能的服务器一定是多线程的？
2. 误区2：多线程（CPU上下文切换！）一定比单线程效率高！

先对CPU>内存>硬盘的速度有所了解！

核心：redis是将所有的数据全部放在内存中的，所以说使用单线程去操作效率就是最高的，多线程（CPU上下文切换：耗时的操作！！！），对于内存系统来说，如果没有上下文切换效率就是最高的！多次读写就是在一个CPU上的，在内存情况下，这个就是最佳方案！



# 五大数据类型

> 什么是五大数据类型？

Redis是一个开源（BSD许可），内存存储的数据结构服务器，可用作**数据库**，高速缓存和**消息中间件MQ**。它支持[字符串](https://www.redis.net.cn/tutorial/3508.html)（String）、[哈希表](https://www.redis.net.cn/tutorial/3509.html)（Hash）、[列表](https://www.redis.net.cn/tutorial/3510.html)（list）、[集合](https://www.redis.net.cn/tutorial/3511.html)（set）、[有序集合](https://www.redis.net.cn/tutorial/3512.html)（zset），[位图](https://www.redis.net.cn/tutorial/3508.html)geospatial），[hyperloglogs](https://www.redis.net.cn/tutorial/3513.html)等数据类型。内置复制（replication）、[Lua脚本](https://www.redis.net.cn/tutorial/3516.html)、LRU收回、[事务](https://www.redis.net.cn/tutorial/3515.html)以及不同级别磁盘持久化功能，同时通过Redis Sentinel提供高可用，通过Redis哨兵（Sentinel）和自动分区（Cluster）提供搞可用性（high availability）

> 以下讲解的所有命令一定要全部记住，后面使用SpringBoot。Jedis，所有的方法就是这些命令。

## Redis-Key

```bash
127.0.0.1:6379> keys *  # 查看所有的key
(empty array)
127.0.0.1:6379> set name akun  #设置key
OK
127.0.0.1:6379> exists name		# 判断当前key是否存在
(integer) 1
127.0.0.1:6379> move name 1		#移除当前的key
(integer) 1
127.0.0.1:6379> keys *
(empty array)
127.0.0.1:6379> set name akun
OK
127.0.0.1:6379> expire name 10   # 设置key的国企事件
(integer) 1
127.0.0.1:6379> ttl name   # 查看当前key的国企事件
(integer) 5
127.0.0.1:6379> type name	# 查看当前key的类型
string

```

后面如果遇到不会的命令，可以在官网查看

![image-20220601171553708](C:\Users\啊坤\AppData\Roaming\Typora\typora-user-images\image-20220601171553708.png)



## String（字符串类型）

90%的java程序员使用redis只会使用一个String类型！

```bash
127.0.0.1:6379> set key1 v1  #设置值
OK
127.0.0.1:6379> get key1   # 获得值
"v1"
127.0.0.1:6379> keys *  # 获得所有的值
1) "key1"
127.0.0.1:6379> exists key1  #判断某一个key是否岑在
(integer) 1  
127.0.0.1:6379> append key1 "hello"   #在指定的key上追加字符串，如果当前key不存在，就相当于set key
(integer) 7
127.0.0.1:6379> get key1
"v1hello"
127.0.0.1:6379> strlen key1   # 获取字符串的长度！
(integer) 7

###########################################################################################
127.0.0.1:6379> set views 0
OK
127.0.0.1:6379> get views
"0"
127.0.0.1:6379> incr views  #自增 1
(integer) 1

127.0.0.1:6379> incr views
(integer) 2
127.0.0.1:6379> get views
"2"
127.0.0.1:6379> decr views  # 自减 1
(integer) 1

127.0.0.1:6379> incrby views 10  # 可以设置步长，指定增量！
(integer) 11
127.0.0.1:6379> decrby views 5
(integer) 6

#############################################################################3##
# 字符串范围：range
127.0.0.1:6379> set key1 "hello,akun"  # 设置key1的值
OK
127.0.0.1:6379> getrange key1 0 3  # 截取字符串[0,3],包含3
"hell"
127.0.0.1:6379> getrange key1 0 -1   #获取全部的字符串和get key是一样得
"hello,akun"


# 替换
127.0.0.1:6379> set key2 abcdefg
OK
127.0.0.1:6379> get key2
"abcdefg"
127.0.0.1:6379> setrange key2 1 xx   # 替换指定位置开始得字符串！
(integer) 7
127.0.0.1:6379> get key2
"axxdefg"

####################################################################################
# setex(set with expire) #设置过期时间
# setnx(set if not exists) #如果不存在再设置(在分布式锁中会常常使用)

127.0.0.1:6379> setex key3 30 "hello"   #设置key3的值为“hello”，30秒后过去
OK
127.0.0.1:6379> ttl key3
(integer) 26
127.0.0.1:6379> get key3
"hello"
127.0.0.1:6379> setnx mykey "redis"	 #如果mykey不存在，创建mykey
(integer) 1
127.0.0.1:6379> keys *
1) "key2"
2) "mykey"
127.0.0.1:6379> ttl key3
(integer) -2
127.0.0.1:6379> setnx mykey "MongoDB"    # mykey存在，创建失败，返回0
(integer) 0
127.0.0.1:6379> get mykey
"redis"
127.0.0.1:6379> 
#########################################################################################
mset
mget
 
127.0.0.1:6379> mset k1 v1 k2 v2 k3 v4  #同时设置多个值
OK
127.0.0.1:6379> keys *
1) "key2"
2) "k3"
3) "k2"
4) "k1"
5) "mykey"
127.0.0.1:6379> mget k1 k2 k3   # 同时获取多个值
1) "v1"
2) "v2"
3) "v4"
127.0.0.1:6379> msetnx k1 v1 k4 v4   #msetnx 是一个原子性操作，要么一起成功，要么一起失败！
(integer) 0

127.0.0.1:6379> get k4
(nil)
127.0.0.1:6379> 

#对象
set user:1 {name:zhagsan,age:3} #设置user:1对象，值用json字符串来保存一个对象

#这里的key是一个巧妙的涉及：user：{id}：{filed}，如此涉及在Redis中式完全OK的！
127.0.0.1:6379> mset user:1:name zhagnsan user:1:age 2
OK
127.0.0.1:6379> mget user:1:name user:1:age
1) "zhagnsan"
2) "2"

#########################################################################################33
getset # 先get然后再set

[root@akun ~]# getset db redis  #如果不存在值，则返回nil
-bash: getset: command not found
[root@akun ~]# redis-cli
127.0.0.1:6379> getset db redis   
(nil)
127.0.0.1:6379> get db
"redis"
127.0.0.1:6379> getset db mongodb如果存在值，获取原来的值，并设置新的值
"redis"
127.0.0.1:6379> get db
"mongodb"
127.0.0.1:6379> 

```

数据结构式相同的!

String类型的使用场景：value处理时我们的字符串还可以是我们的数字！

- 计数器
- 统计多单位的数量
- 粉丝数
- 对象缓存存储



## List

基本的数据类型，列表

在redis里面，我们可以把list玩成，栈、队列、阻塞队列！

所有的list命令都是以 l 开头的！ redis不区分大小写命令

```bash
#############################################################################################
127.0.0.1:6379> lpush list one  #将一个或多个值，插入到列表的头部（左）
(integer) 1
127.0.0.1:6379> lpush list two
(integer) 2
127.0.0.1:6379> lpush list three
(integer) 3
127.0.0.1:6379> lrange list 0 -1   #获取list中的值
1) "three"
2) "two"
3) "one"
127.0.0.1:6379> lrange list 0 1  # 通过区间获取具体的值！
1) "three"
2) "two"
127.0.0.1:6379> rpush list righr   # 将一个或者多个值，插入到列表尾部（有）
(integer) 4
127.0.0.1:6379> lrange list 0 -1
1) "three"
2) "two"
3) "one"
4) "righr"

#############################################################################################
lpop  # 从左边移除
rpop #从右边移除

127.0.0.1:6379> lrange list 0 -1
1) "three"
2) "two"
3) "one"
4) "righr"
127.0.0.1:6379> lpop list    # 移除list的第一个元素
"three"
127.0.0.1:6379> rpop list    #移除list最后一个元素
"righr"
127.0.0.1:6379> lrange list 0 -1
1) "two"
2) "one"
127.0.0.1:6379> 
#############################################################################################
Lrem ： 移除指定的值！

127.0.0.1:6379> lrem list 1 one   #移除list集合中指定个数的value，因为list里面的内容可以重复，所以要添加个数
(integer) 1


#############################################################################################
Lindex 键值 下标    # 通过下表获取list中的某个值

127.0.0.1:6379> lrange list 0 -1
1) "two"
2) "one"
127.0.0.1:6379> lindex list 1   # 通过下表获取list中的某个值
"one"

#############################################################################################
Llen   #查看list的长度

127.0.0.1:6379> llen list
(integer) 2

#############################################################################################
trim 修建 ：list 截断


127.0.0.1:6379> Rpush mulist "hello"
(integer) 1
127.0.0.1:6379> Rpush mulist "hello1"
(integer) 2
127.0.0.1:6379> Rpush mulist "hello2"
(integer) 3
127.0.0.1:6379> Rpush mulist "hell03"
(integer) 4
127.0.0.1:6379> ltrim mulist 1 2   # 通过下表截取指定的长度，这个list已经被改变了，截断了只剩下截取的 元素
OK
127.0.0.1:6379> lrange mulist 0 -1
1) "hello1"
2) "hello2"

#############################################################################################
rpoplpush #移除列表的最后一个元素，将它移动到新的列表中

127.0.0.1:6379> rpush mylist "hello" "hello1" "hello2"
(integer) 3
127.0.0.1:6379> lrange mylist 0 -1
1) "hello"
2) "hello1"
3) "hello2"
127.0.0.1:6379> rpoplpush mylist myotherlist  #移除列表的最后一个元素，将他移动到新的列表中！
"hello2"
127.0.0.1:6379> lrange mylist 0 -1  #查看原来的列表
1) "hello"
2) "hello1" 
127.0.0.1:6379> lrange myotherlist 0 -1  #查看目标列表，确实存在该值！
1) "hello2"
127.0.0.1:6379> 

#############################################################################################
lset   将列表中的指定位置的值进行更新，如果没有该列表或者指定位置没有值，则不能执行成功（相当于更新操作），首先要判断这个列表是否存在

#############################################################################################
Linsert 往指定列表中的指定的值前面或者后面插入值
```

> 小结

- 它实际上是一个链表， before Node after， left，right都可以插入值
- 如果key不存在，创建新的链表
- 如果key存在，新增内容
- 如果移除了所有的值，空链表，也代表不存在！
- 在两边插入或者改动值，效率最高！中间元素，相对来说效率会低一点。



## Set（集合）

set中的值，是不能重复的！

```bash
127.0.0.1:6379> sadd myset "hello"  #set集合中添加元素，添加重复值就会失败！
(integer) 1
127.0.0.1:6379> sadd myset "hello1"
(integer) 1
127.0.0.1:6379> sadd myset "hello2"
(integer) 1
127.0.0.1:6379> smembers myset   # 查看set集合
1) "hello1"
2) "hello2"
3) "hello"
127.0.0.1:6379> sismember myset  hello3  #判断指定值是不是在set中
(integer) 0
127.0.0.1:6379> 

127.0.0.1:6379> scard myset  #获取set中的元素个数
(integer) 3


#############################################################################################
rem  # 移除元素 	

127.0.0.1:6379> srem myset hello  #移除set中的指定元素
(integer) 1

#############################################################################################
set 无序不重复集合，抽随机

[root@akun ~]# redis-cli
127.0.0.1:6379> srandmember myset   #随机抽选素一个元素
"hello1"
 
127.0.0.1:6379> srandmember myset 2  #随机抽选指定个数的元素
1) "hello1"
2) "hello2"

#############################################################################################
删除指定的key， 随机删除key
127.0.0.1:6379> smembers myset
1) "hello1"
2) "hello"
3) "hello2"
127.0.0.1:6379> spop myset  随机删除一个set集合中的元素，【可以指定个数】
"hello"
127.0.0.1:6379> smembers myset
1) "hello1"
2) "hello2"

#############################################################################################
smove 将一个指定的值，移动到另外一个set集合

#############################################################################################
微博，B栈，共同关注！（并集）
数字集合类：
 - 差集  sdiff key1 key2
 - 交集  sinter key1 key2   #交集，共同好友就可以这样实现
 - 并集  sunion key1 key2
```

微博，A用户将所有的关注的人放在一个集合中！将它的粉丝也放在一个集合中！

共同关注，共同爱好，二度好友，推荐好友（六度分隔理论）





## Hash（哈希）

可以想象为Map集合

Map集合，key-map！这时候这个值是一个map集合。【我们平常的map是key-value，这里value只不过是一个map】

```bash
127.0.0.1:6379> hset myhash field1 akun filed2 zhangsan  #set key value
(integer) 2

####################################
hdel  #删除hash指定的字段
hlen  #获取hash的长度
hkeys  key  #获取所有的field
hvals  key  #获取所有的值
hincby  key field 步长
hdecby  key field 步长 
hsetnx  key  field value   #如果不存在则可以设置

其他方法基本和上上面的类型基本一样！
```

hash的应用：
hash变更的数据user name、age，尤其是用户信息之类的，经常变动的信息！hash更加适合对象的存储，而String更加适合字符串的存储！



Zset（有序集合）

在set的基础上，增加了一个值

```bash
127.0.0.1:6379> zadd myset 1 one
(integer) 1
127.0.0.1:6379> zadd myset 2 two 3 three
(integer) 2
127.0.0.1:6379> zrange myset 0 -1
1) "one"
2) "two"
3) "three"
#########################################################################
排序
127.0.0.1:6379> zrange myset 0 -1
1) "one"
2) "two"
3) "three"

127.0.0.1:6379> zrangebyscore myset -inf +inf
1) "one"
2) "two"
3) "three"

127.0.0.1:6379> zrangebyscore myset -inf 2   #可以指定到哪排序
1) "one"
2) "two"



127.0.0.1:6379> zrangebyscore myset -inf +inf withscores   #显示全部的用户并且附带成绩
1) "one"
2) "1"
3) "two"
4) "2"
5) "three"
6) "3"
127.0.0.1:6379> 

zrevrange  #从大到小进行排序

zcount key 1 3  #获取指定区间的成员数量

```

案例思路：zset比set多一个排序，存储班级成绩表，工资表排序！

普通消息 1，，重要消息 2， 带权重进行判断！

排行版应用实现，topN测试！



# 三种特殊数据类型

## geospatial地理位置

朋友的定位，附近的人，打车距离计算？

Redis的Geo在Redis3.2版本就推出了！这个功能可以推算初地址位置的信息，两地之间的距离，方圆几里的人！



![image-20220601210708451](C:\Users\啊坤\AppData\Roaming\Typora\typora-user-images\image-20220601210708451.png)

> geoadd

```BASH
#geoadd 添加地址位置
#规则：两极无法直接添加，我们一般会下载城市数据，直接通过java程序一次性导入！

#有效的经度从-180度到180度。
#有效的纬度从-85.05112878度到85.05112878度。
#当坐标位置超出上述指定范围时，该命令将会返回一个错误。
#(error) ERR invalid longitude,latitude pair 39.900000,116.400000


127.0.0.1:6379> geoadd china:key 39.90 116.40  beijing
(integer) 1
127.0.0.1:6379> geoadd china:key 121.47 31.23 shagnhai
(integer) 1
127.0.0.1:6379> geoadd china:key 106.50 29.53 chongqing 114.05 22.52 shenzheng
(integer) 2
127.0.0.1:6379> geoadd china:key 120.16 30.24 hangzhou 108.96 34.26 xian
(integer) 2
127.0.0.1:6379> 

```

> ​	geopos

获得当前定位：一定是一个坐标值！

```bash
127.0.0.1:6379> geopos china:key beijing  #获取指定的城市的精度和唯度！
1) 1) "116.39999896287918091"
   2) "39.90000009167092543"
127.0.0.1:6379> geopos china:key chongqing
1) 1) "106.49999767541885376"
   2) "29.52999957900659211"
127.0.0.1:6379> 

```

> GEODIST 显示两个地方的直线距离

指定单位的参数 unit 必须是以下单位的其中一个：

- **m** 表示单位为米。
- **km** 表示单位为千米。
- **mi** 表示单位为英里。
- **ft** 表示单位为英尺。

如果用户没有显式地指定单位参数， 那么 `GEODIST` 默认使用米作为单位。

`GEODIST` 命令在计算距离时会假设地球为完美的球形， 在极限情况下， 这一假设最大会造成 0.5% 的误差。

```bash
127.0.0.1:6379> GEODIST china:key beijing shagnhai
"1067378.7564"
127.0.0.1:6379> GEODIST china:key beijing shagnhai km
"1067.3788"
```



> georadius 以给定的经纬度为中心，找出某一半径内的元素

附近的人？（获得所有附近的人的地址，定位！通过半径来查询!）

所有数据应该都录入：china:city，才会让结果更加清晰！

```bash
127.0.0.1:6379> georadius china:key 110 30 1000 km
1) "chongqing"
2) "xian"
3) "shenzheng"
4) "hangzhou"

还可以加相应的参数，返回经纬度和指定显示数量
```



> georandiusbymember 

```bash
#找出位于指定元素周围的其他元素
127.0.0.1:6379> georadiusbymember china:key beijing 1000 km
1) "beijing"
2) "xian"

```



> GEOHASH     返回一个或多个位置的元素

该命令将返回11个字符的GeoHash字符串

```bash
# 将二维的经纬度转换未一位的字符串，如果两个字符串越接近，那么则距离就越近！
127.0.0.1:6379> geohash china:key beijing chongqing
1) "wx4fbxxfke0"
2) "wm5xzrybty0"

```



> GEO底层的实现原理起始就是Zset！我们可以使用Zset命令来操作geo！



```bash
127.0.0.1:6379> zrange china:key 0 -1  #查看地图中所有的元素
1) "chongqing"
2) "xian"
3) "shenzheng"
4) "hangzhou"
5) "shagnhai"
6) "beijing"

```



## Hyperloglog

> 什么是基数

A{1，3，5，7，8，9，7}

B{1，3，5，7，8}

基数（不重复的元素）=5，可以接收误差！

> 简介

Redis 2.8.9版本更新了Hyperloglog数据结构

Redis Hyperloglog基数统计算法！

优点：占用的内存是固定的，2……64不同的元素的技术，只需要费12kb内存！如果从内存角度来比较的话Hyperloglog首选！

网页的UV（一个人访问一个网站多次，但是还算作一个人！）

传统的方式，set保存用户的id，然后就可以统计set中的元素作为标准判断！

这个方式如果保存大量的id，就会比较麻烦！我们的目的是为了技术，而不是保存用户id;

0.85%错误率！统计UV任务，可以忽略不计的！

> 测试使用

```bash
127.0.0.1:6379> pfadd mykey a b c d e f g h i  #创建第一组元素  mykey
(integer) 1 
127.0.0.1:6379> pfcount mykey   # 统计 mykey元素的基数数量
(integer) 9
127.0.0.1:6379> pfadd mykey1 i j z x c v b n m  
(integer) 1
127.0.0.1:6379> pfcount mykey1
(integer) 9
127.0.0.1:6379> pfmerge mykey3 mykey mykey1  #合并两组  mykey mykey1=》mykey3并集
OK
127.0.0.1:6379> pfcount mykey3
(integer) 15
127.0.0.1:6379> 

```

如果允许容错，那么一定可以使用hyperloglog！

如果允许容错，就使用set或者自己的数据类型即可！



## BitMaps

统计用户信息，活跃，不活跃！ 登录，未登录！两个状态的，都可以使用Bitmaps！

Bitmaps位图，数据结构！都是操作二进制来进行记录，就只有0和1两个状态！

使用bitmap来记录周一到周日的打卡！

周一：1 周二 ：0 周三 ：0 周四：1......

![image-20220601230403145](C:\Users\啊坤\AppData\Roaming\Typora\typora-user-images\image-20220601230403145.png)

查看某一天是否有打卡！

```bash
127.0.0.1:6379> getbit sign 4
(integer) 1
```

统计操作，统计打卡天数！

```bash
127.0.0.1:6379> bitcount sign #统计这周的打卡记录，是否全勤
(integer) 3

```





# 事务

原子性：要么同时成功，要么同时失败。

**Redis单条命令是保证原子性的，但是事务不保证原子性。**



Redis事务本质：一组命令的集合！一个事务中的所有命令都会被序列化，在事务执行的过程中，会按照顺序执行！

一次性，顺序性，排他性！执行一系列的命令

```bash
----------------队列  set set set  执行------
```

**Redis事务没有隔离级别的概念！**

所有的命令在事务中，并没有直接被执行！只有发起执行命令的时候才会执行！Exex



Redis的事务：

- 开启事务（multi）
- 命令入队（）
- 执行事务（exec）

> 正常执行事务！

```bash
127.0.0.1:6379> multi    #开启事务
OK
127.0.0.1:6379(TX)> set k1 v1    # 事务入队
QUEUED
127.0.0.1:6379(TX)> set k2 v2
QUEUED
127.0.0.1:6379(TX)> get k2
QUEUED
127.0.0.1:6379(TX)> set k3 v3
QUEUED
127.0.0.1:6379(TX)> exec    # 执行事务
1) OK
2) OK
3) "v2"
4) OK
```



> 放弃事务

```bash
127.0.0.1:6379> multi    #开启事务
OK
127.0.0.1:6379(TX)> set k1 v1
QUEUED
127.0.0.1:6379(TX)> set k2 v2
QUEUED
127.0.0.1:6379(TX)> discard    #取消事务

```

> 编译型异常（代码有问题！命令有错！），事务中所有的命令都不被执行！

```bash
127.0.0.1:6379> multi
OK
127.0.0.1:6379(TX)> set k1 v1
QUEUED
127.0.0.1:6379(TX)> set k2 v2
QUEUED
127.0.0.1:6379(TX)> getset k2     # 错误的命令
(error) ERR wrong number of arguments for 'getset' command
127.0.0.1:6379(TX)> set k3 v3
QUEUED
127.0.0.1:6379(TX)> exec     #执行命令时报错
(error) EXECABORT Transaction discarded because of previous errors.
127.0.0.1:6379> get k2		#所有命令都不会执行
(nil)


```

> 运行时异常（1/0）如果队列中存在语法性异常，那么执行命令的时候，其他命令时可以正常执行的，错误命令抛出异常。

```bash
127.0.0.1:6379> set k1 "v1"
OK
127.0.0.1:6379> multi
OK
127.0.0.1:6379(TX)> incr k1  #会执行的时候失败！
QUEUED
127.0.0.1:6379(TX)> set k2 v2
QUEUED
127.0.0.1:6379(TX)> set k3 v3
QUEUED
127.0.0.1:6379(TX)> get k3
QUEUED
127.0.0.1:6379(TX)> exec
1) (error) ERR value is not an integer or out of range   # 虽然第一条命令报错了，但是依旧正常执行成功了！
2) OK
3) OK
4) "v3"

```



# 监控 Watch（面试常问）

**悲观锁：**

- 很悲观，认为什么时候都会出问题，无论做什么都会加锁

**乐观锁:**

- 很乐观，认为什么时候都不会出现问题，所以不会上锁！更新数据的时候去判断以下，再此期间是否有人修改过这个数据

- 获取version
- 更更新的时候比较version



> Redis的监视测试

正常执行成功！

```bash
127.0.0.1:6379> set money 100
OK
127.0.0.1:6379> set out 0
OK
127.0.0.1:6379> watch money    #监视money对象
OK
127.0.0.1:6379> multi    #事务正常结束，数据期间没有发生变动，这个时候就正常执行成功！
OK
127.0.0.1:6379(TX)> decrby money 20
QUEUED
127.0.0.1:6379(TX)> incrby out 20
QUEUED
127.0.0.1:6379(TX)> exec
1) (integer) 80
2) (integer) 20
```

测试多线程修改值，使用watch可以当作redis的乐观锁操作！

```bash
127.0.0.1:6379> watch money   #监视 money
OK
127.0.0.1:6379> multi
OK
127.0.0.1:6379(TX)> decrby money 10
QUEUED
127.0.0.1:6379(TX)> incrby out 10
QUEUED
127.0.0.1:6379(TX)> exec		# 执行之前，另外一个线程，修改了我们的值，这个时候，就会导致事物执行失败！
(error) EXECABORT Transaction discarded because of previous errors.
127.0.0.1:6379> 

```

如果修改失败，获取最新的值就好！

![image-20220602130455533](C:\Users\啊坤\AppData\Roaming\Typora\typora-user-images\image-20220602130455533.png)





# Jedis

我们要使用java来操作Redis

> 什么是Jedis是Redis官方对舰的java连接开发工具！使用java操作Redis中间件！如果你要使用java操作redis，那么仪狄格要对redis十分熟悉！

**用Jedis连接远程redis修改配置文件**

- 将bind 127.0.0.1注释
- 设置protected-mode no
- 打开 requirepass 的注释，在其后面配置密码 requirepass password

> 我已经将redis的配置文件复制了一份在/usr/local/bin/kconfige目录下，可以修改复制后的配置文件
>
> 在启动时要指定相应的配置文件  redis-server /usr/local/bin/kconfige/redis.conf
>
> 当从宝塔面板启动的时候，是从默认配置文件启动的。





> 测试

1. 导入相应的依赖

```java
<!--导入jedis以来-->
 <dependencies>
        <!-- https://mvnrepository.com/artifact/redis.clients/jedis -->
        <dependency>
            <groupId>redis.clients</groupId>
            <artifactId>jedis</artifactId>
            <version>4.1.1</version>
        </dependency>

        <!-- https://mvnrepository.com/artifact/com.alibaba/fastjson -->
        <dependency>
            <groupId>com.alibaba</groupId>
            <artifactId>fastjson</artifactId>
            <version>2.0.5</version>
        </dependency>

    </dependencies>
    
```

2. 编码测试：

- 连接数据库

- 操作命令
- 断开连接

```java
 public static void main(String[] args){
        //    1、 new Jedis
        Jedis jedis = new Jedis("8.130.100.99",6379);
        // jedis 所有的命令就是我们之前学习的所有指令!所以之前的指令学习很重要
        System.out.println(jedis.ping());
    }
```

![image-20220602161835091](C:\Users\啊坤\AppData\Roaming\Typora\typora-user-images\image-20220602161835091.png)



## 常用的API

```java
//    1、 new Jedis
        Jedis jedis = new Jedis("8.130.100.99",6379);

        System.out.println("清空数据:"+jedis.flushDB());
        System.out.println("判断某个键是否存在:"+jedis.exists("username"));
        System.out.println("新增<'username','akun'>的键值对:"+jedis.set("username","akun"));
        System.out.println("新增<'password','password'>的键值对："+jedis.set("password","password"));
        System.out.println("系统中所有的键如下");
        Set<String> keys = jedis.keys("*");
        System.out.println(keys);

        System.out.println("删除键password："+jedis.del("password"));
        System.out.println("判断键password是否存在:"+jedis.exists("password"));
        System.out.println("查看键username所有存储值的类型:"+jedis.type("username"));
        System.out.println("随机返回一个key空间的一个:"+jedis.randomKey());
        System.out.println("重命名key:"+jedis.rename("username","name"));
        System.out.println("取出该后的name："+jedis.get("name"));
        System.out.println("按索引查询："+jedis.select(0));
        System.out.println("删除当前选择数据库中的所有key："+jedis.flushDB());
        System.out.println("返回当前数据库中key的数据:"+jedis.dbSize());
        System.out.println("删除所有数据库中的key:"+jedis.flushAll());
```



# SpringBoot整合

SpringBoot操作数据：Spring-data jpa jdbc mongodb redis！

SpringData也是和SpringBoot齐名的项目

说明，在SpringBoot2.x之后，原来使用的jedis被替换承了lettuce？

jedis：采用的是直连，多线程操作的话，是不安全的，如果想要避免不安全的，使用jedis pool连接池！更像BIO模式

lettuce：采用netty，实例可以在多个线程中进行共享，不存在线程不安全的情况！可以减少线程数量了。更像NIO模式

源码分析：

```java
@Bean
@ConditionalOnMissingBean(name = "redisTemplate")  //当这个bean不存在的时候，才生效，可以自己写一个替换这个默认的！
public RedisTemplate<Object, Object> redisTemplate(RedisConnectionFactory redisConnectionFactory)
      throws UnknownHostException {
    //默认的RedisTemplate没有过多的设置，redis对象的保存时需要序列化的！
    //两个泛型都是Object类型，后面使用需要强势转换<String, Object>
   RedisTemplate<Object, Object> template = new RedisTemplate<>();
   template.setConnectionFactory(redisConnectionFactory);
   return template;
}

@Bean
@ConditionalOnMissingBean  //由于String类型是redis中最常用的类型，所以单独提出来了一个bean！
public StringRedisTemplate stringRedisTemplate(RedisConnectionFactory redisConnectionFactory)
      throws UnknownHostException {
   StringRedisTemplate template = new StringRedisTemplate();
   template.setConnectionFactory(redisConnectionFactory);
   return template;
}
```





> 整合测试一下

1. 导入依赖

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-redis</artifactId>
</dependency>
```

2. 配置连接

```properties
#配置redis
spring.redis.host=127.0.0.1
spring.redis.port=6379
```

3. 测试

   ```java
   @SpringBootTest
   class Redis02SpringbootApplicationTests {
   
       @Autowired
        private RedisTemplate redisTemplate;
   
       
   
       @Test
       void contextLoads() {
           //在企业开发中，我们80%的情况下，都不会使用这个原生的方式去编写代码！
           
           //redisTemplate
           // opsForValue 操作字符串  类似String
           //opsForList  操作list 类型List
           //opsForHash
           //.....
           
           //除了基本的操作，物品们常用的方法都可以直接通过redisTemplate操作，比如事务，基本的CRUD
           
           //获取对象的连接对象
           //RedisConnection connection = redisTemplate.getConnectionFactory().getConnection();
           //connection.flushAll();
           //connection.flushDb();
           
           redisTemplate.opsForValue().set("mykey","akun");
           System.out.println(redisTemplate.opsForValue().get("mykey"));
   
   
       }
   
   }
   
   //这样在java端仅存key值得存储，在redis上查看key会乱码，原因是key没有进行序列化
   ```

源码：

在RedisTemplate.java类里面，进行了序列化配置

![image-20220602220603027](C:\Users\啊坤\AppData\Roaming\Typora\typora-user-images\image-20220602220603027.png)

在真实的开发中，所有的对象都需要将其序列化之后才能存入数据库，序列化的方式有两种：

1. 在实体类中，实现Serializable接口

```java
public class User implements Serializable {
    private String name;
    private Integer age;
}
```



2. 用ObjectMapper类去序列化创建的对象,ObjectMapper类其实就是springBoot配置的一个json类

```java
public void test() throws JsonProcessingException {
    //其实真实开发，都是用json来传递对象
    User user = new User("阿坤", 3);
    String jsonUser = new ObjectMapper().writeValueAsString(user);
    
    System.out.println(redisTemplate.opsForValue().get("user"));
}
```

3.在实际开发中，我们一般都去自定RedisConfig类，如自定义一个序列化，从源头一次解决

```java
@Configuration
public class RedisConfig {

    @Bean
    public RedisTemplate<String, Object> redisTemplate(RedisConnectionFactory redisConnectionFactory)
            throws UnknownHostException {
        //我们为了自己开发方便，一般直接使用<String,Object>
        RedisTemplate<String, Object> template = new RedisTemplate<>();
        template.setConnectionFactory(redisConnectionFactory);
        
        //json序列化
        Jackson2JsonRedisSerializer objectJackson2JsonRedisSerializer = new Jackson2JsonRedisSerializer(Object.class);
        //序列化之后，通过ObjectMapper进行一波转义
        ObjectMapper objectMapper = new ObjectMapper();
        objectMapper.setVisibility(PropertyAccessor.ALL, JsonAutoDetect.Visibility.ANY);
        objectMapper.enableDefaultTyping(ObjectMapper.DefaultTyping.NON_FINAL);
        objectJackson2JsonRedisSerializer.setObjectMapper(objectMapper);
        //String 序列化
        StringRedisSerializer stringRedisSerializer  = new StringRedisSerializer();
        
        //key采用String方式序列化
        template.setKeySerializer(stringRedisSerializer);
        //hashkey也采用string的方式序列化
        template.setHashKeySerializer(stringRedisSerializer);
        //value序列化的方式采用Jackson
        template.setValueSerializer(objectJackson2JsonRedisSerializer);
        //hash的value序列化方式采用json
        template.setHashKeySerializer(objectJackson2JsonRedisSerializer);
        template.afterPropertiesSet();
        
        template.setConnectionFactory(redisConnectionFactory);
        return template;
    }
}
```

```java
@Test
public void test() throws JsonProcessingException {
    //其实真实开发，都是用json来传递对象
    User user = new User("阿坤", 3);

    redisTemplate.opsForValue().set("user", user);

    System.out.println(redisTemplate.opsForValue().get("user"));
}
```



在我们的开发中，我们一般不会用原生的东西去编码，一般都是编写一个自己的工具类，方便我们去编程和管理。

所有的redis操作，其实对于java开发人员来说，十分简单，更重要是要去了解redis的思想和每一种数据结构的用户和作用场景！





# Redis.conf详解

启动的时候，就通过配置文件来启动！

工作中，一些小小的配置，可以让你脱颖而出！

## 单位

![image-20220603103412982](C:\Users\啊坤\AppData\Roaming\Typora\typora-user-images\image-20220603103412982.png)

1. 配置文件unit单位 对大小不敏感

## 包含

![image-20220603103536173](C:\Users\啊坤\AppData\Roaming\Typora\typora-user-images\image-20220603103536173.png)

redis可以包含多个配置文件，就好比我们学习spring、import、include

![image-20220603103740535](C:\Users\啊坤\AppData\Roaming\Typora\typora-user-images\image-20220603103740535.png)

```bash
bingd 127.0.0.1   #绑定的ip
```

![image-20220603103957117](C:\Users\啊坤\AppData\Roaming\Typora\typora-user-images\image-20220603103957117.png)



## 通用GENERAL

![image-20220603104426522](C:\Users\啊坤\AppData\Roaming\Typora\typora-user-images\image-20220603104426522.png)

![image-20220603104741512](C:\Users\啊坤\AppData\Roaming\Typora\typora-user-images\image-20220603104741512.png)

![image-20220603104859260](C:\Users\啊坤\AppData\Roaming\Typora\typora-user-images\image-20220603104859260.png)

## 快照

持久化，在规定的时间内，执行多少次操作，则会持久化到 .rdb.aof

![image-20220603105353773](C:\Users\啊坤\AppData\Roaming\Typora\typora-user-images\image-20220603105353773.png)

![image-20220603105542807](C:\Users\啊坤\AppData\Roaming\Typora\typora-user-images\image-20220603105542807.png)

![image-20220603105816396](C:\Users\啊坤\AppData\Roaming\Typora\typora-user-images\image-20220603105816396.png)



## REPLICATION  复制，我们后面讲主从复制的时候，再进行讲解

![image-20220603195310592](C:\Users\啊坤\AppData\Roaming\Typora\typora-user-images\image-20220603195310592.png)



## SECURITY 安全

![image-20220603110358146](C:\Users\啊坤\AppData\Roaming\Typora\typora-user-images\image-20220603110358146.png)

也可以从用命令行去设置密码

![image-20220603110836125](C:\Users\啊坤\AppData\Roaming\Typora\typora-user-images\image-20220603110836125.png)



## 客户CLIENTS

![image-20220603111329106](C:\Users\啊坤\AppData\Roaming\Typora\typora-user-images\image-20220603111329106.png)

## Memory MANAGEMENT   内存管理

![image-20220603111459098](C:\Users\啊坤\AppData\Roaming\Typora\typora-user-images\image-20220603111459098.png)

![image-20220603111549134](C:\Users\啊坤\AppData\Roaming\Typora\typora-user-images\image-20220603111549134.png)

**maxmemory-policy 六种方式**

**1、volatile-lru：**只对设置了过期时间的key进行LRU（默认值） 

**2、allkeys-lru ：** 删除lru算法的key  

**3、volatile-random：**随机删除即将过期key  

**4、allkeys-random：**随机删除  

**5、volatile-ttl ：** 删除即将过期的  

**6、noeviction ：** 永不过期，返回错误



> APPEND ONLY MODE   模式  aof配置

![image-20220603112037600](C:\Users\啊坤\AppData\Roaming\Typora\typora-user-images\image-20220603112037600.png)

![image-20220603112327180](C:\Users\啊坤\AppData\Roaming\Typora\typora-user-images\image-20220603112327180.png)





# Redis持久化

面试和工作，持久化都是重点！
 Redis 是内存数据库，如果不将内存中的数据库状态保存到磁盘，那么一旦服务器进程退出，服务器中
 的数据库状态也会消失。所以 Redis 提供了持久化功能！

## RDB（Redis DataBase）

> 什么是RDB

在主从复制中，rdb就是备用了！从机上面！

![image.png](https://i.loli.net/2020/12/10/8bZ21fKTvtBWjdl.png?ynotemdtimestamp=1654239367617)

在指定的时间间隔内将内存中的数据集快照写入磁盘，也就是行话讲的Snapshot快照，它恢复时是将快
 照文件直接读到内存里。

Redis会单独创建（fork）一个子进程来进行持久化，会先将数据写入到一个临时文件中，待持久化过程
 都结束了，再用这个临时文件替换上次持久化好的文件。整个过程中，主进程是不进行任何IO操作的。
 这就确保了极高的性能。如果需要进行大规模数据的恢复，且对于数据恢复的完整性不是非常敏感，那
 RDB方式要比AOF方式更加的高效。RDB的缺点是最后一次持久化后的数据可能丢失。我们默认的就是
 RDB，一般情况下不需要修改这个配置！

有时候在生产环境我们会将这个文件进行备份！

**rdb保存的文件是dump.rdb**，都是在我们的配置文件中快照中进行配置的！

![image.png](https://i.loli.net/2020/12/10/CYywKnkc1j4irWQ.png?ynotemdtimestamp=1654239367617)

> 触发机制

 **RDB提供两种触发Redis备份的的方式：**

1. 手动命令去触发保存
   1. save命令、BGSAVE
   2. 执行 flushall 命令，也会触发我们的rdb规则！
   3. 退出redis，也会产生 rdb 文件！
2. 自动触发保存
   1. 满足配置文件中的配置save。。。。

备份就自动生成一个 dump.rdb

![image.png](https://i.loli.net/2020/12/10/MPuOmgse2VhQpiz.png?ynotemdtimestamp=1654239367617)

> 如果恢复rdb文件！

1. 只需要将rdb文件放在我们redis启动目录就可以，redis启动的时候会自动检查dump.rdb 恢复其中
   的数据！
2. 查看需要存在的位置

```
127.0.0.1:6379> config get dir
1) "dir"
2) "/usr/local/bin" # 如果在这个目录下存在 dump.rdb 文件，启动就会自动恢复其中的数据
```

> 几乎就他自己默认的配置就够用了，但是我们还是需要去学习！

**优点**：

1. 适合大规模的数据恢复！
2. 对数据的完整性要不高！
   **缺点：**
3. 需要一定的时间间隔进程操作！如果redis意外宕机了，这个最后一次修改数据就没有的了！
4. fork进程的时候，会占用一定的内容空间！！

## AOF（Append Only File）

将我们的所有命令都记录下来，history，恢复的时候就把这个文件全部在执行一遍！

> 是什么

![image.png](https://i.loli.net/2020/12/11/R9bNlX8x27iBQtM.png?ynotemdtimestamp=1654239367617)

以日志的形式来记录每个写操作，将Redis执行过的所有指令记录下来（读操作不记录），只许追加文件
 但不可以改写文件，redis启动之初会读取该文件重新构建数据，换言之，redis重启的话就根据日志文件
 的内容将写指令从前到后执行一次以完成数据的恢复工作

**Aof保存的是 appendonly.aof 文件**

> append

![image.png](https://i.loli.net/2020/12/10/gh7NDSxLMc5eyvj.png?ynotemdtimestamp=1654239367617)

默认是不开启的，我们需要手动进行配置！我们只需要将 appendonly 改为yes就开启了 aof！
 **重启，redis 就可以生效了！**

如果这个 aof 文件有错位，这时候 redis 是启动不起来的，我们需要修复这个aof文件

redis 给我们提供了一个工具 redis-check-aof --fix 文件名

![image.png](https://i.loli.net/2020/12/10/CR783Gf9d5X46jY.png?ynotemdtimestamp=1654239367617)

> 如果文件正常，重启就可以直接恢复了！

![image.png](https://i.loli.net/2020/12/10/kSuZWoPEqVidfaD.png?ynotemdtimestamp=1654239367617)

> 重写规则说明

aof 默认就是文件的无限追加，文件会越来越大！

![image.png](https://i.loli.net/2020/12/10/i7jlzDIPphJ3k4K.png?ynotemdtimestamp=1654239367617)

如果 aof 文件大于 64m，太大了！ fork一个新的进程来将我们的文件进行重写！

> 优点和缺点！

```bash
appendonly no # 默认是不开启aof模式的，默认是使用rdb方式持久化的，在大部分所有的情况下，
rdb完全够用！
appendfilename "appendonly.aof" # 持久化的文件的名字
# appendfsync always # 每次修改都会 sync。消耗性能
appendfsync everysec # 每秒执行一次 sync，可能会丢失这1s的数据！
# appendfsync no # 不执行 sync，这个时候操作系统自己同步数据，速度最快！
# rewrite 重写，
```

优点：

1. 每一次修改都同步，文件的完整会更加好！
2. 每秒同步一次，可能会丢失一秒的数据
3. 从不同步，效率最高的！

 缺点：

1. 相对于数据文件来说，aof远远大于 rdb，修复的速度也比 rdb慢！
2. Aof 运行效率也要比rdb慢，所以我们redis默认的配置就是rdb持久化

**扩展：**

1. RDB 持久化方式能够在指定的时间间隔内对你的数据进行快照存储
2. AOF 持久化方式记录每次对服务器写的操作，当服务器重启的时候会重新执行这些命令来恢复原始
   的数据，AOF命令以Redis 协议追加保存每次写的操作到文件末尾，Redis还能对AOF文件进行后台重
   写，使得AOF文件的体积不至于过大。
3. 只做缓存，如果你只希望你的数据在服务器运行的时候存在，你也可以不使用任何持久化
4. 同时开启两种持久化方式

- 在这种情况下，当redis重启的时候会优先载入AOF文件来恢复原始的数据，因为在通常情况下AOF
  文件保存的数据集要比RDB文件保存的数据集要完整。
- RDB 的数据不实时，同时使用两者时服务器重启也只会找AOF文件，那要不要只使用AOF呢？作者
  建议不要，因为RDB更适合用于备份数据库（AOF在不断变化不好备份），快速重启，而且不会有
  AOF可能潜在的Bug，留着作为一个万一的手段。

1. 性能建议

- 因为RDB文件只用作后备用途，建议只在Slave上持久化RDB文件，而且只要15分钟备份一次就够
  了，只保留 save 900 1 这条规则。
- 如果Enable AOF ，好处是在最恶劣情况下也只会丢失不超过两秒数据，启动脚本较简单只load自
  己的AOF文件就可以了，代价一是带来了持续的IO，二是AOF rewrite 的最后将 rewrite 过程中产
  生的新数据写到新文件造成的阻塞几乎是不可避免的。只要硬盘许可，应该尽量减少AOF rewrite
  的频率，AOF重写的基础大小默认值64M太小了，可以设到5G以上，默认超过原大小100%大小重
  写可以改到适当的数值。
- 如果不Enable AOF ，仅靠 Master-Slave Repllcation 实现高可用性也可以，能省掉一大笔IO，也
  减少了rewrite时带来的系统波动。代价是如果Master/Slave 同时倒掉，会丢失十几分钟的数据，
  启动脚本也要比较两个 Master/Slave 中的 RDB文件，载入较新的那个，微博就是这种架构。



# Redis发布订阅

Redis发布订阅（pub/sub）是一种消息通信模式：发送者（pub）发送消息，订阅者（sub）接收消息。微信、微博、关注系统！

Redis客户端可以订阅任意数量的频道。

订阅/发布消息图：

第一个角色：消息发送者，第二个角色：频道  第三个角色：消息订阅者

![image-20220603161336762](C:\Users\啊坤\AppData\Roaming\Typora\typora-user-images\image-20220603161336762.png)

![image-20220603161652077](C:\Users\啊坤\AppData\Roaming\Typora\typora-user-images\image-20220603161652077.png)

> 命令

这些命令被广泛用于构建即使通讯应用，比如网络聊天室（chatroom）和实时广播，实时提醒等。

![image-20220603162348749](C:\Users\啊坤\AppData\Roaming\Typora\typora-user-images\image-20220603162348749.png)

> 测试

订阅端：

```bash
127.0.0.1:6379> subscribe akun				#订阅频道
Reading messages... (press Ctrl-C to quit)  #等待接收信息
1) "subscribe"
2) "akun"
3) (integer) 1
# 接收信息
1) "message"    #消息
2) "akun"		# 消息来自哪个频道
3) "hello"     #消息具体内容

```

发送端：

```bash
127.0.0.1:6379> publish akun hello
(integer) 1
127.0.0.1:6379> 

```



> 原理

Redis是使用C实现的，通过分析Redis源码里面的pubsub.c文件，了解发布和订阅机制的底层实现，借此加深对Redis的理解！

Redis通过PUBLISH、SUBSCRIBE和PSUBSCRIBE等命令实现发布和订阅功能。

微信：

![image-20220603165213354](C:\Users\啊坤\AppData\Roaming\Typora\typora-user-images\image-20220603165213354.png)

通过SUBSCRIBE命令订阅某频道后，redis-server里维护了一个字典，字典就是一个个频道！而字典的值则是一个链表，链表中保存了所有订阅了这个channel的客户端。SUBSCRIBE命令的关键字，就是将客户端添加到给定channel的订阅链表中。

通过PUBLISH命令向订阅者发送消息，redis-server会使用给定的频道作为键，在它所维护的channel字典中查找记录了订阅这个频道的所有客户端的链表，遍历这个链表，将消息发布给所有订阅者。

Pub/sub从字面上理解就是发布（Publish）与订阅（subscribe），在Redis中，你可以设定对某一个key值进行消息发布及消息订阅，当一个key值上进行了消息发布后，所有订阅它的客户端都会收到相应的消息。这一功能最明显的用法就是用作实时消息系统，比如普通的即时聊天，群聊等功能。



**使用场景：**

1. 实时消息系统！比如注册网站，就是订阅了网站这个频道。
2. 实时聊天！（频道当作聊天室，将信息回显给所有人即可）
3. 订阅，关注系统都是可以的！

稍微复杂的场景，我们就会使用消息中间件MQ



# Redis主从复制

## 概念

主从复制，是指将一台Redis服务器的数据，复制到其他的Redis服务器。前者成为主节点（master/leader），后者称为从节点（slave/follower）；**数据的复制是单向的，只能由主节点到从节点。Master以写为主，Slave以读为主。**

**默认情况下，每台Redis服务器都是主节点；且一个主节点可以有多个从节点（或者没有从节点），但一个从节点只能由一个主节点。**

主从复制的作用主要包括：

1. 数据冗余：主从复制实现了数据的热备份，是持久化之外的一种数据冗余方式
2. 故障恢复：当主节点出现问题时，可以由从节点提供服务，实现快速的故障恢复；实际上是一种服务的冗余。
3. 负载均衡：在主从复制的基础上，配合读写分离，可以由主节点提供写服务，由从节点提供读服务（即写Redis数据时应用连接主节点，读Redis数据时应用连接从节点），分担服务器负载；尤其是在写少读多的场景下，通过多个从节点分担读负载，可以大大提高Redis服务器的并发量。
4. **高可用基石：除了上述作用以外，主从复制还是哨兵和集群能够实施的基础，因此说主从复制时Redis高可用的基础。**



一般来说：要将Redis运用于工程项目中，只使用一台Redis是万万不能的（宕机，最少一主二从），原因如下：

1. 从结构上，单个Redis服务器会发生单点故障，并且一台服务器需要处理所有的请求负载，压力较大。
2. 从容量上，单个Redis服务器容量是有限的，就算一台Redis服务器内存容量为256G，也不能将所有内存用作Redis存储内存，一般来说，**单台Redis最大使用内存不应该超过20G**

电商网站上的商品，一般都是一次上传，无数次浏览的，说专业点也就是“多读少些”。

对于这种场景，我们可以使用如下这种架构：

![image-20220603172033774](C:\Users\啊坤\AppData\Roaming\Typora\typora-user-images\image-20220603172033774.png)

主从复制，读写分析！80%的情况下都是在进行读操作！减缓服务器的压力！架构中经常使用！最少一主二从！

只要在公司中，主从复制就是必须要使用的，因为在真实的项目中不可能单机使用Redis！



# 集群环境搭建

只配置从库，不用配置主库！

```bash
127.0.0.1:6379> info replication   #查看当前库的信息
# Replication
role:master  
connected_slaves:0   # 没有从机
master_failover_state:no-failover
master_replid:9bed43d70635426a204114c9d0537c350d3e352a
master_replid2:0000000000000000000000000000000000000000
master_repl_offset:0
second_repl_offset:-1
repl_backlog_active:0
repl_backlog_size:1048576
repl_backlog_first_byte_offset:0
repl_backlog_histlen:0

```

复制三个配置文件，然后修改对应的信息

1. 端口
2. pid名字
3. log文件名字
4. dump.rdb名字

修改完毕之后，启动我们的3个redis服务，可以通过进程信息查看！

![image-20220603193644203](C:\Users\啊坤\AppData\Roaming\Typora\typora-user-images\image-20220603193644203.png)



## 一主二从

**默认情况下，每台Redis服务器都都是主节点；**我们一般情况下 只配置从机就好了！

认老大！一主（79）二从（80，81）

slaveof命令配置

```bash
#在从机中查看
127.0.0.1:6380> slaveof 127.0.0.1 6379   #slaveof  host port  找谁当老大
OK
127.0.0.1:6380> info replication
# Replication
role:slave						#当前角色是从机
master_host:127.0.0.1				#可以查看主机的信息
master_port:6379
master_link_status:up
master_last_io_seconds_ago:7
master_sync_in_progress:0
slave_read_repl_offset:0
slave_repl_offset:0
slave_priority:100
slave_read_only:1
replica_announced:1
connected_slaves:0
master_failover_state:no-failover
master_replid:9cd6610ee9206b35e3ba9799d90de15d5015ef96
master_replid2:0000000000000000000000000000000000000000
master_repl_offset:0
second_repl_offset:-1
repl_backlog_active:1
repl_backlog_size:1048576
repl_backlog_first_byte_offset:1
repl_backlog_histlen:0
127.0.0.1:6380> 


#在主机中查看
127.0.0.1:6379> info replication
# Replication
role:master
connected_slaves:1    # 多了从机的配置	
slave0:ip=127.0.0.1,port=6380,state=online,offset=28,lag=1
master_failover_state:no-failover
master_replid:9cd6610ee9206b35e3ba9799d90de15d5015ef96
master_replid2:0000000000000000000000000000000000000000
master_repl_offset:28
second_repl_offset:-1
repl_backlog_active:1
repl_backlog_size:1048576
repl_backlog_first_byte_offset:1
repl_backlog_histlen:28
127.0.0.1:6379> 

```

如果两个都配置完了，是有两个从机的。

![image-20220603194832246](C:\Users\啊坤\AppData\Roaming\Typora\typora-user-images\image-20220603194832246.png)

真实的主从配置应该是在配置文件中配置，这样的话是永久的，我们这里使用的是命令，是暂时的！

从配置文件中配置

![image-20220603195247860](C:\Users\啊坤\AppData\Roaming\Typora\typora-user-images\image-20220603195247860.png)



> 细节

主机可以写，从机不能写只能读!主机中所有的信息和数据，都会自动被从机保存！

主机写：

![image-20220603195617351](C:\Users\啊坤\AppData\Roaming\Typora\typora-user-images\image-20220603195617351.png)

从机只能读取内容

![image-20220603195643409](C:\Users\啊坤\AppData\Roaming\Typora\typora-user-images\image-20220603195643409.png)

测试：

1. 主机断开连接，从机依旧连接到主机，从机依然没有写操作，这个时候，主机如果回来了，从机依旧可以直接获取到主机写的信息
2. 如果是使用命令行来配置的主从，这个时候，如果从机重启了，就会自动变成一个主机。这个时候，主机可以拿到断开连接之前，在主机获取的数据，但不能再获取断开之后更新的数据。只要再次变为从机，立马就会从主机中获取值！



> 复制原理

Slave启动成功连接到master后会发送一个sync同步命令

Master连接到命令，启动后台的存盘进程，同时收集所有接收到的用于修改数据集的命令，在后台进程执行完毕之后，**master将传送整个数据文件到slave，并完成一次完全同步（全量复制）。**

**全量复制**：而slave服务在接收到数据库文件后，将其存盘并加载到内存中。

**增量复制**：Master继续将所有收集到的修改命令一次传给slave，完成同步。

但是只要是重新连接master，一次完全同步（全量复制）将被自动执行。我们的数据一定可以在从机中看到！





> 层层链路

上一个M连接下一个S！

![image-20220603203004537](C:\Users\啊坤\AppData\Roaming\Typora\typora-user-images\image-20220603203004537.png)

这时候，也可以完成我们的主从复制。



> 如果没有老大了（主节点断了），这个时候能不能选择一个老大出来呢？手动

如果主机断了连接，我们可以使用slaveof no  one 让自己变成主机！其他节点（中间节点）就可以手动连接到最新这个主节点（手动）！

如果这个时候老大修复了（原主机），那就要重新连接！（直接再新老大上配置手动进行配置从机，因为原本的就是层层链路的关系）



# 哨兵模式【重点】

（自动选举主机的模式）

> 概述

主从切换技术的方法是：当主服务器宕机后，需要手动把一台服务器切换为主服务器，这就需要人工干预，费时费力。还会造成一段时间内服务不可用。这不是一种推荐的方式，更多时候，我们优先考虑哨兵模式。Redis从2.8开始正式提供了Sentinel（哨兵模式）架构来解决这个问题！

谋朝篡位的自动版，能够后台监控主机是否故障，如果故障了就根据投票数自动将从库转换为主库。

哨兵模式是一种特殊模式，首先Redis提供了哨兵的命令，哨兵是一个独立的进程（还要开一个进程），作为进程，它会独立运行。其原理是**哨兵通过发送命令，等待Redis服务器响应，聪哥监控运行多少个Redis实例。**

![image-20220603204343109](C:\Users\啊坤\AppData\Roaming\Typora\typora-user-images\image-20220603204343109.png)



这里的哨兵有两个作用：

- 通过发送命令，让Redis服务器返回监控其运行状态，包括主服务器和从服务器。
- 当哨兵监测到master宕机，会自动将slave切换成master，然后通过**发布订阅模式**通知其他的从服务器，修改配置文件，让他们切换主机

然而一个哨兵进程对Redis服务器进行监控，可能会出现问题，为此，我们可以使用多个哨兵进行监控。各个哨兵之间还会进行监控，这样就形成了多少兵模式。

![image-20220603204928364](C:\Users\啊坤\AppData\Roaming\Typora\typora-user-images\image-20220603204928364.png)

假设主服务器宕机，哨兵1先检测到这个结果，系统并不会马上进行failover过程，仅仅是哨兵1主观认为的主服务器不可用，这个现象称为**主观下线**。当后面的哨兵也检测到主服务器不可用，并且数量达到一定值时，那么哨兵之间就会进行一个投票，投票的结果就由一个哨兵发起，进行failover【故障转移】操作。切换成功后，就会通过发布订阅模式，让各个哨兵把自己监控的从服务器实现切换主机。这个过程称为**客观下线**。



> 测试！

我们目前的状态是一主二从！

1. 配置哨兵配置文件sentinel.conf

```bash
# sentinel  monitor  被监控的名称 host port 1
sentinel monitor myredis 127.0.0.1 6379 1

```

后面的这个数字1，代表主机挂了，slave投票看让谁接替称为主机，票数最多的，就会称为主机！

2. 启动哨兵！

![image-20220603213431271](C:\Users\啊坤\AppData\Roaming\Typora\typora-user-images\image-20220603213431271.png)

如果Master节点断开，这个时候就会在从机中随机选一个服务器称为主机！（这里面有一个投票算法）

![image-20220604084619086](C:\Users\啊坤\AppData\Roaming\Typora\typora-user-images\image-20220604084619086.png)

哨兵日志

![image-20220604084636117](C:\Users\啊坤\AppData\Roaming\Typora\typora-user-images\image-20220604084636117.png)

如果主机此时回来了，只能归并到新的主机下，当做从机，这就是哨兵模式的规则！

> 哨兵模式

优点：

1. 哨兵集群，基于主从复制模式，所有的主从配置优点，它全有
2. 主从可以切换，故障可以转移，系统的可用性就会更好
3. 哨兵模式就是主从模式的升级，手动到自动，更加健壮！

缺点：

1. Redis不好在线扩容，集群容量一旦到达上限，在就扩容就十分麻烦！
2. 实现哨兵模式的配置其实是很麻烦的，里面有很多选择！

> 哨兵模式的全部配置！

```bash
# Example sentinel.conf
# 哨兵sentinel实例运行的端口 默认26379
port 26379

# 哨兵sentinel的工作目录
dir /tmp

# 哨兵sentinel监控的redis主节点的 ip port
# master-name 可以自己命名的主节点名字 只能由字母A-z、数字0-9 、这三个字符".-_"组成。
# quorum 配置多少个sentinel哨兵统一认为master主节点失联 那么这时客观上认为主节点失联了
# sentinel monitor <master-name> <ip> <redis-port> <quorum>
sentinel monitor mymaster 127.0.0.1 6379 2

# 当在Redis实例中开启了requirepass foobared 授权密码 这样所有连接Redis实例的客户端都要提供密码
# 设置哨兵sentinel 连接主从的密码 注意必须为主从设置一样的验证密码
# sentinel auth-pass <master-name> <password>
sentinel auth-pass mymaster MySUPER--secret-0123passw0rd

# 指定多少毫秒之后 主节点没有应答哨兵sentinel 此时 哨兵主观上认为主节点下线 默认30秒
# sentinel down-after-milliseconds <master-name> <milliseconds>
sentinel down-after-milliseconds mymaster 30000

# 这个配置项指定了在发生failover主备切换时最多可以有多少个slave同时对新的master进行 同步，这个数字越小，完成failover所需的时间就越长，但是如果这个数字越大，就意味着越 多的slave因为replication而不可用。可以通过将这个值设为 1 来保证每次只有一个slave 处于不能处理命令请求的状态。
# sentinel parallel-syncs <master-name> <numslaves>
sentinel parallel-syncs mymaster 1

# 故障转移的超时时间 failover-timeout 可以用在以下这些方面：
#1. 同一个sentinel对同一个master两次failover之间的间隔时间。
#2. 当一个slave从一个错误的master那里同步数据开始计算时间。直到slave被纠正为向正确的master那里同步数据时。
#3.当想要取消一个正在进行的failover所需要的时间。 
#4.当进行failover时，配置所有slaves指向新的master所需的最大时间。不过，即使过了这个超时，slaves依然会被正确配置为指向master，但是就不按parallel-syncs所配置的规则来了
# 默认三分钟
# sentinel failover-timeout <master-name> <milliseconds>
sentinel failover-timeout mymaster 180000

# SCRIPTS EXECUTION
#配置当某一事件发生时所需要执行的脚本，可以通过脚本来通知管理员，例如当系统运行不正常时发邮件通知相关人员。
#对于脚本的运行结果有以下规则：
#若脚本执行后返回1，那么该脚本稍后将会被再次执行，重复次数目前默认为10
#若脚本执行后返回2，或者比2更高的一个返回值，脚本将不会重复执行。
#如果脚本在执行过程中由于收到系统中断信号被终止了，则同返回值为1时的行为相同。
#一个脚本的最大执行时间为60s，如果超过这个时间，脚本将会被一个SIGKILL信号终止，之后重新执行。
#通知型脚本:当sentinel有任何警告级别的事件发生时（比如说redis实例的主观失效和客观失效等等），将会去调用这个脚本，这时这个脚本应该通过邮件，SMS等方式去通知系统管理员关于系统不正常运行的信息。调用该脚本时，将传给脚本两个参数，一个是事件的类型，一个是事件的描述。如果sentinel.conf配置文件中配置了这个脚本路径，那么必须保证这个脚本存在于这个路径，并且是可执行的，否则sentinel无法正常启动成功。

#通知脚本
# shell编程
# sentinel notification-script <master-name> <script-path>
sentinel notification-script mymaster /var/redis/notify.sh

# 客户端重新配置主节点参数脚本
# 当一个master由于failover而发生改变时，这个脚本将会被调用，通知相关的客户端关于master地址已经发生改变的信息。
# 以下参数将会在调用脚本时传给脚本:
# <master-name> <role> <state> <from-ip> <from-port> <to-ip> <to-port>
# 目前<state>总是“failover”,
# <role>是“leader”或者“observer”中的一个。
# 参数 from-ip, from-port, to-ip, to-port是用来和旧的master和新的master(即旧的slave)通信的
# 这个脚本应该是通用的，能被多次调用，不是针对性的。
# sentinel client-reconfig-script <master-name> <script-path>
sentinel client-reconfig-script mymaster /var/redis/reconfig.sh # 一般都是由运维来配置！

```



# Redis缓存穿透和雪崩

> 服务器的高可用问题

Redia缓存的使用，极大的提升了应用程序的性能和效率，特别是数据查询方面。但同时，它也带来了一些问题。其中，最重要的问题，就是数据的一致性问题。从严格意义上讲，这个问题无解，如果对数据的一致性要求很高，那么就不能使用缓存。

另外的一些典型问题就是，缓存穿透、缓存雪崩和缓存击穿。目前，业界也都有比较流行的解决方案。



## 缓存穿透（查不到导致的）

> 概念

缓存穿透的概念和简单，用户想要查询一个数据，发现redis内存数据库都没有，也就是缓存没命中，于是向持久层数据查询，发现也没有，于是本次查询失败。当用户很多的时候，缓存都没有命中（秒杀），于是都去请求了持久层数据库。这会给持久层数据库造成很大的压力，这时候就相当于出现了缓存穿透。



> 解决方案

**布隆过滤器**

布隆过滤器是一种数据结构，对所有可能查询的参数以hash形式存储，在控制层先进行校验，不符合则丢弃，从而避免了对底层存储系统的查询压力！

![image-20220604091726539](C:\Users\啊坤\AppData\Roaming\Typora\typora-user-images\image-20220604091726539.png)

**缓存空对象**

当存储层不命中后，即使返回的空对象也将其缓存起来，同时会设置一个过期的时间，之后再访问这个数据将会从缓存中获取，保护了后端数据源；

![image-20220604092159240](C:\Users\啊坤\AppData\Roaming\Typora\typora-user-images\image-20220604092159240.png)

但是这种方法会存在两个问题：

1. 如果空值能够被缓存起来，这就意味着缓存需要更多的空间存储更多的键，因为这当中可能会有很多的空值的键；
2. 即使对控制设置了过期时间，还是会存在缓存层和存储层的数据会有一段时间窗口不一致，这对需要保持一致性的业务会有影响。





## 缓存击穿（量太大，缓存过期）

> 概述

这里需要注意和缓存穿透的区别，缓存击穿，是指一个key非常热点，在不同的扛着大并发，大并发集中对这一个点进行访问，当这个key在失效的瞬间，持续的大并发就穿破缓存，直接请求数据库，就像在一个屏障上凿开一个洞。

当某个key在过期的瞬间，有大量的请求并发访问，这类数据一般是热点数据，由于缓存过期，会同时访问数据库来查询最新数据，并且回写缓存，会导致数据库瞬间压力过大。



> 解决方案

**设置热点数据永不过期**

从缓存层面来看，没有设置过期时间，所以不会出现热点key过期后产生的问题。

**加互斥锁**

分布式锁：使用分布式锁，保证对于每个key同时只有一个线程去查询后端服务，其他线程没有获得分布式锁的权限，因此只需要等待即可。这种方式将高并发的压力转移到分布式锁，因此对分布式锁的考验很大。

![image-20220604093405740](C:\Users\啊坤\AppData\Roaming\Typora\typora-user-images\image-20220604093405740.png)

# 缓存雪崩

> 概念

缓存雪崩，是指在某一个时间段，缓存集体失效。Redis宕机！

产生雪崩的原因之一，比如在双十一零点，会迎来一波抢购，这波商品时间比较集中的放入缓存，假设缓存了一个小时。那么到了凌晨一点钟的时候，这批商品的缓存就都过期了。而对这批商品的访问查询，都落到了数据库上，对于数据库而言，就会产生周期性的压力波峰。于是所有的请求都会达到存储层，存储层的调用量会暴增，造成存储层也会挂掉的情况。

![image-20220604093924690](C:\Users\啊坤\AppData\Roaming\Typora\typora-user-images\image-20220604093924690.png)

其实集中过期，倒不是非常致命，比较致命的是缓存雪崩，是缓存服务器某个节点宕机或断网。因为自然形成的缓存雪崩，一定是在某个时间段集中创建缓存，这个时候，数据库也是可以顶着压力的。无非就是对数据库产生周期性的压力而已。而缓存服务节点的宕机，对数据库服务器造成的压力是不可预知的，很有可能瞬间就把数据库压垮。

双十一：定掉一些服务，（保证主要的服务可用！）



> 解决方案

**reids高可用**

这个思想的含义是：既然redis有可能挂掉，那我多增设几台reids，这样一台挂掉之后其他的还可以继续工作，其实就是搭建的集群。（异地多活）

**限流降级**

这个解决方案的思想是：在缓存失效后，通过加锁或者队列来控制读取数据库缓存的线程数量。比如对某个key只允许一个线程查询和写缓存，其他线程等待。

**数据预热**

数据加热的含义就是在正式部署之前，我先把可能的数据先预先访问以以便，这样部分可能访问的数据就会加载到缓存中。在即将发生大并发访问前动手触发加载缓存不同的key，设置不同的过期时间，让缓存失效的时间点尽量均匀。

