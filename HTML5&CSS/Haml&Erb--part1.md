## 以下是Haml[官方教程](http://haml.info/tutorial.html)的学习记录：

### ERB基础知识
erb是ruby的一种特殊用法，用于在各种文本文件中使用ruby。首先介绍erb文件里面看见的各种符号的意义：
```html
<%   %>   执行括号内的ruby代码
% code    与上面的一致,在haml中用的多
<%=  %>   打印括号内部的值/用值替换括号内部的内容
<%  -%>   在这个符号内的属于一行，不会收到换行的影响
<%#  %>   注释
```

#### 一个简单例子
```ruby
require 'erb'
x = 42
template = ERB.new <<EOF
  The value of x is: <%= x %>
EOF
puts template.result(binding)
```
**Prints: The value of x is: 42**





### Haml从第一个例子开始
```ERB
<strong><%= item.title %></strong>  #文字加粗的html格式
```
那么怎么用Haml转换呢？
```haml
%strong= item.title
```
我们可以发现&lt;strong&gt;&lt;/strong&gt;在**haml**里面被%strong所代替，表达方式更加简洁.。Haml可以自动检测返回值的新行和格式。

### 添加属性
```html
<strong class="code" id="message">Hello, World!</strong> #HTML
```
```haml
%strong{:class => "code", :id => "message"} Hello, World!  #Haml
```
通过对比我们可以发现：haml中使用大括号表达属性，使用符号格式表达属性，赋值符号变为使用双引号。渲染修饰的内容跟在大括号后面。此外，由于格式标记符**strong**后面没有等于号，即格式并非**strong=**，所以Hello, World！并非ruby代码而是字符串。
对于class和id这两个属性，有一种简写的方法：
```ruby
%strong.code#message Hello, World! 
```
在标记符的.后面的是class，#后面的是id， 在后面就是文本信息。