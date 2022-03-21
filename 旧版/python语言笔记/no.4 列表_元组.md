[toc]

# 列表

~~~python
list1 = ['Google', 'Runoob', 1997, 2000]
list2 = [1, 2, 3, 4, 5 ]
list3 = ["a", "b", "c", "d"]
list4 = ['red', 'green', 'blue', 'yellow', 'white', 'black']
~~~

* 列表都可以进行的操作包括：索引，切片，加，乘，检查成员。
* 列表的数据项不需要具有相同的类型
* 列表使用中括号`[]`，用逗号`,`隔开
* 用索引访问列表

> 序列中每个值都有对应的位置值，称之为索引，第一个索引是0，以此类推；倒数第一个索引是-1，以此类推；

## 更新列表

* 对数据项进行修改更新

~~~python
list = ['Google', 'Runoob', 1997, 2000]
print ("第三个元素为 : ", list[2])
list[2] = 2001
print ("更新后的第三个元素为 : ", list[2])
#输出结果：
#第三个元素为 : 1997
#更新后的第三个元素为 : 2001
~~~

* 添加列表项：`append()`

## 删除列表元素

* `del 列表项`

~~~python
list = ['Google', 'Runoob', 1997, 2000]
print ("原始列表 : ", list)
del list[2]
print ("删除第三个元素 : ", list)
#输出结果：
#原始列表 :  ['Google', 'Runoob', 1997, 2000]
#删除第三个元素 :  ['Google', 'Runoob', 2000]
~~~

* `remove()`

## python列表脚本操作符

| Python 表达式                         | 结果                         | 描述                 |
| :------------------------------------ | :--------------------------- | :------------------- |
| len([1, 2, 3])                        | 3                            | 长度                 |
| [1, 2, 3] + [4, 5, 6]                 | [1, 2, 3, 4, 5, 6]           | 组合                 |
| ['Hi!'] * 4                           | ['Hi!', 'Hi!', 'Hi!', 'Hi!'] | 重复                 |
| 3 in [1, 2, 3]                        | True                         | 元素是否存在于列表中 |
| for x in [1, 2, 3]: print(x, end=" ") | 1 2 3                        | 迭代                 |

## python拼接

~~~python
>>>squares = [1, 4, 9, 16, 25]
>>> squares += [36, 49, 64, 81, 100]
>>> squares
[1, 4, 9, 16, 25, 36, 49, 64, 81, 100]
~~~

## python嵌套列表

> 类似于多元数组

~~~python
>>>a = ['a', 'b', 'c']
>>> n = [1, 2, 3]
>>> x = [a, n]
>>> x
[['a', 'b', 'c'], [1, 2, 3]]
>>> x[0]
['a', 'b', 'c']
>>> x[0][1]
'b'
~~~

## **？**python列表函数&方法

| 序号 | 函数                                                         |
| :--- | :----------------------------------------------------------- |
| 1    | [len(list)](https://www.runoob.com/python3/python3-att-list-len.html) 列表元素个数 |
| 2    | [max(list)](https://www.runoob.com/python3/python3-att-list-max.html) 返回列表元素最大值 |
| 3    | [min(list)](https://www.runoob.com/python3/python3-att-list-min.html) 返回列表元素最小值 |
| 4    | [list(seq)](https://www.runoob.com/python3/python3-att-list-list.html) 将元组转换为列表 |

| 序号 | 方法                                                         |
| :--- | :----------------------------------------------------------- |
| 1    | [list.append(obj)](https://www.runoob.com/python3/python3-att-list-append.html) 在列表末尾添加新的对象 |
| 2    | [list.count(obj)](https://www.runoob.com/python3/python3-att-list-count.html) 统计某个元素在列表中出现的次数 |
| 3    | [list.extend(seq)](https://www.runoob.com/python3/python3-att-list-extend.html) 在列表末尾一次性追加另一个序列中的多个值（用新列表扩展原来的列表） |
| 4    | [list.index(obj)](https://www.runoob.com/python3/python3-att-list-index.html) 从列表中找出某个值第一个匹配项的索引位置 |
| 5    | [list.insert(index, obj)](https://www.runoob.com/python3/python3-att-list-insert.html) 将对象插入列表 |
| 6    | [list.pop([index=-1\])](https://www.runoob.com/python3/python3-att-list-pop.html) 移除列表中的一个元素（默认最后一个元素），并且返回该元素的值 |
| 7    | [list.remove(obj)](https://www.runoob.com/python3/python3-att-list-remove.html) 移除列表中某个值的第一个匹配项 |
| 8    | [list.reverse()](https://www.runoob.com/python3/python3-att-list-reverse.html) 反向列表中元素 |
| 9    | [list.sort( key=None, reverse=False)](https://www.runoob.com/python3/python3-att-list-sort.html) 对原列表进行排序 |
| 10   | [list.clear()](https://www.runoob.com/python3/python3-att-list-clear.html) 清空列表 |
| 11   | [list.copy()](https://www.runoob.com/python3/python3-att-list-copy.html) 复制列表 |

# 元组

~~~python
>>> tup1 = ('Google', 'Runoob', 1997, 2000)
>>> tup2 = (1, 2, 3, 4, 5 )
>>> tup3 = "a", "b", "c", "d"   #  不需要括号也可以
>>> tup = ()	#创建空元组
>>> type(tup3)
<class 'tuple'>
~~~

* 元组的元素不能修改，**修改删除都是以元组为基本单位，不是元组元素**

~~~python
>>> tup = ('r', 'u', 'n', 'o', 'o', 'b')
>>> tup[0] = 'g'     # 不支持修改元素
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: 'tuple' object does not support item assignment
>>> id(tup)     # 查看内存地址
4440687904
>>> tup = (1,2,3)
>>> id(tup)
4441088800    # 内存地址不一样了
~~~

> 从以上实例可以看出，重新赋值的元组 tup，绑定到新的对象了，不是修改了原来的对象。

* 使用小括号`()`，用逗号`,`隔开
* 元组运算符	元组索引，截取	同列表一致

## 修改元组

* 元组中的元素值是不允许修改的，但我们可以对元组进行连接组合:创建一个新元组以合并两个旧元组

~~~python
tup1 = (12, 34.56)
tup2 = ('abc', 'xyz')
# 以下修改元组元素操作是非法的。
# tup1[0] = 100
# 创建一个新的元组
tup3 = tup1 + tup2
print (tup3)
#输出结果：
#(12, 34.56, 'abc', 'xyz')
~~~

## 删除元组

* 元组中的元素值是不允许删除的，但我们可以使用`del 元组`来删除整个元组

~~~python
tup = ('Google', 'Runoob', 1997, 2000)
print (tup)
del tup
print ("删除后的元组 tup : ")
print (tup)
#输出结果：
#删除后的元组 tup : 
#Traceback (most recent call last):
#  File "test.py", line 8, in <module>
#    print (tup)
#NameError: name 'tup' is not defined
~~~

## 嵌套元组

~~~python
>>> t = 12,14,1241,12325
>>> t
(12, 14, 1241, 12325)

'''-------------------------'''
>>> u = t,24,113,222	#** 区别于下面的**
>>> u
((12, 14, 1241, 12325), 24, 113, 222)

'''-------------------------'''
>>> y = t,(1,2,3,4)		#**
>>> y
((12, 14, 1241, 12325), (1, 2, 3, 4))
~~~

## **？**元组内置函数

| 序号 | 方法及描述                               | 实例                                                         |
| :--- | :--------------------------------------- | :----------------------------------------------------------- |
| 1    | len(tuple) 计算元组元素个数。            | `>>> tuple1 = ('Google', 'Runoob', 'Taobao') ` <br />`>>> len(tuple1) ` <br />`3  ` |
| 2    | max(tuple) 返回元组中元素最大值。        | `>>> tuple2 = ('5', '4', '8')` <br />` >>> max(tuple2)`<br />` '8' ` |
| 3    | min(tuple) 返回元组中元素最小值。        | `>>> tuple2 = ('5', '4', '8') `<br />`>>> min(tuple2)`<br />` '4'  ` |
| 4    | tuple(iterable) 将可迭代系列转换为元组。 | `>>> list1= ['Google', 'Taobao', 'Runoob', 'Baidu']` <br />`>>> tuple1=tuple(list1) `<br />`>>> tuple1 `<br />`('Google', 'Taobao', 'Runoob', 'Baidu')` |