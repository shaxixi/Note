[toc]

# SpringMVC

## 概念

* MVC是一种软件架构的思想，将软件按照模型（Model）、视图（View）、控制器（Controller）来划分。
  \> Model：指工程中的JavaBean，作用是处理数据
  \> View：指工程中的html或jsp等页面，作用是与用户进行交互和展示数据
  \> Controller：指工程中的servlet，作用是接收请求和响应浏览器
* MVC的工作流程:
  用户通过 **view 发送请求到服务器**，在服务器中**请求被 controller 接收** ===>
  controller 调用相应的 **model 层处理请求**，处理完毕将**结果返回到 controller** ===>
  controller 再根据请求处理的结果**找到相应的 view 视图**，**渲染数据**后最终响应给浏览器
* SpringMVC是Spring为表述层（三层结构分为表述层 [表示前台页面和后台servlet]、业务逻辑层、数据访问层）开发提供的一套完备解决方案，是spring的一个子项目。
* 表述层架构历程：Strust ==> WebWork ==> Strust2 ==> …… ==> SpringMVC 
  SpringMVC作为JavaEE 项目表述层的首选方案
* SpringMVC特点
  \> Spring 家族原生产品，与IOC容器等基础元素无缝对接
  \> 基于原生Servlet，通过功能强大的前端控制器DispatcherServlet,对请求和响应进行统一处理
  \> 内部组件化程度高，可插拔式组件即插即用，想要什么功能配置响应组件即可
  \> 代码清晰简洁、性能显著、解决问题全方位覆盖

## 创建maven工程

* 构建工具：maven
  Spring、tomcat、idea

1. 创建maven模块/工程

2. pom.xml配置文件中
   ① 添加打包方式war

   ~~~xml
   <!-- 格式 -->
   <packaging>打包方式</packaging>
   
   <!-- 常用方式 -->
   <packaging>war</packaging>
   ~~~

   ② 引入依赖

   ~~~xml
   <!-- 格式 -->
   <dependencies>
       <dependency>
           <groupId>路径</groupId>
           <artifactId>jar包名</artifactId>
           <version>版本号</version>
       </dependency>
   </dependencies>
   
   <!-- 常用依赖 -->
   <dependencies>
       <!-- SpringMVC -->
       <dependency>
           <groupId>org.springframework</groupId>
           <artifactId>spring-webmvc</artifactId>
           <version>5.3.1</version>
       </dependency>
   
       <!-- 日志 -->
       <dependency>
           <groupId>ch.qos.logback</groupId>
           <artifactId>logback-classic</artifactId>
           <version>1.2.3</version>
       </dependency>
   
       <!-- ServletAPI -->
       <dependency>
           <groupId>javax.servlet</groupId>
           <artifactId>javax.servlet-api</artifactId>
           <version>3.1.0</version>
           <scope>provided</scope>
       </dependency>
   
       <!-- Spring5和Thymeleaf整合包 -->
       <dependency>
           <groupId>org.thymeleaf</groupId>
           <artifactId>thymeleaf-spring5</artifactId>
           <version>3.0.12.RELEASE</version>
       </dependency>
   </dependencies>
   ~~~

   注：Maven有传递性，我们不必将所有需要的包全部配置依赖，而是配置最顶端的依赖，其他靠传递性导入
   ![img001](E:\软工\typora笔记\JavaEE框架\SpringMVC.assets\img001.png)

3. 添加 web模块 并 添加 WEB-INF\web.xml

4. 配置web.xml
   ① 注册SpringMVC的前端控制器 DispatcherServlet
        对浏览器所发送的请求统一进行处理

   ~~~xml
   <!-- 默认配置方式 
           在此配置下，springMVC的配置文件具有默认的位置和名称
           默认的位置：WEB-INF
           默认的名称：<servlet-name>-servlet.xml
   -->
       <servlet>
           <servlet-name>springMVC</servlet-name>
           <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
           
           <!-- 扩展配置方式 ----------------------	
                   若要为springMVC的配置文件设置自定义的位置和名称
                   需要在servlet标签中添加init-param标签设置SpringMVC配置文件的位置和名称
                   load-on-startup：设置SpringMVC前端控制器DispatcherServlet的初始化时间
           -->
           <init-param>
               <!-- contextConfigLocation为固定值 -->
               <param-name>contextConfigLocation</param-name>
               <!-- 使用classpath:表示从类路径查找配置文件，例如maven工程中的src/main/resources -->
               <param-value>classpath:springMVC.xml</param-value>
           </init-param>
           <!-- 作为框架的核心组件，在启动过程中有大量的初始化操作要做，而这些操作放在第一次请求时才执行会严重影响访问速度，因此需要通过此标签将启动控制DispatcherServlet的初始化时间提前到服务器启* 动时-->
           <load-on-startup>1</load-on-startup>
           <!-- ---------------------- -->
       </servlet>
       <servlet-mapping>
           <servlet-name>springMVC</servlet-name>
           <!--设置springMVC的核心控制器所能处理的请求的请求路径
           /  所匹配的请求可以是 /login 或.html 或.js 或.css方式的请求路径，但是 / 不能匹配.jsp请求路径的请求。因此就可以避免在访问jsp页面时，该请求被DispatcherServlet处理，从而找不到相应的页面
   		/* 则能够匹配所有请求，例如在使用过滤器时，若需要对所有请求进行过滤，就需要使用 /*的写法-->
           <url-pattern>/</url-pattern>
       </servlet-mapping>
   ~~~

5. 创建请求控制器

   * 请求控制器：前端控制器对浏览器发送的请求进行了统一处理，但具体的请求有不同的处理过程，因此需要创建处理具体请求的类
   * 控制器方法：请求控制器中每一个请求的方法
   * SpringMVC的控制器由普通的Java类担任，因此需要通过 @Controller 注解将其标识为一个控制层组件，交给Spring的IOC容器管理，此时SpringMVC才能够识别控制器的存在

   ~~~java
   @Controller 
   public class HelloController {	//① 请求控制器
       @RequestMapping("/") 
       public String index(){	//② 控制器方法
           return "index";
       }
   
       @RequestMapping("/target")
       public String toTarget(){	//② 控制器方法
           return "target";
       }
   }
   ~~~

6. 创建 springMVC的配置文件
   根据 web.xml 文件中的 springMVC配置文件 路径，来创建 springMVC 配置文件

7. 配置 springMVC 的配置文件
   ① 自动扫描包 ② 配置thymeleaf视图解析器 ③ 解决 post请求 乱码问题  ***

   ~~~xml
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns="http://www.springframework.org/schema/beans"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xmlns:context="http://www.springframework.org/schema/context"
          xmlns:mvc="http://www.springframework.org/schema/mvc"
          xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/context https://www.springframework.org/schema/context/spring-context.xsd http://www.springframework.org/schema/mvc https://www.springframework.org/schema/mvc/spring-mvc.xsd">
   
   <!-- 自动扫描包 -->
   <context:component-scan base-package="com.atguigu.mvc.controller"/>
   
   <!-- 配置Thymeleaf视图解析器 -->
   <bean id="viewResolver" class="org.thymeleaf.spring5.view.ThymeleafViewResolver">
       <property name="order" value="1"/>
       <property name="characterEncoding" value="UTF-8"/>
       <property name="templateEngine">
           <bean class="org.thymeleaf.spring5.SpringTemplateEngine">
               <property name="templateResolver">
                   <bean class="org.thymeleaf.spring5.templateresolver.SpringResourceTemplateResolver">
       
                       <!-- 视图前缀 -->
                       <property name="prefix" value="/WEB-INF/templates/"/>   
                       <!-- 视图后缀 -->
                       <property name="suffix" value=".html"/>
                       
                       <property name="templateMode" value="HTML5"/>
                       <property name="characterEncoding" value="UTF-8" />
                   </bean>
               </property>
           </bean>
       </property>
   </bean>
   
   <!-- 
      处理静态资源，例如html、js、css、jpg
     若只设置该标签，则只能访问静态资源，其他请求则无法访问
     此时必须设置<mvc:annotation-driven/>解决问题
    -->
   <mvc:default-servlet-handler/>
   
   <!-- 开启mvc注解驱动 -->
   <mvc:annotation-driven>
       <mvc:message-converters>
           <!-- 处理响应中文内容乱码 -->
           <bean class="org.springframework.http.converter.StringHttpMessageConverter">
               <property name="defaultCharset" value="UTF-8" />
               <property name="supportedMediaTypes">
                   <list>
                       <value>text/html</value>
                       <value>application/json</value>
                   </list>
               </property>
           </bean>
       </mvc:message-converters>
   </mvc:annotation-driven>
   ~~~

8. 测试

   * 在请求控制器中创建处理请求的方法

     ~~~java
     @RequestMapping("/")
     public String index() {
         //设置视图名称
         return "index";
     }
     
     @RequestMapping("/hello")
     public String HelloWorld() {
         return "target";
     }
     ~~~

     注：@RequestMapping注解：处理请求和控制器方法之间的映射关系
     		@RequestMapping注解的value属性可以通过请求地址匹配请求，
     		/ 表示的当前工程的上下文路径：localhost:8080/springMVC/

   * 在页面中设置超链接跳转到指定页面

     ~~~html
     <!DOCTYPE html>
     <html lang="en" xmlns:th="http://www.thymeleaf.org">
     <head>
         <meta charset="UTF-8">
         <title>首页</title>
     </head>
     <body>
         <h1>首页</h1>
         <a th:href="@{/hello}">HelloWorld</a><br/>
     </body>
     </html>
     ~~~

     注：xmlns:th="http://www.thymeleaf.org"： 用于thymeleaf视图解析
     		th:href="@{/hello}"： 应用thymeleaf视图解析

总结：

​	**浏览器**发送请求，若请求地址符合前端控制器的 **url-pattern**，该请求就会被**前端控制器**DispatcherServlet处理。前端控制器会读取**SpringMVC的核心配置文件**，通过**扫描组件**找到**控制器**，将请求地址和控制器中**@RequestMapping注解**的**values属性**值进行匹配，若匹配成功，该注解所标识的控制器方法就是处理请求的方法。处理请求的方法需要**返回一个字符串类型的视图名称**，该视图名称会被**视图解析器解析**，加上前缀和后缀组成视图的路径，通过Thymeleaf对视图进行渲染，最终转发到视图所对应**页面**。



## @RequestMapping注解

* 作用：将请求和处理请求的控制器方法关联起来，建立映射关系
  SpringMVC 接收到指定的请求，就会找到映射关系中对应的控制器方法来处理这个请求

* 注解位置：
  标识一个类：设置映射请求的请求路径的初始信息
  标识一个方法：设置映射请求的请求路径的具体信息

  ~~~java
  @Controller
  @RequestMapping("/test")
  public class RequestMappingController {
      @RequestMapping("/testRequestMapping")
      public String testRequestMapping(){
          return "success";
      }
  }
  ~~~

  ~~~html
  <a th:href="@{/test/testRequestMapping}">测试RequestMapping注解的位置</a><br>
  ~~~

### @RequestMapping注解属性

* SpringMVC支持ant风格的路径

  | 符号 | 说明                                              |
  | ---- | ------------------------------------------------- |
  | ？   | 表示任意单个字符                                  |
  | *    | 表示任意0个或多个字符                             |
  | **   | 表示任意的一层或多层目录，只能使用 /**/xxx 的方式 |

  ~~~java
  @RequestMapping("/a?a/testAnt")
  public String testAnt(){return "success";}
  
  @RequestMapping("/a*a/testAnt")
  public String testAnt(){return "success";}
  
  @RequestMapping("/**/testAnt")
  public String testAnt(){return "success";}
  ~~~

  ~~~html
  <a th:href="@{/a1a/testAnt}">测试@RequestMapping可以匹配ant风格的路径-->使用?</a><br>
  <a th:href="@{/a1a/testAnt}">测试@RequestMapping可以匹配ant风格的路径-->使用*</a><br>
  <a th:href="@{/a1a/testAnt}">测试@RequestMapping可以匹配ant风格的路径-->使用**</a><br>
  ~~~

#### value 属性

* value属性通过请求的**请求地址**匹配请求映射
* value属性是一个字符串类型**数组**，表示该请求映射能够**匹配多个请求地址**所对应的请求
* value属性必须设置，**至少一个**通过请求地址匹配请求映射
* 只有value属性且该属性只有一个值时，可忽略写value

~~~java
@RequestMapping(
        value = {"/testRequestMapping", "/test"}
 		//或者 "/testRequestMapping"
)
public String testRequestMapping(){
    return "success";
}
~~~

~~~html
<a th:href="@{/testRequestMapping}">测试@RequestMapping的value属性-->/testRequestMapping</a><br>
<a th:href="@{/test}">测试@RequestMapping的value属性-->/test</a><br>
~~~

#### method属性

* method属性通过请求的**请求方式**（get或post）匹配请求映射
* method属性是一个RequestMethod类型的**数组**，表示该请求映射能够**匹配多种请求方式**的请求
* 请求的地址满足value属性，方式不满足method属性，则浏览器报错：
  405：Request method 'POST' not supported

~~~java
@RequestMapping(
        value = {"/testRequestMapping", "/test"},
        method = {RequestMethod.GET, RequestMethod.POST}
)
public String testRequestMapping(){
    return "success";
}
~~~

~~~html
<a th:href="@{/test}">测试@RequestMapping的value属性-->/test</a><br>
<form th:action="@{/test}" method="post">
    <input type="submit">
</form>
~~~

* @RequestMapping派生注解
  \> @GetMapping					处理get请求的映射
  \> @PostMapping				  处理post请求的映射
  \> @PutMapping					处理put请求的映射
  \> @DeleteMapping			  处理delete请求的映射

  ~~~java
  @GetMapping("/testGetMapping")
      public String testGetMapping(){
          return "success";
      }
  ~~~

* 常用请求方式：get post put delete
  \> 目前浏览器只支持 get 和 post，若form表单的method属性设置了其他请求方式(put、delete)，则按照默认的请求方式get处理
  \> 如要发送put和delete请求，需要通过spring提供的过滤器 HiddenHttpMethodFilter

#### params属性（了解）

* params属性通过请求的**请求参数**匹配请求映射

* params属性是一个字符串类型的数组，可以通过四种表达式设置请求参数和请求映射的匹配关系，且**必须满足所有表达式**

  | 参数         | 说明                                                         |
  | ------------ | ------------------------------------------------------------ |
  | param        | 请求映射所匹配的请求**必须携带**param请求参数                |
  | !param       | 请求映射所匹配的请求必须不能携带param请求参数                |
  | param=value  | 请求映射所匹配的请求**必须携带**param请求参数且**param=value** |
  | param!=value | 请求映射所匹配的请求**必须携带**param请求参数但是**param!=value** |

~~~java
@RequestMapping(
        value = {"/testRequestMapping", "/test"},
        method = {RequestMethod.GET, RequestMethod.POST},
        params = {"username","password!=123456"}
)
public String testRequestMapping(){
    return "success";
}
~~~

~~~html
<a th:href="@{/test(username='admin',password=123456)}">测试@RequestMapping的params属性-->/test</a><br>
<!-- @{/test?username=admin&password=123456} 会报错，但不影响 -->
~~~

* 请求的地址满足value属性，方式满足method属性，参数不满足params属性，则浏览器报错：
  400：Parameter conditions "username, password!=123456" not met for actual request parameters: username={admin}, password={123456}

#### headers属性（了解）

* header属性通过请求的**请求头信息**匹配请求映射

* headers属性是一个字符串类型的数组，可以通过四种表达式设置请求头信息和请求映射的匹配关系

  | 参数          | 说明                                                         |
  | ------------- | ------------------------------------------------------------ |
  | header        | 请求映射所匹配的请求**必须携带**header请求参数               |
  | !header       | 请求映射所匹配的请求必须不能携带header请求参数               |
  | header=value  | 请求映射所匹配的请求**必须携带**header请求参数且**header=value** |
  | header!=value | 请求映射所匹配的请求**必须携带**header请求参数但是**header!=value** |

* 请求的地址满足value属性，方式满足method属性，参数不满足header属性，则浏览器报错：
  404：资源未找到

~~~java
@RequestMapping(
    value = "/testParamsAndHeaders",
    params = {"username","password!=123456"},
    headers = {"Host=localhost:8080"}
)
public String testParamsAndHeaders(){
    return "success";
}
~~~

~~~html
<a th:href="@{/testGetMapping}">测试GetMapping注解-->/testGetMapping</a><br>
~~~

## SpringMVC获取请求参数

* 方式一：ServletAPI获取
  将HttpServletRequest作为控制器方法的形参，此时该形参表示封装了当前请求的请求报文的对象。即原始servlet方法

  ~~~java
  @RequestMapping("/testParam")
  public String testParam(HttpServletRequest request){
      String username = request.getParameter("username");
      String password = request.getParameter("password");
      System.out.println("username:"+username+",password:"+password);
      return "success";
  }
  ~~~

  

* 方式二：控制器方法的形参获取

  **获取基本数据类型**
  在控制器方法的形参位置，设置和请求参数同名的形参，当浏览器发送请求，匹配到请求映射时，在DispatcherServlet中就会将请求参数赋值给相应的形参

  ~~~java
  @RequestMapping("/testParam")
  public String testParam(String username, String password){
      System.out.println("username:"+username+",password:"+password);
      return "success";
  }
  //最终结果-->username:admin,password:123456
  ~~~

  ~~~html
  <a th:href="@{/testParam(username='admin',password=123456)}">测试获取请求参数-->/testParam</a><br>
  ~~~

  注：
  \> 若请求所传输的请求参数中有多个同名的请求参数，则控制器方法的形参中设置字符串数组或者字符串类型的形参接收
  		字符串数组类型的形参：参数的数组中包含了每一个数据
  		字符串类型的形参：参数的值为每个数据中间使用逗号拼接的结果 

  **获取POJO对象**
  在控制器方法的形参位置，设置一个实体类类型的形参，当浏览器发送请求，请求参数的参数名和实体类中的属性名一致，那么请求参数就会为此属性赋值

  ~~~java
  @RequestMapping("/testpojo")
  public String testPOJO(User user){
      System.out.println(user);
      return "success";
  }
  //最终结果-->User{id=null, username='张三', password='123', age=23, sex='男', email='123@qq.com'}
  ~~~

  ~~~html
  <form th:action="@{/testpojo}" method="post">
      用户名：<input type="text" name="username"><br>
      密码：<input type="password" name="password"><br>
      性别：<input type="radio" name="sex" value="男">男<input type="radio" name="sex" value="女">女<br>
      年龄：<input type="text" name="age"><br>
      邮箱：<input type="text" name="email"><br>
      <input type="submit">
  </form>
  ~~~

### 形参注解

| 属性         | 说明                                                         |
| ------------ | ------------------------------------------------------------ |
| value        | 指定为形参赋值的请求参数的参数名                             |
| required     | 设置是否必须传输此请求参数，默认值true<br />1) 若设置为true时，则当前请求必须传输value所指定的请求参数，若没有传输该请求参数，且没有设置defaultValue属性，则页面报错400：Required String parameter 'xxx' is not present；<br />2) 若设置为false，则当前请求不是必须传输value所指定的请求参数，若没有传输，则注解所标识的形参的值为null |
| defaultValue | 不管required属性值为true或false，当value所指定的请求参数没有传输或传输的值为“”时，则使用默认值为形参赋值 |

* @RequestParam
  将**请求参数**和控制器方法的形参创建映射关系
* @RequestHeader
  将**请求头信息**和控制器方法的形参创建映射关系
* @CookieValue
  将**cookie数据**和控制器方法的形参创建映射关系

~~~html
<form th:action="@{/testParam}" method="get">
    用户名：<input type="text" name="user_name"><br>
    密码：<input type="password" name="password"><br>
    爱好：<input type="checkbox" name="hobby" value="a">a
    <input type="checkbox" name="hobby" value="b">b
    <input type="checkbox" name="hobby" value="c">c<br>
    <input type="submit" value="测试使用控制器的形参获取请求参数">
</form>
~~~

~~~java
    @RequestMapping("/testParam")
    public String testParam(
            @RequestParam(value = "user_name", required = false, defaultValue = "hehe") String username,
            String password,
            String[] hobby,
            @RequestHeader(value = "sayHaha", required = true, defaultValue = "haha") String host,
            @CookieValue("JSESSIONID") String JSESSIONID){
 System.out.println("username:"+username+",password:"+password+",hobby:"+ Arrays.toString(hobby));
        System.out.println("host:"+host);
        System.out.println("JSESSIONID:"+JSESSIONID);
        return "success";
    }
~~~



### 获取请求参数的乱码解决

* 解决方法：SpringMVC提供的编码过滤器CharacterEncodingFilter,并在web.xml中进行注册

  ~~~xml
  <!--配置springMVC的编码过滤器-->
  <filter>
      <filter-name>CharacterEncodingFilter</filter-name>
      <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
      <!--设置编码格式的初始值-->
      <init-param>
          <param-name>encoding</param-name>
          <param-value>UTF-8</param-value>
      </init-param>
      <!--设置强制性响应编码初始值，即响应的编码格式-->
      <init-param>
          <param-name>forceResponseEncoding</param-name>
          <param-value>true</param-value>
      </init-param>
  </filter>
  <filter-mapping>
      <filter-name>CharacterEncodingFilter</filter-name>
      <url-pattern>/*</url-pattern>
  </filter-mapping>
  ~~~

* SpringMVC中处理编码的过滤器一定要配置到其他过滤器之前，否则无效

## 域对象共享数据

### 向request域共享数据

~~~html
<p th:text="${testScope}"></p>
~~~

* 方式一：使用Servlet API

  ~~~java
  @RequestMapping("/testServletAPI")
  public String testServletAPI(HttpServletRequest request){
      request.setAttribute("testScope", "hello,servletAPI");
      return "success";
  }
  ~~~

* 方式二：使用ModelAndView

  ~~~java
  @RequestMapping("/testModelAndView")
  public ModelAndView testModelAndView(){
      ModelAndView mav = new ModelAndView();
      //向请求域共享数据
      mav.addObject("testScope", "hello,ModelAndView");
      //设置视图，实现页面跳转
      mav.setViewName("success");
      return mav;
  }

* 方式三：使用Model

  ~~~java
  @RequestMapping("/testModel")
  public String testModel(Model model){
      model.addAttribute("testScope", "hello,Model");
      return "success";
  }

* 方式四：使用map

  ~~~java
  @RequestMapping("/testMap")
  public String testMap(Map<String, Object> map){
      map.put("testScope", "hello,Map");
      return "success";
  }

* 方式五：使用ModelMap

  ~~~java
  @RequestMapping("/testModelMap")
  public String testModelMap(ModelMap modelMap){
      modelMap.addAttribute("testScope", "hello,ModelMap");
      return "success";
  }

* Model、ModelMap、Map类型的参数其实本质上都是 BindingAwareModelMap类型

  ~~~java
  public interface Model{}
  public class ModelMap extends LinkedHashMap<String, Object> {}
  public class ExtendedModelMap extends ModelMap implements Model {}
  public class BindingAwareModelMap extends ExtendedModelMap {}
  ~~~



### 向session域共享数据

~~~java
@RequestMapping("/testSession")
public String testSession(HttpSession session){
    session.setAttribute("testSessionScope", "hello,session");
    return "success";
}
~~~

~~~html
<p th:text="${session.testSessionScope}"></p>
~~~



### 向application域共享数据

~~~java
@RequestMapping("/testApplication")
public String testApplication(HttpSession session){
	ServletContext application = session.getServletContext();
    application.setAttribute("testApplicationScope", "hello,application");
    return "success";
}
~~~

~~~html
<p th:text="${application.testApplicationScope}"></p>
~~~



## SpringMVC的视图

* SpringMVC中的视图是View接口，视图的作用渲染数据，将模型Model中的数据展示给用户
* SpringMVC视图的种类很多，默认有转发视图和重定向视图。
* 当工程引入jstl的依赖，转发视图会自动转换为JstlView
  当工程使用视图技术Thymeleaf，在SpringMVC的配置文件中配置了Thymeleaf的视图解析器，由此视图解析器解析之后得到的是ThymeleafView

### ThymeleafView
当控制器方法中所设置的视图名称没有任何前缀时，此时的视图名称会被SpringMVC配置文件中所配置的视图解析器解析，视图名称拼接视图前缀和视图后缀所得到的最终路径，会通过转发的方式实现跳转

~~~java
@RequestMapping("/testThymeleafView")
public String testHello(){
    return "success"; //视图
}
~~~

~~~html
<a th:href="@{/testThymeleafView}">测试ThymeleafView</a><br>
~~~



![img002](E:\软工\typora笔记\JavaEE框架\SpringMVC.assets\img002.png)

### 转发视图

SpringMVC中默认（internalResourceView）

当控制器所设置的视图名称为“forward:"为前缀时，创建internalResourceView视图，此时的视图不会被SpringMVC配置文件中所配置的视图解析器解析，而是会将前缀”forward:"去掉，剩余部分作为最终路径通过转发的方式实现跳转

~~~java
@RequestMapping("/testForward")
public String testForward(){
    return "forward:/testThymeleafView";
}
~~~

~~~html
<a th:href="@{/testForward}">测试InternalResourceView</a><br>
~~~



![img003](E:\软工\typora笔记\JavaEE框架\SpringMVC.assets\img003.png)

### 重定向视图

（RedirectView）

当控制器 所设置的视图名称为“redirect：”为前缀是，创建RedirectView视图，此时视图名称不会被SpringMVC配置文件中所配置的视图解析器解析，而是会将前缀”redirect:"去掉，**判断剩余部分是否以/开头，若是则会自动拼接上下文路径**，作为最终路径通过重定向 的方式实现跳转

~~~java
@RequestMapping("/testRedirect")
public String testRedirect(){
    return "redirect:/testThymeleafView";
}
~~~

~~~html
<a th:href="@{/testRedirect}">测试RedirectView</a><br>
~~~



![img004](E:\软工\typora笔记\JavaEE框架\SpringMVC.assets\img004.png)

### 视图控制器

(view-controller)

当控制器方法中**仅仅用来实现页面跳转**，即只需要设置视图名称时，可以将处理器方法使用view-controller标签进行表示。
spring MVC的配置文件中

~~~xml
<!-- 
	当SpringMVC中设置任何一个view-controller时，
	其他控制器中的请求映射将全部失效，
	此时需要在SpringMVC的核心配置文件中设置开启mvc注解驱动的标签
-->
<mvc:annotation-driven />

<!--
	path：设置处理的请求地址
	view-name：设置请求地址所对应的视图名称
-->
<mvc:view-controller path="/testView" view-name="success"></mvc:view-controller>
~~~



## RESTful

* REST:表现层资源状态转移 Representational State Transfer
* 资源：一种看待服务器的方法，即，将服务器看作是由很多离散的资源组成。一个资源可以由一个或多个URI来标识。URI既是资源的名称，也是资源在Web上的地址。
* 表述层资源：资源的表述是一段对于资源在某个特定时刻的状态的描述。可以在客户端-服务器端之间转移（交换）。资源的表述可以有多种格式，例如HTML/XML/JSON/纯文本/图片/视频/音频等等。
* 状态转移：在客户端和服务器端之间转移（transfer）代表资源状态的表述。通过转移和操作资源的表述，来间接实现操作资源的目的。

### 路径中的占位符

原始方式：/deleteUser?id=1
rest方式：/deleteUser/1

* REST 风格提倡 URL 地址使用统一的风格设计，从前到后各个单词使用斜杠分开，不使用问号键值对方式携带请求参数，而是将要发送给服务器的数据作为 URL 地址的一部分，以保证整体风格的一致性。
* SpringMVC路径中的占位符常用于RESTful风格中，
  当请求路径中将某些数据通过路径的方式传输到服务器中，
  就可以在相应的@RequestMapping注解的value属性中通过占位符{xxx}表示传输的数据，
  在通过@PathVariable注解，将占位符所表示的数据赋值给控制器方法的形参

~~~java
@RequestMapping("/testRest/{id}/{username}")
public String testRest(
    @PathVariable("id") String id, 
    @PathVariable("username") String username
){
    System.out.println("id:"+id+",username:"+username);
    return "success";
}
//最终输出的内容为-->id:1,username:admin
~~~

~~~html
<a th:href="@{/testRest/1/admin}">测试路径中的占位符-->/testRest</a><br>
~~~

 ### REST风格实现

* HTTP协议中四个表示操作方式的动词：GET（获取资源）、POST（新建资源）、PUT（更新资源）、DELETE（删除资源）

  | 操作     | 传统方式         | REST风格 | 请求方式 |
  | -------- | ---------------- | -------- | -------- |
  | 查询操作 | getUserById?id=1 | user/1   | get      |
  | 保存操作 | saveUser         | user     | post     |
  | 更新操作 | deleteUser       | user     | put      |
  | 删除操作 | updateUser?id=1  | user/1   | delete   |

#### GET（获取资源）

~~~java
@RequestMapping(value = "/employee", method = RequestMethod.GET)
public String getEmployeeList(Model model){
    //从数据库查询数据放入列表Collection
    Collection<Employee> employeeList = employeeDao.getAll();
    //将列表Collection中的数据放入域中
    model.addAttribute("employeeList", employeeList);
    return "employee_list";
}
~~~

~~~html
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>Employee Info</title>
    <!-- 用到了vue框架 -->
    <script type="text/javascript" th:src="@{/static/js/vue.js}"></script>
</head>
<body>

    <table border="1" cellpadding="0" cellspacing="0" style="text-align: center;" id="dataTable">
        <tr>
            <th colspan="5">Employee Info</th>
        </tr>
        <tr>
            <th>id</th>
            <th>lastName</th>
            <th>email</th>
            <th>gender</th>
            <th>options(<a th:href="@{/toAdd}">add</a>)</th>
        </tr>
        <!-- th:each="循环因子 : ${域中循环体}" -->
        <tr th:each="employee : ${employeeList}">
            <!-- th:text="${循环因子.属性值}" -->
            <td th:text="${employee.id}"></td>
            <td th:text="${employee.lastName}"></td>
            <td th:text="${employee.email}"></td>
            <td th:text="${employee.gender}"></td>
            <td>
                <!-- vue语法 点击触发事件 -->
<!-- 
	注意带域中参数的url写法 th:href="@{'/employee/'+${employee.id}}" 
-->
                <a class="deleteA" @click="deleteEmployee" th:href="@{'/employee/'+${employee.id}}">delete</a>
                <a th:href="@{'/employee/'+${employee.id}}">update</a>
            </td>
        </tr>
    </table>
</body>
</html>
~~~

#### POST（新建资源）

* 前提跳转到添加数据页面

  ~~~java
  <mvc:view-controller path="/toAdd" view-name="employee_add"></mvc:view-controller>
  ~~~

  ~~~html
  <!DOCTYPE html>
  <html lang="en" xmlns:th="http://www.thymeleaf.org">
  <head>
      <meta charset="UTF-8">
      <title>Add Employee</title>
  </head>
  <body>
  
  <form th:action="@{/employee}" method="post">
      lastName:<input type="text" name="lastName"><br>
      email:<input type="text" name="email"><br>
      gender:<input type="radio" name="gender" value="1">male
      <input type="radio" name="gender" value="0">female<br>
      <input type="submit" value="add"><br>
  </form>
  
  </body>
  </html>
  ~~~

* 后期实现 保存

  ~~~java
  @RequestMapping(value = "/employee", method = RequestMethod.POST)
  public String addEmployee(Employee employee){
      employeeDao.save(employee);
      return "redirect:/employee";
  }
  ~~~

  ~~~html
  <!DOCTYPE html>
  <html lang="en" xmlns:th="http://www.thymeleaf.org">
  <head>
      <meta charset="UTF-8">
      <title>Add Employee</title>
  </head>
  <body>
  
  <form th:action="@{/employee}" method="post">
      lastName:<input type="text" name="lastName"><br>
      email:<input type="text" name="email"><br>
      gender:<input type="radio" name="gender" value="1">male
      <input type="radio" name="gender" value="0">female<br>
      <input type="submit" value="add"><br>
  </form>
  
  </body>
  </html>
  ~~~

  

### 解决put 和 delete请求

* SpringMVC提供了 HiddenHttpMethodFilter 将 POST 请求转换成 DELETE 或 PUT 请求

*  HiddenHttpMethodFilter 处理 put 和 delete 条件
  \> 请求方式必须为post
  \> 必须传输请求参数 _method
  此时，HiddenHttpMethodFilter 过滤器就会将当前请求方式转换为请求参数 _method 的值，因此请求参数 _method 的值才是最终的请求方式。

* HiddenHttpMethodFilter 注册在web.xml 中

  ~~~xml
  <filter>
      <filter-name>HiddenHttpMethodFilter</filter-name>
      <filter-class>org.springframework.web.filter.HiddenHttpMethodFilter</filter-class>
  </filter>
  <filter-mapping>
      <filter-name>HiddenHttpMethodFilter</filter-name>
      <url-pattern>/*</url-pattern>
  </filter-mapping>
  ~~~

* CharacterEncodingFilter 和 HiddenHttpMethodFilter 两个过滤器的先后顺序
  在web.xml中注册时，
  必须先注册CharacterEncodingFilter，
  再注册HiddenHttpMethodFilter

  > 原因：
  >
  > 1. 在 CharacterEncodingFilter 中通过 request.setCharacterEncoding(encoding) 方法设置字符集的
  >
  > 2. request.setCharacterEncoding(encoding) 方法要求前面不能有任何获取请求参数的操作
  >
  > 3. HiddenHttpMethodFilter 恰恰有一个获取请求方式的操作：
  >
  >    ~~~java
  >    String paramValue = request.getParameter(this.methodParam);
  >    ~~~

#### PUT（更新资源）

* 前提跳转到更新数据页面

  ~~~java
  @RequestMapping(value = "/employee/{id}", method = RequestMethod.GET)
  public String getEmployeeById(@PathVariable("id") Integer id, Model model){
      Employee employee = employeeDao.get(id);
      model.addAttribute("employee", employee);
      return "employee_update";
  }
  ~~~

  ~~~html
  <a th:href="@{'/employee/'+${employee.id}}">update</a>
  ~~~

* 后期实现 回显＋更新

  ~~~java
  @RequestMapping(value = "/employee", method = RequestMethod.PUT)
  public String updateEmployee(Employee employee){
      employeeDao.save(employee);
      return "redirect:/employee";
  }
  ~~~

  ~~~html
  <!DOCTYPE html>
  <html lang="en" xmlns:th="http://www.thymeleaf.org">
  <head>
      <meta charset="UTF-8">
      <title>Update Employee</title>
  </head>
  <body>
  
  <!-- 作用：通过超链接控制表单的提交，将post请求转换为delete请求 -->
  <form th:action="@{/employee}" method="post">
      <!-- HiddenHttpMethodFilter要求：必须传输_method请求参数，并且值为最终的请求方式 -->
      <input type="hidden" name="_method" value="put">
      
      <input type="hidden" name="id" th:value="${employee.id}">
      lastName:<input type="text" name="lastName" th:value="${employee.lastName}"><br>
      email:<input type="text" name="email" th:value="${employee.email}"><br>
      
      <!--
          th:field="${employee.gender}"可用于单选框或复选框的回显
          若单选框的value和employee.gender的值一致，则添加checked="checked"属性
      -->
      
      gender:<input type="radio" name="gender" value="1" th:field="${employee.gender}">male
      <input type="radio" name="gender" value="0" th:field="${employee.gender}">female<br>
      <input type="submit" value="update"><br>
  </form>
  
  </body>
  </html>
  ~~~

#### DELETE（删除资源）

![image-20211128110104131](E:\软工\typora笔记\JavaEE框架\SpringMVC.assets\image-20211128110104131.png)

~~~java
@RequestMapping(value = "/employee/{id}", method = RequestMethod.DELETE)
public String deleteEmployee(@PathVariable("id") Integer id){
    employeeDao.delete(id);
    return "redirect:/employee";
}
~~~

~~~html
<!-- 作用：通过超链接控制表单的提交，将post请求转换为delete请求 -->
<form id="delete_form" method="post">
    <!-- HiddenHttpMethodFilter要求：必须传输_method请求参数，并且值为最终的请求方式 -->
    <input type="hidden" name="_method" value="delete"/>
</form>

<!-- 引入：vue -->
<script type="text/javascript" th:src="@{/static/js/vue.js}"></script>

<!-- vue点击事件 -->
<a class="deleteA" @click="deleteEmployee" th:href="@{'/employee/'+${employee.id}}">delete</a>

<script type="text/javascript">
    var vue = new Vue({
        el:"#dataTable",
        methods:{
            //event表示当前事件
            deleteEmployee:function (event) {
                //通过id获取表单标签
                var delete_form = document.getElementById("delete_form");
                //将触发事件的超链接的href属性为表单的action属性赋值
                delete_form.action = event.target.href;
                //提交表单
                delete_form.submit();
                //阻止超链接的默认跳转行为
                event.preventDefault();
            }
        }
    });
</script>
~~~



## HttpMessageConverter

* 报文转换器，将请求报文转换成Java对象，或将Java对象转换成响应报文。
* 提供两个注解和两个类型：@RequestBody，@ResponseBody，
  RequestEntity，ResponseEntity

### @RequestBody 和 @ResponseBody

* @RequestBody可以获取请求体，需要在控制器方法设置一个形参，使用此注解进行标识，当前请求的请求体就会为当前注解所标识的形参赋值

  ```html
  <form th:action="@{/testRequestBody}" method="post">
      用户名：<input type="text" name="username"><br>
      密码：<input type="password" name="password"><br>
      <input type="submit">
  </form>
  ```

  ```java
  @RequestMapping("/testRequestBody")
  public String testRequestBody(@RequestBody String requestBody){
      System.out.println("requestBody:"+requestBody);
      return "success";
  }
  
  //输出结果：requestBody:username=admin&password=123456
  ```

* @ResponseBody用于标识一个控制器方法，可以将该方法的返回值直接作为响应报文的响应体响应到浏览器页面上

  ```java
  @RequestMapping("/testResponseBody")
  @ResponseBody
  public String testResponseBody(){
      return "success"; //不是指页面，是指输出到页面的字符串
  }
  
  //结果：浏览器页面显示 success
  ```

  **@RestController**是springMVC提供的一个复合注解，**标识在控制器的类上**，
  就相当于为类添加了@Controller，并且为其中每个方法添加了@ResponseBody

#### @ResponseBody 处理 json 和 ajax

* 处理json

  1. 在pom.xml中导入jackson依赖

     ~~~xml
     <dependency>
         <groupId>com.fasterxml.jackson.core</groupId>
         <artifactId>jackson-databind</artifactId>
         <version>2.12.1</version>
     </dependency>
     ~~~

  2. 在springMVC配置文件中开启mvc注解驱动，
     此时在HandlerAdaptor中会自动装配一个信息转换器：MappingJackson2HttpMessageConverter
     可以将响应到浏览器的Java对象转换成Json格式的字符串

     ~~~xml
     <mvc:annotation-driven />
     ~~~

  3. 在处理器方法方法上用@ResponseBody进行标识

  4. 将Java对象直接作为控制器方法的返回值返回，就会自动转换成为Json格式的字符串

     ```java
     @RequestMapping("/testResponseUser")
     @ResponseBody
     public User testResponseUser(){
         return new User(1001,"admin","123456",23,"男");
     }
     
     //浏览器的页面中展示的结果：{"id":1001,"username":"admin","password":"123456","age":23,"sex":"男"}
     ```

* 处理ajax
  ![image-20211128131957910](E:\软工\typora笔记\JavaEE框架\SpringMVC.assets\image-20211128131957910.png)

  1. 请求超链接

     ```html
     <div id="app">
     	<a th:href="@{/testAjax}" @click="testAjax">testAjax</a><br>
     </div>
     ```

  2. 通过vue和axios处理点击事件

     ```html
     <script type="text/javascript" th:src="@{/static/js/vue.js}"></script>
     <script type="text/javascript" th:src="@{/static/js/axios.min.js}"></script>
     <script type="text/javascript">
         var vue = new Vue({
             el:"#app",
             methods:{
                 testAjax:function (event) {
                     axios({
                         method:"post",
                         url:event.target.href,
                         params:{
                             username:"admin",
                             password:"123456"
                         }
                     }).then(function (response) {
                         alert(response.data);
                     });
                     event.preventDefault();
                 }
             }
         });
     </script>
     ```

  3. 控制器方法

     ```java
     @RequestMapping("/testAjax")
     @ResponseBody
     public String testAjax(String username, String password){
         System.out.println("username:"+username+",password:"+password);
         return "hello,ajax";
     }
     ```

     

### RequestEntity 和 ResponseEntity

* RequestEntity封装请求报文的一种类型，需要在控制方法的形参中设置该类型的形参，当前请求的请求报文就会赋值给该形参。
  getHeaders()			获取请求头信息
  getBody()				  获取请求体信息

  ```java
  @RequestMapping("/testRequestEntity")
  public String testRequestEntity(RequestEntity<String> requestEntity){
      System.out.println("requestHeader:"+requestEntity.getHeaders());
      System.out.println("\n");
      System.out.println("requestBody:"+requestEntity.getBody());
      return "success";
  }
  
  /*
  输出结果：
  requestHeader:[host:"localhost:8080", connection:"keep-alive", content-length:"27", cache-control:"max-age=0", sec-ch-ua:"" Not A;Brand";v="99", "Chromium";v="90", "Google Chrome";v="90"", sec-ch-ua-mobile:"?0", upgrade-insecure-requests:"1", origin:"http://localhost:8080", user-agent:"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/90.0.4430.93 Safari/537.36"]
  
  requestBody:username=admin&password=123
  */
  ```

* ResponseEntity用于控制器方法的返回值类型，该控制器方法的返回值就是响应到浏览器的响应报文



## 文件上传和下载

* 下载：使用ResponseEntity实现下载文件功能

  ~~~html
  <a th:href="@{/testDown}">下载1.jpg</a><br>
  ~~~

  ```java
  @RequestMapping("/testDown")
  public ResponseEntity<byte[]> testResponseEntity(HttpSession session) throws IOException {
      //1. 获取ServletContext对象
      ServletContext servletContext = session.getServletContext();
      //2. 获取服务器中文件的真实路径
      String realPath = servletContext.getRealPath("/static/img/1.jpg");
      //3. 创建输入流
      InputStream is = new FileInputStream(realPath);
      //4. 创建字节数组 将流读到字节数组中
      byte[] bytes = new byte[is.available()];
      is.read(bytes);
      //5. 创建HttpHeaders对象设置响应头信息
      MultiValueMap<String, String> headers = new HttpHeaders();
      //6. 设置要下载方式以及下载文件的名字
      headers.add("Content-Disposition", "attachment;filename=1.jpg");
      //7. 设置响应状态码
      HttpStatus statusCode = HttpStatus.OK;
      //8. 创建ResponseEntity对象
      ResponseEntity<byte[]> responseEntity = new ResponseEntity<>(bytes, headers, statusCode);
      //9. 关闭输入流
      is.close();
      return responseEntity;
  }
  ```

* 上传：form表单的请求方式必须为post，并且添加属性enctype="multipart/form-data"
  SpringMVC中将上传的文件封装到MultipartFile对象中，通过此对象可以获取文件相关信息

  ~~~html
  <form th:action="@{/testUp}" method="post" enctype="multipart/form-data">
      头像：<input type="file" name="photo"><br>
      <input type="submit" value="上传">
  </form>
  ~~~

  1. 添加依赖：

     ```xml
     <!-- https://mvnrepository.com/artifact/commons-fileupload/commons-fileupload -->
     <dependency>
         <groupId>commons-fileupload</groupId>
         <artifactId>commons-fileupload</artifactId>
         <version>1.3.1</version>
     </dependency>
     ```

  2. 在SpringMVC的配置文件中添加配置:

     ```xml
     <!--必须通过文件解析器的解析才能将文件转换为MultipartFile对象-->
     <bean id="multipartResolver" class="org.springframework.web.multipart.commons.CommonsMultipartResolver"></bean>
     ```

  3. 控制器方法:

     ```java
     @RequestMapping("/testUp")
     public String testUp(MultipartFile photo, HttpSession session) throws IOException {
         //1. 获取上传的文件的文件名
         String fileName = photo.getOriginalFilename();
         //2. 处理文件重名问题 获取上传的文件的后缀名 将UUID作为文件名 
         String hzName = fileName.substring(fileName.lastIndexOf("."));
         fileName = UUID.randomUUID().toString() + hzName;
         //3. 获取服务器中photo目录的路径 将uuid和后缀名拼接后的结果作为最终的文件名 
         ServletContext servletContext = session.getServletContext();
         String photoPath = servletContext.getRealPath("photo");
         File file = new File(photoPath);
         if(!file.exists()){
             file.mkdir();
         }
         //File.separator 根据系统自身判断分隔符是 \ 还是 /
         String finalPath = photoPath + File.separator + fileName;
         //4. 实现上传功能
         photo.transferTo(new File(finalPath));
         return "success";
     }
     ```


## 拦截器

* 拦截器用于拦截控制器方法的执行

* 拦截器配置：必须在SpringMVC的配置文件中进行配置

  ```xml
  <bean class="com.atguigu.interceptor.FirstInterceptor"></bean>
  <ref bean="firstInterceptor"></ref>
  <!-- 以上两种配置方式都是对DispatcherServlet所处理的所有的请求进行拦截 -->
  
  <mvc:interceptor>
      <mvc:mapping path="/**"/>
      <mvc:exclude-mapping path="/testRequestEntity"/>
      <ref bean="firstInterceptor"></ref>
  </mvc:interceptor>
  <!-- 
  	以上配置方式可以通过ref或bean标签设置拦截器，通过mvc:mapping设置需要拦截的请求，通过mvc:exclude-mapping设置需要排除的请求，即不需要拦截的请求
  -->
  ```

* 拦截器方法：需要实现HandlerInterceptor，并重写三个抽象方法

  | 方法            | 说明                                                         |
  | --------------- | ------------------------------------------------------------ |
  | preHandle       | 控制器方法执行之前执行preHandle()，其boolean类型的返回值表示是否拦截或放行，返回**true为放行**，即调用控制器方法；返回**false表示拦截**，即不调用控制器方法 |
  | postHandle      | 控制器方法执行之后执行postHandle()                           |
  | afterComplation | 处理完视图和模型数据，渲染视图完毕之后执行afterComplation()  |

  ~~~java
  @Component
  public class FirstInterceptor implements HandlerInterceptor {
  
      @Override
      public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
          System.out.println("FirstInterceptor-->preHandle");
          return true;
      }
  
      @Override
      public void postHandle(HttpServletRequest request, HttpServletResponse response, Object handler, ModelAndView modelAndView) throws Exception {
          System.out.println("FirstInterceptor-->postHandle");
      }
  
      @Override
      public void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler, Exception ex) throws Exception {
          System.out.println("FirstInterceptor-->afterCompletion");
      }
  }
  
  ~~~

  

* 多个拦截器的执行顺序
  \> 若每个拦截器的preHandle()都返回 true
  此时多个拦截器的执行顺序和拦截器在SpringMVC的配置文件的**配置顺序有关**：
  preHandle()会按照配置的顺序执行，
  而postHandle()和afterComplation()会按照配置的反序执行

  \> 若某个拦截器的preHandle()返回了 false
  preHandle()返回false和它之前的拦截器的preHandle()都会执行，
  postHandle()都不执行，
  返回false的拦截器之前的拦截器的afterComplation()会执行

## 异常处理

~~~html
<p th:text="${ex}"></p>
~~~

* 基于配置的异常处理

  \> SpringMVC提供了一个处理控制器方法执行过程中所出现的异常的接口：HandlerExceptionResolver

  \> HandlerExceptionResolver接口的实现类有：DefaultHandlerExceptionResolver和SimpleMappingExceptionResolver

  \> SpringMVC提供了自定义的异常处理器SimpleMappingExceptionResolver

  ```xml
  <bean class="org.springframework.web.servlet.handler.SimpleMappingExceptionResolver">
      <!--
            properties的键表示处理器方法执行过程中出现的异常
            properties的值表示若出现指定异常时，设置一个新的视图名称，跳转到指定页面
           -->
      <property name="exceptionMappings">
          <props>
              <prop key="java.lang.ArithmeticException">error</prop>
          </props>
      </property>
      <!--
      	exceptionAttribute属性设置一个属性名，将出现的异常信息在请求域中进行共享
      -->
      <property name="exceptionAttribute" value="ex"></property>
  </bean>
  ```

* 基于注解的异常处理

  ```java
  //@ControllerAdvice将当前类标识为异常处理的组件
  @ControllerAdvice
  public class ExceptionController {
  
      //@ExceptionHandler用于设置所标识方法处理的异常
      @ExceptionHandler(ArithmeticException.class)
      //ex表示当前请求处理中出现的异常对象
      public String handleArithmeticException(Exception ex, Model model){
          model.addAttribute("ex", ex);
          return "error";
      }
  
  }
  ```




## 基于注解配置SpringMVC

使用配置类和注解代替web.xml和SpringMVC配置文件的功能

1. 创建初始化类，代替web.xml

   > 在Servlet3.0环境中
   >
   > 容器会在类路径中查找实现javax.servlet.ServletContainerInitializer接口的类，如果找到的话就用它来配置Servlet容器。
   >
   > Spring提供了这个接口的实现，名为SpringServletContainerInitializer，这个类反过来又会查找实现WebApplicationInitializer的类并将配置的任务交给它们来完成。
   >
   > Spring3.2引入了一个便利的WebApplicationInitializer基础实现，名为AbstractAnnotationConfigDispatcherServletInitializer。
   >
   > 当我们的类扩展了AbstractAnnotationConfigDispatcherServletInitializer并将其部署到Servlet3.0容器的时候，容器会自动发现它，并用它来配置Servlet上下文。

   ```java
   public class WebInit extends AbstractAnnotationConfigDispatcherServletInitializer {
   
       /**
        * 指定spring的配置类
        * @return
        */
       @Override
       protected Class<?>[] getRootConfigClasses() {
           return new Class[]{SpringConfig.class};
       }
   
       /**
        * 指定SpringMVC的配置类
        * @return
        */
       @Override
       protected Class<?>[] getServletConfigClasses() {
           return new Class[]{WebConfig.class};
       }
   
       /**
        * 指定DispatcherServlet的映射规则，即url-pattern
        * @return
        */
       @Override
       protected String[] getServletMappings() {
           return new String[]{"/"};
       }
   
       /**
        * 添加过滤器
        * @return
        */
       @Override
       protected Filter[] getServletFilters() {
           CharacterEncodingFilter encodingFilter = new CharacterEncodingFilter();
           encodingFilter.setEncoding("UTF-8");
           encodingFilter.setForceRequestEncoding(true);
           
           HiddenHttpMethodFilter hiddenHttpMethodFilter = new HiddenHttpMethodFilter();
           return new Filter[]{encodingFilter, hiddenHttpMethodFilter};
       }
   }
   ```

2. 创建SpringConfig配置类，代替spring配置文件

   ```java
   @Configuration
   public class SpringConfig {
   	//ssm整合之后，spring的配置信息写在此类中
   }
   ```

3. 创建WebConfig配置类，代替SpringMVC的配置文件

   ```java
   /**
    * 代替SpringMVC的配置文件：
    * 1、扫描组件   2、视图解析器     3、view-controller    4、default-servlet-handler
    * 5、mvc注解驱动    6、文件上传解析器   7、异常处理      8、拦截器
    */
   
   @Configuration
   //1、扫描组件
   @ComponentScan("com.atguigu.mvc.controller")
   //5、mvc注解驱动
   @EnableWebMvc
   public class WebConfig implements WebMvcConfigurer {
   
       //4、使用默认的servlet处理静态资源
       @Override
       public void configureDefaultServletHandling(DefaultServletHandlerConfigurer configurer) {
           configurer.enable();
       }
   
       //6、文件上传解析器
       @Bean
       public CommonsMultipartResolver multipartResolver(){
           return new CommonsMultipartResolver();
       }
   
       //8、拦截器
       @Override
       public void addInterceptors(InterceptorRegistry registry) {
           FirstInterceptor firstInterceptor = new FirstInterceptor();
           registry.addInterceptor(firstInterceptor).addPathPatterns("/**");
       }
       
       //3、配置视图控制
       
       @Override
       public void addViewControllers(ViewControllerRegistry registry) {
           registry.addViewController("/").setViewName("index");
       }
       
       //7、配置异常映射
       @Override
       public void configureHandlerExceptionResolvers(List<HandlerExceptionResolver> resolvers) {
           SimpleMappingExceptionResolver exceptionResolver = new SimpleMappingExceptionResolver();
           Properties prop = new Properties();
           prop.setProperty("java.lang.ArithmeticException", "error");
           //设置异常映射
           exceptionResolver.setExceptionMappings(prop);
           //设置共享异常信息的键
           exceptionResolver.setExceptionAttribute("ex");
           resolvers.add(exceptionResolver);
       }
   
       //配置生成模板解析器
       @Bean
       public ITemplateResolver templateResolver() {
           WebApplicationContext webApplicationContext = ContextLoader.getCurrentWebApplicationContext();
           // ServletContextTemplateResolver需要一个ServletContext作为构造参数，可通过WebApplicationContext 的方法获得
           ServletContextTemplateResolver templateResolver = new ServletContextTemplateResolver(
                   webApplicationContext.getServletContext());
           templateResolver.setPrefix("/WEB-INF/templates/");
           templateResolver.setSuffix(".html");
           templateResolver.setCharacterEncoding("UTF-8");
           templateResolver.setTemplateMode(TemplateMode.HTML);
           return templateResolver;
       }
   
       //生成模板引擎并为模板引擎注入模板解析器
       @Bean
       public SpringTemplateEngine templateEngine(ITemplateResolver templateResolver) {
           SpringTemplateEngine templateEngine = new SpringTemplateEngine();
           templateEngine.setTemplateResolver(templateResolver);
           return templateEngine;
       }
   
       //生成视图解析器并为解析器注入模板引擎
       @Bean
       public ViewResolver viewResolver(SpringTemplateEngine templateEngine) {
           ThymeleafViewResolver viewResolver = new ThymeleafViewResolver();
           viewResolver.setCharacterEncoding("UTF-8");
           viewResolver.setTemplateEngine(templateEngine);
           return viewResolver;
       }
   
   
   }
   ```




## SpringMVC执行流程

![绘图2](E:\软工\typora笔记\JavaEE框架\SpringMVC.assets\绘图2-16380803738141.png)

* 底层原理 （略）