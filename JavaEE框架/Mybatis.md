[toc]

# Mybatis

* Mybatis 是一个半自动化的持久化层框架，原iBatis。
     ![image-20211214161849595](E:\软工\typora笔记\JavaEE框架\Mybatis.assets\image-20211214161849595.png)

* 解决问题：sql夹在Java代码块里，耦合度高导致硬编码内伤；
                     手动编写jdbc代码和设置参数以及获取结果集；
  解决方式：使用xml 或注解 用于配置和原始映射；

* Mybatis VS Hibernate

  | Mybatis     | Hibernate                            |
  | ----------- | ------------------------------------ |
  | 半自动化    | 全自动化                             |
  | 手动编写sql | 内部自动生产的SQL                    |
  | 手动映射    | 大量字段的POJO进行部分映射时比较困难 |

  ![image-20211214161906854](E:\软工\typora笔记\JavaEE框架\Mybatis.assets\image-20211214161906854.png)

## 基本流程

1. 导入jar包： 官方文档： https://github.com/mybatis/mybatis-3/
   \> mysql-connector-java-5.1.37-bin.jar
   \> mybatis-3.4.1.jar
   \> log4j.jar

2. 编写mybatis配置文件：包含数据库连接池信息，事务管理器信息等...系统运行环境信息

   ~~~xml
   <?xml version="1.0" encoding="UTF-8" ?>
   <!DOCTYPE configuration
    PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
    "http://mybatis.org/dtd/mybatis-3-config.dtd">
   <configuration>
       <!-- 配置数据库连接池信息 -->
   	<environments default="development">
   		<environment id="development">
   			<transactionManager type="JDBC" />
   			<dataSource type="POOLED">
   				<property name="driver" value="com.mysql.jdbc.Driver" />
   				<property name="url" value="jdbc:mysql://localhost:3306/mybatis" />
   				<property name="username" value="root" />
   				<property name="password" value="123456" />
   			</dataSource>
   		</environment>
   	</environments>
       
   	<!-- 将我们写好的sql映射文件（EmployeeMapper.xml）一定要注册到全局配置文件（mybatis-config.xml）中 -->
   	<mappers>
   		<mapper resource="EmployeeMapper.xml" />
   	</mappers>
       
   </configuration>
   ~~~

3. 编写 Bean类 和 Dao接口 

   ~~~java
   package com.atguigu.mybatis.dao;
   import com.atguigu.mybatis.bean.Employee;
   public interface EmployeeMapper {
   	public Employee getEmpById(Integer id);
   }
   ~~~

4. sql映射文件：保存了每一个sql语句的映射信息，将sql抽取出来
   ==> 将sql映射文件注册在全局配置文件中

   | 原生          | mybatis          |
   | ------------- | ---------------- |
   | Dao类         | Mapper接口       |
   | DaoImpl实现类 | xxMapper.xml文件 |

   \> mapper接口没有实现类，但是mybatis会为这个接口生成一个代理对象。（将接口和xml进行绑定）
   EmployeeMapper empMapper =	sqlSession.getMapper(EmployeeMapper.class);

   ~~~xml
   <?xml version="1.0" encoding="UTF-8" ?>
   <!DOCTYPE mapper
    PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
    "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
   <mapper namespace="com.atguigu.mybatis.dao.EmployeeMapper">
   	<select id="getEmpById" resultType="com.atguigu.mybatis.bean.Employee">
   		select id,last_name lastName,email,gender from tbl_employee where id = #{id}
   	</select>
   </mapper>
   ~~~

   > namespace：名称空间;指定为接口的全类名
   >
   > id：唯一标识
   >
   > resultType：返回值类型
   >
   > public Employee getEmpById(Integer id);
   > #{id}：从传递过来的参数中取出id值

5. 编写测试类
   \> SqlSession代表和数据库的一次会话；用完必须关闭；
   \> SqlSession和connection一样她都是非线程安全。每次使用都应该去获取新的对象
   \> 使用sql的唯一标志来告诉MyBatis执行哪个sql。sql都是保存在sql映射文件中的。

   ~~~java
   public class MyBatisTest {
       //1.工厂模式：根据全局配置文件， 利用SqlSessionFactoryBuilder 创建 SqlSessionFactory ，有数据源一些运行环境信息
   	public SqlSessionFactory getSqlSessionFactory() throws IOException {
   		String resource = "mybatis-config.xml";
   		InputStream inputStream = Resources.getResourceAsStream(resource);
   		return new SqlSessionFactoryBuilder().build(inputStream);
   	}
       
       //方式一： 通过sqlSession实例 直接 执行已经映射的sql语句
   	@Test
   	public void test() throws IOException {
   		SqlSessionFactory sqlSessionFactory = getSqlSessionFactory();
           //2. 获取 sqlSession 对象。
   		SqlSession openSession = sqlSessionFactory.openSession();
   		try {
              //3、直接执行已经映射的sql语句，参数(sql的唯一标识,执行sql要用的参数)
   			Employee employee = openSession.selectOne(
   					"com.atguigu.mybatis.EmployeeMapper.emp", 1);
   			System.out.println(employee);
   		} finally {
   			openSession.close();
   		}
   
   	}
   
       //方式二：通过接口自动创建的一个代理对象，代理对象去执行增删改查方法
   	@Test
   	public void test01() throws IOException {
   		SqlSessionFactory sqlSessionFactory = getSqlSessionFactory();
   		// 2、获取sqlSession对象
   		SqlSession openSession = sqlSessionFactory.openSession();
   		try {
   			// 3、获取接口的实现类对象，代理对象
   			EmployeeMapper mapper = openSession.getMapper(EmployeeMapper.class);
               // 4、执行增删改查,sql中唯一标识
   			Employee employee = mapper.getEmpById(1);
   			System.out.println(mapper.getClass());
   			System.out.println(employee);
   		} finally {
   			openSession.close();
   		}
   
   	}
   
   }
   ~~~

   

## 全局配置文件

* 引入 XML 的 dtd 约束文件，方便编写 XML 的时候有 提示

  ~~~xml
  <!DOCTYPE configuration
   PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
   "http://mybatis.org/dtd/mybatis-3-config.dtd">
  ~~~

  ![image-20211208180608218](E:\软工\typora笔记\JavaEE框架\Mybatis.assets\image-20211208180608218.png)

* properties

  > 引入外部properties配置文件的内容
  > 		resource：引入类路径下的资源
  > 		url：引入网络路径或者磁盘路径下的资源

  ~~~xml
  	<properties resource="dbconfig.properties"></properties>
  ~~~

* settings
  ![image-20211209085902129](E:\软工\typora笔记\JavaEE框架\Mybatis.assets\image-20211209085902129.png)

  > 包含很多重要的设置项
  > 		setting	用来设置每一个设置项
  > 			name：设置项名
  > 			value：设置项取值

  ~~~xml
  	<settings>
  		<setting name="mapUnderscoreToCamelCase" value="true"/>
  	</settings>
  ~~~

* typeAliases

  > 别名处理器：可以为我们的java类型起别名, 别名不区分大小写
  >
  > ​		typeAlias	为某个java类型起别名
  > ​			type：指定要起别名的类型全类名;默认别名就是类名小写；employee
  > ​			alias：指定新的别名
  >
  > ​		package	为某个包下的所有类批量起别名 
  > ​			name:指定包名（为当前包以及下面所有的后代包的每一个类都起一个默认别名（类名小写））
  >
  > ​		@Alias注解	批量起别名的情况下，使用@Alias注解为某个类型指定新的别名

  ~~~xml
  	<typeAliases>
  		<typeAlias type="com.atguigu.mybatis.bean.Employee" alias="emp"/> 
  		<package name="com.atguigu.mybatis.bean"/>
  	</typeAliases>
  ~~~

  ~~~java
  @Alias("emp")
  public class Employee{
      ...
  }
  ~~~

* environments

  > mybatis可以配置多种环境 
  > 	default：指定使用某种环境。可以达到快速切换环境。
  >
  > ​	environment	配置一个具体的环境信息；
  > ​		id：代表当前环境的唯一标识
  >
  > ​			transactionManager	事务管理器；（必须有）
  > ​				type：事务管理器的类型;JDBC(提交和回滚)|MANAGED(反之)
  > ​							自定义事务管理器：实现TransactionFactory接口.type指定为全类名
  >
  > ​			dataSource	数据源;（必须有）
  > ​				type：数据源类型;UNPOOLED(反之)|POOLED(使用连接池)|JNDI
  > ​							自定义数据源：实现DataSourceFactory接口，type是全类名

  ~~~xml
  	<environments default="dev_mysql">
  		<environment id="dev_mysql">
  			<transactionManager type="JDBC"></transactionManager>
  			<dataSource type="POOLED">
  				<property name="driver" value="${jdbc.driver}" />
  				<property name="url" value="${jdbc.url}" />
  				<property name="username" value="${jdbc.username}" />
  				<property name="password" value="${jdbc.password}" />
  			</dataSource>
  		</environment>
  	
  		<environment id="dev_oracle">
  			<transactionManager type="JDBC" />
  			<dataSource type="POOLED">
  				<property name="driver" value="${orcl.driver}" />
  				<property name="url" value="${orcl.url}" />
  				<property name="username" value="${orcl.username}" />
  				<property name="password" value="${orcl.password}" />
  			</dataSource>
  		</environment>
  	</environments>
  ~~~

* databaseIdProvider

  > 支持多数据库厂商
  > 		 type="DB_VENDOR"：作用就是得到数据库厂商的标识，mybatis就能根据数据库厂商标识来执行不同的sql;
  > 		 	property	为不同的数据库厂商起别名
  > 				name：数据库厂商标识 （MySQL，Oracle，SQL Server,xxxx）
  > 				value：数据库厂商别名

  ~~~xml
  	<databaseIdProvider type="DB_VENDOR">
  		<property name="MySQL" value="mysql"/>
  		<property name="Oracle" value="oracle"/>
  		<property name="SQL Server" value="sqlserver"/>
  	</databaseIdProvider>
  ~~~

  * sql映射文件

    ~~~xml
    <mapper namespace="com.atguigu.mybatis.dao.EmployeeMapper">
     	<select id="getEmpById" resultType="com.atguigu.mybatis.bean.Employee">
    		select * from tbl_employee where id = #{id}
    	</select>
    	<select id="getEmpById" resultType="com.atguigu.mybatis.bean.Employee"
    		databaseId="mysql">
    		select * from tbl_employee where id = #{id}
    	</select>
        <select id="getEmpById" resultType="com.atguigu.mybatis.bean.Employee"
                databaseId="oracle">
            select EMPLOYEE_ID id,LAST_NAME	lastName,EMAIL email 
            from employees where EMPLOYEE_ID=#{id}
        </select>
    </mapper>
    ~~~

    > 匹配规则：
    >
    > 1. 没有配置databaseIdProvider，则databaseId=null
    > 2. 配置databaseIdProvider
    >    情况一：databaseId全为null，直接使用语句
    >    情况二：databaseId全不为null，使用databaseIdProvider标签配置的name去匹配数据库信息，匹配上设置databaseId=配置指定的值
    >    情况三：databaseId不全为null，会加载不带 databaseId 属性和带有匹配当前数据库databaseId 属性的所有语句。如果同时找到带有 databaseId 和不带
    >    databaseId 的相同语句，则后者会被舍弃。

* mappers

  > 将sql映射注册到全局配置中
  >
  > ​	mapper	注册一个sql映射 
  > ​		方式一：注册配置文件:有sql映射文件，映射文件名必须和接口同名，并且放在与接口同一目录下
  > ​			resource：引用类路径下的sql映射文件
  > ​			url：引用网路路径或者磁盘路径下的sql映射文件
  >
  > ​		方式二：注册接口:没有sql映射文件，所有的sql都是利用注解写在接口上
  > ​			class：引用（注册）接口
  >
  > ​	package	批量注册
  > ​		name：包名

  ~~~xml
  	<mappers>
  		<mapper resource="mybatis/mapper/EmployeeMapper.xml"/> 
  		<mapper class="com.atguigu.mybatis.dao.EmployeeMapperAnnotation"/> 
  		<package name="com.atguigu.mybatis.dao"/>
  	</mappers>
  ~~~

  ~~~java
  public interface EmployeeMapperAnnotation {
  	@Select("select * from tbl_employee where id=#{id}")
  	public Employee getEmpById(Integer id);
  }
  ~~~



## 映射文件

* 映射文件指导着MyBatis进行数据库增删改查

| 标签      | 说明                       |
| --------- | -------------------------- |
| insert    | 映射插入语句               |
| update    | 映射更新语句               |
| delete    | 映射删除语句               |
| select    | 映射查询语句               |
| sql       | 抽取可重用语句块           |
| resultMap | 自定义结果集映射           |
| cache     | 命名空间的二级缓存配置     |
| cache-ref | 其他命名空间缓存配置的引用 |

### 参数处理

> 底层原理：(@Param("id")Integer id,@Param("lastName")String lastName);
> ParamNameResolver解析参数封装map
>
> 1. 构造器：
>    \> 获取每个标了param注解的参数的@Param的值：id，lastName；赋值给name：{0=id, 1=lastName};
>    \> 没有标注，则name=参数名
>
> 2. 方法：
>    ==> 参数为null直接返回
>    ==> 如果只有一个元素，并且没有Param注解；args[0]：单个参数直接返回
>    ==> 多个元素或者有Param标注，遍历names集合；{0=id, 1=lastName,2=2}
>
>    ​       ==> names集合的value作为key;  names集合的key又作为取值的参考args[0]；args[1，"Tom"]：{id=args[0],lastName=args[1],2=args[2]}
>    ​       ==> 将每一个参数也保存到map中，使用新的key：param1...paramN
>    ​       ==> map 返回：有Param注解可以#{指定的key}，或者#{param1}

#### 传参

* 单个参数：mybatis**不会**做特殊处理
  `#{参数名/任意名}：取出参数值`

  ~~~java
  public Employee getEmp(@Param("id")Integer id,String lastName);
  //取值：id==>#{id/param1}   lastName==>#{param2}
  ~~~

* 多个参数：mybatis**会**做特殊处理

  > 多个参数会被封装成 一个map，#{}就是从map中获取指定的key的值；
  > 	key：param1...paramN,或者参数的索引也可以
  > 	value：传入的参数值
  >
  > 如果用#{参数名}取出参数值，会报异常：
  > 	org.apache.ibatis.binding.BindingException: Parameter 'id' not found. 

  方式一：`#{默认key的值}：取出对应的参数值`
  方式二：在mapper接口文件中用@Param("xxx")明确指定封装参数时map的key；
  				此时：key=使用@Param注解指定的值，value：参数值
  			   `#{指定的key}：取出对应的参数值`

  ~~~xml
   	<!-- public Employee getEmpByIdAndLastName(@Param("id")Integer id,@Param("lastName")String lastName);-->
   	<select id="getEmpByIdAndLastName" resultType="com.atguigu.mybatis.bean.Employee">
   		select * from tbl_employee where id = #{id} and last_name=#{lastName}
   	</select>
  ~~~

* POJO参数：mybatis**不会**做特殊处理
  `#{属性名}：取出传入的pojo的属性值`	

  ~~~java
  public Employee getEmp(Integer id,@Param("e")Employee emp);
  //取值：id==>#{param1}    lastName===>#{param2.lastName/e.lastName}
  ~~~

* Map参数：mybatis**不会**做特殊处理
  `#{key}：取出map中对应的值`

  ~~~xml
   	<!-- public Employee getEmpByMap(Map<String, Object> map); -->
   	<select id="getEmpByMap" resultType="com.atguigu.mybatis.bean.Employee">
   		select * from ${tableName} where id=#{id} and last_name=#{lastName}
   	</select>
  ~~~

* List、Set、数组参数：mybatis**会**做特殊处理

  Collection	`#{collection[i]}：取出对象的第i个值`
  List	`#{list[i]}：取出对象的第i个值`
  数组	`#{array[i]}：取出对象的第i个值`

  ~~~java
  public Employee getEmpById(List<Integer> ids);
  //取值：取出第一个id的值：   #{list[0]}
  ~~~

#### 取参

~~~java
select * from tbl_employee where id=${id} and last_name=#{lastName}
//Preparing: select * from tbl_employee where id=2 and last_name=?
~~~

* #{}:是以预编译的形式，将参数设置到sql语句中；PreparedStatement；防止sql注入
  参数的一些规则：
  javaType、 jdbcType、 mode（存储过程）、 numericScale、resultMap、 
  typeHandler、 jdbcTypeName、 expression（未来准备支持的功能）；

  ~~~xml
  <!-- 在数据为null的时候，有些数据库可能不能识别mybatis对null的默认处理。
  		JdbcType OTHER：无效的类型；因为mybatis对所有的null都映射的是原生Jdbc的OTHER类型，oracle不能正确处理;	
  		解决方式一：在映射文件中取参：#{email,jdbcType=OTHER};
  		解决方式二：全局配置中jdbcTypeForNull=OTHER，oracle不支持；改为支持的参数：
  -->
  	<settings>
  		<setting name="jdbcTypeForNull" value="NULL"/>
  	</settings>
  ~~~
  
  
  
* ${}:取出的值直接拼装在sql语句中；会有安全问题；

  ~~~java
  /*原生jdbc不支持占位符的地方我们就可以使用${}进行取值
    比如分表、排序、按照年份分表拆分……*/
  select * from ${year}_salary where xxx;
  select * from tbl_employee order by ${f_name} ${order}
  ~~~

  

### insert\update\delete 

| 属性             | 说明                                                         |
| ---------------- | ------------------------------------------------------------ |
| useGeneratedKeys | (仅对insert和update有用)使用自增主键获取主键,取出由数据库内部生成的主键 |
| keyProperty      | (仅对insert和update有用)查出的主键值封装给javaBean的哪个属性 |
| resultType       | 查出的数据的返回值类型<br />允许定义以下类型返回值：Integer、Long、Boolean、void |
| parameterType    | 参数类型，MyBatis会根据TypeHandler自动推断                   |

~~~xml
<mapper namespace="com.atguigu.mybatis.dao.EmployeeMapper">
    <!-- public void addEmp(Employee employee); -->
    <insert id="addEmp" parameterType="com.atguigu.mybatis.bean.Employee"
		useGeneratedKeys="true" keyProperty="id" resultType="Integer" databaseId="mysql">
		insert into tbl_employee(last_name,email,gender) 
		values(#{lastName},#{email},#{gender})
	</insert>
    
    <!-- public void updateEmp(Employee employee);  -->
	<update id="updateEmp" >
		update tbl_employee 
		set last_name=#{lastName},email=#{email},gender=#{gender}
		where id=#{id}
	</update>
	
	<!-- public void deleteEmpById(Integer id); -->
	<delete id="deleteEmpById">
		delete from tbl_employee where id=#{id}
	</delete>
</mapper>
~~~

> 设置提交数据类型：
> 	手动提交：sqlSessionFactory.openSession();
> 	自动提交：sqlSessionFactory.openSession(true);

~~~java
	@Test
	public void test03() throws IOException{
		SqlSessionFactory sqlSessionFactory = getSqlSessionFactory();
		//1、获取到的SqlSession不会自动提交数据
		SqlSession openSession = sqlSessionFactory.openSession();
		try{
			EmployeeMapper mapper = openSession.getMapper(EmployeeMapper.class);
			//测试添加
			Employee employee = new Employee(null, "jerry4",null, "1");
			mapper.addEmp(employee);
			System.out.println(employee.getId());
			
			//测试修改
			Employee employee = new Employee(1, "Tom", "jerry@atguigu.com", "0");
			boolean updateEmp = mapper.updateEmp(employee);
			System.out.println(updateEmp);
            
			//测试删除
			mapper.deleteEmpById(2);
            
			//2、手动提交数据
			openSession.commit();
		}finally{
			openSession.close();
		}
    }
~~~

### select

| 属性       | 说明                                 |
| ---------- | ------------------------------------ |
| resultType | 返回值类型。别名或者全类名           |
| resultMap  | 返回自定义类型，自定义结果集映射规则 |

* 对象、普通类型（返回一条记录）
  ==>   resultType要写对象类型或者数据类型

    ~~~java
    // mapper接口
    public interface EmployeeMapper {
        public Employee getEmpByIdAndLastName(@Param("id")Integer id,@Param("lastName")String lastName); 
    }
    ~~~
  
    ~~~xml
    <!-- sql映射文件 -->
        <!--  public Employee getEmpByIdAndLastName(Integer id,String lastName);-->
        <select id="getEmpByIdAndLastName" resultType="com.atguigu.mybatis.bean.Employee">
            select * from tbl_employee where id = #{id} and last_name=#{lastName}
        </select>
    ~~~
  
    ~~~java
  // 测试类
        @Test
        public void test04() throws IOException{
            SqlSessionFactory sqlSessionFactory = getSqlSessionFactory();
            SqlSession openSession = sqlSessionFactory.openSession(true);
            try{
                EmployeeMapper mapper = openSession.getMapper(EmployeeMapper.class);
  
                Employee employee = mapper.getEmpByIdAndLastName(1, "tom");
                System.out.println(employee);
        }
    ~~~
  
* List集合（返回一条或多条记录）
  ==> resultType要写List集合中元素的类型

  ~~~xml
  <!-- sql映射文件 -->
  	<!-- public List<Employee> getEmpsByLastNameLike(String lastName); -->
  	<select id="getEmpsByLastNameLike" resultType="com.atguigu.mybatis.bean.Employee">
  		select * from tbl_employee where last_name like #{lastName}
  	</select>
  ~~~
  
  ~~~java
  // 测试类
  	@Test
  	public void test04() throws IOException{
  		SqlSessionFactory sqlSessionFactory = getSqlSessionFactory();
  		SqlSession openSession = sqlSessionFactory.openSession(true);
  		try{
  			EmployeeMapper mapper = openSession.getMapper(EmployeeMapper.class);
  
  			List<Employee> like = mapper.getEmpsByLastNameLike("%e%");
  			for (Employee employee : like) {
  				System.out.println(employee);
  			}
  	}
  ~~~

  
  
* Map集合

  ~~~java
  // mapper接口
  public interface EmployeeMapper {
      //1. key就是列名，值就是对应的值（返回一条记录）
  	public Map<String, Object> getEmpByIdReturnMap(Integer id);
      
  	//2. 键是这条记录的主键，值是记录封装后的javaBean（返回多条记录）
      public Map<Integer, Employee> getEmpByLastNameLikeReturnMap(String lastName);
      
      //3. 键是@MapKey的属性值，值是记录封装后的javaBean（返回多条记录）
  	@MapKey("lastName")//告诉mybatis封装这个map的时候使用哪个属性作为map的key
  	public Map<String, Employee> getEmpByLastNameLikeReturnMap(String lastName);
  }
  ~~~
  
  ~~~xml
  <!-- sql映射文件 -->
  <!-- 返回一条记录  ==> resultType要写Map集合的别名map -->
   	<!--public Map<String, Object> getEmpByIdReturnMap(Integer id);  -->
   	<select id="getEmpByIdReturnMap" resultType="map">
   		select * from tbl_employee where id=#{id}
   	</select>
  
  <!-- 返回多条记录  ==> resultType要写Map集合中值的类型 -->
   	<!--public Map<Integer, Employee> getEmpByLastNameLikeReturnMap(String lastName);  -->
   	<select id="getEmpByLastNameLikeReturnMap" resultType="com.atguigu.mybatis.bean.Employee">
   		select * from tbl_employee where last_name like #{lastName}
   	</select>
  ~~~
  
  ~~~java
  // 测试类
  	@Test
  	public void test04() throws IOException{
  		SqlSessionFactory sqlSessionFactory = getSqlSessionFactory();
  		SqlSession openSession = sqlSessionFactory.openSession(true);
  		try{
  			EmployeeMapper mapper = openSession.getMapper(EmployeeMapper.class);
  		//返回一条记录
  			Map<String, Object> map = mapper.getEmpByIdReturnMap(1);
  			System.out.println(map);
              
  		//返回多条记录
  			//键是主键
              Map<Integer, Employee> map = mapper.getEmpByLastNameLikeReturnMap("%r%");
              System.out.println(map);
              
          	//键是自定义属性
  			Map<String, Employee> map = mapper.getEmpByLastNameLikeReturnMap("%r%");
  			System.out.println(map);
  		}finally{
  			openSession.close();
  		}
  	}
  ~~~
  

### resultMap

* 自动映射：列名和javaBean属性名一致，或者按照字段命名规范进行自动转换：

  ~~~xml
  <!-- 数据库字段命名规范，POJO属性符合驼峰命名法，如A_COLUMN -> aColumn，我们可以开启自动驼峰命名规则映射功能 -->
  	<settings>
  		<setting name="mapUnderscoreToCamelCase" value="true"/> 
  	</settings>
  ~~~

* 自定义映射，resultMap；实现高级结果集映射。

  | 标签        | 属性                                                         | 说明                                     |
  | ----------- | ------------------------------------------------------------ | ---------------------------------------- |
  | resultMap   | type：自定义规则的Java类型<br />id:唯一id方便引用            | 自定义某个javaBean的封装规则             |
  | association | ① **property**：指定对应的javaBean的哪个属性是联合的对象<br />**javaType**:指定这个属性对象的类型[不能省略]<br />② **property**：指定对应的javaBean的哪个属性是联合的对象<br />**select**:表明当前属性是调用select指定的方法查出的结果<br />column:指定将哪一列的值传给这个方法 | 定义关联对象的封装规则（属性是一个对象） |
  | collection  | ① **property**：指定对应的javaBean的哪个属性是联合的集合<br />**ofType**:指定集合里面元素的类型<br />② **property**：指定对应的javaBean的哪个属性是联合的对象<br />**select**:表明当前属性是调用select指定的方法查出的结果<br />**column**:指定将哪一列的值传给这个方法<br />**fetchType**：设置延迟加载；lazy：延迟eager：立即 | 定义关联集合类型的属性的封装规则         |
  | id          | column：指定表中哪一列<br />property：指定对应的javaBean属性 | 定义主键会底层有优化                     |
  | result      | column：指定表中哪一列<br />property：指定对应的javaBean属性 | 定义普通列封装规则                       |
  
  > 建议：其他不指定的列会自动封装，我们只要写resultMap就把全部的映射规则都写上	
  
  ~~~xml
  	<resultMap type="com.atguigu.mybatis.bean.Employee" id="MySimpleEmp">
  		<id column="id" property="id"/>
  		<result column="last_name" property="lastName"/>
  
  		<result column="email" property="email"/>
  		<result column="gender" property="gender"/>
  	</resultMap>
  	
  	<!-- public Employee getEmpById(Integer id); -->
  	<select id="getEmpById"  resultMap="MySimpleEmp">
  		select * from tbl_employee where id=#{id}
  	</select>
  ~~~
  
  * 延迟加载和属性按需加载：用到时才加载，没用到则不加载
  
    ~~~xml
    <!-- 方式一：在全局配置文件中加上两个配置：
    (旧版本mybatis需要jar包支持：asm-3.3.1.jar 	cglib-2.2.2.jar) 
    -->
    <setting name="lazyLoadingEnabled" value="true"/>
    <setting name="aggressiveLazyLoading" value="false"/>
    ~~~
  
    ~~~xml
    <!-- 方式二：在映射文件上设置属性值：-->
     	<association fetchType="lazy"></association>
    	<collection fetchType="lazy"></collection>
    ~~~
  
    

#### association 复杂类型的关联

* 联合查询

  ~~~xml
  <!-- 方式一: result -->
  	<resultMap type="com.atguigu.mybatis.bean.Employee" id="MyDifEmp">
  		<id column="id" property="id"/>
  		<result column="last_name" property="lastName"/>
  		<result column="gender" property="gender"/>
          <!-- 另一张表数据 -->
  		<result column="did" property="dept.id"/>
  		<result column="dept_name" property="dept.departmentName"/>
  	</resultMap>
  
  	<!--  public Employee getEmpAndDept(Integer id);-->
  	<select id="getEmpAndDept" resultMap="MyDifEmp">
  		SELECT e.id id,e.last_name last_name,e.gender gender,e.d_id d_id,d.id did,d.dept_name dept_name 
          FROM tbl_employee e,tbl_dept d
  		WHERE e.d_id=d.id AND e.id=#{id}
  	</select>
  ~~~

  ~~~xml
  <!-- 方式二 : association 嵌套结果集-->
  	<resultMap type="com.atguigu.mybatis.bean.Employee" id="MyDifEmp2">
  		<id column="id" property="id"/>
  		<result column="last_name" property="lastName"/>
  		<result column="gender" property="gender"/>
  
  		<association property="dept" javaType="com.atguigu.mybatis.bean.Department">
  			<id column="did" property="id"/>
  			<result column="dept_name" property="departmentName"/>
  		</association>
  	</resultMap>
  ~~~

* 分段查询 

  > 流程：
  >
  > 1. 使用select指定的方法查出对象；参数是传入column指定的这列参数的值
  >
  > 2. 查出的对象封装给property指定的属性

  ~~~xml
  <mapper namespace="com.atguigu.mybatis.dao.EmployeeMapperPlus">
  <!-- 如：1、先按照员工id查询员工信息
  		2、根据查询员工信息中的d_id值去部门表查出部门信息
  		3、部门设置到员工中；-->
      
  	 <!--id  last_name  email   gender    d_id   -->
  	 <resultMap type="com.atguigu.mybatis.bean.Employee" id="MyEmpByStep">
  	 	<id column="id" property="id"/>
  	 	<result column="last_name" property="lastName"/>
  	 	<result column="email" property="email"/>
  	 	<result column="gender" property="gender"/>
           
   		<association property="dept" 	 		select="com.atguigu.mybatis.dao.DepartmentMapper.getDeptById"
  	 		column="d_id">
   		</association>
  	 </resultMap>
  
  	 <!--  public Employee getEmpByIdStep(Integer id);-->
  	 <select id="getEmpByIdStep" resultMap="MyEmpByStep">
  	 	select * from tbl_employee where id=#{id}
  	 </select>
  </mapper> 
  ~~~

  ~~~xml
  <mapper namespace="com.atguigu.mybatis.dao.DepartmentMapper">
  	<!--public Department getDeptById(Integer id);  -->
  	<select id="getDeptById" resultType="com.atguigu.mybatis.bean.Department">
  		select id,dept_name departmentName from tbl_dept where id=#{id}
  	</select>
  </mapper>
  ~~~

  

#### collection 复杂类型的集

* 联合查询

  ~~~xml
  	<resultMap type="com.atguigu.mybatis.bean.Department" id="MyDept">
  		<id column="did" property="id"/>
  		<result column="dept_name" property="departmentName"/>
  		<collection property="emps" ofType="com.atguigu.mybatis.bean.Employee">
  			<id column="eid" property="id"/>
  			<result column="last_name" property="lastName"/>
  			<result column="email" property="email"/>
  			<result column="gender" property="gender"/>
  		</collection>
  	</resultMap>
  
  	<!-- public Department getDeptByIdPlus(Integer id); -->
  	<select id="getDeptByIdPlus" resultMap="MyDept">
  		SELECT d.id did,d.dept_name dept_name,
  				e.id eid,e.last_name last_name,e.email email,e.gender gender
  		FROM tbl_dept d
  		LEFT JOIN tbl_employee e
  		ON d.id=e.d_id
  		WHERE d.id=#{id}
  	</select>
  ~~~

* 分段查询、延迟加载

  > 多列的值传递过去：封装成map；
  > 			column="{key1=column1,key2=column2}"

  ~~~xml
  <mapper namespace="com.atguigu.mybatis.dao.DepartmentMapper">
  <!-- 如：查询部门的时候将部门对应的所有员工信息也查询出来 -->
  	<resultMap type="com.atguigu.mybatis.bean.Department" id="MyDeptStep">
  		<id column="id" property="id"/>
  		<id column="dept_name" property="departmentName"/>
  		<collection property="emps" 
  	select="com.atguigu.mybatis.dao.EmployeeMapperPlus.getEmpsByDeptId"
  			column="{deptId=id}" fetchType="lazy"></collection>
  	</resultMap>
  
  	<!-- public Department getDeptByIdStep(Integer id); -->
  	<select id="getDeptByIdStep" resultMap="MyDeptStep">
  		select id,dept_name from tbl_dept where id=#{id}
  	</select>
  </mapper>
  ~~~

  ~~~xml
  <mapper namespace="com.atguigu.mybatis.dao.EmployeeMapperPlus">
      <!-- public List<Employee> getEmpsByDeptId(Integer deptId); -->
  	<select id="getEmpsByDeptId" resultType="com.atguigu.mybatis.bean.Employee">
  		select * from tbl_employee where d_id=#{deptId}
  	</select>
  </mapper>
  ~~~

## 动态sql

* 基于OGNL 的表达式（类似于JSTL）来简化操作，基于 XML 的文本。

* 字符串截取 trim(封装任何条件) where(封装查询条件) set(封装修改条件)

  \> 查询操作：where 标签只会去掉第一个多出来的`and或者or`

  ~~~xml
  	 <!-- public List<Employee> getEmpsByConditionIf(Employee employee); -->
  	 <select id="getEmpsByConditionIf" resultType="com.atguigu.mybatis.bean.Employee">
  	 	select * from tbl_employee
  	 	<where>
  		 	and last_name like #{lastName} and email=#{email} and gender=#{gender}
  	 	</where>
  	 </select>
  ~~~

  \> 增改操作：set 标签只会去掉第一个多出来的 `，`

  ~~~xml
  	 <!--public void updateEmp(Employee employee);  -->
  	 <update id="updateEmp">
  	 	update tbl_employee 
  		<set>
  			last_name=#{lastName},email=#{email},gender=#{gender},
  		</set>
  		where id=#{id} 
  ~~~

  \> trim标签可以**自定义**字符串的截取规则

  | 属性            | 说明                                    |
  | --------------- | --------------------------------------- |
  | prefix          | 前缀：给拼串后的整个字符串加一个前缀    |
  | prefixOverrides | 前缀覆盖： 去掉整个字符串前面多余的字符 |
  | suffix          | 后缀：给拼串后的整个字符串加一个后缀    |
  | suffixOverrides | 后缀覆盖：去掉整个字符串后面多余的字符  |

  ~~~xml
  	 <!--public List<Employee> getEmpsByConditionTrim(Employee employee);  -->
  	 <select id="getEmpsByConditionTrim" resultType="com.atguigu.mybatis.bean.Employee">
  	 	select * from tbl_employee
  	 	<trim prefix="where" suffixOverrides="and">
  	 		id=#{id} and last_name like #{lastName} and email=#{email} and
  		 </trim>
  	 </select>
  ~~~

* 判断 if 

  > test：判断表达式（OGNL）：
  >
  > 1. 从参数中取值进行判断
  > 2. 特殊符号如”,>,<等这些都需要使用转义字符
  > 3. ognl会进行字符串与数字的转换判断  "0"==0

  ~~~xml
  	 <!-- public List<Employee> getEmpsByConditionIf(Employee employee); -->
  	 <select id="getEmpsByConditionIf" resultType="com.atguigu.mybatis.bean.Employee">
  	 	select * from tbl_employee
           <where>
  		 	<if test="id!=null">
  		 		id=#{id}
  		 	</if>
  		 	<if test="lastName!=null &amp;&amp; lastName!=&quot;&quot;">
  		 		 and last_name like #{lastName}
  		 	</if>
  		 	<if test="email!=null and email.trim()!=&quot;&quot;">
  		 		email=#{email}
  		 	</if> 
           <where>
  	 </select>
  ~~~

* 分支选择 choose  when otherwise

  > 带了break的swtich-case；只会进入choose中的一个标签中

  ~~~xml
  	 <!-- public List<Employee> getEmpsByConditionChoose(Employee employee); -->
  	 <select id="getEmpsByConditionChoose" resultType="com.atguigu.mybatis.bean.Employee">
  	 	select * from tbl_employee 
  	 	<where>
  	 		<choose>
  	 			<when test="id!=null">
  	 				id=#{id}
  	 			</when>
  	 			<when test="lastName!=null">
  	 				last_name like #{lastName}
  	 			</when>
  	 			<otherwise>
  	 				gender = 0
  	 			</otherwise>
  	 		</choose>
  	 	</where>
  	 </select>
  ~~~

* 遍历集合 foreach

  | 属性       | 说明                                                         |
  | ---------- | ------------------------------------------------------------ |
  | collection | 指定要遍历的集合                                             |
  | item       | 将当前遍历出的元素赋值给指定的变量                           |
  | separator  | 每个元素之间的分隔符                                         |
  | open       | 遍历出所有结果拼接一个开始的字符                             |
  | close      | 遍历出所有结果拼接一个结束的字符                             |
  | index      | 索引。遍历list的时候是index就是索引，item就是当前值。遍历map的时候index表示的就是map的key，item就是map的值 |

  \> 查询 操作

  ~~~xml
  	 <!--public List<Employee> getEmpsByConditionForeach(List<Integer> ids);  -->
  	 <select id="getEmpsByConditionForeach" resultType="com.atguigu.mybatis.bean.Employee">
  	 	select * from tbl_employee
  	 	<foreach collection="ids" item="item_id" separator=","
  	 		open="where id in(" close=")">
  	 		#{item_id}
  	 	</foreach>
  	 </select>
  ~~~

  \> 插入操作

  ~~~xml
  	 <!--public void addEmps(@Param("emps")List<Employee> emps);  -->
  	<insert id="addEmps">
  	 	insert into tbl_employee(
  	 		<include refid="insertColumn"></include>
  	 	) 
  		values
  		<foreach collection="emps" item="emp" separator=",">
  			(#{emp.lastName},#{emp.email},#{emp.gender},#{emp.dept.id})
  		</foreach>
  	 </insert>
  ~~~

  > 如果要分隔多个sql可以用于其他的批量操作（删除，修改），需要数据库连接属性allowMultiQueries=true；

  ~~~xml
  	 <insert id="addEmps">
  	 	<foreach collection="emps" item="emp" separator=";">
  	 		insert into tbl_employee(last_name,email,gender,d_id)
  	 		values(#{emp.lastName},#{emp.email},#{emp.gender},#{emp.dept.id})
  	 	</foreach>
  	 </insert>
  ~~~

* mybatis默认内置参数

  | 参数        | 说明                                                         |
  | ----------- | ------------------------------------------------------------ |
  | _parameter  | 单个参数：\_parameter就是这个参数<br/>多个参数：参数会被封装为一个map；_parameter就是代表这个map |
  | _databaseId | 如果配置了databaseIdProvider标签，_databaseId就是代表当前数据库的别名 |

  ~~~xml
  	  <!--public List<Employee> getEmpsTestInnerParameter(Employee employee);  -->
  	  <select id="getEmpsTestInnerParameter" resultType="com.atguigu.mybatis.bean.Employee">
  	  		<if test="_databaseId=='mysql'">
  	  			select * from tbl_employee
  	  			<if test="_parameter!=null">
  	  				where last_name like #{lastName}
  	  			</if>
  	  		</if>
  	  		<if test="_databaseId=='oracle'">
  	  			select * from employees
  	  			<if test="_parameter!=null">
  	  				where last_name like #{_parameter.lastName}
  	  			</if>
  	  		</if>
  	  </select>
  ~~~

* sql 抽取可重用的sql片段，方便后面引用 

  > 1. sql抽取：经常将要查询的列名，或者插入用的列名抽取出来方便引用
  > 2. include来引用已经抽取的sql：
  >    include还可以自定义一些property，sql标签内部就能使用自定义的属性；property：取值的正确方式${prop},#{不能使用这种方式}

  ~~~xml
  	  <sql id="insertColumn">
  	  		<if test="_databaseId=='mysql'">
  	  			last_name,email,gender,d_id
  	  		</if>
  	  </sql>
  ~~~

  ~~~xml
  	<insert id="addEmps">
  	 	insert into tbl_employee(
  	 		<include refid="insertColumn">
          		<property name="testColomn" value="abc"/>
          	</include>
  	 	) 
  		values
  		<foreach collection="emps" item="emp" separator=",">
  			(#{emp.lastName},#{emp.email},#{emp.gender},#{emp.dept.id})
  		</foreach>
  	 </insert>
  ~~~

* bind 创建一个变量并将其绑定到上下文

  ~~~xml
  	  <!--public List<Employee> getEmpsTestInnerParameter(Employee employee);  -->
  	  <select id="getEmpsTestInnerParameter" resultType="com.atguigu.mybatis.bean.Employee">
  	  		<bind name="_lastName" value="'%'+lastName+'%'"/>
  	  		select * from tbl_employee where last_name like #{_lastName}
  	  </select>
  ~~~



## 缓存机制

* MyBatis系统中默认定义了两级缓存
* 为了提高扩展性，MyBatis定义了缓存接口Cache。通过实现Cache接口来自定义二级缓存![image-20211214161801664](E:\软工\typora笔记\JavaEE框架\Mybatis.assets\image-20211214161801664.png)

### 一级缓存

* （本地缓存）sqlSession级别的缓存；一级缓存是一直开启的。

* SqlSession级别的一个Map与数据库同一次会话期间查询到的数据会放在本地缓存中。

  以后如果需要获取相同的数据，直接从缓存中拿，没必要再去查询数据库；

* 一级缓存失效情况（没有使用到当前一级缓存的情况，效果就是，还需要再向数据库发出查询）：
  1. 不同的SqlSession对应不同的一级缓存
  
     ~~~java
     	@Test
     	public void testFirstLevelCache() throws IOException{
     		SqlSessionFactory sqlSessionFactory = getSqlSessionFactory();
             //第一个SqlSession
     		SqlSession openSession = sqlSessionFactory.openSession();
     		try{
     			EmployeeMapper mapper = openSession.getMapper(EmployeeMapper.class);
     			Employee emp01 = mapper.getEmpById(1);
     			System.out.println(emp01);
     
                 //第二个SqlSession
     			SqlSession openSession2 = sqlSessionFactory.openSession();
     			EmployeeMapper mapper2 = openSession2.getMapper(EmployeeMapper.class);
                 Employee emp02 = mapper.getEmpById(1);
     			
     			System.out.println(emp01==emp02);
     			openSession2.close();
     		}finally{
     			openSession.close();
     		}
     	}
     ~~~
  
     
  
  2. 同一个SqlSession但是查询条件不同(当前一级缓存中还没有这个数据)
  
     ~~~java
     	@Test
     	public void testFirstLevelCache() throws IOException{
     		SqlSessionFactory sqlSessionFactory = getSqlSessionFactory();
     		SqlSession openSession = sqlSessionFactory.openSession();
     		try{
     			EmployeeMapper mapper = openSession.getMapper(EmployeeMapper.class);
     			Employee emp01 = mapper.getEmpById(1);
     			System.out.println(emp01);
     			
     			Employee emp03 = mapper.getEmpById(3);
     			System.out.println(emp03);
                 
     			System.out.println(emp01==emp02);
     		}finally{
     			openSession.close();
     		}
     	}
     ~~~
  
     
  
  3. 同一个SqlSession两次查询期间执行了任何一次增删改操作(这次增删改可能对当前数据有影响)
  
     ~~~java
     	@Test
     	public void testFirstLevelCache() throws IOException{
     		SqlSessionFactory sqlSessionFactory = getSqlSessionFactory();
     		SqlSession openSession = sqlSessionFactory.openSession();
     		try{
     			EmployeeMapper mapper = openSession.getMapper(EmployeeMapper.class);
     			Employee emp01 = mapper.getEmpById(1);
     			System.out.println(emp01);
     			
     			mapper.addEmp(new Employee(null, "testCache", "cache", "1"));
     			System.out.println("数据添加成功");
     
     			Employee emp02 = mapper.getEmpById(1);
     			System.out.println(emp02);
     			System.out.println(emp01==emp02);
     		}finally{
     			openSession.close();
     		}
     	}
     ~~~
  
     
  
  4. 同一个SqlSession两次查询期间手动清空了缓存（缓存清空）
  
     ~~~java
     	@Test
     	public void testFirstLevelCache() throws IOException{
     		SqlSessionFactory sqlSessionFactory = getSqlSessionFactory();
     		SqlSession openSession = sqlSessionFactory.openSession();
     		try{
     			EmployeeMapper mapper = openSession.getMapper(EmployeeMapper.class);
     			Employee emp01 = mapper.getEmpById(1);
     			System.out.println(emp01);
     
     			openSession.clearCache();
     			
     			Employee emp02 = mapper.getEmpById(1);
     			System.out.println(emp02);
     			System.out.println(emp01==emp02);
     		}finally{
     			openSession.close();
     		}
     	}
     ~~~
  
     

### 二级缓存

* （全局缓存）基于namespace级别的缓存：一个namespace对应一个二级缓存

* 工作机制：（只有会话提交或者关闭以后，一级缓存中的数据才会转移到二级缓存中）

  1. 一个会话，查询一条数据，这个数据就会被放在当前会话的一级缓存中；

  2. 如果会话关闭；一级缓存中的数据会被保存到二级缓存中；新的会话查询信息，就可以参照二级缓存中的内容；

  3. sqlSession===EmployeeMapper==>Employee

     ​						 DepartmentMapper===>Department

     不同namespace查出的数据会放在自己对应的缓存中（map）

  4. 效果：数据会从二级缓存中获取，查出的数据都会被默认先放在一级缓存中。

  ~~~java
  	@Test
  	public void testSecondLevelCache() throws IOException{
  		SqlSessionFactory sqlSessionFactory = getSqlSessionFactory();
  		SqlSession openSession = sqlSessionFactory.openSession();
  		SqlSession openSession2 = sqlSessionFactory.openSession();
  		try{
  			EmployeeMapper mapper = openSession.getMapper(EmployeeMapper.class);
  			EmployeeMapper mapper2 = openSession2.getMapper(EmployeeMapper.class);
  			
  			Employee emp01 = mapper.getEmpById(1);
  			System.out.println(emp01);
  			openSession.close();
  			
  			//第二次查询是从二级缓存中拿到的数据，并没有发送新的sql
  			Employee emp02 = mapper2.getEmpById(1);
  			System.out.println(emp02);
  			openSession2.close();
  		}finally{
  			
  		}
  	}
  ~~~

* 开启和配置步骤：
  1. 全局配置文件中开启二级缓存

      > cacheEnabled=false：关闭二级缓存(一级缓存一直可用的)

      ~~~xml
      <setting name= "cacheEnabled" value="true"/>
      ~~~

  2. mapper.xml中配置使用二级缓存

      | 属性          | 说明                                      | 属性值                                                       |
      | ------------- | ----------------------------------------- | ------------------------------------------------------------ |
      | eviction      | 缓存的回收策略                            | LRU – 最近最少使用的：移除最长时间不被使用的对象（默认）<br />FIFO – 先进先出：按对象进入缓存的顺序来移除它们<br />SOFT – 软引用：移除基于垃圾回收器状态和软引用规则的对象<br /><br />WEAK – 弱引用：更积极地移除基于垃圾收集器状态和弱引用规则的对象 |
      | flushInterval | 缓存刷新间隔，缓存多长时间清空一次        | 默认不清空，设置一个毫秒值                                   |
      | readOnly      | 是否只读                                  | true 只读；mybatis认为所有从缓存中获取数据的操作都是只读操作，不会修改数据。mybatis为了加快获取速度，直接就会将数据在缓存中的引用交给用户。不安全，速度快<br />false：非只读：mybatis觉得获取的数据可能会被修改。mybatis会利用序列化&反序列的技术克隆一份新的数据给你。安全，速度慢 |
      | size          | 缓存存放多少元素                          |                                                              |
      | type          | 指定自定义缓存的全类名；实现Cache接口即可 |                                                              |

      ~~~xml
      	<cache eviction="FIFO" flushInterval="60000" readOnly="false" size="1024"></cache>
      ~~~

  3. POJO需要实现Serializable接口

* 缓存的有关设置
  \> 查询标签 `默认：useCache="true"`
  false：不使用二级缓存（一级缓存依然使用）
  \> 增删改标签 `默认：flushCache="true"` 
     查询标签 `默认：flushCache="false"`
  true：每次执行之后都会清空缓存；缓存是没有被使用的
  \> sqlSession
  sqlSession.clearCache()  只是用来清除一级缓存

### 第三方缓存整合

* EhCache 是一个纯Java的进程内缓存框架，具有快速、精干等特点。以这个为例整合：

1. 导入第三方缓存包、整合包、日志包：
   ehcache-core-2.6.8.jar	mybatis-ehcache-1.0.3.jar
   slf4j-api-1.6.1.jar	slf4j-log4j12-1.6.2.jar

2. 编写ehcache.xml配置文件

   | 标签         | 说明                                                         |
   | ------------ | ------------------------------------------------------------ |
   | diskStore    | 指定数据在磁盘中的存储位置                                   |
   | defaultCache | 当借助CacheManager.add("demoCache")创建Cache时，EhCache便会采用\<defalutCache/>指定的的管理策略 |

   | defaultCache属性                | 说明                                                         |
   | ------------------------------- | ------------------------------------------------------------ |
   | maxElementsInMemory（必写）     | 在内存中缓存的element的最大数目                              |
   | maxElementsOnDisk（必写）       | 在磁盘上缓存的element的最大数目，若是0表示无穷大             |
   | eternal（必写）                 | 设定缓存的elements是否永远不过期。如果为true，则缓存的数据始终有效，如果为false那么还要根据timeToIdleSeconds，timeToLiveSeconds判断 |
   | overflowToDisk（必写）          | 设定当内存缓存溢出的时候是否将过期的element缓存到磁盘上      |
   | timeToIdleSeconds               | 当缓存在EhCache中的数据前后两次访问的时间超过timeToIdleSeconds的属性取值时，这些数据便会删除，默认值是0,也就是可闲置时间无穷大 |
   | timeToLiveSeconds               | 缓存element的有效生命期，默认是0.,也就是element存活时间无穷大 |
   | diskSpoolBufferSizeMB           | 这个参数设置DiskStore(磁盘缓存)的缓存区大小.默认是30MB.每个Cache都应该有自己的一个缓冲区. |
   | diskPersistent                  | 在VM重启的时候是否启用磁盘保存EhCache中的数据，默认是false。 |
   | diskExpiryThreadIntervalSeconds | 磁盘缓存的清理线程运行间隔，默认是120秒。每个120s，相应的线程会进行一次EhCache中数据的清理工作 |
   | memoryStoreEvictionPolicy       | 当内存缓存达到最大，有新的element加入的时候， 移除缓存中element的策略。默认是LRU（最近最少使用），可选的有LFU（最不常使用）和FIFO（先进先出） |

   ~~~xml
   <?xml version="1.0" encoding="UTF-8"?>
   <ehcache xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:noNamespaceSchemaLocation="../config/ehcache.xsd">
    <!-- 磁盘保存路径 -->
    <diskStore path="D:\44\ehcache" />
    
    <defaultCache 
      maxElementsInMemory="10000" 
      maxElementsOnDisk="10000000"
      eternal="false" 
      overflowToDisk="true" 
      timeToIdleSeconds="120"
      timeToLiveSeconds="120" 
      diskExpiryThreadIntervalSeconds="120"
      memoryStoreEvictionPolicy="LRU">
    </defaultCache>
   </ehcache>
   ~~~

3. 配置cache标签

   ~~~xml
   <cache type="org.mybatis.caches.ehcache.EhcacheCache"></cache>
   ~~~

4. 如果需要共享相同的缓存配置和实例，可引用缓存：
   namespace   指定和哪个名称空间下的缓存一样

   ~~~xml
   <cache-ref namespace="com.atguigu.mybatis.dao.EmployeeMapper"/>
   ~~~

## myBatis-Spring整合

1. 查看不同MyBatis版本整合Spring时使用的适配包；
   http://www.mybatis.org/spring/

2. 下载整合适配包
   https://github.com/mybatis/spring/releases

3. 官方整合示例，jpetstore
   https://github.com/mybatis/jpetstore-6

4. 在spring配置文件中：整合关键配置

   > 整合mybatis ，目的：
   >
   > 1. spring管理所有组件。mapper的实现类。
   >    service==>Dao   @Autowired:自动注入mapper；
   > 2. spring用来管理事务，spring声明式事务

   ~~~xml
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns="http://www.springframework.org/schema/beans"
   	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
   	xmlns:context="http://www.springframework.org/schema/context"
   	xmlns:mybatis-spring="http://mybatis.org/schema/mybatis-spring"
   	xmlns:tx="http://www.springframework.org/schema/tx"
   	xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
   		http://mybatis.org/schema/mybatis-spring http://mybatis.org/schema/mybatis-spring-1.2.xsd
   		http://www.springframework.org/schema/tx http://www.springframework.org/schema/tx/spring-tx-4.0.xsd
   		http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context-4.0.xsd">
   
   	<!-- Spring希望管理所有的业务逻辑组件，等。。。 -->
   	<context:component-scan base-package="com.atguigu.mybatis">
   		<context:exclude-filter type="annotation"
   			expression="org.springframework.stereotype.Controller" />
   	</context:component-scan>
   
   	<!-- 引入数据库的配置文件 -->
   	<context:property-placeholder location="classpath:dbconfig.properties" />
       
   	<!-- Spring用来控制业务逻辑。数据源、事务控制、aop -->
   	<bean id="dataSource" class="com.mchange.v2.c3p0.ComboPooledDataSource">
   		<property name="jdbcUrl" value="${jdbc.url}"></property>
   		<property name="driverClass" value="${jdbc.driver}"></property>
   		<property name="user" value="${jdbc.username}"></property>
   		<property name="password" value="${jdbc.password}"></property>
   	</bean>
       
   	<!-- spring事务管理 -->
   	<bean id="dataSourceTransactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
   		<property name="dataSource" ref="dataSource"></property>
   	</bean>
   
   	<!-- 开启基于注解的事务 -->
   	<tx:annotation-driven transaction-manager="dataSourceTransactionManager"/>
   	
   	<!--创建出SqlSessionFactory对象  -->
   	<bean id="sqlSessionFactoryBean" class="org.mybatis.spring.SqlSessionFactoryBean">
   		<property name="dataSource" ref="dataSource"></property>
   		<!-- configLocation指定全局配置文件的位置 -->
   		<property name="configLocation" value="classpath:mybatis-config.xml"></property>
   		<!--mapperLocations: 指定mapper文件的位置-->
   		<property name="mapperLocations" value="classpath:mybatis/mapper/*.xml"></property>
   	</bean>
   	
   	<!--配置一个可以进行批量执行的sqlSession  -->
   	<bean id="sqlSession" class="org.mybatis.spring.SqlSessionTemplate">
   		<constructor-arg name="sqlSessionFactory" ref="sqlSessionFactoryBean"></constructor-arg>
   		<constructor-arg name="executorType" value="BATCH"></constructor-arg>
   	</bean>
   	
   	<!-- 扫描所有的mapper接口的实现，让这些mapper能够自动注入；
   	base-package：指定mapper接口的包名
   	 -->
   	<mybatis-spring:scan base-package="com.atguigu.mybatis.dao"/>
   	<bean class="org.mybatis.spring.mapper.MapperScannerConfigurer">
   		<property name="basePackage" value="com.atguigu.mybatis.dao"></property>
   	</bean>
   	
   </beans>
   ~~~




## 逆向工程

简称MBG，代码生成器。可以根据数据库表生成对应的映射文件、接口、bean类。

1. 编写MBG的配置文件

   > Context
   > 		targetRuntime =“MyBatis3“									可以生成带条件的增删改查
   > 		targetRuntime =“MyBatis3Simple“						可以生成基本的增删改查
   > jdbcConnection																配置数据库连接信息
   > javaModelGenerator														配置javaBean的生成策略
   > 		targetPackage="test.model"									目标包名
   > 		targetProject="\MBGTestProject\src"					目标工程
   > sqlMapGenerator				 											配置sql映射文件生成策略
   > javaClientGenerator														配置Mapper接口的生成策略
   > table 																					配置要逆向解析的数据表
   > 		tableName																表名
   > 		domainObjectName												对应的javaBean名

   ~~~xml
   <generatorConfiguration>
       <context id= "DB2Tables" targetRuntime="MyBatis3">
           //数据库连接信息配置
           <jdbcConnection driverClass= "com.mysql.jdbc.Driver"
           connectionURL= "jdbc:mysql://localhost:3306/bookstore0629"
           userId= "root" password= "123456">
           </jdbcConnection>
           
           //javaBean的生成策略
           <javaModelGenerator targetPackage= "com.atguigu.bean" targetProject= ".\src">
               <property name= "enableSubPackages" value="true" />
               <property name= "trimStrings" value="true" />
           </javaModelGenerator>
           
           //映射文件的生成策略
           <sqlMapGenerator targetPackage= "mybatis.mapper" targetProject= ".\conf">
               <property name= "enableSubPackages" value="true" />
           </sqlMapGenerator>
           
           //dao接口java文件的生成策略
           <javaClientGenerator type= "XMLMAPPER" targetPackage= "com.atguigu.dao"
           targetProject= ".\src">
           	<property name= "enableSubPackages" value="true" />
           </javaClientGenerator>
           
           //数据表与javaBean的映射
           <table tableName= "books" domainObjectName="Book"></table>
       </context>
   </generatorConfiguration>
   ~~~

   

2. 运行代码生成器生成代码

   > 注意：
   >
   > 如果再次生成，建议将之前生成的数据删除，避免xml向后追加内容出现的问题。

   ~~~java
   public c static d void main(String[] args) s throws Exception {
       List<String> warnings = new ArrayList<String>();
       boolean overwrite = true; 
       File configFile = new File( "mbg.xml" );
       ConfigurationParser cp = new ConfigurationParser(warnings);
       Configuration config = cp.parseConfiguration(configFile);
       DefaultShellCallback callback = new DefaultShellCallback(overwrite);
       MyBatisGenerator myBatisGenerator = new MyBatisGenerator(config,
       callback, warnings);
       myBatisGenerator.generate( null);
   }
   ~~~

3. 测试

   ~~~java
   @Test
   public void test01(){
       SqlSession openSession = build.openSession();
       DeptMapper mapper = openSession.getMapper(DeptMapper. class);
       
       DeptExample example = new DeptExample();
       //所有的条件都在example中封装
       Criteria criteria = example.createCriteria();
       //select id, deptName, locAdd from tbl_dept WHERE
       //( deptName like ? and id > ? )
       criteria.andDeptnameLike("%部%");
       criteria.andIdGreaterThan(2);
       List<Dept> list = mapper.selectByExample(example);
       
       for (Dept dept : list) {
     	  System. out.println(dept);
       }
   }
   ~~~

## 底层原理

略

## 插件开发（不理解）

1. 编写Interceptor的实现类

   | interceptor接口方法 | 说明                                                      |
   | ------------------- | --------------------------------------------------------- |
   | Intercept           | 拦截目标方法执行                                          |
   | plugin              | 生成动态代理对象，可以使用MyBatis提供的Plugin类的wrap方法 |
   | setProperties       | 注入插件配置时设置的属性                                  |

   ~~~java
   public class MyFirstPlugin implements Interceptor{
   	@Override
   	public Object intercept(Invocation invocation) throws Throwable {
   		System.out.println("MyFirstPlugin...intercept:"+invocation.getMethod());
   		//动态的改变一下sql运行的参数：以前1号员工，实际从数据库查询3号员工
   		Object target = invocation.getTarget();
   		System.out.println("当前拦截到的对象0.
                              target);
   		//拿到：StatementHandler==>ParameterHandler===>parameterObject
   		//拿到target的元数据
   		MetaObject metaObject = SystemMetaObject.forObject(target);
   		Object value = metaObject.getValue("parameterHandler.parameterObject");
   		System.out.println("sql语句用的参数是："+value);
   		//修改完sql语句要用的参数
   		metaObject.setValue("parameterHandler.parameterObject", 11);
   		//执行目标方法
   		Object proceed = invocation.proceed();
   		//返回执行后的返回值
   		return proceed;
   	}
   
   	/**
   	 * plugin：
   	 * 		包装目标对象的：包装：为目标对象创建一个代理对象
   	 */
   	@Override
   	public Object plugin(Object target) {
   		// TODO Auto-generated method stub
   		//我们可以借助Plugin的wrap方法来使用当前Interceptor包装我们目标对象
   		System.out.println("MyFirstPlugin...plugin:mybatis将要包装的对象"+target);
   		Object wrap = Plugin.wrap(target, this);
   		//返回为当前target创建的动态代理
   		return wrap;
   	}
   
   	/**
   	 * setProperties：
   	 * 		将插件注册时 的property属性设置进来
   	 */
   	@Override
   	public void setProperties(Properties properties) {
   		// TODO Auto-generated method stub
   		System.out.println("插件配置的信息："+properties);
   	}
   }
   ~~~

2. 使用@intercepts注解完成插件签名

   ~~~Java
   /** 告诉MyBatis当前插件用来拦截哪个对象的哪个方法 **/
   @Intercepts(
   		{
   			@Signature(type=StatementHandler.class,method="parameterize",args=java.sql.Statement.class)
   		})
   public class MyFirstPlugin implements Interceptor{

3. 在全局配置文件中注册插件

   ~~~xml
   	<!--plugins：注册插件  -->
   	<plugins>
   		<plugin interceptor="com.atguigu.mybatis.dao.MyFirstPlugin">
   			<property name="username" value="root"/>
   			<property name="password" value="123456"/>
   		</plugin>
   	</plugins>
   ~~~

   

