## Ruby Control Stream
今天主要学习ruby的几种主要控制流/条件及循环语句的使用

##### 1.条件判断语句
1. if - elsif - else - end 注意缩进，括号非必需。
2. ==, &&, || 
3. 三元控制符“？ ：”，与c和c++里的类似， （5<2? 1:2）输出2
4. case语句，代替switch。例子：
```ruby
case expression(usually variable)
    when "condition expression"
        code
    when "other condition expression"
        code
    else
        code
end
```

##### 2.循环
在ruby中，循环的使用方式有点不一样。首先是for循环，直接用数字或者数字变量加上方法的使用方式, 这一方式也被称为"do & end"。问题是如何在结束之前跳出循环呢？
1. For循环
表达式为：
```ruby
for variables in range/array do
    code
end
```
需要注意的是range的表达式:
*  x .. y 包含首尾
*  x ... y 不包含首尾

2. Do & end
```ruby
5.times do
  puts "Hello, World!"
end
```
以上这两种一般在循环次数固定情况下使用，如果不固定则一般使用while
3. While
```ruby
while expression dp
    code
end
```
4. loop do... while 循环
do while循环的语法与一般的语言不同：
```ruby
loop do
    code
    code
    break if expression
end
```
但是又与其他语法的do while有相似之处，loop内的代码会被至少执行一次。

5. until 循环
与while循环相反，until循环会一直持续直到条件为 **ture**， 而while里面会循环到条件为**false**。
```ruby
until experssion
    code
end
```
6. break & next & redo & retry
这四个是ruby循环中常用的辅助语句， break和其他语言一致，而next等于continue。重点介绍redo与retry， redo一旦被执行，则循环立马从头开始再执行一次，所以要注意判断redo的条件，否则将会陷入无限循环。
与redo不同的是，retry会从条件的一开始，也就是从头开始再执行一次循环，因此如果不设置好退出条件，很容易死循环。一般通过设置标志位来避免这种情况。


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

##### 类与对象
ruby中所有的类都继承自一个父类BasicObject，也允许创建替代对象层次结构。而ruby对象代表的是类的实例。

###### 创建对象
所有的对象都可以通过调用类的内建方法 **new** 来创建。
```ruby
objectName = className.new
```
###### 定义方法
方法名应始终以小写字母开头，使用下划线分割单词，否则可能会被误解为常数。方法的形参可以设定可被覆盖的默认值。
```ruby
def method_name
    code
end
```
关于返回值： 默认情况下，ruby会返回最后一个语句的值. 而return用于从方法返回一个或多个值。

* 可变参数数：一个方法可以声明使用可变数量的参数，在方法形参之前使用"*"即可。
* 类方法：当方法在类外定义时，默认为**private**，与之对应的类内方法为 **public**。ruby中不用实例化一个类就能访问其类方法






