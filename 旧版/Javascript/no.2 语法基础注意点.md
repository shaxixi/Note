[toc]

## 语法基础注意点

### 变量

* Value = undefined
  在计算机程序中，经常会声明无值的变量。未使用值来声明的变量，其值实际上是 undefined。

  ~~~javascript
  var carname;
  // 变量 carname 的值将是 undefined  
  ~~~

* 重新声明 JavaScript 变量
  如果重新声明 JavaScript 变量，该变量的值不会丢失

  ~~~javascript
  var carname="Volvo";
  var carname;
  //在以下两条语句执行后，变量 carname 的值依然是 "Volvo" 
  ~~~

* 局部变量
  在 JavaScript 函数内部声明的变量（使用 var）是*局部*变量，所以只能在函数内部访问它。（该变量的作用域是局部的）。

  您可以在不同的函数中使用名称相同的局部变量，因为只有声明过该变量的函数才能识别出该变量。

  只要函数运行完毕，本地变量就会被删除。

* 全局变量
  在函数外声明的变量是*全局*变量，网页上的所有脚本和函数都能访问它。

* 向未声明的变量分配值
  如果您把值赋给尚未声明的变量，该变量将被自动作为 window 的一个属性。
  非严格模式下给未声明变量赋值创建的全局变量，是全局对象的可配置属性，可以删除。

  ~~~javascript
  var var1 = 1; // 不可配置全局属性
  var2 = 2; // 没有使用 var 声明，可配置全局属性
  
  console.log(this.var1); // 1
  console.log(window.var1); // 1
  console.log(window.var2); // 2
  
  delete var1; // false 无法删除
  console.log(var1); //1
  
  delete var2; 
  console.log(delete var2); // true
  console.log(var2); // 已经删除 报错变量未定义
  ~~~

* 作用域

  * 局部作用域

    ~~~html
    <p id="demo"></p>
    <script>
        myFunction();
        document.getElementById("demo").innerHTML = "carName 的类型是：" +  typeof carName;
        function myFunction() 
        {
            var carName = "Volvo";
        }
    </script>
    ~~~

  * 全局作用域

    ~~~html
    <p id="demo"></p>
    <script>
        var carName = "Volvo";
        myFunction();
        function myFunction() 
        {
            document.getElementById("demo").innerHTML =
                "我可以显示 " + carName;
        }
    </script>
    
    <!-- 如果变量在函数内没有声明（没有使用 var 关键字），该变量为全局变量。 -->
    <p id="demo"></p>
    <script>
    myFunction();
    document.getElementById("demo").innerHTML =
    	"我可以显示 " + carName;
    function myFunction() 
    {
        carName = "Volvo";
    }
    </script>
    ~~~

  * HTML中的全局变量
    在 HTML 中, 全局变量是 window 对象: 所有数据变量都属于 window 对象。

    ~~~html
    <p id="demo"></p>
    <script>
    myFunction();
    document.getElementById("demo").innerHTML =
    	"我可以显示 " + window.carName;
    function myFunction() 
    {
        carName = "Volvo";
    }
    </script>
    ~~~

  * 变量提升
    函数及变量的声明都将被提升到函数的最顶部，变量可以在使用后声明，也就是变量可以先使用再声明。变量提升：函数声明和变量声明总是会被解释器悄悄地被"提升"到方法体的最顶部。
    只有声明的变量会提升，初始化的不会。
  
    ~~~html
    <p id="demo"></p>
    <script>
    x = 5; // 变量 x 设置为 5
    elem = document.getElementById("demo"); // 查找元素 
    elem.innerHTML = x;                     // 在元素中显示 x
    var x; // 声明 x
    </script>
    
    <p id="demo"></p>
    <script>
    var x; // 声明 x
    x = 5; // 变量 x 设置为 5
    elem = document.getElementById("demo"); // 查找元素 
    elem.innerHTML = x; 
    </script>
    
    <!-- 只有声明的变量会提升，初始化的不会。 -->
    <p id="demo"></p>
    <script>
    var x = 5; // 初始化 x
    var y = 7; // 初始化 y
    elem = document.getElementById("demo"); // 查找元素 
    elem.innerHTML = x + " " + y;           // 显示 x 和 y
    </script>
    
    <p id="demo"></p>
    <script>
    var x = 5; // 初始化 x
    elem = document.getElementById("demo"); // 查找元素 
    elem.innerHTML = "x 为：" + x + "，y 为：" + y;           // 显示 x 和 y
    var y = 7; // 初始化 y
    </script>
    ~~~
  
    

### 数据类型

**字符串 数字 布尔 数组 对象 Undefined** 

* JavaScript 拥有动态类型
  这意味着相同的变量可用作不同的类型

    ~~~javascript
  var x;               // x 为 undefined
  var x = 5;           // 现在 x 为数字
  var x = "John";      // 现在 x 为字符串
    ~~~

* #### JavaScript 字符串

  可以在字符串中使用引号，只要不匹配包围字符串的引号即可

  * 可以在字符串中使用引号，字符串中的引号不要与字符串的引号相同

    ~~~javascript
    var answer = "It's alright";
    var answer = "He is called 'Johnny'";
    var answer = 'He is called "Johnny"';
    <!-- 除非用转义字符 -->
    var x = 'It\'s alright';
    var y = "He is called \"Johnny\"";    
    ~~~

  * 可以使用索引位置来访问字符串中的每个字符

    ~~~javascript
    var carname = "Volvo XC60";
    var carname = 'Volvo XC60';
    <!-- 访问字符 -->
    var character = carname[7];
    ~~~

  * 使用内置属性 **length** 来计算字符串的长度

    ~~~javascript
    var txt = "ABCDEFGHIJKLMNOPQRSTUVWXYZ";
    var sln = txt.length;
    ~~~

  * 转义字符

    | 代码 | 输出        |
    | :--- | :---------- |
    | \\'  | 单引号      |
    | \\"  | 双引号      |
    | \\\  | 反斜杠      |
    | \n   | 换行        |
    | \r   | 回车        |
    | \t   | tab(制表符) |
    | \b   | 退格符      |
    | \f   | 换页符      |

  * 字符串可以是对象

    ~~~javascript
    var x = "John";
    var y = new String("John");
    typeof x // 返回 String
    typeof y // 返回 Object
    <!-- 不要创建 String 对象。它会拖慢执行速度，并可能产生其他副作用： -->
    var x = "John";             
    var y = new String("John");
    (x === y) // 结果为 false，因为 x 是字符串，y 是对象
    /* === 为绝对相等，即数据类型与值都必须相等。 */
    ~~~

  * 字符串属性

    | 属性        | 描述                       |
    | :---------- | :------------------------- |
    | constructor | 返回创建字符串属性的函数   |
    | length      | 返回字符串的长度           |
    | prototype   | 允许您向对象添加属性和方法 |

  * 字符串方法

    | 方法                | 描述                                                         |
    | :------------------ | :----------------------------------------------------------- |
    | charAt()            | 返回指定索引位置的字符                                       |
    | charCodeAt()        | 返回指定索引位置字符的 Unicode 值                            |
    | concat()            | 连接两个或多个字符串，返回连接后的字符串                     |
    | fromCharCode()      | 将 Unicode 转换为字符串                                      |
    | indexOf()           | 返回字符串中检索指定字符第一次出现的位置                     |
    | lastIndexOf()       | 返回字符串中检索指定字符最后一次出现的位置                   |
    | localeCompare()     | 用本地特定的顺序来比较两个字符串                             |
    | match()             | 找到一个或多个正则表达式的匹配                               |
    | replace()           | 替换与正则表达式匹配的子串                                   |
    | search()            | 检索与正则表达式相匹配的值                                   |
    | slice()             | 提取字符串的片断，并在新的字符串中返回被提取的部分           |
    | split()             | 把字符串分割为子字符串数组                                   |
    | substr()            | 从起始索引号提取字符串中指定数目的字符                       |
    | substring()         | 提取字符串中两个指定的索引号之间的字符                       |
    | toLocaleLowerCase() | 根据主机的语言环境把字符串转换为小写，只有几种语言（如土耳其语）具有地方特有的大小写映射 |
    | toLocaleUpperCase() | 根据主机的语言环境把字符串转换为大写，只有几种语言（如土耳其语）具有地方特有的大小写映射 |
    | toLowerCase()       | 把字符串转换为小写                                           |
    | toString()          | 返回字符串对象值                                             |
    | toUpperCase()       | 把字符串转换为大写                                           |
    | trim()              | 移除字符串首尾空白                                           |
    | valueOf()           | 返回某个字符串对象的原始值                                   |

* #### JavaScript 数组

  ~~~javascript
  // 三种表示方法  
  var cars=new Array();
  cars[0]="Saab";
  cars[1]="Volvo";
  cars[2]="BMW";
  // 注意 new Array()  
  var cars=new Array("Saab","Volvo","BMW");
  //  直接赋值  
  var cars=["Saab","Volvo","BMW"];
  ~~~

* #### JavaScript 对象

  ~~~javascript
  // 定义 
  var person={firstname:"John", lastname:"Doe", id:5566};
  //寻址，两种方式
  name=person.lastname;
  name=person["lastname"];
  ~~~

* Undefined 和 Null
  Undefined 这个值表示变量不含有值。可以通过将变量的值设置为 null 来清空变量。

* 声明变量类型
  可以使用关键词 "new" 来声明其类型

  ~~~javascript
  var carname=new String;
  var x=      new Number;
  var y=      new Boolean;
  var cars=   new Array;
  var person= new Object;
  ~~~

### 对象

JavaScript 对象是拥有属性和方法的数据。 JavaScript 对象是属性和方法的容器。

* 对象也是一个变量，但对象可以包含多个值（多个变量）。

* 属性

  * 对象属性
    通常认为 "JavaScript 对象是键值对的容器"。键值对通常写法为 **name : value** (键与值以冒号分割)。键值对在 JavaScript 对象通常称为 **对象属性**。

  * 访问对象属性

    ~~~javascript
    //对象属性
    var person={firstname:"John", lastname:"Doe", id:5566};
    //访问对象属性
    person.lastname;
    person["lastname"];
    ~~~

* 对象方法
  对象的方法定义了一个函数，并作为对象的**属性存储**。

  ~~~javascript
  methodName : function() {
      // 代码 
  }
  ~~~

* 访问对象方法
  对象方法通过添加 () 调用 (作为一个函数)。
  如果使用 属性不添加 **()**, 它会返回函数的定义

  ~~~html
  <p id="demo1"></p>
  <p id="demo2"></p>
  <script>
      var person = {
          firstName: "John",
          lastName : "Doe",
          id : 5566,
          fullName : function() 
          {
             return this.firstName + " " + this.lastName;
          }
      };
      <!-- person.fullName Vs person.fullName() -->
      document.getElementById("demo1").innerHTML = "不加括号输出函数表达式："  + person.fullName;
      document.getElementById("demo2").innerHTML = "加括号输出函数执行结果："  +  person.fullName();
  </script>
  
  <!-- 结果：
  不加括号输出函数表达式：function() { return this.firstName + " " + this.lastName; }
  
  加括号输出函数执行结果：John Doe -->
  ~~~

### 事件

* 常见的HTML事件

  | 事件        | 描述                         |
  | :---------- | :--------------------------- |
  | onchange    | HTML 元素改变                |
  | onclick     | 用户点击 HTML 元素           |
  | onmouseover | 用户在一个HTML元素上移动鼠标 |
  | onmouseout  | 用户从一个HTML元素上移开鼠标 |
  | onkeydown   | 用户按下键盘按键             |
  | onload      | 浏览器已完成页面的加载       |

* HTML 元素中可以添加事件属性，使用 JavaScript 代码来添加 HTML 元素。

  ~~~html
  <!-- 按钮元素中添加了 onclick 属性 -->
  <button onclick="getElementById('demo').innerHTML=Date()">现在的时间是?</button>
  <!-- 修改自身元素的内容 -->
  <button onclick="this.innerHTML=Date()">现在的时间是?</button>
  <!-- JavaScript代码通常是几行代码。比较常见的是通过事件属性来调用 -->
  <button onclick="displayDate()">现在的时间是?</button>
  ~~~

### 循环

* JavaScript标签 （类似于switch)

  ~~~html
  cars=["BMW","Volvo","Saab","Ford"];
  list: 
  {
      document.write(cars[0] + "<br>"); 
      document.write(cars[1] + "<br>"); 
      document.write(cars[2] + "<br>"); 
      break list;
      document.write(cars[3] + "<br>"); 
      document.write(cars[4] + "<br>"); 
      document.write(cars[5] + "<br>"); 
  }
  ~~~

* if：在常规的比较中，数据类型是被忽略的
  switch：会使用恒等计算符(===)进行比较

  ~~~javascript
  // 以下 if 条件语句返回 true：
  var x = 10;
  var y = "10";
  if (x == y)
  // 以下实例由于类型不一致不会执行 alert 弹窗： 
  var x = 10;
  switch(x) {
      case "10": alert("Hello");
  }
  ~~~

  

### typeof null undefined

* typeof
  你可以使用 typeof 操作符来检测变量的数据类型。

  ~~~javascript
  typeof "John"                // 返回 string
  typeof 3.14                  // 返回 number
  typeof false                 // 返回 boolean
  typeof [1,2,3,4]             // 返回 object
  typeof {name:'John', age:34} // 返回 object
  ~~~

* null Vs undefined

  ~~~javascript
  var person = null;           // 值为 null(空), 但类型为对象
  var person = undefined;     // 值为 undefined, 类型为 undefined
  var person;                  // 值为 undefined(空), 类型是undefined
  typeof undefined             // undefined
  typeof null                  // object
  null === undefined           // false
  null == undefined            // true
  ~~~

### 数字

* 浮点型数据
  JavaScript 中的所有数据都是以 64 位浮点型数据(float) 来存储。

  ~~~javascript
  var x = 0.1;
  var y = 0.2;
  var z = x + y            // z 的结果为 0.30000000000000004
  if (z == 0.3)            // 返回 false
      
  //为解决以上问题，可以用整数的乘除法来解决：
  var z = (x * 10 + y * 10) / 10;       // z 的结果为 0.3    
  ~~~

  



