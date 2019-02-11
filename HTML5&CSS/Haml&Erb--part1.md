## 以下是Haml[官方教程](http://haml.info/tutorial.html)的学习记录：

### 基础知识
首先介绍erb文件里面看见的各种符号的意义：
```html
<%   %>   执行括号内的ruby代码
<%=  %>   打印？
<%  -%>   在这个符号内的属于一行，不会收到换行的影响
<%#  %>   
```






### 从第一个例子开始
```ERB
<strong><%= item.title %></strong>  #文字加粗
```
那么怎么用Haml转换呢？
```haml
%strong= item.title
```
我们可以发现&lt;strong&gt;&lt;/strong&gt;在**haml**里面被%strong所代替，表达方式更加简洁。