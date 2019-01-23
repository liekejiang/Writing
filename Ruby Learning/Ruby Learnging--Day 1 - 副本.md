# Ruby Learnging--Day 1
如何快速掌握一门新语言，参考孟岩的文章[快速掌握一个语言最常用的50%](https://blog.csdn.net/myan/article/details/3144661) 

首先从基本数据类型，语法和主要语言构造入手，具体即是数学运算符以及print函数的使用。 然后是数组和其他集合类的使用，包括字符串。本篇日志专注与此。

TIP： Ruby的“REPL”功能，即read, evaluation, print, loop。具体表现就是每执行一条指令，就显示相关结果。（输入IRB开启，Ctrl+D退出）

## 1.变量
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

## 2.字符串
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
### 3.循环，条件与块
##### 1.条件
1. if - elsif - else - end
2. ==, &&, || 
3. 

##### 2.循环
在ruby中，循环的使用方式有点不一样。首先是for循环，直接用数字或者数字变量加上方法的使用方式, 这一方式也被称为"do & end"。问题是如何在结束之前跳出循环呢？
```ruby
5.times do
  puts "Hello, World!"
end
```
##### 3.块
块是ruby中常用的概念，块是一组捆绑在其他地方使用的指令的方式。在上面的循环里面，其实已经展示了块的常见用法，以do开始，以end结束。
###### 支架块
当块仅包含单个指令时，经常使用备用标记{和}来标识块的开始和结束：
```ruby
5.times{ puts "Hello, World!" }
```
###### 块参数与传入块的参数
块可以用作传递给方法的参数，这是之前所学的语言里不曾见过的。
```ruby
irb(main):038:0> "this is a sentence".gsub("e"){ puts "Found an E!"}
Found an E!
Found an E!
Found an E!
=> "this is a sntnc"
irb(main):039:0>
```
这里也可以看出一个特点，当用字符串进行替代的时候，会融合进原字符串，而使用块则会单独显示。

块内部也可以使用传入的参数，同样使用#{}插入标记符指代传入的参数。
```ruby
5.times do |i|
  puts "#{i}: Hello, World!"
end
```

而.gsub在找到的字符串中传递。 尝试这个(用括号表示法)：
```ruby
irb(main):048:0> "this is a sentence".gsub("e"){|letter| letter.upcase}
=> "this is a sEntEncE"
2.4.0 :041 > b = "C"
 => "C" 
2.4.0 :042 > "this is a sentence".gsub("e"){|a| b.upcase}
 => "this is a sCntCncC" 
2.4.0 :043 > "this is a sentence".gsub("e"){|a| puts a}
e
e
e
 => "this is a sntnc" 
```
Ruby在上面结果中看到gsub正在使用块的结果作为原始匹配的替换。我开始糊涂了，.gsub中的给定参数可以被其后大括号中任意名词的局部变量指代，而其后可以进行其他操作。这里需要重点学习！

### 4.数组
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
表示数组是否包含该元素

更多信息参见[文档](http://www.ruby-doc.org/core-2.1.2/Array.html)

### 5.哈希


### 符号与方法

verb.method 可以查看当前变量对应的方法
verb.method.counts 当前变量对应的方法有多少种
verb.method.nil? 当前变量verb为nil时返回true

array.include()  若变量包含给定的元素，返回true



