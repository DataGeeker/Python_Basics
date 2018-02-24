# <a name="sequence-data-types"></a>序列、集合以及字典数据类型

* [字符串](#strings)
* [元组](#tuples)
* [集合](#set)
* [字典](#dictionary)

我们在前面章节已经看到过字符串、区间、列表等序列类型。元组是另外一中序列类型。这一章我们学习更多关于字符串、元组、字典和集合的操作。

<br>

### <a name="strings"></a>字符串

* 我们之前看到在列表中使用的索引也可以应用于字符串
    * 因为字符串不可变长，它们不能像列表一样修改

```python
>>> book = "Alchemist"
>>> book[0]
'A'
>>> book[3]
'h'
>>> book[-1]
't'
>>> book[10]
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
IndexError: string index out of range

>>> book[2:6]
'chem'
>>> book[:5]
'Alche'
>>> book[5:]
'mist'
>>> book[::-1]
'tsimehclA'
>>> book[:]
'Alchemist'

>>> list(book)
['A', 'l', 'c', 'h', 'e', 'm', 'i', 's', 't']

>>> import string
>>> string.ascii_lowercase[:10]
'abcdefghij'
>>> list(string.digits)
['0', '1', '2', '3', '4', '5', '6', '7', '8', '9']
```

* 字符串循环操作

```python
>>> book
'Alchemist'

>>> for char in book:
...     print(char)
...
A
l
c
h
e
m
i
s
t
```

* 其他操作

```python
>>> book
'Alchemist'

>>> len(book)
9

>>> book.index('A')
0
>>> book.index('t')
8

>>> 'A' in book
True
>>> 'B' in book
False
>>> 'z' not in book
True

>>> min('zealous')
'a'
>>> max('zealous')
'z'
```

<br>

### <a name="tuples"></a>元组

* 元素跟列表相似，但是不可变长，在其他场景中很有用
* 单一的元素可变长/不变长

```python
>>> north_dishes = ('Aloo tikki', 'Baati', 'Khichdi', 'Makki roti', 'Poha')
>>> north_dishes
('Aloo tikki', 'Baati', 'Khichdi', 'Makki roti', 'Poha')

>>> north_dishes[0]
'Aloo tikki'
>>> north_dishes[-1]
'Poha'
>>> north_dishes[6]
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
IndexError: tuple index out of range

>>> north_dishes[::-1]
('Poha', 'Makki roti', 'Khichdi', 'Baati', 'Aloo tikki')

>>> north_dishes[0] = 'Poori'
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: 'tuple' object does not support item assignment
```

* 示例操作

```python
>>> 'roti' in north_dishes
False
>>> 'Makki roti' in north_dishes
True

>>> len(north_dishes)
5

>>> min(north_dishes)
'Aloo tikki'
>>> max(north_dishes)
'Poha'

>>> for dish in north_dishes:
...     print(dish)
...
Aloo tikki
Baati
Khichdi
Makki roti
Poha
```

* 元组用于处理多个变量分配（赋值）和在函数中返回多个值
    * 在使用`enumerate`用于迭代列表时我们已经看过例子了

```python
>>> a = 5
>>> b = 20
>>> a, b = b, a
>>> a
20
>>> b
5

>>> c = 'foo'
>>> a, b, c = c, a, b
>>> a
'foo'
>>> b
20
>>> c
5

>>> def min_max(arr):
...     return min(arr), max(arr)
...
>>> min_max([23, 53, 1, -34, 9])
(-34, 53)
```

* 并不总需要使用`()`

```python
>>> words = 'day', 'night'
>>> words
('day', 'night')

>>> coordinates = ((1,2), (4,3), (92,3))
>>> coordinates
((1, 2), (4, 3), (92, 3))

>>> prime = [2, 3, 5, 7, 11]
>>> prime_tuple = tuple((idx + 1, num) for idx, num in enumerate(prime))
>>> prime_tuple
((1, 2), (2, 3), (3, 5), (4, 7), (5, 11))
```

* 将其他类型转换为元组
    * 与`list()`相似

```
>>> tuple('books')
('b', 'o', 'o', 'k', 's')

>>> a = [321, 899.232, 5.3, 2, 1, -1]
>>> tuple(a)
(321, 899.232, 5.3, 2, 1, -1)
```

* 数据类型可用多种方式混合和匹配

```python
>>> a = [(1,2), ['a', 'b'], ('good', 'bad')]
>>> a
[(1, 2), ['a', 'b'], ('good', 'bad')]

>>> b = ((1,2), ['a', 'b'], ('good', 'bad'))
>>> b
((1, 2), ['a', 'b'], ('good', 'bad'))
```

* [Python文档 - 元组](https://docs.python.org/3/library/stdtypes.html#tuple)
* [Python文档 - 元组教程](https://docs.python.org/3/tutorial/datastructures.html#tuples-and-sequences)

<br>

### <a name="set"></a>集合

* 集合是无序的对象集
* 可变数据类型
* 通常用于保持唯一的序列、执行集合操作（像交、并、差等等）

```python
>>> nums = {3, 2, 5, 7, 1, 6.3}
>>> nums
{1, 2, 3, 5, 6.3, 7}

>>> primes = {3, 2, 11, 3, 5, 13, 2}
>>> primes
{2, 3, 11, 13, 5}

>>> nums.union(primes)
{1, 2, 3, 5, 6.3, 7, 11, 13}

>>> primes.difference(nums)
{11, 13}
>>> nums.difference(primes)
{1, 6.3, 7}
```

* 示例操作

```python
>>> len(nums)
6

>>> nums[0]
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: 'set' object does not support indexing

>>> book
'Alchemist'
>>> set(book)
{'i', 'l', 's', 'A', 'e', 'h', 'm', 't', 'c'}
>>> set([1, 5, 3, 1, 9])
{1, 9, 3, 5}
>>> list(set([1, 5, 3, 1, 9]))
[1, 9, 3, 5]

>>> nums = {1, 2, 3, 5, 6.3, 7}
>>> nums
{1, 2, 3, 5, 6.3, 7}
>>> nums.pop()
1
>>> nums
{2, 3, 5, 6.3, 7}

>>> nums.add(1)
>>> nums
{1, 2, 3, 5, 6.3, 7}

>>> 6.3 in nums
True

>>> for n in nums:
...     print(n)
...
1
2
3
5
6.3
7
```

* [Python文档 - 集合](https://docs.python.org/3/library/stdtypes.html#set)
* [Python文档 - frozenset](https://docs.python.org/3/library/stdtypes.html#frozenset)
* [集合教程](http://www.programiz.com/python-programming/set)

<br>

### <a name="dictionary"></a>字典

* 字典类型被看作无序的键值对或有名字的元素列表

```python
>>> marks = {'Rahul' : 86, 'Ravi' : 92, 'Rohit' : 75}
>>> marks
{'Ravi': 92, 'Rohit': 75, 'Rahul': 86}

>>> fav_books = {}
>>> fav_books['fantasy']   = 'Harry Potter'
>>> fav_books['detective'] = 'Sherlock Holmes'
>>> fav_books['thriller']  = 'The Da Vinci Code'
>>> fav_books
{'thriller': 'The Da Vinci Code', 'fantasy': 'Harry Potter', 'detective': 'Sherlock Holmes'}

>>> marks.keys()
dict_keys(['Ravi', 'Rohit', 'Rahul'])

>>> fav_books.values()
dict_values(['The Da Vinci Code', 'Harry Potter', 'Sherlock Holmes'])
```

* 循环和打印

```python
>>> for book in fav_books.values():
...     print(book)
...
The Da Vinci Code
Harry Potter
Sherlock Holmes

>>> for name, mark in marks.items():
...     print(name, mark, sep=': ')
...
Ravi: 92
Rohit: 75
Rahul: 86

>>> import pprint
>>> pp = pprint.PrettyPrinter(indent=4)
>>> pp.pprint(fav_books)
{   'detective': 'Sherlock Holmes',
    'fantasy': 'Harry Potter',
    'thriller': 'The Da Vinci Code'}
```

* 修改字典和示例操作

```python
>>> marks
{'Ravi': 92, 'Rohit': 75, 'Rahul': 86}
>>> marks['Rajan'] = 79
>>> marks
{'Ravi': 92, 'Rohit': 75, 'Rahul': 86, 'Rajan': 79}

>>> del marks['Ravi']
>>> marks
{'Rohit': 75, 'Rahul': 86, 'Rajan': 79}

>>> len(marks)
3

>>> fav_books
{'thriller': 'The Da Vinci Code', 'fantasy': 'Harry Potter', 'detective': 'Sherlock Holmes'}
>>> "fantasy" in fav_books
True
>>> "satire" in fav_books
False
```

* 字典由列表组成并使用随机模块
* 任何对单个列表的改变会反映在字典中
* `keys()`方法的输出必须改变为像`list`或者`tuple`这样的序列类型传入`random.choice`

```python
>>> north = ['aloo tikki', 'baati', 'khichdi', 'makki roti', 'poha']
>>> south = ['appam', 'bisibele bath', 'dosa', 'koottu', 'sevai']
>>> west = ['dhokla', 'khakhra', 'modak', 'shiro', 'vada pav']
>>> east = ['hando guri', 'litti', 'momo', 'rosgulla', 'shondesh']
>>> dishes = {'North': north, 'South': south, 'West': west, 'East': east}

>>> rand_zone = random.choice(tuple(dishes.keys()))
>>> rand_dish = random.choice(dishes[rand_zone])
>>> print("Try the '{}' speciality '{}' today".format(rand_zone, rand_dish))
Try the 'East' speciality 'rosgulla' today
```

**进一步阅读**

* [Python文档 - 字典](https://docs.python.org/3/library/stdtypes.html#dict)
* [Python文档 - pprint](https://docs.python.org/3/library/pprint.html)
* [字典详细教程](http://www.sharats.me/posts/the-python-dictionary/)
