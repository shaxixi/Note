[toc]

# 封装

> 相比较函数，面向对象是更大的封装，根据职责在一个对象中封装多个方法

![image-20201123081355968](C:\Users\傻兮兮乎乎\AppData\Roaming\Typora\typora-user-images\image-20201123081355968.png)

* 向日癸：对象；
* 生命值，生产阳光，摇晃：方法；

## 类 对象

1.  **类概念**

* 抽象的，是对一群具有相同特征或行为的事物的一个统称
  * 特征被称为属性
  * 行为被称为方法
* 以飞机为例，类就相当于制造飞机时的图纸，一个模板，负责创建对象的

![image-20201123082320117](C:\Users\傻兮兮乎乎\AppData\Roaming\Typora\typora-user-images\image-20201123082320117.png)

2. **对象概念**

* 可以直接使用的，由类创建出来的一个具体存在
  * 由哪一类创建出来的对象，就拥有在哪一类中定义的：属性，方法；

* 以飞机为例，对象就相当于用图纸制造的飞机

![image-20201123082716520](C:\Users\傻兮兮乎乎\AppData\Roaming\Typora\typora-user-images\image-20201123082716520.png)

3.  **类与对象的关系**

* 在程序开发中，先有类，再有对象
* 类只有一个，对象有多个
* 不同对象之间属性可能会各不相同
* 类中定义了什么属性和方法，对象中就有什么属性和方法，不能多，也不能少

4.  **类的设计**

> 首先分析需求，程序中需要包含哪些类
>
> 类名：满足大驼峰命名法（每个单词首字母大写）
>
> 属性：这类事物有什么特征
>
> 方法：这类事物有什么行为

* 类名的确定（与对象名注意区分）

  * 所有对象名的共同名

* 属性和方法的确定

  * 对象具有的行为（动词），通常可以定义成方法
  * 对对象的特征描述，通常可以定义成属性

  ![image-20201123084212616](C:\Users\傻兮兮乎乎\AppData\Roaming\Typora\typora-user-images\image-20201123084212616.png)

  > preson: 类
  >
  > name,age,height: 属性（属性内容可以不同）
  >
  > run(),eat(): 方法
  >
  > 小明，小美：对象

* 内置函数（知道）
  * 两个验证方法：
    * 在标识符/数据后输入一个`.`，然后按下`TAB`键，ipython会提示该对象能够调用的方法列表
    * 使用内置函数`dir`传入标识符/数据，可以查看对象内的所有属性及方法
  * `__方法名__`格式的方法是python提供的内置方法/属性

5. **定义 类**

* 定义只包含方法的类

  ~~~python
  class 类名：
  	def 方法1(self,参数列表)：
      	pass
      def 方法2(self,参数列表)：
      	pass
  ~~~

  > 方法的定义格式和之前学习过的函数几乎一样
  >
  > 区别于第一个函数必须是`self`

* 创建对象(定义完类，使用这个类来创建对象)

  ~~~python
  对象变量 = 类名()
  ~~~

* 例子：

  ![image-20201123091951597](C:\Users\傻兮兮乎乎\AppData\Roaming\Typora\typora-user-images\image-20201123091951597.png)

  ~~~python 
  class Cat:
      """这是一个猫类"""
      
  	def eat(self):
  		print("小猫爱吃鱼")
  	def drink(self):
          print("小猫爱喝水")        
  #创建对象
  Tom=Cat()
  Tom.eat()
  Tom.drink()
  
  #输出结果：
  #小猫爱吃鱼
  #小猫爱喝水
  ~~~

* 引用概念`Tom=Cat()`
  * 等号右侧创建对象，等号左侧引用

  * `Tom`变量中记录的是对象在内存中的地址；也就是变量引用了新建的猫对象

  * 用`print`输出对象变量，默认情况下，能够输出这个变量引用的对象是由哪一个类创建的对象，以及内存中的地址（十六进制表示）

    `__main__.Cat object at 0x7fadb22bbb38` 

  * `%x`可以以十六进制输出数字

## 方法中的self参数

1. **给对象增加属性（不推荐使用）**

* 只需要在 类的外部的代码中（对象）直接通过`.`设置一个属性即可

  ~~~python
  tom.name="Tom"
  ...
  lazy_cat.name="大懒猫"
  ~~~

2. **使用self在方法内部输出每只猫的名字**

* 由哪个对象调用的方法，方法内的`self`就是哪个对象的引用

* 在类封装的方法内部，`self`就表示当前调用方法的对象自己

* 在方法内部

  * 可以通过`self.`访问对象的属性
  * 可以通过`self.`调用其他的对象方法

  ~~~python
  class Cat:
      def drink(self):
          print("%s 爱喝水" % self.name)		#**
  tom=Cat()	#** tom==self
  tom.name = "Tom"	#**
  tom.drink()
  #输出结果：
  #Tom 爱喝水
  ~~~

## 初始化方法(属性)

* 当使用`类名（）`创建对象时，会 自动 执行以下操作：

  * 为对象在内存中分配空间——创建对象
  * 为对象的属性设置初始值——初始化方法（\__init__)；
  * 初始化方法：是对象的内置方法，专门用来定义一个类具有哪些属性的方法；

* 在初始化方法内部定义属性

  * 在`__init__`方法内部使用`self.属性名 = 属性的初始值` 就可以 定义属性
  * 定义属性之后，再使用`Cat`类创建的对象，都会拥有该属性

  ~~~python 
  class Cat:
      def __init__(self，new_name):
          self.name=new_name
      def eat(self):
          print("%s 爱吃鱼" % self.name)
  tom=Cat("Tom")
  lazy_cat=Cat("大懒猫")
  tom.eat()
  lazy_cat=eat()
  #输出结果：
  #Tom 爱吃鱼
  #大懒猫 爱吃鱼
  ~~~

## 内置方法和属性

1. `__del__`：在对象销毁前，自动调用的方法；

   ~~~python
   class Cat:
       def __init__(self,new_name):
           self.name=new_name
           print("%s 来了" % self.name)
       def __del__(self):	#**
           print("%s 去了" % self.name)
   tom=Cat("Tom")	#**全局变量，等生命周期结束后从内存销毁前做的一些事情
   tom.new_name
   print("_"*20)
   #输出结果：
   #Tom 来了
   #"Tom"
   #_____________________
   #Tom 去了	
   ~~~

2. `__str__`：用print输出对象变量时，能够打印自定义的内容； `return`必须用到

   ~~~python
   class Cat:
       def __init__(self,new_name):
           self.name=new_name
           print("%s 来了" % self.name)
       def __str__(self):
           return "我是小猫 %s" % self.name	#**用__str__内置方法必须用到return
   tom=Cat("Tom")	
   print(tom)	#**默认输出 类名 和 内存地址
   #输出结果 ：
   #Tom 来了
   # 我是小猫 Tom
   ~~~

## 私有属性和私有方法

* 在属性名或者方法名前加`__`

~~~python
class Women:
    def __init__(self,name):
        self.name=name
        self.age=18
    def secret(self):
        print("%s 的年龄是 %d" %(self.name,self.age))
xiaofang=Women("小芳")
print(xiaofang.name,xiaofang.age)
xiaofang.secret()
#输出结果：
#小芳 18
#小芳 的年龄是 18
~~~

~~~python
"""私有属性"""
class Women:
    def __init__(self,name):
        self.name=name
        self.__age=18
    def secret(self):
        print("%s 的年龄是 %d" %(self.name,self.__age))
xiaofang=Women("小芳")
xiaofang.secret() #可以访问
print(xiaofang.name,xiaofang.__age)	#xiaofang.__age 访问不了，私有属性
#输出结果：
#小芳 的年龄是 18
~~~

~~~python
"""私有方法"""
class Women:
    def __init__(self,name):
        self.name=name
        self.age=18
    def __secret(self):
        print("%s 的年龄是 %d" %(self.name,self.age))
xiaofang=Women("小芳")
print(xiaofang.name,xiaofang.age)	#可以访问
xiaofang.__secret() #不可访问私有方法
#输出结果：
#小芳 18
~~~

* 伪私有属性和方法
  * 没有真正意义上的私有属性和方法
  * 想要访问私有属性和方法：`_类名__属性名/方法名`

* 父类的私有属性和私有方法(和下面的继承知识点有关)

  ![image-20201125145420715](C:\Users\傻兮兮乎乎\AppData\Roaming\Typora\typora-user-images\image-20201125145420715.png)

  * 在子类的对象方法中，不能访问父类的私有属性和私有方法

    ~~~python
    >>> class a():
    	def dome(self):
    		self.name=100
    		self.__age=90
    	def __do(self):	#**
    		print("%d %d" %(self.name,self.__age))
    
    		
    >>> class b(a):
    	def de(self):
    		print("%d" %self.__age)	#**
    
    		
    >>> B=b()
    >>> print(B)
    <__main__.b object at 0x000001FFDA7B65F8>
    >>> B.de()
    Traceback (most recent call last):
      File "<pyshell#45>", line 1, in <module>
        B.de()
      File "<pyshell#42>", line 3, in de
        print("%d" %self.__age)	#**
    AttributeError: 'b' object has no attribute '_b__age'
    >>> B.__do()
    Traceback (most recent call last):
      File "<pyshell#46>", line 1, in <module>
        B.__do()
    AttributeError: 'b' object has no attribute '__do'
    ~~~

  * 子类对象可以通过父类的公有方法间接访问到私有属性或私有方法

    ![image-20201125150850391](C:\Users\傻兮兮乎乎\AppData\Roaming\Typora\typora-user-images\image-20201125150850391.png)

    ~~~python
    >>> class a:
    	def __init__(self):
    		self.name=100
    		self.__age=90
    	def __do(self):	#**
    		print("%d %d" %(self.name,self.__age))
    	def text(self):	#访问自己的私有属性是可以的
    		self.__do()
    
    		
    >>> class b(a):
    	def de(self):
    		print("%d" %self.__age)	#**
    
    		
    >>> B=b()
    >>> B.text()	#外部访问父类的共有方法中的私有方法是可以的
    100 90
    ~~~

# 继承（派生）

## 单继承

![image-20201123183045702](C:\Users\傻兮兮乎乎\AppData\Roaming\Typora\typora-user-images\image-20201123183045702.png)    

~~~python
class 子类名(父类名):
    ...
~~~

* 子类（派生类）拥有父类（基类）的所有属性和方法
* 子类中应该根据职责，封装子类特有的属性和方法 

## 多继承

![image-20201125153113557](C:\Users\傻兮兮乎乎\AppData\Roaming\Typora\typora-user-images\image-20201125153113557.png)

~~~python
class 子类名(父类名1,父类名2...):
    ...
~~~

* 子类可以拥有多个父类，并且具有所有父类的属性和方法

* 如果父类之间存在同名的属性或者方法，应该尽量避免使用多继承

  ![image-20201125154355241](C:\Users\傻兮兮乎乎\AppData\Roaming\Typora\typora-user-images\image-20201125154355241.png)

  ~~~python
  >>> class A:
  	def demo(self):
  		print("A===")
  	def text(self):
  		print("a===")
  		
  >>> class B:
  	def text(self):
  		print("B===")
  	def demo(self):
  		print("b===")
  
  >>> class C(A,B):	#**这个起决定性作用；A在前，所以调用A的
  	pass
  
  >>> x=C()
  >>> x.demo()
  A===
  >>> x.text()
  a===
  ~~~

  * 方法搜索顺序`__mro__`

    ~~~python 
    print(C.__mro__)
    ~~~

## 新式类和经典类

* 新式类：以`object`为基类

  经典类：不以`object`为基类

  * 新式类和经典类在多继承时会影响到方法的搜索顺序
  * 如果没有父类，建议统一继承自`object`

  ~~~python
  class 类名(object):	
      pass
  
  """python 2.x 中 """
  class 类名:	#经典类
  	psss
  class 类名(object):	#新式类
  	pass
  
  """python 3.x 中 """
  class 类名:	#默认为新式类
  ~~~

  

## 方法和重写

![image-20201123184654934](C:\Users\傻兮兮乎乎\AppData\Roaming\Typora\typora-user-images\image-20201123184654934.png)



* 重写父类方法有两种情况：

  * 覆盖父类的方法

    * 在子类中重新编写父类的方法实现（即在子类中定义一个和父类同名的方法并且实现）；重写之后在运行时，只会调用子类中重写的方法，而不会在调用父类封装方法

      ~~~python
      class dog:
          def bark(self):
              print("汪汪")
      class xtq(dog):
          def bark(self):	#**
              print("汪汪汪")
      ~~~

      

  * 对父类方法进行扩展

    * 子类的方法实现中包含父类的方法实现（即父类原本封装的方法实现时子类方法的一部分);用super(). 调用原本在父类中封装的方法

      ~~~python
      class dog:
          def bark(self):
              print("汪汪")
      class xtq(dog):
          def bark(self):	
              print("汪汪汪")
          	super().bark()	#**super().方法 包含在子类方法内的
          	print("&)(^^%(%&^)(*))")
      ~~~

* `父类名.方法（self)`适用于python 2.x ;如果使用当前子类名调用方法，会形成递归调用，出现死循环

# 多态

* 以继承和重写父类方法为前提

  是调用方法的技巧，不会影响到类的内部设计

![image-20201125161138729](C:\Users\傻兮兮乎乎\AppData\Roaming\Typora\typora-user-images\image-20201125161138729.png)

* 例子

  ![image-20201130082458949](C:\Users\傻兮兮乎乎\AppData\Roaming\Typora\typora-user-images\image-20201130082458949.png)

  ~~~python
  >>> class dog(object):
  	def __init__(self,name):
  		self.name=name
  	def game(self):
  		print("%s 在玩耍..." % self.name)
  
  		
  >>> class xiaotianquan(object):
  	def __init__(self,name):
  		self.name=name
  	def game(self):
  		print("%s 在飞天玩耍..." % self.name)
  
  		
  >>> class person(object):	#**难点
  	def __init__(self,name):
  		self.name=name
  	def person_dog(self,dog):	#** dog是参数
  		print("%s %s 一起玩耍" % (self.name,dog.name))
  		dog.game()
  
  		
  >>> wangcai=dog("wangcai")
  >>> xiaoming=person("xiaoming")
  >>> xiaoming.person_dog(wangcai)	#** wangcai是对象
  xiaoming wangcai 一起玩耍
  wangcai 在玩耍...
  
  		
  >>> acai=xiaotianquan("acai")
  >>> xiaoming.person_dog(acai)
  xiaoming acai 一起玩耍
  acai 在飞天玩耍...
  ~~~

# 属性/方法

## 实例属性/方法

* 专业属于：实例 
  * 对象创建后，内存中就有了一个对象的实实在在的存在，称为实例
    * 创建出来的对象叫做类的实例
    * 创建对象的动作叫做实例化
    * 对象的属性叫做实例属性
    * 对象调用的方法叫做实例方法
  * 每一个对象都有自己独立的内存空间，保存各自不同的属性
  * 多个对象的方法，在内存中只有一份，在调用方法时，需要把对象的引用传递到方法内部

## 类属性/方法

* 类是一个特殊的对象

  * `class AAA:` 定义的类属于类对象
  * `obj1 = AAA()` 属于实例对象
  * 在程序运行时，类同样会被加载到内存
  * 通过 `类名.` 的方式可以访问类的属性或者调用类的方法

* 类属性的定义

  * 类属性通常用来记录与这个类相关的特征，不会用于记录具体对象的特征

  * 类名下方使用赋值语句

    ~~~python
    >>> class tool(object):
    	count=0	#**
    	def __init__(self,name):
    		self.name=name
    		tool.count+=1	#**
    
            
    >>> tool1=tool("yi")
    >>> print(tool.count)
    1
    >>> tool2=tool("er")
    >>> print(tool.count)
    2
    >>> tool3=tool("san")
    >>> print(tool.count)
    3
    ~~~

* 类方法的定义

  * 类方法需要用到修饰器 `@classmethod` 来标识，告诉解释器这是一个类方法

  * 类方法的第一个参数应该是`cls` 

    ~~~python
    >>> class tool(object):
    	count=0
    	@classmethod	#**
    	def tool1(cls):	#**
    		cls.count += 1
    		print("count = %d " % cls.count)
    
            
    >>> tool.tool1()	#**
    count = 1 
    >>> tool.tool1()
    count = 2 
    ~~~

## 静态方法

* 不访问实例属性/类属性时用静态方法

* 通过`类名.` 调用静态方法，不需要创建对象

  ~~~python
  >>> class dog(object):
  	@staticmethod	#**
  	def run():	#**
  		print("小狗再跑")
  
  		
  >>> dog.run()
  小狗再跑
  ~~~

## 方法的选择

* 实例方法——方法内部需要访问实例属性

* 类方法——方法内部只需要访问类属性

  ​			——既需要访问实例属性，又需要访问类属性

* 静态方法——方法内部，不需要访问实例属性和类属性

# 单例

##  单例设计模式

* 设计模式是前人工作的总结和提炼，一种模板套路
* 目的：让类创建的对象，在系统中只有唯一的一个实例
  即——每一次执行 `类名（）`返回的对象，内存地址是相同的
* 应用场景：音乐播放对象；回收站对象；打印机对象……

## `_new_` 方法

* 使用 `类名（）` 创建对象时，python的解释器 首先 会
  调用 `__new__`  方法为对象 **分配空间**	调用 `__init__` 方法为对象 **初始化**
* `__new__` ：是由 `object` 基类提供的内置的静态方法，主要作用：
  1. 在内存中为对象 **分配空间**
  2. **返回** 对象的引用 ； 
     python解释器获得对象的引用后，将引用作为第一个参数，传递给 `__init__` 方法
* 重写 `__new__` 方法：代码非常固定 ，一定要 `return super().__new__(cls)`
  否则python的解释器 得不到 分配了空间的 对象引用，就不会调用对象的初始化方法

* `__new__` 是一个静态方法，在调用时需要主动传递 `cls` 参数

![image-20210105140547079](C:\Users\shahuhu\AppData\Roaming\Typora\typora-user-images\image-20210105140547079.png)

~~~python
>>> class MusicPlayer(object):
	def __init__(self):
		print("播放器初始化")

		
>>> player = MusicPlayer()
播放器初始化
>>> print(player)
<__main__.MusicPlayer object at 0x000001BDA573F208>

'''---------------------------------------------------------'''
>>> class MusicPlayer(object):
	def __new__(cls):	#**
		print("创建对象，分配空间")
	def __init__(self):
		print("播放器初始化")

		
>>> player = MusicPlayer()
创建对象，分配空间
>>> print(player)
None

'''---------------------------------------------------------'''
>>> class MusicPlayer(object):
	def __new__(cls):
		print("创建对象，分配空间")
		instance = super().__new__(cls)		#**
		return instance		#**
	def __init__(self):
		print("播放器初始化")

		
>>> player = MusicPlayer()
创建对象，分配空间
播放器初始化
>>> player1 = MusicPlayer()
创建对象，分配空间
播放器初始化
>>> print(player)
<__main__.MusicPlayer object at 0x000001BDA5797978>
>>> print(player1)
<__main__.MusicPlayer object at 0x000001BDA5797978>

'''---------------------------------------------------------'''
>>> class MusicPlayer(object):
	instance = None		#**
	def __new__(cls):
		print("创建对象，分配空间")
		if cls.instance is None:		#**
			cls.instance = super().__new__(cls)
		return cls.instance
	def __init__(self):
		print("播放器初始化")

		
>>> player = MusicPlayer()
创建对象，分配空间
播放器初始化
>>> player1 = MusicPlayer()
创建对象，分配空间
播放器初始化
>>> print(player)
<__main__.MusicPlayer object at 0x000001BDA57CE7B8>
>>> print(player1)
<__main__.MusicPlayer object at 0x000001BDA57CE7B8>
~~~



* 只执行一次初始化工作

~~~python
>>> class MusicPlayer(object):
	instance = None
	init_flag = False 	#**
	def __new__(cls):
		print("创建对象，分配空间")
		if cls.instance is None:
			cls.instance = super().__new__(cls)
		return cls.instance
	def __init__(self):
		if MusicPlayer.init_flag is False:	#**
			print("播放器初始化")
			MusicPlayer.init_flag = True	#**

			
>>> player3 = MusicPlayer()
创建对象，分配空间
播放器初始化
>>> player4 = MusicPlayer()
创建对象，分配空间
~~~

