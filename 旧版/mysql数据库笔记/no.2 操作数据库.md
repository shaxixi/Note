[toc]

## 操作数据库

![image-20201217182258248](C:\Users\傻兮兮乎乎\AppData\Roaming\Typora\typora-user-images\image-20201217182258248.png)

* 创建数据库

  ~~~mysql
  CREATE DATABASE [IF NOT EXISTS] school
  ~~~

* 删除数据库

  ~~~mysql
  DROP DATABASE [IF EXISTS] school;
  ~~~

* 使用数据库

  ~~~mysql
  USE school;	-- 如果你的表名或者字段名是一个特殊字符，就需要带 `` 
  ~~~

* 查看数据库

  ~~~mysql
  SHOW DATABASES; --查看所有的数据库
  ~~~



## 数据库的数据类型/列类型

![image-20201217170355454](C:\Users\傻兮兮乎乎\AppData\Roaming\Typora\typora-user-images\image-20201217170355454.png)

**数值**

| 类型      | 描述               | 大小                          |
| --------- | ------------------ | ----------------------------- |
| tinyint   | 十分小的数据       | 1个字节                       |
| smallint  | 较小的数据         | 2个字节                       |
| mediumint | 中等大小的数据     | 3个字节                       |
| **int**   | **标准的整数**     | **4个字节 常用的int**         |
| bigint    | 较大的数据         | 8个字节                       |
| float     | 浮点数             | 4个字节                       |
| double    | 浮点数             | 8个字节 (精度问题! )          |
| decimal   | 字符串形式的浮点数 | 金融计算的时候，一般用decimal |

**字符串**

| 类型     | 描述                                 | 大小              |
| -------- | ------------------------------------ | ----------------- |
| char     | 字符串，固定大小                     | 0~255             |
| varchar  | 可变字符串，类似（常用的变量）String | 0~65535           |
| tinytext | 微型文本                             | 2^8-1             |
| text     | 文本串                               | 2^16-1 保存大文本 |

**时间日期**

| 类型      | 描述                                        |
| --------- | ------------------------------------------- |
| date      | YYYY-MM-DD, 日期格式                        |
| time      | HH: mm: SS时间格式                          |
| datetime  | YYYY-MM-DD HH: mm: ss最常用的时间格式       |
| timestamp | 时间戳，1970.1.1 到现在的毫秒数!也较为常用! |
| year      | 年份表示                                    |

**null**

> 没有值，未知
>
> 不要使用NULL 进行运算，结果为NULL



## 数据库的字段属性（重要）

![image-20201217170458027](C:\Users\傻兮兮乎乎\AppData\Roaming\Typora\typora-user-images\image-20201217170458027.png)

**Unsigned :**

- 无符号的整数
- 声明了该列不能声明为负数

**zerofill:**

- 0填充的
- 不足的位数，使用0来填充，int (3)，5 — 005

**自增**

- 通常理解为自增，自动在上一条记录的基础上+ 1 (默认)
- 通常用来设计**唯一-的主键**~ index, 必须是整数类型
- 可以自定义设计主键自增的起始值和步长

**非空NUII not null**

- 假设设置为not null ，如果不给它赋值，就会报错
- NUII，如果不填写值，默认就是null

**默认:**

- 设置默认的值!
- sex，默认值为男，如果不指定该列的值，则会有默认的值!

**长度：**

* 设置最小长度
* 如果实际长度超过设置的长度，则以实际长度为准；否则要么前置加0，要么空格，以达到长度为设置的长度

## 创建数据库表(重点)（实操）

![image-20201217181209666](C:\Users\傻兮兮乎乎\AppData\Roaming\Typora\typora-user-images\image-20201217181209666.png)

~~~mysql
-- 格式
CREATE TABLE [IF NOT EXISTS] `表名`（
`字段名` 列类型(长度) [属性] [索引] [注释],
`字段名` 列类型(长度) [属性] [索引] [注释],
...
`字段名` 列类型(长度) [属性] [索引] [注释]
）[表类型] [表的字符集设置] [注释]
~~~

~~~mysql
-- 目标:创建一个schoo1数据库
-- 创建学生表(列,字段) 使用SQL创建
-- 学号int, 登录密码varchar(20), 姓名, 性别varchar(2)，出生日期(datatime),家庭住址, email

/*---------------------------------------------*/
CREATE TABLE `student` (
  `id` INT(4) NOT NULL AUTO_INCREMENT COMMENT '学号',
  `name` VARCHAR(30) NOT NULL DEFAULT '无名氏' COMMENT '名字',
  `age` INT(2) DEFAULT NULL COMMENT '性别',
  `pwd` INT(20) NOT NULL DEFAULT '123456' COMMENT '密码',
  `sex` VARCHAR(2) DEFAULT '男' COMMENT '性别',
  `birthday` INT(8) DEFAULT '19990202' COMMENT '生日',
  PRIMARY KEY (`id`)
) ENGINE=INNODB DEFAULT CHARSET=utf8

/*---------------------------------------------*/
-- 注意点!
-- 使用英文()
-- ！！！表的名称和字段尽量使用 `` 括起来 , 字符串使用单引号 '' 括起来!
-- 所有的语句后面加','(英文的)，最后一个不用加--

-- AUTO_INCREMENT 自增
-- DEFAULT 默认
-- NOT NULL 非空
-- COMMENT 注释
-- PRIMARY KEY 主键，一般一个表只有一个主键! 
-- ENGINE 引擎
-- DEFAULT CHARSET 默认字符集
~~~

~~~mysql
/* 先用可视化操作，再逆向查看操作数据库的代码 */

-- 查看创建表语句
SHOW CREATE TABLE `student`;

-- 查看创建数据库语句
SHOW CREATE DATABASE `school`;

-- 查看表结构
DESC `student1`;
~~~

## 数据库引擎的类型（主要为两个）

![image-20201217173905843](C:\Users\傻兮兮乎乎\AppData\Roaming\Typora\typora-user-images\image-20201217173905843.png)

|            | MYISAM（默认使用） | INNODB（早些年使用的） |
| ---------- | ------------------ | ---------------------- |
| 事务支持   | 不支持             | 支持                   |
| 数据行锁定 | 不支持             | 支持                   |
| 外键约束   | 不支持             | 支持                   |
| 全文索引   | 支持               | 不支持                 |
| 表空间大小 | 较小               | 较大，约为2倍          |

常规使用操作:

- MYISAM 节约空间，速度较快
- INNODB 安全性高，事务的处理，多表多用户操作



MySQL引擎在物理文件上的区别：

​	所有的数据库文件都存在data目录下，一个文件夹就对应一一个数据库，**本质还是文件的存储!**

- InnoDB 对应的文件
  - *.frm 表结构的定义文件
  - *.ibd 索引和数据
- MYISAM对应文件
  - *.frm 表结构的定义文件
  - *.MYD 数据文件(data)
  - *.MYI 索引文件(index)

## 设置数据库字符集编码

![image-20201217175226778](C:\Users\傻兮兮乎乎\AppData\Roaming\Typora\typora-user-images\image-20201217175226778.png)

~~~mysql
CHARTSET=UTF8
~~~

不设置的话，会是mysql默认的字符集编码-（不支持中文）

* 还可以在my.ini中配置默认的编码 (不建议使用)

  ~~~mysql
  character-set-server=utf8
  ~~~



## 修改&删除表结构

![image-20201217182130648](C:\Users\傻兮兮乎乎\AppData\Roaming\Typora\typora-user-images\image-20201217182130648.png)

**修改**

~~~mysql
-- 修改表名: ALTER TABLE	旧表名	RENAME AS	新表名
ALTER TABLE student RENAME AS student1;
 
-- 增加表的字段: ALTER TABLE表名ADD字段名列属性
ALTER TABLE student1 ADD telephone INT(20);

-- 修改表的字段 (重命名，修改约束!) 

-- ALTER TABLE	表名	MODIFY 字段名	列属性[]
ALTER TABLE student1 MODIFY age VARCHAR(20); -- 修改约束(修改列类型和长度)

-- ALTER TABLE	表名	CHANGE 旧名字	新名字 列属性[]
ALTER TABLE student1 CHANGE age age1 VARCHAR(20); -- 字段重名名(重命名) (修改列类型和长度·不建议使用)

-- 删除表的字段: ALTER TABLE 表名 DROP 字段名
ALTER TABLE student1 DROP sex;

~~~

**删除**

```mysql
-- 	删除表(如果表存在再删除)
DROP TABLE IF EXISTS student1;
```

**注意点:**

- `` 字段名，使用这个包裹，目的与关键字区分!
- 注释 `/**/` `--`
- sq| 关键字大小写不敏感，建议大家写小写
- 所有的符号全部用英文!

~~~mysql
/* 本章节相关的一系列操作代码 */
CREATE DATABASE IF NOT EXISTS school
SHOW DATABASES;
USE school;
DROP DATABASE IF EXISTS school;
CREATE TABLE IF NOT EXISTS `student` (
`id` INT(4) NOT NULL AUTO_INCREMENT COMMENT '学号',
`name` VARCHAR(30) NOT NULL DEFAULT '无名氏' COMMENT '名字',
`age` INT(2) COMMENT '性别',
`pwd` INT(20) NOT NULL DEFAULT '123456' COMMENT '密码',
`sex` VARCHAR(2) DEFAULT '男' COMMENT '性别',
`birthday` INT(8) DEFAULT '19990202' COMMENT '生日',
PRIMARY KEY(`id`)
)ENGINE=INNODB DEFAULT CHARSET = utf8
SHOW CREATE TABLE `student`;
-- 复制了代码
CREATE TABLE `student` (
  `id` INT(4) NOT NULL AUTO_INCREMENT COMMENT '学号',
  `name` VARCHAR(30) NOT NULL DEFAULT '无名氏' COMMENT '名字',
  `age` INT(2) DEFAULT NULL COMMENT '性别',
  `pwd` INT(20) NOT NULL DEFAULT '123456' COMMENT '密码',
  `sex` VARCHAR(2) DEFAULT '男' COMMENT '性别',
  `birthday` INT(8) DEFAULT '19990202' COMMENT '生日',
  PRIMARY KEY (`id`)
) ENGINE=INNODB DEFAULT CHARSET=utf8
-- 
SHOW CREATE DATABASE `school`;
-- 复制了代码
CREATE DATABASE `school` /*!40100 DEFAULT CHARACTER SET latin1 */
-- 
DESC `student1`;
ALTER TABLE student RENAME AS student1;
ALTER TABLE student1 ADD telephone INT(20);
ALTER TABLE student1 MODIFY age VARCHAR(20);
ALTER TABLE student1 CHANGE age age1 VARCHAR(20);
ALTER TABLE student1 DROP sex;
DROP TABLE IF EXISTS student1;
~~~



