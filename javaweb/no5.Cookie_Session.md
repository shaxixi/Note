[toc]

# Javaweb——Cookie And Session

## Cookie

* 概念
  Cookie是服务器通知客户端保存键值对的一种技术，客户端有了Cookie后，每次请求都发送给服务器。每个Cookie的大小不能超过4kb

### Cookie的创建

![image-20211012172522309](E:\软工\typora笔记\javaweb\no5.Cookie_Session.assets\image-20211012172522309.png)

~~~java
public class CookieServlet extends BaseServlet {
	protected void createCookie(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        //1 创建Cookie对象
        Cookie cookie = new Cookie("key4", "value4");
        //2 通知客户端保存Cookie
        resp.addCookie(cookie);
        
        //1 创建Cookie对象
        Cookie cookie1 = new Cookie("key5", "value5");
        //2 通知客户端保存Cookie
        resp.addCookie(cookie1);

        resp.getWriter().write("Cookie创建成功");
    }
}
~~~

### Cookie的获取

req.getCookies():Cookie[]

![image-20211012172603137](E:\软工\typora笔记\javaweb\no5.Cookie_Session.assets\image-20211012172603137.png)

~~~java
public class CookieUtils {
    /**
     * 查找指定名称的Cookie对象
     * @param name
     * @param cookies
     * @return
     */
    public static Cookie findCookie(String name , Cookie[] cookies){
        if (name == null || cookies == null || cookies.length == 0) {
            return null;
        }
        for (Cookie cookie : cookies) {
            if (name.equals(cookie.getName())) {
                return cookie;
            }
        }
        return null;
    }
}
~~~

~~~java
public class CookieServlet extends BaseServlet {
	protected void getCookie(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        
        Cookie[] cookies = req.getCookies();
        for (Cookie cookie : cookies) {
            // getName方法返回Cookie的key（名）
            // getValue方法返回Cookie的value值
            resp.getWriter().write("Cookie[" + cookie.getName() + "=" + cookie.getValue() + "] <br/>");
        }

        Cookie iWantCookie = CookieUtils.findCookie("key1", cookies);
        // 如果不等于null，说明赋过值，也就是找到了需要的Cookie
        if (iWantCookie != null) {
            resp.getWriter().write("找到了需要的Cookie");
        }
    }
}
~~~



### Cookie值的修改

* 方式一：
  1、先创建一个要修改的同名（指的就是 key）的 Cookie 对象
  2、在构造器，同时赋于新的 Cookie 值。 
  3、调用 response.addCookie( Cookie )

  ~~~java
  public class CookieServlet extends BaseServlet { 
  	protected void updateCookie(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
         Cookie cookie = new Cookie("key1","newValue1");
         resp.addCookie(cookie);
         resp.getWriter().write("key1的Cookie已经修改好");
      }
  }
  ~~~

* 方式二：

  1、先查找到需要修改的 Cookie 对象 
  2、调用 setValue()方法赋于新的 Cookie 值。 
  3、调用 response.addCookie()通知客户端保存修改

  ~~~java
  public class CookieServlet extends BaseServlet { 
      protected void updateCookie(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
          Cookie cookie = CookieUtils.findCookie("key2", req.getCookies());
          if (cookie != null) {
              cookie.setValue("newValue2");
              resp.addCookie(cookie);
          }
          resp.getWriter().write("key1的Cookie已经修改好");
      }
  }
  ~~~

### Cookie生命控制

> setMaxAge() 
>
> ​			正数，表示在指定的秒数后过期 
> ​			负数，表示浏览器一关，Cookie 就会被删除（默认值是-1） 
> ​			零，表示马上删除 Cookie

* 默认的会话级别的Cookie

  ~~~java
  public class CookieServlet extends BaseServlet { 
      protected void defaultLife(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
          Cookie cookie = new Cookie("defalutLife","defaultLife");
          cookie.setMaxAge(-1);//设置存活时间
          resp.addCookie(cookie);
      }
  }
  ~~~

* 马上删除一个Cookie

  ~~~java
  public class CookieServlet extends BaseServlet { 
      protected void deleteNow(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
          // 先找到你要删除的Cookie对象
          Cookie cookie = CookieUtils.findCookie("key4", req.getCookies());
          if (cookie != null) {
              cookie.setMaxAge(0); // 表示马上删除，都不需要等待浏览器关闭
              resp.addCookie(cookie);
              resp.getWriter().write("key4的Cookie已经被删除");
          }
      }
  }
  ~~~

* 设置存活1个小时的Cooie

  ~~~java
  public class CookieServlet extends BaseServlet { 
      protected void life3600(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
          Cookie cookie = new Cookie("life3600", "life3600");
          cookie.setMaxAge(60 * 60); // 设置Cookie一小时之后被删除。无效
          resp.addCookie(cookie);
          resp.getWriter().write("已经创建了一个存活一小时的Cookie");
      }
  }
  ~~~

### Cookie有效路径Path

Cookie 的 path 属性可以有效的过滤哪些 Cookie 可以发送给服务器。哪些不发。 
path 属性是通过请求的地址来进行有效的过滤。

> CookieA path=/工程路径 
> CookieB path=/工程路径/abc 
>
> 请求地址如下： 
> 				http://ip:port/工程路径/a.html 					CookieA 发送 		CookieB 不发送
> 				http://ip:port/工程路径/abc/a.html 			CookieA 发送 		CookieB 发送

~~~java
public class CookieServlet extends BaseServlet {
    protected void testPath(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        Cookie cookie = new Cookie("path1", "path1");
        // getContextPath() ===>>>>  得到工程路径
        cookie.setPath( req.getContextPath() + "/abc" ); // ===>>>>  /工程路径/abc
        resp.addCookie(cookie);
        resp.getWriter().write("创建了一个带有Path路径的Cookie");
    }
}
~~~



## Session会话

Session 就一个接口（HttpSession） 
Session 就是会话。它是用来维护一个客户端和服务器之间关联的一种技术。 
每个客户端都有自己的一个 Session 会话。 
Session 会话中，我们经常用来保存用户登录之后的信息。

### 创建Session和获取

> request.getSession() 
> 			第一次调用是：创建 Session 会话 
> 			之后调用都是：获取前面创建好的 Session 会话对象。 
>
> isNew()
> 			判断到底是不是刚创建出来的（新的） true 表示刚创建 	false 表示获取之前创建 
> 			每个会话都有一个身份证号。也就是 ID 值。而且这个 ID 是唯一的。 
>
> getId() 
> 			得到 Session 的会话 id 值。

* 创建

  ~~~java
  public class SessionServlet extends BaseServlet {
      protected void createOrGetSession(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
          // 创建和获取Session会话对象
          HttpSession session = req.getSession();
          // 判断 当前Session会话，是否是新创建出来的
          boolean isNew = session.isNew();
          // 获取Session会话的唯一标识 id
          String id = session.getId();
  
          resp.getWriter().write("得到的Session，它的id是：" + id + " <br /> ");
          resp.getWriter().write("这个Session是否是新创建的：" + isNew + " <br /> ");
      }
  }
  ~~~

* 获取

  ~~~java
  public class SessionServlet extends BaseServlet {
      protected void getAttribute(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
          Object attribute = req.getSession().getAttribute("key1");
          resp.getWriter().write("从Session中获取出key1的数据是：" + attribute);
      }
  }
  ~~~



### Session生命周期

> public void setMaxInactiveInterval(int interval) 
> 设置 Session 的超时时间（以秒为单位），超过指定的时长，Session 就会被销毁。			正数		设定 Session 的超时时长。 
> 			负数		表示永不超时（极少使用） 
>
> public int getMaxInactiveInterval()
> 获取 Session 的超时时间 
>
> public void invalidate() 
> 让当前Session 会话马上超时无效。

* Session 默认的超时时间长为 30 分钟。 
  因为在 Tomcat 服务器的配置文件 web.xml中默认有配置
  它就表示配置了当前 Tomcat 服务器下所有的 Session 超时配置默认时长为：30 分钟。 

  ~~~java
  public class SessionServlet extends BaseServlet {
      protected void defaultLife(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
          // 获取了Session的默认超时时长
          int maxInactiveInterval = req.getSession().getMaxInactiveInterval();
  
          resp.getWriter().write("Session的默认超时时长为：" + maxInactiveInterval + " 秒 ");
  
      }
  }
  ~~~

* 设置Session的默认超时时间长

  ~~~xml
  web.xml
  <session-config>
  	<session-timeout>20</session-timeout>
  </session-config>
  ~~~

* 设置个别Session的超时时间长
  session.setMaxInactiveInterval(int interval)          单独设置超时时长。

  ~~~java
  public class SessionServlet extends BaseServlet {
      protected void life3(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
          // 先获取Session对象
          HttpSession session = req.getSession();
          // 设置当前Session3秒后超时
          session.setMaxInactiveInterval(3);
  
          resp.getWriter().write("当前Session已经设置为3秒后超时");
      }
  }
  ~~~

![image-20211012180442727](E:\软工\typora笔记\javaweb\no5.Cookie_Session.assets\image-20211012180442727.png)

* Session会话的销毁

  ~~~java
  public class SessionServlet extends BaseServlet {
      protected void deleteNow(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
          // 先获取Session对象
          HttpSession session = req.getSession();
          // 让Session会话马上超时
          session.invalidate();
  
          resp.getWriter().write("Session已经设置为超时（无效）");
      }
  }
  ~~~

* Session 和 Cookie 之间的关联
  ![image-20211012213649499](E:\软工\typora笔记\javaweb\no5.Cookie_Session.assets\image-20211012213649499.png)

## 验证码

表单重复提交——解决之道

* 提交完表单。服务器使用请求转来进行页面跳转。这个时候，**用户按下功能键 F5**，就会发起最后一次的请求。 造成表单重复提交问题。
  解决方法：**使用重定向来进行跳转**
* 用户正常提交服务器，但是由于网络延迟等原因，迟迟未收到服务器的响应，这个时候，用户以为提交失败， 就会着急，然后**多点了几次提交操作**，也会造成表单重复提交。
* 用户正常提交服务器。服务器也没有延迟，但是**提交完成后，用户回退浏览器**。重新提交。也会造成表单重复提交

![image-20211012224142214](E:\软工\typora笔记\javaweb\no5.Cookie_Session.assets\image-20211012224142214.png)

### 验证码的使用

步骤：

1. 导入谷歌验证码的 jar 包 				kaptcha-2.3.2.jar

2. 在 web.xml 中去配置用于生成验证码的 Servlet 程序

   ~~~xml
       <servlet>
           <servlet-name>KaptchaServlet</servlet-name>
           <servlet-class>com.google.code.kaptcha.servlet.KaptchaServlet</servlet-class>
       </servlet>
       <servlet-mapping>
           <servlet-name>KaptchaServlet</servlet-name>
           <url-pattern>/kaptcha.jpg</url-pattern>
       </servlet-mapping>
   ~~~

3. 在表单中使用 img 标签去显示验证码图片并使用

   ~~~jsp
   <%@ page contentType="text/html;charset=UTF-8" language="java" %>
   <html>
     <head>
       <title>$Title$</title>
     </head>
     <body>
     <form action="http://localhost:8080/tmp/registServlet" method="get">
       用户名：<input type="text" name="username" > <br>
       验证码：<input type="text" style="width: 60px;" name="code">
         
       <img src="http://localhost:8080/tmp/kaptcha.jpg" alt="" style="width: 100px; height: 28px;"> <br> 
         
       <input type="submit" value="登录">
     </form>
     </body>
   </html>
   ~~~

4. 在服务器获取谷歌生成的验证码和客户端发送过来的验证码比较使用。

   ~~~java
   public class RegistServlet extends HttpServlet{
       @Override
       protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
           // 获取Session中的验证码 KAPTCHA_SESSION_KEY
           String token = (String) req.getSession().getAttribute(KAPTCHA_SESSION_KEY);
           // 删除 Session中的验证码
           req.getSession().removeAttribute(KAPTCHA_SESSION_KEY);
           String code = req.getParameter("code");
           
           // 获取用户名
           String username = req.getParameter("username");
           if (token != null && token.equalsIgnoreCase(code)) {
               System.out.println("保存到数据库：" + username);
               resp.sendRedirect(req.getContextPath() + "/ok.jsp");
           } else {
               System.out.println("请不要重复提交表单");
           }
       }
   }
   ~~~

5. 刷新页面，切换验证码。
   因为加载页面时，会把链接内容加载到缓冲中。刷新相同的页面，缓冲会加载到浏览器上，更新不了页面。

   > 在事件响应的 function 函数中有一个 this 对象。这个 this 对象，是当前正在响应事件的 dom 对象
   > src 属性表示验证码 img 标签的 图片路径。它可读，可写

   ~~~javascript
   $("#code_img").click(function () {
   	alert(this.src);
   	this.src = "${basePath}kaptcha.jpg?d=" + new Date();
       //?d=" + new Date();	使得每一次的地址都不一样
   })；
   ~~~

   

## Filter 过滤器

JavaWeb 的三大组件之一。
它是 JavaEE 的规范。也就是接口 
作用是：**拦截请求，过滤响应。**

* 如果用Session判断，则多页面来回跳转，也需要接收多处地址。

  ~~~jsp
  <%
  Object user = session.getAttribute("user");
  // 如果等于 null，说明还没有登录
  if (user == null) {
  request.getRequestDispatcher("/login.jsp").forward(request,response);
  return;
  }
  %>
  ~~~

![image-20211013083059758](E:\软工\typora笔记\javaweb\no5.Cookie_Session.assets\image-20211013083059758.png)

* 使用步骤
  1、编写一个类去实现 Filter 接口 
  2、实现过滤方法 doFilter() 

  ~~~java
  public class AdminFilter implements Filter {
  /**
  * doFilter 方法，专门用于拦截请求。可以做权限检查
  */
      @Override
      public void doFilter(ServletRequest servletRequest, ServletResponse servletResponse, FilterChain
      filterChain) throws IOException, ServletException {
          HttpServletRequest httpServletRequest = (HttpServletRequest) servletRequest;
          HttpSession session = httpServletRequest.getSession();
          
          Object user = session.getAttribute("user");
          // 如果等于 null，说明还没有登录
          if (user == null) {           				servletRequest.getRequestDispatcher("/login.jsp").forward(servletRequest,servletResponse);
          return;
          } else {
              // 让程序继续往下访问用户的目标资源
              filterChain.doFilter(servletRequest,servletResponse);
          }
      }
  }
  ~~~

  3、到 web.xml 中去配置 Filter 的拦截路径

  ~~~xml
  <filter>
      <!--给 filter 起一个别名-->
      <filter-name>AdminFilter</filter-name>
      <!--配置 filter 的全类名-->
      <filter-class>com.atguigu.filter.AdminFilter</filter-class>
  </filter>
  <!--filter-mapping 配置 Filter 过滤器的拦截路径-->
  <filter-mapping>
      <!--filter-name 表示当前的拦截路径给哪个 filter 使用-->
      <filter-name>AdminFilter</filter-name>
      <!--url-pattern 配置拦截路径
      / 表示请求地址为：http://ip:port/工程路径/ 映射到 IDEA 的 web 目录
      /admin/* 表示请求地址为：http://ip:port/工程路径/admin/*
      -->
      <url-pattern>/admin/*</url-pattern>
  </filter-mapping>
  ~~~

### 生命周期

> Filter 的生命周期包含几个方法 
> 1、构造器方法 
> 2、init 初始化方法 
> 			第 1，2 步，在 web 工程启动的时候执行（Filter 已经创建） 
>
> 3、doFilter 过滤方法 
> 			第 3 步，每次拦截到请求，就会执行 
>
> 4、destroy 销毁 
> 			第 4 步，停止 web 工程的时候，就会执行（停止 web 工程，也会销毁 Filter 过滤器）

### FilterConfig类

它是 Filter 过滤器的配置文件类。 
Tomcat 每次创建 Filter 的时候，也会同时创建一个 FilterConfig 类，这里包含了 Filter 配置文件的配置信息。 

* 作用是获取 filter 过滤器的配置内容 

  1. 获取 Filter 的名称 filter-name 的内容 
  2. 获取在 Filter 中配置的 init-param 初始化参数 
  3. 获取 ServletContext 对象

  ~~~java
  @Override
  public void init(FilterConfig filterConfig) throws ServletException {
      // 1、获取 Filter 的名称 filter-name 的内容
      System.out.println("filter-name 的值是：" + filterConfig.getFilterName());
      // 2、获取在 web.xml 中配置的 init-param 初始化参数
      System.out.println("初始化参数 username 的值是：" + filterConfig.getInitParameter("username"));
      System.out.println("初始化参数 url 的值是：" + filterConfig.getInitParameter("url"));
      // 3、获取 ServletContext 对象
      System.out.println(filterConfig.getServletContext());
  }
  ~~~

  ~~~xml
  <filter>
      <!--给 filter 起一个别名-->
      <filter-name>AdminFilter</filter-name>
      <!--配置 filter 的全类名-->
      <filter-class>com.atguigu.filter.AdminFilter</filter-class>
      <init-param>
          <param-name>username</param-name>
          <param-value>root</param-value>
      </init-param>
      <init-param>
          <param-name>url</param-name>
          <param-value>jdbc:mysql://localhost3306/test</param-value>
      </init-param>
  </filter>
  ~~~

### FilterChain 过滤器链

![image-20211013085735423](E:\软工\typora笔记\javaweb\no5.Cookie_Session.assets\image-20211013085735423.png)

### Filter 拦截路径

* 精确匹配

  \<url-pattern>/target.jsp\</url-pattern>
  以上配置的路径，表示请求地址必须为：http://ip:port/工程路径/target.jsp

* 目录匹配

  \<url-pattern>/admin/*\</url-pattern>

  以上配置的路径，表示请求地址必须为：http://ip:port/工程路径/admin/*

* 后缀名匹配
  \<url-pattern>*.html\</url-pattern>
  以上配置的路径，表示请求地址必须以.html 结尾才会拦截到

  \<url-pattern>*.do\</url-pattern>
  以上配置的路径，表示请求地址必须以.do 结尾才会拦截

  注：Filter 过滤器它只关心请求的地址是否匹配，不关心请求的资源是否存在！！！

## Json

json 是一种轻量级的数据交换格式。
数据交换指的是客户端和服务器之间业务数据的传递格式。

### Json在JavaScript中的使用

* 定义
  json 是由**键值对**组成，并且由**花括号**（大括号）包围。每个键由**引号**引起来，键和值之间使用**冒号**进行分隔， 多组键值对之间进行**逗号**进行分隔。

  ~~~html
  <script type="text/javascript">
  			var jsonObj = {
  				"key1":12,
  				"key2":"abc",
  				"key3":true,
  				"key4":[11,"arr",false],
  				"key5":{
  					"key5_1" : 551,
  					"key5_2" : "key5_2_value"
  				},
  				"key6":[{
  					"key6_1_1":6611,
  					"key6_1_2":"key6_1_2_value"
  				},{
  					"key6_2_1":6621,
  					"key6_2_2":"key6_2_2_value"
  				}]
  			};
  </script>
  ~~~

* 访问
  json 本身是一个对象。 
  json 中的 key 我们可以理解为是对象中的一个属性。 
  json 中的 key 访问就跟访问对象的属性一样： json 对象.key

  ~~~html
  <script type="text/javascript"> //接上面代码
  			alert(jsonObj);
  			alert(typeof(jsonObj));// object  json就是一个对象
  			alert(jsonObj.key1); //12
  			alert(jsonObj.key2); // abc
  			alert(jsonObj.key3); // true
  			alert(jsonObj.key4);// 得到数组[11,"arr",false]
  			 // json 中 数组值的遍历
  			for(var i = 0; i < jsonObj.key4.length; i++) {
  				alert(jsonObj.key4[i]);
  			}
  			alert(jsonObj.key5.key5_1);//551
  			alert(jsonObj.key5.key5_2);//key5_2_value
  			alert( jsonObj.key6 );// 得到json数组
  
  			// 取出来每一个元素都是json对象
  			var jsonItem = jsonObj.key6[0];
  			alert( jsonItem.key6_1_1 ); //6611
  			alert( jsonItem.key6_1_2 ); //key6_1_2_value
  </script>
  ~~~

* 常用方法         json 字符串 和 json 对象

  JSON.stringify()                       把 json 对象转换成为 json 字符串 
  JSON.parse()                           把 json 字符串转换成为 json 对象

  一般我们要操作 json 中的数据的时候，需要 json 对象的格式。 
  一般我们要在客户端和服务器之间进行数据交换的时候，使用 json 字符串。

  ~~~html
  <script type="text/javascript"> //接上面代码
  			// 把json对象转换成为 json字符串
  			var jsonObjString = JSON.stringify(jsonObj); // 特别像 Java中对象的toString
  			alert(jsonObjString)
  			// 把json字符串。转换成为json对象
  			var jsonObj2 = JSON.parse(jsonObjString);
  			alert(jsonObj2.key1);// 12
  			alert(jsonObj2.key2);// abc
  </script>
  ~~~

### Json在Java中的使用

导包 gson-2.2.4.jar

> toJson方法			可以把java对象转换成为json字符串
>
> fromJson				把json字符串转换回Java对象
>         	 					第一个参数是json字符串
>         						第二个参数是转换回去的Java对象类型

* JavaBean 和 json 的互转

  ~~~java
  public class JsonTest {
      @Test
      public void test1(){
          Person person = new Person(1,"国哥好帅!");
          // 创建Gson对象实例
          Gson gson = new Gson();
  
          String personJsonString = gson.toJson(person);
          System.out.println(personJsonString);
  
          Person person1 = gson.fromJson(personJsonString, Person.class);
          System.out.println(person1);
      }
  }
  ~~~

* List 和 json 的互转

  ~~~java
  public class JsonTest {
  	@Test
      public void test2() {
          List<Person> personList = new ArrayList<>();
          personList.add(new Person(1, "国哥"));
          personList.add(new Person(2, "康师傅"));
          Gson gson = new Gson();
          // 把 List 转换为 json 字符串
          String personListJsonString = gson.toJson(personList);
          System.out.println(personListJsonString);
          // 把 json字符串 转换为 List  
          List<Person> list = gson.fromJson(personListJsonString, TypeToken<ArrayList<Person>>(){}.getType()));
          System.out.println(list);
          Person person = list.get(0);
          System.out.println(person);
      }
  }
  ~~~

* map 和 json 的互转

  ~~~java
  public class JsonTest {
      @Test
      public void test3(){
          Map<Integer,Person> personMap = new HashMap<>();
          personMap.put(1, new Person(1, "国哥好帅"));
          personMap.put(2, new Person(2, "康师傅也好帅"));
          Gson gson = new Gson();
          // 把 map 集合转换成为 json 字符串
          String personMapJsonString = gson.toJson(personMap);
          System.out.println(personMapJsonString);
          // Map<Integer,Person> personMap2 = gson.fromJson(personMapJsonString, new PersonMapType().getType());
          Map<Integer,Person> personMap2 = gson.fromJson(personMapJsonString, new
          TypeToken<HashMap<Integer,Person>>(){}.getType());
          System.out.println(personMap2);
          Person p = personMap2.get(1);
          System.out.println(p);
      }
  }
  ~~~

## AJAX请求

一种创建交互式网页应用的网页开发 技术。

ajax 是一种浏览器通过 js 异步发起请求，局部更新页面的技术。 
Ajax 请求的局部更新，浏览器地址栏不会发生变化 局部更新不会舍弃原来页面的内容

#### JavaScript中AJAX

~~~html
<script type="text/javascript">
    // 在这里使用javaScript语言发起Ajax请求，访问服务器AjaxServlet中javaScriptAjax
    function ajaxRequest() {
// 				1、我们首先要创建XMLHttpRequest 
        var xmlhttprequest = new XMLHttpRequest();
// 				2、调用open方法设置请求参数
        xmlhttprequest.open("GET","http://localhost:8080/16_json_ajax_i18n/ajaxServlet?action=javaScriptAjax",true);
// 				4、在send方法前绑定onreadystatechange事件，处理请求完成后的操作。
        xmlhttprequest.onreadystatechange = function(){
            if (xmlhttprequest.readyState == 4 && xmlhttprequest.status == 200) {
                alert("收到服务器返回的数据：" + xmlhttprequest.responseText);
                var jsonObj = JSON.parse(xmlhttprequest.responseText);
                // 把响应的数据显示在页面上
                document.getElementById("div01").innerHTML = "编号：" + jsonObj.id + " , 姓名：" + jsonObj.name;
            }
        }
// 				3、调用send方法发送请求
        xmlhttprequest.send();
    }
</script>
<body>
   	<button onclick="ajaxRequest()">ajax request</button>
	<a href="http://localhost:8080/16_json_ajax_i18n/ajaxServlet?action=javaScriptAjax">非Ajax</a>
</body>
~~~

~~~java
public class AjaxServlet extends BaseServlet {

    protected void javaScriptAjax(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        System.out.println("Ajax请求过来了");
        Person person = new Person(1, "国哥");
        try {
            Thread.sleep(3000);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        // json格式的字符串
        Gson gson = new Gson();
        String personJsonString = gson.toJson(person);

        resp.getWriter().write(personJsonString);
    }
}
~~~



#### jQuery中的AJAX请求

> $.ajax 方法 
> url 			  表示请求的地址 
> type 		   表示请求的类型 GET 或 POST 请求 
> data 		   表示发送给服务器的数据 
> 					格式有两种： 一：name=value&name=value 
> 											二：{key:value} 
> success 	   请求成功，响应的回调函数 
> dataType 	响应的数据类型 
> 					  常用的数据类型有：
> 								text 表示纯文本 xml 表示 xml 数据 json 表示 json 对象

~~~html
<script type="text/javascript" src="script/jquery-1.7.2.js"></script>
<script type="text/javascript">
    $(function(){
        $("#ajaxBtn").click(function(){
            $.ajax({
                url:"http://localhost:8080/16_json_ajax_i18n/ajaxServlet",
                /data:"action=jQueryAjax",
                data:{action:"jQueryAjax"},
                type:"GET",
                success:function (data) {
                    alert("服务器返回的数据是：" + data);
                    //如果dataType的值是text,则需类型转换 var jsonObj = JSON.parse(data);
                    $("#msg").html(" ajax 编号：" + data.id + " , 姓名：" + data.name);
                },
                dataType : "json"
            });
        });
    });
</script>
~~~

~~~java
public class AjaxServlet extends BaseServlet {
    protected void jQueryAjax(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        System.out.println("  jQueryAjax == 方法调用了");
        Person person = new Person(1, "国哥");
        // json格式的字符串
        Gson gson = new Gson();
        String personJsonString = gson.toJson(person);

        resp.getWriter().write(personJsonString);
    }
}
~~~



#### jQuery中的GET、POST请求

> $.get 方法和$.post 方法 
> url 			   请求的 url 地址 
> data 			发送的数据 
> callback 	  成功的回调函数 
> type 			返回的数据类型

~~~html
<script type="text/javascript">
    $(function(){
        $("#getBtn").click(function(){
           // ajax--get请求
           $.get("http://localhost:8080/16_json_ajax_i18n/ajaxServlet","action=jQueryGet",function (data) {
                $("#msg").html(" get 编号：" + data.id + " , 姓名：" + data.name);
            },"json");
        });
</script>
~~~

~~~java
public class AjaxServlet extends BaseServlet {
    protected void jQueryGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        System.out.println("  jQueryGet  == 方法调用了");
        Person person = new Person(1, "国哥");
        // json格式的字符串
        Gson gson = new Gson();
        String personJsonString = gson.toJson(person);

        resp.getWriter().write(personJsonString);
    }
}
~~~



~~~html
<script type="text/javascript">
    $(function(){
        $("#postBtn").click(function(){
            // ajax--post请求
            $.post("http://localhost:8080/16_json_ajax_i18n/ajaxServlet","action=jQueryPost",function (data) {
                $("#msg").html(" post 编号：" + data.id + " , 姓名：" + data.name);
            },"json");
        });
    });
</script>
~~~

~~~java
public class AjaxServlet extends BaseServlet {
    protected void jQueryPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        System.out.println("  jQueryPost   == 方法调用了");
        Person person = new Person(1, "国哥");
        // json格式的字符串
        Gson gson = new Gson();
        String personJsonString = gson.toJson(person);

        resp.getWriter().write(personJsonString);
    }
}
~~~



#### jQuery中的GETJSON请求

> $.getJSON 方法 
> url 				 请求的 url 地址 
> data 			  发送给服务器的数据 
> callback 	    成功的回调函数

~~~html
<script type="text/javascript">
    $(function(){
        // ajax--getJson请求
        $("#getJSONBtn").click(function(){
            $.getJSON("http://localhost:8080/16_json_ajax_i18n/ajaxServlet","action=jQueryGetJSON",function (data) {
                $("#msg").html(" getJSON 编号：" + data.id + " , 姓名：" + data.name);
            });
        });
    });
</script>
~~~

~~~java
public class AjaxServlet extends BaseServlet {
    protected void jQueryGetJSON(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        System.out.println("  jQueryGetJSON   == 方法调用了");
        Person person = new Person(1, "国哥");
        // json格式的字符串
        Gson gson = new Gson();
        String personJsonString = gson.toJson(person);

        resp.getWriter().write(personJsonString);
    }
}
~~~



> 表单序列化 serialize() 
> serialize()可以把表单中所有表单项的内容都获取到，并以name=value&name=value  的形式进行拼接。

~~~html
<html>
	<head> 
       <script type="text/javascript">
            $(function(){
                $("#submit").click(function(){
                    // 把参数序列化
                    $.getJSON("http://localhost:8080/16_json_ajax_i18n/ajaxServlet","action=jQuerySerialize&" + $("#form01").serialize(),function (data) {
                        $("#msg").html(" Serialize 编号：" + data.id + " , 姓名：" + data.name);
                    });
                });
            });
        </script>
	</head>
	<body>
        <div id="msg">

		</div>
		<form id="form01" >
			用户名：<input name="username" type="text" /><br/>
			密码：<input name="password" type="password" /><br/>
		</form>
	</body>
</html>
~~~

~~~java
public class AjaxServlet extends BaseServlet {
    protected void jQuerySerialize(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        System.out.println("  jQuerySerialize   == 方法调用了");

        System.out.println("用户名：" + req.getParameter("username"));
        System.out.println("密码：" + req.getParameter("password"));

        Person person = new Person(1, "国哥");
        // json格式的字符串
        Gson gson = new Gson();
        String personJsonString = gson.toJson(person);

        resp.getWriter().write(personJsonString);
    }
}
~~~

