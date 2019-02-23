关于python中函数的基本知识不再赘述，这里只记录一些对我个人而言是盲区的知识点。

## 默认参数值
关于默认参数值，其可以使你定义的函数在使用时用比定义时更少的参数。默认值在函数的定义作用域被解析赋值。
```python
i = 5

def f(arg=i):
    print(arg)

i = 6
f() #输出5
```
需要注意的是默认值只被赋值一次。这使得当默认值是可变对象时会有所不同，比如列表、字典或者大多数类的实例。例如，下面的函数在后续调用过程中会累积（前面）传给它的参数:
```python
def f(a, L=[]):
    L.append(a)
    return L

print(f(1))
print(f(2))
print(f(3))
```
输出为
```python
[1]
[1, 2]
[1, 2, 3]
```
如果你不想让默认值在后续调用中累积，你可以像下面一样定义函数:
```python
def f(a, L=None):
    if L is None:
        L = []
    L.append(a)
    return L
```

## 关键字参数
函数可以通过**关键字参数**的方式来调用：
```python
def parrot(voltage, state='a stiff', action='voom', type='Norwegian Blue'):
    print("-- This parrot wouldn't", action, end=' ')
    print("if you put", voltage, "volts through it.")
    print("-- Lovely plumage, the", type)
    print("-- It's", state, "!")
```
调用函数时若使用关键字，那参数顺序不再重要，而且每个参数的意义更加明确：
```python
parrot(1000)                                          # 1 positional argument
parrot(voltage=1000)                                  # 1 keyword argument
parrot(voltage=1000000, action='VOOOOOM')             # 2 keyword arguments
parrot(action='VOOOOOM', voltage=1000000)             # 2 keyword arguments
parrot('a million', 'bereft of life', 'jump')         # 3 positional arguments
parrot('a thousand', state='pushing up the daisies')  # 1 positional, 1 keyword
```

## 可变参数列表
需要注意的是，关键字和无关键字参数混有可能引发问题，所以添加了在关键字参数之后不能使用无关键字参数的限制，而在关键字之前按照定义的顺序使用无参数关键字则是可以的。除此之外需要注意参数中的两个特殊情况： `*name` `**name`。
* `*name`, 代表函数参数列表中不与关键字对应的参数的集合，是一个tuple（元组）
* `**name`， 代表函数参数列表中不与关键字对应的有默认值的参数的集合，是个字典dict()

此外，`*name`必须出现在`**name`之前，这两个参数必须在定义的时候就声明, 而且使用了这两种可变参数之后，就不能再出现位置参数，而只能是关键字参数。

打包好的参数列表固然方便，但是还有一种情况:当你要传递的参数已经是一个列表，但要调用的函数却接受分开一个个的参数值。这时候你要把已有的列表拆开来。例如内建函数 range() 需要要独立的 start，stop 参数。你可以在调用函数时加一个 `*` 操作符来自动把参数列表拆开，或者使用两个 `**` 拆分为字典:
```python
## * 的用法
>>> list(range(3, 6))            # normal call with separate arguments
[3, 4, 5]
>>> args = [3, 6]
>>> list(range(*args))            # call with arguments unpacked from a list
[3, 4, 5]
# ** 的用法
>>> def parrot(voltage, state='a stiff', action='voom'):
...     print("-- This parrot wouldn't", action, end=' ')
...     print("if you put", voltage, "volts through it.", end=' ')
...     print("E's", state, "!")
...
>>> d = {"voltage": "four million", "state": "bleedin' demised", "action": "VOOM"}
>>> parrot(**d)
```
## Lambda 形式
`Lambda`是python中很重要的功能, 可以使用其创建**匿名函数**。给出一个`Lambda`匿名函数的例子：`lambda a, b: a+b`, 这个函数会返回两个参数之和。Lambda形式可以用于任何需要的函数对象。出于语法限制，它们只能有一个单独的表达式。语义上讲，它们只是普通函数定义中的一个语法技巧。类似于嵌套函数定义，lambda 形式可以从外部作用域引用变量:
```python
>>> def make_incrementor(n):
...     return lambda x: x + n
...
>>> f = make_incrementor(42)
>>> f(0)
42
>>> f(1)
43
```
上面的示例使用 lambda 表达式返回一个函数。`lambda`另一个用途是将一个小函数作为参数传递:
```py
>>> pairs = [(1, 'one'), (2, 'two'), (3, 'three'), (4, 'four')]
>>> pairs.sort(key=lambda pair: pair[1])
>>> pairs
[(4, 'four'), (1, 'one'), (3, 'three'), (2, 'two')]
```
## 文档字符串
这里介绍的文档字符串的概念和格式。

第一行应该是关于对象用途的简介。简短起见，不用明确的陈述对象名或类型，因为它们可以从别的途径了解到（除非这个名字碰巧就是描述这个函数操作的动词）。这一行应该以大写字母开头，以句号结尾。

如果文档字符串有多行，第二行应该空出来，与接下来的详细描述明确分隔。接下来的文档应该有一或多段描述对象的调用约定、边界效应等。

Python 的解释器不会从多行的文档字符串中去除缩进，所以必要的时候应当自己清除缩进。这符合通常的习惯。第一行之后的第一个非空行决定了整个文档的缩进格式。（我们不用第一行是因为它通常紧靠着起始的引号，缩进格式显示的不清楚。）留白“相当于”是字符串的起始缩进。每一行都不应该有缩进，如果有缩进的话，所有的留白都应该清除掉。留白的长度应当等于扩展制表符的宽度（通常是8个空格）。

以下是一个多行文档字符串的示例:
```python
>>> def my_function():
...     """Do nothing, but document it.
...
...     No, really, it doesn't do anything.
...     """
...     pass
...
>>> print(my_function.__doc__)
Do nothing, but document it.

    No, really, it doesn't do anything.
```
## 函数注解
函数注解 是关于用户自定义的函数的完全可选的、随意的元数据信息。无论 Python 本身或者标准库中都没有使用函数注解.

注解是以字典形式存储在函数的`__annotations__`属性中，对函数的其它部分没有任何影响。参数注解（Parameter annotations）是定义在函数参数名称的冒号后面，紧随着一个用来表示注解的值得表达式。如果参数有默认值，注解放在参数名和 = 号之间。返回注释（Return annotations）是定义在一个 `->` 后面，紧随着一个表达式，这个表达式可以是一个字符串或者是一个类的名词如`str`,`int`,注意这里的类名词没有引号，在冒号与 `->` 之间。下面的示例包含一个位置参数，一个关键字参数，和没有意义的返回值注释:
```python
>>> def f(ham: 42, eggs: int = 'spam') -> "Nothing to see here":
...     print("Annotations:", f.__annotations__)
...     print("Arguments:", ham, eggs)
...
>>> f('wonderful')
Annotations: {'eggs': <class 'int'>, 'return': 'Nothing to see here', 'ham': 42}
Arguments: wonderful spam
```