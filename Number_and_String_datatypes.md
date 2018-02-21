# <a name="number-and-string-data-types"></a>数值和字符串数据类型

* [数值](#numbers)
* [字符串](#string)
* [常量](#constants)
* [内置操作符](#built-in-operators)

Python会自动确定变量数据类型。变量在其他地方使用之前仅需要赋值。

<br>

### <a name="numbers"></a>数值

* 整数例子

```python
>>> num1 = 7
>>> num2 = 42
>>> total = num1 + num2
>>> print(total)
49
>>> total
49

# 对整数精度没有限制，仅受限于可分配的内存
>>> 34 ** 32
10170102859315411774579628461341138023025901305856

# 使用单个 / （除法）会返回浮点结果
>>> 9 / 5
1.8

# 使用两个 / （整除）仅返回整数部分（没有取舍）
>>> 9 // 5
1

>>> 9 % 5
4
```

* 浮点数例子

```python
>>> appx_pi = 22 / 7
>>> appx_pi
3.142857142857143

>>> area = 42.16
>>> appx_pi + area
45.30285714285714

>>> num1
7
>>> num1 + area
49.16
```

* [E 科学计数法](https://en.wikipedia.org/wiki/Scientific_notation#E_notation)非常有用

```python
>>> sci_num1 = 3.982e5
>>> sci_num2 = 9.32e-1
>>> sci_num1 + sci_num2
398200.932

>>> 2.13e21 + 5.23e22
5.443e+22
```

* 二进制数值用`0b`或`0B`前缀表示
* 八进制数值用`0o`或`0O`前缀表示
* 相似地，十六进制用`0x`或`0X`前缀表示

```python
>>> bin_num = 0b101
>>> oct_num = 0o12
>>> hex_num = 0xF

>>> bin_num
5
>>> oct_num
10
>>> hex_num
15

>>> oct_num + hex_num
25
```

* `_`分隔让数字更具有可读性
    * Python v3.6 引入

```python
>>> 1_000_000
1000000
>>> 1_00.3_352
100.3352
>>> 0xff_ab1
1047217

# f-strings格式在后面章节解释
>>> num = 34 ** 32
>>> print(f'{num:_}')
10_170_102_859_315_411_774_579_628_461_341_138_023_025_901_305_856
```

**进一步阅读**

* [Python文档 - 数值](https://docs.python.org/3/tutorial/introduction.html#numbers)
* [十进制](https://docs.python.org/3/library/decimal.html)
* [分数](https://docs.python.org/3/library/fractions.html)
* [复数](https://docs.python.org/3/library/functions.html#complex)
* [Python文档 - 关键字](https://docs.python.org/3/reference/lexical_analysis.html#keywords) - 不要把它们作为变量使用

<br>

### <a name="string"></a>字符串

* 使用单/双引号可以声明一个字符串
* 使用`\`进行反义操作：跳过属于字符串本身的引号

```python
>>> str1 = 'This is a string'
>>> str1
'This is a string'
>>> greeting = "Hello World!"
>>> greeting
'Hello World!'

>>> weather = "It's a nice and warm day"
>>> weather
"It's a nice and warm day"
>>> print(weather)
It's a nice and warm day

>>> weather = 'It\'s a nice and warm day'
>>> print(weather)
It's a nice and warm day
```

* 特殊含义符号，像换行符`\n`可以在字符串中使用

```python
>>> colors = 'Blue\nRed\nGreen'
>>> colors
'Blue\nRed\nGreen'

>>> print(colors)
Blue
Red
Green
```

* 使用前缀`r`(代表**raw**)如果你想要字符串被原样输出
* 通常用于正则表达式，参见[模式匹配和抽取](./Text_Processing.md#pattern-matching-and-extraction)示例

```bash
>>> raw_str = r'Blue\nRed\nGreen'
>>> print(raw_str)
Blue\nRed\nGreen

# 查看字符串内部是如何存储的
>>> raw_str
'Blue\\nRed\\nGreen'
```

* 字符串粘连和重复

```python
>>> str1 = 'Hello'
>>> str2 = ' World'
>>> print(str1 + str2)
Hello World

>>> style_char = '-'
>>> style_char * 10
'----------'

>>> word = 'buffalo '
>>> print(word * 8)
buffalo buffalo buffalo buffalo buffalo buffalo buffalo buffalo

# Python v3.6 允许变量使用f-strings进行插入
>>> msg = f'{str1} there'
>>> msg
'Hello there'
```

* 三引号
* `"""`或`'''`可以用于多行注释、字符串以及使用`\`反义

```python
#!/usr/bin/python3

"""
这一行是多行注释的一部分

This program shows examples of triple quoted strings
"""

# 把多行字符串赋值给变量
poem = """\
The woods are lovely, dark and deep,
But I have promises to keep,
And miles to go before I sleep,
And miles to go before I sleep.
"""

print(poem, end='')
```

* 三引号括起的字符串在说明文档中也有用，参见[Docstrings](./Docstrings.md)章节示例

```
$ ./triple_quoted_string.py
The woods are lovely, dark and deep,
But I have promises to keep,
And miles to go before I sleep,
And miles to go before I sleep.
$
```

**进一步阅读**

* [Python文档 - 字符串](https://docs.python.org/3/tutorial/introduction.html#strings)
* [Python文档 - f-strings](https://docs.python.org/3/reference/lexical_analysis.html#f-strings) - 获取更多例子和告诫
* [Python文档 - 转义序列列表和字符串更多信息](https://docs.python.org/3/reference/lexical_analysis.html#strings)
* [Python文档 - Binary序列类型](https://docs.python.org/3/library/stdtypes.html#binary-sequence-types-bytes-bytearray-memoryview)
* [三引号字符串格式化](https://stackoverflow.com/questions/3877623/in-python-can-you-have-variables-within-triple-quotes-if-so-how)

<br>

### <a name="constants"></a>常量

来自[Python文档 - 常量](https://docs.python.org/3/library/constants.html)的释义

* `None`是`NoneType`类型的唯一值
    * `None` 常用于数值的确实
* `False`是`bool`类型的“错误”值
* `True` 是`bool`类型的“正确”值
* [Python文档 - 真值检验](https://docs.python.org/3/library/stdtypes.html#truth)

```python
>>> bool(2)
True
>>> bool(0)
False
>>> bool('')
False
>>> bool('a')
True
```

<br>

### <a name="built-in-operators"></a>内置操作符

* 算术操作符
    * `+` 加
    * `-` 减
    * `*` 乘
    * `/` 除（浮点输出）
    * `//` 整除（整数输出，结果没有取舍）
    * `**` 幂
    * `%` 取模
* 字符串操作符
    * `+` 字符串粘连
    * `*` 字符串重复
* 比较操作符
    * `==` 等于
    * `>` 大于
    * `<` 小于
    * `!=` 不等于
    * `>=` 大于或等于
    * `<=` 小于或等于
* 布尔逻辑操作
    * `and` 逻辑与
    * `or` 逻辑或
    * `not` 逻辑非
* 位操作符
    * `&` 与
    * `|` 或
    * `^` 异或
    * `~` 位反转
    * `>>` 右移
    * `<<` 左移
* 还有更多...

**进一步阅读**

* [Python文档 - 数值类型](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex) - complete list of operations and precedence
* [Python文档 - 字符串方法](https://docs.python.org/3/library/stdtypes.html#string-methods)
