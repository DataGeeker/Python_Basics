# <a name="testing"></a>测试

* [assert语句](#assert-statement)
* [使用assert测试程序](#using-assert-to-test-a-program)
* [使用unittest框架](#using-unittest-framework)
* [使用unittest.mock测试用户输入和程序输出](#using-unittest.mock-to-test-user-input-and-program-output)
* [其他测试框架](#other-testing-frameworks)


<br>

### <a name="assert-statement"></a>assert语句

* `assert`主要用于调试目的，像捕捉非法的输入或者不应该发生的情况
* 备择的信息可以用来描述错误信息而不只是一个简单的**AssertionError**
* 它通过[raise语句](https://docs.python.org/3/tutorial/errors.html#raising-exceptions)实施
* `assert`语句可以通过传入`-O` [命令行选项](https://docs.python.org/3/using/cmdline.html)跳过
* **注意**`assert`是一个语句而不是函数

```python
>>> assert 2 ** 3 == 8
>>> assert 3 > 4
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
AssertionError

>>> assert 3 > 4, "3 is not greater than 4"
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
AssertionError: 3 is not greater than 4
```

让我们看一个阶乘函数例子：

```python
>>> def fact(n):
        total = 1
        for num in range(1, n+1):
            total *= num
        return total

>>> assert fact(4) == 24
>>> assert fact(0) == 1
>>> fact(5)
120

>>> fact(-3)
1
>>> fact(2.3)
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
  File "<stdin>", line 3, in fact
TypeError: 'float' object cannot be interpreted as an integer
```

* `assert fact(4) == 24`和`assert fact(0) == 1`可以看作是函数的样例测试

让我们看看`assert`怎么用于传入函数的参数验证：

```python
>>> def fact(n):
        assert type(n) == int and n >= 0, "Number should be zero or positive integer"
        total = 1
        for num in range(1, n+1):
            total *= num
        return total

>>> fact(5)
120
>>> fact(-3)
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
  File "<stdin>", line 2, in fact
AssertionError: Number should be zero or positive integer
>>> fact(2.3)
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
  File "<stdin>", line 2, in fact
AssertionError: Number should be zero or positive integer
```

<br>

上面的阶乘函数可以用[reduce](https://docs.python.org/3/library/functools.html#functools.reduce)写

```python
>>> def fact(n):
        assert type(n) == int and n >= 0, "Number should be zero or positive integer"
        from functools import reduce
        from operator import mul
        return reduce(mul, range(1, n+1), 1)


>>> fact(23)
25852016738884976640000
```

上面的例子仅仅是用于解释，实际操作使用`math.factorial`函数，它会抛出合适的意外信息

```python
>>> from math import factorial
>>> factorial(10)
3628800

>>> factorial(-5)
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
ValueError: factorial() not defined for negative values
>>> factorial(3.14)
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
ValueError: factorial() only accepts integral values
```

**进一步阅读**

* [Python文档 - assert](https://docs.python.org/3/reference/simple_stmts.html#assert)
* [Python中assert的用处是什么？](https://stackoverflow.com/questions/5142418/what-is-the-use-of-assert-in-python)
* [单元测试重要吗？](https://stackoverflow.com/questions/67299/is-unit-testing-worth-the-effort)

<br>

### <a name="using-assert-to-test-a-program"></a>使用assert测试程序

我们可以使用`assert`测试程序 - 要么在程序内或者作为独立的测试程序

让我们测试在[文档字符串](./Docstrings.md#palindrome-example)章节的例子

```python
#!/usr/bin/python3

import palindrome

assert palindrome.is_palindrome('Madam')
assert palindrome.is_palindrome("Dammit, I'm mad!")
assert not palindrome.is_palindrome('aaa')
assert palindrome.is_palindrome('Malayalam')

try:
    assert palindrome.is_palindrome('as2')
except ValueError as e:
    assert str(e) == 'Characters other than alphabets and punctuations'

try:
    assert palindrome.is_palindrome("a'a")
except ValueError as e:
    assert str(e) == 'Less than 3 alphabets'

print('All tests passed')
```

* 这里测试了**is_palindrome**四种不同的情况
    * 合法的回文字符串
    * 不合法的回文字符串
    * 字符串中不合法的字符
    * 字符串中缺乏的字符
* 被测试的程序和用于测试的程序在同一目录
* 要测试**main**函数，我们需要模拟用户输入。对于这个和其他有用的特性，测试框架更便利

```
$ ./test_palindrome.py
All tests passed
```

<br>

### <a name="using-unittest-framework"></a>使用unittest框架

这部分需要理解[类](https://docs.python.org/3/tutorial/classes.html)

<br>

```python
#!/usr/bin/python3

import palindrome
import unittest

class TestPalindrome(unittest.TestCase):

    def test_valid(self):
        # 检查合法的输入字符串
        self.assertTrue(palindrome.is_palindrome('kek'))
        self.assertTrue(palindrome.is_palindrome("Dammit, I'm mad!"))
        self.assertFalse(palindrome.is_palindrome('zzz'))
        self.assertFalse(palindrome.is_palindrome('cool'))

    def test_error(self):
        # 仅检查抛出的意外c
        with self.assertRaises(ValueError):
            palindrome.is_palindrome('abc123')

        with self.assertRaises(TypeError):
            palindrome.is_palindrome(7)

        # 检查错误信息
        with self.assertRaises(ValueError) as cm:
            palindrome.is_palindrome('on 2 no')
        em = str(cm.exception)
        self.assertEqual(em, 'Characters other than alphabets and punctuations')

        with self.assertRaises(ValueError) as cm:
            palindrome.is_palindrome('to')
        em = str(cm.exception)
        self.assertEqual(em, 'Less than 3 alphabets')

if __name__ == '__main__':
    unittest.main()
```

* 首先我们创建一个**unittest.TestCase**子类（继承）
* 然后，不同的检查类型可以用不同的函数分组，以**test**开始的函数名会自动被 **unittest.main()** 调用
* 基于测试的类型，可以使用**assertTrue, assertFalse, assertRaises, assertEqual等等**
* [类和继承介绍](http://www.jesshamrick.com/2011/05/18/an-introduction-to-classes-and-inheritance-in-python/)
* [Python文档 - unittest](https://docs.python.org/3/library/unittest.html)
    * [命令行接口](https://docs.python.org/3/library/unittest.html#command-line-interface)
    * [测试探索](https://docs.python.org/3/library/unittest.html#test-discovery)
    * [组织测试代码](https://docs.python.org/3/library/unittest.html#organizing-test-code)

```
$ ./unittest_palindrome.py
..
----------------------------------------------------------------------
Ran 2 tests in 0.001s

OK

$ ./unittest_palindrome.py -v
test_error (__main__.TestPalindrome) ... ok
test_valid (__main__.TestPalindrome) ... ok

----------------------------------------------------------------------
Ran 2 tests in 0.001s

OK
```

<br>

### <a name="using-unittest.mock-to-test-user-input-and-program-output"></a>使用unittest.mock测试用户输入和程序输出

这部分需要理解装饰器，[阅读这个很棒的介绍](https://stackoverflow.com/questions/739654/how-to-make-a-chain-of-function-decorators-in-python/1594484#1594484)

<br>

一个简单例子：查看如何捕捉`print`的输出用于测试

```python
>>> from unittest import mock
>>> from io import StringIO

>>> def greeting():
        print('Hi there!')

>>> def test():
        with mock.patch('sys.stdout', new_callable=StringIO) as mock_stdout:
            greeting()
            assert mock_stdout.getvalue() == 'Hi there!\n'

>>> test()
```

One can also use `decorators`

```python
>>> @mock.patch('sys.stdout', new_callable=StringIO)
    def test(mock_stdout):
        greeting()
        assert mock_stdout.getvalue() == 'Hi there!\n'
```

<br>

现在让我们看如何模拟`input`

```python
>>> def greeting():
        name = input('Enter your name: ')
        print('Hello', name)

>>> greeting()
Enter your name: learnbyexample
Hello learnbyexample

>>> with mock.patch('builtins.input', return_value='Tom'):
        greeting()

Hello Tom
```

<br>

组合两者

```python
>>> @mock.patch('sys.stdout', new_callable=StringIO)
    def test_greeting(name, mock_stdout):
        with mock.patch('builtins.input', return_value=name):
            greeting()
            assert mock_stdout.getvalue() == 'Hello ' + name + '\n'

>>> test_greeting('Jo')
```

<br>

我们已经看过了基本的输入/输出测试，再把它们应用到**palindrome**的主函数

```python
#!/usr/bin/python3

import palindrome
import unittest
from unittest import mock
from io import StringIO

class TestPalindrome(unittest.TestCase):

    @mock.patch('sys.stdout', new_callable=StringIO)
    def main_op(self, tst_str, mock_stdout):
        with mock.patch('builtins.input', side_effect=tst_str):
            palindrome.main()
        return mock_stdout.getvalue()

    def test_valid(self):
        for s in ('Malayalam', 'kek'):
            self.assertEqual(self.main_op([s]), s + ' is a palindrome\n')

        for s in ('zzz', 'cool'):
            self.assertEqual(self.main_op([s]), s + ' is NOT a palindrome\n')

    def test_error(self):
        em1 = 'Error: Characters other than alphabets and punctuations\n'
        em2 = 'Error: Less than 3 alphabets\n'

        tst1 = em1 + 'Madam is a palindrome\n'
        self.assertEqual(self.main_op(['123', 'Madam']), tst1)

        tst2 = em2 + em1 + 'Jerry is NOT a palindrome\n'
        self.assertEqual(self.main_op(['to', 'a2a', 'Jerry']), tst2)

if __name__ == '__main__':
    unittest.main()
```

* 两个测试函数 - 一个用于测试合法的输入字符串，另一个用于测试错误信息
* 这里**side_effect**接受像列表这样的迭代变量，而**return_value**仅能模拟一个输入值
* 对于合法的输入字符串，**palindrome** 主函数仅需要一个输入值
* 对于错误的情况，迭代变量更便利，因为主函数会一直等待合法的输入
* [Python文档 - unittest.mock](https://docs.python.org/3/library/unittest.mock.html)
    * [patchers](https://docs.python.org/3/library/unittest.mock.html#the-patchers)
* [Python Mocking介绍](https://www.toptal.com/python/an-introduction-to-mocking-in-python)
* [PEP 0318 - 装饰器](https://www.python.org/dev/peps/pep-0318/)
* [装饰器](https://pythonconquerstheuniverse.wordpress.com/2012/04/29/python-decorators/)

```
$ ./unittest_palindrome_main.py
..
----------------------------------------------------------------------
Ran 2 tests in 0.003s

OK
```

<br>

### <a name="other-testing-frameworks"></a>其他测试框架

* [pytest](http://doc.pytest.org/en/latest/getting-started.html)
* [Python文档 - doctest](https://docs.python.org/3/library/doctest.html)
* [Python测试自动化](https://github.com/atinfo/awesome-test-automation/blob/master/python-test-automation.md)
* [Python测试工具分类](https://wiki.python.org/moin/PythonTestingToolsTaxonomy)
* [Python测试框架](http://docs.python-guide.org/en/latest/writing/tests/)

测试驱动环境 (TDD)

* [Python测试驱动环境](http://chimera.labs.oreilly.com/books/1234000000754/index.html)
* [通过TDD学习Python](https://github.com/gregmalcolm/python_koans)
