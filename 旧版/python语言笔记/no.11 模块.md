[toc]

# 模块 —— import

## 概念

> 模块 就好比是 工具包，想要使用这个工具包中的工具，就需要 导入 **import** 这个模块
>
> 每一个以扩展名 **.py** 结尾的python源代码文件都是一个模块
>
> 在模块中定义的 **全局变量，函数，类** 都是模块中能够提供给外界直接使用的工具

**步骤：**

1. 在 **一个python文件**中定义变量或者函数
2. 然后在**另一个文件**中使用**import导入**这个模块
3. 导入之后就可以使用 **模块名.变量 / 模块名.函数 / 模块名.对象** 的方式，使用这个模块中定义的变量或者函数

* 模块 ：可以让曾经编写过的代码 方便的被复用

* 模块名 ：也是一个标识符，**不能以数字开头**，与关键字重名。可以由字母下划线和数字组成。

* pyc文件 ：调用模块时python解释器将**模块的源代码转换成字节码**（二进制）存储的文件
  python这样保存字节码是作为一种**启动速度的优化**

## 模块的导入方式

### import导入

* 在导入模块时

  * 每个导入应该独占一行

    ~~~python
    import 模块名1
    import 模块名2
    ~~~
    
  * 如果模块的名字太长，可以使用 **as** 指定模块的名称，以方便在代码中的使用

    ~~~python
  import 模块名1 as 模块别名
    ~~~

    > 模块别名 应该符合 大驼峰命名法

* 在导入后

  * 通过 `模块名.` 使用模块提供的工具 —— 全局变量，函数，类


### from...import 导入

> 如果希望 从某个模块中，**导入部分工具**，就可以使用 from...import 的方式
>
> vs `import` 是一次性把模块中**所有工具全部导入**，并且通过 模块名/别名 访问

* 导入前

  ~~~python
  from 模块名1 import 工具名
  ~~~

* 在导入后

  * 不需要通过 `模块名.`  ，直接使用模块提供的工具 —— 全局变量，函数，类

  **注意：**

  ​		如果两个模块，存在同名的函数，那么后导入模块的函数，会覆盖掉先导入的函数
  ​		解决办法： 用 **as** 关键字 给其中一个工具起一个别名 

* from 模块1 import * （不推荐使用）
  导入模块中的所有工具，并且不需要通 过 `模块名.`

### 模块的搜索顺序

1. 先搜索当前目录指定模块名的文件，如果有就直接导入
2. 如果没有，再搜索系统文件

* 在开发时，给文件起名，不要和 系统的模块文件重名
* python的每一个模块都有一个内置属性 `__file__` 可以查看模块的完整路径：
  `print（模块名1.__file__)`

## `__name__` 属性

**前提：**

1. 原则上每一个文件都应该是可以被导入的
   一个独立的 python 文件 就是一个模块
   在导入文件时，文件中所有没有任何缩进的代码都会被执行一遍
2. 在实际开发中，每一个模块都是独立开发的，大多都有专人负责
   开发人员 通常会在 模块下方增加一些测试代码，仅在模块中使用，而被导入到其他文件中不需要执行

**`__name__` 属性**

* 如果 是 **当前执行的程序**  `__name__`是 `__main__`
  ![image-20210111143745537](E:\软工\typora笔记\python语言笔记\no.11 模块.assets\image-20210111143745537.png)

* 如果 是 **被其他文件导入** 的， `__name__` 就是模块名
  ![image-20210111143843494](E:\软工\typora笔记\python语言笔记\no.11 模块.assets\image-20210111143843494.png)

* 可以做到测试模块的代码只在测试情况下被运行，而在被导入时不会被执行
  <img src="E:\软工\typora笔记\python语言笔记\no.11 模块.assets\image-20210111144146877.png" alt="image-20210111144146877" style="zoom:43%;" /><img src="C:\Users\shahuhu\AppData\Roaming\Typora\typora-user-images\image-20210111144241560.png" alt="image-20210111144241560" style="zoom: 43%;" />

# 包（package）

概念：

1. 包是一个包含多个模块的特殊目录
2. 目录下有一个特殊的文件 `__init__.py`
3. 包名 的命名方式 和 变量名 一致
4. 使用 `import 包名` 可以一次性导入包 中所有的模块

* 创建方式
  * 直接创建package目录，package自带 `__init__.py`
  * 先创建 directory 目录，再手动创建 名为 `__init__.py` 的 python文件

* __init__.py 文件
  要在外界使用 包中的模块，需要在 __init__.py 中指定 对外界提供的模块列表

  ~~~python
  from . import 模块1
  from . import 模块2
  ~~~

## 制作模块

* 制作模块压缩包

  1. 创建 setup.py 的文件
     文件内的内容固定模式：

     ~~~python
     from distutils.core import setup
     
     setup(name=" ", # 包名
          version=" ", # 版本
          description=" "， # 描述信息
          long_description=" ", # 完整描述信息
          author=" ", # 作者
          author_mail=" ", # 作者邮箱
          url=" ", # 个人主页
          py_modules=[" ",
                     "  "] # 包中包含的模块
          )
     ~~~

  2. 构建模块 

     `$ python3 setup.py build`
     ![image-20210111155248418](E:\软工\typora笔记\python语言笔记\no.11 模块.assets\image-20210111155248418.png)

  3. 生成发布压缩包
     `$ python3 setup.py sdist`

     ![image-20210111155338698](E:\软工\typora笔记\python语言笔记\no.11 模块.assets\image-20210111155429965.png)![image-20210111155610997](C:\Users\shahuhu\AppData\Roaming\Typora\typora-user-images\image-20210111155610997.png)

* 安装模块压缩包

  1. `$ tar -zxvf 压缩包名.tar.gz`

     ![image-20210111160244357](E:\软工\typora笔记\python语言笔记\no.11 模块.assets\image-20210111160244357.png)

     解压后的 PKG - INFO 文件：
     ![image-20210111160719584](E:\软工\typora笔记\python语言笔记\no.11 模块.assets\image-20210111160719584.png)

  2. `$ sudo python3 setup.py install`

* 卸载模块压缩包

  1. `$ cd 文件路径 # 文件路径可以用 包名.__file__ 查看`
  2. `$ sudo rm -r 包名*`

  ![image-20210111162039931](E:\软工\typora笔记\python语言笔记\no.11 模块.assets\image-20210111162039931.png)

* pip 安装第三方模块

  第三方模块通常是指由知名的第三方团队开发的并且被程序员广泛使用的 python 包 / 模块
  pip 是一个现代的， 通用的 python 包管理工具

  * 安装和卸载命令：

    ~~~python
    # 将模块安装到 python 2.x 环境
    $ sudo pip install pygame
    $ sudo pip upinstall pygame
    
    # 将模块安装到 python 3.x 环境
    $ sudo pip3 install pygame
    $ sudo pip3 upinstall pygame
    ~~~

    