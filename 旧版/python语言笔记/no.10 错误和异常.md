## 语法错误（解析错）

~~~python
>>> while True print('Hello world')
  File "<stdin>", line 1, in ?
    while True print('Hello world')
                   ^
SyntaxError: invalid syntax
    
'''语法分析器：
1. 指出出错的一行
2. 在最先找到的错误位置标记一个小小的箭头	^ '''
~~~

## 异常

运行期检测到的错误称为异常

~~~python
'''大多数的异常都不会被程序处理，都以错误信息的形式展现在这里:'''

>>> 10 * (1/0)             # 0 不能作为除数，触发异常
Traceback (most recent call last):
  File "<stdin>", line 1, in ?
ZeroDivisionError: division by zero
>>> 4 + spam*3             # spam 未定义，触发异常
Traceback (most recent call last):
  File "<stdin>", line 1, in ?
NameError: name 'spam' is not defined
>>> '2' + 2               # int 不能与 str 相加，触发异常
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: can only concatenate str (not "int") to str
~~~

## 异常处理

### try / except

![img](E:\软工\typora笔记\python语言笔记\no.10 错误和异常.assets\try_except.png)

~~~python
while True:
    try:
        x = int(input("请输入一个数字: "))
        break
    except ValueError:
        print("您输入的不是数字，请再次尝试输入！")
        
'''try 语句按照如下方式工作:
	首先，执行 try 子句（在关键字 try 和关键字 except 之间的语句）。
	如果没有异常发生，忽略 except 子句，try 子句执行后结束。
	如果在执行 try 子句的过程中发生了异常，那么 try 子句余下的部分将被忽略。
	如果异常的类型和 except 之后的名称相符，那么对应的 except 子句将被执行。
	如果一个异常没有与任何的 except 匹配，那么这个异常将会传递给上层的 try 中。'''
~~~

* except 的多种形式

  ~~~python
  ''' 1. 一个except子句可以同时处理多个异常，这些异常将被放在一个括号里成为一个元组，例如:'''
  except (RuntimeError, TypeError, NameError):
      pass
  
  ''' 2. 一个 try 语句可能包含多个except子句，分别来处理不同的特定的异常。最多只有一个分支会被执行。'''
  except OSError as err:
      print("OS error: {0}".format(err))
  except ValueError:
      print("Could not convert data to an integer.")
      
  ''' 3. 最后一个except子句可以忽略异常的名称，它将被当作通配符使用。'''
  except OSError as err:
      print("OS error: {0}".format(err))
  except ValueError:
      print("Could not convert data to an integer.")
  except:
      print("Unexpected error:", sys.exc_info()[0])
      raise
  ~~~

### try / except ... else

![img](E:\软工\typora笔记\python语言笔记\no.10 错误和异常.assets\try_except_else.png)

~~~python
'''在 try 语句中判断文件是否可以打开，如果打开文件时正常的没有发生异常则执行 else 部分的语句'''
for arg in sys.argv[1:]:
    try:
        f = open(arg, 'r')
    except IOError:
        print('cannot open', arg)
    else:
        print(arg, 'has', len(f.readlines()), 'lines')
        f.close()
~~~

* 异常处理并不仅仅处理那些直接发生在 try 子句中的异常，而且还能处理子句中调用的函数（甚至间接调用的函数）里抛出的异常。

  ~~~python
  >>> def this_fails():
          x = 1/0
     
  >>> try:
          this_fails()
      except ZeroDivisionError as err:
          print('Handling run-time error:', err)
     
  Handling run-time error: int division or modulo by zero
  ~~~

### try  - finally

![img](E:\软工\typora笔记\python语言笔记\no.10 错误和异常.assets\try_except_else_finally.png)

~~~python 
'''finally 语句无论异常是否发生都会执行'''
try:
    runoob()
except AssertionError as error:
    print(error)
else:
    try:
        with open('file.log') as file:
            read_data = file.read()
    except FileNotFoundError as fnf_error:
        print(fnf_error)
finally:
    print('这句话，无论异常是否发生都会执行。')
~~~

## 抛出异常——raise

![img](E:\软工\typora笔记\python语言笔记\no.10 错误和异常.assets\raise.png)

~~~python
'''raise 唯一的一个参数指定了要被抛出的异常。它必须是一个异常的实例或者是异常的类（也就是 Exception 的子类）'''
x = 10
if x > 5:
    raise Exception('x 不能大于 5。x 的值为: {}'.format(x))
    
Traceback (most recent call last):
  File "test.py", line 3, in <module>
    raise Exception('x 不能大于 5。x 的值为: {}'.format(x))
Exception: x 不能大于 5。x 的值为: 10
~~~

* Exception 可忽略——如果你只想知道这是否抛出了一个异常，并不想去处理它，那么一个简单的 raise 语句就可以再次把它抛出。

  ~~~python
  >>> try:
          raise NameError('HiThere')
      except NameError:
          print('An exception flew by!')
          raise
     
  An exception flew by!
  Traceback (most recent call last):
    File "<stdin>", line 2, in ?
  NameError: HiThere
  ~~~

## 用户自定义异常（学完模块和包在学这个）



## 定义清理行为 —— finally

~~~python
'''不管 try 子句里面有没有发生异常，finally 子句都会执行。'''

>>> try:
...     raise KeyboardInterrupt
... finally:
...     print('Goodbye, world!')
...
Goodbye, world!
Traceback (most recent call last):
  File "<stdin>", line 2, in <module>
KeyboardInterrupt

'''如果一个异常在 try 子句里（或者在 except 和 else 子句里）被抛出，而又没有任何的 except 把它截住，那么这个异常会在 finally 子句执行后被抛出。'''

>>> def divide(x, y):
        try:
            result = x / y
        except ZeroDivisionError:
            print("division by zero!")
        else:
            print("result is", result)
        finally:
            print("executing finally clause")
   
>>> divide(2, 1)
result is 2.0
executing finally clause
>>> divide(2, 0)
division by zero!
executing finally clause
>>> divide("2", "1")
executing finally clause
Traceback (most recent call last):
  File "<stdin>", line 1, in ?
  File "<stdin>", line 3, in divide
TypeError: unsupported operand type(s) for /: 'str' and 'str'
~~~

## 预定义的清理行为—— with

~~~python
'''这段代码当执行完毕后，文件会保持打开状态，并没有被关闭。'''
for line in open("myfile.txt"):
    print(line, end="")
    
'''这段代码执行完毕后，就算在处理过程中出问题了，文件 f 总是会关闭。'''
with open("myfile.txt") as f:
    for line in f:
        print(line, end="")
~~~





