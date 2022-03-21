[toc]

## 外键(不做笔记)

- 数据库就是单纯的表，只用来存数据，只有行(数据) 和列(字段)
- 我们想使用多张表的数据，想使用外键(**程序去实现**)

## DML语言（全部记住）

DML语言：数据操作语言

- insert
- delete
- update

### 增（INSERT）

> 语法: 
>
> `insert into 表名([字段名1],[字段2],[字段3]) values ('值1'),('值2'),('值3'),(…)`

```sql
--  插入语句(添加)
-- insert into 表名([字段名1,字段2,字段3])values('值1'), ('值2'), ('值3'， ....)
INSERT INTO `grade` (`gradename`) VALUES('大四')

-- 由于主键自增我们可以省略(如果不写表的字段，他就会一一匹配)
INSERT INTO `grade` VALUES('1','dasan')

-- 一般写插入语句，我们一定要数据和字段一一对应!
-- 插入多个字段
INSERT INTO `grade` (`gradename`)
VALUES('大二'),('大一')

INSERT INTO `student`(`name`) VALUES('wangwu')

INSERT INTO `student`(`name`,`sex`,`pwd`) VALUES('lisi','n','123456')

INSERT INTO `student`(`name`,`sex`,`pwd`) 
VALUES('lisi','n','123456'),('wangwu','v','123456')
```

**注意事项:**

1. 字段和字段之间使用英文逗号隔开
2. 字段是可以省略的,但是后面的值必须要要一一对应， 不能少
3. 可以同时插入多条数据, VALUES 后面的值，需要使用，隔开即可values(),(),()…

### 删（DEKETE & TRUNCATE）

**DELETE**

> 语法：
>
> `DELETE FROM 表名 WHERE 条件`

```sql
-- 删除数据(避免这样写，会全部删除)
DELETE FROM `Student`

-- 删除指定数据
DELETE FROM `student` WHERE id = 1;
```

* delete删除的问题  ：重启数据库的现象
  * innoDB 自增列会从1开始（存在内存当中，断电即失）
  * MyISAM 继续从上一个自增量开始（存在文件中，不会丢失）

**TRUNCATE**

> 作用：
>
> 完全清一张表，表的约束和索引不会变

```sql
-- 清空student 表
TRUNCATE `student`
```

**DELETE  和  TRUNCATE 的区别**

* 相同点 : 都能删除数据，都不会删除表结构

* 不同 :
  * TRUNCATE 重新设置**自增列计数器会归零**
  * TRUNCATE不会影响事务

```sql
CREATE TABLE test(
	id INT(10) NOT NULL AUTO_INCREMENT,
	`name` VARCHAR(20) NOT NULL,
	PRIMARY KEY (`id`)
)ENGINE=INNODB DEFAULT CHARSET=utf8

INSERT INTO `test`(`name`) VALUES('1'),('2'),('3')

DELETE FROM `test`

TRUNCATE `test`
1234567891011
```



### 改（update）

>  语法：
>
> `UPDATE 表名 SET name=value WHERE 条件`

```sql
-- 修改学员名字,带了简介
UPDATE `student` SET `name`='狂神' WHERE id < 2;

-- 不指定条件的情况下，会改动所有表!
UPDATE `student` SET `name`='长江7号'

-- 修改多个属性,逗号隔开
UPDATE `student` 
SET `name`='狂神',`email`='24736743@qq.com'
WHERE id = 1;

--语法:
-- UPDATE 表名 set name = value, [name = value,....] where [条件]

UPDATE `student` 
SET `pwd`='654321'
WHERE `name`='长江7号' AND `sex`='n'
```

* 条件:  
  * where子句运算符id 等于某个值，大于某个值，在某个区间内修改…

* 操作符会返向布尔值

| 操作符       | 含义   | 范围      | 结果  |
| ------------ | ------ | --------- | ----- |
| =            | 等于   | 5=6       | false |
| <>或!=       | 不等于 | 5<>6      | true  |
| >            |        |           |       |
| <            |        |           |       |
| >=           |        |           |       |
| <=           |        |           |       |
| BETWEEN…AND… | 范围   | [2.5]     |       |
| AND          | 与     | 5>1and6>7 | fasle |
| OR           | 或     | 5>1and6>7 | true  |

* 注意：
  * name是数据库的列,尽量带上
  * 条件,筛选的条件,如果没有指定,则会修改所有的列
  * value, 是一个具体的值，也可以是一 个变量
  * 多个设置的属性之间，使用英文逗号隔开

## DQL查询数据（重点!!!）

**Data Query Language :数据查询语言**

- 所有的查询操作都用它 Select
- 简单的查询，复杂的查询它都能做~
- **数据库中最核心的语言，最重要的语句**
- 使用频率最高的语句

**select完整语法**

```mysql
select [ALL | DISTINCT]
{* | table.* | [ table.filed1 [as alias1] [, table.filed1 [as alias1]] [, ...] ]}
from table_name [as table_alias]
	[left | right | inner join table_name2] -- 联合查询
	[where ...] -- 指定结果需要满足的条件
	[group by ...] -- 指定结果按照哪几个字段来分组
	[having ...] -- 过滤分组的记录必须满足的次要条件
	[order by ... [ASC|DESC]] -- ASC：递增，从低到高排。 DESC：递减，从高到低排
	[limit { [offset,]  row_count | row_countoffset offset }] -- 指定查询的记录从哪条至哪条
	
-- 顺序很重要
-- select 去重 要查询的字段 from 表 （注意：表和字段可以取别名）
-- xxx join 要连接的表 on 等值判断
-- where (具体的值，子查询语句)
-- Group By （通过哪个字段来分组）
-- Having （过滤分组后的信息，条件和where是一样的，位置不同）
-- Order By（通过哪个字段排序） [升序/降序]
-- Limit startlndex,pagesize (startlndex,pagesize：起始位置，筛选数量)

-- 业务层面：
-- 跨表 跨数据库
```

### 指定查询字段

> 语法：
>
> `SELECT 字段,... FROM 表`

```mysql
-- 查询 student 表中的所有
SELECT * FROM `student`

-- 查询指定信息
SELECT `studentno`,`studentname` FROM  `student`

-- 查询时起别名
SELECT `studentno` AS 学号,`studentname` AS 姓名 FROM `student` 

-- AS 可以省略
SELECT `studentno` 学号,`studentname` 姓名 FROM `student`

-- 函数CONCAT(a,b) 在数据内前加字符
SELECT CONCAT('姓名：',studentname) 新名字 FROM student
```

> 有的时候，列名字不是那么的见名知意。
> 我们起别名 **AS** ：字段名 **AS** 别名 ，表名 **AS** 别名 ...

#### 去重 distinct

```sql
-- 查询具有成绩的学生学号
SELECT `studentno` FROM `result`

-- 查询具有成绩的学生学号（去重）
SELECT DISTINCT `studentno` FROM `result`
```

#### 数据库的列（表达式）

```sql
-- 查看数据库版本
SELECT VERSION()

-- 计算器
SELECT 100*3-5 结果 -- as 被忽略了: 100*3-5 as 结果

-- 查询自增的步长
SELECT @@auto_increment_increment

-- 统一修改某列数据
SELECT `studentno`,`studentresult`+1 提分后 FROM result -- as 被忽略了
```

### where条件字句

> 作用:
>
> 检索数据中符合条件的值,，搜索的条件由一个或者多个表达式组成 ! 结果为布尔值

#### 逻辑运算符

| 运算符（常用） | 运算符|语法     | 描述                              |
| ------- | -------- |-------- | --------------------------------- |
| and |&&  | a and b  | 逻辑与，两个都为真，结果为真      |
| or| \|\| | a or b   | 逻辑或，其中-一个为真，则结果为真 |
| not| !=  | not a !a | 逻辑非，真为假，假为真!           |

```sql
-- 查询条件为区间 studentresult>63 AND studentresult<86
SELECT `studentno`,`studentresult` FROM result
WHERE studentresult>63 AND studentresult<86

-- 模糊查询（区间） studentresult BETWEEN 63 AND 100
SELECT `studentno`,`studentresult` FROM result
WHERE studentresult BETWEEN 63 AND 100

-- 查询条件（排除）
SELECT `studentno`,`studentresult` FROM result
WHERE studentno !=1000

-- 查询条件（排除）
SELECT `studentno`,`studentresult` FROM result
WHERE  NOT studentno=1000
```
####  模糊查询：比较运算符

| 运算符        | 语法              | 描述                                         |
| ------------- | ----------------- | -------------------------------------------- |
| IS NULL       | a is null         | 如果操作符为NUI,结果为真                     |
| IS NOT NULL   | a is not null     | 如果操作符不为null,结果为真                  |
| …BETWEEN…and… | a between b and c | 若a在b和c之间,则结果为真                     |
| Like          | a like b          | SQL匹配，如果a匹配b，则结果为真              |
| In            | a in(a1,a2,a3…)   | 假设a在a1,或者a2… 其中的某-个值中， 结果为真 |

```sql
-- ======================= Like =======================
-- 1ike结合 % (代表0到任意个字符)  _ (一个字符)
-- 查询姓张
SELECT `StudentNo`,`studentName`
FROM student
WHERE StudentName LIKE '张%'

-- 查询姓张的同学，名字后面只有一个字的
SELECT `StudentNo`,`StudentName`
FROM student
WHERE StudentName LIKE '张_'

-- 查询姓张的同学，名字后面只有两个字的
SELECT、StudentNo ，StudentName
FROM student
WHERE studentName LIKE '张__'

-- 查询名字中间有嘉字的同学%嘉%
SELECT‘StudentNo’, StudentName’ FROM“ student
WHERE StudentName LIKE '%嘉%'

-- ======================= BETWEEN ... AND ... ============

SELECT `studentno`,`studentresult` FROM result
WHERE studentresult BETWEEN 63 AND 100

-- ======================= In =======================
-- 查询1001, 1002，1003号学员
SELECT `StudentNo`,`StudentName`
FROM student
WHERE studentNo IN (1001, 1002 ,1003);

-- 查询在北京的学生
SELECT `StudentNo`, `StudentName`
FROM student
WHERE `address` IN ('安徽','广东深圳');

-- ======================= NOT NULL 和 NULL =======================
-- 查询地址为空的学生
SELECT `studentNo`,`studentName` 
FROM student
WHERE address=' ' OR address IS NULL

-- 查询出生日期不为空的同学
SELECT `StudentNo` ,`StudentName` 
FROM Student
WHERE `BornDate` IS NOT NULL

-- 查询没有有出生日期的同学
SELECT `StudentNo`, `StudentName` 
FROM student
WHERE `BornDate` IS NOT NULL
```

### 连表查询

<img src="E:\软工\typora笔记\mysql数据库笔记\no.3 MySQL数据管理.assets\20201013192047797.png" alt="在这里插入图片描述" style="zoom: 50%;" /><img src="E:\软工\typora笔记\mysql数据库笔记\no.3 MySQL数据管理.assets\20201013192104811.png" alt="在这里插入图片描述" style="zoom: 50%;" />

```mysql
-- ===================	连表查询 join ========================
-- 查询参加考试的学生的（学号，姓名，科目编号，分数）
SELECT * FROM `student`
SELECT * FROM `result`

/*思路
	分析需求，分析查询的字段来自哪些表，( 连接查询)
	确定使用哪种连接查询?		7种
	确定交叉点	(这两个表中哪个数据是相同的)
	判断的条件:学生表的中	studentNo =成绩表studentNo
*/
-- JION（表） ON （判断的条件）连接查询
-- where 等值查询

-- Inner Join
SELECT be.id,`name` ,`boyname` 
FROM beauty AS be
INNER JOIN boys AS bo
ON be.`boyfriend_id`=bo.`id`

-- Right Join
SELECT be.id,`name` ,`boyname` 
FROM beauty AS be
RIGHT JOIN boys AS bo
ON be.`boyfriend_id`=bo.`id`

-- Left Join
SELECT be.id,`name` ,`boyname` 
FROM beauty AS be
LEFT JOIN boys AS bo
ON be.`boyfriend_id`=bo.`id`
```

| 操作       | 描述                               |
| ---------- | ---------------------------------- |
| Inner join | 返回条件匹配的值                   |
| Left join  | 返回左表所有值，并加上符合条件的值 |
| Right join | 返回右表所有值，并加上符合条件的值 |

```sql
-- 思考题(查询了参加考试的同学信息:学号， 学生姓名，科目名，分数)
/* 思路
	分析需求，分析查询的字段来自哪些表，( 连接查询)
	确定使用哪种连接查询?		7种
	确定交叉点	(这两个表中哪个数据是相同的)
	判断的条件:学生表的中	studentNo =成绩表studentNo
*/
SELECT s.studentno,s.studentname,su.subjectname,r.studentresult
FROM `student` s
INNER JOIN `result` r
ON s.studentno = r.studentno
INNER JOIN `subject` su 
ON r.subjectno = su.subjectno 


-- 我要查询哪些数据 select
-- 从那几个表中查 FROM 表 XXX Join 连接的表 on 交叉条件
-- 假设存在一种多张表查询，慢慢来，先查询两张表然后再慢慢增加
```

**视图展示：**

跨两表：

<img src="C:\Users\傻兮兮乎乎\AppData\Roaming\Typora\typora-user-images\image-20201223164736753.png" alt="image-20201223164736753"  />

​                    

![image-20201223165514121](C:\Users\傻兮兮乎乎\AppData\Roaming\Typora\typora-user-images\image-20201223165514121.png)

跨三表：

![image-20201223174542483](C:\Users\傻兮兮乎乎\AppData\Roaming\Typora\typora-user-images\image-20201223174542483.png)

 **自连接**

> 自己的表跟自己的表连接，核心：**一张表拆为两张一样的表**

父类

| categoryid | categoryName |
| ---------- | ------------ |
| 2          | 信息技术     |
| 3          | 软件开发     |
| 5          | 美术设计     |
|            |              |

子类

| pid  | categoryid | categoryName |
| ---- | ---------- | ------------ |
| 3    | 4          | 数据库       |
| 2    | 8          | 办公信息     |
| 3    | 6          | web开发      |
| 5    | 7          | ps技术       |

操作：查询父类对应子类关系

| 父类     | 子类     |
| -------- | -------- |
| 信息技术 | 办公信息 |
| 软件开发 | 数据库   |
| 软件开发 | web开发  |
| 美术设计 | ps技术   |

```mysql
-- 查询父子信息

SELECT g.`id`,g.`name` girl,g.`boyfriend_id`,b.`id`,b.`name` boy 
FROM beauty AS g 
INNER JOIN beauty AS b
WHERE g.`boyfriend_id` = b.`id`
```

**视图效果：**

![(C:\Users\傻兮兮乎乎\AppData\Roaming\Typora\typora-user-images\image-20201223173148242.png)

<img src="C:\Users\傻兮兮乎乎\AppData\Roaming\Typora\typora-user-images\image-20201223172500394.png" alt="image-20201223172500394" style="zoom:70%;" /><img src="C:\Users\傻兮兮乎乎\AppData\Roaming\Typora\typora-user-images\image-20201223172419046.png" alt="image-20201223172419046" style="zoom: 70%;" />

**分析：**

![image-20201223173309630](C:\Users\傻兮兮乎乎\AppData\Roaming\Typora\typora-user-images\image-20201223173309630.png)

### 分页和排序

#### 排序

```mysql
-- =========== 分页limit 和排序order by ===========
-- 排序: 升序 ASC，降序 DESC 
-- 查询的结果根据成绩降序排序
SELECT s.studentNo,StudentName,SubjectName,studentresult
FROM student s
INNER JOIN result r
ON s.StudentNo = r.StudentNo
INNER JOIN SUBJECT sub
ON r.SubjectNo = sub.SubjectNo
WHERE subjectname ='高等数学-1'
ORDER BY StudentResult ASC
```

#### 分页

```mysql
-- 为什么要分页?
-- 缓解数据库压力，给人的体验更好，瀑布流

-- 分页，每页只显示五条数据
-- 语法：limit 当前页，页面大小
SELECT s.studentNo,StudentName,SubjectName,studentresult
FROM student s
INNER JOIN result r
ON s.StudentNo = r.StudentNo
INNER JOIN SUBJECT sub
ON r.SubjectNo = sub.SubjectNo
WHERE subjectname ='高等数学-1'
ORDER BY StudentResult ASC
LIMIT 5,5

-- 第一页 1imit 0,5 	(1-1) * 5
-- 第二页 limit 5,5 	(2-1) * 5
-- 第三页 limit 10,5 	(3-1) * 5
-- 第 N 页  limit (n-1) * pagesize, pagesize
-- [pagesize:页面大小]
-- [(n-1)* pagesize:起始值][n:当前页]
-- [数据总数/页面大小=总页数]
```

 **思考题**

`查询学习JAVA课程成绩排名前3的学生，并且分数要大于60的学生信息(学号，姓名，课程名称，分数)`

```mysql
SELECT s.studentno,studentname,subjectname,studentresult
FROM student s
LEFT JOIN `result` r
ON s.studentno = r.studentno
LEFT JOIN `subject` sub
ON r.subjectno = sub.subjectno
WHERE sub.subjectname LIKE '%Java%' 
AND studentresult > 60 
ORDER BY StudentResult DESC
LIMIT 0,3
```

### 子查询

> **where （这个值是查出来的）**
>
> **本质：嵌套查询**

```mysql
-- 查询数据库结构-1的所有考试结果(学号，科目编号，成绩)，降序排列
-- 方式一: 使用连接查询
SELECT `StudentNo` , r.`SubjectNo`, `StudentResult`
FROM `result` r
INNER JOIN `subject` sub
ON r.subjectno = sub.SubjectNo
WHERE SubjectName LIKE '%Java%' 
ORDER BY StudentResult DESC

-- 方式二: 使用子查询()
SELECT `StudentNo`, `SubjectNo`, `StudentResult`
FROM `result`
WHERE subjectno = (
	SELECT subjectno 
	FROM `subject` 
	WHERE SubjectName = 'Java程序设计-1'
)

-- 查询所有选数据库结构的学生学号
SELECT subjectno FROM `subject` WHERE SubjectName = 'Java程序设计-1'

-- 分数不小于80分的学生的学号和姓名
SELECT DISTINCT s.`StudentNo`, `StudentName`
FROM student s
INNER JOIN result r
ON r.StudentNo = s. StudentNo
WHERE `StudentResult`>=80

-- 在这个基础上增加一个科目，高等数学-2
-- 查询高等数学-2的编号
SELECT DISTINCT s.`StudentNo`, StudentName
FROM student s
INNER JOIN result r
ON r.StudentNo = s.StudentNo
WHERE `StudentResult` >=80 AND `SubjectNo` = (
	SELECT SubjectNo FROM `subject`
	WHERE `SubjectName`= '高等数学-2'
)

-- 查询课程为 高等数学-2 且分数不小于80分的同学的学号和姓名
SELECT s.`StudentNo`,`StudentName`
FROM student s
INNER JOIN result r
ON s.StudentNo = r.StudentNo
INNER JOIN `subject` sub
ON r.`SubjectName`='高等数学-2'
WHERE `SubjectaName`='高等数学-2' AND StudentResult >=80

-- 在改造(由里及外)
SELECT StudentNo, StudentName FROM student WHERE StudentNo IN (
	SELECT StudentNo 
    FROM result 
    WHERE StudentResult>80 
    AND SubjectNo = (
		SELECT SubjectNo FROM `subject`
		WHERE `SubjectName` = 'Java程序设计-1'
	)
)
```

### 分组和过滤

> GROUP BY
>
> 一般与聚合函数一起使用，选择计算范围
>
> HAVING
>
> 和 GROUP BY 搭配，相当于WHERE.

```mysql
-- 查询不同课程的平均分，最高分，最低分，平均分大于80
-- 核心: (根据不同的课程分组)
SELECT SubjectName, AVG(StudentResult) AS 平均分,MAX(StudentResult) AS 最高分，MIN(studentResult) AS 最低分
FROM result r
INSERT JOIN `subject` sub
ON r.`SubjectNo` = sub.`subjectNo`
GROUP BY r.subjectNo -- 通过什么字段来分组
HAVING 平均分 > 80
```

## MySQL函数

### 常用函数

```sql
-- 数学运算

SELECT ABS(-8) -- 绝对值
SELECT CEILING(9.4) -- 向上取整
SELECT FLOOR(9.4)  -- 向下取整
SELECT RAND() -- 返回0-1随机数
SELECT SIGN(-10) -- 判断一个数的符号 0-0 负数返回-1 正数返回1

-- 字符串函数
SELECT CHAR_LENGTH('2323232') -- 返回字符串长度
SELECT CONCAT('我','233') -- 拼接字符串
SELECT INSERT('java',1,2,'cccc') -- 从某个位置开始替换某个长度
SELECT UPPER('abc') 
SELECT LOWER('ABC')
SELECT REPLACE('坚持就能成功','坚持','努力')

-- 查询姓 周 的同学 ，改成邹
SELECT REPLACE(studentname,'周','邹') FROM student
WHERE studentname LIKE '周%'

-- 时间跟日期函数（记住）
SELECT CURRENT_DATE() -- 获取当前日期
SELECT CURDATE() -- 获取当前日期
SELECT NOW() -- 获取当前日期
SELECT LOCATIME()  -- 本地时间
SELECT SYSDATE()  -- 系统时间

SELECT YEAR(NOW())
SELECT MONTH(NOW())
SELECT DAY(NOW())
SELECT HOUR(NOW())
SELECT MINUTE(NOW())
SELECT SECOND(NOW())

-- 系统
SELECT SYSTEM_USER()
SELECT USER()
SELECT VERSION()
```

### 聚合函数（常用）

| 函数名称 | 描述   |
| -------- | ------ |
| COUNT()  | 计数   |
| SUM()    | 求和   |
| AVG()    | 平均值 |
| MAX()    | 最大值 |
| MIN()    | 最小值 |
| …        | …      |

```sql
-- 都能够统计表中的数据 (想查询一个表中有多少个记录，就使用这个count() )
SELECT COUNT( BornDate ) FROM student; -- Count(字段)，会忽略所有的nu11 值
SELECT COUNT(*) FROM student; -- Count (*) ，不会忽略nu11值，本质计算行数
SELECT COUNT(1) FROM result; -- Count (1) ，不会忽略所有的nu11值，本质计算行数
                     
SELECT SUM( StudentResult ) AS 总和 FROM result
SELECT AVG( studentResult ) AS 平均分 FROM result
SELECT MAX( studentResult ) AS 最高分 FROM result
SELECT MIN( studentResult ) AS 最低分 FROM result
```

