[toc]

# vue——js框架

* 下载和安装
  http://cn.vuejs.org
  学习-> 教程-> 安装-> 开发版本-> 复制代码-> 网页开发工具粘贴-> 应用框架

## 基础语法

* 引入vue: `<script src="https://cdn.jsdelivr.net/npm/vue@2/dist/vue.js"></script>`

* 创建vue实例：

  ~~~vue
  <script>
      var vm = new Vue({
        // 选项
      })
  </script>
  ~~~

### data 和 function

> ① 当一个 Vue 实例被创建时，它将 `data` 对象中的所有的 property 加入到 Vue 的响应式系统中。当这些 property 的值发生改变时，视图将会产生“响应”，即匹配更新为新的值。
>
> ② 只有当实例被创建时就已经存在于 `data` 中的 property 才是**响应式**的。也就是说如果你添加一个新的 property，那么对 `b` 的改动将不会触发任何视图的更新。
>
> ③ `Object.freeze()`，这会阻止修改现有的 property，也意味着响应系统无法再追踪变化。
>
> ④ Vue定义的property和function可用前缀 `$`，以便与用户定义的 property 区分开来。

~~~vue
<script>
    var data = { a: 1 }
    var vm = new Vue({
      data: data // 相当于：data: { a: 1 }， vm.a == data.a => true
    })
    
    vm.a = 2 // data.a => 2    
    data.a = 3 // vm.a => 3
    
    Object.freeze(vm);
    vm.a = 2 // data.a => 1
    
    vm.$data === data // => true
</script>
~~~

### 实例生命周期钩子

> 不应该使用箭头函数 `() =>`来定义一个生命周期方法 。

[^原因]:所有生命周期钩子的 `this` 上下文将自动绑定至实例中，因此你可以访问 data、computed 和 methods。因为箭头函数绑定了父级上下文，所以 `this` 不会指向预期的组件实例，并且`this.fetchTodos` 将会是 undefined。

| 钩子函数 | 说明 |
| -------- | ---- |
|          |      |

### 插值

> 绝不要对用户提供的内容使用插值。

[^原因]:因为它很容易导致 [XSS 攻击](https://en.wikipedia.org/wiki/Cross-site_scripting)。请只对可信内容使用 HTML 插值

~~~vue
<body>
    <!-- 1. 文本 数据改变即内容更新 {{ }} -->
    <span>Message: {{ msg }}</span>
    <!-- 2. 文本 一次插入并永不更新 v-once -->
    <span v-once>这个将不会改变: {{ msg }}</span>
    
    <!-- 3. HTML ""中的值为vue对象中的property v-html="" -->
    <p>Using v-html directive: <span v-html="msg"></span></p>
    
    <!-- 4. Attribute ""中的值为vues中的property v-bind: 或 ：-->
    <div v-bind:id="dynamicId"></div>
    
	<!-- 特殊布尔Attribute：如果 isButtonDisabled 的值是 null、undefined 或 false，则 disabled attribute 甚至不会被包含在渲染出来的 <button> 元素中。 -->
    <button v-bind:disabled="isButtonDisabled">Button</button>
</body>
~~~

> 对于所有的数据绑定，Vue.js 都提供了完全的 JavaScript 表达式支持。
> ① 每个绑定都只能包含单个表达式,不能包含语句。
> ② 不应该在模板表达式中试图访问用户定义的全局变量。

~~~vue
<body>
    <div v-bind:id="'list-' + id"></div>
    {{ number + 1 }}
	{{ ok ? 'YES' : 'NO' }}
	{{ message.split('').reverse().join('') }}
    
    <!-- 这是语句，不是表达式 -->
    {{ var a = 1 }}
    <!-- 流控制也不会生效，请使用三元表达式 -->
    {{ if (ok) { return message } }}
</body>
~~~

### 指令 `v-`

> 指令 (Directives) 是带有 `v-` 前缀的特殊 attribute。

~~~vue
<body>
    <!-- v-bind 指令 href 参数（属性） url property -->
    <a v-bind:href="url">...</a>
    <!-- v-bind 指令 [attributeName] 动态参数（property），url property -->
    <a v-bind:[attributeName]="url"> ... </a>
</body>
~~~

> 动态参数约束：
> ① 动态参数预期会求出一个字符串，异常情况下值为 `null`。这个特殊的 `null` 值可以被显性地用于移除绑定。
> ② 任何其它非字符串类型的值都将会触发一个警告。
>
> 动态参数表达式的约束：
> ① 某些字符，如**空格和引号**，放在 HTML attribute 名里是无效的。
> ② 避免使用大写字符来命名键名，因为浏览器会把 attribute 名**全部强制转为小写**

### 修饰符  `.`

> 修饰符 (modifier) 是以半角句号 `.` 指明的特殊后缀，用于指出一个指令应该以特殊方式绑定。

~~~vue
<body>
    <!-- v-on 指令对于触发的事件调用 event.preventDefault() -->
    <form v-on:submitcprevent="onSubmit">...</form>
</body>
~~~

### 缩写

> `v-bind:` ==> `:`
> `v-on:`  ==> `@`

~~~vue
<body>
    <!-- 完整语法 -->
    <a v-bind:href="url">...</a>
    <!-- 缩写 -->
    <a :href="url">...</a>
    <!-- 动态参数的缩写 (2.6.0+) -->
    <a :[key]="url"> ... </a>
    
    <!-- 完整语法 -->
	<a v-on:click="doSomething">...</a>
    <!-- 缩写 -->
    <a @click="doSomething">...</a>
    <!-- 动态参数的缩写 (2.6.0+) -->
    <a @[event]="doSomething"> ... </a>
</body>
~~~

### 计算属性 `computed`

> ① 对于任何复杂逻辑，你都应当使用计算属性。模板的初衷是用于简单运算的。
>
> ② 计算属性是基于它们的响应式依赖进行**缓存**的。VS 方法 ：调用方法将总会再次执行函数。
>
> ③ 计算属性简约。VS 侦听属性是命令式且重复的。

~~~vue
<body>
    <!-- 没使用计算属性前：在模板中放入太多的逻辑会让模板过重且难以维护。 -->
    <div id="example">
      {{ message.split('').reverse().join('') }}
    </div>
    
    <!-- 使用计算属性后 -->
    <div id="example">
      <p>Original message: "{{ message }}"</p>
      <p>Computed reversed message: "{{ reversedMessage }}"</p>
    </div>
</body>
<script>
    var vm = new Vue({
      el: '#example',
      data: {
        message: 'Hello'
      },
      computed: {
        // 默认：计算属性的 getter
        reversedMessage: function () {
            return this.message.split('').reverse().join('')
        }
          
       // getter and setter
       reversedMessage: {
          get: function () {
            return this.message.split('').reverse().join('')
    	  },
          set: function (newValue){
              this.message = newValue.split('').reverse().join('')
          }
        }   
      }
    })
</script>
~~~

### 侦听器  `watch` 

> 当需要在数据变化时执行异步或开销较大的操作时，这个方式是最有用的。

~~~vue
<script>
    var vm = new Vue({
      el: '#demo',
      data: {
        firstName: 'Foo',
        lastName: 'Bar',
        fullName: 'Foo Bar'
      },
      watch: {
        firstName: function (val) {
          this.fullName = val + ' ' + this.lastName
        },
        lastName: function (val) {
          this.fullName = this.firstName + ' ' + val
        }
      }
    })
</script>

<body>
    <div id="demo">{{ fullName }}</div>
</body>
~~~

### Class  `v-bind:class` 

> ① 值可为 对象 `{}` 、数组 `[]` 、字符串 `""`
>
> ② Class绑定时，property值只能为 truthiness 或 falseness
>
> ③ 在**自定义组件**上使用 class property 时，这些 class 将被添加到该组件的**根元素**上面。这个元素上已经存在的 class **不会被覆盖**。

~~~vue
<body>
    <!-- 对象绑定 -->
    <!-- 1. {class值：property}-->
    <div v-bind:class="{ active: isActive }"></div>
    
    <!-- 2. property 
		可以在这里绑定一个返回对象的计算属性 -->
    <div v-bind:class="classObject"></div>
    
    
	<!-- 3. {字符串 ：property}
		v-bind:class 指令也可以与普通的 class attribute 共存-->
    <div
      class="static"
      v-bind:class="{ active: isActive, 'text-danger': hasError }"
    ></div>
    
    
    <!-- 数组绑定 -->
    <div v-bind:class="[activeClass, errorClass]"></div>
    
    <!-- 1. 可以用三元表达式 -->
    <div v-bind:class="[isActive ? activeClass : '', errorClass]"></div>
    
    <!-- 2. 可以使用对象语法 -->
    <div v-bind:class="[{ active: isActive }, errorClass]"></div>
    
    <!-- vue组件 -->
    <my-component class="baz"></my-component>
    <my-component v-bind:class="{ baz: isActive }"></my-component>
    <!-- 渲染结果: <p class="foo bar baz">Hi</p> -->
    
</body>
<script>
	var vm = new vue({
        data: {
          isActive: true,
          hasError: false,
          classObject: { // 方式一：绑定属性
            active: true,
            'text-danger': false
          }
        },
        computed: {
          classObject: function () {  // 方式二：绑定计算属性
            return {
              active: this.isActive && !this.error,
              'text-danger': this.error && this.error.type === 'fatal'
            }
          }
        }
    });
    Vue.component('my-component', {
      template: '<p class="foo bar">Hi</p>'
    })
</script>
~~~

### Style `v-bind:style`

> 自动添加前缀：
> \> 当 `v-bind:style` 使用需要添加[浏览器引擎前缀](https://developer.mozilla.org/zh-CN/docs/Glossary/Vendor_Prefix)的 CSS property 时，Vue.js 会自动侦测并添加相应的前缀。
> 多重值：
> \> 可以为 `style` 绑定中的 property 提供一个包含多个值的数组，这样写只会渲染数组中最后一个被浏览器支持的值。

~~~vue
<body>
    <!-- 对象 同上 --> 
    <div v-bind:style="{ color: activeColor, fontSize: fontSize + 'px' }"></div>
    <div v-bind:style="styleObject"></div>
    
    <!-- 数组 同上 -->
    <div v-bind:style="[baseStyles, overridingStyles]"></div>
    
    <div :style="{ display: ['-webkit-box', '-ms-flexbox', 'flex'] }"></div>   
</body>
<script>
	var vm = new vue({
        data: {
          activeColor: 'red',
 		  fontSize: 30,
          styleObject: { // 对象语法常常结合返回对象的计算属性使用。
            color: 'red',
            fontSize: '13px'
          }
        }
    })
</script>
~~~

### 条件渲染 `v-if` `v-else` `v-else-if`

> `v-else` 元素必须紧跟在带 `v-if` 或者 `v-else-if` 的元素的后面，否则它将不会被识别。
> `v-else-if` 也必须紧跟在带 `v-if` 或者 `v-else-if` 的元素之后。

~~~vue
<!-- if使用 -->
<h1 v-if="awesome">Vue is awesome!</h1>

<!-- if-else使用 -->
<div v-if="Math.random() > 0.5">
    Now you see me
</div>
<div v-else>
    Now you don't
</div>

<!-- if-else-if使用 -->
<div v-if="type === 'A'">
    A
</div>
<div v-else-if="type === 'B'">
    B
</div>
<div v-else-if="type === 'C'">
    C
</div>
<div v-else>
    Not A/B/C
</div>
~~~

> ① 切换多个元素：可以把一个 `<template>` 元素当做不可见的包裹元素，并在上面使用 `v-if`。最终的渲染结果将不包含 `<template>` 元素。
>
> ② 管理可复用的元素：两个元素是完全独立的，不要复用(Vue 会尽可能高效地渲染元素，通常会复用已有元素而不是从头开始渲染)。只需添加一个具有唯一值的 `key` attribute 即可.

~~~vue
<!-- template 和 if 的搭配 -->
<template v-if="loginType === 'username'">
  <label>Username</label>
  <input placeholder="Enter your username">
</template>
<template v-else>
  <label>Email</label>
  <input placeholder="Enter your email address">
</template>
<!-- 那么在上面的代码中切换 loginType 将不会清除用户已经输入的内容。因为两个模板使用了相同的元素，<input> 不会被替换掉——仅仅是替换了它的 placeholder。 -->

<!-- key使用 -->
<template v-if="loginType === 'username'">
  <label>Username</label>
  <input placeholder="Enter your username" key="username-input">
</template>
<template v-else>
  <label>Email</label>
  <input placeholder="Enter your email address" key="email-input">
</template>
<!-- <input>元素不复用， <label> 元素仍然会被高效地复用 -->
~~~

> ① `v-if` vs `v-show`
> `v-show` 只是简单地切换元素的 CSS property `display`。`v-show` 不支持 `<template>` 元素，也不支持 `v-else`。
> `v-if` 会确保在切换过程中条件块内的事件监听器和子组件适当地被销毁和重建。如果在初始渲染时条件为假，则什么也不做——直到条件第一次变为真时，才会开始渲染条件块。
>
> ② 当 `v-if` 与 `v-for` 一起使用时。
> 	1）`v-for` 具有比 `v-if` 更高的优先级，这意味着 `v-if` 将分别重复运行于每个 `v-for` 循环中。当你只想为*部分*项渲染节点时，这种优先级的机制会十分有用。
> 	2）如果你的目的是有条件地跳过循环的执行，那么可以将 `v-if` 置于外层元素 (或 [`) 上。

### 列表渲染 `v-for`

> 格式：
> ① v-for="item in items" 	遍历数组，（被迭代的数组元素的别名）
> ② v-for="(item, index) in items"	遍历数组，（被迭代的数组元素的别名，当前项的索引）
> ③ v-for="value in object"          遍历对象，（属性）
> ④ v-for="(value, name) in object"              遍历对象，（值名，键名）
> ⑤ v-for="(value, name, index) in object"        遍历对象，（值名，键名，当前属性的索引）
> ⑥ v-for="n in 10"     遍历次数，（当前次数）

~~~vue
<ul id="example-2">
  <li v-for="(item, index) in items">
    <!-- 1. 可以访问所有父作用域的 property -->
    {{ parentMessage }} - {{ index }} - {{ item.message }}
  </li>
    
  <!-- 2. 每项提供一个唯一 key attribute:
	   为了给 Vue 一个提示，以便它能跟踪每个节点的身份，从而重用和重新排序现有元素 -->
  <div v-for="item in items" v-bind:key="item.id">
  </div>  
</ul>
  
<!-- 3. 可以利用带有 v-for 的 <template> 来循环渲染一段包含多个元素的内容 -->
<ul>
    <template v-for="item in items">
        <li>{{ item.msg }}</li>
        <li class="divider" role="presentation"></li>
    </template>
</ul>

<script>
    var example2 = new Vue({
      el: '#example-2',
      data: {
        parentMessage: 'Parent',
        items: [
          { message: 'Foo' },
          { message: 'Bar' }
        ]
      }
    })
</script>
~~~

> 可以用 `of` 替代 `in` 作为分隔符
>
> 不要使用对象或数组之类的非基本类型值作为 `v-for` 的 `key`。请用字符串或数值类型的值。

### 数组方法

\> 变更方法（改变原数组）： `push()` `pop()` `shift()` `unshift()` `splice()` `sort()` `reverse()`
\> 替换数组（总是返回一个新数组）： `filter()` `concat()`  `slice()`

~~~vue
<li v-for="n in evenNumbers">{{ n }}</li>

<ul v-for="set in sets">
    <!-- 嵌套计算属性用法 -->
  <li v-for="n in even(set)">{{ n }}</li>
</ul>

<script>
    new vue(){
        data: {
          numbers: [ 1, 2, 3, 4, 5 ]
        },
            // 可以通过计算属性返回一个新数组，即不失去原数组也保留新数组
        computed: {
          evenNumbers: function () {
            return this.numbers.filter(function (number) {
              return number % 2 === 0
            })
          }
        }        
    },

	new vue(){
        data: {
          sets: [[ 1, 2, 3, 4, 5 ], [6, 7, 8, 9, 10]]
        },
        methods: {
          even: function (numbers) {
            return numbers.filter(function (number) {
              return number % 2 === 0
            })
          }
        }        
    }    
</script>
~~~

