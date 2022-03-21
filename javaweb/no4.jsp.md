[toc]

# javaweb——jsp

## 概念

jsp 全称 java server pages，Java的服务器页面。

* 代替Servlet程序回传html页面的数据，因为Servlet程序回传html页面数据是一件非常繁琐的事情，开发成本和维护成本极高。

  ~~~java
  public class PringHtml extends HttpServlet {
      @Override
      protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
          // 通过响应的回传流回传html页面数据
          resp.setContentType("text/html; charset=UTF-8");
          PrintWriter writer = resp.getWriter();
          writer.write("<!DOCTYPE html>\r\n");
          writer.write("  <html lang=\"en\">\r\n");
          writer.write("  <head>\r\n");
          writer.write("      <meta charset=\"UTF-8\">\r\n");
          writer.write("      <title>Title</title>\r\n");
          writer.write("  </head>\r\n");
          writer.write(" <body>\r\n");
          writer.write("    这是html页面数据 \r\n");
          writer.write("  </body>\r\n");
          writer.write("</html>\r\n");
          writer.write("\r\n");
      }
  }
  ~~~

  jsp 回传 html 页面数据

  ~~~jsp
  <%@ page contentType="text/html;charset=UTF-8" language="java" %>
  <html>
  <head>
      <title>Title</title>
  </head>
  <body>
      很抱歉，您访问的页面服务器出现错误，程序猿小哥正在努力为您抢修！！！
  </body>
  </html>
  ~~~

* jsp页面和html页面都是存放在web目录下，访问方式和访问html页面一样

  > web 目录
  >
  > ​	a.html 页面 			访问地址是 =======>>>>>> http://ip:port/工程路径/a.html 
  > ​	b.jsp 页面 				访问地址是 =======>>>>>> http://ip:port/工程路径/b.jsp

### jsp 本质上是一个Servlet 程序

* 第一次访问jsp页面，tomcat服务器会帮我们把jsp页面翻译成为一个Java源文件，并且对他进行编译为 .class字节码程序。

* jsp翻译出来的Java类 继承 HttpJspBase 类，它直接继承了 HttpServlet类。即jsp翻译出来的是一个Servlet程序
  ![image-20210926090920410](E:\软工\typora笔记\javaweb\no4.jsp.assets\image-20210926090920410.png)


  ![image-20210926091022844](E:\软工\typora笔记\javaweb\no4.jsp.assets\image-20210926091022844.png)

* jsp翻译出来的Java类也是通过输出流，把html页面数据回传给客户端

  ~~~java
  out.write("\r\n");
  out.write("\r\n");
  out.write("<html>\r\n");
  out.write("<head>\r\n");
  out.write(" <title>Title</title>\r\n");
  out.write("</head>\r\n");
  out.write("<body>\r\n");
  out.write(" a.jsp 页面\r\n");
  out.write("</body>\r\n");
  out.write("</html>\r\n");
  ~~~

## 语法

### jsp头部的page指令
（page指令可以修改jsp页面中一些重要的属性、行为）

~~~jsp
<%@ page contentType"text/html;charset=UTF-8" language="java"
    pageEncoding="UTF-8"
    autoFlush="flase"
    errorPage="/b.jps"%>
~~~

| 属性         | 说明                                                         |
| ------------ | ------------------------------------------------------------ |
| language     | 表示jsp翻译后是什么语言文件，只支持Java                      |
| contentType  | jsp返回的是什么数据类型，Servlet中response.setContentType()参数值 |
| pageEncoding | 表示当前jsp页面文件本身的字符集                              |
| import       | 跟Java源码一样，导包、导类                                   |
| autoFlush    | 设置当out输出流缓冲区满了之后，是否自动刷新冲级区。默认true  |
| buffer       | 设置out缓冲区的大小，默认8kb                                 |
| errorPage    | 设置jsp页面运行时出错，自动跳转去的错误页面路径              |
| isErrorPage  | 设置当前jsp页面是否是错误信息页面，默认false。true可以获取异常信息 |
| session      | 设置访问当前jsp页面，是否创建HttpSession对象，默认true       |
| extends      | 设置jsp翻译出来的Java类默认继承谁                            |

> errorPage
> 自动跳转的路径一般都是以斜杠打头，表示请求地址为`http://ip:port/工程路径/` 映射到代码的Web目录
>
> autoFlush
> 输出流满了后不刷新冲级区，会出面错误页面

### jsp中的常用脚本

* 声明脚本：`<%! 声明Java代码 %>`
  作用：可以给jsp翻译出来的Java类定义**属性和方法**甚至是**静态代码块，内部类**。

  ~~~jsp
  <%@ page import="java.util.Map" %>
  <%@ page import="java.util.HashMap" %>
  <%@ page contentType="text/html;charset=UTF-8" language="java" %>
  <html>
  <head>
      <title>Title</title>
  </head>
  <body>
  
  <%--1、声明类属性--%>
      <%!
          private Integer id;
          private String name;
          private static Map<String,Object> map;
      %>
      
  <%--2、声明static静态代码块--%>
      <%!
          static {
              map = new HashMap<String,Object>();
              map.put("key1", "value1");
              map.put("key2", "value2");
              map.put("key3", "value3");
          }
      %>
      
  <%--3、声明类方法--%>
      <%!
          public int abc(){
              return 12;
          }
      %>
      
  <%--4、声明内部类--%>
      <%!
          public static class A {
              private Integer id = 12;
              private String abc = "abc";
          }
      %>
  </body>
  </html>
  ~~~

* 表达式脚本：`<%=表达式%>`
  作用：jsp页面上输出数据

  > 1. 所有的表达式脚本都会被翻译到_jspService() 方法中
  >
  > 2. 表达式脚本都会被翻译成为out.print() 输出到页面上
  >
  > 3. 由于表达式脚本翻译的内容都在_jspService() 方法中，所以 _jspService() 方法中的对象都可以直接使用
  >
  > 4. 表法是脚本中的表达式不能以分号结束

  ~~~jsp
  <%@ page import="java.util.Map" %>
  <%@ page import="java.util.HashMap" %>
  <%@ page contentType="text/html;charset=UTF-8" language="java" %>
  <html>
  <head>
      <title>Title</title>
  </head>
  <body>
      <%!
      static {
          map = new HashMap<String,Object>();
          map.put("key1", "value1");
          map.put("key2", "value2");
          map.put("key3", "value3");
  	}
      %>
      <%=12 %> <br>
  
      <%=12.12 %> <br>
  
      <%="我是字符串" %> <br>
  
      <%=map%> <br>
  
      <%=request.getParameter("username")%>
  </body>
  </html>
  ~~~

* 代码脚本：`<% Java语句 %>`
  作用：可以编写自己需要的功能

  > 1. 代码脚本翻译之后都在_jspService()方法中
  > 2. 代码脚本翻译到_jspService()方法中，所以在 _jspService() 方法中的现有对象都可以直接使用
  > 3. 由多个代码脚本块组合完成一个完整的Java语句

  ~~~jsp
  <%@ page import="java.util.Map" %>
  <%@ page import="java.util.HashMap" %>
  <%@ page contentType="text/html;charset=UTF-8" language="java" %>
  <html>
  <head>
      <title>Title</title>
  </head>
  <body>
      
  <%--1.代码脚本----if 语句--%>
      <%
          int i = 13 ;
          if (i == 12) {
      %>
              <h1>国哥好帅</h1>
      <%	} else {	%>
          <h1>国哥又骗人了！</h1>
      <%	}	%>
  <br>
          
  <%--2.代码脚本----for 循环语句--%>
      <table border="1" cellspacing="0">
      <%	for (int j = 0; j < 10; j++) {	%>
          <tr>
              <td>第 <%=j + 1%>行</td>
          </tr>
      <%	}	%>
      </table>
          
  <%--3.翻译后java文件中_jspService方法内的代码都可以写--%>
      <%
          String username = request.getParameter("username");
          System.out.println("用户名的请求参数值是：" + username);
      %>
          
  </body>
  </html>
  
  ~~~

### 三种注释

* html注释：`<!-- 这是 html 注释 -->`
  html注释会被翻译到Java源代码中，在_jspService方法里，以out.writer输出到客户端中。

* Java注释：`// 单行 java 注释 /* 多行 java 注释 */`
  Java注释会被翻译到Java源代码中
* jsp注释：`<%-- 这是 jsp 注释 --%>`
  jsp注释可以注掉jsp页面中所有代码

### jsp九大内置对象

jsp中的内置对象指tomcat在翻译jsp页面成为Servlet源代码后，内部提供的九大对象，叫做内置对象

| 内置对象    | 说明               |
| ----------- | ------------------ |
| request     | 请求对象           |
| response    | 响应对象           |
| pageContext | jsp的上下文对象    |
| session     | 会话对象           |
| application | ServletContext对象 |
| config      | ServiceConfig对象  |
| out         | jsp输出流对象      |
| page        | 指向当前jsp的对象  |
| exception   | 异常对象           |

### jsp四大域对象

域对象是可以像Map一样存取数据的对象，四个域对象功能一样，不同的是它们对数据的存取范围。

| 域对象      | 类别                 | 说明                                                       |
| ----------- | -------------------- | ---------------------------------------------------------- |
| pageContext | PageContextImpl类    | 当前jsp页面范围内有效                                      |
| request     | HttpServletRequest类 | 一次请求内有效                                             |
| session     | HttpSession类        | 一个会话范围内有效（打开浏览器访问服务器，直到关闭浏览器） |
| application | ServletContext类     | 整个web工程范围内都有效（只要web工程不停止，数据都在）     |

> 使用上的优先顺序：小==>>大
> pageContext ==>> request ==>> session ==>> application

* 实例

  scope.jsp

  ~~~jsp
  <%@ page contentType="text/html;charset=UTF-8" language="java" %>
  <html>
  <head>
      <title>Title</title>
  </head>
  <body>
      <h1>scope.jsp页面</h1>
      <%
          // 往四个域中都分别保存了数据
          pageContext.setAttribute("key", "pageContext");
          request.setAttribute("key", "request");
          session.setAttribute("key", "session");
          application.setAttribute("key", "application");
      %>
      pageContext域是否有值：<%=pageContext.getAttribute("key")%> <br>
      request域是否有值：<%=request.getAttribute("key")%> <br>
      session域是否有值：<%=session.getAttribute("key")%> <br>
      application域是否有值：<%=application.getAttribute("key")%> <br>
  
      <% request.getRequestDispatcher("/scope2.jsp").forward(request,response); %>
  </body>
  </html>
  ~~~

  scope2.jsp

  ~~~jsp
  <%@ page contentType="text/html;charset=UTF-8" language="java" %>
  <html>
  <head>
      <title>Title</title>
  </head>
  <body>
      <h1>scope2.jsp页面</h1>
      pageContext域是否有值：<%=pageContext.getAttribute("key")%> <br>
      request域是否有值：<%=request.getAttribute("key")%> <br>
      session域是否有值：<%=session.getAttribute("key")%> <br>
      application域是否有值：<%=application.getAttribute("key")%> <br>
  </body>
  </html>
  ~~~

  **结果：**

  1. 没有 <jsp:forward page="/scope2.jsp">\</jsp:forward> ,刷新页面，访问到scope.jsp ==>> 都有值
  2. 有 <jsp:forward page="/scope2.jsp">\</jsp:forward>，刷新页面，访问到scope2.jsp ==>> pageContext没有值
  3. 注释掉 request.setAttribute("key", "request")； 刷新页面 ==>> request没有值
  4. 注释掉 session.setAttribute("key", "session"); 重启浏览器，再次访问页面 ==>> session没有值
  5. 注释掉 application.setAttribute("key", "application"); 重启服务器，再次访问页面 ==>> application没有值

### jsp中 out 和 response.getWriter 输出区别

response 中表示响应，经常用于设置返回给客户端的内容（输出）
out 将所有out信息放入 out缓冲区后，再传入response缓冲区
最后把response缓冲区中的所有信息发送给客户端

![image-20210927174748480](E:\软工\typora笔记\javaweb\no4.jsp.assets\image-20210927174748480.png)

**习惯：**
	由于 jsp 翻译之后，底层源代码都是使用 out 来进行输出，所以一般情况下。我们在 jsp 页面中统一使用 out 来进行输出。避免打乱页面输出内容的顺序。

* out.print() 和 out.write() 区别
  out.write() 输出字符串没有问题
  out.print() 输出任意数据都没有问题（都转换成为字符串后调用的write输出）

  ~~~jsp
  <%@ page contentType="text/html;charset=UTF-8" language="java" %>
  <html>
  <head>
      <title>Title</title>
  </head>
  <body>
      <%
          out.write(12); //输出 ASCII码是12的值
          out.print(12); //输出 12
      %>
  </body>
  </html>
  ~~~

  **习惯：**
  在jsp页面中，可以统一使用 out.print() 来进行输出

### 常用标签

* 静态包含： 

  ~~~jsp
  <%@ include file="/" %>
  
  <%-- file 属性指定包含的jsp页面的路径
  地址中 / 斜杠表示为 http://ip:port/工程路径/ 映射到代码的web目录 --%>
  ~~~

  静态包含不会翻译被包含的jsp页面，其实是把被包含的jsp页面的代码拷贝到包含的位置执行输出

* 动态包含

  ~~~jsp
  <jsp:include page="/">
      <%-- 动态包含还可以传递参数 --%>
  	<jsp:param name="username" value="bbj"/>
      <jsp:param name="username" value="bbj"/>
  </jsp:include>
  
  <%-- page 属性是指定包含的jsp页面的路径
  动态包含也可以像静态包含一样。把被包含的内容执行输出到包含位置。 --%>
  ~~~

  动态包含会把包含的页面翻译成为Java代码，并去调用包含的jsp页面执行输出：
  `JspRuntimeLibrary.include(request, response, "/", out, false);`

* 转发

  ~~~jsp
  <jsp:forward page="/"> </jsp:forward>
  ~~~

## Listener 监听器

javaweb三大组件之一，listener 是 JavaEE 的规范，就是接口。
作用：监听某种事物的变化，然后通过回调函数，反馈给客户去做一些相应的处理

* ServletContextListener
  它可以监听 ServletContext对象的创建和销毁，
  ServletContext对象在web工程启动的时候创建，停止的时候销毁。
  监听到创建和销毁之后都会分别调用ServletContextListener 监听器的方法反馈

  ~~~java
  public interface ServletContextListener extends EventListener {
  /**
  * 在 ServletContext 对象创建之后马上调用，做初始化
  */
  public void contextInitialized(ServletContextEvent sce);
  /**
  * 在 ServletContext 对象销毁之后调用
  */
  public void contextDestroyed(ServletContextEvent sce);
  }
  ~~~

  **步骤：**

  1. 编写一个实现类 ServletContextListener
  2. 实现两个回调方法
  3. 到web.xml中去配置监听器

  实现类

  ~~~java
  public class MyServletContextListenerImpl implements ServletContextListener {
      @Override
      public void contextInitialized(ServletContextEvent sce) {
      	System.out.println("ServletContext 对象被创建了");
      }
      
      @Override
      public void contextDestroyed(ServletContextEvent sce) {
      	System.out.println("ServletContext 对象被销毁了");
      }
  }
  ~~~

  web.xml 配置

  ~~~xml
  <listener> 						
      <listenerclass>
      	com.atguigu.listener.MyServletContextListenerImpl
      </listener-class>
  </listener>
  ~~~


## EL 表达式

EL表达式主要**代替jsp页面中的表达式脚本**在jsp页面中进行域对象中的数据输出，EL表达式更加简洁

* 格式：${表达式}
  EL表达式输出null值的时候，输出的是空串。
  jsp表达式脚本输出null值的时候，输出的是null字符串

  ~~~jsp
  <%@ page contentType="text/html;charset=UTF-8" language="java" %>
  <html>
  <head>
      <title>Title</title>
  </head>
  <body>
      <%
          request.setAttribute("key","值");
      %>
      表达式脚本输出key的值是：<%=request.getAttribute("key1")==null?"":request.getAttribute("key1")%><br/>
      EL表达式输出key的值是：${key1}
  </body>
  </html>
  ~~~

### EL表达式输出

* EL表达式搜索域数据的顺序（四大域对象）

  先 ==>> 后：pageContext ==>> request ==>> session ==>> application
  如：当四个域都有相同的 key 的数据，则EL表达式会按照前后顺序进行搜索输出

  ~~~jsp
  <%@ page contentType="text/html;charset=UTF-8" language="java" %>
  <html>
  <head>
      <title>Title</title>
  </head>
  <body>
      <%
          //往四个域中都保存了相同的key的数据。
          request.setAttribute("key", "request");
          session.setAttribute("key", "session");
          application.setAttribute("key", "application");
          pageContext.setAttribute("key", "pageContext");
      %>
      ${ key }
  </body>
  </html>
  ~~~

* EL表达式输出 普通属性、数组属性、list集合属性、map集合属性

  ~~~java
  public class Person {
  //    i.需求——输出Person类中普通属性，数组属性。list集合属性和map集合属性。
      private String name;
      private String[] phones;
      private List<String> cities;
      private Map<String,Object> map;
  
      public int getAge() {
          return 18;
      }
  
      public Person(String name, String[] phones, List<String> cities, Map<String, Object> map) {
          this.name = name;
          this.phones = phones;
          this.cities = cities;
          this.map = map;
      }
  
      public Person() {}
      public String getName() {return name;}
      public void setName(String name) {this.name = name;}
      public String[] getPhones() {return phones;}
      public void setPhones(String[] phones) {this.phones = phones;}
      public List<String> getCities() {return cities;}
      public void setCities(List<String> cities) {this.cities = cities;}
      public Map<String, Object> getMap() {return map;}
      public void setMap(Map<String, Object> map) {this.map = map;}
  
      @Override
      public String toString() {
          return "Person{" +
                  "name=" + name +
                  ", phones=" + Arrays.toString(phones) +
                  ", cities=" + cities +
                  ", map=" + map +
                  '}';
      }
  }
  ~~~
  
  ~~~jsp
  <%@ page import="com.atguigu.pojo.Person" %>
  <%@ page import="java.util.List" %>
  <%@ page import="java.util.ArrayList" %>
  <%@ page import="java.util.Map" %>
  <%@ page import="java.util.HashMap" %>
  <%@ page contentType="text/html;charset=UTF-8" language="java" %>
  <html>
  <head>
      <title>Title</title>
  </head>
  <body>
      <%
          Person person = new Person();
          person.setName("国哥好帅！");
          person.setPhones(new String[]{"18610541354","18688886666","18699998888"});
  
          List<String> cities = new ArrayList<String>();
          cities.add("北京");
          cities.add("上海");
          cities.add("深圳");
          person.setCities(cities);
  
          Map<String,Object>map = new HashMap<>();
          map.put("key1","value1");
          map.put("key2","value2");
          map.put("key3","value3");
          person.setMap(map);
  
          pageContext.setAttribute("p", person);
      %>
  
      输出Person：${ p }<br/>
      <%-- 域对象 用 . 取属性值 --%>
      输出Person的name属性：${p.name} <br>
      
      <%-- 数组、list集合 用 xxx[index] 取指定索引值 --%>
      输出Person的pnones数组属性值：${p.phones[2]} <br>
      输出Person的cities集合中的元素值：${p.cities} <br>
      输出Person的List集合中个别元素值：${p.citie
      s[2]} <br>
      输出Person的Map集合: ${p.map} <br>
      <%-- map集合 用 map.key 取指定Key值 --%>
      输出Person的Map集合中某个key的值: ${p.map.key3} <br>
      <%-- 取属性是调用getxxx()的方法返回值 --%>
      输出Person的age属性：${p.age} <br>
  </body>
  </html>
  ~~~

### EL表达式运算

${运算表达式}

| 关系运算符 | 说明     |
| ---------- | -------- |
| == 或 eq   | 等于     |
| != 或 ne   | 不等于   |
| < 或 lt    | 小于     |
| > 或 gt    | 大于     |
| <= 或 le   | 小于等于 |
| >= 或 ge   | 大于等于 |

| 逻辑运算符 | 说明     |
| ---------- | -------- |
| && 或 and  | 与运算   |
| \|\| 或 or | 或运算   |
| ! 或 not   | 取反运算 |

| 算数运算符 | 说明 |
| ---------- | ---- |
| +          | 加法 |
| -          | 减法 |
| *          | 乘法 |
| / 或 div   | 除法 |
| % 或 mod   | 取模 |

| 三元运算                  | 说明                                                 |
| ------------------------- | ---------------------------------------------------- |
| 表达式1？表达式2：表达式3 | 1值为真，返回表达式2的值；1值为假，返回表达式3的值； |



* empty 运算
  可以判断一个数据是否为空，如果为空，则输出true，反之false

  ~~~jsp
  <%@ page contentType="text/html;charset=UTF-8" language="java" %>
  <html>
  <head>
      <title>Title</title>
  </head>
  <body>
      <%
  //1、值为null值的时候，为空
          request.setAttribute("emptyNull", null);
  //2、值为空串的时候，为空
          request.setAttribute("emptyStr", "");
  //3、值是Object类型数组，长度为零的时候
          request.setAttribute("emptyArr", new Object[]{});
  //4、list集合，元素个数为零
          List<String> list = new ArrayList<>();
  //		list.add("abc");
          request.setAttribute("emptyList", list);
  //5、map集合，元素个数为零
          Map<String,Object> map = new HashMap<String, Object>();
  //		map.put("key1", "value1");
          request.setAttribute("emptyMap", map);
      %>
      ${ empty emptyNull } <br/>
      ${ empty emptyStr } <br/>
      ${ empty emptyArr } <br/>
      ${ empty emptyList } <br/>
      ${ empty emptyMap } <br/>
      <hr>
      
      ${ 12 != 12 ? "国哥帅呆":"国哥又骗人啦" }
  </body>
  </html>
  ~~~

* . 点运算符 和 [] 中括号运算符
  . 点运算可以输出 Bean 对象中某个属性的值
  [] 中括号运算可以输出map集合中key 里含有特殊字符的key的值

  ~~~jsp
  <%@ page contentType="text/html;charset=UTF-8" language="java" %>
  <html>
  <head>
      <title>Title</title>
  </head>
  <body>
      <%
          Map<String,Object> map = new HashMap<String, Object>();
          map.put("a.a.a", "aaaValue");
          map.put("b+b+b", "bbbValue");
          map.put("c-c-c", "cccValue");
          request.setAttribute("map", map);
      %>
          ${ map['a.a.a'] } <br>
          ${ map["b+b+b"] } <br>
          ${ map['c-c-c'] } <br>
  </body>
  </html>
  ~~~

  

 ### EL表达式的11个隐含对象

EL表达式中自己定义的，可以直接使用

| 变量        | 类型            | 作用                          |
| ----------- | --------------- | ----------------------------- |
| pageContext | PageContextImpl | 它可以获取jsp中的九大内置对象 |

~~~jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>Title</title>
</head>
<body>
    <%
        pageContext.setAttribute("req", request);
    %>
    <%=request.getScheme() %> <br>
    1.协议： ${ req.scheme }<br>
    2.服务器ip：${ pageContext.request.serverName }<br>
    3.服务器端口：${ pageContext.request.serverPort }<br>
    4.获取工程路径：${ pageContext.request.contextPath }<br>
    5.获取请求方法：${ pageContext.request.method }<br>
    6.获取客户端ip地址：${ pageContext.request.remoteHost }<br>
    7.获取会话的id编号：${ pageContext.session.id }<br>
</body>
</html>
~~~

> request.getScheme() 它可以获取请求的协议
> request.getServerName() 获取请求的服务器ip或域名
> request.getServerPort() 获取请求的服务器端口号
> getContextPath() 获取当前工程路径
> request.getMethod() 获取请求的方式（GET或POST）
> request.getRemoteHost()  获取客户端的ip 地址
> session.getId() 获取会话的唯一标识



| 变量             | 类型               | 作用                               |
| ---------------- | ------------------ | ---------------------------------- |
| pageScope        | Map<String,Object> | 它可以获取pageContext域中的数据    |
| requestScope     | Map<String,Object> | 它可以获取Request域中的数据        |
| sessionScope     | Map<String,Object> | 它可以获取Session域中的数据        |
| applicationScope | Map<String,Object> | 它可以获取ServletContext域中的数据 |

~~~jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>Title</title>
</head>
<body>
    <%
        pageContext.setAttribute("key2", "pageContext2");
        request.setAttribute("key2", "request");
        session.setAttribute("key2", "session");
        application.setAttribute("key2", "application");
    %>
    ${ applicationScope.key2 }
</body>
</html>
~~~



| 变量        | 类型                 | 作用                                         |
| ----------- | -------------------- | -------------------------------------------- |
| param       | Map<String,String>   | 它可以获取请求参数值                         |
| paramValues | Map<String,String[]> | 它可以获取请求的参数值，获取多个值的时候使用 |

~~~jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>Title</title>
</head>
<body>
    输出请求参数username的值：${ param.username } <br>
    输出请求参数password的值：${ param.password } <br>

    输出请求参数username的值：${ paramValues.username[0] } <br>
    输出请求参数hobby的值：${ paramValues.hobby[0] } <br>
    输出请求参数hobby的值：${ paramValues.hobby[1] } <br>
</body>
</html>
~~~



| 变量         | 类型                 | 作用                                           |
| ------------ | -------------------- | ---------------------------------------------- |
| header       | Map<String,String>   | 它可以获取请求头的信息                         |
| headerValues | Map<String,String[]> | 它可以获取请求头的信息，它可以获取多个值的情况 |

~~~jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>Title</title>
</head>
<body>
    输出请求头【User-Agent】的值：${ header['User-Agent'] } <br>
    输出请求头【Connection】的值：${ header.Connection } <br>
    输出请求头【User-Agent】的值：${ headerValues['User-Agent'][0] } <br>
</body>
</html>
~~~



| 变量   | 类型               | 作用                           |
| ------ | ------------------ | ------------------------------ |
| cookie | Map<String,Cookie> | 它可以获取当前请求的Cookie信息 |

~~~jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>Title</title>
</head>
<body>
    获取Cookie的名称：${ cookie.JSESSIONID.name } <br>
    获取Cookie的值：${ cookie.JSESSIONID.value } <br>
</body>
</html>
~~~



| 变量      | 类型               | 作用                                                  |
| --------- | ------------------ | ----------------------------------------------------- |
| initParam | Map<String,String> | 它可以获取在web.xml中配置的\<context-param>上下文参数 |

~~~jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>Title</title>
</head>
<body>
    输出&lt;Context-param&gt;username的值：${ initParam.username } <br>
    输出&lt;Context-param&gt;url的值：${ initParam.url } <br>
</body>
</html>
~~~

~~~xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
         version="4.0">
    <context-param>
        <param-name>username</param-name>
        <param-value>root</param-value>
    </context-param>

    <context-param>
        <param-name>url</param-name>
        <param-value>jdbc:mysql:///test</param-value>
    </context-param>
</web-app>
~~~

##  JSTL 标签库

标签库为了**替换代码脚本**，使得jsp更加简洁

* JSTL由五个不同功能的标签库组成

  | 功能范围         | url                                    | 前缀 |
  | ---------------- | -------------------------------------- | ---- |
  | 核心标签库--重点 | http://java.sun.com/jsp/jstl/core      | c    |
  | 格式化           | http://java.sun.com/jsp/jstl/fmt       | fmt  |
  | 函数             | http://java.sun.com/jsp/jstl/functions | fn   |

  数据库和XML不使用

* 在jsp标签库中使用 taglib 指令引入标签库

  ~~~jsp
  CORE 标签库
  <%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
  XML 标签库
  <%@ taglib prefix="x" uri="http://java.sun.com/jsp/jstl/xml" %>
  FMT 标签库
  <%@ taglib prefix="fmt" uri="http://java.sun.com/jsp/jstl/fmt" %>
  SQL 标签库
  <%@ taglib prefix="sql" uri="http://java.sun.com/jsp/jstl/sql" %>
  FUNCTIONS 标签库
  <%@ taglib prefix="fn" uri="http://java.sun.com/jsp/jstl/functions" %>
  ~~~

* 使用步骤（以 核心标签库 为例）

  1. 导入jstl标签库的jar包
     taglibs-standard-impl-1.2.1.jar 
     taglibs-standard-spec-1.2.1.jar
  2. 使用 taglib 指令引入标签库
     `<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>`
  3. 使用标签库

### core 核心库 的使用

* \<c:set/> 往域中保存数据

  > 域对象.setAttribute(key,value);
  >
  > scope 						  属性设置保存到哪个域
  >     	page						  表示PageContext域（默认值）
  >     	request					 表示Request域
  >     	session				     表示Session域
  >     	application			   表示ServletContext域
  > var								属性设置key是多少
  > value							属性设置值

  ~~~jsp
  <%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
  <%@ page contentType="text/html;charset=UTF-8" language="java" %>
  <html>
  <head>
      <title>Title</title>
  </head>
  <body>
      保存之前：${ sessionScope.abc } <br>
      <c:set scope="session" var="abc" value="abcValue"/>
      保存之后：${ sessionScope.abc } <br>
  </body>
  </html>
  ~~~

* <c:if >\</c:if> 做 if 判断

  > test 属性表示判断的条件（使用 EL 表达式输出）

  ~~~jsp
  <%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
  <%@ page contentType="text/html;charset=UTF-8" language="java" %>
  <html>
  <head>
      <title>Title</title>
  </head>
  <body>
      <c:if test="${ 12 == 12 }">
          <h1>12等于12</h1>
      </c:if>
      <c:if test="${ 12 != 12 }">
          <h1>12不等于12</h1>
      </c:if>
  </body>
  </html>
  ~~~

* \<c:choose>\<c:when>\<c:otherwise> 多路判断，类似于switch...case...default

  > choose标签							 开始选择判断
  >
  > when标签								表示每一种判断情况
  >     	test属性									表示当前这种判断情况的值
  > otherwise标签						表示剩下的情况
  >
  > 需要注意的点：
  >
  > 1. 标签里不能使用html注释，要使用jsp注释
  > 2. when标签的父标签一定要是choose标签

  ~~~jsp
  <%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
  <%@ page contentType="text/html;charset=UTF-8" language="java" %>
  <html>
  <head>
      <title>Title</title>
  </head>
  <body>
      <%
          request.setAttribute("height", 180);
      %>
      <c:choose>
          <%-- 这是html注释 --%>
          <c:when test="${ requestScope.height > 190 }">
              <h2>小巨人</h2>
          </c:when>
           <c:when test="${ requestScope.height > 180 }">
              <h2>很高</h2>
          </c:when>
          <c:when test="${ requestScope.height > 170 }">
              <h2>还可以</h2>
          </c:when>
          <c:otherwise>
              <c:choose>
                  <c:when test="${requestScope.height > 160}">
                      <h3>大于160</h3>
                  </c:when>
                  <c:when test="${requestScope.height > 150}">
                      <h3>大于150</h3>
                  </c:when>
                  <c:when test="${requestScope.height > 140}">
                      <h3>大于140</h3>
                  </c:when>
                  <c:otherwise>
                      其他小于140
                  </c:otherwise>
              </c:choose>
          </c:otherwise>
      </c:choose>
  </body>
  </html>
  ~~~

* <c:forEach />  遍历输出使用

  * 遍历数据类型

    > for (int i = 1; i < 10; i++)
    >
    > begin属性						设置开始的索引
    > end 属性						  设置结束的索引
    > var 属性						   表示循环的变量(也是当前正在遍历到的数据)

    ~~~jsp
    <%@ page import="java.util.Map" %>
    <%@ page import="java.util.HashMap" %>
    <%@ page import="java.util.List" %>
    <%@ page import="com.atguigu.pojo.Student" %>
    <%@ page import="java.util.ArrayList" %>
    <%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
    <%@ page contentType="text/html;charset=UTF-8" language="java" %>
    <html>
    <head>
        <title>Title</title>
        <style type="text/css">
            table{
                width: 500px;
                border: 1px solid red;
                border-collapse: collapse;
            }
            th , td{border: 1px solid red;}
        </style>
    </head>
    <body>
        <%--1.遍历1到10，输出
            
    	--%>
        <table border="1">
            <c:forEach begin="1" end="10" var="i">
                <tr>
                    <td>第${i}行</td>
                </tr>
            </c:forEach>
        </table>
    </body>
    </html>
    ~~~

  * 遍历 Object 数组

    > for (Object item: arr)
    >
    > items 					表示遍历的数据源（遍历的集合）
    > var 						表示当前遍历到的数据

    ~~~jsp
    <%@ page import="java.util.Map" %>
    <%@ page import="java.util.HashMap" %>
    <%@ page import="java.util.List" %>
    <%@ page import="com.atguigu.pojo.Student" %>
    <%@ page import="java.util.ArrayList" %>
    <%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
    <%@ page contentType="text/html;charset=UTF-8" language="java" %>
    <html>
    <head>
        <title>Title</title>
        <style type="text/css">
            table{
                width: 500px;
                border: 1px solid red;
                border-collapse: collapse;
            }
            th , td{border: 1px solid red;}
        </style>
    </head>
    <body>
        <%
        request.setAttribute("arr", new String[]{"18610541354","18688886666","18699998888"});
        %>
        <c:forEach items="${ requestScope.arr }" var="item">
            ${ item } <br>
        </c:forEach>
    </body>
    </html>
    
    ~~~

  * 遍历Map集合

    > for ( Map.Entry<String,Object> entry : map.entrySet()) {}

    ~~~jsp
    <%@ page import="java.util.Map" %>
    <%@ page import="java.util.HashMap" %>
    <%@ page import="java.util.List" %>
    <%@ page import="com.atguigu.pojo.Student" %>
    <%@ page import="java.util.ArrayList" %>
    <%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
    <%@ page contentType="text/html;charset=UTF-8" language="java" %>
    <html>
    <head>
        <title>Title</title>
        <style type="text/css">
            table{
                width: 500px;
                border: 1px solid red;
                border-collapse: collapse;
            }
            th , td{
                border: 1px solid red;
            }
        </style>
    </head>
    <body>
        <%
            Map<String,Object> map = new HashMap<String, Object>();
            map.put("key1", "value1");
            map.put("key2", "value2");
            map.put("key3", "value3");
            request.setAttribute("map", map);
        %>
        <c:forEach items="${ requestScope.map }" var="entry">
            <h1>${entry.key} = ${entry.value}</h1>
        </c:forEach>
    </body>
    </html>
    ~~~

  * 遍历List集合

    > items 								表示遍历的集合
    > var 									表示遍历到的数据
    > begin								 表示遍历的开始索引值
    > end 					   			表示结束的索引值
    > step 属性			  			表示遍历的步长值
    > varStatus 属性				 表示当前遍历到的数据的状态
    > for（int i = 1; i < 10; i+=2）

    ~~~jsp
    <%@ page import="java.util.Map" %>
    <%@ page import="java.util.HashMap" %>
    <%@ page import="java.util.List" %>
    <%@ page import="com.atguigu.pojo.Student" %>
    <%@ page import="java.util.ArrayList" %>
    <%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
    <%@ page contentType="text/html;charset=UTF-8" language="java" %>
    <html>
    <head>
        <title>Title</title>
        <style type="text/css">
            table{
                width: 500px;
                border: 1px solid red;
                border-collapse: collapse;
            }
            th , td{border: 1px solid red;}
        </style>
    </head>
    <body>
        <%
            List<Student> studentList = new ArrayList<Student>();
            for (int i = 1; i <= 10; i++) {
                studentList.add(new Student(i,"username"+i ,"pass"+i,18+i,"phone"+i));
            }
            request.setAttribute("stus", studentList);
        %>
        <table>
            <tr>
                <th>编号</th>
                <th>用户名</th>
                <th>密码</th>
                <th>年龄</th>
                <th>电话</th>
                <th>操作</th>
            </tr>
        <c:forEach begin="2" end="7" step="2" varStatus="status" items="${requestScope.stus}" var="stu">
            <tr>
                <td>${stu.id}</td>
                <td>${stu.username}</td>
                <td>${stu.password}</td>
                <td>${stu.age}</td>
                <td>${stu.phone}</td>
                <td>${status.step}</td>
            </tr>
        </c:forEach>
        </table>
    </body>
    </html>
    ~~~

    

## 文件的上传和下载

### 上传

1. 要有一个 form 标签，method=post 请求 

2. form 标签的 encType 属性值必须为 multipart/form-data 值 

   > encType=multipart/form-data 表示提交的数据，以多段（每一个表单项一个数据段）的形式进行拼 接，然后以二进制流的形式发送给服务器

3. 在 form 标签中使用 input type=file 添加上传的文件 

4. 编写服务器代码（Servlet 程序）接收，处理上传的数据。

* 文件上传时，HTTP协议的说明
  ![image-20210929180126650](E:\软工\typora笔记\javaweb\no4.jsp.assets\image-20210929180126650.png)

* commons-fileupload.jar 和 commons-io.jar 包中，常用的类、方法

  | 类名、方法                                                   | 说明                                   |
  | ------------------------------------------------------------ | -------------------------------------- |
  | ServletFileUpload类                                          | 用于解析上传的数据                     |
  | boolean ServletFileUpload.isMultipartContent(HttpServletRequest request) | 判断当前上传的数据格式是否是多段的格式 |
  | public List<FileItem> parseRequest(HttpServletRequest request) | 解析上传的数据                         |

  | 类名、方法                     | 说明                                                         |
  | ------------------------------ | ------------------------------------------------------------ |
  | FileItem类                     | 表示每一个表单项                                             |
  | boolean FileItem.isFormField() | 判断当前这个表单项是否是普通的表单项。true 表示普通的表单项，false表示上传的文件类型 |
  | String FileItem.getFieldName() | 获取表单项的name属性值                                       |
  | String FileItem.getString()    | 获取当前表单项的值                                           |
  | String FileItem.getName()      | 获取上传的文件名                                             |
  | void FileItem.write(file)      | 将上传的文件写到 参数file所指向硬盘位置                      |

  ~~~jsp
  <%@ page contentType="text/html;charset=UTF-8" language="java" %>
  <html>
  <head>
      <title>Title</title>
  </head>
  <body>
      <form action="http://192.168.31.74:8080/09_EL_JSTL/uploadServlet" method="post" enctype="multipart/form-data">
          用户名：<input type="text" name="username" /> <br>
          头像：<input type="file" name="photo" > <br>
          <input type="submit" value="上传">
      </form>
  </body>
  </html>
  ~~~

  ~~~java
  public class UploadServlet extends HttpServlet {
      /**
       * 用来处理上传的数据
       * @param req
       * @param resp
       * @throws ServletException
       * @throws IOException
       */
      @Override
      protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
          //1 先判断上传的数据是否多段数据（只有是多段的数据，才是文件上传的）
          if (ServletFileUpload.isMultipartContent(req)) {
  //           创建FileItemFactory工厂实现类
              FileItemFactory fileItemFactory = new DiskFileItemFactory();
              // 创建用于解析上传数据的工具类ServletFileUpload类
              ServletFileUpload servletFileUpload = new ServletFileUpload(fileItemFactory);
              try {
                  // 解析上传的数据，得到每一个表单项FileItem
                  List<FileItem> list = servletFileUpload.parseRequest(req);
                  // 循环判断，每一个表单项，是普通类型，还是上传的文件
                  for (FileItem fileItem : list) {
                      if (fileItem.isFormField()) {
                          // 普通表单项
                          System.out.println("表单项的name属性值：" + fileItem.getFieldName());
                          // 参数UTF-8.解决乱码问题
                          System.out.println("表单项的value属性值：" + fileItem.getString("UTF-8"));
                      } else {
                          // 上传的文件
                          System.out.println("表单项的name属性值：" + fileItem.getFieldName());
                          System.out.println("上传的文件名：" + fileItem.getName());
                          // 上传的文件下载到服务器
                          fileItem.write(new File("e:\\" + fileItem.getName()));
                      }
                  }
              } catch (Exception e) {
                  e.printStackTrace();
              }
          }
      }
  }
  ~~~

### 下载

> resp.setHeader("Content-Disposition", "attachment; filename=" + URLEncoder.encode("中国.jpg", "UTF-8"));
> Content-Disposition响应头						   表示收到的数据怎么处理
> attachment													  表示附件，表示下载使用
> filename= 														表示指定下载的文件名
> url编码															  把汉字转换成为%xx%xx的格式
>
> resp.setHeader("Content-Disposition", "attachment; filename==?charset?B?xxxxx?="
> xxxxxxxx														 Base64编码
> charset															原文的字符集

~~~java
public class Download extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
//1、获取要下载的文件名
        String downloadFileName = "2.jpg";
        
//2、读取要下载的文件内容 (通过ServletContext对象可以读取)
        ServletContext servletContext = getServletContext();
        // 获取要下载的文件类型
        String mimeType = servletContext.getMimeType("/file/" + downloadFileName);
        System.out.println("下载的文件类型：" + mimeType);
        
//4、在回传前，通过响应头告诉客户端返回的数据类型
        resp.setContentType(mimeType);
        
//5、还要告诉客户端收到的数据是用于下载使用（还是使用响应头）
        if (req.getHeader("User-Agent").contains("Firefox")) {
            // 如果是火狐浏览器使用Base64编码
            resp.setHeader("Content-Disposition", "attachment; filename==?UTF-8?B?" + new BASE64Encoder().encode("中国.jpg".getBytes("UTF-8")) + "?=");
        } else {
            // 如果不是火狐，是IE或谷歌，使用URL编码操作
            resp.setHeader("Content-Disposition", "attachment; filename=" + URLEncoder.encode("中国.jpg", "UTF-8"));
        }
        InputStream resourceAsStream = servletContext.getResourceAsStream("/file/" + downloadFileName);
        // 获取响应的输出流
        OutputStream outputStream = resp.getOutputStream();
        
//3、把下载的文件内容回传给客户端
        // 读取输入流中全部的数据，复制给输出流，输出给客户端
        IOUtils.copy(resourceAsStream, outputStream);
    }
}
~~~

> 火狐浏览器中文下载编码：Base64
> 其他浏览器中文下载编码：URL

~~~java
public class Base64Test {
    public static void main(String[] args) throws Exception {
        String content = "这是需要Base64编码的内容";
        // 创建一个Base64编码器
        BASE64Encoder base64Encoder = new BASE64Encoder();
        // 执行Base64编码操作
        String encodedString = base64Encoder.encode(content.getBytes("UTF-8"));

        System.out.println( encodedString );
        // 创建Base64解码器
        BASE64Decoder base64Decoder = new BASE64Decoder();
        // 解码操作
        byte[] bytes = base64Decoder.decodeBuffer(encodedString);

        String str = new String(bytes, "UTF-8");

        System.out.println(str);
    }
}
~~~

