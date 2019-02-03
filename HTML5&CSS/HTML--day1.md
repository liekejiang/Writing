
HyperText Markup Langage(是一种用于创建网页的标准标记语言，而非编程语言，用来描述网页，HTML文档也叫做web页面)。
**注意**：对于中文网页需要使用 <meta charset="utf-8"> 声明编码，否则会出现乱码。有些浏览器(如 360 浏览器)会设置 GBK 为默认编码，则你需要设置为 <meta charset="gbk">。

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
* head可能代表头文件，表示该文本的一些不会显示的格式属性（正确：包含了文档的元数据），如表示文档标题的的<title></title>，和表示支持中文的<meta>。
* 显示的可见内容使用<body>,</body>标记，整个HTML语段，使用<html>,</html>标记，<html>也是HTML页面的根元素。
* <h1>元素定义一个大标题，<p>元素定义了一个段落。