# <a name="lists"></a>列表

* [列表变量赋值](#assigning-list-variables)
* [列表切片和修改](#slicing-and-modifying-lists)
* [列表拷贝](#copying-lists)
* [列表方法和混杂](#list-methods-and-miscellaneous)
* [循环](#looping)
* [列表推导式](#list-comprehension)
* [获取列表作为用户输入](#list-user-input)
* [从列表中获取随机项](#getting-random-items-from-list)

<br>

### <a name="assigning-list-variables"></a>列表变量赋值

* 简单列表和提取列表元素

```python
>>> vowels = ['a', 'e', 'i', 'o', 'u']
>>> vowels
['a', 'e', 'i', 'o', 'u']
>>> vowels[0]
'a'
>>> vowels[2]
'i'
>>> vowels[10]
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
IndexError: list index out of range

>>> even_numbers = list(range(2, 11, 2))
>>> even_numbers
[2, 4, 6, 8, 10]
>>> even_numbers[-1]
10
>>> even_numbers[-2]
8
>>> even_numbers[-15]
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
IndexError: list index out of range
```

* 数据类型混合和多维列表

```python
>>> student = ['learnbyexample', 2016, 'Linux, Vim, Python']
>>> print(student)
['learnbyexample', 2016, 'Linux, Vim, Python']

>>> list_2D = [[1, 3, 2, 10], [1.2, -0.2, 0, 2]]
>>> list_2D[0][0]
1
>>> list_2D[1][0]
1.2
```

* [Python文档 - 列表](https://docs.python.org/3/tutorial/introduction.html#lists)

<br>

### <a name="slicing-and-modifying-lists"></a>列表切片和修改

* 类似`range()`函数，列表索引也是`start:stop:step`， `stop`值不被包含在内
* [stackoverflow - 解释切片符号](https://stackoverflow.com/questions/509211/explain-pythons-slice-notation)

```python
>>> books = ['Harry Potter', 'Sherlock Holmes', 'To Kill a Mocking Bird']
>>> books[2] = "Ender's Game"
>>> print(books)
['Harry Potter', 'Sherlock Holmes', "Ender's Game"]

>>> prime = [2, 3, 5, 7, 11]
>>> prime[2:4]
[5, 7]
>>> prime[:3]
[2, 3, 5]
>>> prime[3:]
[7, 11]

>>> prime[-1]
11
>>> prime[-1:] # 注意和上一个操作的不同
[11]
>>> prime[-2:]
[7, 11]


>>> prime[::1]
[2, 3, 5, 7, 11]
>>> prime[::2]
[2, 5, 11]
>>> prime[3:1:-1]
[7, 5]
>>> prime[::-1]
[11, 7, 5, 3, 2]
>>> prime[:]
[2, 3, 5, 7, 11]
```

* `start`和`stop`值相同在被程序自动生成时很有用
*  查看[文本处理练习](./Exercises.md#text-processing)的例子

```bash
>>> nums = [1.2, -0.2, 0, 2]
>>> nums[0:0]
[]
>>> nums[2:2]
[]
>>> nums[-1:-1]
[]
>>> nums[21:21]
[]
```

* 这种索引格式可以用于抽取列表元素或者修改列表本身

```python
>>> nums = [1.2, -0.2, 0, 2]
>>> nums[:2] = [1]
>>> nums
[1, 0, 2]

>>> nums = [1.2, -0.2, 0, 2, 4, 23]
>>> nums[:5:2] = [1, 4, 3]
>>> nums
[1, -0.2, 4, 2, 3, 23]

>>> nums = [1, 2, 3, 23]
>>> nums[::-1] = [1, 4, 5, 2]
>>> nums
[2, 5, 4, 1]
```

* 可以不改变`id`就修改一个列表，如果变量名在别处使用那么这个操作会非常有用

```python
>>> id(nums)
140598790579336
>>> nums[:] = [1, 2, 5, 4.3]
>>> nums
[1, 2, 5, 4.3]
>>> id(nums)
140598790579336

# 不使用索引[:]则会改变id
>>> nums = [1.2, -0.2, 0, 2]
>>> id(nums)
140598782943752
# 译者注：这应当是另外分配了一个新的地址和变量，然后对当前存在的进行了覆盖
```

<br>

### <a name="copying-lists"></a>列表拷贝

* Python中的变量包含对象的引用
* 例如，当一个整数变量修改后，变量的引用通过一个新的对象进行更新
* [id()](https://docs.python.org/3/library/functions.html#id)函数返回一个对象的“唯一标识符”
* 对于变量指向不可变的类型（如整型和字符串），这种区分通过不会对它们的使用造成困扰

```python
>>> a = 5
>>> id(a)
10105952
>>> a = 10
>>> id(a)
10106112

>>> b = a
>>> id(b)
10106112
>>> b = 4
>>> b
4
>>> a
10
>>> id(b)
10105920
```

* 但是对于指向可变类型（比如列表）的变量，知道变量是如何拷贝和传入函数是非常重要的
* 当列表中的一个元素被修改，它是通过改变对象那个索引的值实现的


```python
>>> a = [1, 2, 5, 4.3]
>>> a
[1, 2, 5, 4.3]
>>> b = a
>>> b
[1, 2, 5, 4.3]
>>> id(a)
140684757120264
>>> id(b)
140684757120264
>>> b[0] = 'xyz'
>>> b
['xyz', 2, 5, 4.3]
>>> a
['xyz', 2, 5, 4.3]
```

* 避免通过索引的方式拷贝列表，它对1维列表起作用，但不适用于更高维度

```python
>>> prime = [2, 3, 5, 7, 11]
>>> b = prime[:]
>>> id(prime)
140684818101064
>>> id(b)
140684818886024
>>> b[0] = 'a'
>>> b
['a', 3, 5, 7, 11]
>>> prime
[2, 3, 5, 7, 11]

>>> list_2D = [[1, 3, 2, 10], [1.2, -0.2, 0, 2]]
>>> a = list_2D[:]
>>> id(list_2D)
140684818102344
>>> id(a)
140684818103048
>>> a
[[1, 3, 2, 10], [1.2, -0.2, 0, 2]]
>>> a[0][0] = 'a'
>>> a
[['a', 3, 2, 10], [1.2, -0.2, 0, 2]]
>>> list_2D
[['a', 3, 2, 10], [1.2, -0.2, 0, 2]]
```

* 使用[copy](https://docs.python.org/3/library/copy.html#module-copy)模块替换

```python
>>> import copy
>>> list_2D = [[1, 3, 2, 10], [1.2, -0.2, 0, 2]]
>>> c = copy.deepcopy(list_2D)
>>> c[0][0] = 'a'
>>> c
[['a', 3, 2, 10], [1.2, -0.2, 0, 2]]
>>> list_2D
[[1, 3, 2, 10], [1.2, -0.2, 0, 2]]
```

<br>

### <a name="list-methods-and-miscellaneous"></a>列表方法和混杂

* 添加元素到列表

```python
>>> books = []
>>> books
[]
>>> books.append('Harry Potter')
>>> books
['Harry Potter']

>>> even_numbers
[2, 4, 6, 8, 10]
>>> even_numbers += [12, 14, 16, 18, 20]
>>> even_numbers
[2, 4, 6, 8, 10, 12, 14, 16, 18, 20]

>>> even_numbers = [2, 4, 6, 8, 10]
>>> even_numbers.extend([12, 14, 16, 18, 20])
>>> even_numbers
[2, 4, 6, 8, 10, 12, 14, 16, 18, 20]

>>> a = [[1, 3], [2, 4]]
>>> a.extend([[5, 6]])
>>> a
[[1, 3], [2, 4], [5, 6]]
```

* 从列表中删除元素 - 基于索引

```python
>>> prime = [2, 3, 5, 7, 11]
>>> prime.pop()
11
>>> prime
[2, 3, 5, 7]
>>> prime.pop(0)
2
>>> prime
[3, 5, 7]

>>> list_2D = [[1, 3, 2, 10], [1.2, -0.2, 0, 2]]
>>> list_2D[0].pop(0)
1
>>> list_2D
[[3, 2, 10], [1.2, -0.2, 0, 2]]

>>> list_2D.pop(1)
[1.2, -0.2, 0, 2]
>>> list_2D
[[3, 2, 10]]
```

* 使用[del](https://docs.python.org/3/reference/simple_stmts.html#del)删除元素

```python
>>> nums = [1.2, -0.2, 0, 2, 4, 23]
>>> del nums[1]
>>> nums
[1.2, 0, 2, 4, 23]
# 也可以使用切片
>>> del nums[1:4]
>>> nums
[1.2, 23]

>>> list_2D = [[1, 3, 2, 10], [1.2, -0.2, 0, 2]]
>>> del list_2D[0][1]
>>> list_2D
[[1, 2, 10], [1.2, -0.2, 0, 2]]

>>> del list_2D[0]
>>> list_2D
[[1.2, -0.2, 0, 2]]
```

* 清洗列表

```python
>>> prime = [2, 3, 5, 7, 11]
>>> prime.clear()
>>> prime
[]
```

* 从列表中删除元素 - 基于值

```python
>>> even_numbers = [2, 4, 6, 8, 10]
>>> even_numbers.remove(8)
>>> even_numbers
[2, 4, 6, 10]
>>> even_numbers.remove(12)
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
ValueError: list.remove(x): x not in list
```

* 在特定的索引位置插入元素

```python
>>> books = ['Harry Potter', 'Sherlock Holmes', 'To Kill a Mocking Bird']
>>> books.insert(2, "The Martian")
>>> books
['Harry Potter', 'Sherlock Holmes', 'The Martian', 'To Kill a Mocking Bird']
```

* 获取元素的索引

```python
>>> even_numbers = [2, 4, 6, 8, 10]
>>> even_numbers.index(6)
2
>>> even_numbers.index(12)
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
ValueError: 12 is not in list
```

* 查看元素是否存在

```python
>>> prime = [2, 3, 5, 7, 11]
>>> 3 in prime
True
>>> 13 in prime
False
```

* [排序](https://docs.python.org/3/library/stdtypes.html#list.sort)

```python
>>> a = [1, 5.3, 321, 0, 1, 2]
>>> a.sort()
>>> a
[0, 1, 1, 2, 5.3, 321]

>>> a = [1, 5.3, 321, 0, 1, 2]
>>> a.sort(reverse=True)
>>> a
[321, 5.3, 2, 1, 1, 0]
```

[基于键key](https://docs.python.org/3/howto/sorting.html#sortinghowto)进行排序，例如基于字符串长度
[lambda表达式](https://docs.python.org/3/tutorial/controlflow.html#lambda-expressions)在比如传递自定义单个表达式、基于第二个字母进行排序中很有用

```python
>>> words = ['fuliginous', 'crusado', 'morello', 'irk', 'seam']
>>> words.sort(key=len)
>>> words
['irk', 'seam', 'crusado', 'morello', 'fuliginous']

>>> words.sort(key=lambda x: x[1])
>>> words
['seam', 'morello', 'irk', 'crusado', 'fuliginous']
```

如果不想要改变原始列表，使用[sorted](https://docs.python.org/3/library/functions.html#sorted)函数

```python
>>> nums = [-1, 34, 0.2, -4, 309]
>>> nums_desc = sorted(nums, reverse=True)
>>> nums_desc
[309, 34, 0.2, -1, -4]

>>> sorted(nums, key=abs)
[0.2, -1, -4, 34, 309]
```

* `min`和`max`

```python
>>> a = [321, 899.232, 5.3, 2, 1, -1]
>>> min(a)
-1
>>> max(a)
899.232
```

* 元素存在的次数（数目）

```python
>>> nums = [15, 99, 19, 382, 43, 19]
>>> nums.count(99)
1
>>> nums.count(19)
2
>>> nums.count(23)
0
```

* 按位置翻转列表

```python
>>> prime = [2, 3, 5, 7, 11]
>>> id(prime)
140684818102088
>>> prime.reverse()
>>> prime
[11, 7, 5, 3, 2]
>>> id(prime)
140684818102088

>>> a = [1, 5.3, 321, 0, 1, 2]
>>> id(a)
140684818886024
>>> a = a[::-1]
>>> a
[2, 1, 0, 321, 5.3, 1]
>>> id(a)
140684818102664
```

* [len](https://docs.python.org/3/library/functions.html#len)函数获取列表的大小

```python
>>> prime
[2, 3, 5, 7, 11]
>>> len(prime)
5

>>> s = len(prime) // 2
>>> prime[:s]
[2, 3]
>>> prime[s:]
[5, 7, 11]
```

* 数值列表求和

```python
>>> a
[321, 5.3, 2, 1, 1, 0]
>>> sum(a)
330.3
```

* [all](https://docs.python.org/3/library/functions.html#all)和[any](https://docs.python.org/3/library/functions.html#any)函数

```python
>>> conditions = [True, False, True]
>>> all(conditions)
False
>>> any(conditions)
True

>>> conditions[1] = True
>>> all(conditions)
True

>>> a = [321, 5.3, 2, 1, 1, 0]
>>> all(a)
False
>>> any(a)
True
```

* 比较列表

```python
>>> prime
[2, 3, 5, 7, 11]
>>> a = [4, 2]
>>> prime == a
False

>>> prime == [2, 3, 5, 11, 7]
False
>>> prime == [2, 3, 5, 7, 11]
True
```

**进一步阅读**

* [Python文档 - 列表详情](https://docs.python.org/3/tutorial/datastructures.html#more-on-lists)
* [Python文档 - 集合](https://docs.python.org/3/library/collections.html)

<br>

### <a name="looping"></a>循环

```python
#!/usr/bin/python3

numbers = [2, 12, 3, 25, 624, 21, 5, 9, 12]
odd_numbers  = []
even_numbers = []

for num in numbers:
    odd_numbers.append(num) if(num % 2) else even_numbers.append(num)

print("numbers:      {}".format(numbers))
print("odd_numbers:  {}".format(odd_numbers))
print("even_numbers: {}".format(even_numbers))
```

* 基本上不需要元素索引也足以处理列表的每一个元素

```
$ ./list_looping.py
numbers:      [2, 12, 3, 25, 624, 21, 5, 9, 12]
odd_numbers:  [3, 25, 21, 5, 9]
even_numbers: [2, 12, 624, 12]
```

* 如果同时需要索引和元素，使用`enumerate()`函数

```python
#!/usr/bin/python3

north_dishes = ['Aloo tikki', 'Baati', 'Khichdi', 'Makki roti', 'Poha']

print("My favorite North Indian dishes:")
for idx, item in enumerate(north_dishes):
    print("{}. {}".format(idx + 1, item))
```

* 在这个例子中，我们获取了包含每一次迭代元素和索引（从0开始）的[元组](./Sequence_Set_Dict_data_types.md#tuples)
* [Python文档 - enumerate](https://docs.python.org/3/library/functions.html#enumerate)

```
$ ./list_looping_enumeration.py
My favorite North Indian dishes:
1. Aloo tikki
2. Baati
3. Khichdi
4. Makki roti
5. Poha
```

* 也可以指定一个start值

```python
>>> north_dishes = ['Aloo tikki', 'Baati', 'Khichdi', 'Makki roti', 'Poha']
>>> for idx, item in enumerate(north_dishes, start=1):
...     print(idx, item, sep='. ')
...
1. Aloo tikki
2. Baati
3. Khichdi
4. Makki roti
5. Poha
```

* 使用`zip()`同时迭代两者及以上的列表
* [Python文档 - zip](https://docs.python.org/3/library/functions.html#zip)

```python
>>> odd = [1, 3, 5]
>>> even = [2, 4, 6]
>>> for i, j in zip(odd, even):
...     print(i + j)
...
3
7
11
```

<br>

### <a name="list-comprehension"></a>列表推导式

```python
#!/usr/bin/python3

import time

numbers = list(range(1,100001))
fl_square_numbers = []

# 引用时间
t0 = time.perf_counter()

# ------------ for 循环 ------------
for num in numbers:
    fl_square_numbers.append(num * num)

# 引用时间
t1 = time.perf_counter()

# ------- 列表推导式 -------
lc_square_numbers = [num * num for num in numbers]

# 执行结果
t2 = time.perf_counter()
fl_time = t1 - t0
lc_time = t2 - t1
improvement = (fl_time - lc_time) / fl_time * 100

print("Time with for loop:           {:.4f}".format(fl_time))
print("Time with list comprehension: {:.4f}".format(lc_time))
print("Improvement:                  {:.2f}%".format(improvement))

if fl_square_numbers == lc_square_numbers:
    print("\nfl_square_numbers and lc_square_numbers are equivalent")
else:
    print("\nfl_square_numbers and lc_square_numbers are NOT equivalent")
```

* List comprehensions is a Pythonic way for some of the common looping constructs
* Usually is a more readable and time saving option than loops
* In this example, not having to call `append()` method also saves lot of time in case of list comprehension
* Time values in this example is indicative and not to be taken as absolute
    * It usually varies even between two runs, let alone different machines

```
$ ./list_comprehension.py
Time with for loop:           0.0142
Time with list comprehension: 0.0062
Improvement:                  56.36%

fl_square_numbers and lc_square_numbers are equivalent
```

* conditional list comprehension

```python
# using if-else conditional in list comprehension
numbers = [2, 12, 3, 25, 624, 21, 5, 9, 12]
odd_numbers  = []
even_numbers = []
[odd_numbers.append(num) if(num % 2) else even_numbers.append(num) for num in numbers]

# or a more simpler and readable approach
numbers = [2, 12, 3, 25, 624, 21, 5, 9, 12]
odd_numbers  = [num for num in numbers if num % 2]
even_numbers = [num for num in numbers if not num % 2]
```

* zip example

```python
>>> p = [1, 3, 5]
>>> q = [3, 214, 53]
>>> [i+j for i,j in zip(p, q)]
[4, 217, 58]
>>> [i*j for i,j in zip(p, q)]
[3, 642, 265]
```

use [generator expressions](https://docs.python.org/3/tutorial/classes.html#generator-expressions) if sequence needs to be passed onto another function

```python
>>> sum(i*j for i,j in zip(p, q))
910
```

**Further Reading**

For more examples, including nested loops, check these

* [Python docs - list comprehensions](https://docs.python.org/3/tutorial/datastructures.html#list-comprehensions)
* [Python List Comprehensions: Explained Visually](http://treyhunner.com/2015/12/python-list-comprehensions-now-in-color/)
* [are list comprehensions and functional functions faster than for loops](http://stackoverflow.com/questions/22108488/are-list-comprehensions-and-functional-functions-faster-than-for-loops)
* [Python docs - perf_counter](https://docs.python.org/3/library/time.html#time.perf_counter)
    * [understanding perf_counter and process_time](http://stackoverflow.com/questions/25785243/understanding-time-perf-counter-and-time-process-time)
* [Python docs - timeit](https://docs.python.org/3/library/timeit.html)

<br>

### <a name="list-user-input"></a>Getting List as user input

```python
>>> b = input('Enter strings separated by space: ').split()
Enter strings separated by space: foo bar baz
>>> b
['foo', 'bar', 'baz']

>>> nums = [int(n) for n in input('Enter numbers separated by space: ').split()]
Enter numbers separated by space: 1 23 5
>>> nums
[1, 23, 5]

>>> ip_str = input('Enter prime numbers separated by comma: ')
Enter prime numbers separated by comma: 3,5,7
>>> primes = [int(n) for n in ip_str.split(',')]
>>> primes
[3, 5, 7]
```

* Since user input is all treated as string, need to process based on agreed delimiter and required data type

<br>

### <a name="getting-random-items-from-list"></a>Getting random items from list

* Get a random item

```python
>>> import random
>>> a = [4, 5, 2, 76]
>>> random.choice(a)
76
>>> random.choice(a)
4
```

* Randomly re-arrange items of list

```python
>>> random.shuffle(a)
>>> a
[5, 2, 76, 4]
```

* Get random slice of list, doesn't modify the list variable

```python
>>> random.sample(a, k=3)
[76, 2, 5]

>>> random.sample(range(1000), k=5)
[68, 203, 15, 757, 580]
```

* Get random items from list without repetition by creating an iterable using [Python docs - iter](https://docs.python.org/3/library/functions.html#iter) function
* The difference from simply using shuffled list is that this avoids the need to maintain a separate index counter and automatic exception raised if it goes out of range

```python
>>> nums = [1, 3, 6, -12, 1.2, 3.14]
>>> random.shuffle(nums)
>>> nums_iter = iter(nums)
>>> print(next(nums_iter))
3.14
>>> print(next(nums_iter))
1.2
>>> for n in nums_iter:
...     print(n)
...
1
3
-12
6
>>> print(next(nums_iter))
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
StopIteration
```

* [Python docs - random](https://docs.python.org/3/library/random.html) for more info
  * new in version 3.6 - [random.choices](https://docs.python.org/3/library/random.html#random.choices)
* [Python docs - next](https://docs.python.org/3/library/functions.html#next)
* See also [yield](https://stackoverflow.com/questions/231767/what-is-the-function-of-the-yield-keyword)
