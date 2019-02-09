# Ruby Class(1)

## Class
类的定义不在赘述，类的命名规则以驼峰命名法，以大写字母开头，没有分隔符。

### 定义类的对象
使用.new方法

### initizalize方法，类变量和实例变量
当我们想在定义一类的对象的时候初始化一些类变量或实例变量，我们可以调用**initizalize**方法，这是一个类的构造函数，与其他语言中的constructor相似。需要注意的是类变量和实例变量的区别，类变量是属于该类的公共变量，同一类的不同实例对象共享；而实例变量同一类的实例不共享，其在创建实例对象时就变成对象的属性。
对于实例变量，在类内部用@表明，而在类的外部，需要使用访问器(getter，公共方法)来访问。
```ruby
class Box
    def initialize(w,h)
        @width, @height = w, h
    end
end
```
### 访问器和设置器(getter & setter), 实例方法
为了在类的外部读取类中已经定义的变量，我们可以通过定义访问器(getter)来访问。
```ruby
class Box
    #构造函数
    def initialize(w,h)
        @width, @height = w, h
    end
    
    #访问器方法
    def printWidth
        @width
    end
    def printHeight
        @height
    end
        
    #设置器方法
    def setWidth(value)
        @width = value
    end
    def setHeight(value)
        @height = value
    end
    
    #实例方法
    def getArea
        @width * @height
    end
end
# initialization
box = Box.nex(10,20)

#使用设置器方法
box.setWidth = 30
box.setHeight = 50

# 使用访问器,获得盒子的宽和高
x = box.printWidth()
y = box.printHeight()

```
实例变量不能直接访问，获得实例变量需要专门定义的访问器函数。与访问器类似，想要改变实例变量需要定义专门的设置器(setter)。而实例方法只能通过类实例来使用，对实例变量进行修改。

为了减少重复工作，践行TRY，Ruby定义了以下三个属性声明方法，只对实例变量有用:
* attr_accessor : variable_name,
* attt_reader : variable_name,
* attr_writer : variable_name,

### 类方法，类变量
类变量是在类的所以实例中共享的变量，可以被所有的对象实例访问。 类变量以两个@@为前缀，**其必须在类定义中被初始化**，也可以通过initialize来修改。 类方法使用def self.methodname()定义，可使用带有类名词的classname.methodname 形式调用。
```ruby
class Box
    @@count = 0
    def initialize(w,h)
        @width, @height = w, h
        @@count += 1
    end
    
    def self.printCount()
        puts "Box count is : #@@count"
    end
end

box1 = Box.new(10,2)

Box.printCount()
```

### to_s 方法
任何定义的类都自带有一个**to_s**实例方法来返回对象的字符串形式。下面是详细定义。
```ruby
class Box
   # 构造器方法
   def initialize(w,h)
      @width, @height = w, h
   end
   # 定义 to_s 方法
   def to_s
      "(w:#@width,h:#@height)"  # 对象的字符串格式
   end
end

# 创建对象
box = Box.new(10, 20)
 
# 自动调用 to_s 方法
puts "String representation of box is : #{box}"
```

### 访问控制
Ruby中提供了三种不同的实例方法保护，分别是**pubilc**, **private**,**protected**。Ruby不在实例和类变量上应用任何访问控制。也就是说类变量和实例变量都是**public**的。

* **public**方法 ：**public**可以被任意对象调用。默认为**public**，而**initialize**总是**private**的。
* **private**方法 ：**private**不能从类外部访问或者查看。只有类方法可以访问私有成员。
* **protected**方法 : **protected**只能被类和子类的对象调用。访问也只能在其子类内部进行。

