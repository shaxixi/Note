[toc]

# javaweb

## XML

可扩展的标记性语言

* 主要作用：
  1. 用来保存数据，而且这些数据具有自我描述性
  2. 可以作为项目或者模块的配置文件
  3. 可以作为网络传输数据的格式（现在以json为主）

### 文档声明

~~~xml
<?xml version="1.0" encoding="UTF-8"?> 
<!-- 这是注释  -->
~~~

> xml 声明 
> version 版本号 
> encoding xml的文件编码
> standalone="yes/no" xml文件是否是独立的xml文件
> `<?xml` 要连在一起写，否则会报错,不能有空格

* 浏览器中可以查看到文档
* xml注释： `<!-- html注释 -->`

### 元素（标签）

Element 元素	元素是指从开始标签到结束标签的内容，如：`<title>java 编程思想</title>`

* 命名规则
  \> 名称不能以数字或者标点符号开头

  ```xml
  <1book id="SN12341235123">  </1book> <!-- 错误 -->
  ```

  \> 名称不能包含空格

  ```xml
  <bo ok id="SN12341235123">  </bo ok> <!-- 错误 -->
  ```

  \> xml中的元素（标签）分为单标签和双标签

  ​	>> 单标签格式：<标签名 属性=”值" 属性="值" ……/>
  ​	>> 双标签格式：<标签名 属性="值" 属性=“值" ……> 文本数据或子标签 </标签名>

  ~~~xml
  <books>
      <!-- 双标签 -->
      <book sn="SN12341232"> 
          <name>辟邪剑谱</name> 
          <price>9.9</price>
          <author>班主任</author>
      </book>
      
      <!-- 单标签 -->
      <book sn="SN12341231" name="葵花宝典" price="99.99" auther="班长" /> 
  </books>
  ~~~

* 属性

  \> 属性可以提供元素的额外信息
  \> 属性必须使用引号引起来，不引会报错

  ~~~xml
  <book sn="SN12341232"> </book> <!-- sn 为属性 -->
  ~~~

* 语法规则
  \> 元素都要有关闭标签

  ~~~xml
  <books>
      <book >  </book>
      <book /> 
  </books>
  ~~~

  \> 标签对大小写敏感

  ~~~xml
  <books> </Books> <!-- 错误 -->
  ~~~

  \> 正确嵌套，不能交叉嵌套

  ~~~xml
  <book sn="SN12341232"> 
      <name>辟邪剑谱</name> 
      <price>9.9</price>
      <author>班主任
  </book> </author> <!-- 错误 -->
  ~~~

  \> 文档必须有根元素（根元素是没有父标签的顶级元素，而且是唯一一个才行）

  ~~~xml
  <books></books>
  <books1></books1> <!-- 错误 -->
  ~~~

  \> 特殊字符用特殊标记

  ~~~xml
  <books>
  	<book>
      	<author>&lt;班长&gt;</author>
      </book>
  </books>
  ~~~

  \> 文本区域（CDATA区）
  	`<![CDATA[ 这里可以输入的字符原样显示，不会解析xml ]]>`

  ~~~xml
  <books>
  	<book>
      	<author>
              <![CDATA[
  				<<<<<<<<班长>>>>>>>>
  			]]>
          </author>
      </book>
  </books>
  ~~~

  

### XML解析

标记性文档都可以使用w3c组织制定的dom技术来解析

* 历程
  早期dom解析技术是w3c组织制定的 
  ==>> sun公司对dom解析技术进行升级为SAX（类似事件机制通过回调告诉用户当前正在解析的内容，一行一行的读取XML文件进行解析，不会创建大量的dom对象
  ==>> jdom 在dom基础上进行了封装
  ==>> dom4j 又对 jdom进行了封装

> 第三方的解析技术，需要使用第三方提供好的类库才可以解析XML文件

### dom4j 解析技术

* 步骤：
  1. dom4j.jar 包复制到 lib 目录下
  2. 将 dom4j.jar 添加到类路径 `Add as Library` 下
  3. dom4j编程

### dom4j编程步骤

1. 加载xml文件创建document对象

   > SaxReader对象用于读取xml文件并且用该对象的`reader()方法` 创建Document

   ~~~java
   package com.atguigu.pojo;
   import org.dom4j.Document;
   import org.dom4j.io.SAXReader;
   import org.junit.Test;
   public class Dom4jTest {
       @Test
       public void test1() throws Exception {
           // 创建一个SaxReader输入流，去读取 xml配置文件，生成Document对象
           SAXReader saxReader = new SAXReader();
           Document document = saxReader.read("src/books.xml");
           System.out.println(document);
       }
   }
   ~~~

2. 通过document对象拿到根元素对象

   > Document对象中`getRootElement()方法`获取根元素
   >
   > Element对象的`asXML()方法`，将当前元素转换成为 String 对象

   ~~~java
   @Test
   public void readXML() throws DocumentException {
       SAXReader reader = new SAXReader();
       Document document = reader.read("src/books.xml");
       //通过 Document 对象。拿到 XML 的根元素对象
       Element root = document.getRootElement();
       System.out.println( root.asXML() );
   }
   ~~~

3. 通过 `根元素对象.elements(标签名);` 可以返回一个集合，该集合有所指定的标签名的元素对象

   > Element对象的elements(标签名)方法可以获取当前元素下的指定子元素的集合

   ~~~java
   @Test
   public void readXML() throws DocumentException {
       SAXReader reader = new SAXReader();
       Document document = reader.read("src/books.xml");
       Element root = document.getRootElement();
       //通过根元素对象。获取所有的 book 标签对象
       List<Element> books = root.elements("book");
   }
   ~~~

4. 找到需要修改、删除的子元素，进行相应操作

   > Element对象的`element(标签名)方法`可以获取对象内的每一个元素
   >
   > Element对象的`getText() 方法`拿到起始标签和结束标签之间的文本内容

   ~~~java
   @Test
   public void readXML() throws DocumentException {
       SAXReader reader = new SAXReader();
       Document document = reader.read("src/books.xml");
       Element root = document.getRootElement();
       List<Element> books = root.elements("book");
       for (Element book : books) {
           // 拿到 book 下面的 name 元素对象
           Element nameElement = book.element("name");
           // 拿到 book 下面的 price 元素对象
           Element priceElement = book.element("price");
           // 拿到 book 下面的 author 元素对象
           Element authorElement = book.element("author");
           System.out.println("书名" + nameElement.getText() + " , 价格:"
           + priceElement.getText() + ", 作者：" + authorElement.getText());
       }
   }
   ~~~

5. 保存到硬盘上



## Tomcat服务器

### javaweb概念

* javaweb是指通过Java语言编写可以通过浏览器访问的程序的总称，是基于请求和响应来开发的。

  \> 请求是指客户端给服务器发送数据，叫做request
  \> 响应是指服务器给客户端回传数据，叫做response
  \> 请求和响应是成对出现的，有请求必有响应

![image-20210911163228087](E:\软工\typora笔记\javaweb\xml.assets\image-20210911163228087.png)

* web资源分为静态资源和动态资源
  \> 静态资源：html、css、js、txt、mp4、jpg
  \> 动态资源：jsp页面、servlet程序

### Tomcat

* 目录介绍

  > bin 专门用来存放 Tomcat 服务器的**可执行程序**
  > conf 专门用来存放 Tocmat 服务器的**配置文件** 
  > lib 专门用来存放 Tomcat 服务器的 **jar 包** 
  > logs 专门用来存放 Tomcat 服务器**运行时输出的日记信息** 
  > temp 专门用来存放 Tomcdat **运行时产生的临时数据** 
  > webapps 专门用来存放部署的 **Web 工程**。 
  > work 是 Tomcat 工作时的目录，用来存放 Tomcat 运行时 jsp 翻译为 Servlet 的源码，和 Session 钝化的目录

* 启动和停止方式
  \> bin目录下的 startup.bat 文件双击启动，shutdown.bat 文件双击停止。
  \> 命令行在bin目录下执行 catalina run 命令启动，关闭startup.bat 弹窗停止。

* 修改Tomcat端口号
  在conf目录下的 server.xml配置文件，找到Connector标签，修改port属性为所需的端口号，再重启Tomcat服务器才能生效。（端口号范围：1-65535）

* http协议默认端口号是：80

* 部署web工程到Tomcat中
  \> 方式一：直接将工程文件复制到webapps目录下
  访问方式： `http://ip:port/工程名/目录下/文件名`

  \> 方式二：创建配置文件在conf\Catalina\localhost\下

  ~~~xml
  <!-- Context 表示一个工程上下文
  path 表示工程的访问路径:/abc(路径代名)
  docBase 表示你的工程目录在哪里(绝对路径)
  -->
  <Context path="/abc" docBase="E:\book" />
  ~~~

  访问方式：`http://ip:port/abc/`

* file协议表示告诉浏览器直接读取file:协议后面的路径，解析展示在浏览器上。访问的是本地路径。

* 默认访问
  \> http://ip:port/ ====>>>> 没有工程名的时候，默认访问的是 ROOT 工程。
  \> http://ip:port/工程名/ ====>>>> 没有资源名，默认访问 index.html 页面

* 创建Javaweb动态工程

  1. IDEA整合Tomcat服务器

  2. 创建 Java Enterprise 工程, 选择 Application Server 服务器

  3. 给动态web工程添加格外jar包

     project Structure ==>> libraries(添加类库并在类库中添加所需要的jar包文件) ==>> artifacts(类库添加到打包部署中)

  4. 在IDEA中部署工程到Tomcat上运行

     run/debug configurations ==>> deployment(部署web工程添加到Tomcat运行实例中) ==>> server(修改web工程相关信息)

* web工程目录介绍

  ![image-20210911192905404](E:\软工\typora笔记\javaweb\xml.assets\image-20210911192905404.png)

## Servlet

\> servlet 是 Java EE 接口之一
\> servlet 就是 Java web 三大组件之一：servlet程序、filter过滤器、listener监听器
\> servlet 是运行在服务器上的一个Java小程序，可以接受接受客户端发送过来的请求，并响应数据给客户端。

### 实现servlet程序

<img src="E:\软工\typora笔记\javaweb\xml.assets\image-20210914112212251.png" alt="image-20210914112212251"  />

1. 编写一个类实现servlet接口

2. 编写方法和属性

   ~~~java
   package com.atguigu.servlet;
   import javax.servlet.*;
   import javax.servlet.http.HttpServletRequest;
   import java.io.IOException;
   
   public class HelloServlet implements Servlet {
       @Override
       public ServletConfig getServletConfig() {
           return null;
       }
   
       @Override
       public String getServletInfo() {
           return null;
       }
   }
   ~~~

3. 在 web.xml 中配置servlet程序的访问地址

   ~~~xml
   <?xml version="1.0" encoding="UTF-8"?>
   <web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
   xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
   xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee
   http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
   version="4.0">
       <servlet> 
           <servlet-name>HelloServlet</servlet-name>       
           <servlet-class>com.atguigu.servlet.HelloServlet</servlet-class>
       </servlet>   
       <servlet-mapping>       
           <servlet-name>HelloServlet</servlet-name>        
           <url-pattern>/hello</url-pattern>
       </servlet-mapping>
   </web-app>
   
   <!-- servlet 标签给 Tomcat 配置 Servlet 程序 -->
   	<!--servlet-name 标签 Servlet 程序起一个别名（一般是类名） -->
   	<!--servlet-class 是 Servlet 程序的全类名-->
   <!--servlet-mapping 标签给 servlet 程序配置访问地址-->
   	<!--servlet-name 标签的作用是告诉服务器，我当前配置的地址给哪个 Servlet 程序使用-->
   	<!--url-pattern 标签配置访问地址 <br/>
           / 斜杠在服务器解析的时候，表示地址为：http://ip:port/工程路径 <br/>
           /hello 表示地址为：http://ip:port/工程路径/hello <br/>
       -->
   ~~~

   常见错误

   ​	\> url-pattern 中配置的路径没有以斜杠打头

   ​	\> servlet-name 配置的值不存在

   ​	\> servlet-class 标签的全类名配置错误

   

### Servlet的生命周期

| 方法      | 意义        | 说明                                      |
| --------- | ----------- | ----------------------------------------- |
|           | 构造器      | 仅在第一次访问的时候创建servlet程序会调用 |
| init()    | S初始化方法 | 仅在第一次访问的时候创建servlet程序会调用 |
|           | 其他方法    | 每次访问都会调用                          |
| destroy() | 销毁方法    | 在web工程停止的时候调用                   |

~~~Java
package com.atguigu.servlet;
import javax.servlet.*;
import javax.servlet.http.HttpServletRequest;
import java.io.IOException;

public class HelloServlet implements Servlet {

    public HelloServlet() {
        System.out.println("1 构造器方法");
    }

    @Override
    public void init(ServletConfig servletConfig) throws ServletException {
        System.out.println("2 init初始化方法");
    }

    @Override
    public void service(ServletRequest servletRequest, ServletResponse servletResponse) throws ServletException, IOException {
        System.out.println("3 service === Hello Servlet 被访问了");
    }

    @Override
    public void destroy() {
        System.out.println("4 . destroy销毁方法");
    }
}
~~~



### get 和 post 请求

~~~java
public class HelloServlet implements Servlet {
    /**
     * service方法是专门用来处理请求和响应的
     * @param servletRequest
     * @param servletResponse
     * @throws ServletException
     * @throws IOException
     */
    @Override
    public void service(ServletRequest servletRequest, ServletResponse servletResponse) throws ServletException, IOException {
        // 类型转换（因为它有getMethod()方法）
        HttpServletRequest httpServletRequest = (HttpServletRequest) servletRequest;
        // 获取请求的方式
        String method = httpServletRequest.getMethod();
        if ("GET".equals(method)) {
            doGet();
        } else if ("POST".equals(method)) {
           doPost();
        }
    }

    /**
     * 做get请求的操作，在get请求的时候调用
     */
    public void doGet(){
        System.out.println("get请求");
        System.out.println("get请求");
    }
    /**
     * 做post请求的操作，在post请求的时候调用
     */
    public void doPost(){
        System.out.println("post请求");
        System.out.println("post请求");
    }
}
~~~



### 继承 HttpServlet 实现 servlet 程序

![image-20210914122209548](E:\软工\typora笔记\javaweb\xml.assets\image-20210914122209548.png)

1. 编写一个类继承 Httpservlet 类

2. 重写 doget 或 dopost 方法

   ```Java
   package com.atguigu.servlet;
   import javax.servlet.ServletConfig;
   import javax.servlet.ServletException;
   import javax.servlet.http.HttpServlet;
   import javax.servlet.http.HttpServletRequest;
   import javax.servlet.http.HttpServletResponse;
   import java.io.IOException;
   
   public class HelloServlet2 extends HttpServlet {
   
       @Override
       protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
           System.out.println("HelloServlet2 的doGet方法");
       }
       
       @Override
       protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
           System.out.println("HelloServlet2 的doPost方法");
       }
   }
   ```

3. 在 web.xml 中配置servlet程序的访问地址



### servletConfig类

\> Servlet程序的配置信息类
\> Servlet程序和ServletConfig对象都是由Tomcat负责创建，我们负责使用
\> Servlet程序默认是第一次访问的时候创建，ServletConfig是每个Servlet程序创建时就创建一个对应的ServletConfig对象

| 方法                     | 说明                                   |
| ------------------------ | -------------------------------------- |
| getServletName()         | 获取Servlet程序的别名 servlet-name的值 |
| getInitParameter("参数") | 获取初始化参数init-param               |
| getServletContext()      | 获取ServletContext对象                 |

* 方式一：

  ~~~java
  public class HelloServlet2 extends HttpServlet {
       @Override
      public void init(ServletConfig servletConfig) throws ServletException {
          // 1、可以获取 Servlet 程序的别名 servlet-name 的值
          System.out.println("HelloServlet 程序的别名是:" + servletConfig.getServletName());
          
          // 2、获取初始化参数 init-param
          System.out.println("初始化参数 username 的值是;" + servletConfig.getInitParameter("username"));
          System.out.println("初始化参数 url 的值是;" + servletConfig.getInitParameter("url"));
          
          // 3、获取 ServletContext 对象
          System.out.println(servletConfig.getServletContext());
      }   
  }
  ~~~

* 方式二：

  ~~~java
  public class HelloServlet2 extends HttpServlet {
      @Override
      public void init(ServletConfig config) throws ServletException {
          super.init(config); // *****
      }
      
      @Override
      protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
          ServletConfig servletConfig = getServletConfig(); //****
          System.out.println(servletConfig);
  
          // 1、可以获取 Servlet 程序的别名 servlet-name 的值
          System.out.println("HelloServlet 程序的别名是:" + servletConfig.getServletName());
          
          // 2、获取初始化参数 init-param
          System.out.println("初始化参数 username 的值是;" + servletConfig.getInitParameter("username"));
          System.out.println("初始化参数 url 的值是;" + servletConfig.getInitParameter("url"));
          
          // 3、获取 ServletContext 对象
          System.out.println(servletConfig.getServletContext());
      }
  }
  ~~~

  注意：重写了Servlet的init方法后一定要记得调用父类的init方法

  > 否则会报错，报错类型是空指针异常，所以可以推断`config`的值为空。而`config`是由`getServletConfig()`方法返回的。自定义的Servlet程序的直接父类是HttpServlet，HttpServlet的父类是GenericServlet，而自定义的Servlet程序直接可以覆盖GenericServlet中的方法的原因是HttpServlet没对某些方法进行重写

![image-20210914172233459](E:\软工\typora笔记\javaweb\xml.assets\image-20210914172233459.png)

* 配置文件：

  ~~~xml
  <servlet>
      <servlet-name>HelloServlet2</servlet-name>
      <servlet-class>com.atguigu.servlet.HelloServlet2</servlet-class>
  
      <!--init-param是初始化参数-->
      <init-param>
          <!--是参数名-->
          <param-name>username</param-name>
          <!--是参数值-->
          <param-value>root2</param-value>
      </init-param>
      <!--init-param是初始化参数-->
      <init-param>
          <!--是参数名-->
          <param-name>url</param-name>
          <!--是参数值-->
          <param-value>jdbc:mysql://localhost:3306/test2</param-value>
      </init-param>
  </servlet>
  ~~~

  

### ServletContext类

\> ServletContext是一个接口，表示Servlet上下文对象
\> 一个web工程，只有一个ServletContext对象实例
\> ServletContext对象是一个域对象(域对象可以像Map一样存取数据的对象，这里的域是存取数据的操作范围是整个web工程)
\> ServletContext是在web工程部署启动的时候创建，在web工程停止的时候销毁

|        | 存数据         | 取数据         | 删除数据          |
| ------ | -------------- | -------------- | ----------------- |
| Map    | put()          | get()          | remove()          |
| 域对象 | setAttribute() | getAttribute() | removeAttribute() |

* 作用

  \> 获取 web.xml 中配置的上下文参数 context-param
  \> 获取当前的工程路径，格式:`/工程路径`
  \> 获取工程部署后在服务器硬盘上的绝对路径
  	/ 斜杠被服务器解析地址为:http://ip:port/工程名/  映射到IDEA代码的web目录

  \> 像Map一样存取数据

  ~~~java
  public class ContextServlet extends HttpServlet {
      protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
  
      }
  
      protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
          ServletContext context = getServletConfig().getServletContext();
          //或者 ServletContext context = getServletContext();
          
  //        1、获取web.xml中配置的上下文参数context-param
          String username = context.getInitParameter("username");
          System.out.println("context-param参数username的值是:" + username);
          System.out.println("context-param参数password的值是:" + context.getInitParameter("password"));
          
  //        2、获取当前的工程路径，格式: /工程路径
          System.out.println( "当前工程路径:" + context.getContextPath() );
          
  //        3、获取工程部署后在服务器硬盘上的绝对路径
          System.out.println("工程部署的路径是:" + context.getRealPath("/"));
          System.out.println("工程下css目录的绝对路径是:" + context.getRealPath("/css"));
          System.out.println("工程下imgs目录1.jpg的绝对路径是:" + context.getRealPath("/imgs/1.jpg"));
          
  //        4、像Map一样存取数据
          System.out.println("保存之前: Context1 获取 key1 的值是:"+ context.getAttribute("key1"));
  context.setAttribute("key1", "value1");
  System.out.println("Context1 中获取域数据 key1 的值是:"+ context.getAttribute("key1"))
      }
  }
  ~~~

  ~~~java
  // 存取数据的操作范围是整个web工程
  public class ContextServlet2 extends HttpServlet {
      protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
          ServletContext context = getServletContext();
          System.out.println(context);
          System.out.println("Context2 中获取域数据key1的值是:"+ context.getAttribute("key1"));
      }
  }
  ~~~

  配置文件:

  ~~~xml
  <!--context-param是上下文参数(它属于整个web工程)-->
  <context-param>
      <param-name>username</param-name>
      <param-value>context</param-value>
  </context-param>
  <!--context-param是上下文参数(它属于整个web工程)-->
  <context-param>
      <param-name>password</param-name>
      <param-value>root</param-value>
  </context-param>
  ~~~

### HTTP协议

客户端给服务器发送的数据叫请求（GET请求、POST请求）
服务器给客户端回传数据叫响应

* GET请求

  * 格式:

    请求行
    	请求的方式 GET
    	请求的资源路径[+?+请求参数] 
    	请求的协议的版本号 HTTP/1.1 

    请求头
    	key : value 组成 不同的键值对，表示不同的含义。

  *  应用场景

    1. form 标签 method=get 
    2. a 标签 
    3. link 标签引入 css 
    4. Script 标签引入 js 文件 
    5. img 标签引入图片 
    6. iframe 引入 html 页面 
    7. 在浏览器地址栏中输入地址后敲回车

![image-20210914181757355](E:\软工\typora笔记\javaweb\xml.assets\image-20210914181757355.png)

* POST请求

  * 格式
    请求行
    	请求的方式 POST 
    	请求的资源路径[+?+请求参数]
    	请求的协议的版本号 HTTP/1.1 

    请求头 
    	key : value 不同的请求头，有不同的含义 

    空行 

    请求体 ===>>> 就是发送给服务器的数据

  * 应用场景

    1. form 标签 method=post

![image-20210914182052788](E:\软工\typora笔记\javaweb\xml.assets\image-20210914182052788.png)

* 常用请求头

  | 请求头          | 说明                           |
  | --------------- | ------------------------------ |
  | Accept          | 表示客户端可以接收的数据类型   |
  | Accpet-Languege | 表示客户端可以接收的语言类型   |
  | User-Agent      | 表示客户端浏览器的信息         |
  | Host            | 表示请求时的服务器 ip 和端口号 |

* 响应的HTTP协议格式

  响应行
  	响应的协议和版本号
  	响应状态码
  	响应状态描述符 

  响应头 
  	key : value 不同的响应头，有其不同含义 

  空行

  响应体 ---->>> 就是回传给客户端的数

![image-20210914183228935](E:\软工\typora笔记\javaweb\xml.assets\image-20210914183228935.png)

* 响应码

  | 响应码 | 说明                                                         |
  | ------ | ------------------------------------------------------------ |
  | 200    | 表示请求成功                                                 |
  | 302    | 表示请求重定向                                               |
  | 404    | 表示请求服务器已经收到了，但是你要的数据不存在（请求地址错误） |
  | 500    | 表示服务器已经收到请求，但是服务器内部错误（代码错误）       |

### HttpServletRequest类

每次只要有请求进入Tomcat服务器，Tomcat服务器就会把请求过来的HTTP协议信息解析好封装到Request对象中。然后传递到service方法（doGet 和 doPost）中给我们使用。我们可以通过HttpServletRequest对象，获取到所有请求的信息。

* 常用API

  | 常用方法        | 说明                                 |
  | --------------- | ------------------------------------ |
  | getRequestURI() | 获取请求的资源路径                   |
  | getRequestURL() | 获取请求的统一资源定位符（绝对路径） |
  | getRemoteHost() | 获取客户端IP地址                     |
  | getHeader()     | 获取请求头                           |
  | getMethod()     | 获取请求的方式（GET 或 POST）        |

  ~~~java
  public class RequestAPIServlet extends HttpServlet {
  
      @Override
      protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
  //        getRequestURI()	获取请求的资源路径
          System.out.println("URI => " + req.getRequestURI());
          
  //        getRequestURL()	获取请求的统一资源定位符（绝对路径）
          System.out.println("URL => " + req.getRequestURL());
          
  //        getRemoteHost()	获取客户端的ip地址
          System.out.println("客户端 ip地址 => " + req.getRemoteHost());
          
  //        getHeader()		获取请求头
          System.out.println("请求头User-Agent ==>> " + req.getHeader("User-Agent"));
          
  //        getMethod()		获取请求的方式GET或POST
          System.out.println( "请求的方式 ==>> " + req.getMethod() );
      }
  }
  
          /**     getRemoteHost()				获取客户端的ip地址
           * 在IDEA中，使用localhost访问时，得到的客户端 ip 地址是 ===>>> 127.0.0.1<br/>
           * 在IDEA中，使用127.0.0.1访问时，得到的客户端 ip 地址是 ===>>> 127.0.0.1<br/>
           * 在IDEA中，使用 真实ip 访问时，得到的客户端 ip 地址是 ===>>> 真实的客户端 ip 地址<br/>
           */
  ~~~

  

* 获取请求参数（解决中文乱码问题）

  | 常用方法             | 说明                               |
  | -------------------- | ---------------------------------- |
  | getParameter()       | 获取请求的参数                     |
  | getParameterValues() | 获取请求的参数（多个值的时候使用） |

  ~~~java
  public class ParameterServlet extends HttpServlet {
  
      @Override
      protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
          System.out.println("-------------doGet------------");
  
          // 获取请求参数
          String username = req.getParameter("username");
  		//解决中文乱码问题：
          //1 先以iso8859-1进行编码
          //2 再以utf-8进行解码
          username = new String(username.getBytes("iso-8859-1"), "UTF-8");
  
          String password = req.getParameter("password");
          String[] hobby = req.getParameterValues("hobby");
  
          System.out.println("用户名：" + username);
          System.out.println("密码：" + password);
          System.out.println("兴趣爱好：" + Arrays.asList(hobby));
      }
  
      @Override
      protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
          // 设置请求体的字符集为UTF-8，从而解决post请求的中文乱码问题
          // 也要在获取请求参数之前调用才有效
          req.setCharacterEncoding("UTF-8");
  
          System.out.println("-------------doPost------------");
          // 获取请求参数
          String username = req.getParameter("username");
          String password = req.getParameter("password");
          String[] hobby = req.getParameterValues("hobby");
  
          System.out.println("用户名：" + username);
          System.out.println("密码：" + password);
          System.out.println("兴趣爱好：" + Arrays.asList(hobby));
      }
  }
  ~~~

  ~~~HTML
  <!DOCTYPE html>
  <html lang="zh_CN">
  <head>
      <meta charset="UTF-8">
      <title>Title</title>
  </head>
  <body>
      <form action="http://localhost:8080/07_servlet/parameterServlet" method="post">
          用户名：<input type="text" name="username"><br/>
          密码：<input type="password" name="password"><br/>
          兴趣爱好：<input type="checkbox" name="hobby" value="cpp">C++
          <input type="checkbox" name="hobby" value="java">Java
          <input type="checkbox" name="hobby" value="js">JavaScript<br/>
          <input type="submit">
      </form>
  </body>
  </html>
  ~~~

* 请求的转发
  请求转发是指，服务器收到请求后，从一次资源跳转到另一个资源的操作
  ![image-20210915082114250](E:\软工\typora笔记\javaweb\no3.JavaWeb.assets\image-20210915082114250.png)

  | 常用方法                | 说明             |
  | ----------------------- | ---------------- |
  | setAttribute(key,value) | 设置域数据       |
  | getAttribute(key)       | 获取域数据       |
  | getRequestDispatcher()  | 获取请求转发对象 |

  ~~~java
  public class Servlet1 extends HttpServlet {
      @Override
      protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
          // 获取请求的参数（办事的材料）查看
          String username = req.getParameter("username");
          System.out.println("在Servlet1（柜台1）中查看参数（材料）：" + username);
  
          // 给材料 盖一个章，并传递到Servlet2（柜台 2）去查看
          req.setAttribute("key1","柜台1的章");
  
          // 问路：Servlet2（柜台 2）怎么走
          RequestDispatcher requestDispatcher = req.getRequestDispatcher("/servlet2");
  		//不能访问工程外的资源
  		//RequestDispatcher requestDispatcher = req.getRequestDispatcher("http://www.baidu.com");
  
          // 走向Sevlet2（柜台 2）
          requestDispatcher.forward(req,resp);
      }
  }
  
          /**
           * 请求转发必须要以斜杠打头，/ 斜杠表示地址为：http://ip:port/工程名/ , 映射到IDEA代码的web目录
           */
  ~~~

  ~~~java
  public class Servlet2 extends HttpServlet {
      @Override
      protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
          // 获取请求的参数（办事的材料）查看
          //共享request域中的数据
          String username = req.getParameter("username");
          System.out.println("在Servlet2（柜台2）中查看参数（材料）：" + username);
  
          // 查看 柜台1 是否有盖章
          Object key1 = req.getAttribute("key1");
          System.out.println("柜台1是否有章：" + key1);
  
          // 处理自己的业务
          System.out.println("Servlet2 处理自己的业务 ");
      }
  }
  ~~~

### base 标签

可以设置当前页面中所有相对路径工作时，参照哪个路径来进行跳转

* 不用base标签，使用请求转发来进行跳转时，容易发生错误
  ![image-20210915084256214](E:\软工\typora笔记\javaweb\no3.JavaWeb.assets\image-20210915084256214.png)

* 使用base标签避免错误发生

  ~~~html
  <!DOCTYPE html> <!-- 二级页面 -->
  <html lang="zh_CN">
  <head>
      <meta charset="UTF-8">
      <title>Title</title>
      <!--base标签设置页面相对路径工作时参照的地址
              href 属性就是参数的地址值
      -->
      <base href="http://localhost:8080/07_servlet/a/b/">
  </head>
  <body>
      这是a下的b下的c.html页面<br/>
      <a href="../../index.html">跳回首页</a><br/>
  </body>
  </html>
  ~~~

  ~~~html
  <!DOCTYPE html> <!-- 首页 -->
  <html lang="zh_CN">
  <head>
      <meta charset="UTF-8">
      <title>首页</title>
  </head>
  <body>
      这是Web下的index.html <br/>
      <a href="a/b/c.html">a/b/c.html</a><br/>
      <a href="http://localhost:8080/07_servlet/forwardC">请求转发：a/b/c.html</a><br/>
      <a href="/">斜杠</a>
  </body>
  </html>
  ~~~

  ~~~java
  public class ForwardC extends HttpServlet {
      @Override
      protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
          System.out.println("经过了ForwardC程序");
          req.getRequestDispatcher("/a/b/c.html").forward(req, resp);
      }
  }
  ~~~

*  web中的相对路径和绝对路径
  绝对路径：http://ip:port/工程路径/资源路径
  相对路径：一样（略）



* web中 / 斜杠的不同意义
  在 web 中 / 斜杠是一种绝对路径

  * / 斜杠 如果被浏览器解析，得到的地址是:http://ip:port/
    `<a href='/'>斜杠</a>`

  * / 斜杠 如果被服务器解析，得到的地址是:http://ip:port/工程路径
    `<url-pattren>/servlet1</url-pattern>`
    特殊情况：response.sendRediect(“/”); 把斜杠发送给浏览器解析。 http://ip:port/

### HttpServletResponse类

每次请求进来，Tomcat服务器都会创建一个response对象传递给servlet程序使用，HttpServletRequest表示请求过来的信息，HttpServletResponse表示所有响应的信息。如果需要设置返回给客户端的信息，都可以通过HttpServletResponse对象来进行设置

* 往客户端回传数据（解决响应的乱码问题）

  ~~~java
  public class ResponseIOServlet extends HttpServlet {
      @Override
      protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
  
            System.out.println( resp.get() );//默认ISO-8859-1
  
          // 设置服务器字符集为UTF-8
          resp.setCharacterEncoding("UTF-8");
          
          // 通过响应头，设置浏览器也使用UTF-8字符集
          // 方式一：
          resp.setHeader("Content-Type", "text/html; charset=UTF-8");
  
          // 方式二:
          // 它会同时设置服务器和客户端都使用UTF-8字符集，还设置了响应头
          // 此方法一定要在获取流对象之前调用才有效
          resp.setContentType("text/html; charset=UTF-8");
  
          System.out.println( resp.getCharacterEncoding() );
  
  //        要求 ： 往客户端回传 字符串 数据。
          PrintWriter writer = resp.getWriter();
          writer.write("国哥很帅！");
      }
  }
  
  ~~~

* 请求重定向
  请求重定向是指客户端给服务器发请求，然后服务器告诉客户端新地址，客户端访问新地址（之前地址可能已废弃）
  ![image-20210915183658910](E:\软工\typora笔记\javaweb\no3.JavaWeb.assets\image-20210915183658910.png)
  
  ~~~java
  public class Response1 extends HttpServlet {
  
      @Override
      protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
          System.out.println("曾到此一游 Response1 ");
          req.setAttribute("key1", "value1");
          
  //方式一：
          // 设置响应状态码302 ，表示重定向，（已搬迁）
          resp.setStatus(302);
          // 设置响应头，说明 新的地址在哪里
          resp.setHeader("Location", "http://localhost:8080/07_servlet/response2");
          //可以访问工程外的资源
          resp.setHeader("Location", "http://localhost:8080");
          
  //方式二：
          resp.sendRedirect("http://localhost:8080");
      }
  }
  ~~~
  
  ~~~java
  public class Response2 extends HttpServlet {
      @Override
      protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
          System.out.println(req.getAttribute("key1"));
          //不能共享request域中数据
          resp.getWriter().write("response2's result!");
      }
  }
  ~~~
  
  

## JavaEE项目架构

![image-20210916105610844](E:\软工\typora笔记\javaweb\no3.JavaWeb.assets\image-20210916105610844.png)

* 分层的目的是为了解耦。解耦就是为了降低代码的耦合度。方便项目后期的维护和升级。

  web 层 					com.atguigu.web/servlet/controller 
  service 层 				com.atguigu.service 								Service 接口包 
  								com.atguigu.service.impl 						Service 接口实现类 
  dao 持久层 			com.atguigu.dao										 Dao接口包 
  								com.atguigu.dao.impl 								Dao 接口实现类 
  实体 bean 对象 	com.atguigu.pojo/entity/domain/bean 	JavaBean 类 
  测试包 					com.atguigu.test/junit 
  工具类 					com.atguigu.utils

![image-20210916110157536](E:\软工\typora笔记\javaweb\no3.JavaWeb.assets\image-20210916110157536.png)

* 后台项目步骤：
  1. 创建数据库和表
  2. 编写数据库表对应的JavaBean对象
  3. 编写工具类（JdbcUtils）
  4. 工具类（jdbcUtil）s测试
  5. 编写Dao接口
  6. 编写Dao接口实现类
  7. Dao实现类（userDao）测试
  8. 编写Service实现类（UserService）
  9. 编写web层

