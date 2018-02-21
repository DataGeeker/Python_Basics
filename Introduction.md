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

* 这里示例使用**类Unix系统**，Python版本3并且使用**bash** shell
* 你也可以线上运行Python代码
    * [pythontutor](http://www.pythontutor.com/visualize.html#mode=edit) - python 2和python 3版本代码执行器，可视化代码流，有样例程序
    * [jupyter](https://try.jupyter.org/) - 一款web应用：允许你创建和分享包含代码、公式、可视化以及解释的动态文档
    * [ideone](https://ideone.com/) - 在线编译和调试工具，允许你在线上执行和编译超过60种编程语言
    * [Python Interpreter shell](https://www.python.org/shell/)
* 假设你熟悉命令行。如果没有，查阅[ryanstutorials上的基本教程](http://ryanstutorials.net/linuxtutorial/)和[Linux整合资源列表](https://github.com/ShixiangWang/scripting_course/blob/master/Linux_curated_resources.md)

<br>

### <a name="hello-world-example"></a>Hello World 示例

让我们从一个简单的程序开始学习使用Python：

```python
#!/usr/bin/python3

print("Hello World")
```

第一行有两部分

* `/usr/bin/python3` 是Python解释器的路径
* `#!`称为 **[shebang](http://blog.csdn.net/jasonchen_gbd/article/details/55012859)**，它指明了执行这个脚本文件的解释程序。

第三行输出`Hello World`信息，`print`函数默认会在后面添加换行符。

**运行Python程序**

你可以用像**gedit**、**[vim](http://yannesposito.com/Scratch/en/blog/Learn-Vim-Progressively/)** 或[其他编辑器](https://github.com/ShixiangWang/Linux_command_line/blob/master/Working_with_Files_and_Directories.md#text-editors)这样一些文本编辑器书写脚本程序。保存文件后，添加执行权限并从终端运行程序。

```shell
$ chmod +x hello_world.py

$ ./hello_world.py
Hello World
```

下面是寻找Python路径及其版本的方式：

```shell
$ type python3
python3 is /usr/bin/python3

$ python3 --version
Python 3.4.3
```

如果你学习过Python 2教程或者有过Perl语言使用经历，很容易忘记给`print`函数添加括号，这是一个常见错误。

```python
#!/usr/bin/python3

print "Have a nice day"
```

* 取决于错误类型，根据执行程序输出的信息定位错误的位置有可能非常容易
* 这个例子中，我们就得到合适的“缺失括号”信息
* Depending on type of error, it may be easy to

```
$ ./syntax_error.py
  File "./syntax_error.py", line 3
    print "Have a nice day"
                          ^
SyntaxError: Missing parentheses in call to 'print'
```

* 单行注释起始于`#`
    * `#!`仅在程序的第一行有特殊的含义
* 在后面章节我们会看到多行注释

```python
#!/usr/bin/python3

# 问候信息
print("Hello World")
```

**进一步阅读**

* [Python文档 - 版本3](https://docs.python.org/3/index.html)
* [执行Python程序的不同方式](https://docs.python.org/3/using/windows.html#executing-scripts)
* [Python应用何处？](https://www.python.org/about/apps/)
* [Python文档 - 错误和异常](https://docs.python.org/3/tutorial/errors.html)
* [常见语法错误](https://opencs.uwaterloo.ca/python-from-scratch/7/7/transcript)

<br>

### <a name="python-interpreter"></a>Python解释器

* 通常用于执行一小段的Python语句，目的是学习和调试
* 提示符为 `>>>`
* 接下来章节的一些主题会使用Python解释器进行示例
* 特殊变量`_`保存上一次输出表达式的结果
* 我们可以只键入部分命令和重复按`Up`键位去匹配历史命令
* `Ctrl+l`组合键用来清屏，会保存任何已键入的命令完整
* `exit()`退出


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

**进一步阅读**

* [Python文档 - 使用Python解释器](https://docs.python.org/3/tutorial/interpreter.html)
* [Python文档 - 解释器示例](https://docs.python.org/3/tutorial/introduction.html#using-python-as-a-calculator)

<br>

### <a name="python-standard-library"></a>Python标准库

* [Python文档 - 库](https://docs.python.org/3/library/index.html)
* [pypi - Python编程语言软件仓库](https://pypi.python.org/pypi)
>该库包含内置模块（用C编写）——提供系统功能特性接口比如文件I/O和Python编写的模块——提供许多日程编程问题的标准方案。

>其中的一些模块通过把平台特异的功能抽象为平台兼容的APIs，鼓励和增强Python程序的兼容特性。
