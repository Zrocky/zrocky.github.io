---
layout: post
title: Python笔记
date: 2016-05-23 18:00:00
tags: Python
categories: Python
---


很早就对`Python`这门语言感兴趣, 终于有时间可以系统地学习, 所以才会有这篇笔记, 笔记本身基于`C`, `OC`, `Swift`三门语言, 相同之处没有太多赘述, 我也是一笔带过, 希望知晓! 我学习的来源主要是廖雪峰的`Python`教程, 本人并不是原创者, 笔记仅为本人自我记录的学习笔记

* [廖雪峰的Python教程](http://www.liaoxuefeng.com/wiki/0014316089557264a6b348958f449949df42a6d3a2e542c000)

## Paython开发基础

### Python安装
在Mac系统中, 系统内置了Python2.7版本, 所以Mac系统用户此时已基本满足编译所需环境。但是随着Python3.x版本的普及,建议使用Python3.x版本进行开发,笔记中代码也是在Python3.x环境下编译的

#### 使用HomeBrew安装Python3

在Terminal通过命令`brew install python3`安装即可

### Python编译器选择
在Mac系统中可以选择Sublime Text作为Python的编译器


### Python3编译说明

虽然我们使用HomeBrew安装了Python3版本，但是因为Mac内置了Python2.7，所以通用的`Python`命令在Mac下寻址的仍然是Python2.7，我们需要使用`Python3`命令来区分两种版本

### 运行Python代码
这里会有两种方式, 一种是讲Python代码写在.py文件中, 利用Terminal命令进行编译

- 在终端运行.py文件

```
$ python3 first.py
```
- 在Terminal的Python交互环境中直接书写Python命令进行编译

```
$ python3
```
在Mac或Linux系统中, 可以在代码最前面加上`#!/usr/bin/env python`, 系统会识别`.py`文件为可执行程序, Windows系统中将会忽略这一句

## Paython语言基础

### 输入输出(I/O)
#### 输出
使用`print()`函数输出, 

```python
print('Hello, World')
```
- 使用`'`表示字符串

- `print()`函数也可以接受多个字符串，使用`,`隔开

- `print()`函数在遇到`,`时会输出一个空格

  ```python
  >>> print('Hello', 'World')
  Hello World
  ```

- `print()`函数也可以打印整数，或者计算结果

  ```python
  >>> print(300)
  300
  >>> print(100 + 200)
  300
  ```

- 混合输出数值和字符串

  ```python
  >>> print('100 + 200 =', 100 + 200)
  100 + 200 = 300
  ```

#### 输入

使用`raw_input("输入提示语: ")`让用户从键盘输入, 括号中可填入友好提醒词

```python
name = raw_input("Please input your name: ")
print "Hello,", name
```
输入的字符会保存在`name`变量中


### 语法规范
#### 注释
使用`#`放在解释语句前面进行注释

#### 大小写区分
严格区分大小写, 混用会造成编译出错


### 数据类型
Python的数据类型与其它开发语言大致相同

整数运算得出的永远是整数

```python
print 10 / 3
```
结果为`3`

如果需要做出精确的计算, 必须带有浮点数

```python
print 10.0 / 3
```
结果为`3.3333333333333335`

#### 字符串
使用`'''...'''`可以打印换行的文本

```python
print '''line1
line2
line3'''
```
输出结果:

```
line1
line2
line3
```

#### 布尔值
包含两个值`True`和`False`(注意区分大小写)

涉及的运算符有`and`,`not`,`or`

```python
# and
print True and False
```
输出结果: `False`

```python
# or
print True or False
```
输出结果: `True`

```python
# not
print not True
```
输出结果: `False`

#### 空值
使用`None`表示空值, 但是空值并不等于`0`


### 变量
Python是一门`动态变量语言`,相对应的有`静态变量语言`, 动态语言的特点是在定义变量时可以不指定变量类型, 而静态语言则必须在定义变量时确定变量的类型

同一个变量可以反复赋值, 并且每次赋值的类型可以不同

```python
a = 10
a = "python is fun"
```

针对变量的赋值操作`a = "ABC"`, Python首先创建了一个`"ABC"`字符串, 然后创建了一个`a`变量, 并且让`a`指向`"ABC"`

### 常量
常量约定俗成使用全大写字母表示, 例如`WIDTH`

### 字符编码
`ASCII`使用一字节保存字符, 包含字母数字和一些符号

`GB2312`等国际语言使用两个字节保存字符

`Unicode`包含所有语言字符

但是`Unicode`的英文数字也是使用2个字节, 所以发明了可变长的编码`UTF-8`, 当是英文数字时, 使用一个字节, 其它字符用多个字节保存

`UTF-8`可变长字符编码

用记事本编辑的时候，从文件读取的`UTF-8`字符被转换为`Unicode`字符到内存里，编辑完成后，保存的时候再把`Unicode`转换为`UTF-8`保存到文件：

![textCode](http://www.liaoxuefeng.com/files/attachments/001387245992536e2ba28125cf04f5c8985dbc94a02245e000/0)

浏览网页的时候，服务器会把动态生成的`Unicode`内容转换为`UTF-8`再传输到浏览器：

![webCode](http://www.liaoxuefeng.com/files/attachments/001387245979827634fd6204f9346a1ae6358d9ed051666000/0)

### Python字符
#### ASCII与字符之间的转换
使用`chr()`和`ord()`函数, 可用于字符与`ASCII`字符之间的互转

```python
# 将ASCII转化为字符
chr(65)
# 将字符转化为ASCII
ord("a")
```
输出结果为:2

```
A
97

```
#### Unicode字符
使用`u"..."`表示`Unicode`字符

```
u'niha'
```
输出结果为`niha`

#### UTF-8字符
使用`encode('utf-8')`方法将`Unicode`字符转化为`UTF-8`字符

 ```python
 u'ABC'.encode('utf-8')
 ```
 输出结果: `'ABC'`

使用`decode('utf-8')`方法将`UTF-8`字符转化为`Unicode`字符

 ```python
 'ABC'.decode('utf-8')
 ```
 输出结果: `u'ABC'`

#### Python中字符的实际应用
使用`len()`函数可以计算出字符的长度

因为Python文件本身也是也是一个文本文件, 所以在文件中使用中文时, 需要使用`UTF-8`编码

在文件开头添加`# -*- coding: utf-8 -*-`, 让编译器以`UTF-8`编码进行编译文件, 不然可能出现乱码

#### 字符格式化
使用格式化字符串进行格式化输出, 格式与C语言保持一致

使用`%`拼接格式化的值, 值用括号包含, 以`,`分隔多个值

```python
print "Hi! %s, your score is %d" % ("Michelle", 97)
```
输出结果: 

```
Hi! Michelle, your scrore is 97
```
常用的占位符有

- %d	整数
- %f浮点数
- %s字符串
- %x十六进制整数

格式化整数和浮点数还可以指定是否补0和整数与小数的位数：

```python
'%2d-%02d' % (3, 1)
'%.2f' % 3.1415926

```
输出结果: 

```
3-01
3.14
```

### 集合类型
#### list类型
使用`[..., ...]`表示一个`list`, `list`内元素可以改变, 且元素类型可以不同

```python
names = ["Rocky", "Shawn", "James"]
```

##### 取值
获取其中一个值, 索引为负数表示从后向前获取索引

```python
names[1]
# 倒数第一个值
names[-1]
```

##### 插值
插入一个值

```python
names.insert(1, "Henry")
```
在末尾拼接一个值

```python
names.append("Harley")
```

##### 删值
删除`list`中某个索引的值

```python
# 删除最后一个值
names.pop()
# 删除指定索引的值
names.pop(1)
```


##### 改值
修改`list`中的某个索引的值

```python
names[1] = "Yuri"
```

##### 排序
使用`sort()`进行排序

```python
name.sort()
```

#### tuple(元组)类型
使用`(..., ...)`表示一个`tuple`, `tuple`内元素不可改变, 元素类型可以不同

因为`tuple`不可改变, 所以更加安全, 在实际使用中推荐尽量使用`tuple`

```ptyhon
names = ("Rocky", "Jack", "James")
```

定义1个元素的`tuple`, 如果元素是数字, 需要使用`(1,)`写法

`tuple`也有可变的形式, 当我们把`tuple`内的元素定义为`list`时, `list`内元素可进行变化, 生成了一个简洁可变的`tuple`

```python
t = ([1, 2], [3, 4])
t[0][1] = 11
```

### 条件判断
使用`if`, `else`, `elif`关键字编写条件判断语句

```python
if a == 1:
	print "HaHa"
elif a == 2:
	print "WaWa"
else:
	print "KaKa"
```

### 循环
`Python`有两种循环, 一种是`for...in`循环, 另一种是`while`循环

#### for...in循环
`for...in`遍历了`list`里所有的元素

```python
for name in names:
	print name
```
输出结果:

```
Rocky
Michael
Jack
```
在实际开发中, 经常需要遍历整数集合, `Python`提供了`range(...)`函数用来遍历整数集合

```python
for index in range(3):
	print index
```
输出结果:

```
0
1
2
3
```

#### while循环
当满足条件时, 则不断调循环体, 当不满足条件时, 则退出循环

```python
a = 3
while < 0:  
	a = a - 1
print a
```

### 字典集合
#### dict
`dict`是使用键值对形式的存储集合, 以`{key : value}`形式表示

```python
score = {"Rocky" : 100, "Jack" : 30}
```

判断`key`是否在`dict`内

返回True或False

```python
"Allen" in score
```
返回None或者自定义的默认返回值

```python
scroe.get("Allen", -1)
```

##### 取值及赋值

```python
print score["Rocky"]
score["Rocky"] = 99
print score["Rocky"]
```
输出结果:

```
100
99
```

##### 删值

```python
score.pop("Jack")
```

#### set
使用`set([key, key])`表示一个set, 创建时需传入一个`list`作为输入集合

`set`是一个元素不重复集合, 也就是说`set`会对所有元素进行去重

```python
score = set([1, 2, 2, 3])
print score
```
输出结果: `set([1, 2, 3])`

##### 添加
使用`add(key)`方法进行添加, 可添加重复值

```python
score.add(5)
```

使用`remove(key)`方法进行删除

```python
score.remove(1)
```

##### 集合运算
`set`的数学本质是无序不重复集合, 所以两个`set`之间可以做交集并集运算

```python
first = set([1, 2, 3])
second = set([2, 3, 4])
print first & second
print first | second
```
输出结果:

```
set([2, 3])
set([1, 2, 3, 4])
```

## 函数
使用抽象方法, 不关心底层实现, 专注问题本身

### 函数调用
在`Python`官网可以查看`Python`内置的函数使用方法

[Python内置函数](https://docs.python.org/2/library/functions.html)

#### 类型转换函数

```python
>>> int('123')
123
>>> int(12.34)
12
>>> float('12.34')
12.34
>>> str(1.23)
'1.23'
>>> unicode(100)
u'100'
>>> bool(1)
True
>>> bool('')
False
```

#### 函数别名
函数名其实是一个指向函数对象的引用, 所以可以将函数名赋值给一个变量, 相当于起别名

```python
a = abs
a(-1)
```

### 函数定义
使用`def`关键字定义函数, 用`return`返回返回值, 如果返回空可以`return None`, 也可以简写为`return`

```python
def rocky(a, b):
	if a > b:
		return a
	else:
		return b

print rocky(1, 2)
```
输出结果: `2`

#### 空函数
如果想定义一个空函数, 可以使用`pass`, 表示什么也不做

```python
def rocky():
	pass
```
也可以使用在判断语句当中

```python
if a > b:
	pass
```

#### 参数检查
系统的函数会自动检查参数类型, 而我们自定义的则没有, 我们可以添加参数检查代码

```python
def rocky(a, b):
	if not isinstance(a, (int)):
		raise TypeError ("bad operand type")
	if a > b:
		return a
	else:
		return b

print rocky(1, 2.1)
```
输出结果: 

```
File "first.py", line 81, in <module>
    print rocky(1, 2.1)
  File "first.py", line 75, in rocky
    raise TypeError ("bad operand type")
```

#### 返回值
函数可以返回多个返回值, 但`Python`本质上是只能返回一个返回值, 之所以能返回多个, 是因为返回值的类型为`tuple`

```python
def rocky(a, b):
	return a, b
```
输出结果:`(1, 2)`

### 函数参数
在`Python`中函数参数有多种实用方法, 使用默认参数、可变参数和关键字参数, 可以不同程度地简化代码

#### 默认参数
当函数中有的值不是经常变化的, 就可以使用默认参数, 让函数调用变得简单

*必选参数必须放在默认参数前面*

*默认参数必须指向不可变对象*, 不然会出现对象不重新初始化问题

```python
# 设置参数b默认值为2
def rocky(a, b = 2):
	return a > b
	
# 使用默认参数, 在函数调用时可以不用写可选参数部分
rocky(1)
# 当参数不是默认值时, 可以改变参数
rocky(1, 4)
```
当多个参数中有的使用默认, 而有的需要自定义值时, 可以指定参数名对应的参数值, 也就意味着, 可以不按顺序填写参数

```python
def rocky(a, b, c = 1, d = 2):
	return a, b, c, d
	
rocky(3, 5, d = 2)
```
#### 可变参数
使用可变参数, 可以让调用对象在定义函数时, 暂时不用管参数的个数

只需要在参数名前面添加`*`, 便可以创建一个可变参数, 在函数内部, 参数获得的是一个`tuple`

```python
def rocky(*numbers):
	return numbers;
	
rocky(1, 2, 3, 4)
```

如果是一个`list`或`tuple`想要调用可变参数函数, 则需要在前面添加`*`

```python
numbers = [1, 2, 3, 4]
rocky(* numbers)
```

#### 关键字参数
关键字参数在必选参数之外, 还可以传入一个`dict`, 用以扩展函数的功能

```python
def rocky(a, b, **kw):
	return a, b, kw
	
# 直接应用key调用	
rocky(1, 3, c = 4, d = 5)

# 使用dict调用
numbers = {"c" : 3, "d" : 4}
rocky(1, 2, **numbers)
```

#### 参数组合
必选参数,默认参数, 可变参数, 关键字参数可以混用, 但是参数定义的顺序必须是：必选参数、默认参数、可变参数和关键字参数。

```python
def rocky(a, b = 2 , *numbers, *kw):
	return a, b, numbers, kw
```
函数`fun(*args, **kw)`可以接收任何参数


