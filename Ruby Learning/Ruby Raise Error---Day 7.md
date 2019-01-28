# Ruby Raise Error---Day 7

异常用于处理各种类型的错误并且明确给出错误信息，并且让程序不至于因此错误而完全停止。
Ruby提供了一个很棒的处理异常的机制，我们可以使用**begin/end**块内附上可能抛出异常的代码，并且使用**rescue**子句告诉程序要处理的异常类型。

这里使用的begin/end块有所不同？在块里面有大写的**BEGIN/END**块，用法已经介绍过了。这里的**begin/end**更像循环，使用小写的**begin**表明一个循环块开始，在**end**处可以加上**if**,**until**等条件，或者不加，整个块只执行一次。

```ruby
begin 
    raise..  #正常运行代码
rescue [ExceptionType = StandardException] #捕获指定类型的异常，缺省值是StandardException。
    $! #表示异常信息
    $@ #表示异常出现的代码位置
else #其余异常
..
ensure #不管有没有异常，进入该代码块
end
```
从begin到rescue的一切是受保护的。若是异常发生，控制会传到resuce和end之间的块。每个rescue子句中，Ruby把抛出的异常与每个参数进行轮流比较匹配。

### 例1，打开文件
```ruby
begin
   file = open("/unexistant_file")
   if file
      puts "File opened successfully"
   end
rescue
      file = STDIN
end
print file, "==", STDIN, "\n"
```
如何打开失败，那么STDIN会代替file。其实也可以并不全用捕获异常。

## 使用**retry**语句
可以使用 rescue 块捕获异常，然后使用 retry 语句从开头开始执行 begin 块。
```ruby
begin
    # 运行代码，可能抛出异常
rescue
    #捕获异常后从此开始运行
    retry #重新从begin开始
end

#实例
begin 
    file = ope("/unexistant_file")
    if file
        puts "File opened successfully"
    end
rescue
    fname = "existant_file"
    retry
end
``` 
可以看作是一次自动的尝试，但是谨慎使用。

## 使用raise语句
可以使用 raise 语句抛出异常。下面的方法在调用时抛出异常。它的第二个消息将被输出。如果在**begin**和**resuce**直接遇到**raise**那么一定会抛出错误然后进入**rescue**。
```ruby
raise
#or
raise "Error Message"
#or
raise ExcepetionType, "Error Message"
#or
raise ExceptionType, "Error Message" condition
```
* 第一种方法简单地重新抛出当前异常(如果没有当前异常则抛出一个RuntimeError)。这用在传入异常之前需要解释异常的异常处理程序中。
* 第二种方法创建一个新的RuntimeError异常，设置其消息为给定的字符串。抛出异常到调用堆栈
* 第三种方法使用第一个参数创建一个异常，然后设置相关的消息为第二个参数。
* 第四种方法与第三种类似，可以添加条件语句来控制.
```ruby
begin
    puts "I am before the raise"
    raise "An error has occured"
    puts "I am after the raise"
rescue
    puts "I am rescued"
end
puts 'I am after the begin block'

## 这段看不懂
begin  
  raise 'A test exception.'  
rescue Exception => e  
  puts e.message  
  puts e.backtrace.inspect  
end
```

## 使用**ensure**语句
有时候，无论是否抛出异常，您需要保证一些处理在代码块结束时完成。例如，您可能在进入时打开了一个文件，当您退出块时，您需要确保关闭文件。

ensure 子句做的就是这个。ensure 放在最后一个 rescue 子句后，并包含一个块终止时总是执行的代码块。它与块是否正常退出、是否抛出并处理异常、是否因一个未捕获的异常而终止，这些都没关系，ensure 块始终都会运行
```ruby
begin
    raise 'A test exception'
rescue Exception => e
    puts e.message
    puts e.backtrace.inspect
ensure
    puts "Ensuring execution"
end
```

## 使用**else**
rescue的**else**子句一般放在任意ensure之前。**else**子句主体只有在代码主题没有抛出异常时执行。也就是没有**raise**的情况。

## **Catch** & **Throw**
**raise**和**rescue**的异常机制能在发生错误时放弃执行，有时候需要在正常处理时跳出一些深层嵌套的结构。此时**catch**和**throw**就拍上用场了。**catch**定义了一个使用给定的名词作为标签的块，块会正常执行直到遇到一个**throw**。
```ruby
def promptAndGet(prompt)
   print prompt
   res = readline.chomp
   throw :quitRequested if res == "!"
   return res
end
 
catch :quitRequested do
   name = promptAndGet("Name: ")
   age = promptAndGet("Age: ")
   sex = promptAndGet("Sex: ")
   # ..
   # 处理信息
end
promptAndGet("Name:")
```
上方代码会要求客户输入信息，如果客户输入“!” 那么就会跳出**catch**块。

### 类Exception
类顶层为 **Exception**, 其下有许多子类：
* Interrupt
* NoMemoryError
* SignalException
* ScriptError
* StandardError
* SystemExit

此时还不需要了解更多，最重要的是创建我们自己的异常类，其必须是Exception或其子类的子类。
```ruby
class FileSaveError < StandardError
    attr_reader :reason
    def initialized(reason)
        @reason = reason
    end
end

File.open(path, "w") do |file|
begin
    # 写出数据
rescue
    # 发生错误
    raise FileSaverError.new($!)
end
end
```
在这里，最重要的一行是 raise FileSaveError.new($!)。我们调用 raise 来示意异常已经发生，把它传给 FileSaveError 的一个新的实例，由于特定的异常引起数据写入失败。

明天来分析这些.



