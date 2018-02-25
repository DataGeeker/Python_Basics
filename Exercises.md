# <a name="exercises"></a>练习

1) [变量和打印](#variables-and-print)
2) [函数](#functions)
3) [控制结构](#control-structures)
4) [列表](#list)
5) [文件](#file)
6) [文本处理](#text-processing)
7) [混杂](#misc)

<br>

对于某些问题，提供assert语法的Python程序会自动测试你在[练习文件](https://github.com/ShixiangWang/Python_Basics/tree/master/exercise_files)目录中的解决方案，比如练习[Q2a - 整数长度](https://github.com/learnbyexample/Python_Basics/blob/master/exercise_files/q2a_int_length.py)。该目录也包含了样例输入文本文件。

<br>

## <a name="variables-and-print"></a>1) 变量和打印

**Q1a)** 询问用户信息，例如：`name`、`department`、 `college`等等并用`print`函数显示

```
# Sample of how program might ask user input and display output afterwards
$ ./usr_ip.py
Please provide the following details
Enter your name: learnbyexample
Enter your department: ECE
Enter your college: PSG Tech

------------------------------------
Name       : learnbyexample
Department : ECE
College    : PSG Tech
```

<br>

## <a name="functions"></a>2) 函数

**Q2a)** 返回整数长度

```python
>>> len_int(962306349871524124750813401378124)
33
>>> len_int(+42)
2
>>> len_int(-42)
3

# bonus: handle -ve numbers and check for input type
>>> len_int(-42)
2
>>> len_int('a')
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
  File "<stdin>", line 3, in len_int
TypeError: provide only integer input
```

**Q2b)** 返回True/False - 两个大小写无关的相同字符串

```python
>>> str_cmp('nice', 'nice')
True
>>> str_cmp('Hi there', 'hi there')
True
>>> str_cmp('GoOd DaY', 'gOOd dAy')
True
>>> str_cmp('foo', 'food')
False
```

**Q2c)** 返回True/False - 两个字符串是相同字母但异位（假设只包含英文字母）

```python
>>> str_anagram('god', 'Dog')
True
>>> str_anagram('beat', 'table')
False
>>> str_anagram('beat', 'abet')
True
```

<br>

## <a name="control-structures"></a>3) 控制结构

**Q3a)** 写一个函数，返回

* 'Good'，输入能被7整除
* 'Food'，输入能被6整除
* 'Universe'，输入能被42整除
* 'Oops'所有其他数字
* 仅1个输出，能被42整除的优先

```python
>>> six_by_seven(66)
'Food'
>>> six_by_seven(13)
'Oops'
>>> six_by_seven(42)
'Universe'
>>> six_by_seven(14)
'Good'
>>> six_by_seven(84)
'Universe'
>>> six_by_seven(235432)
'Oops'
```

*额外奖励*：使用一个循环输出1到100作为输入对应的输出数字和字符串

```python
1 Oops
2 Oops
3 Oops
4 Oops
5 Oops
6 Food
7 Good
...
41 Oops
42 Universe
...
98 Good
99 Oops
100 Oops
```

**Q3b)** 打印所有1-1000能够二进制和十进制正反读都相同的数字

```
$ ./dec_bin.py
1 1
3 11
5 101
7 111
9 1001
33 100001
99 1100011
313 100111001
585 1001001001
717 1011001101
```

*额外奖励*：添加八进制和十六进制

```
$ ./dec_bin_oct.py
1 0b1 0o1
3 0b11 0o3
5 0b101 0o5
7 0b111 0o7
9 0b1001 0o11
585 0b1001001001 0o1111

$ ./dec_bin_oct_hex.py
1 0b1 0o1 0x1
3 0b11 0o3 0x3
5 0b101 0o5 0x5
7 0b111 0o7 0x7
9 0b1001 0o11 0x9
```

<br>

## <a name="list"></a>4) 列表

**Q4a)** 写一个函数返回一个列表所有数字的乘积

```python
>>> product([1, 4, 21])
84
>>> product([-4, 2.3e12, 77.23, 982, 0b101])
-3.48863356e+18
```

*额外奖励*：处理任意种类迭代

```python
>>> product((-3, 11, 2))
-66
>>> product({8, 300})
2400
>>> product([234, 121, 23, 945, 0])
0
>>> product(range(2, 6))
120
# 你能识别最后一个使用的数学函数吗？
```

**Q4b)** 写一个函数返回一个列表（或一般的迭代）第n小的数字W。如果第二个参数未指定就返回最小的。

*注意* 如果一个列表包含重复，它们应该在决定第n个最小值前被处理

```python
>>> nums = [42, 23421341, 234.2e3, 21, 232, 12312, -2343]
>>> nth_lowest(nums, 3)
42
>>> nth_lowest(nums, 5)
12312
>>> nth_lowest(nums, 15)
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
  File "<stdin>", line 2, in nth_lowest
IndexError: list index out of range

>>> nums = [1, -2, 4, 2, 1, 3, 3, 5]
>>> nth_lowest(nums)
-2
>>> nth_lowest(nums, 4)
3
>>> nth_lowest(nums, 5)
4

>>> nth_lowest('unrecognizable', 3)
'c'
>>> nth_lowest('abracadabra', 4)
'd'
```

<br>

## <a name="file"></a>5) 文件

**Q5a)** 打印一个（单列全是数字）文件所有数字的总和

```
$ cat f1.txt
8
53
3.14
84
73e2
100
2937

$ ./col_sum.py
10485.14
```

**Q5b)** 打印一个包含任意字符串文件其中数字（假设只有正整数）的总和

```
$ cat f2.txt
Hello123 World 35
341 2
Good 13day
How are 1784 you

$ ./extract_sum.py
2298
```

<br>

## <a name="text-processing"></a>6) 文本处理

**Q6a)** 检查是否两个单词是否相同或者仅一个字符不同（大小写无关），输入字符串应该有相同的长度

另见[Levenshtein distance](https://en.wikipedia.org/wiki/Levenshtein_distance)

```python
>>> is_one_char_diff('bar', 'bar')
True
>>> is_one_char_diff('bar', 'Baz')
True
>>> is_one_char_diff('Food', 'fold')
True
>>> is_one_char_diff('A', 'b')
True

>>> is_one_char_diff('a', '')
False
>>> is_one_char_diff('Bar', 'Bark')
False
>>> is_one_char_diff('Bar', 'art')
False
>>> is_one_char_diff('Food', 'fled')
False
>>> is_one_char_diff('ab', '')
False
```

**Q6b)** 检查一个单词是否以字母表顺序或逆序排列（无关大小写）

你能思考一种仅用内置函数和字符串方法解决的办法吗？

```python
>>> is_alpha_order('bot')
True
>>> is_alpha_order('arT')
True
>>> is_alpha_order('are')
False
>>> is_alpha_order('boat')
False

>>> is_alpha_order('toe')
True
>>> is_alpha_order('Flee')
False
>>> is_alpha_order('reed')
True
```

*额外奖励*：检查是否一个句子所有的单词（假设只有空格分隔输入）都按照字母表顺序或逆序排列（无关大小写）

**提示** 使用内置函数和生成器表达式

```bash
>>> is_alpha_order_sentence('Toe got bit')
True
>>> is_alpha_order_sentence('All is well')
False
```

**Q6c)** 寻找大括号最大的嵌套深度

不平衡或者错序的括号应该返回`-1`

对输入字符串进行迭代是一种解决方式，另一种是使用正则表达式

```python
>>> max_nested_braces('a*b')
0
>>> max_nested_braces('{a+2}*{b+c}')
1
>>> max_nested_braces('{{a+2}*{{b+{c*d}}+e*d}}')
4
>>> max_nested_braces('a*b+{}')
1
>>> max_nested_braces('}a+b{')
-1
>>> max_nested_braces('a*b{')
-1
```

*额外奖励*: 空括号，例如`{}`应该返回`-1`

```python
>>> max_nested_braces('a*b+{}')
-1
>>> max_nested_braces('a*{b+{}+c*{e*3.14}}')
-1
```

<br>

## <a name="misc"></a>7) 混杂

**Q7a)** 播放一首歌 (**提示** 使用`subprocess`模块)

**Q7b)** 用任意链接打开浏览器，例如： https://github.com/ShixiangWang/Python_Basics (**提示** 使用`webbrowser`模块)

<br>

<br>

关于参考答案，查看[练习答案](https://github.com/learnbyexample/Python_Basics/tree/master/exercise_solutions)目录
