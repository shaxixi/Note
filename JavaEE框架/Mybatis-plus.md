[toc]

# Mybatis-plus

* MyBatis-Plus（简称 MP）是一个 MyBatis 的增强工具，在 MyBatis 的基础上只做增强不做改变，为简化开发、提高效率而生。
* 官网： https://mp.baomidou.com/
  文档地址：https://mybatis.plus/guide/
  源码地址：https://github.com/baomidou/mybatis-plus
  码云地址：https://gitee.com/organizations/baomidou
  MP的配置：https://mybatis.plus/config/
* 特性
  1. 无侵入 ：只做增强不做改变，引入它不会对现有工程产生影响，如丝般顺滑
  2. 损耗小 ：启动即会自动注入基本 CURD，性能基本无损耗，直接面向对象操作
  3. 强大的 CRUD 操作：内置通用 Mapper、通用 Service，仅仅通过少量配置即可实现单大部分 CRUD 操作，更有强大的条件构造器，满足各类使用需求
  4. 支持 Lambda 形式调用：通过 Lambda 表达式，方便的编写各类查询条件，无需再担心字段写错
  5. 支持多种数据库 ：支持 MySQL、MariaDB、Oracle、DB2、H2、HSQL、SQLitePostgre、SQLServer2005、SQLServer 等多种数据库
  6. 支持主键自动生成 ：支持多达 4 种主键策略（内含分布式唯一 ID 生成器 - Sequence），可自由配置，完美解决主键问题
  7. 支持 XML 热加载：Mapper 对应的 XML 支持热加载，对于简单的 CRUD 操作，甚至可以无 XML 启动
  8. 支持 ActiveRecord 模式：支持 ActiveRecord 形式调用，实体类只需继承 Model 类即可进行强大的 CRUD 操作
  9. 支持自定义全局通用操作 ：支持全局通用方法注入（ Write once, use anywhere ）
  10. 支持关键词自动转义 ：支持数据库关键词（order、key......）自动转义，还可自定义关键词
  11. 内置代码生成器 ：采用代码或者 Maven 插件可快速生成 Mapper 、 Model 、 Service、 Controller 层代码，支持模板引擎，更有超多自定义配置等您来使用
  12. 内置分页插件 ：基于 MyBatis 物理分页，开发者无需关心具体操作，配置好插件之后，写分页等同于普通 List查询
  13. 内置性能分析插件 ：可输出 Sql 语句以及其执行时间，建议开发测试时启用该功能，能快速揪出慢查询
  14. 内置全局拦截插件 ：提供全表 delete 、 update 操作智能分析阻断，也可自定义拦截规则，预防误操作
  15. 内置 Sql 注入剥离器：支持 Sql 注入剥离，有效预防 Sql 注入攻击

## Mybatis-plus和其他框架的整合

1. 依赖包
   \> mybatis 和 mybatis-plus 整合、Spring 和 mybatis-plus 整合

   ~~~xml
   <!-- mybatis-plus插件依赖 -->
       <dependency>
         <groupId>com.baomidou</groupId>
         <artifactId>mybatis-plus</artifactId>
         <version>3.1.1</version>
       </dependency>
   ~~~

   \> mybatis-plus 和 springboot 整合 **（主要）**

   ~~~xml
   <!--mybatis-plus的springboot支持-->
       <dependency>
         <groupId>com.baomidou</groupId>
         <artifactId>mybatis-plus-boot-starter</artifactId>
         <version>3.1.1</version>
       </dependency>
   ~~~

2. 配置文件
   \> mybatis 和 mybatis-plus 整合

   ~~~xml
   <configuration>
     <environments default="development">
       <environment id="development">
         <transactionManager type="JDBC"/>
         <dataSource type="POOLED">
           <property name="driver" value="com.mysql.jdbc.Driver"/>
           <property name="url" value="jdbc:mysql://127.0.0.1:3306/mp?
   useUnicode=true&amp;characterEncoding=utf8&amp;autoReconnect=true&amp;allowMultiQuerie
   s=true&amp;useSSL=false"/>
           <property name="username" value="root"/>
           <property name="password" value="root"/>
         </dataSource>
       </environment>
     </environments>
     <mappers>
       <mapper resource="UserMapper.xml"/>
     </mappers>
   </configuration>
   ~~~

   \> Spring 和 mybatis-plus 整合

   ~~~xml
   <context:property-placeholder location="classpath:*.properties"/>
     <!-- 定义数据源 -->
     <bean id="dataSource" class="com.alibaba.druid.pool.DruidDataSource"
        destroy-method="close">
       <property name="url" value="${jdbc.url}"/>
       <property name="username" value="${jdbc.username}"/>
       <property name="password" value="${jdbc.password}"/>
       <property name="driverClassName" value="${jdbc.driver}"/>
       <property name="maxActive" value="10"/>
       <property name="minIdle" value="5"/>
     </bean>
     <!--这里使用MP提供的sqlSessionFactory，完成了Spring与MP的整合-->
     <bean id="sqlSessionFactory"
   class="com.baomidou.mybatisplus.extension.spring.MybatisSqlSessionFactoryBean">
       <property name="dataSource" ref="dataSource"/>
     </bean>
     <!--扫描mapper接口，使用的依然是Mybatis原生的扫描器-->
     <bean class="org.mybatis.spring.mapper.MapperScannerConfigurer">
       <property name="basePackage" value="cn.itcast.mp.simple.mapper"/>
     </bean>
   </beans>
   ~~~

   \> mybatis-plus 和 springboot 整合

   ~~~properties
   spring.application.name = itcast-mp-springboot
   spring.datasource.driver-class-name=com.mysql.jdbc.Driver
   spring.datasource.url=jdbc:mysql://127.0.0.1:3306/mp?
   useUnicode=true&characterEncoding=utf8&autoReconnect=true&allowMultiQueries=true&useSSL
   =false
   spring.datasource.username=root
   spring.datasource.password=root
   ~~~

   

3. 映射文件
   \> mybatis 和 mybatis-plus 整合

   ~~~xml
   <mapper namespace="cn.itcast.mp.simple.mapper.UserMapper">
     <select id="findAll" resultType="cn.itcast.mp.simple.pojo.User">
     select * from tb_user
     </select>
   </mapper>
   ~~~

4. 实体类**（主要）**

   ~~~java
   @Data //lombok
   @NoArgsConstructor
   @AllArgsConstructor
   // mybatis-plus 注解
   @TableName("tb_user")
   public class User {
     private Long id;
     private String userName;
     private String password;
     private String name;
     private Integer age;
     private String email;
   }
   ~~~

5. Mapper继承BaseMapper**（主要）**

   ~~~java
   public interface UserMapper 
     // mybatis-plus
     extends BaseMapper<User> 
   {
     List<User> findAll();
   }
   ~~~

6. 调用父类方法
   \> mybatis 和 mybatis-plus 整合

   ~~~java
   public class TestMybatisPlus {
     @Test
     public void testUserList() throws Exception{
       String resource = "mybatis-config.xml";
       InputStream inputStream = Resources.getResourceAsStream(resource);
         
       // mybatis
       SqlSessionFactory sqlSessionFactory = new
   SqlSessionFactoryBuilder().build(inputStream);
       // mybatis-plus 这里使用的是MP中的MybatisSqlSessionFactoryBuilder
       SqlSessionFactory sqlSessionFactory = new
   MybatisSqlSessionFactoryBuilder().build(inputStream);
         
       SqlSession sqlSession = sqlSessionFactory.openSession();      
       UserMapper userMapper = sqlSession.getMapper(UserMapper.class);
       // mybatis
       List<User> list = userMapper.findAll();
       // 可以调用BaseMapper中定义的方法
       List<User> list = userMapper.selectList(null);
       for (User user : list) {
         System.out.println(user);
      }
    }
   }
   ~~~

   \> Spring 和 mybatis-plus 整合、mybatis-plus 和 springboot 整合

   ~~~java
   @RunWith(SpringJUnit4ClassRunner.class)
   //Spring 和 mybatis-plus
   @ContextConfiguration(locations = "classpath:applicationContext.xml")
   //mybatis-plus 和 springboot
   @SpringBootTest
   public class TestSpringMP {
     @Autowired
     private UserMapper userMapper;
     @Test
     public void testSelectList(){
       List<User> users = this.userMapper.selectList(null);
       for (User user : users) {
         System.out.println(user);
      }
    }
   }
   ~~~

   

## 基本语法

### 实体类注解

| 注解                             | 说明                                                         | 作用于 |
| -------------------------------- | ------------------------------------------------------------ | ------ |
| @TableName("表名")               | 指定数据库表名                                               | 类     |
| @TableId(type = **IdType.AUTO**) | 指定键的类型                                                 | 属性   |
| @TableField(键值)                | 指定字段的一些属性<br />①对象中的属性名和字段名不一致的问题（非驼峰）<br />【键值 value=“表名”】<br />② 对象中的属性字段在表中不存在的问题<br />【键值 exist=“表名”】<br />③ 字段不加入查询字段<br />【键值 select=false \| true】<br />④ 自动填充功能<br />【键值 fill = FieldFill.INSERT】 | 属性   |
| @TableLogic                      | 逻辑删除                                                     | 属性   |

> IdType类型的枚举：
>
> ~~~java
> /* 数据库ID自增 */ AUTO(0),
> /* 该类型为未设置主键类型 */ NONE(1),
> /* 用户输入ID <p>该类型可以通过自己注册自动填充插件进行填充</p> */ INPUT(2),
> /* 以下3种类型、只有当插入对象ID 为空，才自动填充。 */
> /* 全局唯一ID (idWorker) */ ID_WORKER(3),
> /* 全局唯一ID (UUID) */ UUID(4),
> /* 字符串全局唯一ID (idWorker 的字符串表示) */ ID_WORKER_STR(5);
> ~~~
>
> FieldFill类型的枚举：
>
> ~~~java
> /* 默认不处理 */ DEFAULT,
> /* 插入时填充字段 */ INSERT,
> /* 更新时填充字段 */ UPDATE,
> /* 插入和更新时填充字段 */ INSERT_UPDATE
> ~~~

* 【自动填充功能的FieldFill类型实现】MetaObjectHandler实现类：

  ~~~java
  @Component
  public class MyMetaObjectHandler implements MetaObjectHandler {
    @Override
    public void insertFill(MetaObject metaObject) {
      Object password = getFieldValByName("password", metaObject);
      if(null == password){
        //字段为空，可以进行填充
        setFieldValByName("password", "123456", metaObject);
     }
   }
    @Override
    public void updateFill(MetaObject metaObject) {
   }
  }
  ~~~

* 【逻辑删除】配置：数据库建表时可以注释不同数字代表不同含义

  ~~~properties
  # 逻辑已删除值(默认为 1)
  mybatis-plus.global-config.db-config.logic-delete-value=1
  # 逻辑未删除值(默认为 0)
  mybatis-plus.global-config.db-config.logic-not-delete-value=0
  ~~~

### BaseMapper通用CRUD

* 插入一条记录：

  > `int insert(T entity);`
  > @param entity 实体对象

  ~~~java
  @RunWith(SpringRunner.class)
  @SpringBootTest
  public class UserMapperTest {
    @Autowired
    private UserMapper userMapper;
    @Test
    public void testInsert(){
      User user = new User();
      user.setAge(20);
      user.setEmail("test@itcast.cn");
      user.setName("曹操");
      user.setUserName("caocao");
      user.setPassword("123456");
      //1. 返回的result是受影响的行数，并不是自增
      int result = this.userMapper.insert(user); 
  后的id
      System.out.println("result = " + result);
      //2. 自增后的id会回填到对象中
      System.out.println(user.getId()); 
   }
  }
  ~~~

* 更新

  > 1. 根据 ID 修改
  >    `int updateById(@Param(Constants.ENTITY) T entity);`
  >    @param entity 实体对象
  > 2. 根据 whereEntity 条件，更新记录
  >    `int update(@Param(Constants.ENTITY) T entity, @Param(Constants.WRAPPER) Wrapper<T> updateWrapper);`
  >    @param entity    实体对象 (set 条件值,可以为 null)
  >    @param updateWrapper 实体对象封装操作类（可以为 null,里面的 entity 用于生成 where 语句）

  ~~~java
  @RunWith (SpringRunner.class)
  @SpringBootTest
  public class UserMapperTest {
    @Autowired
    private UserMapper userMapper;
    @Test
    public void testUpdateById() {
      User user = new User();
      user.setId(6L); //主键
      user.setAge(21); //更新的字段
      //1. 根据id更新，更新不为null的字段
      this.userMapper.updateById(user);
   }
      
    @Test
    public void testUpdate_1() {
      User user = new User();
      //更新的字段
      user.setAge(22); 
      //更新的条件
      QueryWrapper<User> wrapper = new QueryWrapper<>();
      wrapper.eq("id", 6);
      //执行更新操作
      int result = this.userMapper.update(user, wrapper);
      System.out.println("result = " + result);
   }
      
    @Test
    public void testUpdate_2() {
      //更新的条件以及字段
      UpdateWrapper<User> wrapper = new UpdateWrapper<>();
      wrapper.eq("id", 6).set("age", 23);
      //执行更新操作
      int result = this.userMapper.update(null, wrapper);
      System.out.println("result = " + result);
   }
  }
  ~~~

* 删除

  > 1. 根据 ID 删除
  >    `int deleteById(Serializable id);`
  >    @param id 主键ID
  > 2. 根据 columnMap 条件，删除记录
  >    `int deleteByMap(@Param(Constants.COLUMN_MAP) Map<String, Object> columnMap);`
  >    @param columnMap 表字段 map 对象
  > 3. 根据 entity 条件，删除记录
  >    `int delete(@Param(Constants.WRAPPER) Wrapper<T> wrapper);`
  >    @param wrapper 实体对象封装操作类（可以为 null）
  > 4. 删除（根据ID 批量删除）
  >    `int deleteBatchIds(@Param(Constants.COLLECTION) Collection<? extends Serializable> idList);`
  >    @param idList 主键ID列表(不能为 null 以及 empty)

  ~~~java
  @RunWith(SpringRunner.class)
  @SpringBootTest
  public class UserMapperTest {
    @Autowired
    private UserMapper userMapper;
    @Test
    public void testDeleteById() {
      //执行删除操作
      int result = this.userMapper.deleteById(6L);
      System.out.println("result = " + result);
   }
      
    @Test
    public void testDeleteByMap() {
      Map<String, Object> columnMap = new HashMap<>();
      columnMap.put("age",20);
      columnMap.put("name","张三");
      //将columnMap中的元素设置为删除的条件，多个之间为and关系
      int result = this.userMapper.deleteByMap(columnMap);
      System.out.println("result = " + result);
   }
      
    @Test
    public void testDelete() {
      User user = new User();
      user.setAge(20);
      user.setName("张三");
      //将实体对象进行包装，包装为操作条件
      QueryWrapper<User> wrapper = new QueryWrapper<>(user);
      int result = this.userMapper.delete(wrapper);
      System.out.println("result = " + result);
   }
      
    @Test
    public void testDeleteByMap() {
      //根据id集合批量删除
      int result = this.userMapper.deleteBatchIds(Arrays.asList(1L,10L,20L));
      System.out.println("result = " + result);
   }
  }
  ~~~

* 查询

  > 1. 根据 ID 查询
  >    `T selectById(Serializable id);`
  >    @param id 主键ID
  > 2. 查询（根据ID 批量查询）
  >    `List<T> selectBatchIds(@Param(Constants.COLLECTION) Collection<? extends Serializable> idList);`
  >    @param idList 主键ID列表(不能为 null 以及 empty)
  > 3. 根据 entity 条件，查询一条记录
  >    `T selectOne(@Param(Constants.WRAPPER) Wrapper<T> queryWrapper);`
  >    @param queryWrapper 实体对象封装操作类（可以为 null）
  > 4. 根据 Wrapper 条件，查询总记录数
  >    `Integer selectCount(@Param(Constants.WRAPPER) Wrapper<T> queryWrapper);`
  >    @param queryWrapper 实体对象封装操作类（可以为 null）
  > 5. 根据 entity 条件，查询全部记录
  >    `List<T> selectList(@Param(Constants.WRAPPER) Wrapper<T> queryWrapper);`
  >    @param queryWrapper 实体对象封装操作类（可以为 null）

  ~~~java
  @RunWith(SpringRunner.class)
  @SpringBootTest
  public class UserMapperTest {
    @Autowired
    private UserMapper userMapper;
    @Test
    public void testSelectById() {
      //根据id查询数据
      User user = this.userMapper.selectById(2L);
      System.out.println("result = " + user);
   }
      
    @Test
    public void testSelectBatchIds() {
      //根据id集合批量查询
      List<User> users = this.userMapper.selectBatchIds(Arrays.asList(2L, 3L, 10L));
      for (User user : users) {
        System.out.println(user);
     }
   }
      
    @Test
    public void testSelectOne() {
      QueryWrapper<User> wrapper = new QueryWrapper<User>();
      wrapper.eq("name", "李四");
      //根据条件查询一条数据，如果结果超过一条会报错
      User user = this.userMapper.selectOne(wrapper);
      System.out.println(user);
   }
      
    @Test
    public void testSelectCount() {
      QueryWrapper<User> wrapper = new QueryWrapper<User>();
      wrapper.gt("age", 23); //年龄大于23岁
      //根据条件查询数据条数
      Integer count = this.userMapper.selectCount(wrapper);
      System.out.println("count = " + count);
   }
      
    @Test
    public void testSelectList() {
      QueryWrapper<User> wrapper = new QueryWrapper<User>();
      wrapper.gt("age", 23); //年龄大于23岁
       //根据条件查询数据
      List<User> users = this.userMapper.selectList(wrapper);
      for (User user : users) {
        System.out.println("user = " + user);
     }
   }
  }
  ~~~

* 分页

  > 1. 根据 entity 条件，查询全部记录（并翻页）
  >    `IPage<T> selectPage(IPage<T> page, @Param(Constants.WRAPPER) Wrapper<T> queryWrapper);`
  >    @param page     分页查询条件（可以为 RowBounds.DEFAULT）
  >    @param queryWrapper 实体对象封装操作类（可以为 null）

  ~~~java
  @Configuration
  @MapperScan("cn.itcast.mp.mapper") //设置mapper接口的扫描包
  public class MybatisPlusConfig {
    /**
    * 分页插件
    */
    @Bean
    public PaginationInterceptor paginationInterceptor() {
       return new PaginationInterceptor();
   }
  }
  ~~~

  ~~~java
  @RunWith(SpringRunner.class)
  @SpringBootTest
  public class UserMapperTest {
    @Autowired
    private UserMapper userMapper;
    @Test
    public void testSelectPage() {
      QueryWrapper<User> wrapper = new QueryWrapper<User>();
      wrapper.gt("age", 20); //年龄大于20岁
        
      Page<User> page = new Page<>(1,1);
      //根据条件查询数据
      IPage<User> iPage = this.userMapper.selectPage(page, wrapper);
      System.out.println("数据总条数：" + iPage.getTotal());
      System.out.println("总页数：" + iPage.getPages());
      List<User> users = iPage.getRecords();
      for (User user : users) {
        System.out.println("user = " + user);
     }
   }
  }
  ~~~

### 条件构造器 Wrapper

官网文档地址： https://mybatis.plus/guide/wrapper.html

> QueryWrapper(LambdaQueryWrapper) 和 UpdateWrapper(LambdaUpdateWrapper) 的父类 用于生成 sql的 where 条件, entity 属性也用于生成 sql 的 where 条件 注意: entity 生成的 where 条件与 使用各个 api 生成的 where 条件没有任何关联行为

| Wrapper对象方法     | 说明                                                         | 例题                                                         |
| ------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| eq                  | 等于 =                                                       | eq("password", "123456") ---> password = 123456              |
| ne                  | 不等于 <>                                                    |                                                              |
| gt                  | 大于 >                                                       |                                                              |
| ge                  | 大于等于 >=                                                  | ge("age", 20) ---> age>=20                                   |
| lt                  | 小于 <                                                       |                                                              |
| le                  | 小于等于 <=                                                  |                                                              |
| between             | BETWEEN 值1 AND 值2                                          |                                                              |
| notBetween          | NOT BETWEEN 值1 AND 值2                                      |                                                              |
| in                  | 字段 IN (value.get(0), value.get(1), ...)                    | in("name", "李四", "王五", "赵六") ---> name in ("李四", "王五", "赵六") |
| notIn               | 字段 NOT IN (v0, v1, ...)                                    |                                                              |
| like                | LIKE '% 值%'                                                 | like("name", " 王") ---> name like '% 王%'                   |
| notLike             | NOT LIKE '% 值%'                                             |                                                              |
| likeLeft            | LIKE '% 值'                                                  | likeLeft("name", " 王") ---> name like '% 王'                |
| likeRight           | LIKE ' 值%'                                                  |                                                              |
| orderBy\|orderByAsc | 排序： ORDER BY 字段, ...\|排序： ORDER BY 字段, ... ASC     | orderBy(true, true, "id", "name") ---> order by id ASC,name ASC \| orderByAsc("id", "name") ---> order by id ASC,name ASC |
| orderByDesc         | 排序： ORDER BY 字段, ... DESC                               | orderByDesc("id", "name") ---> order by id DESC,name DESC    |
| or                  | 拼接 OR<br />主动调用 or 表示紧接着下一个方法不是用 and 连接!(不调用 or 则默认为使用 and 连接) | or()                                                         |
| and                 | AND 嵌套                                                     | and(i -> i.eq("name", "李白").ne("status", "活着")) ---> and (name = '李白' and status<> '活着') |
| select              | 默认查询所有的字段，如果有需要也可以通过select方法进行指定字段。 | select("id", "name", "age") ---> SELECT id,name,age          |

### ActiveRecord

ActiveRecord也属于ORM（对象关系映射）层，由Rails最早提出，遵循标准的ORM模型：**表映射到记录，记录映射到对象，字段映射到对象属性**。配合遵循的命名和配置惯例，能够很大程度的快速实现模型的操作，而且简洁易懂。

* 开启AR

  > 只需要将实体对象继承 Model即可。

  ~~~java
  @Data
  @NoArgsConstructor
  @AllArgsConstructor
  public class User extends Model<User> {
    private Long id;
    private String userName;
    private String password;
    private String name;
    private Integer age;
    private String email;
  }
  ~~~

* CURD

  ~~~java
  @RunWith (SpringRunner.class)
  @SpringBootTest
  public class UserMapperTest {
    @Autowired
    private UserMapper userMapper;
      
    //根据ID查询
    @Test
    public void testAR() {
      User user = new User();
      user.setId(2L);
      User user2 = user.selectById();
      System.out.println(user2);
   }
    //根据条件查询  
    @Test
    public void testAR() {
      User user = new User();
      QueryWrapper<User> userQueryWrapper = new QueryWrapper<>();
      userQueryWrapper.le("age","20");
      List<User> users = user.selectList(userQueryWrapper);
      for (User user1 : users) {
        System.out.println(user1);
      }
    }
     
    //增加
    @Test
    public void testAR() {
      User user = new User();
      user.setName("刘备");
      user.setAge(30);
      user.setPassword("123456");
      user.setUserName("liubei");
      user.setEmail("liubei@itcast.cn");
      boolean insert = user.insert();
      System.out.println(insert);
    }
      
    //更新
    @Test
    public void testAR() {
      User user = new User();
      user.setId(8L);
      user.setAge(35);
      boolean update = user.updateById();
      System.out.println(update);
    }
      
    //删除
    @Test
    public void testAR() {
      User user = new User();
      user.setId(7L);
      boolean delete = user.deleteById();
      System.out.println(delete);
    }
  }
  ~~~

### 枚举

> 查询出来的数据是枚举中的desc，不是value

1. 定义枚举

   ~~~java
   public enum SexEnum implements IEnum<Integer> {
     MAN(1,"男"),
     WOMAN(2,"女");
     private int value;
     private String desc;
     SexEnum(int value, String desc) {
       this.value = value;
       this.desc = desc;
    }
     @Override
     public Integer getValue() {
       return this.value;
    }
     @Override
     public String toString() {
       return this.desc;
    }
   }
   ~~~

2. 配置

   ~~~properties
   # 枚举包扫描
   mybatis-plus.type-enums-package=cn.itcast.mp.enums
   ~~~

3. 实体

   ~~~java
   private SexEnum sex; 
   ~~~

4. 测试

   ~~~java
   @Test
   public void testInsert(){
     User user = new User();
     user.setName("貂蝉");
     user.setUserName("diaochan");
     user.setAge(20);
     user.setEmail("diaochan@itast.cn");
     user.setVersion(1);
     user.setSex(SexEnum.WOMAN);
     int result = this.userMapper.insert(user);
     System.out.println("result = " + result);
   }
   ~~~

## 基本配置

* configLocation

  > MyBatis 配置文件位置，如果您有单独的 MyBatis 配置，请将其路径配置到 configLocation 中。

  ~~~properties
  # Springboot
  mybatis-plus.config-location = classpath:mybatis-config.xml 
  ~~~

  ~~~xml
  <!-- Spring -->
  < bean id="sqlSessionFactory"
  class="com.baomidou.mybatisplus.extension.spring.MybatisSqlSessionFactoryBean">
    <property name="configLocation" value="classpath:mybatis-config.xml"/>
  </bean>
  ~~~

* mapperLocations

  > MyBatis Mapper 所对应的 XML 文件位置，如果您在 Mapper 中有自定义方法（XML 中有自定义实现），需要进行该配置，告诉 Mapper 所对应的 XML 文件位置。
  >
  > Maven 多模块项目的扫描路径需以 **classpath*:** 开头 （即加载多个 jar 包下的 XML 文件）

  ~~~properties
  # Springboot
  mybatis-plus.mapper-locations = classpath*:mybatis/*.xml 
  ~~~

  ~~~xml
  <!-- Spring -->
  < bean id="sqlSessionFactory"
  class="com.baomidou.mybatisplus.extension.spring.MybatisSqlSessionFactoryBean">
    <property name="mapperLocations" value="classpath*:mybatis/*.xml"/>
  </bean>
  ~~~

* typeAliasesPackage

  > MyBaits 别名包扫描路径，通过该属性可以给包中的类注册别名，注册后在 Mapper 对应的 XML 文件中可以直接使用类名，而不用使用全限定的类名（即 XML 中调用的时候不用包含包名）。

  ~~~properties
  # Springboot
  mybatis-plus.type-aliases-package = cn.itcast.mp.pojo 
  ~~~

  ~~~xml
  <!-- Spring -->
  < bean id="sqlSessionFactory"
  class="com.baomidou.mybatisplus.extension.spring.MybatisSqlSessionFactoryBean">
    <property name="typeAliasesPackage"
  value="com.baomidou.mybatisplus.samples.quickstart.entity"/>
  </bean>
  ~~~

* mapUnderscoreToCamelCase

  > 【默认true】是否开启自动驼峰命名规则（camel case）映射，即从经典数据库列名 A_COLUMN（下划线命名） 到经典 Java 属性名 aColumn（驼峰命名） 的类似映射

  ~~~properties
  # 关闭自动驼峰映射，该参数不能和mybatis-plus.config-location同时存在
  mybatis-plus.configuration.map-underscore-to-camel-case=false
  ~~~

* cacheEnabled

  > 【默认true】全局地开启或关闭配置文件中的所有映射器已经配置的任何缓存

  ~~~properties
  mybatis-plus.configuration.cache-enabled =false 
  ~~~

### DB策略配置（全局配置属性）

| 键          | 说明                                                         | 默认      |
| ----------- | ------------------------------------------------------------ | --------- |
| idType      | 全局默认主键类型，设置后，即可省略实体对象中的@TableId(type = IdType.AUTO)配置。 | ID_WORKER |
| tablePrefix | 表名前缀，全局配置后可省略 @TableName()配置。                | null      |

~~~properties
# Springboot
mybatis-plus.global-config.db-config.id-type =auto
mybatis-plus.global-config.db-config.table-prefix =tb_ 
~~~

~~~xml
<!-- Spring -->
  <bean id="sqlSessionFactory"
class="com.baomidou.mybatisplus.extension.spring.MybatisSqlSessionFactoryBean">
    <property name="dataSource" ref="dataSource"/>
    <property name="globalConfig">
      <bean class="com.baomidou.mybatisplus.core.config.GlobalConfig">
        <property name="dbConfig">
            
          <bean
class="com.baomidou.mybatisplus.core.config.GlobalConfig$DbConfig">
            <property name="idType" value="AUTO"/>
          </bean>
            
          <bean
class="com.baomidou.mybatisplus.core.config.GlobalConfig$DbConfig">
            <property name="idType" value="AUTO"/>
            <property name="tablePrefix" value="tb_"/>
          </bean>
        </property>
      </bean>
    </property>
  </bean>
~~~



## 插件

> MyBatis 允许你在已映射语句执行过程中的某一点进行拦截调用。默认情况下，MyBatis 允许使用插件来拦截的方法，可以拦截Executor接口的部分方法，比如update，query，commit，rollback等方法，还有其他接口的一些方法等。
> 调用包括：
>
> 1. 拦截执行器的方法
> 2. 拦截参数的处理
> 3. 拦截结果集的处理
> 4. 拦截Sql语法构建的处理

1. 创建拦截器

   ~~~java
   @Intercepts({@Signature(
       type= Executor.class,
       method = "update",
       args = {MappedStatement.class,Object.class})})
   public class MyInterceptor implements Interceptor {
     @Override
     public Object intercept(Invocation invocation) throws Throwable {
       //拦截方法，具体业务逻辑编写的位置
       return invocation.proceed();
       }
     @Override
     public Object plugin(Object target) {
       //创建target对象的代理对象,目的是将当前拦截器加入到该对象中
       return Plugin.wrap(target, this);
    }
     @Override
     public void setProperties(Properties properties) {
       //属性设置
    }
   }
   ~~~

2. 注入Spring容器

   ~~~java
    /**
     * 自定义拦截器
     */
     @Bean
     public MyInterceptor myInterceptor(){
       return new MyInterceptor();
    }
   ~~~

   或者xml配置，mybatis-config.xml

   ~~~xml
   <?xml version="1.0" encoding="UTF-8" ?>
   <!DOCTYPE configuration
       PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
       "http://mybatis.org/dtd/mybatis-3-config.dtd">
   <configuration>
     <plugins>
       <plugin interceptor="cn.itcast.mp.plugins.MyInterceptor"></plugin>
     </plugins>
   </configuration>
   ~~~

* 执行分析插件
  可用作阻断全表更新、删除的操作，注意：该插件仅适用于开发环境，不适用于生产环境。

  1. 注入spring容器

     ~~~java
     @Bean
     public SqlExplainInterceptor sqlExplainInterceptor(){
       SqlExplainInterceptor sqlExplainInterceptor = new SqlExplainInterceptor();
       List<ISqlParser> sqlParserList = new ArrayList<>();
       // 攻击 SQL 阻断解析器、加入解析链
       sqlParserList.add(new BlockAttackSqlParser());
       sqlExplainInterceptor.setSqlParserList(sqlParserList);
       return sqlExplainInterceptor;
     }
     ~~~

  2. 测试:

     ~~~java
     @Test
     public void testUpdate(){
       User user = new User();
       user.setAge(20);
       int result = this.userMapper.update(user, null);
       System.out.println("result = " + result);
     }
     ~~~

* 性能分析插件
  用于输出每条 SQL 语句及其执行时间，可以设置最大执行时间，超过时间会抛出异常。

  1. 配置

     ~~~xml
     <?xml version="1.0" encoding="UTF-8" ?>
     <!DOCTYPE configuration
         PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
         "http://mybatis.org/dtd/mybatis-3-config.dtd">
     <configuration>
       <plugins>
         <!-- SQL 执行性能分析，开发环境使用，线上不推荐。 maxTime 指的是 sql 最大执行时长 -->
         <plugin
     interceptor="com.baomidou.mybatisplus.extension.plugins.PerformanceInterceptor">
           <property name="maxTime" value="100" />
           <!--SQL是否格式化 默认false-->
           <property name="format" value="true" />
         </plugin>
       </plugins>
     </configuration>
     ~~~

* 乐观锁插件

  > 乐观锁实现方式：
  >
  > 1. 取出记录时，获取当前 version
  > 2. 更新时，带上这个 version
  > 3. 执行更新时， set version = newVersion where version = oldVersion
  > 4. 如果 version不对，就更新失败
  >
  > 特别说明：
  >
  > 1. 支持的数据类型只有 :int,Integer,long,Long,Date,Timestamp,LocalDateTime
  > 2. 整数类型下 newVersion = oldVersion + 1
  > 3. newVersion 会回写到 entity 中
  > 4. 仅支持 updateById(id) 与 update(entity, wrapper) 方法
  > 5. 在 update(entity, wrapper) 方法下, wrapper 不能复用

  1. 注入spring容器，springboot

     ~~~java
     @Bean
     public OptimisticLockerInterceptor optimisticLockerInterceptor() {
       return new OptimisticLockerInterceptor();
     }
     ~~~

     spring xml:

     ~~~xml
     < bean class="com.baomidou.mybatisplus.extension.plugins.OptimisticLockerInterceptor"/> 
     ~~~

  2. 注解实体字段
     1）为表添加version字段，并且设置初始值为1
     2）为User实体对象添加version字段，并且添加@Version注解

     ~~~java
     @Version
     private Integer version;
     ~~~

  3. 测试

     ~~~java
     @Test
     public void testUpdate(){
       User user = new User();
       user.setAge(30);
       user.setId(2L);
       user.setVersion(1); //获取到version为1
       int result = this.userMapper.updateById(user);
       System.out.println("result = " + result);
     }
     ~~~

## Sql注入器

* 原理
  MP在启动后会将BaseMapper中的一系列的方法注册到meppedStatements中

  1. ISqlInjector负责SQL的注入工作，它是一个接口，AbstractSqlInjector是它的实现类
  2. 在 AbstractSqlInjector中，主要是由inspectInject()方法进行注入的
  3. 在实现方法中， methodList.forEach(m -> m.inject(builderAssistant, mapperClass, modelClass,tableInfo)); 是关键，循环遍历方法，进行注入。
  4. 最终调用抽象方法injectMappedStatement进行真正的注入

* 自定义sql注入器

  1. 扩展

     > 统一扩展：MyBaseMapper\<T> extends BaseMapper\<T>
     > 				   其他的Mapper都可以继承该MyBaseMapper，这样实现了统一的扩展。
     > 单一扩展：UserMapper extends MyBaseMapper\<User>

     ~~~java
     public interface MyBaseMapper<T> extends BaseMapper<T> {
       List<T>  findAll();
     }
     ~~~

     ~~~Java
     public interface UserMapper extends MyBaseMapper<User> {
       User findById(Long id);
     }
     ~~~

  2. 编写sql方法

     ~~~java
     public class FindAll extends AbstractMethod {
       @Override
       public MappedStatement injectMappedStatement(Class<?> mapperClass, Class<?> modelClass, TableInfo tableInfo) {
         String sqlMethod = "findAll"; // 方法名
         String sql = "select * from " + tableInfo.getTableName(); // sql
           
         SqlSource sqlSource = languageDriver.createSqlSource(configuration, sql,modelClass);
         return this.addSelectMappedStatement(mapperClass, sqlMethod, sqlSource,modelClass, tableInfo);
      }
     }
     ~~~

  3. 编写MySqlInjector

     > 如果直接继承AbstractSqlInjector的话，原有的BaseMapper中的方法将失效，所以我们选择继承DefaultSqlInjector进行扩展。

     ~~~java
     public class MySqlInjector extends DefaultSqlInjector {
       @Override
       public List<AbstractMethod> getMethodList() {
         // 获取baseMapper中的方法
         List<AbstractMethod> methodList = super.getMethodList();
         methodList.add(new FindAll());
        
         // 再扩充自定义的方法
         list.add(new FindAll());
         return methodList;
      }
     }
     ~~~

  4. 注入到spring容器

     ~~~java
     /**
     * 自定义SQL注入器
     */
     @Bean
     public MySqlInjector mySqlInjector(){
       return new MySqlInjector();
     }
     ~~~

  5. 测试

     ~~~java
     @Test
     public void testFindAll(){
       List<User> users = this.userMapper.findAll();
       for (User user : users) {
         System.out.println(user);
      }
     }
     ~~~



## 代码生成器

> AutoGenerator 是 MyBatis-Plus 的代码生成器，通过 AutoGenerator 可以快速生成 Entity、Mapper、MapperXML、Service、Controller 等各个模块的代码，极大的提升了开发效率。

* 依赖包

  ~~~xml
      <dependency>
        <groupId>com.baomidou</groupId>
        <artifactId>mybatis-plus-generator</artifactId>
        <version>3.1.1</version>
      </dependency>
      <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-freemarker</artifactId>
      </dependency>
  ~~~

* 生成器代码（可运行）

  ~~~java
  /**
  * <p>
  * mysql 代码生成器演示例子
  * </p>
  */
  public class MysqlGenerator {
    /**
    * <p>
    * 读取控制台内容
    * </p>
    */
    public static String scanner(String tip) {
      Scanner scanner = new Scanner(System.in);
      StringBuilder help = new StringBuilder();
      help.append("请输入" + tip + "：");
      System.out.println(help.toString());
      if (scanner.hasNext()) {
        String ipt = scanner.next();
          if (StringUtils.isNotEmpty(ipt)) {
          return ipt;
       }
     }
      throw new MybatisPlusException("请输入正确的" + tip + "！");
   }
    /**
    * RUN THIS
    */
    public static void main(String[] args) {
      // 代码生成器
      AutoGenerator mpg = new AutoGenerator();
      // 全局配置
      GlobalConfig gc = new GlobalConfig();
      String projectPath = System.getProperty("user.dir");
      gc.setOutputDir(projectPath + "/src/main/java");
      gc.setAuthor("itcast");
      gc.setOpen(false);
      mpg.setGlobalConfig(gc);
      // 数据源配置
      DataSourceConfig dsc = new DataSourceConfig();
      dsc.setUrl("jdbc:mysql://127.0.0.1:3306/mp?
  useUnicode=true&useSSL=false&characterEncoding=utf8");
      // dsc.setSchemaName("public");
      dsc.setDriverName("com.mysql.jdbc.Driver");
      dsc.setUsername("root");
      dsc.setPassword("root");
      mpg.setDataSource(dsc);
      // 包配置
      PackageConfig pc = new PackageConfig();
      pc.setModuleName(scanner("模块名"));
      pc.setParent("cn.itcast.mp.generator");
      mpg.setPackageInfo(pc);
      // 自定义配置
      InjectionConfig cfg = new InjectionConfig() {
        @Override
        public void initMap() {
          // to do nothing
       }
     };
      List<FileOutConfig> focList = new ArrayList<>();
      focList.add(new FileOutConfig("/templates/mapper.xml.ftl") {
        @Override
        public String outputFile(TableInfo tableInfo) {
          // 自定义输入文件名称
          return projectPath + "/itcast-mp-
  generator/src/main/resources/mapper/" + pc.getModuleName()
             + "/" + tableInfo.getEntityName() + "Mapper" +
  StringPool.DOT_XML;
       }
     });
      cfg.setFileOutConfigList(focList);
      mpg.setCfg(cfg);
      mpg.setTemplate(new TemplateConfig().setXml(null));
      // 策略配置
      StrategyConfig strategy = new StrategyConfig();
      strategy.setNaming(NamingStrategy.underline_to_camel);
      strategy.setColumnNaming(NamingStrategy.underline_to_camel);
  //   
  strategy.setSuperEntityClass("com.baomidou.mybatisplus.samples.generator.common.BaseE
  ntity");
      strategy.setEntityLombokModel(true);
  //   
  strategy.setSuperControllerClass("com.baomidou.mybatisplus.samples.generator.common.B
  aseController");
      strategy.setInclude(scanner("表名"));
      strategy.setSuperEntityColumns("id");
      strategy.setControllerMappingHyphenStyle(true);
      strategy.setTablePrefix(pc.getModuleName() + "_");
      mpg.setStrategy(strategy);
      // 选择 freemarker 引擎需要指定如下加，注意 pom 依赖必须有！
      mpg.setTemplateEngine(new FreemarkerTemplateEngine());
      mpg.execute();
   }
  }
  ~~~

## MybatisX 快速开发插件

* 安装方法：打开 IDEA，进入 File -> Settings -> Plugins -> Browse Repositories，输入mybatisx 搜索并安装。
* 功能：
  1. Java 与 XML 调回跳转
  2. Mapper 方法自动生成 XML
