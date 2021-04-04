---
title: PyPython核心编程--Python起步
date: 2018-04-18 12:54:29
categories: 编码
tags: Python
---
### Hello world
由于Python的简洁优雅，它输出Hello world只需要敲打出如下代码：
```python
print('Hello world!')

# 或如下
myString = "Hello world!"
print(mystring)
```
### 程序输入和raw_input()内建函数
Python的raw_input()<font color = 0xff000000>(Python 3.x 中变成了input)</font>函数类似C++中的std::cin或是scanf.

```python
user = raw_input('Please input yours name:')
```

如需查询内建函数的使用，或参数说明，输入如下代码：
```python
help(raw_input)
```

> 核心风格：一直在函数外做用户交互
> 新手在作输出或输入操作时，容易直接使用print或raw_input进行操作。但函数一般情况应该只作输入参数，返回结果。从用户输入得到数据，调用函数进行输出
### 注释
Python中的注释类似Unix_shell中的风格，都是用**#**开始某行注释。

```python
# 这里是注释
# 通常按照代码格式规范，会在#后边添加空格
```
而在Python中添加函数说明、类说明或是模块说明时通常使用**DocString**

```python
# DocString存在一个惯例：
# 首行以英文字母大写开始（前提需要注释为英文），句号结束
# 然后空一行，第三行开始详细说明

def function():
'''这里时函数说明

这里是函数详细说明'''
```
### 运算符
和绝大多数变成语言一样，Python中的标准算术运算符以你熟悉的方式工作

```python
# 加法
value = 2 + 3
# 减法
value = 3 - 2
# 乘法
value = 3 * 2
# 乘方
value = 3 ** 2  # 结果为 9
# 传统除法
value = 2 / 4	# 结果为 0.5
# 浮点除法
value = 2 // 4	# 结果为 0
```
#### 赋值运算符
标识符						  |				描述
------------------------------|------------------------------
=                             |				赋值
+=							  |				加法赋值
-=							  |				减法赋值
*=							  |				乘法赋值
/=							  |				除法赋值
%=							  |				取模赋值
**=							  |				幂赋值
//=							  |				取整赋值
#### 比较运算符
标识符			|		表达式的值
 ---------------|---------------------
		  >     |  大于
		 \>=    |  大于等于
		 <      |  小于
		 <=     |  小于等于
		 ==     |  等于
!=(或<>,不建议使用)| 不等于
#### 逻辑运算符
标识符					  |		表达式的值
--------------------------|--------------------------
and						  |		与
or						  |		或
not						  |		非
#### 位运算
标识符			 |	表达式的值  |描述
-----------------|------------|-------------------------
&				 |	与运算	  | 同时为1，则为1	
\	    		 |	或运算	  |	其中一个为1，则为1
~				 |	非运算	  |	遇1为0
^				 |  异或运算    | 对立为0与1，则为1
\>>				 |	右移运算符  | 右移指定位数
<<			     |	左移运算符  |高位丢弃，低位补0
#### 成员运算符
标识符			   |	     描述
-------------------|-------------------
in				   |在列表中找到某值则返回True
not in			   |不在列表中则返回True
#### 身份运算符

标识符			|		描述
----------------|----------------
is				| x is y类似id(x)==id(y),若引用的是同一个对象，则返回True
not is			| x not is y 类似id(x)!=id（y）,若引用的不时同一对象，则返回True

>核心风格：合理使用括号增强代码可读性，在很多场合使用括号都是一个好主意。

### 变量和赋值
Python是动态类型语言，也就是不需要预先声明变量的类型。变量类型在赋值的那一刻确定
### 数字
Python支持5种基本数字类型，其中三种是整形类型

 - int          (有符号整形)
 - long(长整形)
 - bool(布尔值)
 - float(浮点值)
 - complex(复数值)

Python长整形表达的数值范围远远超过C/C++表示范围，事实上，长整形仅受限于用户计算机虚拟内存总数。类似Java的BigInteger。

长远看，int与long最终将融为一种数据类型，在Python2.3之前的版本中会出现的整数型溢出问题，之后已优化为自动转化为long数据类型。

而布尔类型，在参与与数字类型运算时True会当作1，False会当作0

其实还存在第6种数据类型**decimal**，用于十进制浮点数。不过它并非内建数据类型，需要导入模块**decimal**
### 字符串
Python中字符串被定义为引号之间的字符集合，其引号可以是***单引号***，***双引号***和***三引号***（可以用来包含特殊字符）

使用索引运算符**[]**和切片运算符**[:]**可以得到子串。
字符串特有的索引规则，第一个字符索引为0,最后一个字符索引为**长度减1**或**-1**

 **+** 号可用于连接字符串，***** 可用于重复字符串
 

```python
In [1]: mystring = 'Hello World'

In [2]: print(mystring)
Hello World

In [4]: print(mystring[0:5])
Hello

In [5]: print(mystring[0:5] * 3)
HelloHelloHello

In [6]: print(mystring[0:])
Hello World

In [7]: print(mystring[:-1])
Hello Worl

In [8]: print(mystring[:len(mystring)])
Hello World

In [10]: mystring + ',Hello China!'
Out[10]: 'Hello World,Hello China!'

In [12]: mystring = '''python is \
    ...: so cool'''

In [13]: print(mystring)
python is so cool
```
### 列表(List)和元组(Tuple)
列表与元组即数组，它可以容纳任意数量任意数据类型的Python对象。都可以利用索引访问其中的元素。

列表与元组的区别：
	

 - 列表内容是用 ***[]*** 包裹或是声明，而元组是用 ***()*** 包裹或是声明
 - 列表元素的个数及其中的值可以改变，元组的个数和其中非引用类型值不可改变(***只读***)，若元组中包含某列表子项，则改变列表，相应元组中的列表也改变。
 - 都可以使用索引及切片得到子集

#### 列表
    ```python
    # 列表推导式生成列表
    In [34]: lst = [v for v in randint(10,size = 10)]

    In [35]: lst
    Out[35]: [4, 3, 1, 7, 0, 4, 3, 7, 3, 5]

    In [36]: lst[2:8]
    Out[36]: [1, 7, 0, 4, 3, 7]
    # 切片访问
    In [40]: lst[2:-2]
    Out[40]: [1, 7, 0, 4, 3, 7]
    # 获得列表长度
    In [41]: len(lst)
    Out[41]: 10
    # 索引访问
    In [42]: lst[-2]
    # 删除元素
    Out[42]: 3
    In [43]: del lst[2]

    In [45]: print(lst)
    [4, 3, 7, 0, 4, 3, 7, 3, 5]
    # 删除间隔元素
    In [46]: del lst[::2]

    In [47]: print(lst)
    [3, 0, 3, 3]
    # 更多列表操作及成员方法使用以后章节详述
    ```
#### 元组
```python
# 列表方式初始化元组
In [64]: tuples = tuple([ v for v in randint(10,size = 10)])

In [65]: tuples
Out[65]: (8, 1, 6, 4, 9, 7, 4, 8, 9, 3)

In [66]: tuples[2]
Out[66]: 6

In [67]: tuples[2:6]
Out[67]: (6, 4, 9, 7)
# 尝试改变元组内子项，报错
In [68]: tuples[3] = 5
---------------------------------------------------------------------------
TypeError                                 Traceback (most recent call last)
<ipython-input-68-a89880fbf86d> in <module>()
----> 1 tuples[3] = 5

TypeError: 'tuple' object does not support item assignment
```
### 字典(dictionary)
字典是Python中映射数据类型，工作原理类似Perl中的关联数组或者哈希表，由键-值（key-value）对构成
值可以是任意类型的Python对象，*键必须为不可变类型*

```python
In [14]: dic = {\
    ...: 0:'Monday',
    ...: 1:'Tuesday',
    ...: 2:'Wednesday',
    ...: 3:'Thursday',
    ...: 4:'Friday',
    ...: 5:'Saturday',
    ...: 6:'Sunday'
    ...: }

In [15]: dic
Out[15]:
{0: 'Monday',
 1: 'Tuesday',
 2: 'Wednesday',
 3: 'Thursday',
 4: 'Friday',
 5: 'Saturday',
 6: 'Sunday'}
 
In [16]: dic[6]
Out[16]: 'Sunday'
```
### 代码块及缩进对齐
空白在Python中是很重要的。事实上行首空白非常重要，它称为缩进。在逻辑行首的空白（空格或制表符）用来决定逻辑行的缩进层次，从而用来决定语句的分组。

> **逻辑行**:Python的单个语句就是一个逻辑行
> **物理行**:即肉眼所看到的一行
> *默认Python希望每个物理行只由一个逻辑行*
> 如果希望某一物理行含有多个逻辑行（仅当此物理行语句过长时），请用分号（;）分隔每个逻辑行
> **强烈建议**：每个物理行仅仅使用一个逻辑行，过长时，请使用行连接，即使用反斜杠（\）

这意味着同一层次的语句必须由相同的缩进。每一组这样的语句称为一个块。
缩进不规范将引发错误，如下例子：

```python
introduction = "I'm Quenwaz"
	print introduction         # 此语句将引发错误
print introduction
```

```python

    print(introduction)         # 此语句将引发错误
    ^
IndentationError: unexpected indent
```

> **如何缩进**
> 不要混合使用制表符和空格来缩进，因为这*在跨越不同的平台的时候，无法正常工作*。强烈建议 你在每个缩进层次使用 单个制表符 或 两个或四个空格 。
> 选择这三种缩进风格之一。更加重要的是，选择一种风格，然后一贯地使用它，即 只 使用这一种风格。
> 《简明 Python 教程》

### if语句
标准if语句仅仅在格式上与C/C++语句上有所区别，即if空格后即是判定条件，冒号（：）结束条件判定，进入其子语句块。

格式如下：

```
if True:
    print('Hello world！')
else:
    print('Hello china')
```
### while循环
while循环类似if语句，注意各语句块缩进，格式如下：

```
while True:
    print('Hello China!')
else:
    print('Hello World!')
```
### for循环与内建函数ange()
Python中的for循环与传统的for循环不一样的地方，即它可以用来进行迭代序列或是迭代器。

如下迭代某列表：

```
In [4]: for item in ['e-mail', 'net-surfing', 'homework', 'chat']:
   ...:     print(item)
   ...:
e-mail
net-surfing
homework
chat
```
迭代某字符串

```python
In [12]: str = "I'm Quenwaz"

In [13]: for c in str:
    ...:     print(c)
    ...:
I
'
m

Q
u
e
n
w
a
z

```
**range内建函数**

```
# range内建函数为左闭右开区间，第三各参数为步长，
# 步长设置为负数，则逆序
In [15]: for num in range(0,10,2):
    ...:     print(num)
    ...:
0
2
4
6
8

In [16]: for num in range(0,10):
    ...:     print(num)
    ...:
0
1
2
3
4
5
6
7
8
9
```
### 列表推导式

```
# 列表推导式请从for语句开始逻辑直到语句结束
# 再到列表开始直到for语句结束
In [21]: lst =  [i * 2 for i in range(10)]

In [22]: lst
Out[22]: [0, 2, 4, 6, 8, 10, 12, 14, 16, 18]
```
### 文件和内建函数open()、file()
如何打开文件

```python
# open()第一个参数为文件路径，第二各参数为读写权限
# r 读取 w 写入 a 添加 b 表示二进制访问 
# 若函数成功将返回文件句柄，之后对此文件的操作对针对此句柄展开
In [25]: handle = open('C:\\Users\\Quenwaz\\Desktop\\test.png','r')

```
其他文件属性，请自行查询相关文档，或者以如下方式查看对象属性：

```python
In [29]: dir(handle)
Out[29]:
['_CHUNK_SIZE',
 '__class__',
 '__del__',
 '__delattr__',
 '__dict__',
 '__dir__',
	...
]
```
或查看是否包含某属性

```python
In [30]: hasattr(handle,'write')
Out[30]: True
```
### 错误与异常
编译时会检查语法错误，Python也允许再程序运行时检测错误。当检测到一个错误时，Python解释器就会引发异常，并显示异常详细信息。便于程序员快速定位修复。
需要给给代码添加错误检测或异常处理，需要添加**try-except**语句当中。

```python
In [37]: try:
    ...:     num = input('please input a number:')
    ...:     num = int(num)
    ...: except ValueError:
		     # 抛出错误
    ...:     print('input error')
    ...: finally:
			 # 不论是否执行成功，都需要执行finally语句块
    ...:     print('finally output a value')
    ...:
please input a number:fdsa
input error
finally output a value
```

## 函数
Python中的函数定义如下

```python
In [39]: printValue('Hello Quenwaz!')
Hello Quenwaz!

In [42]: def returnValue(value):
    ...:     return 2 * value
    ...:

In [43]: v = returnValue(100)

In [44]: v
Out[44]: 200
```
与其他编程语言一样，可以传参，与返回值
默认Python是通过引用调用，如果函数输入的是不可变数据类型，则函数按照值传递，若输入的是可变数据类型（如列表，字典等），则是引用传递。

其他函数可以默认传参，关键字传参等，后续章节将详述。

### 类
类是面向对象编程的核心，它扮演相关数据与逻辑的容器角色。它提供了创建“真实”对象的蓝图。此处仅稍微讲解如何声明类。

```python
class Person:
    '''a Person class
    '''
    def __init__(self, age, sex, name):
        '''初始化函数，类似构造函数

        :param age: 年龄
        :param sex: 性别
        :param name: 名字
        '''
        self.name = name
        self.age = age
        self.sex = sex

    def __del__(self):
        '''类似析构函数，对象销毁时调用

        :return:
        '''
        self.name = ''
        self.age = 0
        self.sex = ''

    def print_info(self):
        print("我是%s,%s,今年%d岁" %(self.name, self.sex, self.age))

P = Person(18, '男性', 'Quenwaz')
P.print_info()

[out]:我是Quenwaz,男性,今年18岁

```

> 什么是 self ? 它是类实例自身的引用。其他语言通常使用一个名为 this 的标识符。

### 模块
模块是一种组织形式，它将有联系的Python代码组织到一个独立文件中。模块可包含可执行代码，函数或类。

**如何导入一个模块**

```python
# 此处导入内置模块sys与time
import sys, time

# 导入后可直接用模块名调用其内部函数或变量以及类
In [3]: import sys, time

In [4]: print(sys.platform)
win32

In [5]: print(time.localtime)
<built-in function localtime>

# 或是将导入的模块取一个别名
In [7]: import time as tm

In [8]: tm.localtime
Out[8]: <function time.localtime>
```

### 部分内建函数
函数|描述
-------------|-----------------------------------
dir([obj])	|显示对象的属性列表，如果没有提供参数，将返回全局变量的名字。
help([obje])|以一种整齐美观的形式显示对象的文档字符串。若不提供任何参数，将进入交互式帮助界面。
int(obj)|将一个对象转换成整形
len(obj)|返回对象的长度
open(fn,  mode)	|以mode('r'=读  'w'=写)方式打开一个名字为fn的文件
range(start:int,stop:int,step:int=1)|返回一个左闭右开的列表，步长为step
input(str)|等待用户输入一个字符串，其中str参数为提示字符串
str(obj)|将一个对象转换成字符串
type(obj)|返回对象类型




<br/>
----------
<small>此文为《Python核心编程》（第二版）学习笔记
若有错误处，烦请指出。</small>

---
<small><font color= "gray">本文[Quenwaz](http://quenwaz.github.io)版权所有，转载请说明出处</font></small>