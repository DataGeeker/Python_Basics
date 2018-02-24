# <a name="file-handling"></a>文件处理

* [open函数](#open-function)
* [读文件](#reading-files)
* [写文件](#writing-to-files)
* [使用fileinput就地编辑](#inplace-editing-with-fileinput)

<br>

### <a name="open-function"></a>open函数

* 语法：open(file, mode='r', buffering=-1, encoding=None, errors=None, newline=None, closefd=True, opener=None)

```python
>>> import locale
>>> locale.getpreferredencoding()
'UTF-8'
```

* 这一章中我们将看到下面一些文件操作模式
    * `r`打开文件用来读入
    * `w`打开文件用来写入
    * `a`打开文件用来追加
* 默认是文本模式，所以传入`r`和`rt`等价
    * 对于二进制模式，将对应是`rb`，`wb`等等
* `locale.getpreferredencoding()`给出默认使用的编码方式
* [Python文档 - open](https://docs.python.org/3/library/functions.html#open)
* [Python文档 - 标准编码](https://docs.python.org/3/library/codecs.html#standard-encodings)

<br>

### <a name="reading-files"></a>读文件

```python
#!/usr/bin/python3

# 打开文件，逐行读入并打印
filename = 'hello_world.py'
f = open(filename, 'r', encoding='ascii')

print("Contents of " + filename)
print('-' * 30)
for line in f:
    print(line, end='')

f.close()

# 'with'是一种更简单的方式，会自动处理文件关闭
filename = 'while_loop.py'

print("\n\nContents of " + filename)
print('-' * 30)
with open(filename, 'r', encoding='ascii') as f:
    for line in f:
        print(line, end='')
```

* 通常默认编码为'UTF-8'，这里设定为'ascii'
* 使用`with`并设定`f`为文件句柄是一种习惯

```bash
$ ./file_reading.py
Contents of hello_world.py
------------------------------
#!/usr/bin/python3

print("Hello World")


Contents of while_loop.py
------------------------------
#!/usr/bin/python3

# continuously ask user input till it is a positive integer
usr_string = 'not a number'
while not usr_string.isnumeric():
    usr_string = input("Enter a positive integer: ")
```

**如果文件不存在**

```python
#!/usr/bin/python3

with open('xyz.py', 'r', encoding='ascii') as f:
    for line in f:
        print(line, end='')
```

* 如果文件没有找到会返回错误

```bash
$ ./file_reading_error.py
Traceback (most recent call last):
  File "./file_reading_error.py", line 3, in <module>
    with open('xyz.py', 'r', encoding='ascii') as f:
FileNotFoundError: [Errno 2] No such file or directory: 'xyz.py'
$ echo $?
1
```

* 使用`read()`读入整个文件内容为单个字符串

```python
>>> f = open('hello_world.py', 'r', encoding='ascii')
>>> f
<_io.TextIOWrapper name='hello_world.py' mode='r' encoding='ascii'>
>>> print(f.read())
#!/usr/bin/python3

print("Hello World")

```

* 逐行读入使用`readline()`

```python
>>> f = open('hello_world.py', 'r', encoding='ascii')
>>> print(f.readline(), end='')
#!/usr/bin/python3
>>> print(f.readline(), end='')

>>> print(f.readline(), end='')
print("Hello World")

>>> f.close()
>>> print(f.readline(), end='')
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
ValueError: I/O operation on closed file.
```

* 使用`readlines()`读入所有行为列表
    * 注意复数形式

```python
>>> f = open('hello_world.py', 'r', encoding='ascii')
>>> all_lines = f.readlines()
>>> all_lines
['#!/usr/bin/python3\n', '\n', 'print("Hello World")\n']
```

* 检查文件是否关闭

```python
>>> f = open('hello_world.py', 'r', encoding='ascii')

>>> f.closed
False

>>> f.close()
>>> f.closed
True
```

<br>

### <a name="writing-to-files"></a>写文件

```python
#!/usr/bin/python3

with open('new_file.txt', 'w', encoding='ascii') as f:
    f.write("This is a sample line of text\n")
    f.write("Yet another line\n")
```

* 使用`write()`方法打印一个字符串到文件
* 想要添加文本到已存在的文件，使用`a`模式而不是`w`模式


```bash
$ ./file_writing.py
$ cat new_file.txt
This is a sample line of text
Yet another line
```

<br>

### <a name="inplace-editing-with-fileinput"></a>使用fileinput就地编辑

```python
#!/usr/bin/python3

import fileinput

with fileinput.input(inplace=True) as f:
    for line in f:
        line = line.replace('line of text', 'line')
        print(line, end='')
```

* 当程序运行时，将被修改的文件都会指定为[命令行参数](./Command_line_arguments.md)
* 注意`print`函数必须用`f.write`替代
* 因为迭代的每行已经有换行符，尾部给空字符串
* [Python文档 - fileinput](https://docs.python.org/3/library/fileinput.html)

```bash
$ ./inplace_file_editing.py new_file.txt
$ cat new_file.txt
This is a sample line
Yet another line

$ # 要改变当前目录中所有以.txt结尾的文件，使用
$ ./inplace_file_editing.py *.txt

$ # stdin也可以作为输入
$ echo 'a line of text' | ./inplace_file_editing.py
a line
```

* 指定文件名和备份扩展

```python
# 在程序内指定文件名
with fileinput.input(inplace=True, files=('file1.txt', 'file2.txt')) as f:

# 要创建一个不修改的文件备份，传入对应的备份参数
with fileinput.input(inplace=True, backup='.bkp') as f:
```
