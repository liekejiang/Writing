# Ruby Learnging--Day 2
如何快速掌握一门新语言，参考孟岩的文章[快速掌握一个语言最常用的50%](https://blog.csdn.net/myan/article/details/3144661) 

Day 1 ：[基本数据类型（数组和其他集合类的使用，包括字符串），语法和部分主要语言构造](https://github.com/liekejiang/Writing/blob/master/Ruby%20Learning/Ruby%20Learnging--Day%201.md)

本篇主要关于ruby的几种常用变量类型和常用语法的使用。

TIP： Ruby的“REPL”功能，即read, evaluation, print, loop。具体表现就是每执行一条指令，就显示相关结果。（输入IRB开启，Ctrl+D退出）
### 1.局部变量
局部变量名以小写字母或下划线(_)开头。变量可在它的初始化块内或范围内访问。代码块完成后，变量就不在作用域存在了。当未初始化的局部变量被调用时，它们被解释为对没有参数的方法的调用。

### 2.类变量
类变量名以@@符号开头。需要在使用前进行初始化。 类变量属于整个类，可以从类中的任何位置访问。 如果在一个实例中该值将被更改，则它将在每个实例中被改变。类变量由类的所有后代共享，未初始化的类变量将导致错误。

示例：创建一个Ruby文件：class-variables.rb，编写以下代码 - #!/usr/bin/ruby   
```ruby
class States   
   @@no_of_states=0   
   def initialize(name)   
      @states_name=name   
      @@no_of_states += 1   
   end   
   def display()   
     puts "State name #@states_name"   
    end   
    def total_no_of_states()   
       puts "Total number of states written: #@@no_of_states"   
    end   
end   

# Create Objects   
first=States.new("Assam")   
second=States.new("Meghalaya")   
third=States.new("Maharashtra")   
fourth=States.new("Pondicherry")   

# Call Methods   
first.total_no_of_states()   
second.total_no_of_states()   
third.total_no_of_states()   
fourth.total_no_of_states()
```
Ruby在上面的例子中，@@no_of_states是一个类变量。执行上面代码，输出结果如下：

```ruby
F:\worksp\ruby>ruby class-variables.rb
Total number of states written: 4
Total number of states written: 4
Total number of states written: 4
Total number of states written: 4
```
观察之后有如下几个疑问，为什么没有定义new，就可以使用？ 类变量在引用的时候格式为""#@@..."
使用单个@的是~~块变量或者局部变量~~实例变量，在类外应该不能调用。

### 2.实例变量
实例变量名以“@”开头，上面代码里面的@state_name就属于实例变量。 它属于类的一个实例，可以从方法中的类的任何实例访问。 它们只能访问一个特定的类的实例。它们不需要初始化，未初始化的实例变量的值是：nil 

### 3.全局变量
全局变量名以$号开头。在全局范围内可访问，可以从程序中的任何位置访问它。未初始化的全局变量的值会被初始化为：nil。建议不要使用全局变量，因为它们使程序变得秘密和复杂。Ruby中有一些预定义的全局变量
```ruby
$global_var = "GLOBAL"   
class One   
  def display   
     puts "Global variable in One is #$global_var"   
  end   
end   
class Two   
  def display   
     puts "Global variable in Two is #$global_var"   
  end   
end   

oneobj = One.new   
oneobj.display   
twoobj = Two.new   
twoobj.display
```

执行上面代码，输出结果如下：
```ruby
F:\worksp\ruby>ruby global-variables.rb
Global variable in One is GLOBAL
Global variable in Two is GLOBALs
```

### 5.循环，条件与块
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



