# <a name="getting-user-input"></a>获取用户输入

* [整数输入](#integer-input)
* [浮点数输入](#floating-point-input)
* [字符串输入](#string-input)

<br>

### <a name="integer-input"></a>整数输入

```python
#!/usr/bin/python3

usr_ip = input("Enter an integer number: ")

# 需要将输入的字符串显式地指定为需要的类型
usr_num = int(usr_ip)
sqr_num = usr_num * usr_num

print("Square of entered number is: {}".format(sqr_num))
```

* 让我们给定一个整数和字符串来测定这个程序
* [Python docs - 整数文本](https://docs.python.org/3/reference/lexical_analysis.html#integer-literals)

```
$ ./user_input_int.py
Enter an integer number: 23
Square of entered number is: 529

$ ./user_input_int.py
Enter an integer number: abc
Traceback (most recent call last):
  File "./user_input_int.py", line 6, in <module>
    usr_num = int(usr_ip)
ValueError: invalid literal for int() with base 10: 'abc'
```

<br>

### <a name="floating-point-input"></a>浮点数输入

```python
#!/usr/bin/python3

usr_ip = input("Enter a floating point number: ")

# 需要将输入的字符串显式地指定为我们需要的类型
usr_num = float(usr_ip)
sqr_num = usr_num * usr_num

# 限制小数点位数
print("Square of entered number is: {0:.2f}".format(sqr_num))
```

* [E 科学计数法](https://en.wikipedia.org/wiki/Scientific_notation#E_notation)在需要时可以使用
* [Python文档 - 浮点数文本](https://docs.python.org/3/reference/lexical_analysis.html#floating-point-literals)
* [Python文档 - 浮点数](https://docs.python.org/3/tutorial/floatingpoint.html)

```
$ ./user_input_float.py
Enter a floating point number: 3.232
Square of entered number is: 10.45

$ ./user_input_float.py
Enter a floating point number: 42.7e5
Square of entered number is: 18232900000000.00

$ ./user_input_float.py
Enter a floating point number: abc
Traceback (most recent call last):
  File "./user_input_float.py", line 6, in <module>
    usr_num = float(usr_ip)
ValueError: could not convert string to float: 'abc'
```

<br>

### <a name="string-input"></a>字符串输入

```python
#!/usr/bin/python3

usr_name  = input("Hi there! What's your name? ")
usr_color = input("And your favorite color is? ")

print("{}, I like the {} color too".format(usr_name, usr_color))
```

* 不像Perl，字符串输入不需要进行类型转换和注意换行符

```
$ ./user_input_str.py
Hi there! What's your name? learnbyexample
And your favorite color is? blue
learnbyexample, I like the blue color too
```
