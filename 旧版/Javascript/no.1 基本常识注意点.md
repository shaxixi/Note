[toc]

##  基本常识注意点

### Javascript用法

* **在HTML页面中插入Javascript：** 用<script> </script>标签

    ~~~html
    /* <head> </head> 或者 <body> </body> 中均可插入 */
    <script> 
    //代码
    </script>
    ~~~

* **外部的 Javascript ：**用<script src=" "> </script>引用

  javascript文件:

    ~~~javascript
    /* 	外部脚本不能包含 <script> 标签。 */
    function myFunction()
    {
        document.getElementById("demo").innerHTML="我的第一个 JavaScript 函数";
    }
    ~~~
  html文件:
    ~~~html
    <!-- 如需使用外部文件，在 <script> 标签的 "src" 属性中设置该 .js 文件 -->
    <!DOCTYPE html>
    <html>
    	<body>
    		<script src="myScript.js"></script>
    	</body>
    </html>
    ~~~

### 输出

> JavaScript 没有任何打印或者输出的函数。JavaScript 可以通过不同的方式来输出数据：
>
> 1. 使用 **window.alert()** 弹出警告框。
>2. 使用 **document.write()** 方法将内容写到 HTML 文档中。
> 3. 使用 **innerHTML** 写入到 HTML 元素。
>4. 使用 **console.log()** 写入到浏览器的控制台。



* 使用 window.alert() :
  你可以弹出警告框来显示数据

  ~~~html
  <!DOCTYPE html>
  <html>
  	<body>
  		<h1>我的第一个页面</h1>
  		<p>我的第一个段落。</p>
  		<script>
  			window.alert(5 + 6);
  		</script>
  	</body>
  </html>
  ~~~

* 使用 document.getElementById(*id*) 方法: 
  如需从 JavaScript 访问某个 HTML 元素,使用 "id" 属性来标识 HTML 元素，并 innerHTML 来获取或插入元素内容

  ~~~html
  <!DOCTYPE html>
  <html>
  	<body>
  		<h1>我的第一个 Web 页面</h1>
  		<p id="demo">我的第一个段落</p>
  		<script>
  			document.getElementById("demo").innerHTML = "段落已修改。";
  		</script>
  	</body>
  </html>
  
  <!-- 以上 JavaScript 语句（在 <script> 标签中）可以在 web 浏览器中执行：
  document.getElementById("demo") 是使用 id 属性来查找 HTML 元素的 JavaScript 代码 。
  innerHTML = "段落已修改。" 是用于修改元素的 HTML 内容(innerHTML)的 JavaScript 代码。 -->
  ~~~

* 使用 **document.write()** 方法:
  出于测试目的，您可以将JavaScript直接写在HTML 文档中

  ~~~html
  <!DOCTYPE html>
  <html>
  	<body>
  		<h1>我的第一个 Web 页面</h1>
  		<p>我的第一个段落。</p>
  		<script>
  			document.write(Date());
  		</script>
  	</body>
  </html>
  
  <!-- 请使用 document.write() 仅仅向文档输出写内容。如果在文档已完成加载后执行 document.write，整个 HTML 页面将被覆盖。  如：-->
  <!DOCTYPE html>
  <html>
  	<body>
  		<h1>我的第一个 Web 页面</h1>
  		<p>我的第一个段落。</p>
  		<button onclick="myFunction()">点我</button>
  		<script>
  			function myFunction() {
  			    document.write(Date());
  			}
  		</script>
  	</body>
  </html>
  ~~~

* 使用 **console.log()** 方法
  在浏览器中显示 JavaScript 值。浏览器中使用 F12 来启用调试模式， 在调试窗口中点击 "Console" 菜单。

  ~~~html
  <!DOCTYPE html>
  <html>
  	<body>
  		<h1>我的第一个 Web 页面</h1>
  		<script>
  			a = 5;
  			b = 6;
  			c = a + b;
  			console.log(c);
  		</script>
  	</body>
  </html>
  ~~~

  ![img](E:\软工\typora笔记\Javascript\no.1 基本常识注意点.assets\console-log.jpg)
### 语法

* 字面量 Vs 变量
  在指令式语言中，变量通常是可变的。字面量是一个恒定的值。

  * 字面量：数字字面量 （整数：1001  小数：3.14 科学计数(e)：123e5）
    				字符串字面量 （单引号：'John Doe' 双引号："John Doe"）
    				表达式字面量 （计算：5 * 10）
    				数组字面量 （[40, 100, 1, 5, 25, 10]）
    				对象字面量 （{firstName:"John", lastName:"Doe", age:50, eyeColor:"blue"})
    				函数字面量 （function myFunction(a, b) { return a * b;}）

  * 变量： 用 **关键字 var** 定义**变量**，**等号 =** 为变量 **赋值**

    ~~~javascript
    var x, length
    x = 5
    length = 6
    ~~~

* 注释
  单行： `//`   多行： `/*   */`

* javascript区分字母大小写

### 语句

JavaScript 语句向浏览器发出的命令。语句的作用是告诉浏览器该做什么。

* 标识符（关键字）

  | 语句         | 描述                                                         |
  | :----------- | :----------------------------------------------------------- |
  | break        | 用于跳出循环。                                               |
  | **catch**    | **语句块，在 try 语句块执行出错时执行 catch 语句块。**       |
  | continue     | 跳过循环中的一个迭代。                                       |
  | do ... while | 执行一个语句块，在条件语句为 true 时继续执行该语句块。       |
  | for          | 在条件语句为 true 时，可以将代码块执行指定的次数。           |
  | for ... in   | 用于遍历数组或者对象的属性（对数组或者对象的属性进行循环操作）。 |
  | **function** | **定义一个函数**                                             |
  | if ... else  | 用于基于不同的条件来执行不同的动作。                         |
  | return       | 退出函数                                                     |
  | switch       | 用于基于不同的条件来执行不同的动作。                         |
  | **throw**    | **抛出（生成）错误 。**                                      |
  | **try**      | **实现错误处理，与 catch 一同使用。**                        |
  | var          | 声明一个变量。                                               |
  | while        | 当条件语句为 true 时，执行语句块。                           |

* 空格
  JavaScript 会忽略多余的空格

* 拆行（代码换行，运行结果不换行）
  在**文本字符串中**使用**反斜杠**对代码行进行换行

  ~~~JavaScript
  document.write("你好 \
  世界!");
  ~~~

