# MySQL



## 1、操作数据库

### 1、数据库的列类型

> 数值

- tinyint	十分小的数据      1个字节
- smallint   较小的数据         2个字节
- mediiumint   中等大小的数据    3个字节
- **int       标准的整数             4个字节        常用的int**  【int（3）不是指定字节，而是指定0填充位数】
- bigint     较大的整数             8个字节
- float       浮点数            4个字节
- double         浮点数                 8个字节
- decimal      字符串形式的浮点数        金融计算的时候，一般使用deciaml



> 字符串

- char     字符串大小固定  0-255
- **varchar    可变字符串    0-65535 **  常用的变量    String
- tinytext    微型文本   2^8-1
- **text      文本串    2^16-1     保存大文本**



> 时间日期

java.util.Date

- date    YYYY-MM-DD，日期格式
- time    HH：mm：ss  时间格式
- dateTime    YYYY-MM-DD   HH：mm：ss  最常用的时间格式
- **timestamp 时间戳   1970.1.1到现在的毫秒数！**  也较为常用！





> null

- 没有值，位置
- ==注意，不要使用NULL进行运算，结果为NULL



## 2、数据库的字段属性

Unsigned：

- 无符号的整数（只有整数的列可以使用）
- 声明了该列不能声明为负数



zerofill:

- 0填充的
- 不足的位数，使用0来填充，int（3）， 5----005



自增：

- 通常理解为自增，自动在上一条记录的基础上+1（默认）
- 通常用来设计唯一的主键，index，必须是整数类型
- 可以自定义设计主键自增的起始值和步长



非空：NULL      not null

- 假设设置为not null，如果不给它赋值，就会报错！
- NULL，如果不填写之，就是null！



默认：

- 设置默认的值
- sex，默认值为男，如果不指定默认的值，默认值为男



**每个表都必须存在以下五个字段**【未来做项目用的，表示记录存在的意义】

```sql
id：主键

version   乐观锁

id_delete   伪删除

gmt_creat   创建时间

gmt_update  修改时间


```



常用命令

```sql
SHOW CREATE DATABASE shool   --查看创建数据库的语句
SHOW CREATE TABLE  student  --查看student数据表的定义语句
DESC student  --显示表的结构
```





## 3、数据表的类型【引擎的区别】

```sql
- InnoDB  默认使用
- MyISAM  早些年使用的
```

|            | MYISAM | INNODB                 |
| ---------- | ------ | ---------------------- |
| 事务支持   | 不支持 | 支持                   |
| 数据行锁定 | 不支持 | 支持                   |
| 外键       | 不支持 | 支持                   |
| 全文索引   | 支持   | 不支持                 |
| 表空间大小 | 较小   | 较大，约为MyISAM的两倍 |

常规使用操作：

- MYISAM    节约空间，速度较块
- INNODB  安全性高，事务处理，多表多用户操作



> 在物理空间的位置

所有的数据库文件都存在data目录下

本质还是文件存储！

MYSQL引擎在物理文件的区别：

- InnoDB在数据库表中只有一个*.frm文件，以及上级目录下的ibdata1文件
- MyISAM对应文件
  - *.frm  --表结构的定义文件
  - *.MYD  --数据文件（data）
  - *.MYI    --索引文件（index）



> 设置数据库表的字符集编码

```sql
CHARSET=utf8
```

不是指的化，会是mysql默认的字符集编码  （不支持中文）

MYSQL的默认编码是Latin1，不支持中文

在my.ini中配置默认的编码

```sql
character-set-server = utf8
```



## 4、MySQL数据管理

### 	1、外键（了解即可）

> ​		方式一：在创建表的时候，增加约束

![image-20220526162417571](C:\Users\啊坤\AppData\Roaming\Typora\typora-user-images\image-20220526162417571.png)

​	删除有外键关系的表的时候，必须先删除引用别人的表（从表），再删除被引用的表（主表）

> 方式二：先建表，然后增加约束进去

```sql
ALTERS TABLE 表 ADD CONSTRAINT 约束名  FOREIGN KEY (作为外键的列) REFERENCES 哪个表（哪个字段)
```



以上的操作都是物理外键，数据库级别的晚见，我们不建议使用（避免数据库过多而造成困扰，这里了解即可）



**最佳实践**：

- 数据库就是单纯的表，知识用来存数据，只有行（数据）和列（字段）
- 我们像使用多张表的数据，想使用外键（程序去实现）

为什么不使用外键：

​	每次做DELETE或者UPDATE都必须考虑外键约束，会导致开发的时候很痛苦，测试数据极为不方便

### 2、DML语言（全部记住）

数据库的意义：数据存储，数据管理

DML：数据库操作语言

- INSERT
- UPDATE
- DELETE



1. **插入语句**

   ```sql
   insert into 表名(字段名1，字段名2，字段名3) values('值1'，'值2'，'值3')
   ```

   注意事项：

   - 字段和字段之间，使用英文逗号隔开
   - 字段是可以省略饿，但是后面的值必须要一一对应，不能少
   - 可以同时插入多条数据，VALUES后面的值，需要使用都好隔开即可  VALUES(),()....

2. **修改语句**

   > update  表明 set  原来的值 = 新值  （条件）

   注意：

   - colnum_name 是数据库的列，尽量带上''
   - 条件，帅选的条件，如果没有指定，则会修改所有的列
   - value，是一个具体的值，也可以是一个变量
   - 多个设置的属性之间，使用英文逗号隔开

   

​	3、**删除语句**

> DELETE 命令

```sql
--删除数据（避免这样写，会删除全部数据）
DELETE FROM 'student'

--删除指定数据
DELETE FROM 'student' WHERE id = 1
```



> TRUNCATE命令

作用:完全清空一个数据库表，表的结构和索引越苏不会变！

```sql
--清空 student表
TRUNCATE 'student'
```



> DELETE和TRUNCATE区别

- 相同点：都能删除数据，不会删除表的结构
- 不同
  - TRUNCATE 重新设置，自增列、计数器会归零
  - TRUNCATE不会影响事务
  - DELETE删除的问题，重启数据库后，现象
    - InnoDB  自增列会重1开始（存在内存当中，断电即失），如果不重启的话，就会继续从之前的子增量开始
    - MyISAM 继续从上一个子增量开始（存在文件重的，不会丢失）



### 3、DQL查询数据（最重点）

1、DQL

​	（Data Query Language：数据查询语言)

- 所有的查询操作都用它  select
- 简单的查询，复杂的查询它都能做
- **数据库最核心的语言，最重要的语句**
- 使用频率最高的语句



![image-20220526211227555](C:\Users\啊坤\AppData\Roaming\Typora\typora-user-images\image-20220526211227555.png)

> select  字段 【as 别名】 from 表名 【where条件】



> 去重 distinct

```sql
select distinct 'studentNo' from  student;
```



> 数据库的列（表达式）

```sql
SELECT VERSION() --查看系统版本（函数）
SELECT 100*3  AS 计算结果    （表达式）
SELECT  @@auto_increat_increment -- 查询自增的补偿（变量）
```



> 模糊查询：比较运算符

| 运算符      | 语法              | 描述                                               |
| ----------- | ----------------- | -------------------------------------------------- |
| IS NULL     | a is null         | 如果操作符为NULL，结果为真                         |
| IS NOT NULL | a is not null     | 如果操作符不为null，结果为真                       |
| BETWEEN     | a between b and c | 如果a再b和c之间，则结果为真                        |
| LIKE        | a Like b          | SQL匹配，如果a匹配b，则结果为真                    |
| In          | a in（a1，a2）    | 假设a在a1，或者a2.。。。其中的某一个值中，结果为真 |

like用法

like '刘_'  :刘后面只有一个字，

like ’刘%' ：刘后面有多个字

一个_代表一个字，两个代表两个，%代表多个



### 4、联表查询 JOIN

![img](https://img-blog.csdnimg.cn/20181103160140252.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2h1YW5nX18y,size_16,color_FFFFFF,t_70)

> JOIN

```sql
--查询 参加了考试得同学（学号，姓名，科目编号，分数）
SELECT * FROM student
SELECT * FROM result
(学生表有学号，姓名等字段，结果表有学号，成绩等字段)

/*
思路：
	1、分析需求，分析查询的字段来自哪些表（连接查询）
	2、确定使用哪种连接查询？   7中
	确定交叉点（这两个表中，哪个数据是相同的）
	判断的条件：学生表中 studentNo = 成绩表 studentNo
*/

--join on 连接查询
--where 等值查询
--多表查询直接在后面加相应的join on

--INNER JOIN
--两个表中相同的字段，要指定从哪个表中查询
SELECT s.studentNo, studentName,SubjectNo, StudentResult
FROM student AS s
INNER JOIN result as r
ON s.studentNo == r.studentNo


--Right JOIN

SELECT s.studentNo, studentName,SubjectNo, StudentResult
FROM student AS s
RIGHT JOIN result as r
ON s.studentNo == r.studentNo

```

| 操作       | 描述                                       |
| ---------- | ------------------------------------------ |
| Inner join | 如果表中至少有一个匹配，就返回这个结果     |
| left  join | 会从左表中返回所有的值，即使右表中没有匹配 |
| right join | 会从右表中返回所有的值，即使右表中没有匹配 |



> 自连接（了解）

==自己的表和自己的表连接，核心：**一张表拆为两张一样的表即可**【就是设置别名】去指定来查==



### 5、分页和排序

- 分页：limit
- 排序 order by， 升序 ：ASC  降序：DESC

> 排序

![image-20220526211459718](C:\Users\啊坤\AppData\Roaming\Typora\typora-user-images\image-20220526211459718.png)

> 分页：为什么分页？---->缓解数据库压力，给人的体验更好；  瀑布流：拉布到底

![image-20220526212427015](C:\Users\啊坤\AppData\Roaming\Typora\typora-user-images\image-20220526212427015.png)

语法：limit 查询起始下表，pageSize



### 6、子查询

where（这个值是计算出来的）

本质：在where语句中嵌套一个查询语句

where（select）跟连表查询逻辑是一样的，只不过是where 判定条件中，有一个条件是查出来的集合。



### 7、分组和过滤

![image-20220527003100819](C:\Users\啊坤\AppData\Roaming\Typora\typora-user-images\image-20220527003100819.png)

- 如果不分组的话，只能查出一个，因为最高分和最低分只有一个
- 分组之后，不能用where去作判定条件，只能用HAVING



## 5、MYSQL函数

### 1、常用函数

![image-20220527001923469](C:\Users\啊坤\AppData\Roaming\Typora\typora-user-images\image-20220527001923469.png)

### 2、聚合函数

| 函数名称 | 描述   |
| -------- | ------ |
| COUNT()  | 计数   |
| SUM()    | 求和   |
| AVG()    | 平均值 |
| MAX()    | 最大值 |
| MIN()    | 最小值 |
| ......   |        |



![image-20220527002604110](C:\Users\啊坤\AppData\Roaming\Typora\typora-user-images\image-20220527002604110.png)





## 6、事务

### 1、什么是事务？

​		要么都成功，要么都失败

======================

1、SQL执行   A给B转账   A 1000  --->200     B    200

2、SQL执行    B收到A的钱     A 800   ------>     B 400

=====================

将一组SQL放在一个批次中去执行~



> 事务原则：ACID事务原则  原子性、一致性、隔离性、持久性   （脏读、幻读）

参考博客链接：https://blog.csdn.net/weixin_43907800/article/details/104571137?utm_medium=distribute.pc_aggpage_search_result.none-task-blog-2~aggregatepage~first_rank_ecpm_v1~rank_v31_ecpm-3-104571137-null-null.pc_agg_new_rank&utm_term=%E4%BA%8B%E5%8A%A1%E5%9B%9B%E4%B8%AA%E7%89%B9%E6%80%A7&spm=1000.2123.3001.4430

**原子性（Atomicity）**

- 要么都成功，要么都失败

**一致性（Consistency）**

- 事务前后的数据完整性要保证一致

**持久性（Durability）**  --事务提交

- 事务一旦提交则不可逆，被持久化到数据库中

**隔离性（Isolation）**

- 事务的隔离性是多个用户并发访问数据库时，数据库为每个用户开启一个事务，不能被其他事务的操作数据所干扰，事务之间要相互隔离。



> 隔离所导致的一些问题

**脏读：**指一个事务读取了另一个事务未提交的数据

**不可重复读：**在一个事务内读取表中的某一行数据，多次读取结果不同。（这个不一样时错误，只是某些场合不对）

**幻读（虚读）：**是指在一个事务内读取到了别的事务插入的数据，导致前后读取不一致。



## 7、索引：在小数据量的时候用处不大，大数据的时候，十分区别

> MySQL官方对索引的定义为:索引（index）是帮助MySQL高效获数据的数据结构。提取句子主干，就可以得到索引的本质：索引就是数据结构 。
>
> ```sql
> 查询是数据库最主要的功能之一。我们知道很多查询算法，比如顺序查找，二分法查找，二叉树查找等。但是，每种查找算法都只能应用在特定的数据机构之上，例如二分查找要求被检索的数据有序，二叉树查找只能应用于二叉查找树上，但数据本身的结构组织不可能完全满足各种数据结构。所以，在数据之外，数据库系统还维护者满足特定查找算法的数据结构，这些数据结构以某种方式引用（只想数据），这样就可以在这些数据结构上实现高级查找算法。
> ```
>
> 
>
> 参考博客：https://www.kancloud.cn/kancloud/theory-of-mysql-index/41855

### 1、索引的分类

> 在一个表中，主键索引只能有一个，唯一索引可以有多个

- 主键索引（PRIMARY KEY）
  - 唯一标识，主键不可重复，只能有一列作为主键
- 唯一索引（UBIQUE KEY）
  - 避免重复的列出现，唯一索引可以重复，多个列都可以标识位i的唯一索引
- 常规索引（KEY/INDEX）
  - 默认的，index，key关键字来设置
- 全文索引（FULLTEXT INDEX）
  - 在特定的数据库引擎下蔡有，MyISAM，现在INnodb也支持
  - 快速定位数据

![image-20220527103307169](C:\Users\啊坤\AppData\Roaming\Typora\typora-user-images\image-20220527103307169.png)



### 2、索引原则

- 索引不是越多越好
- 不要对进程变动数据加索引
- 小数据量的表不需要加索引
- 索引一般加在常用来查询的字段上！



> 索引的数据结构

Hash类型的索引

BTree：InnoDB的默认数据接结构~

参考：http://blog.codinglabs.org/articles/theory-of-mysql-index.html



## 8、权限管理和备份

### 1、用户管理

> SQL命令操作

用户表：mysql.user

本质：读这张表进行增删改查



### 2、MySql备份

为什么要备份：

- 保证重要的数据不对是
- 数据转移

MYSQL数据库备份的方式

- 直接拷贝物理文件
- 在sqlyog这种可视化工具中手动导出
- 使用命令行导出  mysqldump 命令行导出

```sql
# mysqldump -h主机 -u用户名  -p密码  数据库 表名 【表1】【表2】  >物理磁盘位置/文件名
```

![image-20220529093049165](C:\Users\啊坤\AppData\Roaming\Typora\typora-user-images\image-20220529093049165.png)



假设你要备份呢数据库，防止数据丢失，

把数据库给朋友，sql文件发给别人即可！



## 9、规范数据库设计

### 9.1、为什么需要设计

当数据库比较复杂的时候，我们就需要设计了

糟糕的数据库设计：

- 数据冗余、浪费空间
- 数据库插入和删除都会麻烦、异常【屏蔽物理外键】
- 程序的性能差

良好的数据库设计：

- 节省内存空间
- 保证数据库的完成性
- 方便我们开发系统



**软件开发中，关于数据库的设计**

1. 分析需求：分析业务和需要处理的数据库的需求
2. 概要设计L设计关系图E-R图



设计数据库的步骤：（个人博客）

- 收集信息，分析需求
  - 用户表（用户登录注销，用户的个人信息，写博客，创建分类）
  - 分类表（文章分类，谁创建的）
  - 文章表（文章的信息）
  - 友情链表（友链信息）
  - 自定义表（系统信息，某个关键的字，或者一些主字段）key：value
- 标识实体（把需求落地到每个字段）
- 标识实体之间的关系
  - 写博客user-->blog
  - 创建分类user ->category
  - 关注user -> user



### 2、三大范式

**为什么需要数据规范化**？

- 信息重复
- 更新异常
- 插入异常
  - 无法正常显示信息
- 删除异常
  - 丢失有效信息



> 三大范式

**第一范式(1NF)**：要求数据库表中每一列都是不可分割的原子数据项（原子性）

原子性：保证每一列不可再分

![image-20220529095858903](C:\Users\啊坤\AppData\Roaming\Typora\typora-user-images\image-20220529095858903.png)

**第二范式（2NF）：**

前提：满足第一范式

每张表只描述一间事情

![image-20220529100229260](C:\Users\啊坤\AppData\Roaming\Typora\typora-user-images\image-20220529100229260.png)

一个订单可以有多个产品，如果上面不分开两个表，就会出现订单号与订单金额和时间的数据出现冗余

**第三范式（3NF）**

前提：满足1NF、2NF

第三范式需要确保数据表中的每一列数据都和书简直接相关，而不能间接相关

![image-20220529100950950](C:\Users\啊坤\AppData\Roaming\Typora\typora-user-images\image-20220529100950950.png)

（三大范式的设计是为了规范数据库）



**规范性和性能的问题**

关联查询的表不得超过三张表

- 考虑商业化的需求和目标（成本和用户体验）数据库的性能更加重要
- 在规范性能问题的时候，需要适当的考虑以下规范性！
- 故意给某些表郑家一些冗余字段。（从多表查询变为单表查询）
- 故意增加一些计算列{从大数据量降低为小数据量的查询：索引）
