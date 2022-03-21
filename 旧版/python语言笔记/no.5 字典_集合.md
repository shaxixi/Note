[toc]

# 字典

~~~python
dict3 = {'name': 'runoob', 'likes': 123, 'url': 'www.runoob.com'}
dict1 = { 'abc': 456 }
dict2 = { 'abc': 123, 98.6: 37 }
dict = {}	#空字典
~~~

* 字典的每个键值`key=>value` 对用冒号` : `分割，每个对之间用逗号`,`分割，整个字典包括在花括号 `{}` 中 
* 键是唯一的，值可不唯一；不允许同一个键出现两次。创建时如果同一个键被赋值两次，**后一个值会被记住**
* 值可以取任何数据类型，但键必须不可变，所以可以用数字，字符串或元组充当，而用列表就不行。

~~~python
dict = {['Name']: 'Runoob', 'Age': 7}
print ("dict['Name']: ", dict['Name'])
#输出结果：
#Traceback (most recent call last):
#  File "test.py", line 3, in <module>
#    dict = {['Name']: 'Runoob', 'Age': 7}
#TypeError: unhashable type: 'list'
~~~

## 访问字典里的值

* 把相应的键放入到方括号中

~~~python
dict = {'Name': 'Runoob', 'Age': 7, 'Class': 'First'}
print ("dict['Name']: ", dict['Name'])
print ("dict['Age']: ", dict['Age'])
#输出结果：
#dict['Name']:  Runoob
#dict['Age']:  7
~~~

## 修改字典

* 向字典添加新内容的方法是增加新的键/值对，修改或删除已有键/值对

~~~python
dict = {'Name': 'Runoob', 'Age': 7, 'Class': 'First'}
dict['Age'] = 8               # 更新 Age
dict['School'] = "菜鸟教程"  # 添加信息
print ("dict['Age']: ", dict['Age'])
print ("dict['School']: ", dict['School'])
#输出结果：
#dict['Age']:  8
#dict['School']:  菜鸟教程
~~~

## 删除字典元素

* 能删单一的元素也能清空字典，还能删除一个字典

~~~python
dict = {'Name': 'Runoob', 'Age': 7, 'Class': 'First'}
del dict['Name'] # 删除键 'Name'
dict.clear()     # 清空字典
del dict         # 删除字典
print ("dict['Age']: ", dict['Age'])
print ("dict['School']: ", dict['School'])
~~~

## 有关字典的函数

~~~python
>>> dict = {'222':323,'42fa':4222,'dgag':334455}
>>> dict
{'222': 323, '42fa': 4222, 'dgag': 334455}

'''-----------list() 函数 和 key() 方法--------------'''
>>> list(dict.keys())
['222', '42fa', 'dgag']

'''-----------sorted() 函数 和 key() 方法--------------'''
>>> sorted(dict.keys())
['222', '42fa', 'dgag']

'''-----------in / not in--------------'''
>>> '33' in dict
False
>>> '33' not in dict
True

'''-----------推导式--------------'''
>>> {x:x*4 for x in dict}
{'222': '222222222222', '42fa': '42fa42fa42fa42fa', 'dgag': 'dgagdgagdgagdgag'}
~~~



## **？**字典内置函数&方法

| 序号 | 函数及描述                                                   | 实例                                                         |
| :--- | :----------------------------------------------------------- | :----------------------------------------------------------- |
| 1    | len(dict) 计算字典元素个数，即键的总数。                     | `>>> dict = {'Name': 'Runoob', 'Age': 7, 'Class': 'First'}` <br />`>>> len(dict)` <br />` 3` |
| 2    | str(dict) 输出字典，以可打印的字符串表示。                   | `>>> dict = {'Name': 'Runoob', 'Age': 7, 'Class': 'First'} ` <br />`>>> str(dict)` <br /> `"{'Name': 'Runoob', 'Class': 'First', 'Age': 7}"` |
| 3    | type(variable) 返回输入的变量类型，如果变量是字典就返回字典类型。 | `>>> dict = {'Name': 'Runoob', 'Age': 7, 'Class': 'First'} ` <br />``>>> type` <br />`(dict) <class 'dict'>` |

| 序号 | 函数及描述                                                   |
| :--- | :----------------------------------------------------------- |
| 1    | [radiansdict.clear()](https://www.runoob.com/python3/python3-att-dictionary-clear.html) 删除字典内所有元素 |
| 2    | [radiansdict.copy()](https://www.runoob.com/python3/python3-att-dictionary-copy.html) 返回一个字典的浅复制 |
| 3    | [radiansdict.fromkeys()](https://www.runoob.com/python3/python3-att-dictionary-fromkeys.html) 创建一个新字典，以序列seq中元素做字典的键，val为字典所有键对应的初始值 |
| 4    | [radiansdict.get(key, default=None)](https://www.runoob.com/python3/python3-att-dictionary-get.html) 返回指定键的值，如果键不在字典中返回 default 设置的默认值 |
| 5    | [key in dict](https://www.runoob.com/python3/python3-att-dictionary-in.html) 如果键在字典dict里返回true，否则返回false |
| 6    | [radiansdict.items()](https://www.runoob.com/python3/python3-att-dictionary-items.html) 以列表返回可遍历的(键, 值) 元组数组 |
| 7    | [radiansdict.keys()](https://www.runoob.com/python3/python3-att-dictionary-keys.html) 返回一个迭代器，可以使用 list() 来转换为列表 |
| 8    | [radiansdict.setdefault(key, default=None)](https://www.runoob.com/python3/python3-att-dictionary-setdefault.html) 和get()类似, 但如果键不存在于字典中，将会添加键并将值设为default |
| 9    | [radiansdict.update(dict2)](https://www.runoob.com/python3/python3-att-dictionary-update.html) 把字典dict2的键/值对更新到dict里 |
| 10   | [radiansdict.values()](https://www.runoob.com/python3/python3-att-dictionary-values.html) 返回一个迭代器，可以使用 list() 来转换为列表 |
| 11   | [pop(key[,default\])](https://www.runoob.com/python3/python3-att-dictionary-pop.html) 删除字典给定键 key 所对应的值，返回值为被删除的值。key值必须给出。 否则，返回default值。 |
| 12   | [popitem()](https://www.runoob.com/python3/python3-att-dictionary-popitem.html) 随机返回并删除字典中的最后一对键和值。 |

# 集合

~~~python
>>> basket = {'apple', 'orange', 'apple', 'pear', 'orange', 'banana'}
>>> print(basket)                      # 删除重复的
{'orange', 'banana', 'pear', 'apple'}
>>> 'orange' in basket                 # 检测成员
True
>>> 'crabgrass' in basket
False
~~~

* 用`{}`或者`set()`函数创建集合，用逗号`,`隔开

* 创建空集合必须用`set()`；`{}`是用来创建一个空字典
* 集合是一个无序的不重复元素序列

## 集合的基本操作

* 添加元素

  *  `s.add(x)`将元素 x 添加到集合 s 中，如果元素已存在，则不进行任何操作。

    > \>>> thisset = set(("Google", "Runoob", "Taobao"))
    > \>>> thisset.add("Facebook")
    > \>>> **print**(thisset)
    > {'Taobao', 'Facebook', 'Google', 'Runoob'}

  * `s.update( x )`可以添加元素，且参数可以是列表，元组，字典等,x 可以有多个，用逗号分开。

    > \>>> thisset = set(("Google", "Runoob", "Taobao"))
    > \>>> thisset.update({1,3})
    > \>>> **print**(thisset)
    > {1, 3, 'Google', 'Taobao', 'Runoob'}
    > \>>> thisset.update([1,4],[5,6]) 
    > \>>> **print**(thisset)
    > {1, 3, 4, 5, 6, 'Google', 'Taobao', 'Runoob'}

* 移除元素

  * `s.remove( x )`将元素 x 从集合 s 中移除，如果元素不存在，则会发生错误。

    > \>>> thisset = set(("Google", "Runoob", "Taobao"))
    > \>>> thisset.remove("Taobao")
    > \>>> **print**(thisset)
    > {'Google', 'Runoob'}
    > \>>> thisset.remove("Facebook")  # 不存在会发生错误
    > Traceback (most recent call last):
    >  File "<stdin>", line 1, **in** <module>
    > KeyError: 'Facebook'

  * `s.discard( x )`移除集合中的元素，且如果元素不存在，不会发生错误

    > \>>> thisset = set(("Google", "Runoob", "Taobao"))
    > \>>> thisset.discard("Facebook") # 不存在不会发生错误
    > \>>> **print**(thisset)
    > {'Taobao', 'Google', 'Runoob'}

  * `s.pop() `可以设置随机删除集合中的一个元素;多次执行测试结果都不一样。

    set 集合的 pop 方法会对集合进行无序的排列，然后将这个无序排列集合的左面第一个元素进行删除。

    > \>>>thisset = set(("Google", "Runoob", "Taobao", "Facebook"))
    > \>>>x = thisset.pop()	
    >
    > \>>>print(x)
    >
    > Runoob

* 计算集合元素个数

  * `len(s)`

    > \>>> thisset = set(("Google", "Runoob", "Taobao"))
    > \>>> len(thisset)
    > 3

* 清空集合

  * `s.clear()`

    > \>>> thisset = set(("Google", "Runoob", "Taobao"))
    > \>>> thisset.clear()
    > \>>> **print**(thisset)
    > set()

* 判断元素是否在集合中存在

  * `x in s`

    > \>>> thisset = set(("Google", "Runoob", "Taobao"))
    > \>>> "Runoob" **in** thisset
    > True
    > \>>> "Facebook" **in** thisset
    > False

* 集合运算

  ~~~python
  >>> a = set('abracadabra')
  >>> b = set('alacazam')
  >>> a                                  
  {'a', 'r', 'b', 'c', 'd'}
  >>> a - b                              # 在 a 中的字母，但不在 b 中
  {'r', 'd', 'b'}
  >>> a | b                              # 在 a 或 b 中的字母
  {'a', 'c', 'r', 'd', 'b', 'm', 'z', 'l'}
  >>> a & b                              #  在 a 和 b 中都有的字母
  {'a', 'c'}
  >>> a ^ b                              #  在 a 或 b 中的字母，但不同时在 a 和 b 中
  {'r', 'd', 'b', 'm', 'z', 'l'}
  ~~~

## 集合的推导式

~~~python
>>> a = {x for x in 'abracadabra' if x not in 'abc'}
>>> a
{'r', 'd'}
~~~

## **？**集合内置方法完整列表

| 方法                                                         | 描述                                                         |
| :----------------------------------------------------------- | :----------------------------------------------------------- |
| [add()](https://www.runoob.com/python3/ref-set-add.html)     | 为集合添加元素                                               |
| [clear()](https://www.runoob.com/python3/ref-set-clear.html) | 移除集合中的所有元素                                         |
| [copy()](https://www.runoob.com/python3/ref-set-copy.html)   | 拷贝一个集合                                                 |
| [difference()](https://www.runoob.com/python3/ref-set-difference.html) | 返回多个集合的差集                                           |
| [difference_update()](https://www.runoob.com/python3/ref-set-difference_update.html) | 移除集合中的元素，该元素在指定的集合也存在。                 |
| [discard()](https://www.runoob.com/python3/ref-set-discard.html) | 删除集合中指定的元素                                         |
| [intersection()](https://www.runoob.com/python3/ref-set-intersection.html) | 返回集合的交集                                               |
| [intersection_update()](https://www.runoob.com/python3/ref-set-intersection_update.html) | 返回集合的交集。                                             |
| [isdisjoint()](https://www.runoob.com/python3/ref-set-isdisjoint.html) | 判断两个集合是否包含相同的元素，如果没有返回 True，否则返回 False。 |
| [issubset()](https://www.runoob.com/python3/ref-set-issubset.html) | 判断指定集合是否为该方法参数集合的子集。                     |
| [issuperset()](https://www.runoob.com/python3/ref-set-issuperset.html) | 判断该方法的参数集合是否为指定集合的子集                     |
| [pop()](https://www.runoob.com/python3/ref-set-pop.html)     | 随机移除元素                                                 |
| [remove()](https://www.runoob.com/python3/ref-set-remove.html) | 移除指定元素                                                 |
| [symmetric_difference()](https://www.runoob.com/python3/ref-set-symmetric_difference.html) | 返回两个集合中不重复的元素集合。                             |
| [symmetric_difference_update()](https://www.runoob.com/python3/ref-set-symmetric_difference_update.html) | 移除当前集合中在另外一个指定集合相同的元素，并将另外一个指定集合中不同的元素插入到当前集合中。 |
| [union()](https://www.runoob.com/python3/ref-set-union.html) | 返回两个集合的并集                                           |
| [update()](https://www.runoob.com/python3/ref-set-update.html) | 给集合添加元素                                               |