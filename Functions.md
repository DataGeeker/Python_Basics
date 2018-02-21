# <a name="functions"></a>函数

* [def：定义函数](#def)
* [print 函数](#print-function)
* [range 函数](#range-function)
* [type 函数](#type-function)
* [变量作用域](#variable-scope)

<br>

### <a name="def"></a>def：定义函数

```python
#!/usr/bin/python3

# ----- function without arguments -----
def greeting():
    print("-----------------------------")
    print("         Hello World         ")
    print("-----------------------------")

greeting()

# ----- 带参数的函数 -----
def sum_two_numbers(num1, num2):
    total = num1 + num2
    print("{} + {} = {}".format(num1, num2, total))

sum_two_numbers(3, 4)

# ----- 带返回值的函数 -----
def num_square(num):
    return num * num

my_num = 3
print(num_square(2))
print(num_square(my_num))
```

* `def`关键字用于定义函数
* 函数必须在使用前定义
* 一个常见的错误是忘记了`def`声明语句后的`:`
* 函数、控制结构等等代码块都是根据缩进进行区分
    * 推荐使用4个空格缩进
    * [Python文档 - 编码风格](https://docs.python.org/3/tutorial/controlflow.html#intermezzo-coding-style)
* 默认`return`值是`None`
* [在Python怎么将变量传递给函数](http://robertheaton.com/2014/02/09/pythons-pass-by-object-reference-as-explained-by-philip-k-dick/)
* `format`包含在下一个主题中

```shell
$ ./functions.py
-----------------------------
         Hello World
-----------------------------
3 + 4 = 7
4
9
```

**默认参数**

```python
#!/usr/bin/python3

# ----- function with default valued argument -----
def greeting(style_char='-'):
    print(style_char * 29)
    print("         Hello World         ")
    print(style_char * 29)

print("Default style")
greeting()

print("\nStyle character *")
greeting('*')

print("\nStyle character =")
greeting(style_char='=')
```

* 通常，如果函数需要根据相关参数改变，可以设定一个默认的行为

```shell
$ ./functions_default_arg_value.py
Default style
-----------------------------
         Hello World
-----------------------------

Style character *
*****************************
         Hello World
*****************************

Style character =
=============================
         Hello World
=============================
```

* 三引号注释常用于描述函数的功能目的
* 为了避免示例代码分散注意力，这个指南中不会经常为函数和程序使用文档字符串（docstrings）
    * 查看[Docstrings](./Docstrings.md)章节的例子和讨论

```python
def num_square(num):
    """
    returns square of number
    """

    return num * num
```

**进一步阅读**

有许多调用函数和其他声明类型的方式，参考下面的链接获取更多信息：

* [Python文档 - 定义函数](https://docs.python.org/3/tutorial/controlflow.html#defining-functions)
* [Python文档 - 内置函数](https://docs.python.org/3/library/functions.html)

<br>

### <a name="print-function"></a>print函数

* 默认`print`函数会添加换行符
* 这可以通过`end`参数传入我们自己想要的字符串进行更改

```python
>>> print("hi")
hi
>>> print("hi", end='')
hi>>>
>>> print("hi", end=' !!\n')
hi !!
>>>
```

* [help](https://docs.python.org/3/library/functions.html#help)函数可以用来从解释器获取函数的快速使用帮助
* 按`q`从帮助页面退出

```python
>>> help(print)

Help on built-in function print in module builtins:

print(...)
    print(value, ..., sep=' ', end='\n', file=sys.stdout, flush=False)

    Prints the values to a stream, or to sys.stdout by default.
    Optional keyword arguments:
    file:  a file-like object (stream); defaults to the current sys.stdout.
    sep:   string inserted between values, default a space.
    end:   string appended after the last value, default a newline.
    flush: whether to forcibly flush the stream.
```

* 传入`print`函数的多个参数可以通过`,`分隔
* 默认的`sep`值是单个空格

```python
>>> a = 5
>>> b = 2

>>> print(a+b, a-b)
7 3

>>> print(a+b, a-b, sep=' : ')
7 : 3

>>> print(a+b, a-b, sep='\n')
7
3
```

* 当打印变量时会调用[__str__](https://docs.python.org/3/reference/datamodel.html#object.__str__)方法输出字符结果
* 所以，除非需要粘连字符串，我们不需要显式地指定输出类型

```python
>>> greeting = 'Hello World'
>>> print(greeting)
Hello World
>>> num = 42
>>> print(num)
42

>>> print(greeting + '. We are learning Python')
Hello World. We are learning Python
>>> print(greeting, '. We are learning Python', sep='')
Hello World. We are learning Python

>>> print("She bought " + num + " apples")
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: Can't convert 'int' object to str implicitly

>>> print("She bought " + str(num) + " apples")
She bought 42 apples
```

* 作为备用选项，使用多个参数并根据情况改变`sep`分隔符

```python
>>> print("She bought", num, "apples")
She bought 42 apples

>>> items = 15
>>> print("No. of items:", items)
No. of items: 15

>>> print("No. of items:", items, sep='')
No. of items:15
```

* 为了重定向打印输出到[stderr](https://stackoverflow.com/questions/3385201/confused-about-stdin-stdout-and-stderr)，我们更改`file`参数
* 另见[sys.exit()](https://docs.python.org/3/library/sys.html#sys.exit)

```python
>>> import sys
>>> print("Error!! Not a valid input", file=sys.stderr)
Error!! Not a valid input
```

* `str.format()`可以用来设定字符串的风格，处理多个字符串时比粘连方式更优雅

```python
>>> num1 = 42
>>> num2 = 7

>>> '{} + {} = {}'.format(num1, num2, num1 + num2)
'42 + 7 = 49'

# 或者将格式保存为一个变量然后在需要的地方使用
>>> op_fmt = '{} + {} = {}'
>>> op_fmt.format(num1, num2, num1 + num2)
'42 + 7 = 49'
>>> op_fmt.format(num1, 29, num1 + 29)
'42 + 29 = 71'

# 在print函数内部使用也当然没问题
>>> print('{} + {} = {}'.format(num1, num2, num1 + num2))
42 + 7 = 49
```

* 使用有序的参数

```python
>>> num1
42
>>> num2
7
>>> print("{0} + {1} * {0} = {2}".format(num1, num2, num1 + num2 * num1))
42 + 7 * 42 = 336
```

* 数值格式——使用可选的参数值接`:`和格式风格进行指定

```python
>>> appx_pi = 22 / 7
>>> appx_pi
3.142857142857143

# 在小数点后限制数字的位数
# 数值会进行取舍
>>> print("{0:.2f}".format(appx_pi))
3.14
>>> print("{0:.3f}".format(appx_pi))
3.143

# 对齐
>>> print("{0:<10.3f} and 5.12".format(appx_pi))
3.143      and 5.12
>>> print("{0:>10.3f} and 5.12".format(appx_pi))
     3.143 and 5.12

# 用0填充
>>> print("{0:08.3f}".format(appx_pi))
0003.143
```

* 不同的基数

```python
>>> print("42 in binary = {:b}".format(42))
42 in binary = 101010
>>> print("42 in octal = {:o}".format(42))
42 in octal = 52
>>> print("241 in hex = {:x}".format(241))
241 in hex = f1

# 通过#添加0b/0o/0x前缀
>>> print("42 in binary = {:#b}".format(42))
42 in binary = 0b101010

>>> hex_str = "{:x}".format(42)
>>> hex_str
'2a'

# 也可以使用内置的format函数
>>> format(42, 'x')
'2a'
>>> format(42, '#x')
'0x2a'

# 将字符串转换为整型
>>> int(hex_str, base=16)
42
>>> int('0x2a', base=16)
42
```

* 跟`r`前缀相似，使用`f`前缀可以用来表示格式化的字符串
    * Python v3.6引进
* 跟`str.format()`相似，在`{}`中指定变量/表达式

```python
>>> num1 = 42
>>> num2 = 7
>>> f'{num1} + {num2} = {num1 + num2}'
'42 + 7 = 49'
>>> print(f'{num1} + {num2} = {num1 + num2}')
42 + 7 = 49

>>> appx_pi = 22 / 7
>>> f'{appx_pi:08.3f}'
'0003.143'

>>> f'{20:x}'
'14'
>>> f'{20:#x}'
'0x14'
```

**进一步阅读**

* [Python文档 - 格式化字符串](https://docs.python.org/3/library/string.html#formatstrings) - 更多信息和例子
* [Python docs - f-strings](https://docs.python.org/3/reference/lexical_analysis.html#f-strings) - 更多例子和告诫

<br>

### <a name="range-function"></a>range 函数

* 默认参数`start=0`和`step=1`,因此它们可以跳过或者根据需要合适地设定
    * `range(stop)`
    * `range(start, stop)`
    * `range(start, stop, step)`
* 注意`range`的输出不包含`stop`值（左闭右开）
* 查阅[列表](./Lists.md)章节获取列表的讨论和例子
* [Python文档 - Ranges](https://docs.python.org/3/library/stdtypes.html#typesseq-range) - 更多信息和例子

```python
>>> range(5)
range(0, 5)

>>> list(range(5))
[0, 1, 2, 3, 4]

>>> list(range(-2, 2))
[-2, -1, 0, 1]

>>> list(range(1, 15, 2))
[1, 3, 5, 7, 9, 11, 13]

>>> list(range(10, -5, -2))
[10, 8, 6, 4, 2, 0, -2, -4]
```

<br>

### <a name="type-function"></a>type函数

用于检查变量和数值的数据类型

```python
>>> type(5)
<class 'int'>

>>> type('Hi there!')
<class 'str'>

>>> type(range(7))
<class 'range'>

>>> type(None)
<class 'NoneType'>

>>> type(True)
<class 'bool'>

>>> arr = list(range(4))
>>> arr
[0, 1, 2, 3]
>>> type(arr)
<class 'list'>
```

<br>

### <a name="variable-scope"></a>变量作用域

```python
#!/usr/bin/python3

def print_num():
    print("Yeehaw! num is visible in this scope, its value is: " + str(num))

num = 25
print_num()
```

* 在函数调用之前定义的变量在函数作用域内可视
* [Python docs - 默认参数值](https://docs.python.org/3/tutorial/controlflow.html#default-argument-values) - 查看默认数值使用的描述

```
$ ./variable_scope_1.py
Yeehaw! num is visible in this scope, its value is: 25
```

在函数代码块内声明的一个变量在代码块之外使用会发生什么？

```python
#!/usr/bin/python3

def square_of_num(num):
    sqr_num = num * num

square_of_num(5)
print("5 * 5 = {}".format(sqr_num))
```

* 这里，`sqr_num`声明在`square_of_num`函数内，在代码块外不能够使用

```
$ ./variable_scope_2.py
Traceback (most recent call last):
  File "./variable_scope_2.py", line 7, in <module>
    print("5 * 5 = {}".format(sqr_num))
NameError: name 'sqr_num' is not defined
```

一种解决办法是使用`global`关键字

```python
#!/usr/bin/python3

def square_of_num(num):
    global sqr_num
    sqr_num = num * num

square_of_num(5)
print("5 * 5 = {}".format(sqr_num))
```

* 现在，即使在函数外我们也能够使用`sqr_num`

```
$ ./variable_scope_3.py
5 * 5 = 25
```

如果一个变量名在函数内外都进行了定义，函数内的变量不会影响函数外变量的使用

```python
#!/usr/bin/python3

sqr_num = 4

def square_of_num(num):
    sqr_num = num * num
    print("5 * 5 = {}".format(sqr_num))

square_of_num(5)
print("Whoops! sqr_num is still {}!".format(sqr_num))
```

* 注意使用`global sqr_num`会影响函数外的`sqr_num`

```
$ ./variable_scope_4.py
5 * 5 = 25
Whoops! sqr_num is still 4!
```

**进一步阅读**

* [Python文档 - 作用域示例](https://docs.python.org/3/tutorial/classes.html#scopes-and-namespaces-example)
* [Python文档 - global语句](https://docs.python.org/3/reference/simple_stmts.html#the-global-statement)
