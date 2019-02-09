# Ruby Learnging--Day 1
如何快速掌握一门新语言，参考孟岩的文章[快速掌握一个语言最常用的50%](https://blog.csdn.net/myan/article/details/3144661) 

Version 1.0：首先从基本数据类型，语法和主要语言构造入手，具体即是数学运算符以及print函数的使用。 然后是数组和其他集合类的使用，包括字符串。本篇日志专注与此。

Version 1.1 ：为了方便查阅，该笔记主要用于记录Ruby中主要数据类型的使用：
1. 数字
2. 字符串
3. 数组
4. 符号
5. 常量
6. 布尔
7. 哈希

TIP： Ruby的“REPL”功能，即read, evaluation, print, loop。具体表现就是每执行一条指令，就显示相关结果。（输入irb开启，Ctrl+D退出）

## 1.变量与Ruby主要命名规则
****
变量的初始化和赋值在Ruby被合并了，这点与python相似。变量可以通过赋予不同的值改变变量的类型。
变量中需要注意的是命名规则与约定俗成的风格。Ruby变量
* 通常以小写字母开头
* 不能使用空格
* 不能包含特殊字符，如$ @ &

而命名风格主要是：
* 使用小写+下划线的命名风格，而不是驼峰规则
* 使用具体的含义命名
* 尽量不使用缩写

在ruby中我们对不同的实体采用不同的命名规则，对于普通变量的通用命名规则我们已经给出，接下来介绍其他对象的命名规则：
1. 类对象使用驼峰命名：ClassName use UpperCamelCase
2. 方法使用和变量相同的蛇形命名，使用下划线分割
3. 常量使用大写蛇形命名方式，且全局变量之前要加“$”符号， CONSTANTS(scoped) & $GLOBALS(not scoped)， TEST_MODE = true
4.  符号之前要加":"， fravorite_framework = :rails


## 2.字符串(数字不介绍)
****
Ruby和python相似，使用双引号“”来表示字符串，最短的字符串为空串“”
### 1. 截取字符串
使用下标即可，
```ruby
irb(main):017:0> string = "Ruby in 30 Minutes at yiibai.com"
=> "Ruby in 30 Minutes at yiibai.com"
irb(main):018:0> string[0..8]
=> "Ruby in 3"
irb(main):019:0> string[8..16]
=> "30 Minute"
irb(main):020:0> string[8..-1]
=> "30 Minutes at yiibai.com"
irb(main):021:0> string[8..-2]
=> "30 Minutes at yiibai.co"
irb(main):022:0>
```
在上面的例子里面我们需要注意几个点，下标提取使用中有两个格式，一个如同上面的代码使用两个点，这种情况下首位下标都会被提取；而传统的使用逗号时，尾部下标则不会被提取出来。-1 这里指代最后一位，与其他语言相似，-2则是倒数第二位。

### 2. 常用字符串方法
由于字符串这一类有很多内建函数/方法，所以字符串类变量可以直接使用，这里由于不带括号所以使用“方法”一词更多。
##### .length
求出字符串实体长度，包括空格
```ruby
irb(main):022:0> string = "0123456789"
=> "0123456789"
irb(main):023:0> string.length
=> 10
irb(main):024:0> string = "maxsu"
=> "maxsu"
irb(main):025:0> string.length
=> 5
irb(main):026:0>
```
##### .split
分割字符串，默认分割符为“ ”空格，也可以自己设定分割符参数. 被选中的分割符会被删除。
```ruby
2.4.0 :006 > string = "I am SZY"
 => "I am SZY" 
2.4.0 :007 > string.split
 => ["I", "am", "SZY"] 
 2.4.0 :018 > string.split("I")
 => ["", " am SZY"] 
 ```
##### .sub与.gsub
用于替换字符串中的部分字符，不同的是前者只替换第一次，而后者是全局替换

##### 字符串连接
1. 使用加号“+” 直接接连。 但是有限制，能连接字符串变量与字符串变量或者字符串，但是不能两个字符串直接想连接。
```ruby
irb(main):021:0> name = "Maxsu"
=> "Maxsu"
irb(main):022:0> puts "Good morning, " + name + " ! "
Good morning, Maxsu !
````
2. 使用 #{} 插值标记符
仅适用于双引号字符串内，使用#{string}，其中string为字符串变量。 除此之外，插值标记符内还可以使用运算符
```ruby
irb(main):025:0> modifier = "very "
=> "very "
irb(main):026:0> mood = "excited"
=> "excited"
irb(main):028:0> puts "I am #{modifier * 3 + mood} for today's play!"
I am very very very excited for today's play!
=> nil
irb(main):029:0>
```


### 3.数组
##### 1.数组的初始化
首先关注数组的初始化方式，我们一共有三种初始化方法：
```ruby
irb(main):049:0> meals = ["Breakfast", "Lunch", "Dinner"]
=> ["Breakfast", "Lunch", "Dinner"]
2.4.0 :048 > arr = []
 => [] 
```
我们既可以直接初始化一个数组实体，又可以通过数组方法的方式构建一个空数组。在初始化数组的时候也可以使用**块**，使其初始化一个以String，Hash或者其他数据类型为实例的数组，同样而可以使用这个方法初始化多维数组。最后还可以用数组函数直接初始化
```ruby
ary = Array.new    #=> []
Array.new(3)       #=> [nil, nil, nil]
Array.new(3, true) #=> [true, true, true]
Array.new(4){String.new}  #=> ["", "", "", ""] 
empty_table = Array.new(3) { Array.new(3) } 
#=> [[nil, nil, nil], [nil, nil, nil], [nil, nil, nil]]
Array({:a => "a", :b => "b"}) #=> [[:a, "a"], [:b, "b"]]
```
##### 2.数组的增减
接下来关注与数组相关的操作，首先是数组的增减
```ruby
irb(main):049:0> meals = ["Breakfast", "Lunch", "Dinner"]
=> ["Breakfast", "Lunch", "Dinner"]
##增加元素
2.4.0 :057 > meals << "egg"
 => ["Breakfast", "Lunch", "Dinner", "egg"] 
##删除元素
 2.4.0 :065 > meals.delete("egg")
 => "egg" 
2.4.0 :066 > meals
 => ["Breakfast", "Lunch", "Dinner"] 
##求差，并不会改变数组
 2.4.0 :059 > meals -[ "egg"]
 => ["Breakfast", "Lunch", "Dinner"] 
```
##### 3.其他技巧
之后会开一个专题讲数组的应用，这里就先介绍部分
###### .sort 排序
字符串用字母排序，数字用升序排序
```ruby
irb(main):056:0> array1 = ["this", "is", "an", "array"]
=> ["this", "is", "an", "array"]
irb(main):057:0> array1.sort
=> ["an", "array", "is", "this"]
irb(main):058:0> array1
=> ["this", "is", "an", "array"]
irb(main):059:0>
```
###### .each
遍历数组中的每个元素, 是一个枚举（Enumerator）实例。
```ruby
2.4.0 :067 > meals.each
 => #<Enumerator: ["Breakfast", "Lunch", "Dinner"]:each> 
 ```
###### .join
将数组中的元素进行组合
```ruby
2.4.0 :077 > meals.join
 => "BreakfastLunchDinner" 
```
###### .first .last. 
数组传统的下标提取。
###### .index
输入元素，返回该元素在数组中的位置或者返回“nil”表示不存在
###### .include?
array.include()  若变量包含给定的元素，返回true。表示数组是否包含该元素

更多信息参见[文档](http://www.ruby-doc.org/core-2.1.2/Array.html)

### 4.哈希
哈希将一个键值"key"与一个值"value"相匹配，不能有相同键但是可以有相同值。字符串和符号可以作为键。或者你可以采用初始化一个空hash变量的方式，如果有需求的话。如果hash变量没能找到键对应的值，会返回默认值nil，你也可以自己设定默认值，通过Hash.new(value)或hash.default = value的方式。
```ruby
2.4.0 :009 > b = {"key1" => "value1", :key2 => "Chemistry", ":key3" => "Maths"}         
 => {"key1"=>"value1", :key2=>"Chemistry", ":key3"=>"Maths"} 

2.4.0 :004 > grades = Hash.new
 => {} 
 
2.4.0 :005 > grades = Hash.new(0)
 => {} 
 
2.4.0 :006 > grades.default = 0
 => 0 
```
在hash变量中增加新键值对很简单，可以直接在hash变量后赋予新值，或者使用.store的方法，注意不要覆盖。
```ruby
2.4.0 :009 > b["key5"] = 1                                                          
 => 1 

2.4.0 :002 > b.store("key4",123)
 => 123 
```

可以通过匹配hash变量中的键来获取对应的值。
```ruby
2.4.0 :007 > b["key1"]
 => "value1" 
```
常用哈希方法：
```ruby
2.4.0 :017 > b.keys
 => ["key1", :key2, ":key3"] 
 
2.4.0 :018 > b.values
 => ["value1", "Chemistry", "Maths"] 
 
 
```

在键全是符号的情况下，ruby提供了一种简写的方式：
```ruby
2.4.0 :003 > b = {key1:1, key2:2}
 => {:key1=>1, :key2=>2} 
```
### 符号
符号一般以冒号“：”开头，本身可以看作是一个等于自身的常数字符串。多个单词直接可以使用下划线“_”隔开。
```ruby
2.4.0 :003 > :syb
 => :syb 
```




