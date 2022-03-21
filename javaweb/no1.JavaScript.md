[toc]

# JavaScript

## 概念

* 需要运行浏览器来解析执行Javacript代码
* Js 是弱类型；Java 是强类型
* 特点：
  1. 交互性（信息的动态交互）
  2. 安全性（不允许直接访问本地硬盘）
  3. 跨平台性（只要可以解析JS的浏览器都可以执行，和平台无关）

## 运行 JavaScript 方式

* 第一种方式
   .html 文件的基础上，在 head / body 标签中，使用 script 标签写JavaScript代码

  > alert()
  > JavaScript语言提供的一个警告框函数。它可以接收任意类型的参数，这个参数就是警告框的提示信息

  ~~~html
  <!DOCTYPE html>
  <html lang="en">
  <head>
      <meta charset="UTF-8">
      <title>Title</title>
      <script type="text/javascript">
          alert("hello javaScript!");
      </script>
  </head>
  <body>
  
  </body>
  </html>
  ~~~

* 第二种方式

  .html 文件的基础上，使用 script 标签引入 + 单独 JavaScript 代码文件

  > 使用script引入外部的js文件来执行
  >             src 属性专门用来引入js文件路径（可以是相对路径，也可以是绝对路径）
  >
  > script标签可以用来定义js代码，也可以用来引入js文件
  > 但是，两个功能二选一使用。不能同时使用两个功能

  ~~~html
  <!DOCTYPE html>
  <html lang="en">
  <head>
      <meta charset="UTF-8">
      <title>Title</title>
      <script type="text/javascript" src="1.js"></script>
      <script type="text/javascript">
          alert("国哥现在可以帅了");
      </script>
  </head>
  <body>
  
  </body>
  </html>
  ~~~

## 基本语法

### 变量

| 变量类型 | 说明       |
| -------- | ---------- |
| number   | 数值类型   |
| string   | 字符串类型 |
| object   | 对象类型   |
| boolean  | 布尔类型   |
| function | 函数类型   |

| 特殊值    | 说明                                                  |
| --------- | ----------------------------------------------------- |
| undefined | 未定义，js变量未赋予初始值的时候，默认值都是undefined |
| null      | 空值                                                  |
| NaN       | 非数字，非数值。全称：Not a Number                    |

* 定义变量格式：
  `var 变量名;`
  `var 变量名 = 值;`

  > typeof()是JavaScript语言提供的一个函数。它可以取变量的数据类型返回

  ~~~html
  <!DOCTYPE html>
  <html lang="en">
  <head>
      <meta charset="UTF-8">
      <title>Title</title>
      <script type="text/javascript">
          var i;
          alert(i); // undefined
          i = 12;
          alert( typeof(i) ); // number
          i = "abc";
          alert( typeof(i) ); // String
  
          var a = 12;
          var b = "abc";
          alert( a * b ); // NaN是非数字，非数值。
      </script>
  </head>
  <body>
  
  </body>
  </html>
  ~~~

### 运算符号

#### 关系运算符

== 		等于是简单的做字面值的比较
===		除了做字面值的比较之外，还会比较两个变量的数据类型

~~~html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <script type="text/javascript">
        var a = "12";
        var b = 12;

        alert( a == b ); // true
        alert( a === b ); // false
    </script>
</head>
<body>

</body>
</html>
~~~

#### 逻辑运算符

* &&  且运算
  当表达式全为真的时候，返回最后一个表达式；当表达式有一个为假的时候，返回第一个为假的表达式的值

  ~~~html
  <!DOCTYPE html>
  <html lang="en">
  <head>
      <meta charset="UTF-8">
      <title>Title</title>
      <script type="text/javascript">
          var a = "abc";
          var b = true;
          var d = false;
          var c = null;
  
          alert( a && b );//true
          alert( b && a );//true
          alert( a && d ); // false
          alert( a && c ); // null
      </script>
  </head>
  <body>
  
  </body>
  </html>
  ~~~

* ||   或运算
  当表达式全为假时，返回最后一个表达式的值，只要有一个表达式为真，就会返回第一个为真的表达式的值

  ~~~html
  <!DOCTYPE html>
  <html lang="en">
  <head>
      <meta charset="UTF-8">
      <title>Title</title>
      <script type="text/javascript">
          var a = "abc";
          var b = true;
          var d = false;
          var c = null;
          
          alert( d || c ); // null
          alert( c|| d ); //false
          alert( a || c ); //abc
          alert( b || c ); //true
      </script>
  </head>
  <body>
  
  </body>
  </html>
  ~~~

【&& || 有短路，当这个&& 或||运算有结果了之后，后面的表达式不再执行】

* ！  取反运算

* 所有变量都可以作为一个Boolean类型的变量去使用：`0、null、undefined、"" 都认为是false`

  ~~~html
  <!DOCTYPE html>
  <html lang="en">
  <head>
      <meta charset="UTF-8">
      <title>Title</title>
      <script type="text/javascript">
          var a = 0;
          if (a) { 
              alert("零为真");
          } else {
              alert("零为假");
          }
  
          var b = null;
          if (b) {
             alert("null为真");
          } else {
             alert("null为假");
          }
  
          var c = undefined;
          if (c) {
              alert("undefined为真");
          } else {
              alert("undefined为假");
          }
  
          var d = "";
          if (d) {
              alert("空串为真");
          } else {
              alert("空串为假");
          }
      </script>
  </head>
  <body>
  
  </body>
  </html>
  ~~~

### 数组

* 定义格式：
  `var 数组名 = []; //空数组`
  `var 数组名 = [1,'abc',true]; //定义数组并赋值` 

  > javaScript语言中的数组，只要我们通过数组下标赋值，那么最大的下标值，就会自动的给数组做扩容操作。

  ~~~html
  <!DOCTYPE html>
  <html lang="en">
  <head>
      <meta charset="UTF-8">
      <title>Title</title>
      <script type="text/javascript">
          var arr = []; // 定义一个空数组
          alert( arr.length ); // 0
          arr[0] = 12;
          alert( arr[0] );//12
          alert( arr.length ); // 1
          arr[2] = "abc";
          alert(arr.length); //3
          alert(arr[1]);// undefined
          
          // 数组的遍历
          for (var i = 0; i < arr.length; i++){
              alert(arr[i]);
          }
      </script>
  </head>
  <body>
  </body>
  </html>
  ~~~

### 函数

* 方式一：function 关键字来定义函数
  `function 函数名（形参列表）{`
  		`函数体`
  `}`

  > 定义带有返回值的函数，在函数体内直接使用return语句返回值。 

  ~~~html
  <!DOCTYPE html>
  <html lang="en">
  <head>
      <meta charset="UTF-8">
      <title>Title</title>
      <script type="text/javascript">
          // 定义一个无参函数
          function fun(){
              alert("无参函数fun()被调用了");
          }
  		fun();// 函数调用
  
          // 定义一个有参函数
          function fun2(a ,b) {
              alert("有参函数fun2()被调用了 a=>" + a + ",b=>"+b);
          }
          fun2(12,"abc");
  
          // 定义带有返回值的函数
          function sum(num1,num2) {
              var result = num1 + num2;
              return result;
          }
          alert( sum(100,50) );
      </script>
  </head>
  <body>
  
  </body>
  </html>
  ~~~

* 方式二：
  `var 函数名 = function（形参列表）{ 函数体 }`

  ~~~html
  <!DOCTYPE html>
  <html lang="en">
  <head>
      <meta charset="UTF-8">
      <title>Title</title>
      <script type="text/javascript">
          var fun = function () {
              alert("无参函数");
          }
          fun();
  
          var fun2 = function (a,b) {
              alert("有参函数a=" + a + ",b=" + b);
          }
          fun2(1,2);
  
          var fun3 = function (num1,num2) {
              return num1 + num2;
          }
          alert( fun3(100,200) );
      </script>
  </head>
  <body>
  
  </body>
  </html>
  ~~~

注意：JS中函数的重载会直接覆盖掉上一次的定义

~~~html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <script type="text/javascript">
        function fun(a,b) {
            alert("有参函数fun(a,b)");
        }
        function fun() {
            alert("无参函数fun()");
        }
        fun(1,"ad");
    </script>
</head>
<body>

</body>
</html>
~~~

* arguments 隐形参数（只在function函数内）
  在function函数中不需要定义，但却可以直接用来获取所有参数的变量，即隐形参数。
  类似于 Java 中的可变长参数：public void fun(object ... args);

  ~~~html
  <!DOCTYPE html>
  <html lang="en">
  <head>
      <meta charset="UTF-8">
      <title>Title</title>
  
      <script type="text/javascript">
          function fun(a) {
              alert( arguments.length );//可看参数个数
  
              alert( arguments[0] );
              alert( arguments[1] );
              alert( arguments[2] );
  
              alert("a = " + a);
  
              for (var i = 0; i < arguments.length; i++){
                  alert( arguments[i] );
              }
  
              alert("无参函数fun()");
          }
          fun(1,"ad",true);
  
          //计算所有参数相加的和并返回
          function sum(num1,num2) {
              var result = 0;
              for (var i = 0; i < arguments.length; i++) {
                  if (typeof(arguments[i]) == "number") {
                      result += arguments[i];
                  }
              }
              return result;
          }
          alert( sum(1,2,3,4,"abc",5,6,7,8,9) );
      </script>
  </head>
  <body>
  
  </body>
  </html>
  ~~~

### 自定义对象

#### object 形式

* 定义：
  `var 变量名 = new object(); // 对象实例，空对象`
  `变量名.属性名 = 值; // 定义一个属性`
  `变量名.函数名 = function(){}; // 定义一个函数`

* 访问:
  `变量名.属性名 / 函数名();`

~~~html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <script type="text/javascript">
        // 对象的定义：
        var 变量名 = new Object();   // 对象实例（空对象）
        变量名.属性名 = 值;		  // 定义一个属性
        变量名.函数名 = function(){}  // 定义一个函数
        var obj = new Object();
        obj.name = "华仔";
        obj.age = 18;
        obj.fun = function () {
            alert("姓名：" + this.name + " , 年龄：" + this.age);
        }
        // 对象的访问：
        alert( obj.age );
        obj.fun();
    </script>
</head>
<body>

</body>
</html>
~~~



#### {} 花括号形式

* 定义:
  `var 变量名 = { //空对象`
  		`属性名：值, //定义一个属性`
  		`属性名：值, //定义一个属性`
  		`函数名：function(){} //定义一个函数`
  `};`

* 访问：
  `变量名.属性 / 函数名();`

~~~html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <script type="text/javascript">
        // 对象的定义：
        var obj = {
            name:"国哥",
            age:18,
            fun : function () {
                alert("姓名：" + this.name + " , 年龄：" + this.age);
            }
        };
        // 对象的访问：
        alert(obj.name);
        obj.fun();
    </script>
</head>
<body>

</body>
</html>
~~~

## 事件

事件是电脑输入设备与页面进行交互的响应

| 常用事件 | 事件名称         | 说明                                           |
| -------- | ---------------- | ---------------------------------------------- |
| onload   | 加载完成事件     | 页面加载完成之后，常用于做页面js代码初始化操作 |
| onclick  | 单击事件         | 常用于按钮点击响应操作                         |
| onblur   | 失去焦点事件     | 常用于输入框失去焦点之后验证其输入内容是否合法 |
| onchange | 内容发生改变事件 | 常用于下拉列表和输入框内容发生改变后操作       |
| onsubmit | 表单 提交事件    | 常用于表单提交前，验证所有表单项是否合法       |

| 事件注册 | 说明：告诉浏览器，当事件响应后要执行哪些操作代码             |
| -------- | ------------------------------------------------------------ |
| 静态注册 | 通过HTML标签的事件属性直接赋于事件响应后的代码               |
| 动态注册 | 通过js代码得到标签的dom对象，然后再通过dom对象.事件名 = function(){} 这种形式赋于事件响应后的代码 |

> 动态注册基本步骤：
>
> 1. 获取标签对象
> 2. 标签对象.事件名 = function(){}

* onload
  浏览器解析完页面之后就会自动触发的事件

  ~~~html
  <!-- 静态注册 -->
  <!DOCTYPE html>
  <html lang="en">
  <head>
      <meta charset="UTF-8">
      <title>Title</title>
      <script type="text/javascript">
          function onloadFun() {
              alert('静态注册onload事件，所有代码');
          }
      </script>
  </head>        
  <body onload="onloadFun();">
  
  </body>
  </html>
  ~~~

  ~~~html
  <!-- 动态注册 -->
  <!DOCTYPE html>
  <html lang="en">
  <head>
      <meta charset="UTF-8">
      <title>Title</title>
      <script type="text/javascript">
          window.onload = function () {
              alert("动态注册的onload事件");
          }
      </script>
  </head>
  <body>
  
  </body>
  </html>
  ~~~

  

* onclick

  ~~~html
  <!--静态注册-->
  <!DOCTYPE html>
  <html lang="en">
  <head>
      <meta charset="UTF-8">
      <title>Title</title>
      <script type="text/javascript">
          function onclickFun() {
              alert("静态注册onclick事件");
          }
      </script>
  </head>
  <body>
      <button onclick="onclickFun();">按钮1</button>
      <button id="btn01">按钮2</button>
  </body>
  </html>
  ~~~

  > document 是JavaScript语言提供的一个对象（文档）
  > getElementById通过id属性获取标签对象

  ~~~html
  <!-- 动态注册 -->
  <!DOCTYPE html>
  <html lang="en">
  <head>
      <meta charset="UTF-8">
      <title>Title</title>
      <script type="text/javascript">
          window.onload = function () {
              // 1 获取标签对象
              var btnObj = document.getElementById("btn01");
              alert( btnObj );
              // 2 通过标签对象.事件名 = function(){}
              btnObj.onclick = function () {
                  alert("动态注册的onclick事件");
              }
          }
      </script>
  </head>
  <body>
      <button id="btn01">按钮2</button>
  </body>
  </html>
  ~~~

  

* onblur

  > console是控制台对象，是由JavaScript语言提供，专门用来向浏览器的控制器打印输出， 用于测试使用
  > log() 是打印的方法

  ~~~html
  <!-- 静态注册 -->
  <!DOCTYPE html>
  <html lang="en">
  <head>
      <meta charset="UTF-8">
      <title>Title</title>
      <script type="text/javascript">
          function onblurFun() {
              console.log("静态注册失去焦点事件");
          }
      </script>
  </head>
  <body>
      用户名:<input type="text" onblur="onblurFun();"><br/>
  </body>
  </html>
  ~~~

  ~~~html
  <!-- 动态注册 -->
  <!DOCTYPE html>
  <html lang="en">
  <head>
      <meta charset="UTF-8">
      <title>Title</title>
      <script type="text/javascript">
          window.onload = function () {
              //1 获取标签对象
             var passwordObj = document.getElementById("password");
             alert(passwordObj);
              //2 通过标签对象.事件名 = function(){};
              passwordObj.onblur = function () {
                  console.log("动态注册失去焦点事件");
              }
          }
      </script>
  </head>
  <body>
      密码:<input id="password" type="text" ><br/>
  </body>
  </html>
  ~~~

  

* onchange

  ~~~html
  <!-- 静态注册 -->
  <!DOCTYPE html>
  <html lang="en">
  <head>
      <meta charset="UTF-8">
      <title>Title</title>
      <script type="text/javascript">
          function onchangeFun() {
              alert("女神已经改变了");
          }
      </script>
  </head>
  <body>
      请选择你心中的女神：
      <select onchange="onchangeFun();">
          <option>--女神--</option>
          <option>芳芳</option>
          <option>佳佳</option>
          <option>娘娘</option>
      </select>
  </body>
  </html>
  ~~~

  ~~~html
  <!-- 动态注册 -->
  <!DOCTYPE html>
  <html lang="en">
  <head>
      <meta charset="UTF-8">
      <title>Title</title>
  
      <script type="text/javascript">
          window.onload = function () {
              //1 获取标签对象
              var selObj = document.getElementById("sel01");
              alert( selObj );
              //2 通过标签对象.事件名 = function(){}
              selObj.onchange = function () {
                  alert("男神已经改变了");
              }
          }
      </script>
  </head>
  <body>
      请选择你心中的男神：
      <select id="sel01">
          <option>--男神--</option>
          <option>国哥</option>
          <option>华仔</option>
          <option>富城</option>
      </select>
  </body>
  </html>
  ~~~

  

* onsubmit

  > return false 可以阻止 表单提交

  ~~~html
  <!-- 静态注册 -->
  <!DOCTYPE html>
  <html lang="en">
  <head>
      <meta charset="UTF-8">
      <title>Title</title>
      <script type="text/javascript" >
          function onsubmitFun(){
              // 要验证所有表单项是否合法，如果，有一个不合法就阻止表单提交
              alert("静态注册表单提交事件----发现不合法");
              return flase;
          }
      </script>
  </head>
  <body>
      <form action="http://localhost:8080" method="get" onsubmit="return onsubmitFun();">
          <input type="submit" value="静态注册"/>
      </form>
  </body>
  </html>
  ~~~

  ~~~html
  <!DOCTYPE html>
  <html lang="en">
  <head>
      <meta charset="UTF-8">
      <title>Title</title>
      <script type="text/javascript" >
          window.onload = function () {
              //1 获取标签对象
              var formObj = document.getElementById("form01");
              //2 通过标签对象.事件名 = function(){}
              formObj.onsubmit = function () {
                  // 要验证所有表单项是否合法，如果，有一个不合法就阻止表单提交
                  alert("动态注册表单提交事件----发现不合法");
                  return false;
              }
          }
      </script>
  </head>
  <body>
      <form action="http://localhost:8080" id="form01">
          <input type="submit" value="动态注册"/>
      </form>
  </body>
  </html>
  ~~~

## DOM模型

全称是 Document Object Model 文档对象模型
简而言之，就是把文档中的标签、属性、文本转换成对象来管理

### Document对象

![image-20210901160005822](E:\软工\typora笔记\javaweb\JavaScript.assets\image-20210901160005822.png)

* Document理解：

  1. Document 它管理了所有的 HTML 文档内容。 

  2. document 它是一种树结构的文档。有层级关系。 

  3. 它让我们把所有的标签 都 对象化 

  4. 我们可以通过 document 访问所有的标签对象。

* Document对象中的方法

  | 方法                                    | 说明                                                         |
  | --------------------------------------- | ------------------------------------------------------------ |
  | document.getElementById(elementId)      | 通过标签的id属性查找标签dom对象,elementId是标签的id属性值    |
  | document.getElementsByName(elementName) | 通过标签的的name属性查找标签dom对象，elementName是标签的name属性值 |
  | document.getElementsByTagName(tagname)  | 通过标签名查找标签dom对象，tagname是标签名                   |
  | document.createElement(tagName)         | 通过给定的标签名，创建一个 标签对象。tagname是要创建的标签名 |

  优先顺序：getElementById() > getElementsByName >  getElementsByTagName()
  注意：前三个方法，一定要在页面加载完毕之后执行，才能查询到标签对象

* getElementById

  > test()方法
  > 用于测试某个字符串，是不是匹配我的规则 ，匹配就返回true。不匹配就返回false.
  >
  > innerHTML 属性
  > 表示起始标签和结束标签中的内容，这个属性可读，可写

  ~~~html
  <!DOCTYPE html>
  <html lang="en">
  <head>
      <meta charset="UTF-8">
      <title>Title</title>
      <script type="text/javascript" >
          /*
          * 当用户点击了较验按钮，要获取输出框中的内容。然后验证其是否合法
          * 验证的规则是：必须由字母，数字。下划线组成。并且长度是5到12位。
          * */
          function onclickFun() {
              // 1 当我们要操作一个标签的时候，一定要先获取这个标签对象。
              var usernameObj = document.getElementById("username");
              // [object HTMLInputElement] 它就是dom对象
              var usernameText = usernameObj.value;
              // 如何 验证 字符串，符合某个规则 ，需要使用正则表达式技术
              var patt = /^\w{5,12}$/;
              var usernameSpanObj = document.getElementById("usernameSpan");
  
              if (patt.test(usernameText)) {
                  usernameSpanObj.innerHTML = "<img src=\"right.png\" width=\"18\" height=\"18\">";
              } else {
                  usernameSpanObj.innerHTML = "<img src=\"wrong.png\" width=\"18\" height=\"18\">";
              }
          }
      </script>
  </head>
  <body>
      用户名：<input type="text" id="username" value="wzg"/>
      <span id="usernameSpan" style="color:red;">
      </span>
      <button onclick="onclickFun()">较验</button>
  </body>
  </html>
  ~~~

  

* getElementByName

  是根据 指定的name属性查询返回多个标签对象集合，这个集合的操作跟数组 一样，集合中每个元素都是dom对象，这个集合中的元素顺序是他们在html页面中从上到下的顺序

  > checked属性 
  > 表示复选框的选中状态。如果选中是true，不选中是false，这个属性可读，可写

  ~~~html
  <!DOCTYPE html>
  <html lang="en">
  <head>
      <meta charset="UTF-8">
      <title>Title</title>
      <script type="text/javascript">
          // 全选
          function checkAll() {
              var hobbies = document.getElementsByName("hobby");
              for (var i = 0; i < hobbies.length; i++){
                  hobbies[i].checked = true;
              }
          }
          //全不选
          function checkNo() {
              var hobbies = document.getElementsByName("hobby");
              for (var i = 0; i < hobbies.length; i++){
                  hobbies[i].checked = false;
              }
          }
          // 反选
          function checkReverse() {
              var hobbies = document.getElementsByName("hobby");
              for (var i = 0; i < hobbies.length; i++) {
                  hobbies[i].checked = !hobbies[i].checked;
  
                  // if (hobbies[i].checked) {
                  //     hobbies[i].checked = false;
                  // }else {
                  //     hobbies[i].checked = true;
                  // }
              }
          }
      </script>
  </head>
  <body>
      兴趣爱好：
      <input type="checkbox" name="hobby" value="cpp" checked="checked">C++
      <input type="checkbox" name="hobby" value="java">Java
      <input type="checkbox" name="hobby" value="js">JavaScript
      <br/>
      <button onclick="checkAll()">全选</button>
      <button onclick="checkNo()">全不选</button>
      <button onclick="checkReverse()">反选</button>
  </body>
  </html>
  ~~~

  

* getElementByTagName
  是按照指定标签名来进行查询并返回集合，这个集合的操作跟数组 一样，集合中都是dom对象，集合中元素顺序 是他们在html页面中从上到下的顺序。

  ~~~html
  <!DOCTYPE html>
  <html lang="en">
  <head>
      <meta charset="UTF-8">
      <title>Title</title>
      <script type="text/javascript">
          window.onload = function(){
              alert( document.getElementById("btn01") );
          }
  
          // 全选
          function checkAll() {
              var inputs = document.getElementsByTagName("input");
              for (var i = 0; i < inputs.length; i++){
                  inputs[i].checked = true;
              }
          }
      </script>
  </head>
  <body>
      兴趣爱好：
      <input type="checkbox" value="cpp" checked="checked">C++
      <input type="checkbox" value="java">Java
      <input type="checkbox" value="js">JavaScript
      <br/>
      <button id="btn01" onclick="checkAll()">全选</button>
  </body>
  </html>
  ~~~

  

* createElement
  使用js代码来创建html标签，并显示在页面上

  ~~~html
  <!DOCTYPE html>
  <html lang="en">
  <head>
      <meta charset="UTF-8">
      <title>Title</title>
      <script type="text/javascript">
          window.onload = function () {
              var divObj = document.createElement("div"); // 在内存中 <div></div>
              var textNodeObj = document.createTextNode("国哥，我爱你"); // 有一个文本节点对象：国哥，我爱你
              divObj.appendChild(textNodeObj); // <div>国哥，我爱你</div>
              
              //divObj.innerHTML = "国哥，我爱你"; // <div>国哥，我爱你</div>,但还只是在内存中
              
              // 添加子元素
              document.body.appendChild(divObj);
          }
      </script>
  </head>
  <body>
  
  </body>
  </html>
  ~~~

### 节点的常用属性和方法

节点就是标签对象

| 方法                   | 说明                                            |
| ---------------------- | ----------------------------------------------- |
| getElementsByTagName() | 获取当前节点的指定标签名孩子节点                |
| appendChild(ChildNode) | 可以添加一个子节点，ChildNode是要添加的孩子节点 |

| 属性           | 说明                                 |
| -------------- | ------------------------------------ |
| childNodes     | 获取当前节点的所有子节点             |
| firstChild     | 获取当前节点的第一个子节点           |
| lastChild      | 获取当前节点的最后一个子节点         |
| parentNode     | 获取当前节点的父节点                 |
| nextSibling    | 获取当前节点的下一个节点             |
| previousSibing | 获取当前节点的上一个节点             |
| className      | 获取或设置标签的class属性值          |
| innerHTML      | 获取或设置起始标签和结束标签中的内容 |
| innerText      | 获取或设置起始标签和结束标签中的文本 |

~~~html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>dom查询</title>
<link rel="stylesheet" type="text/css" href="style/css.css" />
<script type="text/javascript">
	window.onload = function(){
		//1.查找#bj节点
		document.getElementById("btn01").onclick = function () {
			var bjObj = document.getElementById("bj");
			alert(bjObj.innerHTML);
		};
        
		//2.查找所有li节点
		var btn02Ele = document.getElementById("btn02");
		btn02Ele.onclick = function(){
			var lis = document.getElementsByTagName("li");
			alert(lis.length)
		};
        
		//3.查找name=gender的所有节点
		var btn03Ele = document.getElementById("btn03");
		btn03Ele.onclick = function(){
			var genders = document.getElementsByName("gender");
			alert(genders.length)
		};
        
		//4.查找#city下所有li节点
		var btn04Ele = document.getElementById("btn04");
		btn04Ele.onclick = function(){
			var lis = document.getElementById("city").getElementsByTagName("li");
			alert(lis.length)
		};
        
		//5.返回#city的所有子节点
		var btn05Ele = document.getElementById("btn05");
		btn05Ele.onclick = function(){
			alert(document.getElementById("city").childNodes.length);
		};
        
		//6.返回#phone的第一个子节点
		var btn06Ele = document.getElementById("btn06");
		btn06Ele.onclick = function(){
			alert( document.getElementById("phone").firstChild.innerHTML );
		};
        
		//7.返回#bj的父节点
		var btn07Ele = document.getElementById("btn07");
		btn07Ele.onclick = function(){
			var bjObj = document.getElementById("bj")
			alert( bjObj.parentNode.innerHTML );
		};
        
		//8.返回#android的前一个兄弟节点
		var btn08Ele = document.getElementById("btn08");
		btn08Ele.onclick = function(){
			alert( document.getElementById("android").previousSibling.innerHTML );
		};
        
		//9.读取#username的value属性值
		var btn09Ele = document.getElementById("btn09");
		btn09Ele.onclick = function(){
			alert(document.getElementById("username").value);
		};
        
		//10.设置#username的value属性值
		var btn10Ele = document.getElementById("btn10");
		btn10Ele.onclick = function(){
			document.getElementById("username").value = "国哥你真牛逼";
		};
        
		//11.返回#bj的文本值
		var btn11Ele = document.getElementById("btn11");
		btn11Ele.onclick = function(){
			alert(document.getElementById("city").innerHTML);
			// alert(document.getElementById("city").innerText);
		};
	};
</script>
</head>
<body>
<div id="total">
	<div class="inner">
		<p>你喜欢哪个城市?</p>
		<ul id="city">
			<li id="bj">北京</li>
			<li>上海</li>
			<li>东京</li>
			<li>首尔</li>
		</ul>
		<br><br>
        
		<p>你喜欢哪款单机游戏?</p>
		<ul id="game">
			<li id="rl">红警</li>
			<li>实况</li>
			<li>极品飞车</li>
			<li>魔兽</li>
		</ul>
		<br /><br />

		<p>你手机的操作系统是?</p>
		<ul id="phone"><li>IOS</li><li id="android">Android</li><li>Windows Phone</li></ul>
	</div>

	<div class="inner">
		gender:
		<input type="radio" name="gender" value="male"/>Male
		<input type="radio" name="gender" value="female"/>Female
		<br><br>
        
		name:
		<input type="text" name="name" id="username" value="abcde"/>
	</div>
</div>
<div id="btnList">
	<div><button id="btn01">查找#bj节点</button></div>
	<div><button id="btn02">查找所有li节点</button></div>
	<div><button id="btn03">查找name=gender的所有节点</button></div>
	<div><button id="btn04">查找#city下所有li节点</button></div>
	<div><button id="btn05">返回#city的所有子节点</button></div>
	<div><button id="btn06">返回#phone的第一个子节点</button></div>
	<div><button id="btn07">返回#bj的父节点</button></div>
	<div><button id="btn08">返回#android的前一个兄弟节点</button></div>
	<div><button id="btn09">返回#username的value属性值</button></div>
	<div><button id="btn10">设置#username的value属性值</button></div>
	<div><button id="btn11">返回#bj的文本值</button></div>
</div>
</body>
</html>
~~~

