[toc]

# javaEE项目开发流程

## 基本概要

* 前端 ==> 前台 、后台

  后端 ==> 前台 、 后台

* 前后端要用到的相关框架
  ![image-20220103101726208](E:\软工\typora笔记\JavaEE框架\JavaEE项目.assets\image-20220103101726208.png)



## 后端环境搭建

### 1. 创建maven工程

* 确定项目架构图（父工程、子工程、独立工程、依赖关系）
  
  > 一般架构：
  > \> parent工程【统一管理项目所需要的包】
  > 		\> webui工程 （依赖 component）
  > 			【1. resources统一存放所有 xml配置文件 2. 测试】
  > 		\> component工程 （依赖entity、util）
  > 			【1. mapper(Dao) 2. service 3. mvc(servlet)】
  > 		\> entity工程【Bean】
  >
  > \> util工程【常用工具类】
  > \> reverse工程【mybatis的逆向工程】
  
  确定工程的创建计划名命（groupId、artifactId、packaging）
  
  > 注意：
  >
  > 1. 父工程的packaging用pom、webui工程的packaging用war、其余工程用jar。
  > 2. 该项目的所有工程的groupId都是一样的，名命规范：网址的逆序＋项目名称。
  > 3. 工程名称名命规范：at项目名称0序号-前台/后台-模块名称
  >    （如：atcrowdfunding01-admin-webui）
  
* 创建 Maven 工程 和 Maven 模块
  \> 参与继承、聚合的工程以“Maven module”的方式创建，继承和聚合可以自动配置出来
  \> 独立工程、不参与继承、聚合的工程以“Maven project”的方式创建
  【参数：artifactId/module name 、groupId、packaging】
  
  > 注意：
  >
  > 1. 前期准备：配置 jdk、maven 和 tomcat 环境、UTF-8编码格式等等
  > 2. packaging为war的工程需要自动生成 web.xml，否则报错
  >    （除此之外，本工程的pom.xml在新版本的eclipse的容易报错插件版本过低，
  >    需要加入新版本插件 ==> 删除缓存的中已有的旧版本插件==>更新maven）
  
* 创建工程之间的依赖关系、引用所有需要的jar包
  \> pom.xml文件下dependencies图形化方式进行操作
  【参数：Group id、artifact id、version】version：0.0.1-SNAPSHOT
  
  \> 每个工程需要什么依赖包就从父工程中的pom.xml选择所需要的依赖包复制过来，再把版本号删除。
  
  > 注意：
  >
  > 1. 增加依赖的时候，容易报错。解决办法：maven clean 或者 update project
  > 2. 有些依赖增加失败。解决办法：删除缓存中的 `*.lastUpdated` 文件，再更新maven
  > 3. 依赖信息来源：https://mvnrepository.com（需要调试来确认jar包之间的兼容性）
  > 4. 除了提供的现有jar包，还有其他需要可以自行导入。
  >
  > 每个工程所需依赖包：
  >
  > parent
  >
  > ~~~xml
  > 	<properties>
  > 		<!-- 【可修改】声明属性，对 Spring 的版本进行统一管理 -->
  > 		<redcap.spring.version>4.3.20.RELEASE</redcap.spring.version>
  > 		<!-- 【可修改】声明属性，对 SpringSecurity 的版本进行统一管理 -->
  > 		<redcap.spring.security.version>4.2.10.RELEASE</redcap.spring.security.version>
  > 	</properties>
  > 
  > 	<dependencyManagement>
  > 		<dependencies>
  > 			<!-- Spring 依赖 -->
  > 			<!-- https://mvnrepository.com/artifact/org.springframework/spring-orm -->
  > 			<dependency>
  > 				<groupId>org.springframework</groupId>
  > 				<artifactId>spring-orm</artifactId>
  > 				<version>${redcap.spring.version}</version>
  > 			</dependency>
  > 			<!-- https://mvnrepository.com/artifact/org.springframework/spring-webmvc -->
  > 			<dependency>
  > 				<groupId>org.springframework</groupId>
  > 				<artifactId>spring-webmvc</artifactId>
  > 				<version>${redcap.spring.version}</version>
  > 			</dependency>
  > 			<dependency>
  > 				<groupId>org.springframework</groupId>
  > 				<artifactId>spring-test</artifactId>
  > 				<version>${redcap.spring.version}</version>
  > 			</dependency>
  > 			<!-- https://mvnrepository.com/artifact/org.aspectj/aspectjweaver -->
  > 			<dependency>
  > 				<groupId>org.aspectj</groupId>
  > 				<artifactId>aspectjweaver</artifactId>
  > 				<version>1.9.2</version>
  > 			</dependency>
  > 			<!-- https://mvnrepository.com/artifact/cglib/cglib -->
  > 			<dependency>
  > 				<groupId>cglib</groupId>
  > 				<artifactId>cglib</artifactId>
  > 				<version>2.2</version>
  > 			</dependency>
  >             
  > 			<!-- 数据库依赖 -->
  > 			<!-- MySQL 驱动 -->
  > 			<dependency>
  > 				<groupId>mysql</groupId>
  > 				<artifactId>mysql-connector-java</artifactId>
  > 				<version>5.1.3</version>
  > 			</dependency>
  > 			<!-- 数据源 -->
  > 			<dependency>
  > 				<groupId>com.alibaba</groupId>
  > 				<artifactId>druid</artifactId>
  > 				<version>1.0.31</version>
  > 			</dependency>
  >             
  > 			<!-- MyBatis -->
  > 			<dependency>
  > 				<groupId>org.mybatis</groupId>
  > 				<artifactId>mybatis</artifactId>
  > 				<version>3.2.8</version>
  > 			</dependency>
  >             
  > 			<!-- MyBatis 与 Spring 整合 -->
  > 			<dependency>
  > 				<groupId>org.mybatis</groupId>
  > 				<artifactId>mybatis-spring</artifactId>
  > 				<version>1.2.2</version>
  > 			</dependency>
  >             
  > 			<!-- MyBatis 分页插件 -->
  > 			<dependency>
  > 				<groupId>com.github.pagehelper</groupId>
  > 				<artifactId>pagehelper</artifactId>
  > 				<version>4.0.0</version>
  > 			</dependency>
  >             
  > 			<!-- 日志 -->
  > 			<dependency>
  > 				<groupId>org.slf4j</groupId>
  > 				<artifactId>slf4j-api</artifactId>
  > 				<version>1.7.7</version>
  > 			</dependency>
  > 			<dependency>
  > 				<groupId>ch.qos.logback</groupId>
  > 				<artifactId>logback-classic</artifactId>
  > 				<version>1.2.3</version>
  > 			</dependency>
  > 			<!-- 其他日志框架的中间转换包 -->
  > 			<dependency>
  > 				<groupId>org.slf4j</groupId>
  > 				<artifactId>jcl-over-slf4j</artifactId>
  > 				<version>1.7.25</version>
  > 			</dependency>
  > 			<dependency>
  > 				<groupId>org.slf4j</groupId>
  > 				<artifactId>jul-to-slf4j</artifactId>
  > 				<version>1.7.25</version>
  > 			</dependency>
  >             
  > 			<!-- Spring 进行 JSON 数据转换依赖 -->
  > 			<dependency>
  > 				<groupId>com.fasterxml.jackson.core</groupId>
  > 				<artifactId>jackson-core</artifactId>
  > 				<version>2.9.8</version>
  > 			</dependency>
  > 			<dependency>
  > 				<groupId>com.fasterxml.jackson.core</groupId>
  > 				<artifactId>jackson-databind</artifactId>
  > 				<version>2.9.8</version>
  > 			</dependency>
  >             
  > 			<!-- JSTL 标签库 -->
  > 			<dependency>
  > 				<groupId>jstl</groupId>
  > 				<artifactId>jstl</artifactId>
  > 				<version>1.2</version>
  > 			</dependency>
  >             
  > 			<!-- junit 测试 -->
  > 			<dependency>
  > 				<groupId>junit</groupId>
  > 				<artifactId>junit</artifactId>
  > 				<version>4.12</version>
  > 				<scope>test</scope>
  > 			</dependency>
  >             
  > 			<!-- 引入 Servlet 容器中相关依赖 -->
  > 			<dependency>
  > 				<groupId>javax.servlet</groupId>
  > 				<artifactId>servlet-api</artifactId>
  > 				<version>2.5</version>
  > 				<scope>provided</scope>
  > 			</dependency>
  >             
  > 			<!-- JSP 页面使用的依赖 -->
  > 			<dependency>
  > 				<groupId>javax.servlet.jsp</groupId>
  > 				<artifactId>jsp-api</artifactId>
  > 				<version>2.1.3-b06</version>
  > 				<scope>provided</scope>
  > 			</dependency>
  > 			<!-- https://mvnrepository.com/artifact/com.google.code.gson/gson -->
  > 			<dependency>
  > 				<groupId>com.google.code.gson</groupId>
  > 				<artifactId>gson</artifactId>
  > 				<version>2.8.5</version>
  > 				</dependency>
  >             
  > 			<!-- SpringSecurity 对 Web 应用进行权限管理 -->
  > 			<dependency>
  > 				<groupId>org.springframework.security</groupId>
  > 				<artifactId>spring-security-web</artifactId>
  > 				<version>4.2.10.RELEASE</version>
  > 			</dependency>
  > 			<!-- SpringSecurity 配置 -->
  > 			<dependency>
  > 				<groupId>org.springframework.security</groupId>
  > 				<artifactId>spring-security-config</artifactId>
  > 				<version>4.2.10.RELEASE</version>
  > 			</dependency>
  > 			<!-- SpringSecurity 标签库 -->
  > 			<dependency>
  > 				<groupId>org.springframework.security</groupId>
  > 				<artifactId>spring-security-taglibs</artifactId>
  > 				<version>4.2.10.RELEASE</version>
  > 			</dependency>
  > 		</dependencies>
  > 	</dependencyManagement>
  > </project>
  > ~~~
  >
  > webui
  >
  > ~~~xml
  > <!-- 因为插件版本过低，所以插入一个新版本插件，解决报错问题。此问题在新版本eclipse上容易报错，旧版本一般不报错-->
  > <build>
  >     <finalName>umm-facade</finalName>
  >     <plugins>
  >         <plugin>
  >             <groupId>org.apache.maven.plugins</groupId>
  >             <artifactId>maven-war-plugin</artifactId>
  >             <version>3.3.1</version>
  >         </plugin>
  >     </plugins>
  > </build>
  > 
  > <dependencies>
  >     <!-- junit 测试 -->
  >     <dependency>
  >         <groupId>junit</groupId>
  >         <artifactId>junit</artifactId>
  >         <scope>test</scope>
  >     </dependency>
  >     <dependency>
  >         <groupId>org.springframework</groupId>
  >         <artifactId>spring-test</artifactId>
  >         <exclusions>
  >             <exclusion>
  >                 <groupId>commons-logging</groupId>
  >                 <artifactId>commons-logging</artifactId>
  >             </exclusion>
  >         </exclusions>
  >     </dependency>
  >     
  >     <!-- 引入 Servlet 容器中相关依赖 -->
  >     <dependency>
  >         <groupId>javax.servlet</groupId>
  >         <artifactId>servlet-api</artifactId>
  >         <scope>provided</scope>
  >     </dependency>
  >     
  >     <!-- JSP 页面使用的依赖 -->
  >     <dependency>
  >         <groupId>javax.servlet.jsp</groupId>
  >         <artifactId>jsp-api</artifactId>
  >         <scope>provided</scope>
  >     </dependency>
  > </dependencies>
  > ~~~
  >
  > component
  >
  > ~~~xml
  > <dependencies>
  >     <!-- Spring 依赖 -->
  >     <!-- https://mvnrepository.com/artifact/org.springframework/spring-orm -->
  >     <dependency>
  >         <groupId>org.springframework</groupId>
  >         <artifactId>spring-orm</artifactId>
  >         <!-- 用到其他日志组合时即可移除 -->
  >         <exclusions>
  >             <exclusion>
  >                 <groupId>commons-logging</groupId>
  >                 <artifactId>commons-logging</artifactId>
  >             </exclusion>
  >         </exclusions>
  >     </dependency>
  >    
  > <!--------------------------- 以上是 spring ------------------------>
  >     
  >     <!-- SpringMVC 依赖 -->
  >     <!-- https://mvnrepository.com/artifact/org.springframework/spring-webmvc -->
  >     <dependency>
  >         <groupId>org.springframework</groupId>
  >         <artifactId>spring-webmvc</artifactId>
  >     </dependency>
  >     
  >     <!-- Spring 进行 JSON 数据转换依赖 -->
  >     <dependency>
  >         <groupId>com.fasterxml.jackson.core</groupId>
  >         <artifactId>jackson-core</artifactId>
  >     </dependency>
  >     <dependency>
  >         <groupId>com.fasterxml.jackson.core</groupId>
  >         <artifactId>jackson-databind</artifactId>
  >     </dependency>
  >     
  >     <!-- 引入 Servlet 容器中相关依赖 -->
  >     <dependency>
  >         <groupId>javax.servlet</groupId>
  >         <artifactId>servlet-api</artifactId>
  >         <scope>provided</scope>
  >     </dependency>
  >     
  >     <!-- JSTL 标签库 -->
  >     <dependency>
  >         <groupId>jstl</groupId>
  >         <artifactId>jstl</artifactId>
  >     </dependency>
  >     
  >     <!-- json数据 -->
  >     <!-- https://mvnrepository.com/artifact/com.google.code.gson/gson -->
  >     <dependency>
  >         <groupId>com.google.code.gson</groupId>
  >         <artifactId>gson</artifactId>
  >     </dependency>
  >     
  > <!------------------------ 以上是 spring mvc ------------------------>    
  >     <!-- MyBatis 依赖 -->
  >     <dependency>
  >         <groupId>org.mybatis</groupId>
  >         <artifactId>mybatis</artifactId>
  >     </dependency>  
  > 
  >     <!-- MyBatis 与 Spring 整合 -->
  >     <dependency>
  >         <groupId>org.mybatis</groupId>
  >         <artifactId>mybatis-spring</artifactId>
  >     </dependency>
  > 
  >     <!--  AOP 依赖包 -->
  >     <!-- https://mvnrepository.com/artifact/org.aspectj/aspectjweaver -->
  >     <dependency>
  >         <groupId>org.aspectj</groupId>
  >         <artifactId>aspectjweaver</artifactId>
  >     </dependency>
  >     <!-- https://mvnrepository.com/artifact/cglib/cglib -->
  >     <dependency>
  >         <groupId>cglib</groupId>
  >         <artifactId>cglib</artifactId>
  >     </dependency> 
  >     
  >     <!-- MySQL 驱动 -->
  >     <dependency>
  >         <groupId>mysql</groupId>
  >         <artifactId>mysql-connector-java</artifactId>
  >     </dependency>
  >     
  >     <!-- 数据源 -->
  >     <dependency>
  >         <groupId>com.alibaba</groupId>
  >         <artifactId>druid</artifactId>
  >     </dependency>
  >     
  >     <!-- MyBatis 分页插件 -->
  >     <dependency>
  >         <groupId>com.github.pagehelper</groupId>
  >         <artifactId>pagehelper</artifactId>
  >     </dependency>
  > <!------------- 以上是 mybatis 和 spring-mybatis整合 ---------------->
  > 
  >     <!-- 日志 -->
  >     <dependency>
  >         <groupId>org.slf4j</groupId>
  >         <artifactId>slf4j-api</artifactId>
  >     </dependency>
  >     <dependency>
  >         <groupId>ch.qos.logback</groupId>
  >         <artifactId>logback-classic</artifactId>
  >     </dependency>
  >     
  >     <!-- 其他日志框架的中间转换包 -->
  >     <!-- 移除commons-logging后报错，解决报错所需要的包 -->
  >     <dependency>
  >         <groupId>org.slf4j</groupId>
  >         <artifactId>jcl-over-slf4j</artifactId>
  >     </dependency>
  > </dependencies>
  > ~~~
  >
  > entity
  >
  > ~~~xml
  > <!-- 暂无用到依赖包 -->
  > ~~~
  >
  > util: 工具类中要用到哪个依赖包就引入相关的依赖包
  >
  > ~~~xml
  > <!-- 工具类中有需要 HttpServletRequest ，所以引入了相关依赖-->
  > <dependencies>	
  >     <!-- 引入 Servlet 容器中相关依赖 -->
  >     <dependency>
  >         <groupId>javax.servlet</groupId>
  >         <artifactId>servlet-api</artifactId>
  >         <version>2.5</version>
  >         <scope>provided</scope>
  >     </dependency>
  > </dependencies>		
  > ~~~
  >
  > reverse
  >
  > ~~~xml
  > <!-- 依赖 MyBatis 核心包 -->
  > <dependencies>
  >     <dependency>
  >         <groupId>org.mybatis</groupId>
  >         <artifactId>mybatis</artifactId>
  >         <version>3.2.8</version>
  >     </dependency>
  > </dependencies>
  > 
  > <!-- 控制 Maven 在构建过程中相关配置 -->
  > <build>
  >     <!-- 构建过程中用到的插件 -->
  >     <plugins>
  >         <!-- 具体插件，逆向工程的操作是以构建过程中插件形式出现的 -->
  >         <plugin>
  >             <groupId>org.mybatis.generator</groupId>
  >             <artifactId>mybatis-generator-maven-plugin</artifactId>
  >             <version>1.3.0</version>
  >             <!-- 插件的依赖 -->
  >             <dependencies>
  >                 <!-- 逆向工程的核心依赖 -->
  >                 <dependency>
  >                     <groupId>org.mybatis.generator</groupId>
  >                     <artifactId>mybatis-generator-core</artifactId>
  >                     <version>1.3.2</version>
  >                 </dependency>
  >                 <!-- 数据库连接池 -->
  >                 <dependency>
  >                     <groupId>com.mchange</groupId>
  >                     <artifactId>c3p0</artifactId>
  >                     <version>0.9.2</version>
  >                 </dependency>
  >                 <!-- MySQL 驱动 -->
  >                 <dependency>
  >                     <groupId>mysql</groupId>
  >                     <artifactId>mysql-connector-java</artifactId>
  >                     <version>5.1.8</version>
  >                 </dependency>
  >             </dependencies>
  >         </plugin>
  >     </plugins>
  > </build>
  > ~~~

### 2. 创建数据库

* 设计数据库表（看得见的字段、看不见的字段、冗余字段/备用字段）
* 第一范式原子性、第二范式完整性、第三范式直接性

### 3. myBatis逆向工程 ==> bean、mapper

（在 reverse 工程中配置）

* 编写配置文件 generatorConfig.xml

  ~~~xml
  <generatorConfiguration>
  	<!-- 【可修改】mybatis-generator:generate -->
  	<context id="redcapTables" targetRuntime="MyBatis3">
  		<commentGenerator>
  			<!-- 是否去除自动生成的注释 true:是;false:否 -->
  			<property name="suppressAllComments" value="true" />
  		</commentGenerator>
          
  		<!--【可修改】数据库连接的信息：驱动类、连接地址、用户名、密码 -->
  		<jdbcConnection driverClass="com.mysql.jdbc.Driver"
  			connectionURL="jdbc:mysql://localhost:3306/project_crowd"
  			userId="root" password="123456">
  		</jdbcConnection>
          
  		<!-- 默认 false，把 JDBC DECIMAL 和 NUMERIC 类型解析为 Integer，为 true 时把 JDBC DECIMAL 
  			和 NUMERIC 类型解析为 java.math.BigDecimal -->
  		<javaTypeResolver>
  			<property name="forceBigDecimals" value="false" />
  		</javaTypeResolver>
          
  		<!-- 【可修改】 targetProject:生成 Entity 类的路径 -->
  		<javaModelGenerator targetProject=".\src\main\java"
  			targetPackage="com.redcap.crowd.entity">
  			<!-- enableSubPackages:是否让 schema 作为包的后缀 -->
  			<property name="enableSubPackages" value="false" />
  			<!-- 从数据库返回的值被清理前后的空格 -->
  			<property name="trimStrings" value="true" />
  		</javaModelGenerator>
          
  		<!-- 【可修改】targetProject:XxxMapper.xml 映射文件生成的路径 -->
  		<sqlMapGenerator targetProject=".\src\main\java"
  			targetPackage="com.redcap.crowd.mapper">
  			<!-- enableSubPackages:是否让 schema 作为包的后缀 -->
  			<property name="enableSubPackages" value="false" />
  		</sqlMapGenerator>
          
  		<!-- 【可修改】targetPackage：Mapper 接口生成的位置 -->
  		<javaClientGenerator type="XMLMAPPER"
  			targetProject=".\src\main\java"
  			targetPackage="com.redcap.crowd.mapper">
  			<!-- enableSubPackages:是否让 schema 作为包的后缀 -->
  			<property name="enableSubPackages" value="false" />
  		</javaClientGenerator>
          
  		<!-- 【可修改】数据库表名字和我们的 entity 类对应的映射指定 -->
  		<table tableName="t_admin" domainObjectName="Admin" />
  	</context>
  </generatorConfiguration>
  ~~~

* 执行maven命令逆向生成bean和mapper
  `mybatis-generator:generate`

* 生成的资源各归各位（如果生成失败，则没有生成资源）

  > Bean包 ==> entity工程下
  > Mapper.xml文件 ==> webui工程下
  > Mapper.java文件 ==> component工程下

### 4. Spring 整合 mybatis ==>  mapper、service

#### 测试mapper（增删改查）

* jdbc.properties

* mybatis 配置文件（逆向工程都完成了）

  ~~~xml
  <?xml version="1.0" encoding="UTF-8"?>
  <!DOCTYPE configuration PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
  "http://mybatis.org/dtd/mybatis-3-config.dtd">
  <configuration>
  </configuration>
  ~~~

* spring 配置文件 ==> spring-persist-mybatis.xml
  ![image-20220118131402101](E:\软工\typora笔记\JavaEE框架\JavaEE项目.assets\image-20220118131402101.png)

  ~~~xml
  	<!-- 加载 jdbc.properties -->
  	<context:property-placeholder
  		location="classpath:jdbc.properties" />
  	
  	<!-- 配置数据源 -->
  	<bean id="dataSource"
  		class="com.alibaba.druid.pool.DruidDataSource">
  		<!-- 连接数据库的用户名 -->
  		<property name="username" value="${jdbc.user}" />
  		<!-- 连接数据库的密码 -->
  		<property name="password" value="${jdbc.password}" />
  		<!-- 目标数据库的 URL 地址 -->
  		<property name="url" value="${jdbc.url}" />
  		<!-- 数据库驱动全类名 -->
  		<property name="driverClassName" value="${jdbc.driver}" />
  	</bean>
  	
  	<!-- 配置 SqlSessionFactoryBean -->
  	<bean id="sqlSessionFactoryBean" class="org.mybatis.spring.SqlSessionFactoryBean">
  		<!-- 装配数据源 -->
  		<property name="dataSource" ref="dataSource"/>
  		<!-- 指定 MyBatis 全局配置文件位置 -->
  		<property name="configLocation" value="classpath:mybatis-config.xml"/>
  		<!-- 指定 Mapper 配置文件位置 -->
  		<property name="mapperLocations" value="classpath:mybatis/mapper/*Mapper.xml"/>
  	</bean>
  	
  	<!-- 配置 MapperScannerConfigurer -->
  	<!-- 把 MyBatis 创建的 Mapper 接口类型的代理对象扫描到 IOC 容器中 -->
  	<bean id="mapperScannerConfigurer"
  	class="org.mybatis.spring.mapper.MapperScannerConfigurer">
  		<!-- 使用 basePackage 属性指定 Mapper 接口所在包 -->
  		<property name="basePackage" value="com.redcap.crowd.mapper"/>
  	</bean>
  ~~~

* 测试

  ~~~java
  //指定 Spring 给 Junit 提供的运行器类
  @RunWith(SpringJUnit4ClassRunner.class)
  //加载 Spring 配置文件的注解
  @ContextConfiguration(locations = { "classpath:spring-persist-mybatis.xml"})
  public class CrowdSpringTest {
  	@Autowired
  	private DataSource dataSource;
  	/**
  	 * 测试数据库的连接
  	 * @throws SQLException sql报错
  	 */
  	@Test
  	public void testDataSource() throws SQLException {
  		// 1.通过数据源对象获取数据源连接
  		Connection connection = dataSource.getConnection();
  		// 2.打印数据库连接
  		System.out.println(connection);
  	}
  
  	@Autowired
  	private AdminMapper adminMapper;
  	/**
  	 * 测试逆向工程的mybatis的增删改查(Dao)
  	 */
  	@Test
  	public void testAdminMapper() {
  		Admin admin = adminMapper.selectByPrimaryKey(1);
  		System.out.println(admin);
  	}
  }
  ~~~

#### 测试service（事务处理）

* spring配置文件 ==> spring-persist-tx.xml
  ![image-20220118132521831](E:\软工\typora笔记\JavaEE框架\JavaEE项目.assets\image-20220118132521831.png)

  ~~~xml
  	<!-- 配置自动扫描的包：主要是为了把Service扫描到IOC容器中 -->
  	<context:component-scan base-package="com.redcap.crowd.service"/>
  	
  	<!-- 配置事务管理器 -->
  	<bean id="dataSourceTransactionManager"
  		class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
  		<!-- 装配数据源 -->
  		<property name="dataSource" ref="dataSource" />
  	</bean>
  
  	<!-- 配置 AOP -->
  	<aop:config>
  		<!-- 配置切入点表达式 -->
  		<!-- public String com.redcap.crowd.service.AdminService.getXxx(Integer 
  			id) -->
  		<aop:pointcut expression="execution(* *..*Service.*(..))"
  			id="txPointCut" />
  		<!-- 将事务通知和切入点表达式关联到一起 -->
  		<aop:advisor advice-ref="txAdvice"
  			pointcut-ref="txPointCut" />
  	</aop:config>
  
  	<!-- 配置事务通知 -->
  	<!-- id 属性用于在 aop:advisor 中引用事务通知 -->
  	<!-- transaction-manager 属性用于引用事务管理器，如果事务管理器的 bean 的 id 正好是 transactionManager，可以省略这个属性 -->
  	<tx:advice id="txAdvice"
  		transaction-manager="dataSourceTransactionManager">
  		<tx:attributes>
  			<!-- name 属性指定当前要配置的事务方法的方法名 -->
  			<!-- 查询的方法通常设置为只读，便于数据库根据只读属性进行相关性能优化 -->
  			<tx:method name="get*" read-only="true" />
  			<tx:method name="query*" read-only="true" />
  			<tx:method name="find*" read-only="true" />
  			<tx:method name="count*" read-only="true" />
  			<!-- 增删改方法另外配置 -->
  			<!-- propagation 属性配置事务方法的传播行为 -->
  			<!-- 默认值：REQUIRED 表示：当前方法必须运行在事务中，如果没有事务，则开 启事务，在自己的事务中运行。如果已经有了已开启的事务，则在当前事务中运行。有可能 
  				和其他方法共用同一个事务。 -->
  			<!-- 建议值：REQUIRES_NEW 表示：当前方法必须运行在事务中，如果没有事务， 则开启事务，在自己的事务中运行。和 REQUIRED 
  				的区别是就算现在已经有了已开启的事务， 也一定要开启自己的事务，避免和其他方法共用同一个事务。 -->
  			<!-- rollback-for 属性配置回滚的异常 -->
  			<!-- 默认值：运行时异常 -->
  			<!-- 建议值：编译时异常+运行时异常 -->
  			<tx:method name="save*" propagation="REQUIRES_NEW"
  				rollback-for="java.lang.Exception" />
  			<tx:method name="remove*" propagation="REQUIRES_NEW"
  				rollback-for="java.lang.Exception" />
  			<tx:method name="update*" propagation="REQUIRES_NEW"
  				rollback-for="java.lang.Exception" />
  		</tx:attributes>
  	</tx:advice>
  ~~~

* 编写 xxxservice(接口，不用注解) 和 xxxserviceimpl(实现类，属性、类都要注解)

* 测试 

  ~~~java
  //指定 Spring 给 Junit 提供的运行器类
  @RunWith(SpringJUnit4ClassRunner.class)
  //加载 Spring 配置文件的注解
  @ContextConfiguration(locations = { "classpath:spring-persist-mybatis.xml","classpath:spring-persist-tx.xml" })
  public class CrowdSpringTest {
  	@Autowired
  	private AdminService adminService;	
  	@Test
  	public void testTx() {
  		Admin admin = new Admin(null, "jerry", "123456", "杰瑞", "jerry@qq.com", null);
  		adminService.saveAdmin(admin);
  	}
  }
  ~~~

### 5. Spring 整合 springmvc ==> servlet

#### 普通请求 ==> 返回页面

* web.xml配置文件

  ~~~xml
  	<!-- 配置 ContextLoaderListener 加载 Spring 配置文件 -->
  	<!-- needed for ContextLoaderListener -->
  	<context-param>
  		<param-name>contextConfigLocation</param-name>
  		<param-value>classpath:spring-persist-*.xml</param-value>
  	</context-param>
  
  	<!-- Bootstraps the root web application context before servlet initialization -->
  	<listener>
  		<listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
  	</listener>
  
  	<!-- 配置 CharacterEncodingFilter 解决 POST 请求的字符乱码问题 -->
  	<filter>
  		<filter-name>CharacterEncodingFilter</filter-name>
  		<filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
  		<!-- 指定字符集 -->
  		<init-param>
  			<param-name>encoding</param-name>
  			<param-value>UTF-8</param-value>
  		</init-param>
  		<!-- 强制请求进行编码 -->
  		<init-param>
  			<param-name>forceRequestEncoding</param-name>
  			<param-value>true</param-value>
  		</init-param>
  		<!-- 强制响应进行编码 -->
  		<init-param>
  			<param-name>forceResponseEncoding</param-name>
  			<param-value>true</param-value>
  		</init-param>
  	</filter>
  	<filter-mapping>
  		<filter-name>CharacterEncodingFilter</filter-name>
  		<url-pattern>/*</url-pattern>
  	</filter-mapping>
  
  	<!-- 配置 SpringMVC 的前端控制器 -->
  	<!-- The front controller of this Spring Web application, responsible for 
  		handling all application requests -->
  	<servlet>
  		<servlet-name>springDispatcherServlet</servlet-name>
  		<servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
  		<!-- 以初始化参数的形式指定 SpringMVC 配置文件的位置 -->
  		<init-param>
  			<param-name>contextConfigLocation</param-name>
  			<param-value>classpath:spring-web-mvc.xml</param-value>
  		</init-param>
  		<!-- 让 DispatcherServlet 在 Web 应用启动时创建对象、初始化 -->
  		<!-- 默认情况：Servlet 在第一次请求的时候创建对象、初始化 -->
  		<load-on-startup>1</load-on-startup>
  	</servlet>
  	<!-- Map all requests to the DispatcherServlet for handling -->
  	<servlet-mapping>
  		<servlet-name>springDispatcherServlet</servlet-name>
  		<!-- DispatcherServlet 映射的 URL 地址 -->
  		<!-- 大白话：什么样的访问地址会交给 SpringMVC 来处理 -->
  		<!-- 配置方式一：符合 RESTFUL 风格使用“/” -->
  		<!-- <url-pattern>/</url-pattern> -->
  		<!-- 配置方式二：请求扩展名 -->
  		<url-pattern>*.html</url-pattern>
  		<url-pattern>*.json</url-pattern>
  	</servlet-mapping>
  </web-app>
  ~~~

* spring配置文件 ==> spring-web-mvc.xml

  ~~~xml
  	<!-- 配置自动扫描的包 -->
  	<context:component-scan
  		base-package="com.redcap.crowd.mvc" />
  	<!-- 配置视图解析器 -->
  	<!-- 拼接公式→前缀+逻辑视图+后缀=物理视图 -->
  	<!-- @RequestMapping("/xxx/xxx") public String xxx() { // 这个返回值就是逻辑视图 return 
  		"target"; } 物理视图是一个可以直接转发过去的地址 物理视图："/WEB-INF/"+"target"+".jsp" 转发路径："/WEB-INF/target.jsp" -->
  	<bean id="viewResolver"
  		class="org.springframework.web.servlet.view.InternalResourceViewResolver">
  		<!-- 前缀：附加到逻辑视图名称前 -->
  		<property name="prefix" value="/WEB-INF/" />
  		<!-- 后缀：附加到逻辑视图名称后 -->
  		<property name="suffix" value=".jsp" />
  	</bean>
  	<!-- 启用注解驱动 -->
  	<mvc:annotation-driven />
  ~~~

* 测试

  1. jsp页面：每页需加 base标签

     ~~~html
     <base
     href="http://${pageContext.request.serverName } : ${pageContext.request.serverPort }${page
     Context.request.contextPath } / "/>
     ~~~

  2. handler/servlet方法

     ~~~java
     @Controller
     public class TestHandler {
         @RequestMapping("/test/aaa/bbb/ccc.html")
         public String doTest() {
         	return "";
         }
     }
     ~~~

#### Ajax请求 ==> 返回json数据

* 创建 jQuery.js 文件
* handler方法加 @ResponseBody注解（返回值本身就是响应的数据）
  handler参数加 @RequestBody注解（接收json数据）
* jsp页面 
  json数据 ==> json转成字符串 ==> 字符串赋值给data 
  ==> contentType为application/json;charset=UTF-8

![image-20220118142622928](E:\软工\typora笔记\JavaEE框架\JavaEE项目.assets\image-20220118142622928.png)

* 工具类：统一返回数据格式

  ~~~java
  package com.redcap.crowd.util;
  /**
   * 统一整个项目中Ajax请求返回的结果（未来也可以用于分布式架构各个模块间调用时返回统一类型）
   */
  public class ResultEntity<T> {
  	public static final String SUCCESS = "SUCCESS";
  	public static final String FAILED = "FAILED";
  	// 用来封装当前请求处理的结果是成功还是失败
  	private String result;
  	// 请求处理失败时返回的错误消息
  	private String message;
  	// 要返回的数据
  	private T data;
  	/**
  	 * 请求处理成功且不需要返回数据时使用的工具方法
  	 * @return
  	 */
  	public static <Type> ResultEntity<Type> successWithoutData() {
  		return new ResultEntity<Type>(SUCCESS, null, null);
  	}
  	/**
  	 * 请求处理成功且需要返回数据时使用的工具方法
  	 * @param data 要返回的数据
  	 * @return
  	 */
  	public static <Type> ResultEntity<Type> successWithData(Type data) {
  		return new ResultEntity<Type>(SUCCESS, null, data);
  	}
  	/**
  	 * 请求处理失败后使用的工具方法
  	 * @param message 失败的错误消息
  	 * @return
  	 */
  	public static <Type> ResultEntity<Type> failed(String message) {
  		return new ResultEntity<Type>(FAILED, message, null);
  	}
  	public ResultEntity() {
  		
  	}
  	public ResultEntity(String result, String message, T data) {
  		super();
  		this.result = result;
  		this.message = message;
  		this.data = data;
  	}
  	@Override
  	public String toString() {
  		return "ResultEntity [result=" + result + ", message=" + message + ", data=" + data + "]";
  	}
  	public String getResult() {
  		return result;
  	}
  	public void setResult(String result) {
  		this.result = result;
  	}
  	public String getMessage() {
  		return message;
  	}
  	public void setMessage(String message) {
  		this.message = message;
  	}
  	public T getData() {
  		return data;
  	}
  	public void setData(T data) {
  		this.data = data;
  	}
  }
  ~~~

#### 异常映射

* 普通请求 ==> 在页面上显示异常信息
   Ajax 请求 ==> 返回 JSON 数据

![image-20220118142828487](E:\软工\typora笔记\JavaEE框架\JavaEE项目.assets\image-20220118142828487.png)

* 工具类：判断请求类型

  ~~~java
  package com.redcap.crowd.util;
  
  public class CrowdUtil {
  	/**
  	 * 判断当前请求是否为Ajax请求
  	 * @param request 请求对象
  	 * @return
  	 * 		true：当前请求是Ajax请求
  	 * 		false：当前请求不是Ajax请求
  	 */
  	public static boolean judgeRequestType(HttpServletRequest request) {
  		// 1.获取请求消息头
  		String acceptHeader = request.getHeader("Accept");
  		String xRequestHeader = request.getHeader("X-Requested-With");
  		
  		// 2.判断
  		return (acceptHeader != null && acceptHeader.contains("application/json"))			
  				||			
  				(xRequestHeader != null && xRequestHeader.equals("XMLHttpRequest"));
  	}
  }
  ~~~

* 创建异常jsp页面

* xml实现异常映射

  ~~~xml
  	<!-- 配置基于XML的异常映射 -->
  	<bean id="simpleMappingExceptionResolver"
  		class="org.springframework.web.servlet.handler.SimpleMappingExceptionResolver">
  		<!-- 配置异常类型和具体视图页面的对应关系 -->
  		<property name="exceptionMappings">
  			<props>
  				<!-- key属性指定异常全类名 -->
  				<!-- 标签体中写对应的视图（这个值要拼前后缀得到具体路径） -->
  				<prop key="java.lang.Exception">system-error</prop>
  			</props>
  		</property>
  	</bean>
  ~~~

* 注解实现异常映射

  ~~~java
  package com.redcap.crowd.mvc.config;
  
  
  // @ControllerAdvice表示当前类是一个基于注解的异常处理器类
  @ControllerAdvice
  public class CrowdExceptionResolver {
  
  	@ExceptionHandler(value = ArithmeticException.class)
  	public ModelAndView resolveMathException(ArithmeticException exception, HttpServletRequest request,
  			HttpServletResponse response) throws IOException {
  
  		String viewName = "system-error";
  
  		return commonResolve(viewName, exception, request, response);
  	}
  
  	@ExceptionHandler(value = NullPointerException.class)
  	public ModelAndView resolveNullPointerException(NullPointerException exception, HttpServletRequest request,
  			HttpServletResponse response) throws IOException {
  
  		String viewName = "system-error";
  
  		return commonResolve(viewName, exception, request, response);
  	}
  
  	// @ExceptionHandler将一个具体的异常类型和一个方法关联起来
  	private ModelAndView commonResolve(
  
  			// 异常处理完成后要去的页面
  			String viewName,
  
  			// 实际捕获到的异常类型
  			Exception exception,
  
  			// 当前请求对象
  			HttpServletRequest request,
  
  			// 当前响应对象
  			HttpServletResponse response) throws IOException {
  
  		// 1.判断当前请求类型
  		boolean judgeResult = CrowdUtil.judgeRequestType(request);
  
  		// 2.如果是Ajax请求
  		if (judgeResult) {
  
  			// 3.创建ResultEntity对象
  			ResultEntity<Object> resultEntity = ResultEntity.failed(exception.getMessage());
  
  			// 4.创建Gson对象
  			Gson gson = new Gson();
  
  			// 5.将ResultEntity对象转换为JSON字符串
  			String json = gson.toJson(resultEntity);
  
  			// 6.将JSON字符串作为响应体返回给浏览器
  			response.getWriter().write(json);
  
  			// 7.由于上面已经通过原生的response对象返回了响应，所以不提供ModelAndView对象
  			return null;
  		}
  
  		// 8.如果不是Ajax请求则创建ModelAndView对象
  		ModelAndView modelAndView = new ModelAndView();
  
  		// 9.将Exception对象存入模型
  		modelAndView.addObject("exception", exception);
  
  		// 10.设置对应的视图名称
  		modelAndView.setViewName(viewName);
  
  		// 11.返回modelAndView对象
  		return modelAndView;
  	}
  }
  ~~~


## 功能编写流程

### 登录、退出

1. 常量类的创建（所有字符串信息）

   ~~~java
   public class CrowdConstant {
   	public static final String MESSAGE_STRING_INVALIDATE = "不能为空";
   	public static final String ATTR_NAME_LOGIN_ADMIN = "loginAdmin";
   	public static final String MESSAGE_LOGIN_FAILED = "账号或密码有误";
   	public static final String MESSAGE_SYSTEM_ERROR_LOGIN_NOT_UNQUE = "数据库账号出错了";
   	public static final String MESSAGE_ACCESS_FORBIDEN = "请先登录";
   }
   ~~~

2. 创建md5加密工具方法

   ~~~java
   /**
   * 对明文字符串进行 MD5 加密
   * @param source 传入的明文字符串
   * @return 加密结果
   */
   public static String md5(String source) {
   // 1.判断 source 是否有效
   if(source == null || source.length() == 0) {
   // 2.如果不是有效的字符串抛出异常
   throw new RuntimeException(CrowdConstant.MESSAGE_STRING_INVALIDATE);
   }
   try {
   // 3.获取 MessageDigest 对象
   String algorithm = "md5";
   MessageDigest messageDigest = MessageDigest.getInstance(algorithm);
   // 4.获取明文字符串对应的字节数组
   byte[] input = source.getBytes();
   // 5.执行加密
   byte[] output = messageDigest.digest(input);
   // 6.创建 BigInteger 对象
   int signum = 1;
   BigInteger bigInteger = new BigInteger(signum, output);
   // 7.按照 16 进制将 bigInteger 的值转换为字符串
   int radix = 16;
   String encoded = bigInteger.toString(radix).toUpperCase();
   return encoded;
   } catch (NoSuchAlgorithmException e) {
   e.printStackTrace();
   }
   return null;
   }
   ~~~

3. 创建登录失败异常类

   ~~~java
   public class LoginFailedException extends RuntimeException {
   	private static final long serialVersionUID = 1L;
   }
   ~~~

4. 创建异常处理方法（基于注解、基于xml）

   ~~~java
   @ExceptionHandler(value = LoginFailedException.class)
   public ModelAndView resolveLoginFailedException(
   	LoginFailedException exception,
   	HttpServletRequest request,
   	HttpServletResponse response
   ) throws IOException {
   	String viewName = "admin-login";
   	return commonResolve(viewName, exception, request, response);
   }
   ~~~

5. 创建handler方法、service方法、
   mvc:view-controlle
   （重定向 为了避免跳转到后台主页面再刷新浏览器导致重复提交登录表单）
   ![image-20220126232801311](E:\软工\typora笔记\JavaEE框架\JavaEE项目.assets\image-20220126232801311.png)

6. 前端页面的调整
   （域中值的获取、提交请求的地址、错误信息提示的标签、编码格式、url绝对路径……）

7. 公共部分的抽取并创建jsp模板

### 拦截器拦截页面

1. 创建拦截器的异常（便于异常映射处理）

2. 创建拦截器类（拦截逻辑）
   <img src="E:\软工\typora笔记\JavaEE框架\JavaEE项目.assets\image-20220126232831119.png" alt="image-20220126232831119" style="zoom:80%;" />

3. 注册拦截器类

   ~~~xml
   <!-- 注册拦截器 -->
   <mvc:interceptors>
       <mvc:interceptor>
           <!-- mvc:mapping 配置要拦截的资源 -->
           <!-- /*对应一层路径，比如：/aaa -->
           <!-- /**对应多层路径，比如：/aaa/bbb 或/aaa/bbb/ccc 或/aaa/bbb/ccc/ddd -->
           <mvc:mapping path="/**"/>
           <!-- mvc:exclude-mapping 配置不拦截的资源 -->
           <mvc:exclude-mapping path="/admin/to/login/page.html"/>
           <mvc:exclude-mapping path="/admin/do/login.html"/>
           <mvc:exclude-mapping path="/admin/do/logout.html"/>
           <!-- 配置拦截器类 -->
           <bean class="com.atguigu.crowd.mvc.interceptor.LoginInterceptor"/>
       </mvc:interceptor>
   </mvc:interceptors>
   ~~~

4. 完善异常映射（基于xml、基于注解）

   ~~~xml
   <!-- 配置基于 XML 的异常映射 -->
   <bean id="simpleMappingExceptionResolver"
   class="org.springframework.web.servlet.handler.SimpleMappingExceptionResolver">
       <!-- 配置异常类型和具体视图页面的对应关系 -->
       <property name="exceptionMappings">
           <props>
               <!-- key 属性指定异常全类名 -->
               <!-- 标签体中写对应的视图（这个值要拼前后缀得到具体路径） -->
               <prop key="java.lang.Exception">system-error</prop>
               <prop
               key="com.atguigu.crowd.exception.AccessForbiddenException">admin-login</prop>
           </props>
       </property>
   </bean>
   ~~~

### 分页



