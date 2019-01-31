# Ruby block--Day 4
虽然之前在day3已经简单地介绍过ruby中块（block）的用法，但是不够详细，今天来正式介绍一下块的用法。

Ruby代码块在其他编程语言中被称为闭包。 它由一组代码组成，它们始终用大括号括起来，或者在do..end之间书写。 大括号语法总是具有比do..end语法更高的优先级。也就是说大括号优先级高，do..end优先级低。

### 块的基本形式
Ruby块可用两种方式来编写 
* -do和end之间的多行(多行块不是内联的)
* 大括号{}之间的内嵌

"||"内的局部变量代表了块的参数。
```ruby
### 多行程序段
[10,20,30].each do |n|
    puts n
end
### 内联块(支架块)
[10,20,30].each {|n| puts n}
```
### yield语句
**yield**语句用于调用具有值的方法中的块。乍一看可能觉得有点迷糊，关注以下代码：
```ruby
def met   
   puts "This is method"   
   yield if block_given?
   puts "You will be back to method"   
   yield   
end   
met {puts "This is block"}
```
我们定义met方法并将一个内联块作为它的参数传入，那么这个块什么时候会被执行呢？ 答案是遇到**yield**就会执行一次。可以使用**yield**构造迭代器，使用各种条件筛选我们想要的数据，比如each和every_nth。 我们可以使用yield传递一个或者多个参数，若**yield**执行，这些参数会被传入块。一般情况下，yield必须有对应的代码块传入，否则会报错；但是有一种灵活应用的方法，使用block_given? 与if配合，在有block传入的时候才会调用。注意这里的**if**使用方法。

```ruby
def met   
   yield 1   
   puts "This is method"   
   yield 2   
end   
met {|i| puts "This is block #{i}"}
```
仔细观察上面程序，发现met后面的内联块中使用了|i|作为块的形参，而方法中**yield**后面的数字就是传给|i|的变量。**yield**也可以带有参数，**yield**的参数就是块的参数，




### 块变量
可以在块的内外使用相同的变量：
```ruby
x = "Outer variable"    
3.times do |x|    
    puts "Inside the block: #{x}"    
end    
puts "Outside the block: #{x}"
```
在这种情况下，相同变量相同的仅仅是名字而已，在块内部，该变量的值会被形参变量所覆盖。

### BEGIN & END 块
**BEGIN**和**END**块用于声明该文件正在加载，以及结束。一个**BEGIN**对应一个**END**块。它们的运行顺序是这样的：
先执行所有的**BEGIN**块，按照出现的先后顺序执行；然后按正常顺序执行剩下所有的普通命令；最后按照自底向上执行所有的**END**语句，过程很像栈的顺序，后入栈的**BEGIN**其对应的**END**先执行。
```ruby
BEGIN { 
  # BEGIN 代码块
  puts "1 start"
} 
 
BEGIN { 
  # BEGIN 代码块
  puts "2 start"
} 
 
END { 
  # END 代码块
  puts "1 end"
}
  # MAIN 代码块
puts "1 go on"

END { 
  # END 代码块
  puts "2 end"
}
  # MAIN 代码块
puts "2 go on"
```
其结果是：
```ruby
1 start
2 start
1 go on
2 go on
2 end
1 end
```

### 符号参数(&块)
&块是一种将参考（而非局部变量）传递给块的方法，块之后的**&**只是一个引用的名字，任何其他的名字都可以用它来代替**this**。参考或者说引用，与C++的&十分相似。引用块使用名为**.call**的方法调用。
```ruby
def met(&block)   
  puts "This is method"   
  block.call   
end   
met { puts "This is &block example" }
```
显示结果为
```ruby
This is method
This is &block example
```
这里**&block**就是块的引用，也是**Proc**对象，使用call来执行，与**yield**类似。那么和yield有什么区别呢，下一节会回答这个问题。



### 块如何转换为**Proc**对象
第一个问题，什么是**Proc**对象：在回答这个问题之前，我们先思考一下我们之前提到过的块的使用方式，一般都是在一个对象的方法或者定义的方法中被调用，而且是要使用的时候才调用，那么可不可以事先把块准备好，到时候再调用呢？
这，就是为什么要引入**Proc**对象，将块转化为一个对象保存下来以待后用。
```ruby
inc = Proc.new{|x| x+1}
inc.call
```
网上有说法**Proc**类与匿名函数很像，~~不会匿名函数，以后补充~~。绝大多数情况下,yield就可以满足需求，但是以下两种情况适合用**Proc**：
* 被调用方法A调用另一个方法B，而块在B中才被使用。
* 使用Proc的call方法调用块而不是**yield**。

基于这两种情况，我们需要一个“块的引用”，使用**&**实现。Ruby允许任何对象以“块”的形式传递给方法，而**Proc**对象是“块”的引用，因为 Ruby 方法中最后一个参数有 & 参数时，其必须是 Proc 对象，如果不是，则会调用这个对象的to_proc方法把其转换为 Proc 对象，也就是块的引用。
```ruby
p = Proc.new{ puts "Hello!" }
def my_method(block)
  block.call
end
my_method(p) 
```
显示结果为
```ruby
Hello!
```
这里如果我们把**&**加上，则会报错，错误信息是给的参数超过了预期，为什么呢？难道说Proc.new就已经完成了**&**的功能了吗


### EX章节：.map(&:some_method) 的工作原理


