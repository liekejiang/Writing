#HTML基础：基本结构以及常用语句

HyperText Markup Langage(是一种用于创建网页的标准标记语言，而非编程语言，用来描述网页，HTML文档也叫做web页面)。HTML大小写不敏感但是推荐（未来强制）使用小写。
**注意**：对于中文网页需要使用 <meta charset="utf-8"> 声明编码，否则会出现乱码。有些浏览器(如 360 浏览器)会设置 GBK 为默认编码，则你需要设置为 <meta charset="gbk">。
有多种软件支持HTML5的书写：**Notepad++**, **VScode**, **Sublime Text**。

第一个实例：
```html
<!DOCTYPE html>
<html>
<head>
<meta charset="utf-8">
<title> First </title>
</head>
<body>

<h1>我的第一个标题</h1>
<p>我的第一个段落。</p>

</body>
</html>
```
以上代码会显示大号标题“我的第一个标题”和第二行的小号“我的第一个段落”。代码可以复制到[在线编辑器](http://www.runoob.com/try/try.php?filename=tryhtml_intro)运行查看。
从第一个实例观察可知：
* <!DOCTYPE html> 声明为HTML5文档。
* HTML中不同元素需要使用<>标记，有些元素（如head，body，html）有始有终，起始正常书写，结束要加上/，HTML标签通常成对出现。
* head可能代表头文件，表示该文本的一些不会显示的格式属性（正确：包含了文档的元数据），如表示页面标题（出现在浏览器最上面那个，不会在页面里面显示）的的<title></title>，和表示支持中文的<meta>。
* 显示的可见内容使用<body>,</body>标记，整个HTML语段，使用<html>,</html>标记，<html>也是HTML页面的根元素。
* <h1>元素定义一个大标题，<p>元素定义了一个段落。

#### HTML中的注释
```html
<!-- 这是注释 -->

<!-- 
xxx
xxx
-->多行注释

<!-- [if IE 8]>
    some HTML here
<![endif]-->条件注释
```
注释的格式为<!--...-->, 同时也可以使用多行注释。
**注意**： 条件注释定义只有 Internet Explorer 执行的 HTML 标签。
#### <!DOCTYPE> 声明
该命令用于在浏览器中正确显示页面，对于不同的文件，只有正确声明HTML的版本，才能在浏览器显示正确的网页内容。**doctype**声明不区分大小写。
```html
<!DOCTYPE html> #这是HTML5的通用声明，被大多数主流浏览器所支持。
```
其他版本的HTML的<!DOCTYPE>要复杂的多，需要添加各种参数。对于不同的浏览器（主要是IE)有时要使用条件句声明针对不同ie版本的<!DOCTYPE>格式。
各种有效的DOCTYPES在[这里](http://www.runoob.com/tags/html-elementsdoctypes.html)查阅。

#### HTML标题
在第一个实例中已经提过，HTML在显示部分的标题通过<h1>--<h6>实现的，一共六种大小依次减小的成对标签。<h5></h5>和<h6></h6>的标题就过于小了。

#### HTML段落
使用<p></p>实现

#### 插入链接
通过标签<a></a>实现, herf是指定链接的地址的属性:
```ruby
<a herf = "http://majsoul.union-game.com/#/">这是GBF</a>
<a herf = "http://majsoul.union-game.com/#/"><!--这是GBF--></a>
```

#### 插入图象
通过标签<img>实现，这不是一个成对标签, 还可以控制长宽大小：
```ruby
<img src="/images/logo.png" width="258" height="39"/>
```
问题：src是什么？ 为什么把最后一个/删除，不影响显示结果？

#### 换行
通过标签<br>实现。可以直接插入<p></p>或标题的中间。如果不使用<br>，在显示的时候不会自动换行，会扩展横向滑动栏无限延长。<br>也是一个没有内容的HTML元素，这种元素又被称为空元素。使用斜杠可关闭空元素，即使<br>有效，使用<br/>更加安全。

#### HTML属性
HTML属性指的是HTML元素的属性，在属性中可以添加开始标签，属性也一般描述于开始标签，并且总是以名称/值对的形式出现： name = "value"。在前面写过的HTML链接<a>中就出现过属性**href**。

一般来说，属性值应该始终被包括在引号内，在属性值有双引号的时候必须使用单引号，反之亦然。

有一些适用于大多数HTML的属性：
* class ： 为HTML元素定义一个/多个类名（类名从样式文件中引入）
*    id :  定义元素的唯一id，该属性可选但是唯一。
* style :  规定元素的行内样式
* title :  描述元素的额外信息（作为工具条使用）。
更多的元素参见[链接](http://www.runoob.com/tags/ref-standardattributes.html)
