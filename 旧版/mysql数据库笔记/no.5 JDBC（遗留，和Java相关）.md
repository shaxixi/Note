## JDBC (重点)

### 数据驱动

> 我们的程序会通过数据库驱动，和数据库打交道!



![在这里插入图片描述](E:\软工\typora笔记\mysql数据库笔记\no.5 JDBC（遗留，和Java相关）.assets\20201013213011818.png)


### JDBC

> SUN公司为了简化开发人员的(对数据库的统一) 操作,提供了一个Uava操作数据库的)规范,俗称JDBC
>
> 这些规范的实现由具体的厂商去做~ 
>
> 对于开发人员来说，我们只需要掌握JDBC接口的操作即可!
>
> 数据库驱动包 mysql-connector-java-5.1.49.jar



![在这里插入图片描述](E:\软工\typora笔记\mysql数据库笔记\no.5 JDBC（遗留，和Java相关）.assets\20201013213108723.png)



### 第一个JDBC程序

1. 创建测试数据库

```sql
CREATE DATABASE jdbcStudy CHARACTER SET utf8 COLLATE utf8_general_ci;

USE jdbcStudy;

CREATE TABLE users (
    id INT PRIMARY KEY,
    NAME VARCHAR(40) ,
    PASSWORD VARCHAR(40),
    email VARCHAR(60) ,
    birthday DATE
);

INSERT INTO users (id, NAME, PASSWORD, email, birthday)
VALUES(1, ' zhansan', ' 123456', 'zs@sina.com', ' 1980-12-04'),
(2, 'lisi', '123456','lisi@sina.com', '1981-12-04'),
(3, ' wangwu','123456', 'wangwu@sina.com', '1979-12-04') ;
```

2. 创建一个普通项目

3. 导入数据库驱动

![在这里插入图片描述](E:\软工\typora笔记\mysql数据库笔记\no.5 JDBC（遗留，和Java相关）.assets\20201013213457274.png)

3. 编写测试代码

程序描述：

1）加载JDBC驱动

2）拿到数据库连接、登录账号、密码

3）驱动器中方法，拿到连接、账户、密码后，创建连接对象

4）连接对象创建提交sql的对象

5）编写sql

6）提交sql对象获得编写sql，执行提交，返回结果

7）输出结果

8）关闭结果对象，关闭提交sql对象、关闭连接

```java
package com.csh.test01;

import java.sql.*;

// JDBC连接测试
public class JdbcFirstDemo {
    public static void main(String[] args) throws ClassNotFoundException, SQLException {

        // 1. 加载驱动
        Class.forName("com.mysql.jdbc.Driver"); // 固定写法，加载驱动

        // 2. 用户信息和url
        String url = "jdbc:mysql://localhost:3306/jdbcstudy?useUnicode=true&characterEncoding =utf8&useSSL=true";
        String username = "root";
        String password = "123456";

        // 3. 连接成功，保存数据库对象 Connection 代表数据库
        Connection connection = DriverManager.getConnection(url,username,password);

        // 4. 执行SQl对象的 Statement（声明）指向SQL语句
        Statement statemant = connection.createStatement();

        // 5.获取SQL语句封装的对象，可能存在结果，查看结果
        String sql = "SELECT * FROM users";

        ResultSet resultSet = statemant.executeQuery(sql);

        while(resultSet.next()){
            System.out.println("id=" + resultSet.getObject("id"));
            System.out.println("name=" + resultSet.getObject("NAME"));
            System.out.println("pwd=" + resultSet.getObject("PASSWORD"));
            System.out.println("email=" + resultSet.getObject("email"));
            System.out.println("birth=" + resultSet.getObject( "birthday"));
            System.out.println("****************************************************");
        }
        // 6. 释放连接
        resultSet.close();
        statemant.close();
        connection.close();
    }
}
```

> DriverManager

```java
// DriverManager.registerDriver(new com.mysql.jdbc.Driver());
Class.forName("com.mysql.jdbc.Driver"); // 固定写法，加载驱动
Connection connection =  DriverManager.getConnection(url,username,password);

// connection代表数据库
// 数据库设置自动提交
// 事务提交
// 事务滚回
connection.ro11back();
connection.commit O;
connection.setAutocommit();
```

> URL

```java
string ur1 = "jdbc:mysql://localhost:3306/jdbcstudy?useUnicode=true&characterEncoding=utf8&usessL=true";
    
// mysq1 -- 3306
// 协议://主机地址:端口号/数据库名?参数1&参数2&参数3
// oralce -- 1521
// jdbc:oracle: thin:@1ocalhost:1521:sid
```

> Statement 执行SQL的对象 PrepareStatement 执行SQL的对象

```java
string sql = "SELECT * FROM users"; // 编写SQL

statement.executeQuery(); // 查询操作返回Resu]tset
statement.execute(); // 执行任何SQL
statement.executeUpdate(); // 更新、插入、删除。都是用这个，返回一个受影响的行数
```

> ResultSet 查询的结果集：封装了所有的查询结果

**获取指定的数据类型**

```java
resultset.getobject(); // 在不知道列类型的情况下使用
// 如果知道列的类型就使用指定的类型
resultset.getstring();
resultset.getInt() ;
resultset.getF1oat();
resultSet.getDate();
resultset.getobject();
```

**遍历、指针**

```java
resultset.beforeFirst(; // 移动到最前面
resultset.afterlast(); // 移动到最后面
resultset.next(); // 移动到下一个数据
resultset.previous(); // 移动到前一行
resultset.absolute(row); // 移动到指定行
```

> 释放资源

```java
//6、 释放连接
resultset.close();
statement.close();
connection.close(); // 耗资源，用完关掉!
```

### statement对象

**Jdbc中的statement对象用于向数据库发送SQL语句，想完成对数据库的增删改查，只需要通过这个对象向数据库发送增删改查语句即可。**

Statement对象的executeUpdate方法，用于向数据库发送增、删、改的sq|语句， executeUpdate执行完后， 将会返回一个整数(即增删改语句导致了数据库几行数据发生了变化)。

Statement.executeQuery方法用于向数据库发生查询语句，executeQuery方法返回代表查询结果的ResultSet对象。

> CRUD操作-create

使用executeUpdate(String sql)方法完成数据添加操作，示例操作：

```java
 Statement statement = connection.createStatement();
        String sql = "insert into user(...) values(...)";
        int num = statement.executeUpdate(sql);
        if(num>0){
            System.out.println("插入成功");
        }

1234567
```

> CRUD操作-delete

使用executeUpdate(String sql)方法完成数据删除操作，示例操作：

```java
Statement statement = connection.createStatement();
        String sql = "delete from user where id =1";
        int num = statement.executeUpdate(sql);
        if(num>0){
            System.out.println("删除成功");
        }

1234567
```

> CURD操作-update

使用executeUpdate(String sql)方法完成数据修改操作，示例操作：

```java
Statement statement = connection.createStatement();
        String sql = "update user set name ='' where name = ''";
        int num = statement.executeUpdate(sql);
        if(num>0){
            System.out.println("修改成功");
        }

1234567
```

> CURD操作-read

使用executeUpdate(String sql)方法完成数据查询操作，示例操作：

```java
Statement statement = connection.createStatement();
        String sql = "select * from  user where id =1";
        ResultSet rs= statement.executeQuery(sql);
        if(rs.next()){
            System.out.println("");
        }

1234567
```

> 代码实现

1.提取工具类

```java
package com.kuang.lesson02.utils;

import java.io.IOException;
import java.io.InputStream;
import java.sql.*;
import java.util.Properties;

public class JdbcUtils {
    private static String driver = null;
    private static String url = null;
    private static String username = null;
    private static String password = null;
    static {
        try{
            InputStream in = JdbcUtils.class.getClassLoader().getResourceAsStream("db.properties");
            Properties properties = new Properties();
            properties.load(in);
            driver=properties.getProperty("driver");
            url=properties.getProperty("url");
            username=properties.getProperty("username");
            password=properties.getProperty("password");

            //1.驱动只用加载一次
            Class.forName(driver);

        } catch (IOException e) {
            e.printStackTrace();
        } catch (ClassNotFoundException e) {
            e.printStackTrace();
        }
    }


    //2.获取连接
    public static Connection getConnection() throws Exception{
        return DriverManager.getConnection(url, username, password);
    }
    //3.释放资源
    public static void release(Connection conn, Statement st, ResultSet rs) throws SQLException {

        if(rs!=null){
            rs.close();
        }
        if (st!=null){
            st.close();
        }
        if(conn!=null){
            conn.close();
        }

    }
}


123456789101112131415161718192021222324252627282930313233343536373839404142434445464748495051525354
```

2.编写增删改的方法，`exectueUpdate`

```java
package com.kuang.lesson02.utils;

import java.sql.Connection;
import java.sql.ResultSet;
import java.sql.Statement;

import static com.kuang.lesson02.utils.JdbcUtils.*;

public class TestInnsert {
    public static void main(String[] args){
        Connection conn =null;
        Statement st = null;
        ResultSet rs =null;



        try {
             conn = getConnection();//获取连接
            st = conn.createStatement();//获取SQL执行对象
            String sql = "INSERT INTO users(id,`NAME`,`PASSWORD`,`email`,`birthday`)" +
                    "VALUES(5,'sanjin','123456','233223@qq.com','2020-01-01')";

            int i = st.executeUpdate(sql);
            if(i>0){
                System.out.println("插入成功");
            }
        JdbcUtils.release(conn,st,rs);
        } catch (Exception e) {
            e.printStackTrace();
        }
    }

}


1234567891011121314151617181920212223242526272829303132333435
```

3.查询 `executeQuery`

```java
package com.kuang.lesson02.utils;

import java.sql.Connection;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.sql.Statement;

import static com.kuang.lesson02.utils.JdbcUtils.*;

public class TestInnsert {
    public static void main(String[] args) throws SQLException {
        Connection conn =null;
        Statement st = null;
        ResultSet rs =null;



        try {
             conn = getConnection();//获取连接
            st = conn.createStatement();//获取SQL执行对象
            String sql = "select * from users";
            rs=st.executeQuery(sql);//查询完毕返回结果集

            while (rs.next()){
                System.out.println(rs.getString("NAME"));
            }
        JdbcUtils.release(conn,st,rs);
        } catch (Exception e) {
            e.printStackTrace();
        }finally {
            JdbcUtils.release(conn,st,rs);
        }
    }

}

123456789101112131415161718192021222324252627282930313233343536
```

> SQL注入问题

sql存在漏洞，会被攻击导致数据泄露 **SQL会被拼接 or**

```java
package com.kuang.lesson02.utils;

import java.sql.Connection;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.sql.Statement;

import static com.kuang.lesson02.utils.JdbcUtils.getConnection;

public class SQL注入 {
    public static void main(String[] args) {


        //SQL注入
 login("' or '1=1","123456");
    }
    public static void login(String name,String password){


        Connection conn =null;
        Statement st = null;
        ResultSet rs =null;



        try {
            conn = getConnection();//获取连接
            st = conn.createStatement();//获取SQL执行对象
            String sql = "select * from users where `NAME`='"+ name +"'  AND `PASSWORD`='"+ password +"'" ;
            rs=st.executeQuery(sql);//查询完毕返回结果集

            while (rs.next()){
                System.out.println(rs.getString("NAME"));
            }
            JdbcUtils.release(conn,st,rs);
        } catch (Exception e) {
            e.printStackTrace();
        }finally {
            try {
                JdbcUtils.release(conn,st,rs);
            } catch (SQLException throwables) {
                throwables.printStackTrace();
            }
        }
    }
}


123456789101112131415161718192021222324252627282930313233343536373839404142434445464748
```

### 10.5 PreparedStatement对象

PreparedStatement 可以防止SQL注入 ，效率更高。

1. 新增
2. 删除
3. 查询

```java
package com.kuang.lesson03;

import com.kuang.lesson02.utils.JdbcUtils;

import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.SQLException;

public class Test {
    public static void main(String[] args) {
        Connection connection= null;
        PreparedStatement pstm=null;
        try {

            connection = JdbcUtils.getConnection();
            //区别
            //使用问好占位符代替参数
            String sql = "insert into users(id,`NAME`) values(?,?)";
            pstm = connection.prepareStatement(sql);//预编译sql，先写sql然后不执行
            //手动赋值
            pstm.setInt(1,8);
            pstm.setString(2,"SANJIN");

            //执行
            int i = pstm.executeUpdate();
            if (i>0){
                System.out.println("插入成功");
            }
        } catch (Exception e) {
            e.printStackTrace();
        }finally {
            try {
                JdbcUtils.release(connection,pstm,null);
            } catch (SQLException throwables) {
                throwables.printStackTrace();
            }
        }
    }
}

1234567891011121314151617181920212223242526272829303132333435363738394041
```

防止SQL注入本质，传递字符 带有“ ”，转义字符会被转义

### 10.6 使用IDEA连接数据库

![在这里插入图片描述](E:\软工\typora笔记\mysql数据库笔记\no.5 JDBC（遗留，和Java相关）.assets\20200708013123797.png)

连接成功后，可以选择数据库

![在这里插入图片描述](E:\软工\typora笔记\mysql数据库笔记\no.5 JDBC（遗留，和Java相关）.assets\20200708013148715.png)
双击数据库

![在这里插入图片描述](E:\软工\typora笔记\mysql数据库笔记\no.5 JDBC（遗留，和Java相关）.assets\20200708013157507.png)

更新数据

![在这里插入图片描述](E:\软工\typora笔记\mysql数据库笔记\no.5 JDBC（遗留，和Java相关）.assets\20200708013204924.png)
编写sql代码的地方
![在这里插入图片描述](E:\软工\typora笔记\mysql数据库笔记\no.5 JDBC（遗留，和Java相关）.assets\20200708013211958.png)

### 10.7 JDBC事务

**要么都成功，要么都失败**

> ACID原则

原子性：要么全部完成，要么都不完成

一致性：结果总数不变

隔离性：**多个进程互不干扰**

持久性：一旦提交不可逆，持久化到数据库了

隔离性的问题：

脏读： 一个事务读取了另一个没有提交的事务

不可重复读：在同一个事务内，重复读取表中的数据，表发生了改变

虚读（幻读）：在一个事务内，读取到了别人插入的数据，导致前后读出来的结果不一致

> 代码实现

1. 开启事务`conn.setAutoCommit(false);`

2. 一组业务执行完毕，提交事务

3. 可以在catch语句中显示的定义回滚，但是默认失败会回滚

   ```java
   package com.kuang.lesson04;
   
   import com.kuang.lesson02.utils.JdbcUtils;
   
   import java.sql.Connection;
   import java.sql.PreparedStatement;
   import java.sql.ResultSet;
   import java.sql.SQLException;
   
   public class Action {
       public static void main(String[] args) {
   
           Connection conn =null;
           PreparedStatement ps = null;
           ResultSet rs = null;
   
           try {
               conn = JdbcUtils.getConnection();
               //关闭数据库的自动提交功能， 开启事务
               conn.setAutoCommit(false);
               //自动开启事务
               String sql = "update account set money = money-500 where id = 1";
               ps =conn.prepareStatement(sql);
               ps.executeUpdate();
               String sql2 = "update account set money = money-500 where id = 2";
               ps=conn.prepareStatement(sql2);
               ps.executeUpdate();
   
               //业务完毕，提交事务
               conn.commit();
               System.out.println("操作成功");
           } catch (Exception e) {
               try {
                   //如果失败，则默认回滚
                   conn.rollback();//如果失败，回滚
               } catch (SQLException throwables) {
                   throwables.printStackTrace();
               }
               e.printStackTrace();
           }finally {
               try {
                   JdbcUtils.release(conn,ps,rs);
               } catch (SQLException throwables) {
                   throwables.printStackTrace();
               }
           }
       }
   }
   
   12345678910111213141516171819202122232425262728293031323334353637383940414243444546474849
   ```

### 10.8数据库连接池

数据库连接–执行完毕–释放

连接–释放 十分浪费资源

**池化技术**： 准备一些预先的资源，过来就连接预先准备好的

常用连接数 100

最少连接数：100

最大连接数 ： 120 业务最高承载上限

排队等待，

等待超时：100ms

编写连接池，实现一个接口 DateSource

> 开源数据源实现(拿来即用)

DBCP

C3P0

Druid: 阿里巴巴

使用了这些数据库连接池之后，我们在项目开发中就不需要编写连接数据库的代码了

> DBCP

需要用到的jar包

dbcp.ar

> C3P0

> 结论

无论使用什么数据源，本质是不变的，