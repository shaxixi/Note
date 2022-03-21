[toc]



# 基础语法



## 多行语句



* 用反斜杠 '\' 来实现     // 为了使代码工整，运行结果是没有影响的

~~~python
total = "item_one +" \
​    "item_two + "\
​    "item_three"

# 输出结果：
# Item_one +item_two +item_three
~~~

* 在[ ],{ },( )中的多行语句不需要反斜杠，直接回车换行

 ~~~python
total = ['item_one', 'item_two', 'item_three',
​    'item_four', 'item_five']
 ~~~



## 多个变量赋值

 

 ~~~python
a = b = c = 1
a, b, c = 1, 2, "runoob" 

# 输出结果：
# a=1,b=2,c='runoob'
 ~~~



## print输出

 

* 不换行：变量末尾加上end=" "

 ~~~python
print( x, end=" " )
print( y, end=" " )
 ~~~



## Number（数字）

 

* 查询变量类型:

~~~python
type() # 返回数据类型 
isinstance(变量, 数据类型) # 返回bool 
~~~

* 删除变量（从内存中彻底删除变量）：

~~~python
del var # 单个变量
del var_a, var_b # 多个对象
~~~

* 数值运算：

除法： '/' 返回一个浮点数     '//' 返回一个整数  /* 混合计算时，自动为返回浮点数 */

   复数： a + bj     或    complex(a,b)     /* 实部虚部都是浮点型 */

 ~~~python
# 除法：
/  	# 返回一个浮点数     
// 	# 返回一个整数  
	# 混合计算时，自动为返回浮点数 
# 复数：  实部虚部都是浮点型 （两种表示类型）
a + bj        
complex(a,b)     
 ~~~



## String（字符串）

 

* 单引号和双引号使用完全相同。三引号('''或""")可以指定一个多行字符串。//运行结果和代码区一样会换行

~~~python
word = '字符串'
sentence = "这是一个句子。"
paragraph = """这是一个段落，
可以由多行组成"""

#输出结果：
#字符串
#这是一个句子
#这是一个段落
#可以由多行组
~~~

* 转义符 '\'。使用r/R可以让反斜杠不发生转义。

~~~python
print( r"this is a line with \n" )	 #\n会显示，并不是换行。

#输出结果：
#this is a line with \n
~~~

* 按字面意义级联字符串

~~~python
print("this " "is " "string")	#会被自动转换为this is string。

#输出结果：
#this is string
~~~

* 字符串可以用 + 运算符连接在一起，用 * 运算符重复。

~~~python
str='Runoob'
print(str * 2)	# 输出字符串两次 
print(str + '你好')	# 连接字符串

#输出结果：
#RunoobRunoob
#Runoob你好
~~~

* 字符串有两种索引方式，从左往右以 0 开始，从右往左以 -1 开始。

~~~python
str='Runoob'
print(str[0]) # 输出字符串第一个字符

#输出结果：
#R
~~~

* 字符串不能改变。

* 没有单独的字符类型，一个字符就是长度为 1 的字符串。

* 字符串的截取的语法格式如下：变量[头下标:尾下标:步长]  // 变量[头下标：尾下标] 实心圆->空心圆

 ~~~python
str='Runoob'
print(str[2:5]) # 输出从第三个开始到第五个的字符
print(str[0:-1]) # 输出第一个到倒数第二个的所有字符  # 从右往左以-1开始

#输出结果：
#noo
#Runoo
 ~~~

注意：Python 没有单独的字符类型，一个字符就是长度为1的字符串。

## List（列表）

 ~~~python
list = [ 'abcd', 786 , 2.23, 'runoob', 70.2 ]
tinylist = [123, 'runoob']
 ~~~



* List写在方括号之间，元素用逗号隔开。

* 和字符串一样，list可以被索引和切片。

* List可以使用+操作符进行拼接。

* List中的元素是可以改变的。

* 索引： 变量[头下标:尾下标：步长]

 ~~~python
letters = ['c','h','e','c','k','i','o']
letters[1:4:2]    # 1:'h' 4:'k' 2:往前走两步

#输出结果：
#['h','c']
 ~~~

* 难点：步长为负数：

![image-20201023213723375](C:\Users\傻兮兮乎乎\AppData\Roaming\Typora\typora-user-images\image-20201023213723375.png)

 

## Tuple(元组）

元组（tuple）与列表类似，不同之处在于元组的元素不能修改。元组写在小括号 **()** 里，元素之间用逗号隔开。

元组中的元素类型也可以不相同：

 ~~~python
tuple = ( 'abcd', 786 , 2.23, 'runoob', 70.2 )
tinytuple = (123, 'runoob')
 ~~~

* 与字符串一样，元组的元素不能修改。

* 元组也可以被索引和切片，方法一样。

* 注意构造包含 0 或 1 个元素的元组的特殊语法规则。

~~~python
tup1 = ()  	# 空元组
tup2 = (20,) # 一个元素，需要在元素后添加逗号
~~~

* 元组也可以使用+操作符进行拼接。

* string、list 和 tuple 都属于 sequence（序列）。

 

## Set（集合）

* 可以用 { } 或者 set( ) 创建集合,创建空集合用 set( )

~~~python
sites = {'goole','taobao','runoob','facebook','zhihu','baidu'}	#{ }里面的都是大小不一的元组组成的集合

a=set('abracadabra')	#字符串属于特殊的元组
b=set('alacazam')
~~~

* 输出

~~~python
print(sites)	#输出集合，会自动删除重复的元素
~~~

* set 进行集合运算

~~~python
a=set('abracadabra')	
b=set('alacazam')
print(a)
print(a-b)	#a和b的差集
print(a|b)	#a并b
print(a&b)	#a交b
print(a^b)	#a和b不同时存在的元素

#输出结果：
#{'b', 'c', 'a', 'r', 'd'}	#忽略了重复的元素
#{'r', 'b', 'd'}	#将a中所有的元素减去b中所有的元素，所剩下的元素
#{'b', 'c', 'a', 'z', 'm', 'r', 'l', 'd'}	
#{'c', 'a'}
#{'z', 'b', 'm', 'r', 'l', 'd'}	#注意于差集区分，这是计算出a有b没有的元素，和b有a没有的元素
~~~

## Dictionary（字典）

* 内置数据类型，列表是有序的对象集合，字典是无序的对象集合，区别在于：字典中的元素是通过键来存取的，列表是通过偏移量存取。
* 字典用 { } ，用key[^注释1] : value 来定义元素，key相当于位置，value相当于位置上的内容

[^注释1]:key必须使用不可变类型；在同一个字典中，key必须是唯一的。

~~~python
#创造空字典	方法一
dict = {}
dict['one'] = "1 - 菜鸟"
dict[2] = "2 - 菜鸟"
print(dict['one'])
print(dict[2])

#方法二 
tindict = {'name' : 'runoob' , 'code' : 1 , 'site' : 'www.runoob.com'}
print(tindict.keys())	#内置函数 clear(),keys(),values()
print(tindict.values())

#方法三
dict = ([('ruoob':1),('google':2),('taobao':3)])
print(dict)

#方法四
dict = ('ruoob'=1,'goole'=2,'taobao'=3)
print(dict)

#输出结果：

#1 - 菜鸟
#2 - 菜鸟

#dict_keys(['name', 'code', 'site'])
#dict_values(['runoob', 1, 'www.runoob.com'])

#{'Runoob': 1, 'Google': 2, 'Taobao': 3}

#{'Runoob': 1, 'Google': 2, 'Taobao': 3}
~~~

## python数据类型的转换

![image-20201026120238707](C:\Users\傻兮兮乎乎\AppData\Roaming\Typora\typora-user-images\image-20201026120238707.png)

## python3注释

* 单行注释：#
* 多行注释：单引号（’‘’）双引号（”“”），都是三个 。