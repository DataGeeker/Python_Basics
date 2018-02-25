# <a name="command-line-arguments"></a>命令行参数

* [已知参数数目](#known-number-of-arguments)
* [变长参数](#varying-number-of-arguments)
* [代码中使用程序名](#using-program-name-in-code)
* [命令行开关](#command-line-switches)

<br>

### <a name="known-number-of-arguments"></a>已知参数数目

```python
#!/usr/bin/python3

import sys

if len(sys.argv) != 3:
    sys.exit("Error: Please provide exactly two numbers as arguments")
else:
    (num1, num2) = sys.argv[1:]
    total = int(num1) + int(num2)
    print("{} + {} = {}".format(num1, num2, total))
```

* 传入Python程序的命令行参数自动保存在`sys.argv`列表中
* 让用户知道错误信息是非常好的想法
* 当程序因为错误的使用而终止报错，使用`sys.exit()`获取错误信息或者一个自定义状态码
* [Python文档 - sys模块](https://docs.python.org/3/library/sys.html#module-sys)

```
$ ./sum_of_two_numbers.py 2 3
2 + 3 = 5
$ echo $?
0

$ ./sum_of_two_numbers.py 2 3 7
Error: Please provide exactly two numbers as arguments
$ echo $?
1

$ ./sum_of_two_numbers.py 2 'x'
Traceback (most recent call last):
  File "./sum_of_two_numbers.py", line 9, in <module>
    total = int(num1) + int(num2)
ValueError: invalid literal for int() with base 10: 'x'
$ echo $?
1
```

<br>

### <a name="varying-number-of-arguments"></a>变长参数

```python
#!/usr/bin/python3

import sys, pathlib, subprocess

if len(sys.argv) < 2:
    sys.exit("Error: Please provide at least one filename as argument")

input_files = sys.argv[1:]
files_not_found = []

for filename in input_files:
    if not pathlib.Path(filename).is_file():
        files_not_found.append("File '{}' not found".format(filename))
        continue

    line_count = subprocess.getoutput('wc -l < ' + filename)
    print("{0:40}: {1:4} lines".format(filename, line_count))

print("\n".join(files_not_found))
```

* 当处理的用户输入包含文件名时，最好在处理之前进行检查
* [Python文档 - pathlib](https://docs.python.org/3/library/pathlib.html)

```
$ ./varying_command_line_args.py
Error: Please provide at least one filename as argument
$ echo $?
1

$ #选择性呈现输出
$ ./varying_command_line_args.py *.py
calling_shell_commands.py               : 14   lines
for_loop.py                             : 6    lines
functions_default_arg_value.py          : 16   lines
functions.py                            : 24   lines
hello_world.py                          : 3    lines
if_elif_else.py                         : 22   lines
if_else_oneliner.py                     : 6    lines
shell_command_output_redirections.py    : 21   lines
...

$ ./varying_command_line_args.py hello_world.py xyz.py for_loop.py abc.py
hello_world.py                          : 3    lines
for_loop.py                             : 6    lines
File 'xyz.py' not found
File 'abc.py' not found
```

* 如果你的Python版本不是3.4或者更高，使用`os`模块而不是`pathlib`模块进行文件检查

```python
import os

if not os.path.isfile(filename):
    sys.exit("File '{}' not found".format(filename))
```

<br>

### <a name="using-program-name-in-code"></a>在代码中使用程序名

```python
#!/usr/bin/python3

import sys, pathlib, subprocess, re

if len(sys.argv) != 2:
    sys.exit("Error: Please provide exactly one filename as argument")

program_name = sys.argv[0]
filename = sys.argv[1]

if not pathlib.Path(filename).is_file():
    sys.exit("File '{}' not found".format(filename))

if re.search(r'line_count.py', program_name):
    lc = subprocess.getoutput('wc -l < ' + filename)
    print("No. of lines in '{}' is: {}".format(filename, lc))
elif re.search(r'word_count.py', program_name):
    wc = subprocess.getoutput('wc -w < ' + filename)
    print("No. of words in '{}' is: {}".format(filename, wc))
else:
    sys.exit("Program name '{}' not recognized".format(program_name))
```

* 基于程序名，同一程序可以用作不同的目的

```
$ ./line_count.py if_elif_else.py
No. of lines in 'if_elif_else.py' is: 22

$ ln -s line_count.py word_count.py
$ ./word_count.py if_elif_else.py
No. of words in 'if_elif_else.py' is: 73

$ ln -s line_count.py abc.py
$ ./abc.py if_elif_else.py
Program name './abc.py' not recognized

$ wc -lw if_elif_else.py
 22  73 if_elif_else.py
```

<br>

### <a name="command-line-switches"></a>命令行开关

```python
#!/usr/bin/python3

import argparse, subprocess

parser = argparse.ArgumentParser()
parser.add_argument('-f', '--file', help="file to be sorted", required=True)
parser.add_argument('-u', help="sort uniquely", action="store_true")
args = parser.parse_args()

if args.u:
    subprocess.call(['sort', '-u', args.file, '-o', args.file])
else:
    subprocess.call(['sort', args.file, '-o', args.file])
```

* 使用`argparse`模块是一种更简单的方式使得构建的程序像shell命令
* 默认它添加一个help选项`-h`（短格式）和`--help`选项（长格式）处理错误的使用

在这个例子中：

* `-f`或`--file`声明为必需选项，它接受一个参数（文件名）
* `-u`是一个可选的标记
* `help`用于指定展示的文本和选项的帮助信息

```
$ ./sort_file.py
usage: sort_file.py [-h] -f FILE [-u]
sort_file.py: error: the following arguments are required: -f/--file

$ ./sort_file.py -h
usage: sort_file.py [-h] -f FILE [-u]

optional arguments:
  -h, --help            show this help message and exit
  -f FILE, --file FILE  file to be sorted
  -u                    sort uniquely

$ ./sort_file.py -f test_list.txt
$ cat test_list.txt
async_test
basic_test
input_test
input_test
output_test
sync_test

$ ./sort_file.py -f test_list.txt -u
$ cat test_list.txt
async_test
basic_test
input_test
output_test
sync_test

$ ./sort_file.py -f xyz.txt
sort: cannot read: xyz.txt: No such file or directory
```

* 指定位置参数

```python
#!/usr/bin/python3

import argparse

parser = argparse.ArgumentParser()
parser.add_argument('num1', type=int, help="first number")
parser.add_argument('num2', type=int, help="second number")
args = parser.parse_args()

total = args.num1 + args.num2
print("{} + {} = {}".format(args.num1, args.num2, total))
```

* 比手动解析`sys.argv`更可读的方法，同时提供默认帮助和错误处理
* 需要的类型转换可以在构建参数时指定

```
$ ./sum2nums_argparse.py
usage: sum2nums_argparse.py [-h] num1 num2
sum2nums_argparse.py: error: the following arguments are required: num1, num2

$ ./sum2nums_argparse.py -h
usage: sum2nums_argparse.py [-h] num1 num2

positional arguments:
  num1        first number
  num2        second number

optional arguments:
  -h, --help  show this help message and exit

$ ./sum2nums_argparse.py 3 4
3 + 4 = 7

$ ./sum2nums_argparse.py 3 4 7
usage: sum2nums_argparse.py [-h] num1 num2
sum2nums_argparse.py: error: unrecognized arguments: 7
```


**进一步阅读**

* [Python文档 - argparse教程](https://docs.python.org/3/howto/argparse.html#id1)
* [Python文档 - argparse模块](https://docs.python.org/3/library/argparse.html#module-argparse)
* [argparse解释实例](https://stackoverflow.com/questions/7427101/dead-simple-argparse-example-wanted-1-argument-3-results)
* [Python文档 - getopt模块](https://docs.python.org/3/library/getopt.html#module-getopt)
