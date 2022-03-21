[toc]

# Spring5 框架

## Spring框架概述

* Spring是轻量级的开源的JavaEE框架
* Spring可以解决企业应用开发的复杂性
* Spring有两个核心部分：IOC 和 AOP
  1. IOC：控制反转，把创建对象过程交给Spring进行管理
  2. AOP：面向切面，不修改源代码进行功能增强
* Spring 特点
  \> 方便解耦，简化开发
  \> AOP 编程支持
  \> 方便程序测试
  \> 方便和其他框架进行整合
  \> 方便进行事务操作
  \> 降低API开发难度

## Spring工程前期准备

* 下载Spring
  https://repo.spring.io/release/org/springframework/spring/

* 打开idea工具，创建普通Java工程

* 导入Spring5 相关jar包
  ![Spring5模块](E:\软工\typora笔记\JavaEE框架\Spring5.assets\Spring5模块.bmp)

  IOC基本包：commons-logging、spring-beans、spring-context、spring-core、spring-expression

* 创建普通类，普通方法

* 创建配置文件，在配置文件配置创建的对象

  ~~~xml
  <?xml version="1.0" encoding="UTF-8"?>
  <beans xmlns="http://www.springframework.org/schema/beans"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  	   xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
  
      <!--配置User对象创建-->
      <bean id="user" class="com.atguigu.spring5.User"></bean>
  </beans>
  ~~~

* 测试代码

  ~~~java
  public class TestBean {
      @Test
      public void testAdd() {
          //1 加载spring配置文件
          BeanFactory context =
                  new ClassPathXmlApplicationContext("bean1.xml");
  
          //2 获取配置创建的对象
          User user = context.getBean("user", User.class);
  
          System.out.println(user);
          user.add();
      }
  }
  ~~~

## IOC

* 控制反转，把对象创建和对象之间的调用过程，交给Spring进行管理

* 目的：为了耦合度降低

### 底层原理

xml解析、工厂模式、反射

![图2](E:\软工\typora笔记\JavaEE框架\Spring5.assets\图2.png)

### IOC 接口

* IOC思想基于IOC容器完成，IOC容器底层就是对象工厂

* Spring提供IOC容器实现方式：

  1. BeanFactory：IOC容器基本实现，是Spring内部的使用接口，不提供开发人员进行使用
     <>加载配置文件时候不会创建对象，在获取对象才去创建对象

  2. ApplocationContext：BeanFactory 接口的子接口，提供更多更强大的功能，一般由开发人员进行使用
     <>加载配置文件时候就会把在配置文件对象进行创建

     ![image-20211109154532564](E:\软工\typora笔记\JavaEE框架\Spring5.assets\image-20211109154532564.png)
     FileSystemXmlApplicationContext：参数是磁盘下的路径
     ClassPathXmlApplicationContext：参数是工程下的路径

### IOC操作Bean管理

#### 概念

* Bean管理有两个操作：1. Spring创建对象 2. Spring注入属性
* Bean管理有两种方式：1. 基于xml配置文件方式实现 2. 基于注解方式实现

#### 基于xml配置文件方式实现

1. 创建对象

   ~~~xml
   <!--配置User对象创建-->
   <bean id="user" class="com.atguigu.spring5.User">
   ~~~

   * 在Spring配置文件中，使用bean标签，标签里面添加对应属性，就可以实现对象创建
   * 在bean标签有很多属性，id属性：唯一标识 class属性：类全路径
   * 创建对象时候，**默认执行无参数构造方法**完成对象创建

2. 注入属性
   方式一：创建类，定义属性和对应的set方法
   方式二：spring配置文件配置对象创建，配置属性注入

   ~~~xml
   <!-- set方法注入属性-->
       <bean id="book" class="com.atguigu.spring5.Book">
           <!--使用property完成属性注入
               name：类里面属性名称
               value：向属性注入的值
           -->
           <property name="bname" value="易筋经"></property>
           <property name="bauthor" value="达摩老祖"></property>
       </bean>
   ~~~

   方式三：通过 参数构造器 进行注入属性

   ~~~xml
   <!--有参数构造注入属性-->
       <bean id="orders" class="com.atguigu.spring5.Orders">
           <constructor-arg name="oname" value="电脑"></constructor-arg>
           <constructor-arg name="address" value="China"></constructor-arg>
       </bean>
   ~~~

   方式四：通过 p 名称空间注入属性（可以简化基于xml配置方式）

   ~~~xml
   <!-- 1. 添加p名称空间在配置文件中 -->
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns="http://www.springframework.org/schema/beans"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xmlns:p="http://www.springframework.org/schema/p"
          xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
       
    <!-- 2. 在bean标签里面进行属性注入 -->
       <bean id="book" class="com.atguigu.spring5.Book" p:bname="九阳神功" p:bauthor="无名氏"></bean>
   ~~~

3. 加载配置文件、获取对象

   ~~~java
   public class TestSpring5 {
       @Test
       public void testAdd() {
           //1 加载spring配置文件
           BeanFactory context =
                   new ClassPathXmlApplicationContext("bean1.xml");
   
           //2 获取配置创建的对象
           User user = context.getBean("user", User.class);
           System.out.println(user);
           user.add();
       }
   }
   ~~~

##### xml注入其他类型属性值

* 字面量
  \> null值

  ~~~xml
      <bean id="book" class="com.atguigu.spring5.Book">
          <property name="address">
              <null/>
          </property>
      </bean>
  ~~~

  \> 特殊符号

  1. 把 < > 进行转义 \&lt; \&gt;
  2. 把带特殊符号内容写到 \<![CDATA[]]>

  ```xml
  <bean id="book" class="com.atguigu.spring5.Book">
      <property name="address">
          <value><![CDATA[<<南京>>]]></value>
      </property>
  </bean>
  ```

* 外部bean (适用于一对一的关系)

  1. 创建两个类service类和dao类

  2. 在service调用dao里面的方法

     ~~~java
     public class UserService {
     
         //创建UserDao类型属性，生成set方法
         private UserDao userDao;
         public void setUserDao(UserDao userDao) {
             this.userDao = userDao;
         }
     
         public void add() {
             System.out.println("service add...............");
             userDao.update();
         }
     }
     ~~~

  3. 在spring配置文件中进行配置

     ~~~xml
     <!--1 service和dao对象创建-->
         <bean id="userService" class="com.atguigu.spring5.service.UserService">
             <!--注入userDao对象
                 name属性：类里面属性名称
                 ref属性：创建userDao对象bean标签id值
             -->
             <property name="userDao" ref="userDaoImpl"></property>
         </bean>
         <bean id="userDaoImpl" class="com.atguigu.spring5.dao.UserDaoImpl"></bean>
     ~~~

* 内部bean （适用于一对多的关系）

  1. 部门是一，员工是多。员工表示所属部门

     ~~~java
     //部门类
     public class Dept {
         private String dname;
         public void setDname(String dname) {
             this.dname = dname;
         }
         @Override
         public String toString() {
             return "Dept{" +
                     "dname='" + dname + '\'' +
                     '}';
         }
     }
     
     //员工类
     public class Emp {
         private String ename;
         private String gender;
         //员工属于某一个部门，使用对象形式表示
         private Dept dept;
     
         public void setDept(Dept dept) {
             this.dept = dept;
         }
         public void setEname(String ename) {
             this.ename = ename;
         }
         public void setGender(String gender) {
             this.gender = gender;
         }
     
         public void add() {
             System.out.println(ename+"::"+gender+"::"+dept);
         }
     }
     ~~~

  2. 在spring配置文件中进行配置

     ~~~xml
         <bean id="emp" class="com.atguigu.spring5.bean.Emp">
             <!--设置两个普通属性-->
             <property name="ename" value="lucy"></property>
             <property name="gender" value="女"></property>
             <!--设置对象类型属性-->
             <property name="dept">
                 <bean id="dept" class="com.atguigu.spring5.bean.Dept">
                     <property name="dname" value="安保部"></property>
                 </bean>
             </property>
         </bean>
     ~~~

* 级联赋值（适用于一对多 且 对对象中的对象的属性注入）
  方式一

  ~~~xml
      <bean id="emp" class="com.atguigu.spring5.bean.Emp">
          <!--设置两个普通属性-->
          <property name="ename" value="lucy"></property>
          <property name="gender" value="女"></property>
          <!--级联赋值-->
          <property name="dept" ref="dept"></property>
      </bean>
      <bean id="dept" class="com.atguigu.spring5.bean.Dept">
          <property name="dname" value="财务部"></property>
      </bean>
  ~~~

  方式二

  ~~~java
  //员工类
  public class Emp {
      //……
      private Dept dept;
  
      //生成dept的get方法
      public Dept getDept() {
          return dept;
      }
  	//……
  }
  ~~~

  ~~~xml
      <bean id="emp" class="com.atguigu.spring5.bean.Emp">
          <!--设置两个普通属性-->
          <property name="ename" value="lucy"></property>
          <property name="gender" value="女"></property>
          <!--级联赋值-->
          <property name="dept.dname" value="技术部"></property>
      </bean>
      <bean id="dept" class="com.atguigu.spring5.bean.Dept">
          <property name="dname" value="财务部"></property>
      </bean>
  ~~~

##### xml注入集合属性

~~~java
public class Stu {
    //1 数组类型属性
    private String[] courses;
    
    public void setCourses(String[] courses) {
        this.courses = courses;
    }
    
    //2 list集合类型属性
    private List<String> list;

    public void setList(List<String> list) {
        this.list = list;
    }
    
    //3 map集合类型属性
    private Map<String,String> maps;

    public void setMaps(Map<String, String> maps) {
        this.maps = maps;
    }
    
    //4 set集合类型属性
    private Set<String> sets;

    public void setSets(Set<String> sets) {
        this.sets = sets;
    }

    //5  集合里面设置对象类型值
    private List<Course> courseList;
    public void setCourseList(List<Course> courseList) {
        this.courseList = courseList;
    }

    public void test() {
        System.out.println(Arrays.toString(courses));
        System.out.println(list);
        System.out.println(maps);
        System.out.println(sets);
    }
~~~

* 注入数组类型属性

  ~~~xml
     <bean id="stu" class="com.atguigu.spring5.collectiontype.Stu">
          <!--数组类型属性注入-->
          <property name="courses">
              <array>
                  <value>java课程</value>
                  <value>数据库课程</value>
              </array>
          </property>
      </bean>
  ~~~

  

* 注入List集合类型属性

  ~~~xml
     <bean id="stu" class="com.atguigu.spring5.collectiontype.Stu">
          <!--list类型属性注入-->
          <property name="list">
              <list>
                  <value>张三</value>
                  <value>小三</value>
              </list>
          </property>
      </bean>
  ~~~

  

* 注入Map集合类型属性

  ~~~xml
     <bean id="stu" class="com.atguigu.spring5.collectiontype.Stu">
          <!--map类型属性注入-->
          <property name="maps">
              <map>
                  <entry key="JAVA" value="java"></entry>
                  <entry key="PHP" value="php"></entry>
              </map>
          </property>
      </bean>
  ~~~

* 注入Set集合类型属性

  ~~~xml
     <bean id="stu" class="com.atguigu.spring5.collectiontype.Stu">
          <!--set类型属性注入-->
          <property name="sets">
              <set>
                  <value>MySQL</value>
                  <value>Redis</value>
              </set>
          </property>
      </bean>
  ~~~

* 注入集合类型属性

  1. 在spring配置文件中引入名称空间 util

     ~~~xml
     <?xml version="1.0" encoding="UTF-8"?>
     <beans xmlns="http://www.springframework.org/schema/beans"
            xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
            xmlns:util="http://www.springframework.org/schema/util" <!-- ** -->
            xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
                                http://www.springframework.org/schema/util http://www.springframework.org/schema/util/spring-util.xsd"> <!-- ** -->
     </beans>
     ~~~

  2. 使用util标签完成list集合注入提取

     ~~~xml
         <!--1 提取list集合类型属性注入-->
         <util:list id="bookList">
             <value>易筋经</value>
             <value>九阴真经</value>
             <value>九阳神功</value>
         </util:list>
     
         <!--2 提取list集合类型属性注入使用-->
         <bean id="book" class="com.atguigu.spring5.collectiontype.Book">
             <property name="list" ref="bookList"></property>
         </bean>
     ~~~

     

* 注入集合类型的值是对象属性

  ~~~xml
     <bean id="stu" class="com.atguigu.spring5.collectiontype.Stu">
          <!--注入list集合类型，值是对象-->
          <property name="courseList">
              <list>
                  <ref bean="course1"></ref>
                  <ref bean="course2"></ref>
              </list>
          </property>
      </bean>
  
      <!--创建多个course对象-->
      <bean id="course1" class="com.atguigu.spring5.collectiontype.Course">
          <property name="cname" value="Spring5框架"></property>
      </bean>
      <bean id="course2" class="com.atguigu.spring5.collectiontype.Course">
          <property name="cname" value="MyBatis框架"></property>
      </bean>
  </beans>
  ~~~

##### FactoryBean 

bean类型可以和返回类型不一样

* Spring有两种类型bean，一种普通bean，另外一种工厂bean(FactoryBean)
  \> 普通bean：在配置文件中定义bean类型就是返回类型
  \> 工厂bean：在配置文件定义bean类型可以和返回类型不一样

* 工厂bean

  1. 创建类，让这个类作为工厂bean，即实现接口FactoryBean
  2. 实现接口里面的方法，在实现方法中定义返回的bean类型

  ~~~java
  public class MyBean implements FactoryBean<Course> {
  
      //定义返回bean
      @Override
      public Course getObject() throws Exception {
          Course course = new Course();
          course.setCname("abc");
          return course;
      }
  
      @Override
      public Class<?> getObjectType() {
          return null;
      }
  
      @Override
      public boolean isSingleton() {
          return false;
      }
  }
  ~~~

  ~~~xml
      <bean id="myBean" class="com.atguigu.spring5.factorybean.MyBean">
      </bean>
  ~~~

  ~~~java
  public class TestSpring5Demo1 {
      @Test
      public void test3() {
          ApplicationContext context =
                  new ClassPathXmlApplicationContext("bean3.xml");
          Course course = context.getBean("myBean", Course.class);
          System.out.println(course);
      }
  }
  ~~~

##### bean作用域

* 在Spring里面，默认情况下，bean是单实例对象

  ~~~java
  public class TestSpring5Demo1 {
      @Test
      public void testCollection2() {
          ApplicationContext context =
                  new ClassPathXmlApplicationContext("bean2.xml");
          Book book1 = context.getBean("book", Book.class);
          Book book2 = context.getBean("book", Book.class);
          System.out.println(book1);
          System.out.println(book2);
          // book1 book2 地址相同
      }
  }
  ~~~

* 设置多实例

  1. 在spring配置文件bean标签里面有属性（scope）用于设置单实例还是多实例
  2. scope属性值：默认值——singleton 单实例对象；prototype 多实例对象；

  ~~~xml
     <bean id="book" class="com.atguigu.spring5.collectiontype.Book" scope="prototype">
      </bean>
  <!-- book1 book2 地址不同 -->
  ~~~

* 注意：
  设置scope值是singleton时候，**加载spring配置文件**时候就会创建单实例对象
  设置scope值是prototype时候，在**调用getBean方法**时候创建多实例对象

##### bean生命周期

1. 通过无参构造器创建bean实例

2. 自动调用set方法，为bean的属性设置值和对其他bean引用

   bean的后置处理器：把bean实例传递bean后置处理器的方法——postProcessBeforeInitialization

3. 自动调用bean的初始化方法（需要进行配置初始化的方法）

   bean的后置处理器：把bean实例传递bean后置处理器的方法——postProcessAfterInitialization

4. 手动可调用bean的方法，获取到了bean对象

5. 手动调用bean的销毁方法，在容器关闭前（需要进行配置销毁的方法)

* 配置初始化方法和销毁方法

  ~~~java
  public class Orders {
  
      //无参数构造
      public Orders() {
          System.out.println("第一步 执行无参数构造创建bean实例");
      }
  
      private String oname;
      public void setOname(String oname) {
          this.oname = oname;
          System.out.println("第二步 调用set方法设置属性值");
      }
  
      //创建执行的初始化的方法
      public void initMethod() {
          System.out.println("第三步 执行初始化的方法");
      }
  
      //创建执行的销毁的方法
      public void destroyMethod() {
          System.out.println("第五步 执行销毁的方法");
      }
  }
  ~~~

  ~~~xml
  <!-- 配置 初始化方法 和 销毁方法 -->
      <bean id="orders" class="com.atguigu.spring5.bean.Orders" init-method="initMethod" destroy-method="destroyMethod">
          <property name="oname" value="手机"></property>
      </bean>
  ~~~

  ~~~java
  public class TestSpring5Demo1 {
      @Test
      public void testBean3() {
          ApplicationContext context =
                  new ClassPathXmlApplicationContext("bean4.xml");
          Orders orders = context.getBean("orders", Orders.class);
          System.out.println("第四步 获取创建bean实例对象");
          System.out.println(orders);
  
          //手动让bean实例销毁
          context.close();
      }
  }
  ~~~

* 添加bean的后置处理器

  \> 后置处理器会对所有在Spring配置的bean添加后置处理器
  \> 如果有多个后置处理器则按照配置后置处理器的顺序依次执行

  1. 创建类，实现接口 BeanPostProcessor，创建后置处理器

     ~~~java
     public class MyBeanPost implements BeanPostProcessor {
         @Override
         public Object postProcessBeforeInitialization(Object bean, String beanName) throws BeansException {
             System.out.println("在初始化之前执行的方法");
             return bean;
         }
         @Override
         public Object postProcessAfterInitialization(Object bean, String beanName) throws BeansException {
             System.out.println("在初始化之后执行的方法");
             return bean;
         }
     }
     ~~~

  2. 配置后置处理器

     ~~~xml
      <bean id="myBeanPost" class="com.atguigu.spring5.bean.MyBeanPost"></bean>
     ~~~

##### xml自动装配

* 根据指定装配规则（属性名称或者属性类型），Spring自动将匹配的属性值进行注入

* bean标签 属性autowire 用来配置自动装配
  属性值：byName 根据属性名称（即注入值bean的id值）注入；
                 byType 根据属性类型(即注入值bean的class值)注入;

* 以属性名称注入

  ~~~xml
      <bean id="emp" class="com.atguigu.spring5.autowire.Emp" autowire="byName">
          <!--<property name="dept" ref="dept"></property>-->
      </bean>
      <bean id="dept" class="com.atguigu.spring5.autowire.Dept"></bean>
  ~~~

* 以属性类型注入

  ```xml
  <bean id="emp" class="com.atguigu.spring5.autowire.Emp" autowire="byType">
      <!--<property name="dept" ref="dept"></property>-->
  </bean>
  <bean id="dept" class="com.atguigu.spring5.autowire.Dept"></bean>
  ```

##### 外部属性文件（例：配置数据库）

* 直接配置数据据信息

  1. 引入德鲁伊连接池依赖包 druid.jar

  2. 基于xml配置德鲁伊连接池

     ~~~xml
         <!--直接配置连接池-->
         <bean id="dataSource" class="com.alibaba.druid.pool.DruidDataSource">
             <property name="driverClassName" value="com.mysql.jdbc.Driver"></property>
             <property name="url" value="jdbc:mysql://localhost:3306/userDb"></property>
             <property name="username" value="root"></property>
             <property name="password" value="root"></property>
         </bean>
     ~~~

* 引入外部属性文件配置数据库连接池

  1. 引入德鲁伊连接池依赖包 druid.jar

  2. 创建外部属性文件，properties格式文件，写数据库信息

     ~~~properties
     prop.driverClass=com.mysql.jdbc.Driver
     prop.url=jdbc:mysql://localhost:3306/userDb
     prop.userName=root
     prop.password=root
     ~~~

  3. 把外部properties属性文件引入到spring配置文件中
     \> 引入context名称空间

     ~~~xml
     <?xml version="1.0" encoding="UTF-8"?>
     <beans xmlns="http://www.springframework.org/schema/beans"
            xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" 
            
            xmlns:context="http://www.springframework.org/schema/context" 
            
            xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
                                
                                http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd">
     </beans>
     ~~~

     \> 在spring配置文件使用标签引入外部属性文件

     ~~~xml
         <!--引入外部属性文件-->
     	<!-- classpath：==>> 工程路径下（到src目录下）-->
         <context:property-placeholder location="classpath:jdbc.properties"/>
     
         <!--配置连接池-->
     	<!-- ${属性} ==>> 引用外部文件中属性的属性值 -->
         <bean id="dataSource" class="com.alibaba.druid.pool.DruidDataSource">
             <property name="driverClassName" value="${prop.driverClass}"></property>
             <property name="url" value="${prop.url}"></property>
             <property name="username" value="${prop.userName}"></property>
             <property name="password" value="${prop.password}"></property>
         </bean>
     ~~~

#### 基于注解配置文件实现方式

* 注解是代码特殊标记
  格式：@注解名称(属性名称=属性值，属性名称=属性值)
* 注解作用在 类、方法、属性上
* 目的：简化xml配置

##### 创建对象提供注解

@Component(泛指组件)		@Service(业务层组件)	
@Controller(控制层组件)		@Repository(DAO层组件)

上面四个注解功能是一样的，都用来创建bean实例

1. 引入依赖 spring-aop

2. 开启组件扫描

   ~~~xml
   <!-- 如果扫描多个包，可以多个包使用逗号隔开，或者扫描包上层目录 -->
   <context:component-scan base-package="com.atguigu"></context:component-scan>
   ~~~

3. 创建类，在类上面添加创建对象注解
   \> 在注解里面value属性值可以省略不写。value 默认值是类名称，首字母小写

   ~~~java
   //<bean id="userService" class=".."/>
   @Repository(value = "userDaoImpl1")
   public class UserDaoImpl implements UserDao {
       @Override
       public void add() {
           System.out.println("dao add.....");
       }
   }
   ~~~

##### 组件扫描配置

* use-default-filters="false" 表示不使用默认 filter，自己配置filter
  \> context:include-filter 表示设置扫描哪些内容

  ~~~xml
  <context:component-scan base-package="com.atguigu" use-default-filters="false">
   <context:include-filter type="annotation" 
   
  expression="org.springframework.stereotype.Controller"/>
  </context:component-scan>
  <!-- 只扫描注解类型中的Controller注解 -->
  ~~~

  \> context:exclude-filter 表示哪些内容不进行扫描

  ~~~xml
  <context:component-scan base-package="com.atguigu">
   <context:exclude-filter type="annotation" 
   
  expression="org.springframework.stereotype.Controller"/>
  </context:component-scan>
  <!-- 不扫描注解类型中的Controller注解，其他都扫描 -->
  ~~~

  

##### 属性注入提供注解

* @Autowired 根据属性类型进行自动装配

  1. 创建service和dao类，在两个类添加创建对象注解

  2. 在service类添加dao类型属性，并在属性上面使用注解注入

     ~~~java
     @Service
     public class UserService {
         //不需要添加set方法
         //注入的是UserDao接口的实现类，如果有多个实现类，则需要通过属性名称方式进行注入
         @Autowired 
         private UserDao userDao;
     
         public void add() {
             System.out.println("service add......."+name);
             userDao.add();
         }
     }
     ~~~

* @Qualifier 根据属性名称进行自动装配

  ~~~java
  @Service
  public class UserService {
      //不需要添加set方法
      //@Qualifier注解要和@Autowired一起使用
      //@Qualifier中属性value的值对应注入类的value属性值
      @Autowired 
      @Qualifier(value = "userDaoImpl1")
      private UserDao userDao;
  
      public void add() {
          System.out.println("service add......."+name);
          userDao.add();
      }
  }
  
  @Repository(value = "userDaoImpl1")
  public class UserDaoImpl implements UserDao {
      @Override
      public void add() {
          System.out.println("dao add.....");
      }
  }
  ~~~

* @Resource 根据类型注入，也可以根据名称注入 
  此注解不是由Spring提供而是由javax提供，所以不建议使用

  ~~~JAVA
  @Service
  public class UserService {
      @Resource  // 方式一：根据类型进行注入 
      @Resource(name = "userDaoImpl1")  // 方式二：根据名称进行注入
      private UserDao userDao;
  
      public void add() {
          System.out.println("service add......."+name);
          userDao.add();
      }
  }
  ~~~

* @value 注入普通类型属性

  ~~~java
  @Service
  public class UserService {
      @Value(value = "abc")
      private String name;
  
      public void add() {
          System.out.println("service add......."+name);
          userDao.add();
      }
  }
  ~~~

##### 完全注解开发

1. 创建配置类，代替xml配置文件

   ~~~java
   @Configuration  //作为配置类，替代xml配置文件
   @ComponentScan(basePackages = {"com.atguigu"}) 
   //代替：<context:component-scan base-package="com.atguigu"></context:component-scan>
   public class SpringConfig {
   }
   ~~~

2. 编写测试类
   加载配置类：AnnotationConfigApplicationContext(Class clazz)

   ~~~java
   public class TestSpring5Demo1 {
       @Test
       public void testService2() {
           //加载配置类
           ApplicationContext context
                   = new AnnotationConfigApplicationContext(SpringConfig.class);
           UserService userService = context.getBean("userService", UserService.class);
           System.out.println(userService);
           userService.add();
       }
   }
   ~~~

## AOP

* 面向切面编程，利用AOP可以对业务逻辑的各个部分进行隔离，从而使得业务逻辑各个部分之间的耦合度降低，提高程序的可重用性，同时提高了开发的效率

  简而言之:不通过修改源代码方式，在主干功能里面添加新功能

* 例如：登录流程面向AOP形式
  ![图3](E:\软工\typora笔记\JavaEE框架\Spring5.assets\图3.png)

### 底层原理

* AOP底层用动态代理
  \> 情况一：有接口，使用jdk动态代理
  创建接口实现类代理对象，增强类的方法

  ![image-20211111131508358](E:\软工\typora笔记\JavaEE框架\Spring5.assets\image-20211111131508358.png)
  \> 情况二：没接口，使用CGLIB动态代理

  创建子类的代理对象，增强类的方法
  ![image-20211111131524527](E:\软工\typora笔记\JavaEE框架\Spring5.assets\image-20211111131524527.png)

* JDK动态代理
  使用Proxy类里面的方法 newProxyInstance 创建代理对象
  方法中的参数：
  参数一 —— 类加载器
  参数二 —— 增强方法所在的类实现的接口，支持多个接口
  参数三 —— 实现这个接口 InvocationHandler,创建代理对象，写增强的部分

  1. 创建接口，定义方法

     ~~~java
     public interface UserDao {
         public int add(int a,int b);
         public String update(String id);
     }
     ~~~

  2. 创建接口实现类，实现方法

     ~~~java
     public class UserDaoImpl implements UserDao {
         @Override
         public int add(int a, int b) {
             System.out.println("add方法执行了.....");
             return a+b;
         }
     
         @Override
         public String update(String id) {
             System.out.println("update方法执行了.....");
             return id;
         }
     }
     ~~~

  3. 使用Proxy类创建接口代理对象
     \> 匿名实现类方式（不推荐）

     ~~~java
     public class JDKProxy {
     
         public static void main(String[] args) {
             //创建接口实现类代理对象
             Class[] interfaces = {UserDao.class};
             UserDao dao = (UserDao)Proxy.newProxyInstance(JDKProxy.class.getClassLoader(), interfaces, new InvocationHandler() {
                 @Override
                 public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
                     return null;
                 }
             });
     
             int result = dao.add(1, 2);
             System.out.println("result:"+result);
         }
     }
     ~~~

     \> 非匿名实现类方式

     ~~~java
     public class JDKProxy {
     
         public static void main(String[] args) {
             //创建接口实现类代理对象
             Class[] interfaces = {UserDao.class};
             UserDaoImpl userDao = new UserDaoImpl();
             UserDao dao = (UserDao)Proxy.newProxyInstance(JDKProxy.class.getClassLoader(), interfaces, new UserDaoProxy(userDao)); //执行：②
             
             int result = dao.add(1, 2);  //执行：①
             System.out.println("result:"+result); //执行：⑤
         }
     }
     ~~~

     ~~~java
     //创建代理对象代码
     class UserDaoProxy implements InvocationHandler {
     
     	//1 把创建的是谁的代理对象，把谁传递过来
         private Object obj;
         public UserDaoProxy(Object obj) { //执行：③
             this.obj = obj;
         }
     
         //2 增强的逻辑
         @Override //执行：④
         public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
             //方法之前
             System.out.println("方法之前执行...."+method.getName()+" :传递的参数..."+ Arrays.toString(args));
     
             //被增强的方法执行
             Object res = method.invoke(obj, args);
     
             //方法之后
             System.out.println("方法之后执行...."+obj);
             return res;
         }
     }
     ~~~

### AOP术语

![图5](E:\软工\typora笔记\JavaEE框架\Spring5.assets\图5.png)
通知类型对应的注解：
			@Before							  前置通知
			@AfterReturning			   后置通知
			@After								 最终通知
			@AfterThrowing				异常通知
			@Around							环绕通知



### AOP操作

#### 概念

* Spring框架一般都是基于AspectJ实现AOP操作
  Aspect J不是Spring组成部分，独立AOP框架，一般把Aspect J和Spring框架一起使用进行Aop操作
* AspectJ实现AOP操作方式: 1. 基于 xml配置文件实现。 2. 基于注解方式实现

#### 前提提要

* AOP相关依赖：
  ![image-20211113121209815](E:\软工\typora笔记\JavaEE框架\Spring5.assets\image-20211113121209815.png)

* 切入点表达式
  \> 作用：知道对哪个类里面的哪个方法进行增强
  \> 语法结构：`execution([权限修饰符] [返回类型] [类全路径.][方法名称]([参数列表]))`

  ~~~java
  //举例 1：对 com.atguigu.dao.BookDao 类里面的 add 进行增强
  execution(* com.atguigu.dao.BookDao.add(..))
  
  //举例 2：对 com.atguigu.dao.BookDao 类里面的所有的方法进行增强
  execution(* com.atguigu.dao.BookDao.* (..))
  
  //举例 3：对 com.atguigu.dao 包里面所有类，类里面所有方法进行增强
  execution(* com.atguigu.dao.*.* (..))
  ~~~

#### 基于AspectJ注解方式实现

1. 创建类，并定义方法。
   ① 使用注解注入bean对象。

   ~~~java
   //被增强的类
   @Component
   public class User {
       public void add() {
           int i = 10/0;
           System.out.println("add.......");
       }
   }
   ~~~

2. 创建增强类，编写增强逻辑，不同的方法代表不同的通知类型。
   ① 使用注解注入bean对象，② 使用注解生成代理对象（@Aspect），③ 配置不同类型的通知。

   ```java
   //增强的类
   @Component // ①
   @Aspect  // ②
   public class UserProxy {
   
       //前置通知
       @Before(value = "execution(* com.atguigu.spring5.aopanno.User.add(..))")
       public void before() {
           System.out.println("before.........");
       }
   
       //后置通知（返回通知）
       @AfterReturning(value = "execution(* com.atguigu.spring5.aopanno.User.add(..))")
       public void afterReturning() {
           System.out.println("afterReturning.........");
       }
   
       //最终通知
       @After(value = "execution(* com.atguigu.spring5.aopanno.User.add(..))")
       public void after() {
           System.out.println("after.........");
       }
   
       //异常通知
       @AfterThrowing(value = "execution(* com.atguigu.spring5.aopanno.User.add(..))")
       public void afterThrowing() {
           System.out.println("afterThrowing.........");
       }
   
       //环绕通知
       @Around(value = "execution(* com.atguigu.spring5.aopanno.User.add(..))")
       public void around(ProceedingJoinPoint proceedingJoinPoint) throws Throwable {
           System.out.println("环绕之前.........");
   
           //被增强的方法执行
           proceedingJoinPoint.proceed();
   
           System.out.println("环绕之后.........");
       }
   }
   ```

   改进：相同的切入点抽取（@Pointcut）

   ~~~java
   @Component
   @Aspect  
   public class UserProxy {
   
       //相同切入点抽取
       @Pointcut(value = "execution(* com.atguigu.spring5.aopanno.User.add(..))")
       public void pointdemo() {
       }
   
       @Before(value = "pointdemo()")
       public void before() {
           System.out.println("before.........");
       }
   
       @AfterReturning(value = "pointdemo()")
       public void afterReturning() {
           System.out.println("afterReturning.........");
       }
   }
   ~~~

3. 在Spring配置文件中，开启注解扫描

   ~~~xml
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns="http://www.springframework.org/schema/beans"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xmlns:context="http://www.springframework.org/schema/context"
          xmlns:aop="http://www.springframework.org/schema/aop"
          xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
                           http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd
                           http://www.springframework.org/schema/aop http://www.springframework.org/schema/aop/spring-aop.xsd">
       
       <!-- 开启注解扫描 -->
       <context:component-scan base-package="com.atguigu.spring5.aopanno"></context:component-scan>
   
       <!-- 开启Aspect生成代理对象-->
       <aop:aspectj-autoproxy></aop:aspectj-autoproxy>
   </beans>
   ~~~

   改进：完全使用注解开发。创建配置类，不需要创建xml配置文件。

   ~~~java
   @Configuration
   @ComponentScan(basePackages = {"com.atguigu"})
   @EnableAspectJAutoProxy(proxyTargetClass = true)
   public class ConfigAop {
   }
   ~~~

4. 有多个增强类都同一个方法进行增强，需设置增强类优先级（@Order）

   ~~~java
   @Component
   @Aspect
   @Order(1)
   public class PersonProxy {
       @Before(value = "execution(* com.atguigu.spring5.aopanno.User.add(..))")
       public void afterReturning() {
           System.out.println("Person Before.........");
       }
   }
   ~~~

   ~~~java
   @Component
   @Aspect 
   @Order(3)
   public class UserProxy {
       @Before(value = "execution(* com.atguigu.spring5.aopanno.User.add(..))")
       public void before() {
           System.out.println("before.........");
       }
   }
   ~~~

5. 测试

   ~~~java
   public class TestAop {
       @Test //在spring配置文件中，开启注解扫描
       public void testAopAnno() {
           ApplicationContext context =
                   new ClassPathXmlApplicationContext("bean1.xml");
           User user = context.getBean("user", User.class);
           user.add();
       }
       
       @Test //完全注解开发
       public void testAopAnno() {
           ApplicationContext context
                   = new AnnotationConfigApplicationContext(ConfigAop.class);
           User user = context.getBean("user", User.class);
           user.add();
       }
   }
   ~~~

#### 基于AspectJ配置文件实现方式

1. 创建两个类，增强类和被增强类，创建各类方法

2. 在spring配置文件中创建两个类对象

   ~~~xml
   <bean id="book" class="com.atguigu.spring5.aopxml.Book"></bean>
   <bean id="bookProxy" class="com.atguigu.spring5.aopxml.BookProxy"></bean>
   ~~~

3. 在spring配置文件中配置切入点

   ~~~xml
   <!--配置 aop 增强-->
   <aop:config>
        <!--切入点-->
        <aop:pointcut id="p" expression="execution(* 
       com.atguigu.spring5.aopxml.Book.buy(..))"/>
       
        <!--配置切面-->
        <aop:aspect ref="bookProxy">
            <!--增强作用在具体的方法上-->
            <aop:before method="before" pointcut-ref="p"/>
        </aop:aspect>
   </aop:config>
   ~~~

## JdbcTemplate

* Spring框架对JDBC进行封装，使用JDBCTemplate方便实现对数据库操作

### 配置数据库文件

1. 引入相关依赖
   `spring-jdbc-5.2.6.RELEASE.jar`
   `spring-orm-5.2.6.RELEASE.jar`
   `spring-tx-5.2.6.RELEASE.jar`
   `mysql-connector-java-5.1.7-bin.jar`
   `druid-1.1.9.jar`

2. 在spring配置文件中配置数据库连接池

   ~~~xml
       <!-- 数据库连接池 -->
       <bean id="dataSource" class="com.alibaba.druid.pool.DruidDataSource"
             destroy-method="close">
           <property name="url" value="jdbc:mysql:///user_db" />
           <property name="username" value="root" />
           <property name="password" value="root" />
           <property name="driverClassName" value="com.mysql.jdbc.Driver" />
       </bean>
   ~~~

3. 配置JdbcTemplate对象，注入DataSource

   ~~~xml
       <!-- JdbcTemplate对象 -->
       <bean id="jdbcTemplate" class="org.springframework.jdbc.core.JdbcTemplate">
           <!--注入dataSource-->
           <property name="dataSource" ref="dataSource"></property>
       </bean>
   ~~~

4. 创建service类，Dao类，在Dao注入JdbcTemplate对象

   ~~~java
   @Service
   public class UserService {
       //注入dao
       @Autowired
       private UserDao userDao;
   }
   ~~~

   ~~~java
   @Repository
   public class UserDaoImpl{
       @Autowired
       private JdbcTemplate jdbcTemplate;
   }
   ~~~

   ~~~xml
       <!-- 组件扫描 -->
       <context:component-scan base-package="com.atguigu"></context:component-scan>
   ~~~

### JdbcTemplate操作数据库（增删改查）

| update(String sql,Object ... args) | 添加、修改、删除 |
| ---------------------------------- | ---------------- |

* 添加

  ~~~java
   public void add(Book book) {
       //1 创建 sql 语句
       String sql = "insert into t_book values(?,?,?)";
       //2 调用方法实现
       Object[] args = {book.getUserId(), book.getUsername(), 
       book.getUstatus()};
       int update = jdbcTemplate.update(sql,args);
       System.out.println(update);
   }
  ~~~

* 修改

  ~~~java
  @Override
  public void updateBook(Book book) {
       String sql = "update t_book set username=?,ustatus=? where user_id=?";
       Object[] args = {book.getUsername(), book.getUstatus(),book.getUserId()};
       int update = jdbcTemplate.update(sql, args);
       System.out.println(update);
  }
  ~~~

* 删除

  ~~~java
  @Override
  public void delete(String id) {
       String sql = "delete from t_book where user_id=?";
       int update = jdbcTemplate.update(sql, id);
       System.out.println(update);
  }
  ~~~

* 测试

  ~~~java
  @Test
  public void testJdbcTemplate() {
       ApplicationContext context =
       new ClassPathXmlApplicationContext("bean1.xml");
       BookService bookService = context.getBean("bookService", 
       BookService.class);
       Book book = new Book();
       book.setUserId("1");
       book.setUsername("java");
       book.setUstatus("a");
       bookService.addBook(book); //测试添加
  }
  ~~~

| 查询方法                                                     | 说明                                                         |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| queryForObject(String sql, Class<T> requiredType)            | 查找（返回一个值）                                           |
| queryForObject(String sql, RowMapper<T> rowMapper,Object ... args) | 查找（返回一个对象）<br />RowMapper是接口，针对返回不同类型数据，使用这个接口里面实现类完成数据封装 |
| query(String sql, RowMapper<T> rowMapper,Object ... args)    | 查找（返回对象集合）                                         |

* 返回一个值

  ~~~java
  @Override
  public int selectCount() {
       String sql = "select count(*) from t_book";
       Integer count = jdbcTemplate.queryForObject(sql, Integer.class);
       return count;
  }
  ~~~

* 返回一个对象

  ~~~java
  @Override
  public Book findBookInfo(String id) {
       String sql = "select * from t_book where user_id=?";
       //调用方法
       Book book = jdbcTemplate.queryForObject(sql, new 
       BeanPropertyRowMapper<Book>(Book.class), id);
       return book;
  }
  ~~~

* 返回对象集合

  ~~~java
  @Override
  public List<Book> findAllBook() {
       String sql = "select * from t_book";
       //调用方法
       List<Book> bookList = jdbcTemplate.query(sql, new 
      BeanPropertyRowMapper<Book>(Book.class));
       return bookList;
  }
  ~~~

| batchUpdate(String sql, List<Object[]> batchArgs) | 批量添加、修改、删除 |
| ------------------------------------------------- | -------------------- |

* 批量添加

  ~~~java
  @Override
  public void batchAddBook(List<Object[]> batchArgs) {
       String sql = "insert into t_book values(?,?,?)";
       int[] ints = jdbcTemplate.batchUpdate(sql, batchArgs);
       System.out.println(Arrays.toString(ints));
  }
  ~~~

* 批量修改

  ~~~java
  @Override
  public void batchUpdateBook(List<Object[]> batchArgs) {
       String sql = "update t_book set username=?,ustatus=? where user_id=?";
       int[] ints = jdbcTemplate.batchUpdate(sql, batchArgs);
       System.out.println(Arrays.toString(ints));
  }
  ~~~

* 批量删除

  ~~~java
  @Override
  public void batchDeleteBook(List<Object[]> batchArgs) {
       String sql = "delete from t_book where user_id=?";
       int[] ints = jdbcTemplate.batchUpdate(sql, batchArgs);
       System.out.println(Arrays.toString(ints));
  }
  ~~~

* 测试

  ~~~java
  @Test
  public void testJdbcTemplate() {
      ApplicationContext context =
      new ClassPathXmlApplicationContext("bean1.xml");
      BookService bookService = context.getBean("bookService", 
      BookService.class);
      
      List<Object[]> batchArgs = new ArrayList<>();
      Object[] o1 = {"3","java","a"};
      Object[] o2 = {"4","c++","b"};
      Object[] o3 = {"5","MySQL","c"};
      batchArgs.add(o1);
      batchArgs.add(o2);
      batchArgs.add(o3);
      bookService.batchAdd(batchArgs);//测试批量添加
  }
  ~~~

  ~~~java
  @Test
  public void testJdbcTemplate() {
      ApplicationContext context =
      new ClassPathXmlApplicationContext("bean1.xml");
      BookService bookService = context.getBean("bookService", 
      BookService.class);
      
      List<Object[]> batchArgs = new ArrayList<>();
      Object[] o1 = {"java0909","a3","3"};
      Object[] o2 = {"c++1010","b4","4"};
      Object[] o3 = {"MySQL1111","c5","5"};
      batchArgs.add(o1);
      batchArgs.add(o2);
      batchArgs.add(o3);
      bookService.batchUpdate(batchArgs);//测试批量修改
  }
  ~~~

  ~~~java
  @Test
  public void testJdbcTemplate() {
      ApplicationContext context =
      new ClassPathXmlApplicationContext("bean1.xml");
      BookService bookService = context.getBean("bookService", 
      BookService.class);
      
      List<Object[]> batchArgs = new ArrayList<>();
      Object[] o1 = {"3"};
      Object[] o2 = {"4"};
      batchArgs.add(o1);
      batchArgs.add(o2);
      bookService.batchDelete(batchArgs);//测试批量删除
  }
  ~~~

### 事务操作

以转账为例讲解：

![图6](E:\软工\typora笔记\JavaEE框架\Spring5.assets\图6.png)

1. 创建相关类，编写相关方法，同时创建对象，注入对象

   ~~~java
   public interface UserDao {
       public void addMoney();    //多钱
       public void reduceMoney();    //少钱
   }
   ~~~

   ~~~java
   @Repository
   public class UserDaoImpl implements UserDao {
        @Autowired
        private JdbcTemplate jdbcTemplate;
       
       @Override
       public void reduceMoney() {
           String sql = "update t_account set money=money-? where username=?";
           jdbcTemplate.update(sql,100,"lucy");
       }
   
       @Override
       public void addMoney() {
           String sql = "update t_account set money=money+? where username=?";
           jdbcTemplate.update(sql,100,"mary");
       }
   }
   ~~~

   ~~~java
   @Service
   public class UserService {
        @Autowired
        private UserDao userDao;
       
        //转账的方法
        public void accountMoney() {
   		userDao.reduceMoney();
           userDao.addMoney();
        }
   }
   ~~~

2. 添加事务（避免代码执行过程中出现异常时，业务出现问题）——编程式事务管理

   ~~~java
   public void accountMoney() {
       try {
           //第一步 开启事务
           //第二步 进行业务操作
           userDao.reduceMoney();
           int i = 10/0;//模拟异常
           userDao.addMoney();
           //第三步 没有发生异常，提交事务
       }catch(Exception e) {
           //第四步 出现异常，事务回滚
       }
   }
   ~~~

#### Spring事务管理

* 事务添加到Java EE三层结构里面Service层（业务逻辑层）
* Spring进行事务管理操作方式：1. 编程式事务管理 2. 声明式事务管理
  声明式事务管理方式：1. 基于注解方式 2. 基于xml配置文件方式
* Spring 进行声明式事务管理，底层使用AOP原理
* Spring事务管理API提供一个接口，代表事务管理器（PlatformTransactionManager）
  这个接口针对不同的框架提供不同的实现类 （DataSourceTransactionManager）

#### 基于注解方式声明式事务管理

1. Spring配置文件配置事务管理器

   ~~~xml
       <bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
           <!--注入数据源-->
           <property name="dataSource" ref="dataSource"></property>
       </bean>
   ~~~

2. Spring配置文件中开启事务注解
   ① 引入名称空间 tx ② 开启事务注解

   ~~~xml
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns="http://www.springframework.org/schema/beans"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xmlns:context="http://www.springframework.org/schema/context"
          xmlns:aop="http://www.springframework.org/schema/aop"
          xmlns:tx="http://www.springframework.org/schema/tx"
          xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
                           http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd
                           http://www.springframework.org/schema/aop http://www.springframework.org/schema/aop/spring-aop.xsd
                           http://www.springframework.org/schema/tx http://www.springframework.org/schema/tx/spring-tx.xsd">
      <tx:annotation-driven transaction-manager="transactionManager"></tx:annotation-driven>
   ~~~

3. 在service类或service类里面的方法上面添加事务注解
   @Transactional, 这个注解添加到类或方法上面。
   如果添加到类上面，则类上所有方法都添加事务；如果添加到方法上面，则只为这个方法添加事务

   ~~~java
   @Service
   @Transactional
   public class UserService {
       @Autowired
       private UserDao userDao;
   
       public void accountMoney() {
               userDao.reduceMoney();
               int i = 10/0;
               userDao.addMoney();
       }
   }
   ~~~

##### 参数配置

* 在service类上面添加注解@Transactional，在这个注解里面可以配置事务相关参数
  propagation()	isolation()	timeout()	readOnly()	rollbackFor()	noRollbackFor()

* 事务传播行为：propagation()


  ![事务传播行为](E:\软工\typora笔记\JavaEE框架\Spring5.assets\事务传播行为.bmp)

  如下例子：

  ![图7](E:\软工\typora笔记\JavaEE框架\Spring5.assets\图7.png)

* 事务隔离级别：isolation()
  ![事务隔离级别](E:\软工\typora笔记\JavaEE框架\Spring5.assets\事务隔离级别.bmp)

  \> 脏读：一个未提交事务读取到另一个未提交事务的数据

  \> 不可重复读：一个未提交事务读取到另一个提交事务修改数据

  ![图8](E:\软工\typora笔记\JavaEE框架\Spring5.assets\图8.png)\> 虚读：一个未提交事务读取到另一提交事务添加数据

* 超时时间：timeout()
  \> 事务需要在一定时间内进行提交，如果不提交进行回滚
  \> 默认值是-1，设置时间以秒单位进行计算
* 是否只读：readOnly()
  \> 读：查询操作，写：添加删除修改操作
  \> readOnly默认值false，表示可以改查
  \> 设置readOnly值是true，只能查询
* 回滚：rollbackFor()
  \> 设置出现哪些异常进行事务回滚
* 不回滚：noRollbackFor()
  \> 设置出现哪些异常不进行事务回滚

~~~java
@Service
@Transactional(readOnly = false,timeout = -1,propagation = Propagation.REQUIRED,isolation = Isolation.REPEATABLE_READ)
public class UserService {
    @Autowired
    private UserDao userDao;

    public void accountMoney() {
            userDao.reduceMoney();
            int i = 10/0;
            userDao.addMoney();
    }
}
~~~



#### 基于xml方式声明式事务管理

1. 配置事务管理器

   ~~~xml
       <bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
           <!--注入数据源-->
           <property name="dataSource" ref="dataSource"></property>
       </bean>
   ~~~

2. 配置通知

   ~~~xml
       <tx:advice id="txadvice">
           <!--配置事务参数-->
           <tx:attributes>
               <!--指定哪种规则的方法上面添加事务-->
               <tx:method name="accountMoney" propagation="REQUIRED"/>
               <tx:method name="account*"/>
           </tx:attributes>
       </tx:advice>
   ~~~

3. 配置切入点和切面

   ~~~xml
       <aop:config>
           <!--配置切入点-->
           <aop:pointcut id="pt" expression="execution(* com.atguigu.spring5.service.UserService.*(..))"/>
           <!--配置切面-->
           <aop:advisor advice-ref="txadvice" pointcut-ref="pt"/>
       </aop:config>
   ~~~

### 完全注解声明式事务管理

创建配置类，代替xml文件

~~~java
@Configuration //配置类
@ComponentScan(basePackages = "com.atguigu") //组件扫描
@EnableTransactionManagement //开启事务
public class TxConfig {

    //创建数据库连接池
    @Bean
    public DruidDataSource getDruidDataSource() {
        DruidDataSource dataSource = new DruidDataSource();
        dataSource.setDriverClassName("com.mysql.jdbc.Driver");
        dataSource.setUrl("jdbc:mysql:///user_db");
        dataSource.setUsername("root");
        dataSource.setPassword("root");
        return dataSource;
    }

    //创建JdbcTemplate对象
    @Bean
    public JdbcTemplate getJdbcTemplate(DataSource dataSource) {
        //到ioc容器中根据类型找到dataSource
        JdbcTemplate jdbcTemplate = new JdbcTemplate();
        //注入dataSource
        jdbcTemplate.setDataSource(dataSource);
        return jdbcTemplate;
    }

    //创建事务管理器
    @Bean
    public DataSourceTransactionManager getDataSourceTransactionManager(DataSource dataSource) {
        DataSourceTransactionManager transactionManager = new DataSourceTransactionManager();
        transactionManager.setDataSource(dataSource);
        return transactionManager;
    }
}
~~~

~~~java
public class TestBook {
    @Test
    public void testAccount2() {
        ApplicationContext context =
                new AnnotationConfigApplicationContext(TxConfig.class);
        UserService userService = context.getBean("userService", UserService.class);
        userService.accountMoney();
    }
}
~~~

