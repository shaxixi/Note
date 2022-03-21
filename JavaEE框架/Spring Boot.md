[toc]

# SpringBoot

> 小技巧：（基于idea)
>
> 1. 隐藏文件/类型文件：Setting → File Types → Ignored Files and Folders
> 2. 自动提示内置文件：File → Project Structure → Facets → Spring → 绿叶子按钮 → ＋ → 内置文件

## 概念

* SpringBoot能够快速创建出生产级别的Spring应用
* 优点：
  \> SpringBoot是整合Spring技术栈的一站式框架
  \> SpringBoot是简化Spring技术栈的快速开发脚手架
* 学习SpringBoot
  官网文档：https://github.com/spring-projects/spring-boot/wiki#release-notes
* Spring VS SpringBoot
  ![image-20220315105025107](E:\软工\typora笔记\JavaEE框架\Spring Boot.assets\image-20220315105025107.png)

## 前期提要

* maven配置文件设置
  \> 镜像换成国内的aliyun速度会快很多
  \> 设置jdk版本为1.8避免一些不匹配的麻烦

  ~~~xml
  	<mirrors>
        <mirror>
          <id>nexus-aliyun</id>
          <mirrorOf>central</mirrorOf>
          <name>Nexus aliyun</name>
          <url>http://maven.aliyun.com/nexus/content/groups/public</url>
        </mirror>
   	</mirrors>
   
    	<profiles>
           <profile>
                <id>jdk-1.8</id>
                <activation>
                  <activeByDefault>true</activeByDefault>
                  <jdk>1.8</jdk>
                </activation>
                <properties>
                  <maven.compiler.source>1.8</maven.compiler.source>
                  <maven.compiler.target>1.8</maven.compiler.target>
                  <maven.compiler.compilerVersion>1.8</maven.compiler.compilerVersion>
                </properties>
           </profile>
    	</profiles>
  ~~~

* 创建方式：
  \> Spring Initializr + https://start.spring.io（基于idea)
  \> https://start.spring.io + 下载解压 + 导入项目（基于官网）
  \> Spring Initializr + http://start.aliyun.com（基于阿里云）
  \> maven + 手动导入坐标 + 手工制作引导类（基于maven）

## springboot依赖特性

* parent ：减少依赖冲突【定义了若干个坐标版本号（依赖管理，而非依赖）】

  ~~~xml
  	<parent>
          <groupId>org.springframework.boot</groupId>
          <artifactId>spring-boot-starter-parent</artifactId>
          <version>2.3.4.RELEASE</version>
      </parent>
  ~~~

* starter ：减少依赖配置 【定义了当前项目使用的所有依赖坐标】

  ~~~xml
  	<dependencies>
          <!-- 方式一：springboot中有该依赖 -->
          <dependency>
              <groupId>org.springframework.boot</groupId>
              <artifactId>spring-boot-starter-web</artifactId>
          </dependency>
          
          <!-- 方式二：springboot中有该依赖，但版本号和springboot不同 【properties】-->
          <properties>
              <druid.version>1.1.16</druid.version>
          </properties>
          
          <!-- 方式三：springboot中没有该依赖 【version】-->
          <dependency>
              <groupId>itheima</groupId>
              <artifactId>project-dependencies</artifactId>
              <version>1.1.10</version>
          </dependency>
          
          <!-- 方式四：不要springboot中的该依赖，自己导入所需要的依赖 -->
          <dependency>
              <groupId>org.springframework.boot</groupId>
              <artifactId>spring-boot-starter-web</artifactId>
              <exclusions>
                  <exclusion>
                      <groupId>org.springframework.boot</groupId>
                      <artifactId>spring-boot-starter-tomcat</artifactId>
                  </exclusion>
              </exclusions>
          </dependency>
          <dependency>
              <groupId>org.springframework.boot</groupId>
              <artifactId>spring-boot-starter-jetty</artifactId>
          </dependency>
          
      </dependencies>
  ~~~

* 引导类：启动SpringBoot项目【运行后初始化Spring容器，扫描引导类所在包加载bean】

  ~~~java
  @SpringBootApplication
  public class MainApplication {
      public static void main(String[] args) {
          SpringApplication.run(MainApplication.class,args);
      }
  }
  ~~~

## springboot内置属性

* 官网：https://docs.spring.io/spring-boot/docs/current/reference/html/application-properties.html#application-properties

* 配置文件类型：

  | 文件类型                | 格式                     |
  | ----------------------- | ------------------------ |
  | application.properties  | server.port=80           |
  | application.yml（主流） | server:<br/>    port: 81 |
  | application.yaml        | server:<br/>    port: 81 |

  优先级：application.properties > application.yml > application.yaml
  不同配置文件中相同配置按照加载优先级相互覆盖，不同配置文件中不同配置全部保留

### yml语法
\> 大小写敏感
\> 缩进表示层级关系，**同层级左侧对齐**，只允许使用空格
\> 属性名与属性值之间使用**冒号+空格**作为分隔
\> # 表示注释

* 字面值

  ~~~yaml
  boolean: TRUE #TRUE,true,True,FALSE,false ， False 均可
  float: 6.8523015e+5 # 支持科学计数法
  int: 0b1010 # 支持二进制、八进制、十六进制
  null: ~ # 使用 ~ 表示 null
  string: HelloWorld # 字符串可以直接书写
  string2: "Hello World" # 可以使用双引号包裹特殊字符
  date: 2018-02-17 # 日期必须使用 yyyy-MM-dd 格式
  datetime: 2018-02-17T15:02:31+08:00 # 时间和日期之间使用 T 连接，最后使用 + 代表时区
  ~~~

* 数组：数据之间用 **减号+空格** 分开

  ~~~yaml
  likes: [王者荣耀,刺激战场] # 数组书写缩略格式
  users2: [ { name:Tom , age:4 } , { name:Jerry , age:5 } ] # 对象数组缩略格式
  users: # 对象数组格式
      - name: Tom
        age: 4
      - name: Jerry
        age: 5
  users: # 对象数组格式二
  	-
        name: Tom
        age: 4
  	-
        name: Jerry
        age: 5 
  ~~~

* 数据读取：
  方式一：读取一个属性

  ~~~yaml
  @Value("${一级属性名.二级属性名……}") # yml文件外
  ${一级属性名.二级属性名……} # yml文件内
  ~~~

  方式二：读取所有属性

  ~~~java
  @Autowired
  private Environment env;
  ~~~

  方式三：读取一个对象

  ~~~java
  @ConfigurationProperties(prefix = "enterprise") 
  public class Enterprise {
      private String name;
      private Integer age;
      private String[] subject;
  }
  ~~~

## 整合第三方技术

* JUnit：测试类在SpringBoot启动类的包或子包外，启动类扫描不到，需要手动添加启动类的设置

  ~~~java
  // classes：设置SpringBoot启动类
  @SpringBootTest(classes = Springboot05JUnitApplication.class)
  class Springboot07JUnitApplicationTests {}
  ~~~

* mybatis

  1. 选择当前模块需要使用的技术集（MyBatis、MySQL）
  
  2. 设置数据源参数
     ~~~yaml
     spring: # 自动提示
         datasource:  
             driver-class-name: com.mysql.cj.jdbc.Driver
             url: jdbc:mysql://localhost:3306/ssm_db
             username: root
             password: root
     ~~~
  
     SpringBoot版本低于2.4.3(不含)，Mysql驱动版本大于8.0时，需要在url连接串中配置时区
     `serverTimezone=UTC`
  
  3. 定义接口和映射配置
  
  4. 注入接口，测试功能

* Druid

  1. 设置数据源参数

     ~~~yaml
     spring:
         datasource:
             driver-class-name: com.mysql.cj.jdbc.Driver
             url: jdbc:mysql://localhost:3306/ssm_db?serverTimezone=UTC
             username: root
             password: root
             type: com.alibaba.druid.pool.DruidDataSource
             
     # 方式二:
     spring:
         datasource:
             druid:
                 driver-class-name: com.mysql.cj.jdbc.Driver
                 url: jdbc:mysql://localhost:3306/ssm_db?serverTimezone=UTC
                 username: root
                 password: root
     ~~~
  
  
    2. 导入Druid对应的starter
  
       ~~~xml
       <dependency>
           <groupId>com.alibaba</groupId>
           <artifactId>druid-spring-boot-starter</artifactId>
           <version>1.2.6</version>
       </dependency>
       ~~~
  

* 任意第三方技术
  1. 导入对应的starter
  2. 配置对应的设置或采用默认配置

