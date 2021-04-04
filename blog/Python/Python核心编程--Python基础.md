---
title: Python核心编程--Python基础
categories: 编码
tags:
  - Python
date: 2018-04-19 22:33:50
---

### 语句与语法
字符与基本规则

字符|规则
:-------------:|:-------------:
\#|注释的开始符号，通常#后空一格
\n|换行
\\|继续上一行
;|两个语句连接在一行中
:|将代码块的头和体分开

- 不同的缩进深度分隔不同的代码块
- python文件即模块

#### 注释(#)
```python
# 注释开始了
'''
这是多行注释
'''

"""
这也是多行注释
"""
```

#### 继续(\\)
若一行语句太长，可以利用**\\**进行分隔，如:
```python
if (weather_is_hot == 1) and \
(shark_warnings ==0):
  send_goto_beach_mesg_to_pager()
```
另外，不使用反斜杠也可以换行，即使用小括号，中括号，花括号都可以多行书写。当然三引号字符串也可以。

建议使用括号进行换行，使语句可读性更高。

#### 代码组由不同缩进分隔
> 核心风格:缩进四个空格宽度，避免使用制表符<br/>
  推荐使用空格替代制表符的原因：因为不同文本编辑器中的制表符代表的空白宽度不统一，**如果你的代码要跨平台，或者不同的编辑器读写，建议不要使用制表符**。

#### 同行书写多个语句(;)
分号(;)允许你将多个语句写在同一行上，语句之间用分号隔开，而这些语句子不能在这行开始新的代码块。如:
```python
import sys; x = 'foo'; sys.stdout.write(x + '\n')
```
显然同行多个语句块大大降低代码可读性，python虽允许但不提倡这么做。

#### 模块
每个python脚本文件都可以被当成是一个模块。如果一个模块驱动太多功能，可以考虑进行模块拆分。

### 变量赋值
等号(=)即赋值
- 增量赋值支持

      +=    -=    *=    /=    %=    **=
      <<=   >>=   &=    ^=    |=   

- 多重赋值
  ```python
  x = y = z = 1
  ```

- 多元赋值
  ```python
  x, y, z = 1, 2, 'a string'
  # 通常建议利用元祖进行多元赋值,更具可读性
  (x, y, z) = (1, 2, 'a string')

  # 交换变量的值,如下
  x, y = 1, 2
  x, y = y, x
  ```

### 标识符
python 标识符规则与大多数计算机语言相似:
- 第一个字符必须为字母或下划线
- 剩下字符可以是字母数字或下划线
- 区分大小写

#### 关键字
如需查看关键字，请执行**help('keywords')**：

    False               def                 if                  raise
    None                del                 import              return
    True                elif                in                  try
    and                 else                is                  while
    as                  except              lambda              with
    assert              finally             nonlocal            yield
    break               for                 not
    class               from                or
    continue            global              pass

#### 专用下划线标识符
- _xxx: 不用'from module import *'导入
- _xxx_：系统定义名字
- __xxx：类中的私有变量名

> 核心风格:避免用下划线作为变量名的开始，因为下划线对解释器有特殊的意义。





<br/>
----------
<small>此文为《Python核心编程》（第二版）学习笔记
若有错误处，烦请指出。</small>

---
<small><font color= "gray">本文[Quenwaz](http://quenwaz.github.io)版权所有，转载请说明出处</font></small>