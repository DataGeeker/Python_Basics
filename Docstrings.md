# <a name="docstrings"></a>文档字符串

* [风格指南](#style-guide)
* [回文示例](#palindrome-example)

<br>

### <a name="style-guide"></a>风格指南

引用自[Python文档 - 编码风格](https://docs.python.org/3/tutorial/controlflow.html#intermezzo-coding-style)

* 使用4-空格缩进，不用制表符
* 包裹行让它们不超过79字符
* 使用空行分隔函数、类和函数内较大的代码块
* 尽量注释
* 使用文档字符串
* 逗号后、操作符周围使用空格
* 统一命名类和函数
    * 传统是对类使用骆驼拼写法，函数和方法使用小写和下划线

**风格指南**

* [PEP 0008](https://www.python.org/dev/peps/pep-0008/)
* [Google - pyguide](https://google.github.io/styleguide/pyguide.html)
* [python风格元素](https://github.com/amontalenti/elements-of-python-style)
* [The Hitchhiker’s Guide to Python](http://docs.python-guide.org/en/latest/) - 最好的练习手册

<br>

### <a name="palindrome-example"></a>回文示例

```python
#!/usr/bin/python3

"""
Asks for user input and tells if string is palindrome or not

Allowed characters: alphabets and punctuations .,;:'"-!?
Minimum alphabets: 3 and cannot be all same

Informs if input is invalid and asks user for input again
"""

import re

def is_palindrome(usr_ip):
    """
    Checks if string is a palindrome

    ValueError: if string is invalid

    Returns True if palindrome, False otherwise
    """

    # remove punctuations & whitespace and change to all lowercase
    ip_str = re.sub(r'[\s.;:,\'"!?-]', r'', usr_ip).lower()

    if re.search(r'[^a-zA-Z]', ip_str):
        raise ValueError("Characters other than alphabets and punctuations")
    elif len(ip_str) < 3:
        raise ValueError("Less than 3 alphabets")
    else:
        return ip_str == ip_str[::-1] and not re.search(r'^(.)\1+$', ip_str)

def main():
    while True:
        try:
            usr_ip = input("Enter a palindrome: ")
            if is_palindrome(usr_ip):
                print("{} is a palindrome".format(usr_ip))
            else:
                print("{} is NOT a palindrome".format(usr_ip))
            break
        except ValueError as e:
            print('Error: ' + str(e))

if __name__ == "__main__":
    main()
```
* 首个三引号括起的字符串标记了整个程序的文档字符串
* 第二个是`is_palindrome()`函数特定的文档字符串

```
$ ./palindrome.py
Enter a palindrome: as2
Error: Characters other than alphabets and punctuations
Enter a palindrome: "Dammit, I'm mad!"
"Dammit, I'm mad!" is a palindrome

$ ./palindrome.py
Enter a palindrome: a'a
Error: Less than 3 alphabets
Enter a palindrome: aaa
aaa is NOT a palindrome
```

* 让我们看下文档字符串怎么作为帮助使用
* 注意文档字符串是怎么自动格式化的

```python
>>> import palindrome
>>> help(palindrome)

Help on module palindrome:

NAME
    palindrome - Asks for user input and tells if string is palindrome or not

DESCRIPTION
    Allowed characters: alphabets and punctuations .,;:'"-!?
    Minimum alphabets: 3 and cannot be all same

    Informs if input is invalid and asks user for input again

FUNCTIONS
    is_palindrome(usr_ip)
        Checks if string is a palindrome

        ValueError: if string is invalid

        Returns True if palindrome, False otherwise

    main()

FILE
    /home/learnbyexample/python_programs/palindrome.py
```

* 也可以直接获取函数帮助

```python
>>> help(palindrome.is_palindrome)

Help on function is_palindrome in module palindrome:

is_palindrome(usr_ip)
    Checks if string is a palindrome

    ValueError: if string is invalid

    Returns True if palindrome, False otherwise
```

* 测试函数

```python
>>> palindrome.is_palindrome('aaa')
False
>>> palindrome.is_palindrome('Madam')
True

>>> palindrome.main()
Enter a palindrome: 3452
Error: Characters other than alphabets and punctuations
Enter a palindrome: Malayalam
Malayalam is a palindrome
```

**进一步阅读**

* [文档字符串格式](http://stackoverflow.com/questions/3898572/what-is-the-standard-python-docstring-format)
* [意外信息捕捉](http://stackoverflow.com/questions/4690600/python-exception-message-capturing)
