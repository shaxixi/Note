[toc]

# 基础编程

## end关键字

* 可以用于将结果输出到同一行，或者在输出的末尾添加不同的字符

  ~~~ python 
  a, b = 0, 1
  while b < 1000:
      print(b, end=',')
      a, b = b, a+b
  #输出结果：
  #1,1,2,3,5,8,13,21,34,55,89,144,233,377,610,987,
  ~~~

## 条件控制

~~~python
#if-else-if
if condition_1:
    statement_block_1
elif condition_2:
    statement_block_2
else:
    statement_block_3
~~~

> 每个条件后面要使用冒号`:`，表示接下来是满足条件后要执行的语句块
>
> 使用缩进来划分语句块，相同缩进数的语句在一起组成一个语句块
>
> 在python中没有`switch-case`

~~~python
#if-if-else-else嵌套
if 表达式1:
    语句
    if 表达式2:
        语句
    elif 表达式3:
        语句
    else:
        语句
elif 表达式4:
    语句
else:
    语句
~~~

## 循环语句

* while循环

  ~~~python
  while 判断条件(condition)：
  执行语句(statements)……
  ~~~

  > 无限循环：可以通过表达式永远不为FALSE来实现无限循环；退出用`ctrl+c`

* while循环使用else语句	为false时执行else的语句块

  ~~~python
  while <expr>:
      <statement(s)>
  else:
      <additional_statement(s)>
  ~~~
  > 如果while循环体中只有一条语句，可以将该语句与while写在同一行中
  >
  > `while (flag): print ('欢迎访问菜鸟教程!')`

* for语句

  ~~~python
  for <variable> in <sequence>:	#如果<variable>是变量，则头开始索引
      <statements>
  else:
      <statements>
  ~~~
  > ~~~python 
  > sites = ["Baidu", "Google","Runoob","Taobao"]
  > for site in sites:					#注意site和sites实现遍历的
  >    	if site == "Runoob":
  >            print("菜鸟教程!")
  >            break
  >            print("循环数据 " + site)
  >     else:
  >    		print("没有循环数据!")
  > print("完成循环!")
  > ~~~



## range()函数

* 如果遍历数字序列，可以使用内置range()函数，它会生成数列。

  ~~~python
  >>>for i in range(5):
  ...     print(i)
  ...
  0
  1
  2
  3
  4
  ~~~

* 使用range指定区间的值

  ~~~python
  >>>for i in range(5,9) :
  ...    print(i)    
  ...
  5
  6
  7
  8
  ~~~

* 可以使range以指定数字开始并指定不同的增量(甚至可以是负数，有时这也叫做'步长'):

  ~~~python
  >>>for i in range(0, 10, 3) :
  ...    print(i)  
  ...
  0
  3
  6
  9
  ~~~

  ~~~python
  >>>for i in range(-10, -100, -30) :
  ...    print(i)  
  ...
  -10
  -40
  -70
  ~~~

* 可以结合range()和len()函数以遍历一个序列的索引

  ~~~python
  >>>a = ['Google', 'Baidu', 'Runoob', 'Taobao', 'QQ']
  >>> for i in range(len(a)):
  ...     print(i, a[i])
  ... 
  0 Google
  1 Baidu
  2 Runoob
  3 Taobao
  4 QQ
  ~~~

* 可以使用range()函数来创建一个列表

  ~~~python
  >>>list(range(5))
  [0, 1, 2, 3, 4]
  ~~~

## break continue

* 循环语句可以有 else 子句，它在穷尽列表(以for循环)或条件变为 false (以while循环)导致循环终止时被执行，但循环**被 break 终止时不执行**。

## pass语句

* python pass 是空语句，是为了保持程序结构的完整性；pass 不做任何事情，一般用做占位语句

## 迭代器

* 迭代是Python最强大的功能之一，是访问集合元素的一种方式。

  迭代器是一个可以记住遍历的位置的对象。

  迭代器对象从集合的第一个元素开始访问，直到所有的元素被访问完结束。迭代器只能往前不会后退。

* 迭代器有两个基本的方法：**iter()** 和 **next()**。

~~~python
'''iter() 和 next()'''
>>> list=[1,2,3,4]
>>> it = iter(list)    # 创建迭代器对象
>>> print (next(it))   # 输出迭代器的下一个元素
1
>>> print (next(it))
2

'''for in 和 iter()'''
>>>list=[1,2,3,4]
>>>it = iter(list)    # 创建迭代器对象
>>>for x in it:
>>>    print (x, end=" ")
1 2 3 4

'''for in 和 next() iter()'''
>>> list=[1,2,3,4]
>>> it=iter(list)
>>> for x in list:
	print(next(it),end=" ")
1 2 3 4
~~~

* 在类中实 **\_\_iter\_\_()** 与 **\_\_next\_\_()** 两个方法

~~~python
>>> class mynumber(object):
	def __iter__(self):
		self.a=1
		return self
	def __next__(self):		#**
		x=self.a
		self.a+=1
		return x

	
>>> myclass = mynumber()
>>> myiter = iter(myclass)
>>> print(next(myiter))
1
>>> print(next(myiter))
2
>>> print(next(myiter))
3
>>> print(next(myiter))
4

'''------------------------疑惑---------------------------'''
>>> class mynumber(object):
	def __iter__(self):
		self.a=1
		x=self.a
		self.a+=1
		return x

	
>>> myiter = iter(myclass)
>>> print(next(myiter))
1
>>> print(next(myiter))
2
>>> print(next(myiter))
3
>>> print(next(myiter))
4
~~~

* **StopIteration** 异常——用于标识迭代的完成，防止出现无线循环的情况

~~~python
>>> class mynumber(object):
	def __iter__(self):
		self.a=1
		return self
	def __next__(self):
		if self.a <=20:
			x=self.a
			self.a+=1
			return x
		else:
			raise StopIteration		#**

		
>>> myclass = mynumber()
>>> myiter = iter(myclass)
>>> for x in myiter:
	print(next(myiter),end=" ")		#**

	
2 4 6 8 10 12 14 16 18 20 
>>> for x in myiter:
	print(x,end=" ")	#**

	
1 2 3 4 5 6 7 8 9 10 11 12 13 14 15 16 17 18 19 20 
>>> for x in myclass:
	print(next(myiter),end=" ")		#**

	
2 4 6 8 10 12 14 16 18 20 
>>> for x in myclass:
	print(x,end=" ")	#**

	
1 2 3 4 5 6 7 8 9 10 11 12 13 14 15 16 17 18 19 20 
~~~

## 生成器—— yield 的函数

* 在调用生成器运行的过程中，每次遇到 yield 时函数会暂停并保存当前所有的运行信息，返回 yield 的值（调用一个生成器函数，返回的是一个迭代器对象）, 并在下一次执行 next() 方法时从当前位置继续运行。

~~~python
>>> def fibonacci(n):
	a,b,counter = 0,1,0
	while True:
		if (counter > n):
			return
		yield a		#** 相当于一个个存数据，存到iter()中
		a = b
		b += a
		counter += 1

>>> f = fibonacci(10)
>>> for x in f:
	print(x,end = " ")

	
0 1 2 4 8 16 32 64 128 256 512 

'''----------------a = b ;b += a 区别于 a,b = b,a+b------------------------'''
>>> def fibonacci(n):
	a,b,counter = 0,1,0
	while True:
		if (counter > n):
			return
		yield a
		a,b = b,a+b
		counter += 1

		
>>> f = fibonacci(10)
>>> for x in f:
	print(x,end = " ")

	
0 1 1 2 3 5 8 13 21 34 55 
~~~

## 函数

* 语法

  > def 函数名 （参数列表）：
  >
  > ​	函数体
  * 函数的第一行语句可以选择性地使用文档字符串—用于存放函数说明。
  * 函数内容以冒号 **:** 起始，并且缩进。
  * **return [表达式]** 结束函数，选择性地返回一个值给调用方，不带表达式的 return 相当于返回 None。
~~~python
>>> def area(width,height):
	return width * height

>>> def print_welcome(name):
	print("welcome",name)

	
>>> print_welcome(boom)
Traceback (most recent call last):
  File "<pyshell#129>", line 1, in <module>
    print_welcome(boom)
NameError: name 'boom' is not defined
>>> print_welcome("boom")
welcome boom
>>> a = 10
>>> b = 12
>>> print("width = ",a," height = ",b,"area = ",area(a,b))
width =  10  height =  12 area =  120
~~~
* 函数调用

  > 函数名 （实参列表）

~~~python
>>> def hello():
	print("hello word")

	
>>> hello()		#**
hello word
~~~

* 参数传递 （不懂，过）

* 参数

  * 必须参数——须以**正确的顺序传入函数**。调用时的**数量必须和声明时的一样**。

    ~~~python
    def printme( str ):
       print (str)
       return
    
    '''调用函数'''
    printme()	# × 无参数
    printme( "str" )	# √ 有参数且数量一样
    ~~~

  * 关键字参数——传入的参数值，允许函数调用时**参数的顺序与声明时不一致**，因为 Python 解释器能够用参数名匹配参数值。

    ~~~python
    def printinfo( name, age ):
       print ("名字: ", name)
       print ("年龄: ", age)
       return
     
    '''调用函数'''
    printinfo( age=50, name="runoob" )		# 入参顺序不一致
    ~~~

  * 默认参数——调用函数时，如果没有传递参数，则会使用默认参数（在定义函数中定义）

    ~~~python
    def printinfo( name, age = 35 ):	#** 定义了age的默认参数
       print ("名字: ", name)
       print ("年龄: ", age)
       return
     
    '''调用函数'''
    printinfo( age=50, name="runoob" )
    printinfo( name="runoob" )		#** 默认参数age
    ~~~

  * 不定长参数——一个函数能处理比当初声明时更多的参数，声明时不会命名。

    * 加了星号 ***** 的参数会以 **元组(tuple)** 的形式导入，存放所有未命名的变量参数。

    ~~~python
    def printinfo( arg1, *vartuple ):	#** vartuple可能有多值
       print ("输出: ")
       print (arg1)
       print (vartuple)
     
    '''调用函数'''
    printinfo( 70, 60, 50 )		#** age1:70		vartuple:60,50
    
    '''-------我们也可以不向函数传递未命名的变量,它就是一个空元组。-------'''
    def printinfo( arg1, *vartuple ):
       print ("输出: ")
       print (arg1)
       for var in vartuple:
          print (var)
       return
     
    '''调用函数'''
    printinfo( 10 )		#** arg1:10		vartuple: 空
    printinfo( 70, 60, 50 )		#** arg1:70		vartuple:60,50
    ~~~

    * 加了两个星号 ***\*** 的参数会以 **字典** 的形式导入,存放所有未命名的变量参数。

      ~~~python
      def printinfo( arg1, **vardict ):
         print ("输出: ")
         print (arg1)
         print (vardict)
       
      '''调用函数'''
      printinfo(1, a=2,b=3)	#** arg1:1		vardict:a=2,b=3
      ~~~

    * 单独出现星号 *** 后** 的 参数 必须**用关键字** 传入。

      ~~~python
      def f(a,b,*,c):
           return a+b+c
       
      '''调用函数'''
      f(1,2,3)   #** ×	*,c): 	要用关键字
      f(1,2,c=3) #** √
      ~~~

    *  单独出现 **/ 前**  的形参必须**使用指定位置参数**，**不能使用关键字参数**的形式。

      ~~~python
      def f(a, b, /, c, d, *, e, f):
          print(a, b, c, d, e, f)
          
      '''调用函数'''
      f(10, 20, 30, d=40, e=50, f=60)		#** √
      f(10, b=20, c=30, d=40, e=50, f=60)   #** × b 不能使用关键字参数的形式
      f(10, 20, 30, 40, 50, f=60)           #** × e 必须使用关键字参数的形式
      ~~~

      

* 匿名函数 —— lambda 来创建匿名函数

  ~~~python
  sum = lambda arg1, arg2: arg1 + arg2
   
  # 调用sum函数
  print ("相加后的值为 : ", sum( 10, 20 ))
  print ("相加后的值为 : ", sum( 20, 20 ))
  ~~~

  - lambda的主体是一个表达式，而不是一个代码块。仅仅能在lambda表达式中封装有限的逻辑进去。
  - lambda 函数拥有自己的命名空间，且不能访问自己参数列表之外或全局命名空间里的参数。

* eval（） —— 将字符串转化成表达式