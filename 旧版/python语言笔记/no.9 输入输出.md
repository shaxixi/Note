  ## 输出格式

* 输出的**值转成字符串**：
  str()：函数返回一个**用户**易读的表达形式
  repr(): 函数产生一个**解释器**易读的表达形式

  ~~~python
  >>> hello = 'hello ss\n'
  
  >>> str(hello)
  'hello ss\n'
  
  #  repr() 函数可以转义字符串中的特殊字符
  >>> repr(hello)
  "'hello ss\\n'"
  
  >>> hellos = repr(hello)
  >>> print(hellos)
  'hello ss\n'
  
  # repr() 的参数可以是 Python 的任何对象
  >>> repr((hello,'sd','ge'))
  "('hello ss\\n', 'sd', 'ge')"
  ~~~

* rjust() 方法——字符串靠右，并在左边填充空格
  center() 方法  ljust()方法  
  zfill() 方法——在数字的左边填充 0 

  ~~~python
  # rjust() 方法
  >>> for x in range(1, 11):
          print(repr(x).rjust(2), repr(x*x).rjust(3), end=' ')
          # 注意前一行 'end' 的使用
          print(repr(x*x*x).rjust(4))
    
   1   1    1
   2   4    8
   3   9   27
   4  16   64
   5  25  125
   6  36  216
   7  49  343
   8  64  512
   9  81  729
  10 100 1000
  
  # zfill() 方法
  >>> '12'.zfill(5)
  '00012'
  >>> '-3.14'.zfill(7)
  '-003.14'
  >>> '3.14159265359'.zfill(5)
  '3.14159265359'
  ~~~

* str.format()

  括号及其里面的字符 (称作格式化字段) 将会被 format() 中的参数替换

  * 括号中没参数，则按顺序指向format()中的位置

    ~~~python
    >>> print('{}网址： "{}!"'.format('菜鸟教程', 'www.runoob.com'))
    菜鸟教程网址： "www.runoob.com!"
    ~~~

  * 括号中数字做参数，则指向 format() 中的位置

    ~~~python
    >>> print('{0} 和 {1}'.format('Google', 'Runoob'))
    Google 和 Runoob
    >>> print('{1} 和 {0}'.format('Google', 'Runoob'))
    Runoob 和 Google
    ~~~

  * 括号中关键字做参数，则指向使用该名字的参数

    ~~~python
    >>> print('{name}网址： {site}'.format(name='菜鸟教程', site='www.runoob.com'))
    菜鸟教程网址： www.runoob.com
    ~~~

  * 数字参数和关键字参数可以任意结合

    ~~~python
    >>> print('站点列表 {0}, {1}, 和 {other}。'.format('Google', 'Runoob', other='Taobao'))
    站点列表 Google, Runoob, 和 Taobao。
    ~~~

  * 括号中参数后的 `：`——可决定宽度，位数

    ~~~python
    >>> import math
    >>> print('常量 PI 的值近似为 {0:.3f}。'.format(math.pi))
    常量 PI 的值近似为 3.142。
    
    >>> table = {'Google': 1, 'Runoob': 2, 'Taobao': 3}
    >>> for name, number in table.items():
    ...     print('{0:10} ==> {1:10d}'.format(name, number))
    ...
    Google     ==>          1
    Runoob     ==>          2
    Taobao     ==>          3
    ~~~

  * 传入一个字典

    ~~~python
    # [] 来访问键值 
    >>> table = {'Google': 1, 'Runoob': 2, 'Taobao': 3}
    >>> print('Runoob: {0[Runoob]:d}; Google: {0[Google]:d}; Taobao: {0[Taobao]:d}'.format(table))
    Runoob: 2; Google: 1; Taobao: 3 
                
    # 使用 ** 来实现相同的功能             
    >>> table = {'Google': 1, 'Runoob': 2, 'Taobao': 3}
    >>> print('Runoob: {Runoob:d}; Google: {Google:d}; Taobao: {Taobao:d}'.format(**table))
    Runoob: 2; Google: 1; Taobao: 3    
                
    '''------ 0[Runoob]:d -------- Runoob:d --------'''
    ~~~

## 输入格式

~~~python
>>>str = input("请输入：");
>>>print ("你输入的内容是: ", str)

请输入：菜鸟教程
你输入的内容是:  菜鸟教程
~~~

## 读和写文件

> open() 将会返回一个 file 对象
> 格式：
> open(filename, mode)
> 		filename： 你要访问的文件名称路径
> 		mode：决定了打开文件的模式：只读，写入，追加等。默认文件访问模式为只读(r)。


|    模式    |  r   |  r+  |  w   |  w+  |  a   |  a+  |
| :--------: | :--: | :--: | :--: | :--: | :--: | :--: |
|     读     |  +   |  +   |      |  +   |      |  +   |
|     写     |      |  +   |  +   |  +   |  +   |  +   |
|    创建    |      |      |  +   |  +   |  +   |  +   |
|    覆盖    |      |      |  +   |  +   |      |      |
| 指针在开始 |  +   |  +   |  +   |  +   |      |      |
| 指针在结尾 |      |      |      |      |  +   |  +   |

~~~python
''' 写文件 '''
# 打开一个文件
f = open("/tmp/foo.txt", "w")

f.write( "Python 是一个非常好的语言。\n是的，的确非常好!!\n" )

# 关闭打开的文件
f.close()

''' 打开文件后显示 '''
$ cat /tmp/foo.txt 
Python 是一个非常好的语言。
是的，的确非常好!!
~~~

## 文件对象的方法

例子假设已经创建了一个称为 f 的文件对象（上面代码中已创建）。

* read() 方法——所有内容都将被读取并且返回

  ~~~python
  # 打开一个文件
  f = open("/tmp/foo.txt", "r")
  
  str = f.read()
  print(str)
  
  # 关闭打开的文件
  f.close()
  
  '''输出'''
  Python 是一个非常好的语言。
  是的，的确非常好!!
  ~~~

* readline() 方法——读取单独的一行，标识符：'\n'；如果返回一个空字符串, 说明已经已经读取到最后一行。

  ~~~python
  # 打开一个文件
  f = open("/tmp/foo.txt", "r")
  
  str = f.readline()
  print(str)
  
  # 关闭打开的文件
  f.close()
  
  '''输出'''
  Python 是一个非常好的语言。
  ~~~

* readlines() 方法——返回该文件中包含的所有行

  ~~~python
  # 打开一个文件
  f = open("/tmp/foo.txt", "r")
  
  str = f.readlines()
  print(str)
  
  # 关闭打开的文件
  f.close()
  
  '''输出'''
  ['Python 是一个非常好的语言。\n', '是的，的确非常好!!\n']
  ~~~

* for x in y ——x 以 行 为单位

  ~~~python
  # 打开一个文件
  f = open("/tmp/foo.txt", "r")
  
  for line in f:
      print(line, end='')
  
  # 关闭打开的文件
  f.close()
  
  '''输出'''
  Python 是一个非常好的语言。
  是的，的确非常好!!
  ~~~

* write() 方法——返回写入的字符数

  ~~~python
  # 打开一个文件
  f = open("/tmp/foo.txt", "w")
  
  num = f.write( "Python 是一个非常好的语言。\n是的，的确非常好!!\n" )
  print(num)
  # 关闭打开的文件
  f.close()
  
  '''输出'''
  29
  ~~~

* tell() 方法——返回文件对象当前所处的位置, 它是从文件开头开始算起的字节数。

* seek ( offset , from_what ) 方法 ——改变文件当前的位置

  > from_what 的值, 如果是 0 表示开头, 如果是 1 表示当前位置, 2 表示文件的结尾。
  > from_what 值为默认为0，即文件开头。

  ~~~python
  ''' 如： '''
  	seek(x,0) ： 从起始位置即文件首行首字符开始移动 x 个字符
  	seek(x,1) ： 表示从当前位置往后移动x个字符
  	seek(-x,2)：表示从文件的结尾往前移动x个字符
      
  '''-----------------------'''
  >>> f = open('/tmp/foo.txt', 'rb+')
  >>> f.write(b'0123456789abcdef')
  16
  >>> f.seek(5)     # 移动到文件的第六个字节
  5
  >>> f.read(1)
  b'5'
  >>> f.seek(-3, 2) # 移动到文件的倒数第三字节
  13
  >>> f.read(1)
  b'd'
  ~~~

* close() 方法——关闭文件并释放系统的资源

  ~~~python
  ''' 与 with ... as ... 一起使用 '''
  >>> with open('/tmp/foo.txt', 'r') as f:
  ...     read_data = f.read()
  >>> f.closed
  True
  ~~~

  