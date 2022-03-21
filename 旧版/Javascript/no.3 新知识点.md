

## 类型转换

*  JavaScript 数据类型

  6 种不同的数据类型： string number boolean object function symbol

  3 种对象类型： Object Date Array

  2 个不包含任何值的数据类型： null undefined

  ~~~javascript
  typeof "John"                 // 返回 string
  typeof 3.14                   // 返回 number
  typeof NaN                    // 返回 number
  typeof false                  // 返回 boolean
  typeof [1,2,3,4]              // 返回 object
  typeof {name:'John', age:34}  // 返回 object
  typeof new Date()             // 返回 object
  typeof function () {}         // 返回 function
  typeof myCar                  // 返回 undefined (如果 myCar 没有声明)
  typeof null                   // 返回 object
  
  /* 注意：
  NaN 的数据类型是 number
  数组(Array)的数据类型是 object
  日期(Date)的数据类型为 object
  null 的数据类型是 object
  未定义变量的数据类型为 undefined */
  ~~~

* constructor 属性
  返回所有 JavaScript 变量的构造函数。

  ~~~javascript
  "John".constructor                 // 返回函数 String()  { [native code] }
  (3.14).constructor                 // 返回函数 Number()  { [native code] }
  false.constructor                  // 返回函数 Boolean() { [native code] }
  [1,2,3,4].constructor              // 返回函数 Array()   { [native code] }
  {name:'John', age:34}.constructor  // 返回函数 Object()  { [native code] }
  new Date().constructor             // 返回函数 Date()    { [native code] }
  function () {}.constructor         // 返回函数 Function(){ [native code] }
  ~~~

  ~~~html
  <!-- 实例 -->
  <p id="demo"></p>
  <script>
  var fruits = ["Banana", "Orange", "Apple", "Mango"];
  document.getElementById("demo").innerHTML = isArray(fruits);
  function isArray(myArray) {
      return myArray.constructor.toString().indexOf("Array") > -1;
  }
  </script>
  ~~~

* 一元运算符 +
  可用于将变量转换为数字,如果变量不能转换，它仍然会是一个数字，但值为 NaN (不是一个数字)

  ~~~javascript
  var y = "5";      // y 是一个字符串
  var x = + y;      // x 是一个数字
  var y = "John";   // y 是一个字符串
  var x = + y;      // x 是一个数字 (NaN)
  ~~~

* 类型转化
Number() 转换为数字， String() 转换为字符串， Boolean() 转换为布尔值。
  
* 日期转字符串：1. 全局方法 **String()**  2. Date 方法 **toString()**  3. Date() 
  
  * 日期转数字：1. 全局方法 **Number()**   2. 日期方法 **getTime()** 

  * 自动转换为字符串： 
    尝试输出一个对象或一个变量时 JavaScript 会自动调用变量的 toString() 方法
  
    ~~~JavaScript
    document.getElementById("demo").innerHTML = myVar;
    
    myVar = {name:"Fjohn"}  // toString 转换为 "[object Object]"
    myVar = [1,2,3,4]       // toString 转换为 "1,2,3,4"
    myVar = new Date()      // toString 转换为 "Fri Jul 18 2014 09:08:55 GMT+0200"
    myVar = 123             // toString 转换为 "123"
    myVar = true            // toString 转换为 "true"
    myVar = false           // toString 转换为 "false"
    ~~~
  
    下表展示了使用不同的数值转换为数字(Number), 字符串(String), 布尔值(Boolean):
  
    | 原始值              | 转换为数字 | 转换为字符串      | 转换为布尔值 |
    | :------------------ | :--------- | :---------------- | :----------- |
    | false               | 0          | "false"           | false        |
    | true                | 1          | "true"            | true         |
    | 0                   | 0          | "0"               | false        |
    | 1                   | 1          | "1"               | true         |
    | "0"                 | 0          | "0"               | **true**     |
    | "000"               | 0          | "000"             | **true**     |
    | "1"                 | 1          | "1"               | true         |
    | NaN                 | NaN        | "NaN"             | false        |
    | Infinity            | Infinity   | "Infinity"        | true         |
    | -Infinity           | -Infinity  | "-Infinity"       | true         |
    | ""                  | **0**      | ""                | **false**    |
    | "20"                | 20         | "20"              | true         |
    | "Runoob"            | NaN        | "Runoob"          | true         |
    | [ ]                 | **0**      | ""                | true         |
    | [20]                | **20**     | "20"              | true         |
    | [10,20]             | NaN        | "10,20"           | true         |
    | ["Runoob"]          | NaN        | "Runoob"          | true         |
    | ["Runoob","Google"] | NaN        | "Runoob,Google"   | true         |
    | function(){}        | NaN        | "function(){}"    | true         |
    | { }                 | NaN        | "[object Object]" | true         |
    | null                | **0**      | "null"            | false        |
    | undefined           | NaN        | "undefined"       | false        |

  * 自动转换类型
    当 JavaScript 尝试操作一个 "错误" 的数据类型时，会自动转换为 "正确" 的数据类型。
  
    ~~~JavaScript
    5 + null    // 返回 5         null 转换为 0
    "5" + null  // 返回"5null"   null 转换为 "null"
  "5" + 1     // 返回 "51"      1 转换为 "1" 
    "5" - 1     // 返回 4         "5" 转换为 5
    ~~~
  

## 正则表达式

## 错误

## 严格模式

