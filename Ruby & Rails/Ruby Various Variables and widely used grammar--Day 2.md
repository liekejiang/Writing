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
