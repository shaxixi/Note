[TOC]

## 事务

[参考：事务的隔离级别](https://blog.csdn.net/zhouym_/article/details/90381606?biz_id=102&utm_term=事务的隔离级别&utm_medium=distribute.pc_search_result.none-task-blog-2~all~sobaiduweb~default-1-90381606&spm=1018.2118.3001.4187)

### 什么是事务

> 要么都成功，要么都失败
>
> 将一组SQL放在一个批次中执行

1. SQL执行， A给B转账 A 1000–> 200 B200
2. SQL 执行， B收到A的钱 A800 — B400

### 事务原则：ACID 原则（原子性、一致性、隔离性、持久性）

1. **原子性(Atomicity)**
   	要么都成功，要么都失败

2. **一致性 (Consistency)**

   ​	事务前后的数据完整性要保证一致,

3. **持久性(Durability)** — 事务提交

   ​	事务一 旦提交则不可逆，被持久化到数据库中!

4. **隔离性(Isolation)**

   ​	事务产生多并发时，互不干扰

### 隔离产生的问题

1. **脏读：**

   ​	指一个事务读取了另外一个事务未提交的数据。

2. **不可重复读：**

   ​	在一个事务内读取表中的某一行数据，多次读取结果不同。（这个不一定是错误，只是某些场合不对）

3. **虚读(幻读)**:

   ​	是指在一个事务内读取到了别的事务插入的数据，导致前后读取不一致。（一般是行影响，多了一行）

### 执行事务

```mysql
-- ====================事务=========================
-- mysql 是默认开启事务自动提交的
SET autocommit = 0 /*关闭*/
SET autocommit = 1 /*开启(默认的) */

-- ---------------------------------------------------
-- 手动处理事务
SET autocommit = 0 -- 关闭自动提交
-- 事务开启
START TRANSACTION -- 标记一个事务的开始，从这个之后的sql 都在同一个事务内
INSERT xx
INSERT xx
-- 提交:持久化(成功!)
COMMIT
-- 回滚: 回到的原来的样子(失败! )
ROLLBACK
-- 事务结束
SET autocommit = 1 -- 开启自动提交
-- ---------------------------------------------------

-- 了解
SAVEPOINT  保存点名 -- 设置一个事务的保存点
ROLLBACK to SAVEPOINT 保存点名 -- 回滚到保存点
RELEASE SAVEPOINT 保存点名 -- 撤销保存点
```

**模拟场景**

```sql
CREATE DATABASE shop CHARACTER SET utf8 COLLATE utf8_general_ci
USE shop
CREATE TABLE `account`(
`id` INT(3) NOT NULL AUTO_INCREMENT,
`name` VARCHAR(30) NOT NULL,
`money` DECIMAL(9,2) NOT NULL,
PRIMARY KEY (`id`)
)ENGINE=INNODB DEFAULT CHARSET=utf8

INSERT INTO account(`name`,`money`)
VALUES('A',2000),('B',10000)

-- 模拟转账：事务
SET autocommit = 0; -- 关闭自动提交
START TRANSACTION -- 开启事务（一组事务）
UPDATE account SET money = money-500 WHERE `name` = 'A' -- A 转账给B
UPDATE account SET money = money+500 WHERE `name` = 'B' -- B 收到钱

COMMIT ; -- 提交事务
ROLLBACK ; -- 回滚

SET autocommit=1 -- 恢复默认值
```

## 索引

[索引总结](https://www.cnblogs.com/tgycoder/p/5410057.html)

> MySQL官方对索引的定义为:索引(Index) 是**帮助MySQL高效获取数据的数据结构**。
>
> 提取句子主干，就可以得到索引的本质:索引是数据结构。
>
> MySQL索引的建立对于MySQL的高效运行是很重要的，索引可以大大提高MySQL的检索速度。

### 索引的分类

> 在一个表中，主键索引只能有一个，唯一索引可以有多个

- 主键索引 （PRIMARY KEY）
  - 唯一的标识，主键不可重复，只能有一个列作为主键
- 唯一索引 （UNIQUE KEY）
  - 避免重复的列出现，唯一索引可以重复，多个列都可以标识唯一索引
- 常规索引（KEY/INDEX）
  - 默认的，index,key关键字来设置
- 全文索引（FULLTEXT）
  - 在特点的数据库引擎下才有，MyISAM
  - 快速定位数据

```mysql
-- 索引的使用
-- 1、在创建表的时候给字段增加索引
-- 2、创建完毕后，增加索引

-- 显示所有的索引信息
SHOW INDEX FROM student

-- 增加一个全文索引(索引名) 列名
ALTER TABLE school.student ADD FULLTEXT INDEX `studentName` ( studentName ) ;

-- EXPLAIN 分析sql执行的状况
EXPLAIN SELECT * FROM student; --非全文索引
EXPLAIN SELECT * FROM student WHERE MATCH(studentName) AGAINST('刘');
```

### 测试索引

```mysql
CREATE TABLE `app_user`(
	`id` BIGINT(20) UNSIGNED NOT NULL AUTO_INCREMENT,
	`name` VARCHAR(50) DEFAULT '' COMMENT '用户呢称',
	`email` VARCHAR(50) NOT NULL COMMENT '用户邮箱',
	`phone` VARCHAR(20) DEFAULT '' COMMENT '手机号',
	`gender` TINYINT(4) UNSIGNED DEFAULT'0' COMMENT '性别(0: 男: 1:女)',
	`password` VARCHAR(100) NOT NULL COMMENT '密码',
	`age` TINYINT(4) DEFAULT '0' COMMENT '年龄', 
	`create_time` DATETIME DEFAULT CURRENT_TIMESTAMP,
	`update_time` TIMESTAMP NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
	PRIMARY KEY (`id`)
) ENGINE=INNODB DEFAULT CHARSET=utf8mb4 COMMENT='app用户表'

-- 插入100万数据  34.183 sec
DELIMITER $$ -- 写函数之前必须要写，标志
CREATE FUNCTION mock_data()
RETURNS INT
BEGIN
	DECLARE num INT DEFAULT 1000000; 
	DECLARE i INT DEFAULT 0;
	
	WHILE i<num DO
		INSERT INTO app_user(`name`,`email`,`phone`,`gender`,`password`,`age`)
		VALUES (CONCAT('用户',i), '24736743@qq.com',CONCAT ('18', FLOOR(RAND()*(1000000000))),
		FLOOR(RAND()*2),UUID(),FLOOR(RAND()*100));
		SET i= i + 1;	
	END WHILE; 
	RETURN i;
END;
SELECT mock_data();

SELECT * FROM app_user WHERE `name`='用户9999'; -- 0.764 sec/ 0.925 sec/0.785 sec

EXPLAIN SELECT * FROM app_user WHERE `name`='用户9999';

-- id_表名 字段名
-- CREATE INDEX 索引名 on 表(字段)
CREATE INDEX id_app_user_name ON app_user(`name`);

SELECT * FROM app_user WHERE `name`='用户9999'; -- 0.002 sec/0.011 sec/0.005 sec
EXPLAIN SELECT * FROM app_user WHERE `name`='用户9999';
```

**有索引**
![在这里插入图片描述](E:\软工\typora笔记\mysql数据库笔记\no.4 事务_索引_权限_备份 .assets\20201013212505463.png)



**无索引**
![在这里插入图片描述](E:\软工\typora笔记\mysql数据库笔记\no.4 事务_索引_权限_备份 .assets\20201013212523973.png)

> 索引在小数据量的时候，用户不大，但是在大数据的时候，区别十分明显一

### 索引原则

- 索引不是越多越好
- 不要对进程变动数据加索引
- 小数据量的表不需要加索引
- 索引一般加在常用来查询的字段上!

## 权限管理和备份

### 用户管理

**SQLyog 可视化管理**

![image-20201227190626053](C:\Users\傻兮兮乎乎\AppData\Roaming\Typora\typora-user-images\image-20201227190626053.png)



**SQL命令操作**

用户表：mysql.user

本质：对这张表进行，增删改查

```sql
-- 创建用户  CREATE USER 用户名 IDENTIFIED BY '密码'
CREATE USER sanjin IDENTIFIED BY '123456'

-- 修改密码（修改当前密码）
SET PASSWORD = PASSWORD('111111')

-- 修改密码（修改指定用户密码）
SET PASSWORD FOR sanjin = PASSWORD('111111')

-- 重命名  rename user 原名字 to 新名字
RENAME USER sanjin TO sanjin2

-- 用户授权   ALL PRIVILEGES 全部的权限   库，表
-- ALL PRIVILEGES 除了给别人授权，其他都能干
GRANT ALL PRIVILEGES ON *.* TO sanjin2

-- 查询权限
SHOW GRANTS FOR sanjin2  -- 查看指定用户的权限
SHOW GRANTS FOR root@localhost

-- 撤销权限 REVOKE 哪些权限，在哪个库撤销，给谁撤销
REVOKE ALL PRIVILEGES ON *.* FROM sanjin2

-- 删除用户
DROP USER sanjin2
```

### MySQL备份

* 为什么备份：
  * 保证重要数据不丢失
  * 数据转移

* MySQL数据库备份的方式
  * 直接拷贝物理文件
  * 在SQLyog这种可视化工具中手动导出
    - 在想要导出的表或者库中，右键选择备份和导出
    
      ![image-20201227190702988](C:\Users\傻兮兮乎乎\AppData\Roaming\Typora\typora-user-images\image-20201227190702988.png)

![image-20201227190747681](C:\Users\傻兮兮乎乎\AppData\Roaming\Typora\typora-user-images\image-20201227190747681.png)

### 规范数据库设计

**当数据库比较复杂的时候，我们就需要设计了**

* 糟糕的数据库设计：
  * 数据冗余，浪费空间
  * 数据库插入和删除都会麻烦，异常【屏蔽使用物理外键】
  * 程序的性能差

* 良好的数据库设计：
  * 节省内存空间
  * 保证数据库的完整性
  * 方便我们开发系统

* 软件开发中，关于数据库的设计
  * 分析需求：分析业务和需要处理的数据库的需求
  * 概要设计：设计关系图 E-R图

**设计数据库的步骤（个人博客）**

- 收集信息，分析需求
  - 用户表（用户登录注销，用户的个人信息，写博客，创建分类）
  - 分类表（文章分类，谁创建的）
  - 文章表（文章的信息）
  - 友链表（友链信息）
  - 自定义表（系统信息，某个关键的字，或者某些主字段）
  - 说说表（发表心情…id ,content ,time）
- 标识实体（把需求落地到每个字段）
- 标识实体之间的关系
  - 写博客 user–>blog
  - 创建分类 user–>category
  - 关注 user–>user
  - 友链–>links
  - 评论 user–>user

#### 三大范式

**为什么需要数据规范化？**

- 信息重复
- 更新异常
- 插入异常
- 删除异常
  - 无法正常显示异常
- 删除异常
  - 丢失有效的信息

**三大范式**

* 第一范式（1NF）
  * 原子性：保证每一列不可再分
* 第二范式（2NF）
  * 前提：满足第一范式
  * 每张表只描述一件事情

* 第三范式（3NF）
  * 前提：满足第一范式和第二范式
  * 第三范式需要确保数据表中的每一列数据都和主键直接相关，而不能间接相关。（规范数据库的设计）

**规范性和性能的问题**

> 关联查询的表，不得超过三张表

- 考虑商业化的需求和目标（成本和用户体验） 数据库的性能更加重要
- 再规范性能的问题的时候，需要适当的考虑一下，规范性
- 故意给某些表加一些冗余的字段（从多表，变成单表）
- 故意增加一些计算列（从大数据量降低为小数据量的查询：索引）

DateSource接口不会变，方法就不会变