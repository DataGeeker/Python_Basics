# <a name="introduction"></a>介绍

* [安装](#installation)
* [Hello World 示例](#hello-world-example)
* [Python 解释器](#python-interpreter)
* [Python 标准库](#python-standard-library)


来自 [维基百科](https://wc.yooooo.us/d2lraS9QeXRob24hemg=)
>Python是一种广泛使用的高级编程语言，属于通用型编程语言，由吉多·范罗苏姆 创造，第一版发布于 1991 年。可以视之为一种改良 (加入一些其他编程语言的优点，如面向对象) 的 LISP。作为一种解释型语言，Python 的设计哲学强调代码的可读性和简洁的语法（尤其是使用空格缩进划分代码块，而非使用大括号或者关键词）。相比于 C++ 或 Java，Python 让开发者能够用更少的代码表达想法。不管是小型还是大型程序，该语言都试图让程序的结构清晰明了。

[吉多·范罗苏姆](https://wc.yooooo.us/wiki/%E5%90%89%E5%A4%9A%C2%B7%E8%8C%83%E7%BD%97%E8%8B%8F%E5%A7%86)（荷兰语：Guido van Rossum，1956年1月31日－），生于荷兰哈勒姆，计算机程序员，为Python程序设计语言的最初设计者及主要架构师。在Python社区，吉多·范罗苏姆被人们认为是“仁慈的独裁者”（BDFL），意思是他仍然关注Python的开发进程，并在必要的时刻做出决定。

<br>

### <a name="installation"></a>安装

* 从官网获取适合你系统的Python - https://www.python.org/ 
    * 大多数linux发行版默认安装了Python
* 另 见[这个指南](https://itsfoss.com/python-setup-linux/)获取更多细节和如何设置虚拟环境，如何使用**pip**（绝不要使用 **sudo pip**除非你自己知道自己在做什么）。

<br>

* 这里示例使用**Unix-like 系统**，Python版本3并且使用**bash**shell
* 你也可以线上运行Python代码
    * [pythontutor](http://www.pythontutor.com/visualize.html#mode=edit) - python 2和python 3版本代码执行器，可视化代码流，有样例程序
    * [jupyter](https://try.jupyter.org/) - 一款web应用：允许你创建和分享包含代码、公式、可视化以及解释的动态文档
    * [ideone](https://ideone.com/) - 在线编译和调试工具，允许你在线上执行和编译超过60种编程语言
    * [Python Interpreter shell](https://www.python.org/shell/)
* 假设你熟悉命令行。如果没有，查阅[ryanstutorials上的基本教程](http://ryanstutorials.net/linuxtutorial/)和[Linux整合资源列表](https://github.com/ShixiangWang/scripting_course/blob/master/Linux_curated_resources.md)

<br>

### <a name="hello-world-example"></a>Hello World 示例

Let's start with simple a Python program and how to run it

```python
#!/usr/bin/python3

print("Hello World")
```

The first line has two parts

* `/usr/bin/python3` is the path of Python interpreter
* `#!` called as **[shebang](https://en.wikipedia.org/wiki/Shebang_(Unix))**, directs the program loader to use the interpreter path provided

The third line prints the message `Hello World` with a newline character added by default by the `print` function

**Running Python program**

You can write the program using text editor like **gedit**, **[vim](http://yannesposito.com/Scratch/en/blog/Learn-Vim-Progressively/)** or [other editors](https://github.com/learnbyexample/Linux_command_line/blob/master/Working_with_Files_and_Directories.md#text-editors)  
After saving the file, give execute permission and run the program from a terminal

```
$ chmod +x hello_world.py

$ ./hello_world.py
Hello World
```

To find out path and version of Python in your system

```
$ type python3
python3 is /usr/bin/python3

$ python3 --version
Python 3.4.3
```

If you happen to follow a book/tutorial on Python version 2 or coming with Perl experience, it is a common mistake to forget () with `print` function

```python
#!/usr/bin/python3

print "Have a nice day"
```

* Depending on type of error, it may be easy to spot the mistake based on error messages printed on executing the program
* In this example, we get the appropriate `Missing parentheses` message

```
$ ./syntax_error.py 
  File "./syntax_error.py", line 3
    print "Have a nice day"
                          ^
SyntaxError: Missing parentheses in call to 'print'
```

* single line comments start with `#`
   * `#!` has special meaning only on first line of program
* we will see multiline comments in later chapters

```python
#!/usr/bin/python3

# Greeting message
print("Hello World")
```

**Further Reading**

* [Python docs - version 3](https://docs.python.org/3/index.html)
* [Different ways of executing Python programs](https://docs.python.org/3/using/windows.html#executing-scripts)
* [Where is Python used?](https://www.python.org/about/apps/)
* [Python docs - Errors and Exceptions](https://docs.python.org/3/tutorial/errors.html)
* [Common syntax errors](https://opencs.uwaterloo.ca/python-from-scratch/7/7/transcript)

<br>

### <a name="python-interpreter"></a>Python Interpreter

* It is generally used to execute snippets of Python language as a means to learning Python or for debugging purposes
* The prompt is usually `>>>`
* Some of the topics in coming chapters will be complemented with examples using the Python Interpreter
* A special variable `_` holds the result of last printed expression
* One can type part of command and repeatedly press Up arrow key to match commands from history
* Press `Ctrl+l` to clear the screen, keeping any typed command intact
* `exit()` to exit

```python
$ python3
Python 3.4.3 (default, Oct 14 2015, 20:28:29) 
[GCC 4.8.4] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> print("hi")
hi
>>> abc
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
NameError: name 'abc' is not defined
>>> num = 5
>>> num
5
>>> 3 + 4
7
>>> 12 + _
19
>>> exit()
```

**Further Reading**

* [Python docs - Using the Python Interpreter](https://docs.python.org/3/tutorial/interpreter.html)
* [Python docs - Interpreter example](https://docs.python.org/3/tutorial/introduction.html#using-python-as-a-calculator)

<br>

### <a name="python-standard-library"></a>Python Standard Library

* [Python docs - library](https://docs.python.org/3/library/index.html)
* [pypi - repository of software for the Python programming language](https://pypi.python.org/pypi)

>The library contains built-in modules (written in C) that provide access to system functionality such as file I/O that would otherwise be inaccessible to Python programmers, as well as modules written in Python that provide standardized solutions for many problems that occur in everyday programming.

>Some of these modules are explicitly designed to encourage and enhance the portability of Python programs by abstracting away platform-specifics into platform-neutral APIs
