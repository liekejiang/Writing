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

    