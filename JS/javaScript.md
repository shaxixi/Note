[toc]

# javaScript

## 概述

* JavaScript由**ECMAScript**（JavaScript语法标准）、**DOM**（文档对象模型）、**BOM**（浏览器对象模型）三部分组成。

* 书写位置：

  * 行内式：是将单行或少量的JavaScript代码写在HTML标签的事件属性中（以on开头的属性）

    ~~~html
    <body>
        <input type="button" value="点我" onclick="alert('行内式')" />
    </body>
    ~~~

  * 内嵌式（嵌入式）：使用<script>标签包裹JavaScript代码，<script>标签可以写在<head>或<body>标签中。

    ~~~html
    <head>
    	<meta charset="utf-8">
    	<title>内嵌式</title>
    	<script type="text/javascript">
    		alert("内嵌式") ;
    	</script>
    </head>
    ~~~

  * 外部式（外链式）：将JavaScript代码写在一个单独的文件中，一般使用“**js**”作为文件的扩展名

    ~~~html
    <head>
    	<title>外部式</title>
    	<script src="test.js"></script>
    </head>
    ~~~

* 输入输出语句

  * alert(msg) ：**浏览器**弹出警告框
    ![image-20220225144939204](E:\软工\typora笔记\JS\javaScript.assets\image-20220225144939204-16460949483601.png)
  * console.log(msg) ：**浏览器控制台**输出信息
    ![image-20220225145103147](E:\软工\typora笔记\JS\javaScript.assets\image-20220225145103147-16460949483613.png)
  * prompt(msg)：**浏览器**弹出输入框，用户可以输入内容，返回msg
    ![image-20220225145116288](E:\软工\typora笔记\JS\javaScript.assets\image-20220225145116288-16460949483602.png)
  * confirm(msg)： **浏览器**弹出确认框，用户选择并返回布尔值
    ![image-20220302102257244](E:\软工\typora笔记\JS\javaScript.assets\image-20220302102257244.png)

* 变量： ①只声明不赋值，输出为undefined。②不声明，输出报错。③不声明只赋值，输出正常。
  ①声明赋值 `var a; a = 10; `
  ②初始化 `var a = 10; `
  ③多个变量 `var a = 1, b = 2, c = 3;`
  
* 调试工具：
  在Chrome浏览器中，按**F12键启动开发者工具**后，切换到**“Sources”面板**。

  在**中栏**显示的网页源代码中，单击某一行的行号，即可**添加断点**，再次单击，可以取消断点。
  ![image-20220311081249754](E:\软工\typora笔记\JS\javaScript.assets\image-20220311081249754.png)

## 基础语法

### 数据类型

![image-20220228104948323](E:\软工\typora笔记\JS\javaScript.assets\image-20220228104948323.png)

* `var 变量` 定义数据类型   `typeof 变量` 检测数据类型
* 数字型
  `Number.MAX_VALUE`    数字型最大值        `Number.MIN_VALUE`     数字型最小值
  `Infinity`                     无穷大                    `-Infinity`                   无穷小
  `NAN`                               非数值                     `isNAN()`                       判断是否为非数值型
  ① 转为数字型： `parseInt()` 、`parseFloat()`、 `Number()`、 `- * /`
* 字符串型
  ① 单引号、双引号都可 `var str1 = '单引号字符串'; var str2 = "双引号字符串";`
  ② 单双引号相互嵌套，纯单纯双不能嵌套 
  `var str1 = 'I'm a programmer';` ×         `var str3 = 'I am a programmer";` ×
  ③ `length` 属性，获取长度        `[index]` 数组，获取字符值 index起始0
       `+`  字符串拼接，什么类型和字符串相加都为字符串
  ④ 转换成字符串：`+` 、 `toString()` 、  `String()` 
       \> null 和undefined 不懂使用 `toString()`
       \> 数字型可用 `toString()` 进行进制转换
* undefined 
  ① 只声明不赋值，则默认变量值为undefined
  ② undefined和数字型相加 ==> NAN
* null
  ① null和数字型相加 ==> null转为0,数字型
  ② null和布尔型相加 ==> null转为0，false转为0，true转为1
* 布尔型
  ① 转换成布尔型： `Boolean()` 
       \> 空字符串、0、NaN、null、undefined  ==>  false

### 运算符

* 比较运算符 
  `===` 全等，值和数据类型都要相等			`!==` 不全等，至少值或数据类型其中之一要相等
  \> 其他比较运算符都是比较值的大小
  
* 赋值运算符

  | **运算符** | **运算**           | **示例**          | **结果** |
  | ---------- | ------------------ | ----------------- | -------- |
  | <<=        | 左移位并赋值       | a =  9; a <<= 2;  | a =  36  |
  | >>=        | 右移位并赋值       | a =  -9; a >>= 2; | a =  -3  |
  | >>>=       | 无符号右移位并赋值 | a =  9; a >>>= 2; | a =  2   |
  | &=         | 按位“与”并赋值     | a =  3; a &= 9;   | a = 1    |
  | ^=         | 按位“异或”并赋值   | a =  3; a ^= 9;   | a =  10  |
  | \|=        | 按位“或”并赋值     | a =  3; a \|= 9;  | a =  11  |
  
* 运算符优先级

  | **结合方向** | **运算符**                                                   |
  | :----------: | ------------------------------------------------------------ |
  |      无      | ()                                                           |
  |      左      | .  []   new（有参数，无结合性）                              |
  |      右      | new（无参数）                                                |
  |      无      | ++（后置） --（后置）                                        |
  |      右      | !   ~  -（负数）   +（正数）  ++（前置）  --（前置）  typeof   void  delete |
  |      右      | **                                                           |
  |      左      | *    /   %                                                   |
  |      左      | +   -                                                        |
  |      左      | <<   >>    >>>                                               |
  |      左      | <  <=   >  >=  in  instanceof                                |
  |      左      | ==   !=   ===  !==                                           |
  |      左      | &                                                            |
  |      左      | ^                                                            |
  |      左      | \|                                                           |
  |      左      | &&                                                           |
  |      左      | \|\|                                                         |
  |      右      | ?:                                                           |
  |      右      | =   +=  =  *= /=  %=    <<=  >>=    >>>=   &=   ^=   \|=     |
  |      左      | ,                                                            |

### 数组

* 创建数组：[] 或 new Array()
* 获取数组长度： arr.length
  修改数组长度： arr.length = n
  `arr1 = [1,2,3];  arr1.length=2 ==> arr1 = [1,2]`
  `arr2 = [1,2,3];  arr2.length=4 ==> arr2 = [1,2,3,undefined]`
* 创建二维数组：new Array( new Array() , new Array() ) 或 [ [] , [] ]

### 函数

* 声明函数：`function 函数名(){函数体}`

  > 函数的形参和实参可以个数不同：
  > 	实参 > 形参 ：多余的实参会被忽略
  > 	实参 < 形参 ：多出来的形参类似于已声明为赋值，值为undefined
  >
  > ~~~javascript
  > function getSum(num1, num2) {
  >   console.log(num1, num2);
  > }
  > getSum(1, 2, 3);	// 实参数量大于形参数量，输出结果：1 2
  > getSum(1);           // 实参数量小于形参数量，输出结果：1 undefined
  > ~~~
  >
  > 当不确定函数接受了多少个实参时，可用arguments[n]获取实参
  >
  > ~~~javascript
  > function fn() {
  >   console.log(arguments);	  // 输出结果：Arguments(3) [1, 2, 3, …]
  >   console.log(arguments.length);  // 输出结果：3
  >   console.log(arguments[1]);         // 输出结果：2
  > }
  > fn(1, 2, 3);
  > ~~~

* 调用函数：`函数名()`

* 函数表达式：`var sum = function() {}`
  调用函数：`sum()`
  \> 函数表达式的定义必须在调用前

* 回调函数：

  ~~~javascript
  function cal(num1, num2, fn) {
    return fn(num1, num2);
  }
  console.log(cal(45, 55, function (a, b) {
    return a + b;
  }));
  console.log(cal(10, 20, function (a, b) {
    return a * b;
  }));
  ~~~

  \> 一个函数A作为参数传递给一个函数B，然后在B的函数体内调用函数A。此时，称函数A为回调函数

