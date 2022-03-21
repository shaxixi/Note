[toc]

## 数据库(DB, DataBase)

* 概念：数据仓库、**软件**、安装在操作系统( window 、linux,、mac…）之上! 可以存储大量的数据。

* 作用：存储数据，管理数据。

## 数据库分类

* **关系型数据库: (SQL)**（如：信息）
  * MySQL, Oracle, Sql Server, DB2, SQLlite
  * 通过表和表之间,行和列之间的**关系**进行数据的存储， 学员信息表,考勤表, …

* **非关系型数据库：NoSQL、 Not Only**（如：定位）
  * Redis, MongDB
  * 非关系型数据库，对象存储，通过**对象**的自身的属性来决定

![image-20201217165052009](C:\Users\傻兮兮乎乎\AppData\Roaming\Typora\typora-user-images\image-20201217165052009.png)

* **DBMS(数据库管理系统)**

  * 数据库的管理软件，科学有效的管理我们的数据。维护和获取数据;

  * MySQL,数据库管理系统!

    ![image-20201217164700184](C:\Users\傻兮兮乎乎\AppData\Roaming\Typora\typora-user-images\image-20201217164700184.png)

## MySQL简介

​	MySQL是一个关系型数据库管理系统

​	前世:瑞典MySQL AB公司

​	今生:属于Oracle旗下产品

​	MySQL是最好的RDBMS (Relational Database Management System,关系数据库管理系统)应用软件之一。

​	开源的数据库软件~

​	体积小、速度快、总体拥有成本低，招人成本比较低

​	中小型网站、或者大型网站，集群!

## 安装MySQL

* 安装建议:

  1、尽量不要使用exe，注册表

  2、尽可能使用压缩包安装（卸载方便）~

* 安装**命令行**形式的数据库流程

  <https://www.bilibili.m/video/BV1NJ411J79W?p=3>

  下载地址

  <https://dev.mysql.com/downloads/mysql>

  ~~~mysql
  -- -------my.ini文件
  [mysqld]
  basedir=D:\Environment Files\mysql-5.7.32-winx64\mysql-5.7.32-winx64\
  datadir=D:\Environment Files\mysql-5.7.32-winx64\mysql-5.7.32-winx64\data\
  port=3306
  skip-grant-tables
  
  -- --------cmd
  Microsoft Windows [版本 10.0.18363.1256]
  (c) 2019 Microsoft Corporation。保留所有权利。
  
  C:\Windows\system32>cd /d D:\Environment Files\mysql-5.7.32-winx64\mysql-5.7.32-winx64\bin
  
  D:\Environment Files\mysql-5.7.32-winx64\mysql-5.7.32-winx64\bin>mysqld -install
  Service successfully installed.
  
  D:\Environment Files\mysql-5.7.32-winx64\mysql-5.7.32-winx64\bin>mysqld --initialize-insecure --user=mysql
  
  D:\Environment Files\mysql-5.7.32-winx64\mysql-5.7.32-winx64\bin>net start mysql
  MySQL 服务正在启动 .
  MySQL 服务已经启动成功。
  ~~~

  

  ~~~sql
  /*在命令行中连接数据库所需要用的语句*/
  
  mysql -u root -p123456 -- 链接数据库
  
  update mysql.user set authentication_string=password('123456') 
  where user='root' and host = 'localhost';	-- 修改用户密码
  
  flush privileges;	-- 刷新权限
  ~~~

  ~~~sql
  /*在命令行中操作数据库*/
  
  show 'databases'; -- 查看所有的数据库
  
  use 'school';	-- 切换数据库 use命令	
  
  show 'tables';	--	查看表
  
  describe 'student';	--  显示表结构
  
  create database 'tempbase';	-- 创建数据库
  
  exit; -- 退出链接
  -------------------------------------------------
  --注释和注意事项
  
  --1. 所有的语句都要使用 ; 结尾
  
  --2.  -- 单行注释（SQL）
  
  --3. /* SQL的多行注释  */
  ~~~

* 安装**可视化**的数据库流程
  <https://www.bilibili.com/video/BV1NJ411J79W?p=4>

  下载包
  <https://pan.baidu.com/s/1D_iRna8V90omfHsKHyeBtg>	提取码: bqht

  

