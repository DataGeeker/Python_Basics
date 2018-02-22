# <a name="control-structures"></a>控制结构

* [条件检查](#condition-checking)
* [if](#if)
* [for](#for)
* [while](#while)
* [continue和break](#continue-and-break)

<br>

### <a name="condition-checking"></a>条件检查

* 单个和组合测试

```python
>>> num = 5
>>> num > 2
True
>>> num > 3 and num <= 5
True
>>> 3 < num <= 5
True
>>> num % 3 == 0 or num % 5 == 0
True

>>> fav_fiction = 'Harry Potter'
>>> fav_detective = 'Sherlock Holmes'
>>> fav_fiction == fav_detective
False
>>> fav_fiction == "Harry Potter"
True
```

* 测试变量和值本身

```python
>>> bool(num)
True
>>> bool(fav_detective)
True
>>> bool(3)
True
>>> bool(0)
False
>>> bool("")
False
>>> bool(None)
False

>>> if -1:
...     print("-1 evaluates to True in condition checking")
...
-1 evaluates to True in condition checking
```

* 条件测试中`in`操作符的使用

对比这种检查方式

```python
>>> def num_chk(n):
...     if n == 10 or n == 21 or n == 33:
...         print("Number passes condition")
...     else:
...         print("Number fails condition")
...
>>> num_chk(10)
Number passes condition
>>> num_chk(12)
Number fails condition
```

和另一种

```python
>>> def num_chk(n):
...     if n in (10, 21, 33):
...         print("Number passes condition")
...     else:
...         print("Number fails condition")
...
>>> num_chk(12)
Number fails condition
>>> num_chk(10)
Number passes condition
```

* `(10, 21, 33)`是一个元组数据类型，会在后面的章节讲解
* [Python文档 - 真值检验](https://docs.python.org/3/library/stdtypes.html#truth)

<br>

### <a name="if"></a>if

```python
#!/usr/bin/python3

num = 45

# 单个if
if num > 25:
    print("Hurray! {} is greater than 25".format(num))

# if-else
if num % 2 == 0:
    print("{} is an even number".format(num))
else:
    print("{} is an odd number".format(num))

# if-elif-else
# 可以使用任意数目的elif
if num < 0:
    print("{} is a negative number".format(num))
elif num > 0:
    print("{} is a positive number".format(num))
else:
    print("{} is neither postive nor a negative number".format(num))
```

* 函数代码块、控制结构等等都是通过缩进区分
    * 推荐使用4个空格缩进
    * [Python文档 - 编码风格](https://docs.python.org/3/tutorial/controlflow.html#intermezzo-coding-style)
* 一个常见的语法错误是忘记了控制结构语句后的`:`
* 条件周围的`()`是可选的
* 缩进代码块可以有任意数目的语句，包括空行

```
$ ./if_elif_else.py
Hurray! 45 is greater than 25
45 is an odd number
45 is a positive number
```

**if-else**作为条件操作符

```python
#!/usr/bin/python3

num = 42

num_type = 'even' if num % 2 == 0 else 'odd'
print("{} is an {} number".format(num, num_type))
```

* 不像其他许多语言，Python没有`?:`条件操作符
* 单行`if-else`的使用是一种变通方法
* [模拟三元操作符的更多方法](http://stackoverflow.com/questions/394809/does-python-have-a-ternary-conditional-operator)

```
$ ./if_else_oneliner.py
42 is an even number
```

<br>

### <a name="for"></a>for

```python
#!/usr/bin/python3

number = 9
for i in range(1, 5):
    mul_table = number * i
    print("{} * {} = {}".format(number, i, mul_table))
```

* 传统基于循环的迭代可以通过使用`range`函数实现
    * 默认参数`start=0`、`step=1`，不含`stop`值
* 针对列表、元组等等变量的迭代会在后续章节讲述
* [Python文档 - 迭代工具](https://docs.python.org/3/library/itertools.html)

```
$ ./for_loop.py
9 * 1 = 9
9 * 2 = 18
9 * 3 = 27
9 * 4 = 36
```

<br>

### <a name="while"></a>while

```python
#!/usr/bin/python3

# 持续地询问直到用户输入一个正整数
usr_string = 'not a number'
while not usr_string.isnumeric():
    usr_string = input("Enter a positive integer: ")
```

* while循环允许我们直到某个条件被满足之前不断执行语句块
* [Python docs - 字符串方法](https://docs.python.org/3/library/stdtypes.html#string-methods)

```
$ ./while_loop.py
Enter a positive integer: abc
Enter a positive integer: 1.2
Enter a positive integer: 23
$
```

<br>

### <a name="continue-and-break"></a>continue和break

`continue`和`break`关键字用于在某些条件下改变正常的循环操作

**continue** - 跳过循环代码块余下的语句并进入下一次迭代

```python
#!/usr/bin/python3

prev_num = 0
curr_num = 0
print("The first ten numbers in fibonacci sequence: ", end='')

for num in range(10):
    print(curr_num, end=' ')

    if num == 0:
        curr_num = 1
        continue

    temp = curr_num
    curr_num = curr_num + prev_num
    prev_num = temp

print("")
```

* `continue`放置在循环代码块中的任意位置而不用担心复杂的代码流
* 这个例子仅仅展示`continue`的使用，查看[这里](https://docs.python.org/3/tutorial/controlflow.html#defining-functions)获取更加Python化的操作方式

```
$ ./loop_with_continue.py
The first ten numbers in fibonacci sequence: 0 1 1 2 3 5 8 13 21 34
```

**break** - 跳过循环代码块余下的语句（如果有）并进入退出循环代码块

```python
#!/usr/bin/python3

import random

while True:
    # 使用range函数注意500没有包含在内
    random_int = random.randrange(500)
    if random_int % 4 == 0 and random_int % 6 == 0:
        break
print("Random number divisible by 4 and 6: {}".format(random_int))
```

* `while True:`是常用作无限循环
* **randrange**和[range](./Functions.md#range-function)函数相似，有`start, stop, step`参数
* [Python文档 - random](https://docs.python.org/3/library/random.html)

```
$ ./loop_with_break.py
Random number divisible by 4 and 6: 168
$ ./loop_with_break.py
Random number divisible by 4 and 6: 216
$ ./loop_with_break.py
Random number divisible by 4 and 6: 24
```

这个while_loop.py例子可以用`break`语句重写

```python
>>> while True:
         usr_string = input("Enter a positive integer: ")
         if usr_string.isnumeric():
             break

Enter a positive integer: a
Enter a positive integer: 3.14
Enter a positive integer: 1
>>>
```

* 在嵌套循环中，`continue`和`break`仅影响中间**一层**对应的循环
* [Python文档 - 循环中的else从句](https://docs.python.org/3/tutorial/controlflow.html#break-and-continue-statements-and-else-clauses-on-loops)
