# <a name="executing-external-commands"></a>执行外部命令

* [调用Shell命令](#calling-shell-commands)
* [使用扩展调用Shell命令](#calling-shell-commands-with-expansion)
* [获取命令输出和重定向](#getting-command-output-and-redirections)

这一章节的样例输出可能基于你的用户名、工作目录等等而有所不同

<br>

### <a name="calling-shell-commands"></a>调用Shell命令

```python
#!/usr/bin/python3

import subprocess

# 执行外部命令 'date'
subprocess.call('date')

# 传递选项和参数给命令
print("\nToday is ", end="", flush=True)
subprocess.call(['date', '-u', '+%A'])

# 其他例子
print("\nSearching for 'hello world'", flush=True)
subprocess.call(['grep', '-i', 'hello world', 'hello_world.py'])
```

* 这里的`import`语句用于载入`subprocess`模块，它是[Python标准库](https://docs.python.org/3/library/index.html)的一部分
* `subprocess`模块中的`call`函数是一种执行外部命令的方式
* 通过传递`True`给`flush`参数（默认是`False`），我们确保这个信息在`subprocess.call`运行之前输出
* 想要给命令传递参数，需要使用字符串列表

```
$ ./calling_shell_commands.py
Tue Jun 21 18:35:33 IST 2016

Today is Tuesday

Searching for 'hello world'
print("Hello World")
```

**进一步阅读**

* [Python文档 - subprocess](https://docs.python.org/3/library/subprocess.html)
* [Python文档 - os.system](https://docs.python.org/3/library/os.html#os.system)
    * [os.system和subprocess.call的不同](https://www.quora.com/Whats-the-difference-between-os-system-and-subprocess-call-in-Python)
* [Python文档 - import语句](https://docs.python.org/3/reference/simple_stmts.html#import)

<br>

### <a name="calling-shell-commands-with-expansion"></a>使用扩展调用Shell命令

```python
#!/usr/bin/python3

import subprocess

# 不使用扩展调用Shell命令
print("No shell expansion when shell=False", flush=True)
subprocess.call(['echo', 'Hello $USER'])

# 使用Shell扩展调用Shell命令
print("\nshell expansion when shell=True", flush=True)
subprocess.call('echo Hello $USER', shell=True)

# 如果引号是命令的一部分则进行反义
print("\nSearching for 'hello world'", flush=True)
subprocess.call('grep -i \'hello world\' hello_world.py', shell=True)
```

* 默认`subprocess.call`不会扩展[shell 通配符](https://github.com/ShixiangWang/Linux_command_line/blob/master/Shell.md#wildcards)，使用 [command替换](http://mywiki.wooledge.org/CommandSubstitution)等等
* 可以设定`shell`参数为`True`进行重写
* 注意现在整个命令行都作为一个字符串而不是字符串列表
* 命令中含有引号如要转义
* 仅在你确定命令会正确执行的情况下使用`shell=True`，否则会存在[安全问题](https://stackoverflow.com/questions/3172470/actual-meaning-of-shell-true-in-subprocess)
    * [Python文档 - subprocess.Popen](https://docs.python.org/3/library/subprocess.html#popen-constructor)

```
$ ./shell_expansion.py
No shell expansion when shell=False
Hello $USER

shell expansion when shell=True
Hello learnbyexample

Searching for 'hello world'
print("Hello World")
```

* 在特定情况下，我们可以使用单/双引号的组合来避免使用转义符号

```python
# 像这样使用另一种引号
subprocess.call('grep -i "hello world" hello_world.py', shell=True)

# 或者这样
subprocess.call("grep -i 'hello world' hello_world.py", shell=True)
```

* [Shell命令重定向](https://github.com/ShixiangWang/Linux_command_line/blob/master/Shell.md#redirection)可以正常使用

```python
# 为了避免call函数字符串太常，我们使用变量先存储命令
cmd = "grep -h 'test' report.log test_list.txt > grep_test.txt"
subprocess.call(cmd, shell=True)
```

**避开使用shell=True**

```python
>>> import subprocess, os
>>> subprocess.call(['echo', 'Hello', os.environ.get("USER")])
Hello learnbyexample
0
```

* `os.environ.get("USER")`返回环境变量`USER`的值
* `0`退出状态码，意味着成功执行了命令。

<br>

### <a name="getting-command-output-and-redirections"></a>获取命令输出和重定向

```python
#!/usr/bin/python3

import subprocess

# 输出也包含任何的错误信息
print("Getting output of 'pwd' command", flush=True)
curr_working_dir = subprocess.getoutput('pwd')
print(curr_working_dir)

# 获取命令执行的状态和输出
# 退出状态码不是“0”意味着有什么事情出错了
ls_command = 'ls hello_world.py xyz.py'
print("\nCalling command '{}'".format(ls_command), flush=True)
(ls_status, ls_output) = subprocess.getstatusoutput(ls_command)
print("status: {}\noutput: '{}'".format(ls_status, ls_output))

# 抑制出错信息
# subprocess.call()返回命令的状态码 returns status of command which can be used instead
print("\nCalling command with error msg suppressed", flush=True)
ls_status = subprocess.call(ls_command, shell=True, stderr=subprocess.DEVNULL)
print("status: {}".format(ls_status))
```

* `getstatusoutput()`的输出是元组数据类型，更多信息和例子在后续章节
* `getstatusoutput()`和`getoutput()`老式的函数
* 使用有更多特性和安全选项的新函数
    * [Python文档 - subprocess.check_output](https://docs.python.org/3/library/subprocess.html#subprocess.check_output)
    * [Python文档 - subprocess.run](https://docs.python.org/3/library/subprocess.html#subprocess.run)

```
$ ./shell_command_output_redirections.py
Getting output of 'pwd' command
/home/learnbyexample/Python/python_programs

Calling command 'ls hello_world.py xyz.py'
status: 2
output: 'ls: cannot access xyz.py: No such file or directory
hello_world.py'

Calling command with error msg suppressed
hello_world.py
status: 2
```
