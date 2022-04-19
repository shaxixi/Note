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

### 对象

* 创建对象方式：① 字面量 ② new Object ③ 构造函数

* 对象格式： `key:value`  ==> `属性名/方法名：对应的值`

  ~~~javascript
  // 1. 字面量
  var 对象名={
  	属性名：属性值，
  	属性名：属性值，
  	……
  	方法名：function(){
      }
  }
  
  // 手动赋值属性或方法来添加成员
  var 对象名 = {};
  对象名.属性名 = '属性值';	
  对象名.方法名 = function() { 
    alert('My 属性名 is ' + this.属性名);	// this代表当前对象
  };	
  
  // 2. new Object
  var obj = new Object();	
  obj.key1 = 'value';		
  obj.key2 = value;
  obj.key3 = 'value';
  obj.function1 = function() {
    console.log('Hello');
  };
  
  // 3. 构造函数创建对象 
  function 构造函数名(参数1，参数2，... ) {
    this.属性 = 属性值;
    this.方法 = function() {
      // 方法体
    }
  }
  var obj = new 构造函数名();
  ~~~

* 访问对象

  > 当对象成员中包含特殊字符时，可以用字符串来表示

  ~~~javascript
  // 访问对象的属性
  object.dataName
  object['dataName']
  // 访问对象的方法
  object.functionName()    
  object.['functionName']() 
  // 特殊字符
  var obj = {
  'name-age': '小明-18'
  };
  console.log(obj['name-age']);  // dataName是特殊字符时常用格式
  ~~~

* 遍历对象的属性和方法 `for (... in ...)`

  ~~~javascript
  // obj为待遍历的对象
  var obj = { name: '小明', age: 18, sex: '男' };
  // 遍历obj对象
  for (var k in obj) {
    // 通过k可以获取遍历过程中的属性名或方法名
    console.log(k);	             // 依次输出属性名
    console.log(obj[k]); 	// 依次输出属性值
  }
  ~~~

* in运算符,判断一个对象中的某个成员是否存在。

  ~~~javascript
  var obj = {name: 'Tom', age: 16};
  console.log('age' in obj);	    // 输出：true，表示对象成员存在
  console.log('gender' in obj);  // 输出：false ，表示对象成员不存在
  ~~~

## 内置对象

查阅JavaScript中的内置对象
MDN：https://developer.mozilla.org/zh-CN/

* Math：不需要实例化对象，用来对数字进行与数学相关的运算
  | **成员**            | **功能**                                                 |
  | ------------------- | -------------------------------------------------------- |
  | PI                  | 获取圆周率，结果为3.141592653589793                      |
  | abs(x)              | 获取x的绝对值，可传入普通数值或是用字符串表示的数值      |
  | max()               | 获取所有参数中的最大值                                   |
  | min()               | 获取所有参数中的最小值                                   |
  | pow(base, exponent) | 获取基数（base）的指数（exponent）次幂，即  baseexponent |
  | sqrt(x)             | 获取x的平方根                                            |
  | ceil(x)             | 获取大于或等于x的最小整数，即向上取整                    |
  | floor(x)            | 获取小于或等于x的最大整数，即向下取整                    |
  | round(x)            | 获取x的四舍五入后的整数值                                |
  | random()            | 获取大于或等于0.0且小于1.0的随机值                       |

* Data：需要实例化对象，Date()是日期对象的构造函数

  ~~~javascript
  // 方式1：没有参数
  var date1 = new Date();  
  // 输出：Wed Oct 16 2019 10:57:56 GMT+0800 (中国标准时间)
  
  // 方式2：传入年、月、日、时、分、秒（月的范围是0~11）
  var date2 = new Date(2019, 10, 16, 10, 57, 56);
  // 输出：Sat Nov 16 2019 10:57:56 GMT+0800 (中国标准时间)
  
  // 方式3：用字符串表示日期和时间
  var date3 = new Date('2019-10-16 10:57:56');
  // 输出：Wed Oct 16 2019 10:57:56 GMT+0800 (中国标准时间)
  ~~~

  | **方法**                | **作用**                                                |
  | ----------------------- | ------------------------------------------------------- |
  | getFullYear()           | 获取表示年份的4位数字，如2020                           |
  | getMonth()              | 获取月份，范围0~11（0表示一月，1表示二月，依次类推）    |
  | getDate()               | 获取月份中的某一天，范围1~31                            |
  | getDay()                | 获取星期，范围0~6（0表示星期日，1表示星期一，依次类推） |
  | getHours()              | 获取小时数，返回0~23                                    |
  | getMinutes()            | 获取分钟数，范围0~59                                    |
  | getSeconds()            | 获取秒数，范围0~59                                      |
  | getMilliseconds()       | 获取毫秒数，范围0~999                                   |
  | getTime()  \| valueOf() | 获取时间戳，Date.now() 也可                             |
  |                         |                                                         |

  > “+”运算符转换为数值型：`+new Date();`  ==> 返回时间戳

* 数组类型检测：instanceof运算符 和 使用Array.isArray()方法

  ~~~javascript
  var arr = [];
  var obj = {};
  // 第1种方式
  console.log(arr instanceof Array);	// 输出结果：true
  console.log(obj instanceof Array);	// 输出结果：false
  // 第2种方式
  console.log(Array.isArray(arr));	// 输出结果：true
  console.log(Array.isArray(obj));	// 输出结果：false
  ~~~

  | **方法名**      | **功能描述**                                                 | **返回值**             |
  | --------------- | ------------------------------------------------------------ | ---------------------- |
  | push(参数1…)    | 数组末尾添加一个或多个元素，会修改原数组                     | 返回数组的新长度       |
  | unshift(参数1…) | 数组开头添加一个或多个元素，会修改原数组                     | 返回数组的新长度       |
  | pop()           | 删除数组的最后一个元素，若是空数组则返回undefined，会修改原数组 | 返回删除的元素的值     |
  | shift()         | 删除数组的第一个元素，若是空数组则返回undefined，会修改原数组 | 返回第一个元素的值     |
  | reverse()       | 颠倒数组中元素的位置，该方法会改变原数组                     | 返回新数组的长度       |
  | sort()          | 对数组的元素进行排序，该方法会改变原数组                     | 返回新数组的长度       |
  | indexOf()       | 返回在数组中可以找到给定值的第一个索引，检索方式与运算符“===”相同，即只有全等时才会返回比较成功的结果。 | 如果不存在，则返回-1   |
  | lastIndexOf()   | 返回指定元素在数组中的最后一个的索引，检索方式与运算符“===”相同，即只有全等时才会返回比较成功的结果。 | 如果不存在则返回-1     |
  | toString()      | 把数组转换为字符串，逗号分隔每一项                           |                        |
  | join('分隔符')  | 将数组的所有元素连接到一个字符串中                           |                        |
  | fill()          | 用一个固定值填充数组中指定下标范围内的全部元素  ，在执行后皆会对原数组产生影响。 |                        |
  | splice()        | 数组删除，参数为splice(第几个开始, 要删除个数)，在执行后皆会对原数组产生影响。 | 返回被删除项目的新数组 |
  | slice()         | 数组截取，参数为slice(begin, end)                            | 返回被截取项目的新数组 |
  | concat()        | 连接两个或多个数组，不影响原数组                             | 返回一个新数组  *      |

* 字符串：new String()来创建

  ~~~javascript
  // 方式一：
  var str = new String('apple');
  // 方式二：
  var str = new String();
  ~~~

  | **方法**                                                     | **功能描述**                                                 |
  | ------------------------------------------------------------ | ------------------------------------------------------------ |
  | indexOf(searchValue)   \| indexOf(searchValue，index)        | 获取searchValue在字符串中首次出现的位置                      |
  | lastIndexOf(searchValuex)   \| lastIndexOf(searchValue，index) | 获取searchValue在字符串中最后出现的位置                      |
  | charAt(index)                                                | 获取index位置的字符，位置从0开始计算                         |
  | charCodeAt(index)                                            | 获取index位置的字符的ASCII码                                 |
  | str[index]                                                   | 获取指定位置处的字符（HTML5新增）                            |
  | concat(str1,  str2, str3…)                                   | 连接多个字符串                                               |
  | slice(start,[ end])                                          | 截取从start位置到end位置之间的一个子字符串                   |
  | substring(start[, end])                                      | 截取从start位置到end位置之间的一个子字符串，基本和slice相同，但是不接收负值 |
  | substr(start[,  length])                                     | 截取从start位置开始到length长度的子字符串                    |
  | toLowerCase()                                                | 获取字符串的小写形式                                         |
  | toUpperCase()                                                | 获取字符串的大写形式                                         |
  | split([separator[, limit])                                   | 使用separator分隔符将字符串分隔成数组，limit用于限制数量     |
  | replace(str1, str2)                                          | 使用str2替换字符串中的str1，返回替换结果，只会替换第一个字符 |
  
  ~~~javascript
  var str = 'HelloWorld';
  str.concat('!');  // 在字符串末尾拼接字符，结果：HelloWorld!
  str.slice(1, 3);   // 截取从位置1开始包括到位置3的范围内的内容，结果：el
  str.substring(5);      // 截取从位置5开始到最后的内容，结果：World
  str.substring(5, 7);  // 截取从位置5开始到位置7范围内的内容，结果：Wo
  str.substr(5);           // 截取从位置5开始到字符串结尾的内容，结果：World
  str.substring(5, 7);  // 截取从位置5开始到位置7范围内的内容，结果：Wo
  str.toLowerCase();  // 将字符串转换为小写，结果：helloworld
  str.toUpperCase();  // 将字符串转换为大写，结果：HELLOWORLD
  str.split('l');	  // 使用“l”切割字符串，结果：["He", "", "oWor", "d"]
  str.split('l', 3);	  // 限制最多切割3次，结果：["He", "", "oWor"]
  str.replace('World', '!'); // 替换字符串，结果："Hello!"
  ~~~
  
* 值类型和引用类型
  \> 引用类型的特点是，变量中保存的仅仅是一个引用的地址，当对变量进行赋值时，并不是将对象复制了一份，而是将两个变量指向了同一个对象的引用。
  ![image-20220408081544515](E:\软工\typora笔记\JS\javaScript.assets\image-20220408081544515.png)
  \> 当obj1和obj2两个变量指向了同一个对象后，如果给其中一个变量（如obj1)重新赋值为其他对象，或者重新赋值为其他值，则obj1将不再引用原来的对象，但obj2仍然在引用原来的对象。

  ~~~javascript
  var obj1 = { name: '小明', age: 18 };
  var obj2 = obj1;
  // obj1指向了一个新创建的对象
  obj1 = { name: '小红', age: 17 };
  // obj2仍然指向原来的对象
  console.log(obj2.name);		// 输出结果：小明
  ~~~

  \> 当一个对象只被一个变量引用的时候，如果这个变量又被重新赋值，则该对象就会变成没有任何变量引用的情况，这时候就会由JavaScript的垃圾回收机制自动释放。
  \> 如果在函数的参数中修改对象的属性或方法，则在函数外面通过引用这个对象的变量访问时的结果也是修改后的。

  ~~~javascript
  function change(obj) {
    obj.name = '小红';	// 在函数内修改了对象的属性
  }
  var stu = { name: '小明', age: 18 };
  change(stu);
  console.log(stu.name);	// 输出结果：小红
  ~~~

  
