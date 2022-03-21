[toc]

# shell

* shell的简单写法和复杂写法
  * 简单命令的堆积
  * 复杂程序设计的代码

* 脚本：(js 前端) (jsp php 后端) (shell 内置脚本)

* Linux中默认的shell是/bin/bash

* 编写规范
  * 代码规范

    > #!/bin/bash	[指定告知系统当前这个脚本要使用的shell解释器]
    >
    > ……
    >
    > shell相关指令
    >
    > ……

  * 文件命名规范	

    > 文件名.sh	[.sh是Linux下bash shell 的默认后缀]

* 执行shell脚本`./xxx.sh`，脚本必须得有**执行权限**

  > 运行时一定要写成`./xxx.sh`,而不是`xxx.sh`,运行其他二进制得程序也一样，直接写`xxx.sh`，Linux系统会去`PATH`里寻找，而只有`/bin` `/sbin` `/usr/bin` `/usr/sbin` 等在`PATH`里。

## 简单写法——命令行堆积

* 命令行中`#echo 内容` 输出命令

  > 输出得内容如果包含字母和符号，则需要用引号包括起来，如果是纯数字可以包也可以不包

## 复杂写法——程序设计代码

### 变量

* 在使用变量的时候一定需要在变量名前面添加一个`$`符号（在php也是同理）
* 变量名后面的等号左右不能有空格，空格为断点识别指令
* 不能使用`bash`里的关键词（可以用`help`命令查看保留关键字）

> 单引号 '' ：不能识别变量，只能原样输出，不能转义
>
> 双引号 "" ：识别变量，实现转义
>
> 反引号 `` ：当脚本中需要执行一些指令并且将执行的结果赋给变量的时候需要使用

### 接受用户输入

* `read -p 提示信息 变量名`

  ~~~shell
  #!/bin/bash
  read -p '请输入需要创建的文件路径：' filepath
  touch $filepath
  echo '文件创建成功！'
  ls -l $filepath
  #输出结果：
  #请输入需要创建的文件路径：xxx
  #文件创建成功！
  #--------- xxxx xxx xxx xxxxxx xxxx 
  ~~~

### 删除变量

* `unset 变量名`

  ~~~shell
  #!/bin/bash
  b=20
  echo $b
  unset b
  echo $b
  #输出结果：
  #20
  #
  ~~~

* 会有空行，只要是有`echo`就会有行显示

### 条件语句

~~~shell
#第一种写法：
if condition
then
	command1
	command2
	...
fi
#第二种写法：
if condition
then
	command1
	command2
	...
else
	command
fi
#第三种写法：
if condition1
then
	command1
elif condition2
then 
	command2
else
	command3
fi
~~~

### 运算符

> 条件表达式要放在方括号之间，并且表达式和运算符之间要有空格`[ $a 运算符 $b ]`

* 算术运算符
  * 因为`bash`不支持算术运算；**算术运算用  \`expr 表达式\` **	eg: \`expr $a + $b\`；
  * 算术运算符中的`*`在shell中要用`\*`表示 \`expr $a \\* $b\`；其他不变

  >  \+	\-	\*	/	%	=	==	!=

* 关系运算符

  | 运算符                    | 说明 |
  | ------------------------- | ---- |
  | `-eq` equal               | ==   |
  | `-ne` not equal           | !=   |
  | `-gt` great than          | >    |
  | `-lt` less than           | <    |
  | `-ge` great than or equal | \>=  |
  | `-le` less than or equal  | <=   |

  * 只支持数字，不支持字符串，除非字符串是数字

* 逻辑运算符

  | 运算符   | 说明 |
  | -------- | ---- |
  | ！       | ！   |
  | `-o` or  | \|\| |
  | `-a` and | &&   |

* 字符串运算符

  | 运算符 | 说明               |
  | ------ | ------------------ |
  | =      | ==                 |
  | ！=    | ！=                |
  | -z     | 字符串为0，true    |
  | -n     | 字符串不为0，true  |
  | str    | 字符串不为空，true |

* 文件测试运算符

  | 运算符  | 说明                                           |
  | ------- | ---------------------------------------------- |
  | -d file | 文件是目录，true                               |
  | -f file | 文件是普通文件（既不是目录也不是硬件设备，true |
  | -r file | 文件可读，true，只要有一个可读即true           |
  | -w file | 文件可写，true，只要有一个可写即true           |
  | -x file | 文件可执行，true，只要有一个可执行即true       |
  | -s file | 文件不空，true                                 |
  | -e file | 文件（目录）存在，true                         |

* shell脚本附带选项

  * 正常的命令执行过程

    > tail -10 access.log
    >
    > 调用tail指令
    >
    > 系统把后续选项传递给tail
    >
    > tail先去打开指定的文件
    >
    > 取出最后十行

  *  自己写的shell像内置命令一样传递一些选项

    > 传递：#./test.sh	a	b	c
    >
    > 接收：`$0` 表示`#./test.sh`
    >
    > ​			`$1` 表示`a`
    >
    > ​			`$2` 表示`b`
    >
    > ​			`$3` 表示`c`
    >
    > 接收可以用`$`加上选项对应的序号即可,$1 $2就是变量

    * 选项的创建

    ~~~shell
    #!/bin/bash
    if [ $1 = '-add' ]
    then
    useradd $2
    else
    userdel -r $2
    fi
    ~~~

    * 别名的创建

    ~~~
    //别名创建
    #vim ~/.bashrc
    alias user='/root/test.sh'
    //切换用户，更新命令
    su
    ~~~

    

