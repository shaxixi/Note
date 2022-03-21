[toc]

## 列表的方法

列表可以修改，而字符串和元组不能。

> \>>>a = [22,234,44,12,33,22,12]

| 方法              | 描述                                                         |
| :---------------- | :----------------------------------------------------------- |
| list.append(x)    | 把一个元素添加到列表的结尾，相当于 a[len(a):] = [x]。<br />>>> a.append(3)<br/>>>> a<br/>[22, 234, 44, 12, 33, 22, 12, 3] |
| list.extend(L)    | 通过添加指定列表的所有元素来扩充列表，相当于 a[len(a):] = L。<br />>>> b = [3,2,1]<br/>>>> a.extend(b)<br/>>>> a<br/>[22, 234, 44, 12, 33, 22, 12, 3, 3, 2, 1] |
| list.insert(i, x) | 在指定位置插入一个元素。第一个参数是准备插入到其前面的那个元素的**索引**，例如 a.insert(0, x) 会插入到整个列表之前【第一位】，而 a.insert(len(a), x) 相当于 **a.append(x)**【最后一位】 。<br />>>> a.insert(2,0)<br/>>>> a<br/>[22, 234, 0, 44, 12, 33, 22, 12, 3, 3, 2, 1] |
| list.remove(x)    | 删除列表中**值为 x 的第一个元素**。如果没有这样的元素，就会返回一个错误。<br />>>> a.remove(1)<br/>>>> a<br/>[22, 234, 0, 44, 12, 33, 22, 12, 3, 3, 2] |
| list.pop([i])     | 从列表的指定位置移除元素，并将其返回。如果没有指定索引，a.pop()返回最后一个元素。元素随即从列表中被移除。（方法中 i 两边的方括号表示这个参数是可选的，而不是要求你输入一对方括号，你会经常在 Python 库参考手册中遇到这样的标记。）<br />>>> a.pop(1)<br/>234<br/>>>> a<br/>[22, 0, 44, 12, 33, 22, 12, 3, 3, 2]<br/>>>> a.pop()<br/>2<br/>>>> a<br/>[22, 0, 44, 12, 33, 22, 12, 3, 3] |
| list.clear()      | 移除列表中的**所有项**，等于del a[:]。<br />>>> b.clear()<br/>>>> b<br/>[] |
| list.index(x)     | 返回列表中**第一个值为 x 的元素的索引**。如果没有匹配的元素就会返回一个错误。<br />>>> a.index(3)<br/>7 |
| list.count(x)     | 返回 x 在列表中出现的**次数**。<br />>>> a.count(44)<br/>1   |
| list.sort()       | 对列表中的元素进行**排序**【从小到大】。<br />>>> a.sort()<br/>>>> a<br/>[0, 3, 3, 12, 12, 22, 22, 33, 44] |
| list.reverse()    | **倒排**列表中的元素。<br />>>> a.reverse()<br/>>>> a<br/>[44, 33, 22, 22, 12, 12, 3, 3, 0] |
| list.copy()       | 返回列表的浅复制，等于a[:]。<br />>>> a.copy()<br/>[44, 33, 22, 22, 12, 12, 3, 3, 0] |

> 类似 insert, remove 或 sort 等 **修改列表的方法没有返回值**。

* 把列表当作堆栈使用——后进先出
  append（）——把一个元素添加到堆栈顶
  pop（）——把一个元素从堆栈顶释放出来

  ~~~python
  >>> stack = [3, 4, 5]
  >>> stack.append(6)
  >>> stack.append(7)
  >>> stack
  [3, 4, 5, 6, 7]
  >>> stack.pop()
  7
  >>> stack
  [3, 4, 5, 6]
  >>> stack.pop()
  6
  >>> stack.pop()
  5
  >>> stack
  [3, 4]
  ~~~

* 把列表当作队列使用——先进先出
  append（）——把一个元素添加到队列最后
  popleft（）——把最开始的元素从队列中释放出来

  ~~~python
  >>> from collections import deque	#** 
  >>> queue = deque(["Eric", "John", "Michael"])	#**
  >>> queue.append("Terry")           
  >>> queue.append("Graham")          
  >>> queue.popleft()                 
  'Eric'
  >>> queue.popleft()                
  'John'
  >>> queue                           
  deque(['Michael', 'Terry', 'Graham'])
  ~~~

* 列表推导式  `[表达式 for ... in ... for ... in ... if 表达式]`

  ~~~python
  >>> vec = [2,4,6]
  
  '''表达式 for in'''
  >>> [2*x for x in vec]
  [4, 8, 12]
  
  '''表达式+列表 for in'''
  >>> [[1*x,2*x] for x in vec]
  [[2, 4], [4, 8], [6, 12]]
  
  '''表达式 for in if'''
  >>> [3*x for x in vec if x>3]
  [12, 18]
  
  '''-----------------------------------------------------------'''
  >>> vec2 = [1,2,3]
  
  '''表达式 for in for in '''
  >>> [x*y for x in vec for y in vec2]	#** vec[0]和vec2[0],vec2[1],vec2[2] 分别相乘，然后再到vec[1],vec[2]
  [2, 4, 6, 4, 8, 12, 6, 12, 18]
  
  '''列表元素表达式 for in '''
  >>> [vec[i]*vec2[i] for i in range(len(vec))]
  [2, 8, 18]
  ~~~

* 嵌套列表解析 ——类似于 矩阵，多维数组的运用

  ~~~python
  >>> matrix = [
  	[1,2,3,4],
  	[5,6,7,8],
  	[9,10,11,12]
  	]
  
  '''------------------------方法一----------------------------'''
  >>> [[row[i] for row in matrix] for i in range(4)]
  [[1, 5, 9], [2, 6, 10], [3, 7, 11], [4, 8, 12]]
  
  '''------------------------方法二----------------------------'''
  >>> transposed = []
  >>> for i in range(4):
  	transposed_row = []
  	for row in matrix:
  		transposed_row.append(row[i])
  	transposed.append(transposed_row)	
  >>> transposed
  [[1, 5, 9], [2, 6, 10], [3, 7, 11], [4, 8, 12]]
  
  '''-----------------------方法三-----------------------------'''
  >>> transposed = []
  >>> for i in range(4):
  	transposed.append([row[i] for row in matrix])	
  >>> transposed
  [[1, 5, 9], [2, 6, 10], [3, 7, 11], [4, 8, 12]]
  ~~~

## del 语句 —— 删除元素

* 从一个列表中依 **索引** 而 **不是值** 来删除一个元素

  ~~~python
  >>> a = [-1, 1, 66.25, 333, 333, 1234.5]
  '''-------------------------'''
  >>> del a[0]
  >>> a
  [1, 66.25, 333, 333, 1234.5]
  '''-------------------------'''
  >>> del a[2:4]
  >>> a
  [1, 66.25, 1234.5]
  '''-------------------------'''
  >>> del a[:]
  >>> a
  []
  ~~~

* 用 del 删除实体**变量**

  ~~~python
  >>> a = [1,2,3,4]
  >>> a
  [1, 2, 3, 4]
  >>> del a	#** 删除的是变量，不是变量中的值
  >>> a
  Traceback (most recent call last):
    File "<pyshell#42>", line 1, in <module>
      a
  NameError: name 'a' is not defined
  ~~~

## 遍历技巧

* items()方法——字典遍历，取关键字和对应的值

  ~~~python
  >>> knight = {'fff':32,'gg':42,'hh':67}
  >>> for k,v in knight.items():
  	print(k, v)
  	
  fff 32
  gg 42
  hh 67
  ~~~

* enumerate()函数——列表遍历，取索引和对应值

  ~~~python
  >>> knight=[12,42,53,13]
  >>> for k,v in enumerate(knight):
  	print(k, v)
  	
  0 12
  1 42
  2 53
  3 13
  ~~~

* zip()函数  .form()方法——多个列表遍历

  ~~~python
  >>> questions = ['name','quest','favorite color']
  >>> answers = ['boo','the holy grail','blue']
  >>> for q, a in zip(questions, answers):
  	print('What is your {0}?  It is {1}.'.format(q, a))		#** 引用
  
  What is your name?  It is boo.
  What is your quest?  It is the holy grail.
  What is your favorite color?  It is blue.
  ~~~

* reversed()函数——列表反向遍历

  ~~~python
  >>> knight
  [12, 42, 53, 13]
  
  >>> for i in reversed(knight):
  	print(i)
  	
  13
  53
  42
  12
  ~~~

* sorted()函数——列表排序遍历，从小到大

  ~~~python
  >>> knight
  [12, 42, 53, 13]
  
  >>> for i in sorted(set(knight)):
  	print(i)
  	
  12
  13
  42
  53
  ~~~

  

