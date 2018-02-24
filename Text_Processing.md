# <a name="text-processing"></a>文本处理

* [字符串方法](#string-methods)
* [正则表达式](#regular-expressions)
* [模式匹配和提取](#pattern-matching-and-extraction)
* [搜索和替换](#search-and-replace)
* [编译正则表达式](#compiling-regular-expressions)
* [正则表达式进一步阅读](#further-reading-on-regular-expressions)

<br>

### <a name="string-methods"></a>字符串方法

* 转换字符
    * `str.maketrans()`获取转换表
    * `translate()`基于转换表执行字符串映射
* `maketrans()`第一个参数是被取代的字符，第二个参数是取代的字符，第三个是被映射为`None`的字符
* [字符转换例子](https://stackoverflow.com/questions/555705/character-translation-using-python-like-the-tr-command)

```python
>>> greeting = '===== Have a great day ====='
>>> greeting.translate(str.maketrans('=', '-'))
'----- Have a great day -----'

>>> greeting = '===== Have a great day!! ====='
>>> greeting.translate(str.maketrans('=', '-', '!'))
'----- Have a great day -----'

>>> import string
>>> quote = 'SIMPLICITY IS THE ULTIMATE SOPHISTICATION'
>>> tr_table = str.maketrans(string.ascii_uppercase, string.ascii_lowercase)
>>> quote.translate(tr_table)
'simplicity is the ultimate sophistication'

>>> sentence = "Thi1s is34 a senten6ce"
>>> sentence.translate(str.maketrans('', '', string.digits))
'This is a sentence'
>>> greeting.translate(str.maketrans('', '', string.punctuation))
' Have a great day '
```

* 移除首/尾/两者的字符串
* 仅移除首/尾连续的字符
* 默认空格会被除去
* 如果指定了多个字符，它会被视为集合，并使用其中所有的组合


```python
>>> greeting = '      Have a nice day :)     '
>>> greeting.strip()
'Have a nice day :)'
>>> greeting.rstrip()
'      Have a nice day :)'
>>> greeting.lstrip()
'Have a nice day :)     '

>>> greeting.strip(') :')
'Have a nice day'

>>> greeting = '===== Have a great day!! ====='
>>> greeting.strip('=')
' Have a great day!! '
```

* 风格化
* width参数指定了总的输出字符串长度

```python
>>> ' Hello World '.center(40, '*')
'************* Hello World **************'
```

* 改变大小写和大小写检查

```python
>>> sentence = 'thIs iS a saMple StrIng'

>>> sentence.capitalize()
'This is a sample string'

>>> sentence.title()
'This Is A Sample String'

>>> sentence.lower()
'this is a sample string'

>>> sentence.upper()
'THIS IS A SAMPLE STRING'

>>> sentence.swapcase()
'THiS Is A SAmPLE sTRiNG'

>>> 'good'.islower()
True

>>> 'good'.isupper()
False
```

* 检查是否字符串由数值构成

```python
>>> '1'.isnumeric()
True
>>> 'abc1'.isnumeric()
False
>>> '1.2'.isnumeric()
False
```

* 检查是否字符串序列是否存在

```python
>>> sentence = 'This is a sample string'
>>> 'is' in sentence
True
>>> 'this' in sentence
False
>>> 'This' in sentence
True
>>> 'this' in sentence.lower()
True
>>> 'is a' in sentence
True
>>> 'test' not in sentence
True
```

* 获取字符序列存在的次数（非覆盖）

```python
>>> sentence = 'This is a sample string'
>>> sentence.count('is')
2
>>> sentence.count('w')
0

>>> word = 'phototonic'
>>> word.count('oto')
1
```

* 匹配头尾字符序列

```python
>>> sentence
'This is a sample string'

>>> sentence.startswith('This')
True
>>> sentence.startswith('The')
False

>>> sentence.endswith('ing')
True
>>> sentence.endswith('ly')
False
```

* 基于字符序列分割字符串
* 返回列表
* 要使用正则表达式分割，使用`re.split()`

```python
>>> sentence = 'This is a sample string'

>>> sentence.split()
['This', 'is', 'a', 'sample', 'string']

>>> "oranges:5".split(':')
['oranges', '5']
>>> "oranges :: 5".split(' :: ')
['oranges', '5']

>>> "a e i o u".split(' ', maxsplit=1)
['a', 'e i o u']
>>> "a e i o u".split(' ', maxsplit=2)
['a', 'e', 'i o u']

>>> line = '{1.0 2.0 3.0}'
>>> nums = [float(s) for s in line.strip('{}').split()]
>>> nums
[1.0, 2.0, 3.0]
```

* 连接字符串列表

```python
>>> str_list
['This', 'is', 'a', 'sample', 'string']
>>> ' '.join(str_list)
'This is a sample string'
>>> '-'.join(str_list)
'This-is-a-sample-string'

>>> c = ' :: '
>>> c.join(str_list)
'This :: is :: a :: sample :: string'
```

* 替换字符
* 第三个参数指定使用多少次的替换
* 变量必须显式地重赋值

```python
>>> phrase = '2 be or not 2 be'
>>> phrase.replace('2', 'to')
'to be or not to be'

>>> phrase
'2 be or not 2 be'

>>> phrase.replace('2', 'to', 1)
'to be or not 2 be'

>>> phrase = phrase.replace('2', 'to')
>>> phrase
'to be or not to be'
```

**进一步阅读**

* [Python文档 - 字符串方法](https://docs.python.org/3/library/stdtypes.html#string-methods)
* [python字符串方法教程](http://www.thehelloworldprogram.com/python/python-string-methods/)

<br>

### <a name="regular-expressions"></a>正则表达式

* 正则表达式元素便利参考

| 元字符 | 描述 |
| ------------- | ----------- |
| ^ | 锚定，匹配字符串行首 |
| $ | 锚定，匹配字符串行尾 |
| . | 匹配除换行符\n之外的字符 |
| &#124; | 或操作符，用于匹配多个模式 |
| () | 用于模式分组和提取 |
| [] | 字符类 - 匹配多个字符中的一个 |
| &#92;^ | 使用\ 匹配元字符 |

<br>

| 量词 | 描述 |
| ------------- | ----------- |
| * | 匹配之前的字符0或多次|
| + | 匹配之前的字符1或多次 |
| ? | 匹配之前的字符0或1次 |
| {n} | 匹配n次 |
| {n,} | 匹配至少n次|
| {n,m} | 匹配至少n次，至多m次|

<br>

| 字符类 | 描述 |
| ------------- | ----------- |
| [aeiou] | 匹配任何元音 |
| \[^aeiou] | ^ 倒置选择，所以这会匹配任何的辅音|
| [a-f] | 匹配abcdef中任意字符|
| \d | 匹配数字，跟[0-9]一样|
| \D | 匹配非数字，跟 \[^0-9] 或 \[^\d]一样 |
| \w | 匹配字母和下划线，跟[a-zA-Z_]一样|
| \W | 匹配非字母和非下划线字符，跟\[^a-zA-Z_] 或 \[^\w]一样 |
| \s | 匹配空格符，跟[\ \t\n\r\f\v]一样 |
| \S | 匹配非空行符，跟\[^\s]一样 |
| \b | 单词边界，单词定义为字母序列 |
| \B | 非单词边界 |

<br>

| 编译标记 | 描述 |
| ------------- | ----------- |
| re.I | 忽略大小写 |
| re.M | 多行模式，^和$锚定符号可以处理中间行|
| re.S | 单行模式，.也会匹配\n |
| re.V | 冗余模式，提高可读性和添加注释 |

* [Python文档 - 标记](https://docs.python.org/3/howto/regex.html#compilation-flags) - 详情和标记长名

<br>

| 变量 | 描述 |
| ------------- | ----------- |
| \1, \2, \3 等等 | 引用匹配的模式 |
| \g<1>, \g<2>, \g<3> etc | 引用匹配的模式，用于区分数字和引用|

<br>

### <a name="pattern-matching-and-extraction"></a>模式匹配和提取


* 匹配/提取字符序列
* 使用`re.search()`查看是否一个字符串包含某个模式
* 使用`re.findall()`获得一个匹配模式列表
* 使用`re.split()`获得一个基于模式分割字符串的列表
* 它们的语法如下

```python
re.search(pattern, string, flags=0)
re.findall(pattern, string, flags=0)
re.split(pattern, string, maxsplit=0, flags=0)
```

```python
>>> import re
>>> string = "This is a sample string"

>>> bool(re.search('is', string))
True

>>> bool(re.search('this', string))
False

>>> bool(re.search('this', string, re.I))
True

>>> bool(re.search('T', string))
True

>>> bool(re.search('is a', string))
True

>>> re.findall('i', string)
['i', 'i', 'i']
```

* 使用正则表达式
* 当使用正则表达式元素时用`r''`格式

```python
>>> string
'This is a sample string'

>>> re.findall('is', string)
['is', 'is']

>>> re.findall('\bis', string)
[]

>>> re.findall(r'\bis', string)
['is']

>>> re.findall(r'\w+', string)
['This', 'is', 'a', 'sample', 'string']

>>> re.split(r'\s+', string)
['This', 'is', 'a', 'sample', 'string']

>>> re.split(r'\d+', 'Sample123string54with908numbers')
['Sample', 'string', 'with', 'numbers']

>>> re.split(r'(\d+)', 'Sample123string54with908numbers')
['Sample', '123', 'string', '54', 'with', '908', 'numbers']
```

* 引用

```python
>>> quote = "So many books, so little time"

>>> re.search(r'([a-z]{2,}).*\1', quote, re.I)
<_sre.SRE_Match object; span=(0, 17), match='So many books, so'>

>>> re.search(r'([a-z])\1', quote, re.I)
<_sre.SRE_Match object; span=(9, 11), match='oo'>

>>> re.findall(r'([a-z])\1', quote, re.I)
['o', 't']
```

<br>

### <a name="search-and-replace"></a>搜索和替换

**语法**

```python
re.sub(pattern, repl, string, count=0, flags=0)
```

* 简单替换
* `re.sub`不会改变传入变量的值，必须显式地指定

```python
>>> sentence = 'This is a sample string'
>>> re.sub('sample', 'test', sentence)
'This is a test string'

>>> sentence
'This is a sample string'
>>> sentence = re.sub('sample', 'test', sentence)
>>> sentence
'This is a test string'

>>> re.sub('/', '-', '25/06/2016')
'25-06-2016'
>>> re.sub('/', '-', '25/06/2016', count=1)
'25-06/2016'

>>> greeting = '***** Have a great day *****'
>>> re.sub('\*', '=', greeting)
'===== Have a great day ====='
```

* 引用

```python
>>> words = 'night and day'
>>> re.sub(r'(\w+)( \w+ )(\w+)', r'\3\2\1', words)
'day and night'

>>> line = 'Can you spot the the mistakes? I i seem to not'
>>> re.sub(r'\b(\w+) \1\b', r'\1', line, flags=re.I)
'Can you spot the mistakes? I seem to not'
```

* 在`re.sub()`替换部分使用函数

```python
>>> import math
>>> numbers = '1 2 3 4 5'

>>> def fact_num(n):
...     return str(math.factorial(int(n.group(1))))
...
>>> re.sub(r'(\d+)', fact_num, numbers)
'1 2 6 24 120'

>>> re.sub(r'(\d+)', lambda m: str(math.factorial(int(m.group(1)))), numbers)
'1 2 6 24 120'
```

* [从re.sub调用函数](https://stackoverflow.com/questions/11944978/call-functions-from-re-sub)
* [用函数输出替换字符串模式](https://stackoverflow.com/questions/12597370/python-replace-string-pattern-with-output-of-function)
* [lambda教程](https://pythonconquerstheuniverse.wordpress.com/2011/08/29/lambda_tutorial/)

<br>

### <a name="compiling-regular-expressions"></a>编译正则表达式

```python
>>> swap_words = re.compile(r'(\w+)( \w+ )(\w+)')
>>> swap_words
re.compile('(\\w+)( \\w+ )(\\w+)')

>>> words = 'night and day'

>>> swap_words.search(words).group()
'night and day'
>>> swap_words.search(words).group(1)
'night'
>>> swap_words.search(words).group(2)
' and '
>>> swap_words.search(words).group(3)
'day'
>>> swap_words.search(words).group(4)
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
IndexError: no such group

>>> bool(swap_words.search(words))
True
>>> swap_words.findall(words)
[('night', ' and ', 'day')]

>>> swap_words.sub(r'\3\2\1', words)
'day and night'
>>> swap_words.sub(r'\3\2\1', 'yin and yang')
'yang and yin'
```

<br>

### <a name="further-reading-on-regular-expressions"></a>正则表达式进一步阅读

* [Python文档 - re模块](https://docs.python.org/3/library/re.html)
* [Python文档 - 正则表达式使用介绍](https://docs.python.org/3/howto/regex.html)
* [developers.google - 正则表达式教程](https://developers.google.com/edu/python/regular-expressions)
* [automatetheboringstuff - 正则表达式](https://automatetheboringstuff.com/chapter7/)
* [综合参考：regex是什么？](https://stackoverflow.com/questions/22937618/reference-what-does-this-regex-mean)
* 练习工具
    * [online regex tester](https://regex101.com/#python) 展示解释，提供参考指南和保存、分享regex
    * [regexone](http://regexone.com/) - 交互式教程
	* [cheatsheet](https://www.shortcutfoo.com/app/dojos/python-regex/cheatsheet) -  [交互式](https://www.shortcutfoo.com/app/dojos/python-regex)学习
    * [regexcrossword](https://regexcrossword.com/) - 通过解答纵横游戏练习，开始之前阅读'How to play'部分
